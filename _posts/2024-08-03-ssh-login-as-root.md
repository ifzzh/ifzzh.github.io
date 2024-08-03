---
layout: post
title: "拥有root权限后以root用户登录的方法"
date:   2024-8-3
tags: [root, ssh, Virtual Machine]
comments: true
author: ifzzh
---

<!-- ###### 说明： -->

<!-- more -->


<link rel="stylesheet" type="text/css" href="../css/auto-title-number.css" />

## 目录

- [目录](#目录)
- [获取root权限](#获取root权限)
- [修改配置文件](#修改配置文件)
- [重启sshd服务](#重启sshd服务)

## 获取root权限

```bash
sudo su
```

## 修改配置文件

```bash
# 进入配置文件
vi /etc/ssh/sshd_config

# 定位到 PermitRootLogin
/PermitRootLogin

# 将注释符删掉，进行如下修改
PermitRootLogin yes
```

## 重启sshd服务
```bash
service sshd restart
```