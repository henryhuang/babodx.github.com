--- 
categories: ['Windows']
comments: true
layout: post
title: winxp的远程桌面设置
---
最近换了mac的笔记本，由于一些程序需要跑在winxp上面，比如zmud等。。。

于是就在服务器上安装了xen，开了个winxp的虚拟机。本来xen支持vnc远程访问的，但是vnc感觉真心不给里，比windows自带的远程桌面慢了很多。

于是开始考虑使用远程桌面，可是winxp不是服务器版本，所以远程桌面有一些限制。

一个是解决多用户登陆问题

下面是网上找到的解决办法，测试好用

Windows XP不支持多个用户同时登录远程桌面，当其他用户远程登录Windows XP时，主机上当前已登录的用户

即会自动退出。不过在Windows XP SP2中提供了允许连接会话并发功能，可通过远程桌面进行多用户的同时登录，

但其在默认状态下关闭了该项特性，需要通过修改注册表开启该功能。 

打开注册表编辑器，依次展开
```
“HKEY_LOCAL_MACHINE\System \CurrentControlSet\Control\Terminal Server\Licensing Core”
```
分支，转到右侧窗口，在其中新建一个类型为DWORD的子键，将该键命名为“EnableConcurrentSessions”，
并将键值设置为“1”，即可开启多用户登录功能。 

还有就是空密码登陆问题，因为winxp就是平时随便用用，每次输入密码太麻烦。但是远程桌面默认空密码不让登陆

**解决办法如下**

在运行里，输入`gpedit.msc`调出组策略界面

依次找到“计算机配置”－》“windows设置”－》“安全设置”－》“本地策略”－》“安全选项”
将其中的“帐户：使用空白密码的本地帐户只运行使用控制台登陆”禁用掉。

通过上面的设置就ok了
