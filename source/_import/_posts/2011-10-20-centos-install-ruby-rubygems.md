--- 
categories: []
comments: true
layout: post
title: "centos 源码安装ruby和rubygems"
---
<strong>获得ruby源代码</strong>
wget <a href="http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.2-p290.tar.gz">http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.2-p290.tar.gz</a>
<strong>解压并编译ruby<br></strong>tar zxvf ruby-1.9.2-p290.tar.gz  <br>
cd ruby-1.9.2-p290<br>
./configure --prefix=/usr/local/ruby192<br>
make<br>
make install<br>
在/etc/profile最后添加<br>
export PATH=/usr/local/ruby192/bin:$PATH<br>
保存后，执行<br>
source /etc/profile<br>
查看下ruby版本<br>
ruby -v
<br><strong>下载rubygems1.8.10</strong><strong><br></strong>wget <a href="http://production.cf.rubygems.org/rubygems/rubygems-1.8.10.tgz">http://production.cf.rubygems.org/rubygems/rubygems-1.8.10.tgz</a>
<strong>解压并安装rubygems<br></strong>tar zxvf rubygems-1.8.10.tgz <br>
cd rubygems-1.8.10<br>
ruby setup.rb
安装后用gem -v 可以查看下版本<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/tomcat-memory-check">监控tomcat内存占用</a><a href="http://xinlogs.com/post/40">rails实现图片验证码</a><a href="http://xinlogs.com/post/17">如何快速统计服务器访问量</a><a href="http://xinlogs.com/post/20">ubuntu下arp攻击防御和反击！</a><a href="http://xinlogs.com/debian-administration-site">一个关于debian系统管理的站点</a>
</div>
