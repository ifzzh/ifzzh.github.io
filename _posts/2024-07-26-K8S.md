---
layout: post
title: "K8S集群删除Pod方法"
date:   2024-7-26
tags: [K8S, delete-pod]
comments: true
author: ifzzh
---

<!-- ###### 说明： -->

<!-- more -->

## 目录

- [目录](#目录)
- [常规方法](#常规方法)
- [强制删除](#强制删除)
- [强制删除无法解决时](#强制删除无法解决时)

## 常规方法

```bash
kubectl delete deployment [deployment-name]
kubectl delete pod [pod-name]
```

## 强制删除

```bash
kubectl delete pod <pod-name> --force --grace-period=0
```

## 强制删除无法解决时

```bash
# 先查看pod的名字
kubectl get pod -n [ns]
# 查看该pod的类型（Controlled By字段）
kubectl describe pod -n [ns] [pod-name]
# 查看对应的类型，获得pod对应的name
kubectl get [pod type] -n ns
# 在namespace之前加上刚刚查到的name
kubectl delete [type] [name] -n [ns] --force --grace-period=0
```
