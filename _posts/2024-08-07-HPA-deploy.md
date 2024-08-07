---
layout: post
title: "HorizontalPodAutoscaler演练"
date:   2024-8-7
tags: [k8s, Pod Autoscaling]
comments: true
author: ifzzh
---

<!-- ###### 说明： -->

<!-- more -->


<link rel="stylesheet" type="text/css" href="../css/auto-title-number.css" />

## 目录

- [目录](#目录)
- [运行一个php-apache服务器并暴露服务](#运行一个php-apache服务器并暴露服务)
- [创建 HorizontalPodAutoscaler](#创建-horizontalpodautoscaler)
- [增加负载](#增加负载)

##  运行一个php-apache服务器并暴露服务

为了演示 HorizontalPodAutoscaler，你将首先启动一个 Deployment 用 hpa-example 镜像运行一个容器， 然后使用以下清单文件将其暴露为一个服务（Service）：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: php-apache
spec:
  selector:
    matchLabels:
      run: php-apache
  template:
    metadata:
      labels:
        run: php-apache
    spec:
      containers:
      - name: php-apache
        image: registry.k8s.io/hpa-example
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: 500m
          requests:
            cpu: 200m
---
apiVersion: v1
kind: Service
metadata:
  name: php-apache
  labels:
    run: php-apache
spec:
  ports:
  - port: 80
  selector:
    run: php-apache
```

可以手动创建，在本地离线部署。也可以直接运行以下命令：

```bash 
kubectl apply -f https://k8s.io/examples/application/php-apache.yaml
```

## 创建 HorizontalPodAutoscaler

现在服务器正在运行，使用 kubectl 创建自动扩缩器。 kubectl autoscale 子命令是 kubectl 的一部分， 可以帮助你执行此操作。

你将很快运行一个创建 HorizontalPodAutoscaler 的命令， 该 HorizontalPodAutoscaler 维护由你在这些说明的第一步中创建的 php-apache Deployment 控制的 Pod 存在 1 到 10 个副本。

粗略地说，HPA 控制器将增加和减少副本的数量 （通过更新 Deployment）以保持所有 Pod 的平均 CPU 利用率为 50%。 Deployment 然后更新 ReplicaSet —— 这是所有 Deployment 在 Kubernetes 中工作方式的一部分 —— 然后 ReplicaSet 根据其 .spec 的更改添加或删除 Pod。

由于每个 Pod 通过 kubectl run 请求 200 milli-cores，这意味着平均 CPU 使用率为 100 milli-cores。 有关算法的更多详细信息， 请参阅算法详细信息。

创建 HorizontalPodAutoscaler：

```bash
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
```

你可以通过运行以下命令检查新制作的 HorizontalPodAutoscaler 的当前状态：

```bash
# 你可以使用 “hpa” 或 “horizontalpodautoscaler”；任何一个名字都可以。
kubectl get hpa
```

请注意当前的 CPU 利用率是 0%，这是由于我们尚未发送任何请求到服务器 （TARGET 列显示了相应 Deployment 所控制的所有 Pod 的平均 CPU 利用率）。


## 增加负载

接下来，看看自动扩缩器如何对增加的负载做出反应。 为此，你将启动一个不同的 Pod 作为客户端。 客户端向 php-apache 服务发送查询。

这里使用[hey](https://www.cnblogs.com/jspider/p/16004848.html)来测试

```bash
#下载hey
wget https://hey-release.s3.us-east-2.amazonaws.com/hey_linux_amd64
#如果下载速度较慢，可使用xiaoz软件库链接
wget http://soft.xiaoz.org/linux/hey_linux_amd64
#赋予执行权限
chmod +x hey_linux_amd64
#移动文件到sbin目录
mv hey_linux_amd64 /usr/sbin/hey
```




现在执行：

```bash
hey -n 10000 -c 10 http://php-apache/
```

如果这里找不到服务器，可以修改php-apache服务，改为NodePort方式：

```bash
kubectl edit svc php-apache

# 把type从ClusterIP改为NodePort
type : NodePort

# 保存，退出，查看服务
kubectl get svc php-apache

# 采用IP：Port的方式测试
hey -n 10000 -c 10 http://IP:Port
```

观察负载
```bash
# 准备好后按 Ctrl+C 结束观察
kubectl get hpa php-apache --watch
```

一分钟时间左右之后，通过以下命令，我们可以看到 CPU 负载升高了；例如：
```bash
NAME         REFERENCE                     TARGET      MINPODS   MAXPODS   REPLICAS   AGE
php-apache   Deployment/php-apache/scale   305% / 50%  1         10        1          3m
```

然后，更多的副本被创建。例如：

```
NAME         REFERENCE                     TARGET      MINPODS   MAXPODS   REPLICAS   AGE
php-apache   Deployment/php-apache/scale   305% / 50%  1         10        7          3m
```

这时，由于请求增多，CPU 利用率已经升至请求值的 305%。 可以看到，Deployment 的副本数量已经增长到了 7：

```bash
kubectl get deployment php-apache
```

你应该会看到与 HorizontalPodAutoscaler 中的数字与副本数匹配

```bash
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
php-apache   7/7      7           7           19m
```


>说明：
有时最终副本的数量可能需要几分钟才能稳定下来。由于环境的差异， 不同环境中最终的副本数量可能与本示例中的数量不同。



