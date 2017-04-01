---
title: Linux学习笔记
date: 2017-03-30 18:48:07 +0800
tag: 笔记, Linux
---

基于《鸟哥的Linux私房菜》做成的笔记。

# 目录


<!-- vim-markdown-toc Redcarpet -->
* [第二章](#第二章)
    * [各硬件设备在Linux中的文件名](#各硬件设备在linux中的文件名)
    * [分区表格式](#分区表格式)
        * [MSDOS分区表格式](#msdos分区表格式)
        * [GUID partition table GPT磁盘分区表](#guid-partition-table-gpt磁盘分区表)
* [第四章](#第四章)
    * [基础指令的操作](#基础指令的操作)
        * [显示日期的指令-date](#显示日期的指令-date)
        * [显示日历的指令-cal](#显示日历的指令-cal)
        * [简单计算器-bc](#简单计算器-bc)
    * [重要的热键](#重要的热键)
    * [Linux帮助指令](#linux帮助指令)
        * [指令的--help求助说明](#指令的-help求助说明)
        * [man page](#man-page)
            * [man page中指令后面的代号所代表的内容](#man-page中指令后面的代号所代表的内容)
            * [man page的主要内容划分](#man-page的主要内容划分)
            * [man page中的按键](#man-page中的按键)
            * [man命令的一些选项](#man命令的一些选项)
        * [info page](#info-page)
            * [info的按键](#info的按键)
        * [其他有用的文件](#其他有用的文件)
    * [文字编辑器-nano](#文字编辑器-nano)
        * [nano内的组合键](#nano内的组合键)
    * [关机方法](#关机方法)
        * [查看系统的使用状态](#查看系统的使用状态)
        * [将数据同步写入磁盘-sync](#将数据同步写入磁盘-sync)
        * [常用关机指令-shutdown](#常用关机指令-shutdown)
        * [重新开机和其他关机指令-reboot, halt, poweroff](#重新开机和其他关机指令-reboot-halt-poweroff)
        * [使用管理工具systemctl关机](#使用管理工具systemctl关机)
* [第五章 Linux的文件权限与目录配置](#第五章-linux的文件权限与目录配置)
    * [5.1 使用者与群组](#5-1-使用者与群组)
    * [5.2 Linux文件权限的概念](#5-2-linux文件权限的概念)
        * [Linux文件属性](#linux文件属性)
        * [改变文件属性与权限](#改变文件属性与权限)
            * [改变所属群组-chgrp](#改变所属群组-chgrp)
            * [改变文件拥有者-chown](#改变文件拥有者-chown)
            * [改变权限-chmod](#改变权限-chmod)
        * [目录与文件的权限意义](#目录与文件的权限意义)
            * [权限对文件的重要性](#权限对文件的重要性)
            * [权限对目录的重要性](#权限对目录的重要性)
        * [Linux中文件种类与扩展名](#linux中文件种类与扩展名)
            * [文件种类](#文件种类)
            * [文件扩展名](#文件扩展名)
        * [Linux目录配置](#linux目录配置)
            * [Linux目录配置的依据-FHS标准](#linux目录配置的依据-fhs标准)
            * [根目录的意义和内容](#根目录的意义和内容)

<!-- vim-markdown-toc -->

---

# 第二章

---

## 各硬件设备在Linux中的文件名

|设备|文件名|
|----|------|
|SCSI/STAT/USB硬盘|/dev/sd[a-p]|
|USB闪存盘|/dev/sd[a-p]|
|VirtI/O界面|/dev/vd[a-p]|
|软盘机|/dev/fd[0-7]|
|打印机（25针打印机）|/dev/lp[0-2]|
|打印机（USB接口）|/dev/usb/lp[0-15]|
|鼠标（通用）|/dev/input/mouse[0-15]|
|鼠标（PS/2界面）|/dev/psaux|
|鼠标（当前）|/dev/mouse|
|CDROM/DVDROM（通用）|/dev/scd[0-1]|
|CDROM/DVDROM（通用，CentOS常见）|/dev/sr[0-1]|
|CDROM/DVDROM（当前CDROM）|/dev/cdrom|
|磁带机（IDE界面）|/dev/ht0|
|磁带机（SATA/SCSI界面）|/dev/st0|
|磁带机（当前磁带）|/dev/tape|
|IDE硬盘机（旧式系统才有）|/dev/hd[a-d]|

---

## 分区表格式

---

### MSDOS分区表格式

放在磁盘的第一个扇区。有两个数据：
- 主要开机记录区（Master Boot Record, MBR），是安装开机管理程序的地方，446Bytes。
- 分区表（partition table）：记录整颗硬盘分区的状态，有64Bytes。

分区表只有64Bytes，只能容纳四笔记录。四组分区信息成为主要分区或延伸分区。逻辑分区是延伸分区切割出来的分区。延伸分区最多有一个。

---

### GUID partition table GPT磁盘分区表

区块地址（Logical Block Address，LBA）代表扇区，默认512Bytes。GPT用34个LBA区块来记录分区信息。另外最后的33个LBA来备份。

- 第一个区块（LBA0）与MBR类似。
- LBA1是GPT表头。记录分区表本身的位置与大小、备份的分区、校验码CRC32。
- LBA2-33是实际分区信息。每个LBA可以记录4个分区。总共128个。

---

# 第四章

---

## 基础指令的操作

---

### 显示日期的指令-date

用途：打印或设置系统日期和时间

格式：
- date [OPTION]... [+FORMAT]
- date [-u\|--utc\|--universal] [MMDDhhmm[[CC]YY][.ss]]

选项：
- -s, --set 设置时间

[manual](http://man7.org/linux/man-pages/man1/date.1.html)

---

### 显示日历的指令-cal

用途：打印日历，可指定年月

格式：
- cal [day] [month] [year]

选项：
- -1, --one：显示一个月
- -3, --three：显示三个月
- -n, --months *number*：显示*number*个月
- -s, --sunday：星期天为一个星期第一天
- -m, --monday：星期一为一个星期第一天
- -y, --year：显示一整年的日历
- -w, --week[=*number*]：在日历中显示星期数

[manual](http://man7.org/linux/man-pages/man1/cal.1.html)

---

### 简单计算器-bc

bc是一个小程序，可以支持+-*/%^的计算。

功能还是挺强大，这里放不下所有东西，看[manual](http://man7.org/linux/man-pages/man1/bc.1p.html)

---

## 重要的热键

---

* [Tab]按键  
  作用：补全命令或者文件名  
  用法：  
  - 在命令的前缀按两下[Tab]按键，会补全整个命令。若有多个命令匹配这个前缀，则会 打印出匹配的命令。
  - 在文件名前缀按两下[Tab]按键，补全文件名，若有多个，则打印。
  - 安装[bash-completion](https://github.com/GArik/bash-completion)软件，可以使用选项/参数的补齐。
* [Ctrl]-c按键  
  作用：终止命令的执行
* [Ctrl]-d按键  
  作用：输入EOF字符。或者登出
* [Shift] + [Page Up]/[Page Down]  
  作用：向前翻页和向后翻页

----

## Linux帮助指令

-----

### 指令的--help求助说明

查询比较常用的格式、选项和参数等。

---

### man page

查询方法：man command。command是要查询的指令

#### man page中指令后面的代号所代表的内容

|代号|代表内容|
|----|--------|
|1|使用者在shell环境中可以操作的指令或可执行文件|
|2|系统核心可调用的函数与工具等|
|3|一些常用的函数（function）与函数库（library），大部分为C的函数库（libc）|
|4|设备文件的说明，通常在/dev下的文件|
|5|配置文件或者是某些文件的格式|
|6|游戏|
|7|惯例与协定等，例如Linux文件系统、网络协定、ASCII code等的说明|
|8|系统管理员可用的管理指令|
|9|跟kernel有关的文件|

#### man page的主要内容划分

|代号|内容说明|
|----|--------|
|NAME|简短的指令、数据名称说明|
|SYNOPSIS|简短的指令下达语法（syntax）简介|
|DESCRIPTION|较为完整的说明|
|OPTIONS|针对SYNOPSIS部分中，有列举的所有可用的选项说明|
|COMMANDS|当这个程序（软件）在执行的时候，可以在此程序（软件）中下达的指令|
|FILES|这个程序或数据所使用或参考或链接到的某些文件|
|SEE ALSO|可以参考的，跟这个指令或数据有关的其他说明|
|EXAMPLE|一些可以参考的范例|

#### man page中的按键
|按键|功能|
|----|----|
|[空白键]|向下翻一页|
|[Page Down]|向下翻一页|
|[Page Up]|向上翻一页|
|[Home]|去到第一页|
|[End]|去到最后一页|
|/string|向下搜寻string这个字串|
|?string|向上搜寻string这个字串|
|n,N|“下”一个和“上”一个|
|q|结束这次man page|

#### man命令的一些选项

- -f [*command*]：找寻符合command关键字的所有等级的man page。相当于*whatis*命令。
- -k [*keyword*]：使用keyword关键字搜索man page。相当于*apropos*命令。

[manual](http://man7.org/linux/man-pages/man1/man.1.html)

---

### info page

与man的格式不同。info page将文件拆成段落，每个段落用一个页面撰写，在各个页面中有类似网页的超链接来跳到不同的页面中。

#### info的按键

|按键|功能|
|----|----|
|空白键|向下翻一页|
|[Page Down]|向下翻一页|
|[Page Up]|向上翻一页|
|[tab]|在node之间移动，有node的地方，通常会以*显示|
|[Enter]|当光标在node上面时，按下Enter可以进入该node|
|b|移动光标到info画面当中的第一个node处|
|e|移动光标到该info画面当中的最后一个node处|
|n|前往下一个node处|
|p|前往上一个node处|
|u|向上移动一层|
|s(/)|在info page当中进行搜寻|
|h, ?|显示求助菜单|
|q|结束这次的info page|

---

### 其他有用的文件

一般来说，指令或者软件的制作者，都会将文档做成线上说明文档，放在/usr/share/doc目录中。

---

## 文字编辑器-nano

命令用法：nano *filename*。*filename*是要打开的文件名，若不存在，则会新建文件。

### nano内的组合键

|组合键|作用|
|[ctrl]-G|取得线上说明|
|[ctrl]-X|离开nano|
|[ctrl]-O|储存文件|
|[ctrl]-R|其他文件读取数据|
|[ctrl]-W|搜寻字符串|
|[ctrl]-C|说明目前光标所在处的行数与列数等信息|
|[ctrl]-_|直接输入行号，可以移动到该行|
|[alt]-Y|校正语法功能打开或关闭|
|[alt]-M|支持鼠标来移动光标|

---

## 关机方法

----

### 查看系统的使用状态

who：查看目前有谁在线上。[manual](http://man7.org/linux/man-pages/man1/who.1.html)
netstat -a：查看网络连接状态。[manual](http://man7.org/linux/man-pages/man8/netstat.8.html)
ps -aux：看背景执行程序。[manual](http://man7.org/linux/man-pages/man1/ps.1.html)

---

### 将数据同步写入磁盘-sync

Linux中为了性能，载入文件之后，对它的修改不会立刻写回硬盘当中。

sync命令会将在内存中尚未更新的数据，写入硬盘中。

必须要超级管理员root才能执行。

一般的shutdown, reboot指令默认会执行sync。

[manual](http://man7.org/linux/man-pages/man1/sync.1.html)

---

### 常用关机指令-shutdown

格式：shutdown [-krhc] [时间] [警告讯息]

选项：
- -k：仅发送警告信息
- -r：将系统服务停止之后重新开机
- -h：将系统服务停止之后立即关机
- -c：取消已经在进行的shutdown指令

参数：
- 时间：指定系统关机时间，只输入数字时以分钟为单位，默认1分钟后执行。还有其他特殊参数如now代表立即执行，输入HH:MM格式的时间，则会在那时间执行。

[manual](http://man7.org/linux/man-pages/man2/shutdown.2.html)

---

### 重新开机和其他关机指令-reboot, halt, poweroff

reboot与shutdown差不多，看[manual](http://man7.org/linux/man-pages/man2/reboot.2.html)

halt指令会让系统停止，屏幕保留系统已经停止的讯息。看[manual](http://man7.org/linux/man-pages/man8/halt.8.html)

poweroff指令，另一个关机指令。看[manual](http://man7.org/linux/man-pages/man8/halt.8.html)

----

### 使用管理工具systemctl关机

格式：systemctl [指令]

指令包括：
- halt
- poweroff
- reboot
- suspend：进入休眠模式

---

# 第五章 Linux的文件权限与目录配置

---

## 5.1 使用者与群组

在Linux里面， 任何文件都具有“User, Group及Others”三种身份的个别权限。

---

## 5.2 Linux文件权限的概念

### Linux文件属性

使用ls -l命令时，显示的信息的各个属性如下
-rw-r--r--. 1   root    root    1864    May 4 18:01 foo.txt
[权限]  [链接]  [拥有者]    [群组]  [文件大小]  [修改日期]  [文件名]

部分属性详解
- 权限
- - 第一个字符代表文件类型
- - - d是目录
- - - \-是文件
- - - l是链接文件
- - - b是设备文件里面的块设备文件
- - - c为设备文件里面的字符设备文件
- - 接下来的三个一组，均是rwx的组合，r代表可读，w可写，x可执行。
- - - 第一组为文件拥有者的权限
- - - 第二组为此文件群组的权限
- - - 第三组为其他人的权限
- 链接
- - 代表有多少个文件名链接到此节点（i-node）

---

### 改变文件属性与权限

#### 改变所属群组-chgrp

格式：chgrp [-R] *dirname*/*filename*

选项：
- -R：递归(recursive)变更，即子目录和文件都会更改

#### 改变文件拥有者-chown

格式：
- chown [-R] *账号名称* *文件或目录*
- chown [-R] *账号名称*:*群组名称* *文件或目录*：同时修改群组和拥有者

选项：
- -R：递归变更

#### 改变权限-chmod

**使用数字来修改各个权限**

权限数值：
- r:4
- w:2
- x:1

每种身份的三个权限值累加起来就是最终的权限。比如-rwxrw-r--就是764。

命令格式：chmod [-R] xyz 文件或目录

选项：
- -R：递归变更

参数：
- xyz：3个数字权限值。

**使用符号来修改各个权限**

格式如下：

|chmod|u|+（加入）|r|文件或目录|
|     |g|-（减去）|w|          |
|     |o|=（设置）|x|          |
|     |a|         | |          |

若要分别设置权限，则只需中间加入逗号(,)就行，如chmod u=rwx,go=rx foo

---

### 目录与文件的权限意义

#### 权限对文件的重要性

- r(read)：可读取文件的内容
- w(write)：可编辑、新增或修改文件内容（但不能删除）
- x(execute)：可被系统执行

#### 权限对目录的重要性

- r(read contents in directory)：读取目录结构清单的权限
- w(modify contents of directory)：可创建、删除、重命名、移动该目录内的文件
- x(access directory)：可进入该目录，成为工作目录

---

### Linux中文件种类与扩展名

#### 文件种类

- 正规文件
- - 纯文本文件
- - 二进制文件
- - 数据格式文件
- 目录
- 链接文件
- 设备与设备文件
- - 区块设备文件
- - 字符设备文件
- 套接字文件
- 数据输送档（FIFO, pipe）

#### 文件扩展名

Linux中没有扩展名的概念。扩展名只是用来标识一个文件时什么文件

---

### Linux目录配置

#### Linux目录配置的依据-FHS标准

全称Filesystem Hierachy Standard，将目录定义为下列四种交互形态。

||可分享的（shareable）|不可分享的（unshareable）|
|不变的（static）|/usr（软件放置处）<br>/opt（第三方协助软件）|/etc（配置文件）<br>/boot（开机与核心档）|
|可变动的（variable）|/var/mail（使用者邮件信箱）<br>/var/spool/news（新闻群组）|/var/run（程序相关）<br>/var/lock（程序相关）|

上述表头含义：
- 可分享的：可以分享给其他系统挂载使用的目录
- 不可分享的：自己机器上面运行的设备文件或者与程序有关的socket文件等
- 不变的：不会经常变动的文件，只随着distribution变的。
- 可变动的：经常改变的文件。

FHS针对目录树架构，仅定义出三个目录分别放什么：
- /（root，根目录）：与开机系统有关
- /usr（unix software resource）：与软件安装/执行有关
- /var（variable）：与系统运行过程有关

#### 根目录的意义和内容


