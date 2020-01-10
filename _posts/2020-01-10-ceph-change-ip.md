---
layout:     post
title:      "正确修改Ceph集群IP地址"
author:     "吴俊贤"
date:       2020-01-10 09:43:25
tags:
    - Ceph
---

当我们需要把整个Ceph集群的网络地址修改为另一个子网地址（与ceph.conf的public addr等不相同）时，就需要更改Ceph Monitor，Ceph OSD，Ceph Manager的监听地址。其中，OSD和Manager的地址是根据ceph.conf文件中的配置项public addr与cluster addr等确定的，因此只需修改文件内容即可。而Ceph Monitor的地址则是由monmap维护的，直接修改monmap会产生不可预料的问题。本文将会给出如何在不直接修改monmap的情况下修改Mon的IP地址。

# 样例集群

原Ceph集群部署在10.1.1.0/24子网上，现在需要部署到192.168.1.0/24子网上，且机器已经配置了192.168.1.0/24中的地址。

用户相关设置，ceph的进程将由ceph用户执行，该用户为部署ceph时自动创建的用户。而执行命令的用户为某一拥有sudo权限的普通用户。这个很重要，官方文档没有说明用户权限的重要性，导致根据[Adding/Removing Monitors](https://docs.ceph.com/docs/nautilus/rados/operations/add-or-rm-mons/)一文的Add Monitor操作，将会引起权限的错误。以下所有的命令均使用普通用户执行。

# 配置网卡IP

由于机器的地址已经发生改变，Ceph集群的monitors之间必然无法达成quorum。首先我们需要让其达成quorum，才好进行手动添加和删除monitor的操作。

因此，我们需要把原本的地址配置一下。配置的方法，遵循给网卡添加IP地址的方法。假设旧地址为`10.1.1.1`，新地址为`192.168.1.1`。若使用`ip命令`，则为

```bash
$ ip addr add 10.1.1.1/24 dev <网卡设备名称>
```

使用nmcli的话，则为

```bash
# 方括号内容可以省略以缩短命令长度
$ nmcli c[onnection] m[odify] <设备名或网络配置名> ipv4.addr[ess] "10.1.1.1/24, 192.168.1.1/24"
```

所有机器配置好后，应该能够达成quorum，若不行，则使用systemctl重启mon应该能解决。

# 添加新的Monitor

修改IP地址的时候官方推荐添加新的IP的Monitor，而不是改原有的。假设新的monitor称为`amaster`，原有monitor称为`master`。在此提醒，新的monitor名称不能与原有的重复。

首先创建新的monitor默认使用的文件夹。由于`/var/lib/ceph`文件夹为ceph用户所有，我们需要使用sudo。一般sudo仅能使用root用户，若能以ceph用户执行命令，则省去修改拥有者的麻烦。

```bash
$ sudo mkdir /var/lib/ceph/mon/ceph-amaster
```

此时应有

```bash
$ ls -l /var/lib/ceph/mon/
drwxr-x--- 15 ceph          ceph          4.0K 11月 17 09:56 ceph-master
drwxr-x--- 15 root          root          4.0K 1月  10 09:56 ceph-amaster
```

如果配置了cephx认证，则需要获取一下新的keyring。

```bash
ceph auth get mon.amaster -o <keyring文件名>
```

获取monmap

```bash
ceph mon getmap -o <monmap文件名>
```

准备一下默认文件夹的内容

```bash
ceph-mon -i amaster --mkfs --monmap <monmap文件> [--keyring <keyring文件>]
```

接下来这两步与官网的不同。先是修改monitor文件夹的拥有者，改回ceph，因为创建时为root，而monitor运行时的用户为ceph，无法访问这个文件夹。

```bash
chown -R ceph:ceph /var/lib/ceph/mon/ceph-amaster
```

然后使用systemctl功能启动ceph-monitor

```bash
systemctl start ceph-mon@amaster.service
```

可以设置一下enable，开机启动cephmon
```bash
systemctl enable ceph-mopn@amaster.service
```

按照官网的教程会有Permission Denied的问题。

# 删除旧monitor

官网的教程在这里再一次有问题，不能按照其命令的顺序来，否则可能共识出错。

首先remove掉旧的monitor。

```bash
ceph mon remove master
```

然后，再停止进程，删除symlink

```bash
systemctl stop ceph-mon@master.service
systemctl disable ceph-mon@master.service
```

至此，单个Monitor的IP就修改完了。
