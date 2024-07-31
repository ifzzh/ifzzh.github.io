---
layout: post
title: "升级CentOS内核"
date:   2024-7-29
tags: [CentOS, kernel]
comments: true
author: ifzzh
---

<!-- ###### 说明： -->

<!-- more -->


<link rel="stylesheet" type="text/css" href="../css/auto-title-number.css" />

## 目录

- [目录](#目录)
- [选定版本](#选定版本)
- [下载内核RPM](#下载内核rpm)
- [安装内核](#安装内核)
- [确认已安装内核版本](#确认已安装内核版本)
- [设置启动顺序](#设置启动顺序)


## 选定版本

[查找 kernel rpm 历史版本](https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/)

这里以安装 LT 内核的5.4.278版本为例

## 下载内核RPM

共需下载三个类型rpm
```
wget https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-lt-5.4.278-1.el7.elrepo.x86_64.rpm
wget https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-lt-devel-5.4.278-1.el7.elrepo.x86_64.rpm
wget https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-lt-headers-5.4.278-1.el7.elrepo.x86_64.rpm
```

## 安装内核
```bash
rpm -ivh kernel-lt-5.4.278-1.el7.elrepo.x86_64.rpm
rpm -ivh kernel-lt-devel-5.4.278-1.el7.elrepo.x86_64.rpm
```

## 确认已安装内核版本
```bash
rpm -qa | grep kernel
```

## 设置启动顺序

```bash

```