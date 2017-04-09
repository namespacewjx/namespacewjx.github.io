---
title: Linux学习笔记
date: 2017-03-30 18:48:07 +0800
tag: 笔记, Linux 
---

基于《鸟哥的Linux私房菜》做成的笔记。

查manual手册是极好的，因此在每个指令的最后都附上了在线manual的网址。[在线Linux Manual](http://man7.org/linux/man-pages/index.html)

# 目录


<!-- vim-markdown-toc Redcarpet -->
* [第二章 主机规划与磁盘分区](#第二章-主机规划与磁盘分区)
    * [各硬件设备在Linux中的文件名](#各硬件设备在linux中的文件名)
    * [分区表格式](#分区表格式)
        * [MSDOS分区表格式](#msdos分区表格式)
        * [GUID partition table GPT磁盘分区表](#guid-partition-table-gpt磁盘分区表)
* [第四章 首次登陆与线上求助](#第四章-首次登陆与线上求助)
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
            * [复制指令-cp](#复制指令-cp)
            * [移除指令-rm](#移除指令-rm)
            * [移动或更名指令-mv](#移动或更名指令-mv)
        * [获取路径的文件名称和目录名称](#获取路径的文件名称和目录名称)
    * [文件内容查阅](#文件内容查阅)
        * [cat指令](#cat指令)
        * [tac指令](#tac指令)
        * [带行号打印-nl](#带行号打印-nl)
        * [可翻页显示](#可翻页显示)
            * [more指令](#more指令)
            * [less指令](#less指令)
        * [数据截取指令](#数据截取指令)
            * [取出前面几行-head](#取出前面几行-head)
            * [取出后面几行-tail](#取出后面几行-tail)
        * [查阅非纯文本文件指令-od](#查阅非纯文本文件指令-od)
        * [修改文件时间或创建新文件指令-touch](#修改文件时间或创建新文件指令-touch)
    * [文件与目录的默认权限与隐藏权限](#文件与目录的默认权限与隐藏权限)
        * [文件默认权限-umask](#文件默认权限-umask)
        * [文件隐藏属性](#文件隐藏属性)
            * [设置文件隐藏属性-chattr](#设置文件隐藏属性-chattr)
            * [显示文件隐藏属性-lsattr](#显示文件隐藏属性-lsattr)
        * [文件特殊权限-SUID SGID SBIT](#文件特殊权限-suid-sgid-sbit)
            * [Set UID](#set-uid)
            * [Set GID](#set-gid)
            * [Sticky Bit](#sticky-bit)
            * [SUID SGID SBIT的设置](#suid-sgid-sbit的设置)
        * [查看文件类型-file](#查看文件类型-file)
    * [指令与文件的搜寻](#指令与文件的搜寻)
        * [指令文件的搜寻](#指令文件的搜寻)
            * [寻找可执行文件-which](#寻找可执行文件-which)
        * [文件的搜寻](#文件的搜寻)
            * [特定目录寻找文件-whereis](#特定目录寻找文件-whereis)
            * [locate和updatedb指令](#locate和updatedb指令)
            * [最强大搜寻指令-find](#最强大搜寻指令-find)
* [第七章 Linux磁盘与文件系统管理](#第七章-linux磁盘与文件系统管理)
    * [认识Linux文件系统](#认识linux文件系统)
        * [文件系统特性](#文件系统特性)
        * [Linux的Ext2文件系统](#linux的ext2文件系统)
            * [查询Ext家族superblock信息的指令-dumpe2fs](#查询ext家族superblock信息的指令-dumpe2fs)
            * [列出系统所有格式化设备的指令-blkid](#列出系统所有格式化设备的指令-blkid)
        * [与目录树的关系](#与目录树的关系)
        * [Ext2 Ext3 Ext4文件的存取与日志式文件系统的功能](#ext2-ext3-ext4文件的存取与日志式文件系统的功能)
            * [日志式文件系统](#日志式文件系统)
        * [Linux文件系统的运行](#linux文件系统的运行)

<!-- vim-markdown-toc -->

---

# 第二章 主机规划与磁盘分区

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

# 第四章 首次登陆与线上求助

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

[***manual***](http://man7.org/linux/man-pages/man1/date.1.html)

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

[***manual***](http://man7.org/linux/man-pages/man1/cal.1.html)

---

### 简单计算器-bc

bc是一个小程序，可以支持+-*/%^的计算。

功能还是挺强大，这里放不下所有东西，看[***manual***](http://man7.org/linux/man-pages/man1/bc.1p.html)

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

[***manual***](http://man7.org/linux/man-pages/man1/man.1.html)

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

who：查看目前有谁在线上。[***manual***](http://man7.org/linux/man-pages/man1/who.1.html)
netstat -a：查看网络连接状态。[***manual***](http://man7.org/linux/man-pages/man8/netstat.8.html)
ps -aux：看背景执行程序。[***manual***](http://man7.org/linux/man-pages/man1/ps.1.html)

---

### 将数据同步写入磁盘-sync

Linux中为了性能，载入文件之后，对它的修改不会立刻写回硬盘当中。

sync命令会将在内存中尚未更新的数据，写入硬盘中。

必须要超级管理员root才能执行。

一般的shutdown, reboot指令默认会执行sync。

[***manual***](http://man7.org/linux/man-pages/man1/sync.1.html)

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

[***manual***](http://man7.org/linux/man-pages/man2/shutdown.2.html)

---

### 重新开机和其他关机指令-reboot, halt, poweroff

reboot与shutdown差不多，看[***manual***](http://man7.org/linux/man-pages/man2/reboot.2.html)

halt指令会让系统停止，屏幕保留系统已经停止的讯息。看[***manual***](http://man7.org/linux/man-pages/man8/halt.8.html)

poweroff指令，另一个关机指令。看[***manual***](http://man7.org/linux/man-pages/man8/halt.8.html)

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
  - 第一个字符代表文件类型
    - d是目录
    - \-是文件
    - l是链接文件
    - b是设备文件里面的块设备文件
    - c为设备文件里面的字符设备文件
  - 接下来的三个一组，均是rwx的组合，r代表可读，w可写，x可执行。
    - 第一组为文件拥有者的权限
    - 第二组为此文件群组的权限
    - 第三组为其他人的权限
- 链接
  - 代表有多少个文件名链接到此节点（i-node）

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
  - 纯文本文件
  - 二进制文件
  - 数据格式文件
- 目录
- 链接文件
- 设备与设备文件
  - 区块设备文件
  - 字符设备文件
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

[***manual***](http://man7.org/linux/man-pages/man1/cd.1p.html)

#### 显示当前工作目录-pwd

格式：pwd [-P]

选项：
- -P：显示出实际路径，而非链接路径

[***manual***](http://man7.org/linux/man-pages/man1/pwd.1.html)

#### 创建新目录-mkdir

格式：mkdir [-mp] *目录名*

选项：
- -m：设置文件的权限，覆盖默认的umask。
- -p：创建目录时把未存在的上层目录也会创建

[***manual***](http://man7.org/linux/man-pages/man1/mkdir.1.html)

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
- ls [--color={never, auto, always}] *文件名或目录名称*
- ls [--full-time] *文件名或目录名称*

选项（**加粗代表常用**）：
- **-a：全部的文件，包括隐藏文件都列出来**
- -A：除去.和..目录之外的全部文件，包括隐藏文件
- **-d：仅列出目录本身，而不列出目录内的数据**
- -f：不排序直接列结果，ls默认会按文件名排序
- -F：根据文件、目录等的信息，给予附加数据结构，如`*`代表可执行文件；`/`代表目录；`=:`代表socket文件；`|:`代表FIFO文件
- -h：将文件大小以容易阅读的格式（如多少GB，KB）而不是字节数列出来。
- -i：列出inode号码
- **-l：列出详细数据**
- -n：列出UID和GID而不是用户和群组名
- -r：将排序结果反转过来输出
- -R：把子目录的内容也列出来
- -S：以文件大小排序
- -t：以时间排序
- --color=never：不显示颜色
- --color=always：依据文件格式特征显示颜色
- --color=auto：系统判断是否开启颜色显示
- --full-time：以完整的时间格式输出
- --time={atime, ctime}：输出最后获取文件的时间（atime）或者改变权限的时间（ctime），而不是默认的最后修改时间（mtime）

[***manual***](http://man7.org/linux/man-pages/man1/ls.1.html)

---

### 复制、删除、移动-cp,rm,mv

#### 复制指令-cp

格式：
- cp [-adfilprsu] *来源文件* *目标文件*
- cp [options] source1 source2 ... destination

选项：
- **-a：相当于`-dr --preserve=all`**
- -d：若源文件为链接文件，则保留链接属性
- -f：已存在目标文件，则强制执行，删除后再尝试
- **-i：若目标文件已存在，会先询问再复制**
- -l：创建硬链接，而不是复制文件
- **-p：复制文件及其原本的属性**
- **-r：递归复制，复制子目录内的所有文件，复制目录时必须加**
- -s：创建符号链接，而不复制文件
- -u：当目标文件比来源文件要旧的时候，或者目标文件不存在时，才更新目标文件
- --preserve=all：除了-p外，还加入SELinux的属性，把links，xattr也复制

注意，若源文件有两个及以上的话，目标文件必须是目录。

默认情况下，复制过来的目标文件的拥有者会变成下达cp指令的用户。

复制文件前，最好先思考下面几个问题，以此选择复制的选项：
- 是否需要完整保留源文件信息？
- 源文件是否为链接文件？
- 源文件是否为特殊文件，如FIFO，socket等？
- 源文件是否为目录？

[***manual***](http://man7.org/linux/man-pages/man1/cp.1.html)

#### 移除指令-rm

格式：rm [-fir] 文件或目录

选项：
- -f：强制执行，忽略不存在的文件
- -i：删除前询问
- -r：递归删除，删除整个文件夹

现在许多发行版已经自动加入-i的选项了

[***manual***](http://man7.org/linux/man-pages/man1/rm.1.html)

#### 移动或更名指令-mv

格式：
- mv [-fiu] source destination
- mv [options] source1 source2 ... directory

选项：
- -f：强制执行
- -i：覆盖前询问
- -u：若源文件比目标文件新，则更新目标文件

改名方法：目标文件名写成想改的文件名就可以了。例如`mv foo foo1`。

[***manual***](http://man7.org/linux/man-pages/man1/mv.1.html)

---

### 获取路径的文件名称和目录名称

使用`basename 文件名`获取文件名。使用`dirname 文件名`获取目录名

---

## 文件内容查阅

---

### cat指令

格式：cat [-AbEnTv] *文件名*

选项：
- -A：相当于-vET，显示特殊字符
- -b：显示行号，空白行没有行号
- -E：显示换行符$
- **-n：显示行号，空白行也有行号**
- -T：将制表符[tab]以^I显示出来
- -v：显示看不出的特殊字符

cat适合用于文件内容较少的文件的查阅

[***manual***](http://man7.org/linux/man-pages/man1/cat.1.html)

### tac指令

tac就是cat反过来写，所以，是反过来显示文件内容。选项是差不多的

### 带行号打印-nl

格式：nl [-bnw] *文件名*

选项：
- -b：指定行号显示方式，主要有两种
  - -b a：空行也会显示行号
  - -b t：空行不显示（默认）
- -n：列出行号表示的方法，主要有三种
  - -n ln：行号在屏幕的左方显示
  - -n rn：右方显示，且不加0
  - -n rz：右方显示，且加0
- -w：指定行号占用的字符数

[***manual***](http://man7.org/linux/man-pages/man1/nl.1.html)

---

### 可翻页显示

#### more指令

打开文件直接`more 文件名`即可

more内的指令：

|指令|功能|
|空白键|向下翻一页|
|Enter|向下翻一行|
|/*str*|向下寻找str字符串|
|:f|立即显示文件名以及目前行数|
|q|离开more|
|b或[ctrl]-b|往回翻页，对管道命令无用|

[***manual***](http://man7.org/linux/man-pages/man1/more.1.html)

#### less指令

提供比more更灵活的翻页方式，可以前后翻页和前后搜寻

less内的命令：

|指令|功能|
|空白键|向下翻一页|
|[page down]|向下翻一页|
|[page up]|向上翻一页|
|/*str*|向下搜寻字符串str|
|?*str*|向上搜寻字符串str|
|n|重复前一次搜索|
|N|反向重复前一次搜索|
|g|跳到第一行|
|G|跳到最后一行|
|q|离开|

man用的其实就是less指令

[***manual***](http://man7.org/linux/man-pages/man1/less.1.html)

---

### 数据截取指令

#### 取出前面几行-head

格式：head [-n number] *文件名*

选项：
- -n：接数字，代表显示几行。正数时代表前`number`行，负数时代表除去后`number`行外的前面所有行

默认显示前10行。

[***manual***](http://man7.org/linux/man-pages/man1/head.1.html)

#### 取出后面几行-tail

格式：tail [-n number] *文件名*

选项：
- -n：接数字，代表几行的意思。正数代表后`number`行，负数代表`number`行以后的数据。
- -f：当文件内容增加时，继续输出内容，直到按下[ctrl]-c

[***manual***](http://man7.org/linux/man-pages/man1/tail.1.html)

可以使用管线命令（|）来做更多选取，如选取中间行。

---

### 查阅非纯文本文件指令-od

格式：od [-t TYPE] 文件

选项：
- -t：后面接文件类型，如：
  - a：默认字符输出
  - c：使用ASCII字符输出
  - d[*size*]：十进制输出，每个整数占用size字节
  - u[*size*]：无符号十进制数，每个数占用size字节
  - o[*size*]：八进制输出，每个数占用size字节
  - x[*size*]：十六进制输出，每个数占用size字节
    - size可为下列几个选项：
      - C：等同于sizeof(char)
      - S：sizeof(short)
      - I：sizeof(int)
      - L：sizeof(long)
  - f[*size*]：浮点输出，每个浮点数展示用size字节
    - size可为F代表sizeof(float)，D代表sizeof(double)或L代表sizeof(long double)

[***manual***](http://man7.org/linux/man-pages/man1/od.1.html)

---

### 修改文件时间或创建新文件指令-touch

每个文件都有3个时间记录，分别是：
- mtime：最后修改时间
- ctime：最后权限或属性更改时间
- atime：最后获取时间

修改上面三个时间，可用touch指令

格式：touch [-acdmt] *文件名*

选项：
- -a：仅修改最后获取时间
- -c：仅修改ctime，若文件不存在则不创建文件
- -d：后面接日期，也可用`--date='日期或时间'`
- -m：仅修改最后修改时间
- -t：后面接时间，格式为`YYYYMMDDhhmm`

若touch接不存在的文件名，则会创建一个新的空的文件

[***manual***](http://man7.org/linux/man-pages/man1/touch.1.html)

---

## 文件与目录的默认权限与隐藏权限

---

### 文件默认权限-umask

默认权限上，文件和目录不同。文件默认最大666，而目录默认则为777。

umask指定用户创建文件夹或文件的权限时，默认值需要减掉的分数。

直接输入umask可看到默认权限数字，若加入-S选项，则有符号解释。umask后面跟着3位数字，直接设置umask。

umask由四个数字组成，其中第一位是特殊权限，后面讲解。

[***manual***](http://man7.org/linux/man-pages/man2/umask.2.html)

---

### 文件隐藏属性

#### 设置文件隐藏属性-chattr

格式：chattr [+-=][ASacdistu] *文件或目录名称*

选项：
- +：增加指定参数，其余不变
- -：移除指定参数，其余不变
- =：设置权限为后面指定的权限
- A：不修改atime
- S：对文件修改后，同步写入磁盘，默认是不同步的
- **a：只能增加数据，不能删除也不能修改数据。只有root能设置**
- c：写入时自动压缩文件，读取时自动解压缩
- d：当dump程序执行的时候，指定该文件不会被dump备份
- **i：不能被删除，改名，设置链接，也无法写入或新增数据。只有root能设置**
- s：若文件被删除，将会立即被完全删除。
- u：若文件被删除，数据还会保留在硬盘中，可用于拯救文件

注意：
- 有一些设置是必须root才能设置的，如a和i
- xfs只支持AadiS权限

[***manual***](http://man7.org/linux/man-pages/man1/chattr.1.html)

#### 显示文件隐藏属性-lsattr

格式：lsattr [-adR] *文件名或目录*

选项：
- -a：隐藏文件的属性也列出来
- -d：显示目录本身的属性，而不是目录内的文件的属性
- -R：子目录的数据也列出来

[***manual***](http://man7.org/linux/man-pages/man1/lsattr.1.html)

**这些文件隐藏属性对系统安全非常重要，请谨慎设置！**

---

### 文件特殊权限-SUID SGID SBIT

#### Set UID

当s出现在文件拥有者的x权限上时，被称为Set UID，简称SUID的特殊权限。有这样的限制与功能：
- SUID仅对二进制程序有效
- 执行者对该程序需要有x的可执行权限
- 权限仅在该程序运行过程中有效
- 执行者将拥有该程序拥有者的权限

例如对密码文件/etc/shadow来说，权限就是`---------- 1 root root`，但是在执行passwd指令却可以修改密码，因为passwd指令对应二进制文件的权限为`-rwsr-xr-x 1 root root`。因此，在运行passwd命令的时候，可以让执行者暂时拥有root的权限，去修改/etc/shadow文件。

[***manual***](http://man7.org/linux/man-pages/man2/setuid.2.html)

#### Set GID

当s标志出现在文件群组权限的x位上时，即成为Set GID权限，简称SGID。跟SUID类似，只不过是让执行者暂时拥有该文件群组的权限。如/usr/bin/locate文件就有SGID权限。

而SGID用在目录的时候，就会有如下的功能：
- 若用户对该目录有r和x的权限，则用户能进入该目录
- 用户在该目录下时，有效群组变为该目录的群组
- 若用户拥有w权限，则用户创建新文件时，该文件的群组和目录的群组相同。

[***manual***](http://man7.org/linux/man-pages/man2/setgid.2.html)

#### Sticky Bit

Sticky Bit，简称SBIT，仅对目录有效。作用是若使用者拥有此目录的w和x权限，当他在这个目录下创建文件或者目录的时候，仅有自己和root有权利删除该文件或目录。如/tmp目录就是有SBIT权限的。

[***manual***](http://man7.org/linux/man-pages/man1/chmod.1.html#RESTRICTED_DELETION?FLAG?OR?STICKY?BIT)

#### SUID SGID SBIT的设置

原本权限的3个数字前面的一位，就是设置这三个权限的位置，而三个权限的数字分别为
- 4：SUID
- 2：SGID
- 1：SBIT

注意，因为SUID，SGID需要执行者有x权限，没有的时候设置的话会变成大写的S，代表没有x权限，SUID，SGID无效。SBIT同理。

---

### 查看文件类型-file

格式：file *文件名*

[***manual***](http://man7.org/linux/man-pages/man1/file.1.html)

---

## 指令与文件的搜寻

---

### 指令文件的搜寻

#### 寻找可执行文件-which

格式：which [-a] *command*

选项：
- -a：将所有由PATH目录中找到的指令列出，默认只列第一个

搜寻的指令有别名的时候，也会写出别名的格式。

bash内置指令无法搜索到，如history指令。应使用type指令。

---

### 文件的搜寻

#### 特定目录寻找文件-whereis

格式：whereis [-bmsu] *文件或目录名*

选项：
- -l：列出whereis去寻找的目录
- -b：只找二进制文件
- -m：只找manual路径下的文件
- -s：只找source文件
- -u：找不属于上面3个类型的其他文件

whereis因为只找几个特定的目录，执行文件目录，源代码目录和说明文件目录，找寻这些目录的时候会比其他的搜索指令快。

[***manual***](http://man7.org/linux/man-pages/man1/whereis.1.html)

#### locate和updatedb指令

格式：locate [-ir] *keyword*

选项：
- -i：忽略大小写
- -c：只显示找到的文件数量
- -l：后面指定输出几行
- -S：输出locate所使用的数据库文件的信息，包括数据库记录的文件或目录数量等
- -r：接正则表达式的表示方式

locate使用/var/lib/mlocate里面的数据去寻找文件，因此也是特别快速。但是，数据库的更新默认每天一次。

更新数据库，使用`updatedb`命令。updatedb会读取/etc/updatedb.conf文件的设置去进行搜寻文件的动作。

[***locate manual***](http://man7.org/linux/man-pages/man1/locate.1.html)

[***updatedb manual***](http://man7.org/linux/man-pages/man1/updatedb.1.html)

#### 最强大搜寻指令-find

在指定文件夹及其子目录搜索任意文件的指令，选项非常多，很强大！但是很耗时间和硬盘资源。

格式：find [PATH] [option] [action]

选项：
- 与时间有关的选项
  - -atime, -mtime, -ctime *\[+-\]number*：number为整数。无符号数表示第number天前一天之内的时间，加+号表示n天之前的时间，不包括n天，而加-号则表示n天之内包含n天时间。  - -newer *文件名*：表示比指定文件名更新的文件列出来。
- 与用户和群组有关的选项
  - -uid *n*：拥有者用户id为n
  - -gid *n*：文件拥有群组的id为n
  - -user *name*：拥有者用户名为name
  - -group *name*：拥有群组名为name
  - -nouser：拥有者不存在与/etc/passwd的文件
  - -nogroup：拥有群组不存在于/etc/group的文件（外部传来的文件，可能是没有拥有者和拥有群组的）
- 与文件权限与名称有关的选项
  - -name *filename*：寻找文件名为filename的文件
  - -size [+-]*SIZE*：寻找比SIZE要大(+)或小(-)的文件
    - 单位有：c是Byte，k是1024Bytes。
  - -type *TYPE*：寻找文件类型为TYPE的文件，主要有：
    - f：一般文件
    - b、c：设备文件
    - d：目录
    - l：链接文件
    - s：socket文件
    - p：FIFO文件
  - -perm *mode*：文件权限为mode的文件，类似chmod的数字属性值
  - -perm -*mode*：寻找包括mode权限的文件，也就是权限里面包括有mode指定的权限都列出来，注意不加`-`的区别
  - -perm /*mode*：包含mode的任意权限的文件，只要有mode中指定的一个或多个权限，就列出来
  - 额外的动作(action)选项：
    - -exec *command*：执行command来处理搜索结果。command中，用`{}`代表find找到的文件，用`\;`代表command的结束
    - -print：打印结果到屏幕，默认动作

[***manual***](http://man7.org/linux/man-pages/man1/find.1.html)

---

# 第七章 Linux磁盘与文件系统管理

---

## 认识Linux文件系统

---

### 文件系统特性

磁盘分区完毕之后，必须经过格式化才能使用，因为不同操作系统设置的文件属性和权限不相同，格式化之后，变成操作系统能使用“文件系统格式”。

传统的文件系统中，一个分区只能被格式化为一个文件系统。而新技术的使用，如LVM和磁盘阵列，使得一个分区能格式化为多个文件系统，或者多个分区能合成一个文件系统。

新的操作系统的文件数据，会包括文件实际内容以及权限属性等。权限和属性，放置在**inode**当中，实际数据放在**data block**当中。还有一个**superblock**放置整个文件系统的信息，如inode和block的总量，使用量，剩余量等。

简要介绍如下：
- superblock：记录文件系统的元信息，包括inode和block的总量、使用量、剩余量，以及文件系统的格式和相关信息等。
- inode：有编号，记录文件的属性，一个文件一个inode，同时记录文件数据所在的block号码。
- block：有编号，记录文件的实际内容。block固定大小，一个文件有一个或多个block组成。

---

### Linux的Ext2文件系统

Ext2是以inode为基础的文件系统。

为了让文件系统很大的时候易于管理，减少inode和block的数量，Ext2文件系统会把整个文件系统分为多个区块群组（block group），每个区块有独立的superblock/inode/block。

Ext2文件系统的限制：
- 能够支持2GB以上的文件，但是可能有些程序不能使用。
- block的大小和数量在格式化时确定，不能再更改
- 每个block内最多放置一个文件的数据
- 若一个block放不下整个文件，则会用多个block。
- 最后一个block的剩余空间无法被其他文件使用

每个群组的6个主要内容，如后几小节所述。

- **data block**

存放数据内容的地方。Ext2支持的block大小为1KB，2KB，4KB。文件限制如下：

|Block大小|1KB|2KB|4KB|
|单文件最大限制|16GB|256GB|2TB|
|文件系统最大容量|2TB|8TB|16TB|

- **inode table**

inode记录的内容如下：
- 存取权限（read/write/execute）
- 拥有者和群组（owner/group）
- 文件大小
- 创建或状态改变的时间（ctime）
- 最后读取时间（atime）
- 最近修改时间（mtime）
- 文件特殊的权限旗标（flag），如SUID等
- 文件数据的指针（pointer）区，指的是block号码

inode的特点：
- 每个inode大小固定为128Bytes（Ext4和xfs可以到256Bytes）
- 每个文件一个inode
- 文件系统能创建的文件数量与inode数量有关
- 读取文件时，需要先找到inode，分析inode记录的权限，是否准许用户读取，若是，才开始读取block的内容。

inode的block号码区域，分成了12个直接、1个间接、1个双间接和1个三间接记录区。间接记录区指的是用一个block作为记录block号码的记录区。双间接则再加一个间接层。

因此，算下来最大可以支持16G*Block大小的文件。

- **Superblock**

记录的信息如下：
- block与inode的总量
- 未使用和已使用的block与inode的总量
- block与inode的大小
- 文件系统挂载时间，最近一次写入数据的时间，最近一次硬盘检查的时间等相关信息
- valid bit，表示文件系统是否被挂载，1为挂载了

每个区块群组都可能有一个superblock。第一个群组一定有，后续的群组可能有，作为第一个superblock的备份。

- **Filesystem Description**

描述每个block group开始和结束的block号码，以及每个区段（superblock, bitmap, inodemap, data block）位于哪个block之间。

- **block bitmap**

记录哪些block是空的结构。

- **inode bitmap**

记录哪个inode未使用的结构。

#### 查询Ext家族superblock信息的指令-dumpe2fs

格式：dumpe2fs [-bh] *设备文件名*

选项：
- -b：列出保留为坏轨的部分
- -h：仅列出superblock的数据，不列出其他内容

[***manual***](http://man7.org/linux/man-pages/man8/dumpe2fs.8.html)

#### 列出系统所有格式化设备的指令-blkid

[***manual***](http://man7.org/linux/man-pages/man8/blkid.8.html)

---

### 与目录树的关系

- **目录**

目录拥有一个inode和至少一个block。其中，block记录的是该目录下的文件以及该文件的inode号码

- **文件**

新文件会有一个inode以及放下该文件的block

- **目录树的读取**

读取从根目录开始读起，读取根目录的inode之后，一层一层的读取文件夹的inode以及block，最终读取到目的文件。

---

### Ext2 Ext3 Ext4文件的存取与日志式文件系统的功能

在文件系统中新增一个文件时，步骤有：
1. 确定使用者对新增文件的目录是否具有w和x权限，若有才新增
2. 根据inode bitmap找到没有使用的inode号码，并将新的文件的权限和属性写入
3. 根据block bitmap找到没有使用的block号码，并将实际的数据写入到block中，并且更新inode中block的指针
4. 将刚刚写入的inode和block数据更新到inode bitmap和block bitmap，并更新superblock的内容

早期的Ext2文件系统，在不明原因终端的时候，步骤没有完成，会进入一种不一致状态。此时，若重新启动，需要查看superblock中valid bit和filesystem state状态来判断是否强制执行数据一致性检查。若需要，运行e2fsck检查。这个检查需要扫描整个文件系统，非常费时。

#### 日志式文件系统

为避免不一致的发生，在文件系统中专门划出一块位置，用来存放文件存取的记录。此时，写入文件操作变成下面的步骤：
1. 预备：当系统需要写入文件时，先在日志记录区域记录某个文件准备要写入的信息
2. 实际写入：开始写入文件的权限与数据，开始更新metadata的数据
3. 结束：完成数据和metadata的更新后，与日志记录区块当中完成该文件的记录

在系统崩溃后，可以直接查看日志信息中不一致的部分，使修复时间大大的减少。

---

### Linux文件系统的运行

Linux使用非同步处理的方式，减少硬盘IO的次数，从而提升系统的性能。做法如下所述：

当一个文件载入到内存以后，如果文件没有更改，则标记为干净（clean），若更改过，则标记为脏的（dirty）。这些动作都在内存中执行，并没有写入磁盘。系统会不定时的将dirty的数据写入到磁盘中，以保证一致性。也可以用sync命令来强迫写入。

Linux文件系统与内存：
- 系统会将常用的文件放置到内存的缓冲区，也因此，Linux的物理内存很快会被用光
- 手动使用sync将dirty文件写回硬盘
- 正常关机时，关机指令会调用sync将内存数据写入
- 若没有正常关机，由于数据没有写回硬盘，可能会导致文件数据丢失。
