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
    * [使用者与群组](#使用者与群组)
    * [Linux文件权限的概念](#linux文件权限的概念)
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
            * [/usr的意义和内容](#usr的意义和内容)
            * [/var的意义与内容](#var的意义与内容)
        * [绝对路径与相对路径](#绝对路径与相对路径)
* [第六章 Linux文件与目录管理](#第六章-linux文件与目录管理)
    * [目录与路径](#目录与路径)
        * [目录的相关操作](#目录的相关操作)
            * [变换目录指令-cd](#变换目录指令-cd)
            * [显示当前工作目录-pwd](#显示当前工作目录-pwd)
            * [创建新目录-mkdir](#创建新目录-mkdir)
            * [删除空目录-rmdir](#删除空目录-rmdir)
        * [可执行文件路径的环境变量-PATH](#可执行文件路径的环境变量-path)
    * [文件与目录管理](#文件与目录管理)
        * [文件与目录的查看命令-ls](#文件与目录的查看命令-ls)
        * [复制、删除、移动-cp,rm,mv](#复制、删除、移动-cp-rm-mv)

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

##  使用者与群组

在Linux里面， 任何文件都具有“User, Group及Others”三种身份的个别权限。

---

##  Linux文件权限的概念

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

根目录是系统最重要的一个目录。所有的目录都是由根目录衍生出来的，根目录也与开机/还愿/系统修复等动作有关。若系统出错，根目录也要包含能够修复文件系统的程序才行。

因此，FHS建议，根目录(/)所在**分区应该越小越好**，而且应用程序安装的软件最好不要与根目录放在同一个分区。

FHS定义根目录下的次目录如下，若没有实体，也最好有链接。

|目录|应放置的文件内容|
||**第一部分：FHS要求必须存在的目录**|
|/bin|放置单人维护模式下还能够被操作的指令，一般被root所使用|
|/boot|放置开机用到的文件，包括Linux核心文件以及开机需要的配置文件等等|
|/dev|各个设备的文件形态的储存地点|
|/etc|系统主要的配置文件。/etc/opt目录中放置第三方软件的配置文件|
|/lib|开机使用的函数库，以及在/bin或/sbin下面会调用的函数库。另外/lib/modules/里面应该放驱动程序|
|/media|放置可移动的设备，如光盘，DVD等|
|/mnt|暂时挂载额外设备的目录|
|/opt|放置第三方软件的目录|
|/run|开机后产生的各项信息|
|/sbin|开机过程中需要的，包括开机、修复、还原系统所需的指令。服务器软件放置在/usr/sbin/中。本机安装的软件放置在/usr/local/sbin/中。|
|/srv|网络服务的数据目录|
|/tmp|用户或是程序暂时放置文件的地方|
|/usr|后续介绍|
|/var|后续介绍|
||**第二部分：FHS建议的可存在的目录**|
|/home|使用者主文件夹|
|/lib\<*qual*\>|不同格式的二进制函数库，如lib64|
|/root|系统管理员的主文件夹，应该与根目录放在同一个分区当中|

**Linux下其他的目录**

|目录|应放置文件内容|
|/lost+found|ext2/ext3/ext4文件系统的目录，在系统发生错误时，将遗失的片段放到这里|
|/proc|这是“虚拟文件系统”。实际的储存内容在内存中。重要文件有/proc/cpuinfo，/proc/dma等|
|sys|“虚拟文件系统”，放置核心与硬件信息相关的信息，已载入的核心，核心侦测到的硬件信息等|

#### /usr的意义和内容

usr是Unix Software Resource的缩写，FHS建议软件开发者把数据合理分配到这个目录下的子目录中。系统也会把默认软件放到这个目录中。

FHS建议的usr子目录如下

|目录|应放置的文件内容|
||**FHS要求必须存在的目录**|
|/usr/bin/|一般用户能用的指令|
|/usr/lib/|与/lib功能一样|
|/usr/local/|系统管理员安装的软件|
|/usr/sbin/|非系统正常运行需要的系统指令，如服务指令|
|/usr/share/|只读的数据文件，共享文件，里面还有子目录man放置线上说明文档，doc放置软件杂项说明文档，zoneinfo时区文件|
||**FHS建议可以存在的目录**|
|/usr/games/|游戏数据放置处|
|/usr/include/|c/c++等语言头文件和包含文件的放置处|
|/usr/libexec|不被使用者习惯使用的脚本和可执行文件|
|/usr/lib\<*qual*\>|与/lib\<*qual*\>功能一样|
|/usr/src/|源代码放置处|

#### /var的意义与内容

主要放置经常变动的文件，如软件运行产生的文件，高速缓存，日志文件。

次目录如下

|目录|应放置的文件内容|
||FHS规定必须存在的目录|
|/var/cache/|程序产生的一些缓存文件|
|/var/lib/|程序执行当中需要的数据文件|
|/var/lock/|上锁文件的放置地方|
|/var/log/|重要的日志文件|
|/var/mail/|电子邮件信箱|
|/var/run/|程序或服务启动后放置PID的地方|
|/var/spool/|放置队列数据的地方，程序排队等待使用的数据|

----

### 绝对路径与相对路径

由根目录(/)开始写起的是绝对路径。其余是相对路径。

一个点(./)代表当前目录，两个点(../)代表上级目录。

若要执行当前目录的脚本等，需要在前面加上./，使系统在这个目录下寻找。如果不是的话，默认会从PATH环境变量指定的目录寻找。

----

# 第六章 Linux文件与目录管理

----

## 目录与路径

---

### 目录的相关操作

下面是一些特殊目录

- .：本目录
- ..：上层目录
- -：前一个工作目录
- ~：当前账号的主目录
- ~*account*：account账号的主目录

#### 变换目录指令-cd

格式：cd [*相对路径或绝对路径*]

cd是change directory的缩写。

[manual](http://man7.org/linux/man-pages/man1/cd.1p.html)

#### 显示当前工作目录-pwd

格式：pwd [-P]

选项：
- -P：显示出实际路径，而非链接路径

[manual](http://man7.org/linux/man-pages/man1/pwd.1.html)

#### 创建新目录-mkdir

格式：mkdir [-mp] *目录名*

选项：
- -m：设置文件的权限，覆盖默认的umask。
- -p：创建目录时把未存在的上层目录也会创建

[manual](http://man7.org/linux/man-pages/man1/mkdir.1.html)

#### 删除空目录-rmdir

格式：rmdir [-p] *目录名*

选项：
- -p：删除上层的空目录

注意，这个命令**仅能删除空目录**。

----

### 可执行文件路径的环境变量-PATH

当执行一个命令时，系统会根据PATH指定的路径，搜索特定命令的可执行文件。

PATH中每个目录以冒号(:)分隔开。

添加一个目录到PATH中的方法如下：

`PATH="${PATH}:directory`

若有同名的指令在PATH下，则会执行最先找到的指令。

---

## 文件与目录管理

---

### 文件与目录的查看命令-ls

格式：
- ls [-aAdfFhilnrRSt] *文件名或目录名称*
- ls [--color={never,auto,always}] *文件名或目录名*
- ls [--full-time] *文件名或目录名*

选项：
- **-a  ：全部的文件，连同隐藏文件（ 开头为 . 的文件） 一起列出来**
- -A  ：全部的文件，连同隐藏文件，但不包括 . 与 .. 这两个目录
- **-d  ：仅列出目录本身，而不是列出目录内的文件数据**
- -f  ：直接列出结果，而不进行排序 （ls 默认会以文件名排序！）
- -F  ：根据文件、目录等信息，给予附加数据结构，例如*:代表可可执行文件； /:代表目录； =:代表 socket 文件； |:代表 FIFO 文件；
- -h  ：将文件大小以人类较易读的方式（例如 GB, KB 等等）列出来；
- -i  ：列出 inode 号码，inode 的意义下一章将会介绍；
- **-l  ：长数据串行出，包含文件的属性与权限等等数据**
- -n  ：列出 UID 与 GID 而非使用者与群组的名称 （UID与GID会在帐号管理提到！）
- -r  ：将排序结果反向输出，例如：原本文件名由小到大，反向则为由大到小；
- -R  ：连同子目录内容一起列出来，等于该目录下的所有文件都会显示出来；
- -S  ：以文件大小大小排序，而不是用文件名排序；
- -t  ：依时间排序，而不是用文件名。
- --color=never  ：不要依据文件特性给予颜色显示；
- --color=always ：显示颜色
- --color=auto   ：让系统自行依据设置来判断是否给予颜色
- --full-time    ：以完整时间模式 （包含年、月、日、时、分） 输出
- --time={atime,ctime} ：输出 access 时间或改变权限属性时间 （ctime） 而非内容变更时间 （modification time）

[manual](http://man7.org/linux/man-pages/man1/ls.1.html)

### 复制、删除、移动-cp,rm,mv
