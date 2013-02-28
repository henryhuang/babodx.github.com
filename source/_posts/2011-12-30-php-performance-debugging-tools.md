--- 
categories: ['PHP']
comments: true
layout: post
title: php性能调试工具
---
最近在帮一些客户优化php性能。

排除掉服务器环境、memcache这些办法后，剩下的就是找php代码有没有执行过慢的因素了。

下面列出一些在php性能调试中可以用到的工具。

1、xdebug
     这个工具大家都很熟悉，开始php过程中，用来调试程序用的。

2、webgrind
     这个工具主要是配合xdebug来使用，用来监测php执行时间的。

3、XHProf
     这个工具据说比xdebug轻量，可以考虑用于生产环境下来监测php性能。没有用过，看介绍很棒！facebook用的，肯定差不了。
 
以上只是简单介绍下，用来记录。等以后有机会仔细用来，再发文档说明
