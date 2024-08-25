---
title: 用红帽&amp;红帽衍生发行版 9来优化你的站点吧
date: 2024-07-27 07:05:36
categories: default
tags: []
---
最近，使用了红帽9.4和Rocky9.4 外带升级最新内核，再加上小许调优措施，访问速度已经达到了之前centos 8 2核4G的速度了。主要是最小化的红帽9，启动速度比7和8更加快速，只要你会手动配源就能体验到秒速启动的快感。
然后就是Centos Stream属于滚动发行版，所以红帽9可以尝试使用Rocky的源。为了让最小化安装和server差异缩小，我还特意写了个init脚本
脚本开源地址: https://gitee.com/nuoyis/shell
执行命令:
```
wget -O nuoyis-init.sh https://gitee.com/nuoyis/shell/raw/main/nuoyis-init.sh;bash nuoyis-init.sh
```
目前实现了rhel系列Linux自动配置Rocky镜像源 epel镜像源 remi源 并更新到最新内核(相信我，越更新速度越快)
埋坑功能: lnmp安装 / 编译安装，bbr加速，linux内核调优以及唤醒命令，探针部署以及分布式自动化
实际效果截图
![w.png][1]


  [1]: https://blog.nuoyis.net/usr/uploads/2024/07/3611148571.png