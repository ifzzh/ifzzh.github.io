---
layout: post
title: "重建服务器记录"
date:   2024-8-5
tags: [cnic, server, Virtual Machine]
comments: true
author: ifzzh
---

<!-- ###### 说明： -->

<!-- more -->


<link rel="stylesheet" type="text/css" href="../css/auto-title-number.css" />

## 目录

- [目录](#目录)
- [进入虚拟机镜像所在目录](#进入虚拟机镜像所在目录)
- [删除需要重建的虚拟机的镜像文件](#删除需要重建的虚拟机的镜像文件)
- [将对应虚拟机关闭](#将对应虚拟机关闭)
- [从bak复制对应镜像到镜像目录](#从bak复制对应镜像到镜像目录)
- [启动对应虚拟机](#启动对应虚拟机)

##  进入虚拟机镜像所在目录

```bash
[root@master01 ~]# cd /home/zhengzihao/
[root@master01 zhengzihao]# ls
aware  bak  master01.qcow2  master02.qcow2  node01.qcow2  node02.qcow2
# bak 是备份镜像
[root@master01 zhengzihao]# cd bak/
[root@master01 bak]# ls
master01.qcow2  master02.qcow2  node01.qcow2  node02.qcow2
[root@master01 bak]# cd ..
[root@master01 zhengzihao]# ll
total 32600724
drwxr-xr-x. 10 root root       4096 Aug  4 03:53 aware
drwxr-xr-x.  2 root root         90 Aug  2 03:55 bak
-rw-------.  1 qemu qemu 8603369472 Aug  4 23:26 master01.qcow2
-rw-------.  1 qemu qemu 8352104448 Aug  4 23:25 master02.qcow2
-rw-------.  1 qemu qemu 8307736576 Aug  4 23:26 node01.qcow2
-rw-------.  1 qemu qemu 8118075392 Aug  4 23:26 node02.qcow2
```


## 删除需要重建的虚拟机的镜像文件
```bash
[root@master01 zhengzihao]# rm -rf master01.qcow2
[root@master01 zhengzihao]# ls
aware  bak  master02.qcow2  node01.qcow2  node02.qcow2
```

## 将对应虚拟机关闭
```bash
[root@master01 zhengzihao]# virsh list --all
 Id   Name                   State
---------------------------------------
 5    zzh-master01           running
 7    zzh-node01             running
 8    zzh-node02             running
 10   zzh-master02           running
 -    centos-stream8         shut off
 -    centos-stream8-clone   shut off
 -    master01               shut off
 -    master02               shut off
 -    node01                 shut off
 -    node02                 shut off
 -    origal                 shut off
 -    origal-clone           shut off
 -    overlay-master01       shut off
 -    overlay-master02       shut off

[root@master01 zhengzihao]# virsh shutdown zzh-master01
Domain 'zzh-master01' is being shutdown

[root@master01 zhengzihao]# virsh list --all
 Id   Name                   State
---------------------------------------
 7    zzh-node01             running
 8    zzh-node02             running
 10   zzh-master02           running
 -    centos-stream8         shut off
 -    centos-stream8-clone   shut off
 -    master01               shut off
 -    master02               shut off
 -    node01                 shut off
 -    node02                 shut off
 -    origal                 shut off
 -    origal-clone           shut off
 -    overlay-master01       shut off
 -    overlay-master02       shut off
 -    zzh-master01           shut off

 # 如果关不掉可以使用destroy
 ```

## 从bak复制对应镜像到镜像目录

```bash
[root@master01 zhengzihao]# ls
aware  bak  master02.qcow2  node01.qcow2  node02.qcow2
[root@master01 zhengzihao]# cp ./bak/master01.qcow2 ./
[root@master01 zhengzihao]# ls
aware  bak  master01.qcow2  master02.qcow2  node01.qcow2  node02.qcow2
```

## 启动对应虚拟机

```bash
[root@master01 zhengzihao]# virsh start zzh-master01
Domain 'zzh-master01' started

[root@master01 zhengzihao]# watch virsh list --all
```