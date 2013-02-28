--- 
categories: ['Mac']
comments: true
layout: post
title: "快速提升app store下载速度"
---
前两天开始更新MAC OS Mountain Lion。

为了下载这个4.5G的内容，可是郁闷坏了。计算出来的剩余时间7天。

于是开始找加速下载的办法，看来一些帖子以后基本知道了下载速度主要取决与dns将你的请求解析到哪台服务器。

网上一般是都是基于自己找到一台快速的下载服务器IP，然后手动修改/etc/hosts文件的办法。
 
但是其实看原来就知道，如果我们设置的DNS得当，就能起到从最快服务器下载的目的。

于是我查到了[http://dns.v2ex.com/](http://dns.v2ex.com/)这个网站，但是使用这个dns后，速度依然很慢。

后来又在www.weiphone.com找到了一个台湾的DNS。设置后速度大幅提升。
 
如果你也在北京、也用歌华有线。

试试下面这个台湾的DNS吧
 
```
168.95.1.1
```
