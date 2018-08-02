---
layout: post
title: Go静态库动态库使用
subtitle: 如何提供和使用闭源代码库
tags:
  - Go
author: 吴俊贤
catalog: true
---

内容参考如下文章：
- <http://reborncodinglife.com/2018/04/27/how-to-create-static-lib-in-golang/>

首先给出一个重要结论，**Go语言并不支持使用只有二进制文件（.a与可执行文件）的库**。

# 静态库编译使用

## 库编写者要做的事

假如我写了一个超级厉害的排序库（不会给你看代码的手动滑稽），库的位置在`$GOPATH/src/github.com/packagewjx/gostudy/libuse/niu`，那么我首先需要编译一个静态库文件，下面这个命令就会自动的编译。

```bash
go install github.com/packagewjx/gostudy/libuse/niu
```

然后会在`GOPATH`的`pkg`文件夹下，根据路径找到对应的`.a`文件。

我们可以提供一个头文件，供库的使用者使用。但是Go本身是没有头文件的，所有的东西只会定义一次。

我的想法是，使用buildtag来区别头文件和实际文件，比如，在我们开发者的项目中，所有的头文件都将拥有一个buildtag，叫header就ok。然后，使用`go install`命令编译成静态库。打包的时候，将只包含我们静态库文件和头文件的，依据`GOPATH`的格式，打包`pkg`和`src`文件夹到一个tarball里面，这样用户只需要将这个tarball解压到他们的gopath里面就行了。但是，要注意的是，**用户不能编译那些header文件**，否则就会覆盖那些编译好的静态库文件了。

可能还要找一找方法去规避编译header文件，go本身没有提供可以使用只有静态库文件没有源代码的软件库的功能（被列为实验性功能）。

挖个坑？可能永远都不会填的。想做一个小工具，输入库的路径，就可以自动寻找库中已经被导出的函数，以及自定义结构等数据类型，并连同注释，放入到一个头文件中。完了之后会打包成一个tarball，就能够很方便的输出闭源软件库了。

## 库使用者使用方法

将静态库文件和头文件（如果有）放到相应的目录中，我们的主程序就导入，如下

```go
package main

import (
    "math/rand"
    "time"
    "github.com/packagewjx/gostudy/libuse/niu"
    "fmt"
)

func main() {
    length := 1000
    nums := make([]int, length)
    rand.Seed(time.Now().Unix())
    for i := 0; i < length; i++ {
        nums[i] = rand.Int()
    }
    niuSort := niu.NiuSort(nums)
    fmt.Println(nums)
    fmt.Println(niuSort)
}
```

编译时需要注意，我们不能够让go自动的编译那个头文件，这样会覆盖掉那个静态库。

能做到这么编译的，只能是这样子了

首先编译（工作目录在gostudy）

```bash
go tool compile -I /home/wjx/go/pkg/linux_amd64/ libuse/main.go
```

然后链接

```bash
go tool link -o main -L /home/wjx/go/pkg/linux_amd64/ main.o
```

这样子使用的确非常麻烦。

# 关于Go提供闭源的讨论

## <https://github.com/golang/go/issues/2775>

里面有些有意思的点

1. go的不同版本编译出来的`.a`文件无法共用。只能使用同一个版本编译出的文件。
2. mtime hack

只是到最后也只是把这个功能标注为实验性功能，并没有真正的实现在go编译器中。
