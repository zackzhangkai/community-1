---
layout: post
published: true
title: 彻底告别DockerHub限速实用方法
categories: [document]
tags: [dockerhub,docker]
---
* content
{:toc}

## 背景

自从Docker从11月份开始对docker限速之后，频繁碰到限速的反馈。限速信息如下：

```bash
You have reached your pull rate limit. You may increase the limit by authenticating and upgrading: https://www.docker.com/increase-rate-limits
```

免费帐号会限制 200 pulls/6小时，匿名帐号则限制 100 pulls/6小时。

下文中提供一种搭建私有镜像方式来彻底解决这个烦恼。

## 方法

由于国内网络环境问题，我们经常使用镜像加速器，虽然可以提升镜像下载速度，但是每次都会从外网拉镜像。速度相对仍然较慢。

在这里我们配置内网的镜像仓库，第一次从外网拉取镜像后，以后每次从本地启动，下载速度可以突破10M。

首先我们新建一个VPC，给VPC配置EIP，让其从外网拉取镜像。在其中一台主机上部署私有仓库，其余节点配置从该仓库拉取。流量走内部私有网络，除了第一次有点慢，之后每次速度都超快，而且告别了Docker镜像拉取限制。

## 部署私有仓库

该主机IP 192.168.0.10

1. 首先创建个本地volume，持久化镜像文件

```bash
docker volume create docker-repo
```

2. 使用容器启动服务

```bash
docker run --name=cache -d --restart=always docker-repo:/var/lib/registry -p 5000:5000 registry:2
```

现在已经启动了一个本地registry，监听端口为5000。此时的Registry地址为：192.168.0.10:5000

## 使用方法

在其余主机上配置如下：

```bash
# cat /etc/docker/daemon.json
{
        "registry-mirrors": [
                "http://192.168.0.10:5000"
        ]
}
```

reload docker

```bash
systemctl reload docker
```
