---
layout:     post
title:      "CGo之C字符串使用时的内存泄露问题"
author:     "吴俊贤"
tags:
    - Go
---

# C字符串使用时的内存泄露问题

cgo官方文档中，在CString与CBytes处，指明了我们需要在使用完毕后，释放内存。如下

```go
// Go string to C string
// The C string is allocated in the C heap using malloc.
// It is the caller's responsibility to arrange for it to be
// freed, such as by calling C.free (be sure to include stdlib.h
// if C.free is needed).
func C.CString(string) *C.char

// Go []byte slice to C array
// The C array is allocated in the C heap using malloc.
// It is the caller's responsibility to arrange for it to be
// freed, such as by calling C.free (be sure to include stdlib.h
// if C.free is needed).
func C.CBytes([]byte) unsafe.Pointer
```

之前一直都没有注意到这一点，毕竟用惯了不需要手动管理内存的编程语言，直到最近在写cgo代码时，发现程序运行很长时间以后，就会出现内存泄漏的情况。

不仅从Go字符串转换成C字符串需要释放，若从Go调用的C函数返回了指针，且该指针所指向的空间，是在函数内分配的，那么我们也需要在代码中调用`C.free()`函数。

下面一小段演示程序，揭示了不使用`C.free()`时候带来的危害。

```go
/*
#include <stdlib.h>
#include <string.h>

char* functionMalloc() {
	char *mem = (char*)malloc(1024 * 1024 * 50);
	memset(mem, '-', 1024 * 1024 * 50);
	return mem;
}

 */
import "C"
import (
	"fmt"
	"time"
	"unsafe"
)

func functionMallocNoFree() {
	length := 20;
	cstr := make([]*C.char, 0, length)
	for i := 0; i < length; i++ {
		<-time.After(time.Millisecond * 500)
		fmt.Println("分配", i)
		cptr := C.functionMalloc()
		str := C.GoString(cptr)
		fmt.Println(len(str))
		cstr = append(cstr, cptr)
	}
}

func functionMallocFree() {
	length := 20;
	cstr := make([]*C.char, 0, length)
	for i := 0; i < length; i++ {
		<-time.After(time.Millisecond * 500)
		fmt.Println("分配", i)
		cptr := C.functionMalloc()
		str := C.GoString(cptr)
		fmt.Println(len(str))
		cstr = append(cstr, cptr)
		// 释放内存
		C.free(unsafe.Pointer(cptr))
	}
}

```

两个函数唯一的区别是是否调用了`C.free()`。上面一段会在运行时候，占用越来越大的空间，而下面一段，则只占用一小部分。

# 可能解决方法

我们可以利用go的`defer`语句，在刚拿到C的指针的地方，立刻使用`defer`语句加入释放的逻辑。比如

```go
/*
#include <stdlib.h>
*/
import "C"

func fun() {
  str := "Hello"
  cstr := C.CString(str)
  defer C.free(unsafe.Pointer(cstr))
  // 操作cstr
}
```

或者如果返回了指针，我们也可以这么做，拿上面的函数做例子

```go
/*
#include <stdlib.h>
#include <string.h>

char* functionMalloc() {
	char *mem = (char*)malloc(1024 * 1024 * 50);
	memset(mem, '-', 1024 * 1024 * 50);
	return mem;
}

 */
import "C"

func fun() {
  cstr := C.functionMalloc()
  defer C.free(unsafe.Pointer(cstr))
  // 操作cstr
}
```

在C指针出现的地方立即`defer`释放掉，比在程序写代码释放要好一些，因为这样不怕漏掉任何的指针。
