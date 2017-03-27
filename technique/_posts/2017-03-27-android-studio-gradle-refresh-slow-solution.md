---
title: 解决Android Studio中，gradle更新慢的问题
date: 2017-03-27 15:39:58 +0800
tags: 技巧, Android Studio
---

Android Studio突然叫更新Gradle至3.3版本，好吧，按了下更新。

下面一栏就一直在"Refreshing Gradle Project..."，貌似是在下载的，然后看了下网络监控，50kB/s，下到何时？

于是上网找了下方法，找到了[这篇](http://www.cnblogs.com/exmyth/p/5901834.html)。

试了下，总结下我是怎么做的。

首先，打开工程目录下的/gradle/wrapper，里面有个gradle-wrapper.properties文件，用文本编辑软件（Vim、Notepad++等）打开它。查看里面的内容，我的如下：

>  distributionBase=GRADLE_USER_HOME  
distributionPath=wrapper/dists  
zipStoreBase=GRADLE_USER_HOME  
zipStorePath=wrapper/dists  
distributionUrl=https\://services.gradle.org/distributions/gradle-3.3-all.zip

看到里面的`distributionUrl`的内容，就是Android Studio在龟速下载的文件。复制到下载工具，居然有80多MB，50KB/s的速度下到天亮啊。

接着，下完以后，放到哪里呢？下下来的文件是一个zip文件，上面不是有`zipStoreBase`么？这个应该是指用户目录下的.gradle文件夹，例如我的就是，C:\Users\Administrator\.gradle。然后`zipStorePath`指的应该是相对于`zipStoreBase`的路径。也就是C:\Users\Administrator\.gradle\wrapper\dists。看下内容，果然有一个名为gradle-3.3-all的文件夹。进去里面，有一个名字由数字英文组成的文件夹，再进去，里面有一个part文件和一个lck文件。

现在，可以关闭Android Studio，然后删掉原有的内容，把下载下来的gradle-3.3-all.zip复制进去。重新打开Android Studio。

打开完成之后，还会看到Refreshing什么的，但是已经没有下载了，刚才的文件夹里面多出了一个gradle-3.3文件夹，里面就是解压好的gradle-3.3-all.zip的内容。

再等待一下，Gradle Build完成以后，顺利完成。
