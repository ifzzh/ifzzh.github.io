---
layout: post
title: "在CentOS上修改DNS的记录"
date:   2024-7-31
tags: [CentOS, DNS]
comments: true
author: ifzzh
---

<!-- ###### 说明： -->

<!-- more -->


<link rel="stylesheet" type="text/css" href="../css/auto-title-number.css" />

## 目录

- [目录](#目录)
- [前言](#前言)
- [方法一：限制NetworkManager修改resolv.conf的能力](#方法一限制networkmanager修改resolvconf的能力)
- [方法二：利用NetworkManager自定义DNS](#方法二利用networkmanager自定义dns)


## 前言

目前比较新的Linux发行版都已经采用`NetworkManager`来管理网络了，自定义DNS变得更加麻烦：每次`NetworkManager`启动都会覆盖`/etc/resolv.conf`，因此仅仅在`/etc/resolv.conf`中修改是不保险的。

## 方法一：限制NetworkManager修改resolv.conf的能力

```bash
sudo chattr +i /etc/resolv.conf
```
这个命令将`/etc/resolv.conf`设置为不可修改，如此一来自定义DNS就不会被覆盖了


## 方法二：利用NetworkManager自定义DNS

```bash
sudo nano /etc/NetworkManager/conf.d/dns.conf
# 增加如下两行
[main]
dns=null

sudo nano /etc/NetworkManager/conf.d/dns-servers.conf
# 自定义DNS
[global-dns-domain-*]
servers=::1,127.0.0.1,8.8.8.8

# 最后重启软件
sudo systemctl restart NetworkManager
```