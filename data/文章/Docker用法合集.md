---
title: Docker用法合集
date: 2024-05-24 06:23:00
categories: default
tags: []
---
docker命令大全
```
docker build -t 镜像 .
docker run -d -p 端口:映射端口 --name 名称 --restart always 镜像名 
参数:
-d 后台运行
-e 变量
-t 分配虚拟终端
-
docker start 容器
docker kill 容器
docker exec -it 容器 命令
```

docker常用配置文件
首先`vim /etc/docker/daemon.json`
然后
```
{
  "registry-mirrors": [
    "https://mirror.ccs.tencentyun.com"
  ],
  "bip": "192.168.100.1/24",
  "default-address-pools": [
    {
      "base": "192.168.100.0/16",
      "size": 24
    }
  ]
}
```
registry-mirrors指的是镜像加速，bip指的是docker容器网段区域，default-address-pools指的是默认分配网段区域

