--- 
categories: ['Hardware','Mac']
comments: true
layout: post
title: "2012 macbook Air 使用华为E1750 3g上网卡"
---
最近一直在摸索macbook air的使用方法，目前大部分工作都已经可以放弃windows了。

今天又将华为的E1750上网卡在macbook air上正确安装了。

我的3g上网卡如下

![e1750图](http://flic.kr/p/dYtJa9)

这个上网卡是09年的，现在直接插入usb后，macbook air没有任何反应。

**安装方法：**

1、首先要在Windows机器上更新固件

先在windows机器安装mobile_card_driver驱动。

然后再到华为主页下载固件更新程序

E1750 WINMACB300D00SP05C112(UTPS11.301.08.30.112_MAC11.301.06.13.112)(China Unicom)(05019744)

将上网卡插入windows的usb接口，可以被驱动正确识别以后，运行固件更新程序开始更新固件。

2、macbook下安装

更新好固件以后，我们就可以插入macbook的笔记本。这个时候会识别出来一个mobile card的光驱。

我们从finder进入这个光驱，就会发现“无线上网卡.app“程序

直接安装这个程序就可以了

3、安装好后的程序界面

![运行界面](http://flic.kr/p/dYtJPN)

