---
layout: post
title: "Harbor仓库构建K8S集群部署"
date:   2024-7-26
tags: [K8S]
comments: true
author: ifzzh
---

<!-- ###### 说明： -->

<!-- more -->

## 目录

- [目录](#目录)
- [harbor自建节点](#harbor自建节点)
- [主机硬件配置说明](#主机硬件配置说明)
- [](#)

## harbor自建节点


## 主机硬件配置说明

|主机名|IP|角色|CPU|内存|
|:-: | :-:|:-:|:-:|:-:|
|master01|223.193.36.152|master,etcd|8C|32G|
|master02|223.193.36.155|master,etcd|8C|32G|
|node01|223.193.36.179|worker|8C|32G|
|node02|223.193.36.182|worker|8C|32G|
|harbor|223.193.36.117|repository|8C|32G|

## 