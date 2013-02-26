--- 
categories: []
comments: true
layout: post
title: "centos 单独安装apachebench"
---
这两天在测试php性能优化方法。
为了做压力测试可观察效果，就选择了ApacheBench来作为压力测试工具。其实就是大家常说的ab。
但是这个工具是安装apache web server的时候自带的，现在我服务器上都是跑nginx。也不想为了用这个工具就再装个apache。所以在用下面方法单独安装ab工具，这里记录下步骤。
 
首先安装ab运行需要的软件包apr-util

<div id="kindeditor" class="quote">yum install apr-util</div>

<br>
<span class="Apple-style-span" style="line-height:18px;">然后安装一个yum的工具包，为了可以单独弄出来ab。</span>

<div id="kindeditor" class="quote">yum install yum-utils</div>

<span class="Apple-style-span" style="line-height:18px;"><br></span>
<span class="Apple-style-span" style="line-height:18px;">上面两个安装好以后，我们开始单独安装ab，其实就是下载到apache的rpm包，然后解压后，cp出来ab工具。</span>
<span class="Apple-style-span" style="line-height:18px;">下载apache的rpm包就交给yumdownloader完成了，下载前我们建立一个临时目录用来存放。</span>

<div id="kindeditor" class="quote">
<span class="Apple-style-span" style="line-height:18px;">mkdir httpd</span>
<span class="Apple-style-span" style="line-height:18px;">cd httpd</span>
yumdownloader httpd
</div>

下载后，我们对rpm进行解包

<div id="kindeditor" class="quote">rpm2cpio httpd-2.2.3-43.el5.centos.3.i386.rpm | cpio -idmv</div>

最后将我们用到的ab拷贝到系统的/usr/bin目录下就可以了。最后再删除临时目录就全部ok了。

<div id="kindeditor" class="quote">mv usr/bin/ab /usr/bin/ab<br style="font-family:Arial, Helvetica, Tahoma, Verdana, sans-serif;line-height:18px;text-align:left;background-color:#efefef;">cd ..<br style="font-family:Arial, Helvetica, Tahoma, Verdana, sans-serif;line-height:18px;text-align:left;background-color:#efefef;">rm -rf httpd</div>

<br>
<br>
<br><div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/post/108">CentOS下的Tomcat停止脚本</a><a href="http://xinlogs.com/post/16">一个很好的开源企业内部IM解决方案</a><a href="http://xinlogs.com/centos-install-ruby-rubygems">centos 源码安装ruby和rubygems</a><a href="http://xinlogs.com/php-eaccelerator-vs-fastcgi-cache">php eaccelerator vs fastcgi_cache性能比较</a><a href="http://xinlogs.com/Centos-install-mudos">CENTOS下安装MUDOS跑侠客行</a>
</div>
