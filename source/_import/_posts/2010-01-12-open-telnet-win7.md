--- 
categories: []
comments: true
layout: post
title: win7下启动telnet--转载
---
因为我日常需要用到telnet来管理交换机，所以经常用到telnet。
升级到win7后，发现telnet不能用了，提示如下
<img alt="" src="http://xinlogs.com/attachment/1263273316_9469f971.png">
后来查找资料发现
Windows 7出于安全性考虑屏蔽了Telnet。
如何开启呢？
在开始菜单-》控制面板-》程序界面中，点“打开或关闭Windows功能”如下图
<img alt="" src="http://xinlogs.com/attachment/1263273814_4338d20b.png">
然后在弹出的"Windows功能"界面中，选择上Telnet客户端，点击确定按钮。如下图
<img alt="" src="http://xinlogs.com/attachment/1263273936_67310301.png">
通过上面这些设置，就打开了Windows默认屏蔽的Telnet功能了。
 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/quickmacro-on-win7">windows 7平台下按键精灵的后台运行</a>
</div>
