---
title: 论坛搭建 - 虚拟主机搭建flarum
date: 2023-02-06 00:22:00
categories: 
tags: []
---

资源下载链接:https://www.bilibili.com/read/cv29085115/</br>这次我们来用虚拟主机搭建flarum(某github需要mysql5.7还报错)</br>首先确认你的mysql版本在5.6+</br>我把那个官方的composer安装方式弄好了，压缩后直接丢到虚拟主机上即可</br>mysql小于5.6报错页面Something went wrong: MySQL version too low. You need at least MySQL 5.6.</br>密码错误Something went wrong: SQLSTATE[HY000] [1045] Access denied for user 's5329530'@'localhost' (using password: YES)</br>虚拟主机绑定目录时需要加入一个public(如你的网站主目录是bbs,就需要bbs/public),直接可以进入安装页面</br>table prefix是数据包前缀名称,随便填写</br>安装完成后会自动刷新到主页</br>包里已经配置好中文包,直接看视频食用启用中文方法</br>[#BV#][2]</br>搭建完成结束

[1]: https://caiyun.139.com/m/i?135Ce8H2tpZTS
[2]: https://www.bilibili.com/video/BV1Zy4y1D7Mk
