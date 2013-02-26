--- 
categories: []
comments: true
layout: post
title: linux网络连接数量监控
---
今天早上一个客户的网站停止了响应，不能正常解析php内容。
重启php-fpm后正常，判断应该是php-fpm进程僵死。所以就先参考网上内容，写了一个监控nginx状态脚本
后来又想看看这个php-fpm进程挂掉和访问量是否有关系，于是打算记录下服务器每分钟的访问量。
其实就是用cron每分钟执行我写的这个脚本，而脚本的作用就是获取当前linux的连接数，并记录到日志文件中。
 
监控linux网络连接数的脚本如下
 




``` 
#!/bin/sh
#定义日志的时间格式，用来作为日志结尾的时间标记
logfile=`date +%Y%m%d`
#定义个时间，用来写入日志用
logtime=`date`
#获取当前系统的网路连接数，这里去掉了127.0.0.1的连接，因为我的php-fpm用的127.0.0.1:9000这个连接。
log=`netstat -ntu | awk '{print $5}' | cut -d: -f1 |grep [^127.0.0.1] |wc -l`
#写日志内容到日志文件
echo "$logtime | $log " >> /data/logs/connect_$logfile.log
```


<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/centos_VPN_Howto">CentOS 5.3架设VPN和619错误排除</a><a href="http://xinlogs.com/centos-install-ruby-rubygems">centos 源码安装ruby和rubygems</a><a href="http://xinlogs.com/install-ApacheBench-on-CentOS-without-Apache">centos 单独安装apachebench</a><a href="http://xinlogs.com/debian-administration-site">一个关于debian系统管理的站点</a><a href="http://xinlogs.com/post/108">CentOS下的Tomcat停止脚本</a>
</div>
