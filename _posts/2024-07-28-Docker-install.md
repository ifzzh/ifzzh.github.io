---
layout: post
title: "Docker的安装与卸载"
date:   2024-7-28
tags: [K8S]
comments: true
author: ifzzh
---

> CentOS 7

<!-- ###### 说明： -->

<!-- more -->


<link rel="stylesheet" type="text/css" href="../css/auto-title-number.css" />

## 目录

- [目录](#目录)
- [docker安装](#docker安装)
  - [卸载旧版本](#卸载旧版本)
  - [设置存储库](#设置存储库)
  - [安装最新版本的Docker引擎和容器](#安装最新版本的docker引擎和容器)
  - [安装指定版本](#安装指定版本)
  - [启动docker](#启动docker)
- [卸载docker（如需要）](#卸载docker如需要)




## docker安装

### 卸载旧版本

```bash
# 1. 卸载之前安装的组件
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

```

### 设置存储库
```bash
# 1.安装yum-utils
sudo yum install -y yum-utils

# 2.设置稳定的存储库
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 下面是官网的, 上面使用阿里云的 国内比较快
#    https://download.docker.com/linux/centos/docker-ce.repo
```

### 安装最新版本的Docker引擎和容器

```bash
# 1. 直接安装最新docker版本
sudo yum -y install docker-ce docker-ce-cli containerd.io

# 2. 安装完成后查看版本
docker -v
```

### 安装指定版本
```bash
# 如果想安装不同版本
# 1.列出可用版本
yum list docker-ce --showduplicates | sort -r

# 2.安装指定版本
sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
如:
sudo yum install docker-ce-18.09.1 docker-ce-cli-18.09.1 containerd.io

```

### 启动docker
```bash
# 启动docker
systemctl start docker

# 停止docker
systemctl stop docker

# 重启
systemctl restart docker

# 查看状态
systemctl status docker

# 设置开机自启动
systemctl enable docker

# 查看信息
docker info

# 查看帮助文档
docker --help
```
## 卸载docker（如需要）
```bash
# 1.卸载 Docker 引擎、CLI 和容器包
sudo yum remove docker-ce docker-ce-cli containerd.io

# 2.主机上的图像、容器、卷或自定义配置文件不会自动删除。要删除所有图像、容器和卷
sudo rm -rf /var/lib/docker

# 3.必须手动删除任何编辑的配置文件。
```