---
title: 以 RHCL9 做路由器分配教程
date: 2024-05-11 03:36:00
categories: default
tags: []
---
# 前言
# 装配前置
1.静态IP
2.YUM源
请看文章文章[RHCL 9 合集][1]
根据上一篇文章来配置双网卡
注意:
ens160采取桥接模式,要求绑定在物理网卡上,不然无法启动网卡
ens224采取主机模式,要求仅主机模式
# 环境安装
需要安装dns,dhcp,iptables
dns,dhcp配置请看文章[RHCL 9 合集][2]
iptables安装以及开启启动(如果没有配置好外网卡可以采取本地源)
```
yum -y install iptables-services
systemctl start iptables
systemctl enable iptables
```
开启内核转发
```
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
sysctl -p
```
路由转发
```
route add -net 192.168.50.0/24 dev ens160
```
如果提前将dhcp和dns配置好后,dhcp改配置如下:
```
subnet 192.168.50.0 netmask 255.255.255.0 {
  range 192.168.50.4 192.168.50.100;
  option domain-name "nuoyis's server route";
  option routers 192.168.50.1;
  option broadcast-address 192.168.50.255;
  option domain-name-servers 192.168.50.1;
  default-lease-time 600;
  max-lease-time 7200;
}
```
然后将主机均解析到你的路由服务器上,就可以尝试上网了.
# 效果展示
![2024-05-10T09:41:34.png][3]
# 借鉴文章
https://www.cnblogs.com/qinlulu/p/13204854.html
https://blog.csdn.net/pamdora/article/details/81117268
https://blog.csdn.net/enmo2015/article/details/132000991
https://blog.csdn.net/weixin_53946852/article/details/125626246


  [1]: https://blog.nuoyis.net/posts/f02.html
  [2]: https://blog.nuoyis.net/posts/f02.html
  [3]: https://blog.nuoyis.net/usr/uploads/2024/05/2948852445.png