---
title: 用python实现部分按键自动化
date: 2024-01-29 01:19:22
categories: 
tags: []
---

自从学了python以来，那就得用在最该用的地方->自动化。(爬虫就算了)
目前写了个小的enter键按法，对于gta5已经试验过可以使用。
代码如下:
```
from pynput.keyboard import Key, Controller
import time

keyboard = Controller()

for i in range(999):
    keyboard.press(Key.enter)
    time.sleep(0.1)
    keyboard.release(Key.enter)
    time.sleep(1)
```
代码模拟了人对键盘的按下去和放开。按下去后过0.1秒放开，然后过一秒接着模拟按下，最多按999次，虽然可以写 while true,但是建议写个键来终止程序继续运行
```
from pynput.keyboard import Key, Controller
import time

keyboard = Controller()

while True:
    keyboard.press(Key.enter)
    time.sleep(0.1)
    keyboard.release(Key.enter)
    time.sleep(1)
```

不过，引入pynput得先安装包才能使用,下载慢得更换源。
```
pip install pynput
```