---
title: Linux学习笔记
date: 2017-03-30 18:48:07 +0800
tag: 笔记, Linux
---

补回我一直没有做的linux学习笔记

#目录


<!-- vim-markdown-toc Redcarpet -->
* [第二章](#第二章)
    * [各硬件设备在Linux中的文件名](#各硬件设备在linux中的文件名)
    * [分区表格式](#分区表格式)
        * [MSDOS分区表格式](#msdos分区表格式)
        * [GUID partition table， GPT磁盘分区表](#guid-partition-table，-gpt磁盘分区表)
* [第四章](#第四章)
    * [基础指令的操作](#基础指令的操作)
        * [显示日期的指令-date](#显示日期的指令-date)
        * [显示日历的指令-cal](#显示日历的指令-cal)
        * [简单计算器-bc](#简单计算器-bc)
    * [重要的热键](#重要的热键)

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

### GUID partition table， GPT磁盘分区表

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
- date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]

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


