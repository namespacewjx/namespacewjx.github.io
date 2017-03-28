---
title: JVM学习笔记
date: 2017-03-28 19:25:03 +0800
tags: JVM, 笔记
---

这个笔记是学习《深入理解Java虚拟机：JVM高级特性与最佳实践（第二版）》的过程中记录下的。


---

# 第八章

---

## 8.3.2节 分派

### 静态分派

`Human man = new Man();`中，Human称为变量的**静态类型**或**外观类型**，而Man则是变量的**实际类型**。

虚拟机（严格说是编译器）在重载时通过参数的**静态类型** 作为判定依据。

所有依赖静态类型来定位方法执行版本的分派动作称为**静态分派**，典型有方法重载。

方法重载时，编译器确定应该调用的重载方法的优先级，基础类型的确定过程如下，举一个char类型的例子：

1. 类型一致的方法，如`fun(char c)`
2. 依据能够自动向上转换类型的顺序：`byte -> short, char -> int -> long -> float -> double`，依次匹配到的方法，如`fun(int c)`等。
3. 有对应的包装类作为参数的方法，如`fun (Character c)`。
4. 包装类实现的接口的方法，如Character类实现了Serializable方法，则匹配`fun(Serializable c)`。注意，同级如果有多个接口，编译器无法确定类型，会报错，拒绝编译。
5. 包装类继承链上最接近包装类的方法，Character的父类就是Object，因此可以匹配`fun(Object c)`。对Integer来说，则依次匹配`fun(Number i)`，`fun(Object i)`。
6. 有对应类型的变长参数的方法，如`fun(char... c)`。

关于更多的在重载中确定最合适方法的过程，需要看[*Java Language Specification*](http://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.12.2)中15.12.2.5节内容。




