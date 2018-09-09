---
layout: post
title: Go箴言个人理解
subtitle: 像诗一样简练
tags:
  - Go
author: 吴俊贤
---

# 前言

Go大牛Rob Pike在15年做过一次关于Go的[演讲](https://www.youtube.com/watch?v=PAAkCSZUG1c&t=8m43s)，提出了许多Go的箴言。这个[网站](http://go-proverbs.github.io/)总结了这些箴言。

这篇文章将会一一探讨这些箴言的意义，加上我个人浅显的见解。

# Don't communicate by sharing memory, share memory by communicating.

[不要通过共享内存来通信，而是通过通信来共享内存](https://www.youtube.com/watch?v=PAAkCSZUG1c&t=2m48s)

通过共享内存通信，大概意思是使用一个公共的变量，两个线程修改和读取这个变量来进行数据交换。这就引入了一个大问题。当两个线程同时访问一个变量，且其中一个线程是写入时，就会发生数据竞争（data race），此时的对变量访问的结果将会是不确定的，甚至会导致数据的丢失。

通过通信来共享内存，在Go语言中，指的就是通过通道。Go在语言层面上实现了通道这个类型，使得安全的并发计算被根植在了Go语言的核心中。两个goroutine的交流，就可以使用通道，完成数据的传送或者是命令的发送。甚至，通道还能实现信号量机制，通过创建一个拥有缓存的通道，我们就能够模拟出一个信号量，如

```go
semaphore := make(chan struct{}, 10)

// 当我需要给信号量减一，也就是P操作时，从通道中发送数据
semaphore <- struct{}{}

// 做该做的事

// 释放信号量只需要从通道接收即可，相当于V操作
<-semaphore
```

当使用缓存通道时，如果缓存没有满，发送会立即完成，相当于信号量还没有降成0时，可以立刻获取到信号量。如果缓存满了，就会被阻塞，等待缓存为空的时候，才能发送成功。代码完成后，只需要接收值，缓存就会空出一个，此时，被阻塞的其中一个线程，就能够成功发送，并进行操作了。

Go的通道在语言层面上保证了通道是并发安全的，多个goroutine在同时发送时，一个时刻只有一个会成功发送。这就保证了我们的并发操作的安全性。

# Concurrency is not parallelism.

[并发不是并行](https://www.youtube.com/watch?v=PAAkCSZUG1c&t=3m42s)

Pike的原话是

> Concurrency is a way of structuring your program to make it easy to understand and scalable. Parallelism is simply the execution of multiple goroutines in parallel.

意即，并发是一个使得程序更容易理解和扩展的程序组织方式，并行只是多个goroutine同时在运行而已。

并行非常好理解，想想你的电脑，电脑都是多个核心的了，一台8线程CPU的电脑，每个CPU上都跑着一个线程，那么就跑着8个线程，但是这8个线程一般不是在干同一件事情的不同部分，而根本就在做不同的事，比如一个核在运行系统程序，一个核在跑音乐播放器，一个核在跑浏览器程序，这些正在运行的线程之间的关系，就是并发，因为他们根本是没在干同一件事情，只是同一个时间在跑而已。

但是并行不一样，并行会有多个并发的线程，每个线程在做同一个时间的不同部分，就比如造汽车，一个线程负责造引擎，一个线程负责造车架，一个线程负责造轮胎等等，他们同时在运行，但是他们都是在造车。

为什么说更容易扩容和理解呢？还是造车。我们把造车这件事情，分成了多个能够分开的同时做的任务，造引擎、造车架、造轮胎等等，那么，如果写成go的伪代码，可能是这样的

```go

func MakeCar() Car {
  // 可能有一些并行协调机制，这里略去
  go makeEngine()
  go makeFrame()
  go makeWheel()
  ...

  return car
}

func makeEngine() Engine {
  ...
}

func makeFrame() Frame {
  ...
}

func makeWheel() Wheel {
  ...
}

...
```

将一个大的任务，拆分成了多个并行的小任务，小任务又可以继续拆分成多个并行的小任务，程序的逻辑一目了然，这便是更容易让人理解了。虽然我觉得还是要看写代码的人的风格，见仁见智的。

但是可扩展性就非常好说明，比如哪天我的汽车卖得好了，需要扩展生产的时候，非常的简单，每个能够并发的任务，都已经封装成各自的Go代码。如果我不再运行一次`MakeCar`造一辆车，而是造引擎、车架、轮胎等的工作同时在执行，另外再将组装的逻辑写成一个goroutine，就能够极大的提高效率了。比如我改成下面这样

```go
func CarFactory() chan Car {
  // 在返回的通道上不断的发送造好的车


}

func makeEngineContinuously() chan Engine {
  // 通道上源源不断有引擎
  engineChan := make(chan Engine)
  // 假设有5个生产线
  for i := 0; i < 5; i++ {
    go func() {
      for {
        // 不断的生产引擎
        engineChan <- makeEngine()
      }
    }
  }
  return engineChan
}

// 其它函数类似改法

func assembleContinouesly() chan Car {
  ...
  engineChan := makeEngineContinuously()
  for {
    engine := <-engineChan
    ...

    // 组装起来并发送
  }

}



```
