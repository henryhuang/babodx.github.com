--- 
categories: []
comments: true
layout: post
title: "Centos Linux 远程终端ssh乱码问题"
---
我们经常碰到linux乱码问题。尤其是碰到网页上传个中文文件名的文件，ssh登陆到linux一看全乱码想删除都不行。很郁闷的。如下图所示中文文件名全都是显示问号了（这个乱码由于你的编码设置不同，显示的也不太一样）
<a target="_blank" href="/content/uploadfile/201111/2322075ba26b81fb6c3bbe9784583ec320111110021108.jpg" id="ematt:60"><img src="/content/uploadfile/201111/thum-2322075ba26b81fb6c3bbe9784583ec320111110021108.jpg" width="420" height="20" title="点击查看原图" alt="点击查看原图" border="0"></a>
还有一个就是vim的乱码
<a target="_blank" href="/content/uploadfile/201111/7599c2dd870bfaa7632dc174843a863720111110011704.jpg" id="ematt:61"><img src="/content/uploadfile/201111/7599c2dd870bfaa7632dc174843a863720111110011704.jpg" width="107" height="24" title="点击查看原图" alt="点击查看原图" border="0"></a>
 
解决办法：
首先需要给linux安装中文支持。这里以centos为例，所以采用yum安装

<div id="kindeditor" class="quote">yum groupinstall chinese-support</div>
然后再设置linux系统的i18n文件，位置在/etc/sysconfig/i18n
内容如下

<div id="kindeditor" class="quote">LANG="zh_CN.GB18030"
SUPPORTED="zh_CN.UTF-8:zh_CN.GBK:zh_CN.GB18030:zh_CN:zh:en_US.UTF-8:en_US:en"
SYSFONT="latarcyrheb-sun16"
</div>

接着设置LC_ALL环境变量，在/etc/profile文件里加入
export LC_ALL=zh_CN.GB18030
全部设置好后，重启系统。再次登陆后，用如下命令查看


<div id="kindeditor" class="quote">-bash-3.2# locale
LANG=zh_CN.GB18030
LC_CTYPE="zh_CN.GB18030"
LC_NUMERIC="zh_CN.GB18030"
LC_TIME="zh_CN.GB18030"
LC_COLLATE="zh_CN.GB18030"
LC_MONETARY="zh_CN.GB18030"
LC_MESSAGES="zh_CN.GB18030"
LC_PAPER="zh_CN.GB18030"
LC_NAME="zh_CN.GB18030"
LC_ADDRESS="zh_CN.GB18030"
LC_TELEPHONE="zh_CN.GB18030"
LC_MEASUREMENT="zh_CN.GB18030"
LC_IDENTIFICATION="zh_CN.GB18030"
LC_ALL=zh_CN.GB18030
</div>

<div><br></div>
<div>完成以上操作，应该就可以正常显示中文文件名字了。不过这个只是linux没有问题了，我们的ssh客户端还需要支持才可以。我一般使用Putty来当做ssh客户端，下面就以putty为例子进行设置。</div>
<div>先设置使用的字体，选择windows->Appearance，弹出的字体界面里选择“新宋体”，字符集选择“CHINESE_GB2312”</div>
<div>
<a target="_blank" href="/content/uploadfile/201111/be82d1b2c5e6aa91698156b17933a82c20111110030323.jpg" id="ematt:62"><img src="/content/uploadfile/201111/thum-be82d1b2c5e6aa91698156b17933a82c20111110030323.jpg" alt="点击查看原图" border="0"></a><br>
</div>
<div><br></div>
<div>再设置编码</div>
<a target="_blank" href="/content/uploadfile/201111/d0689cae77ad99c2e8592e5d82eca0e520111110030450.jpg" id="ematt:63"><img src="/content/uploadfile/201111/thum-d0689cae77ad99c2e8592e5d82eca0e520111110030450.jpg" alt="点击查看原图" border="0"></a>
以上设置完成后，就可以正常显示了。效果如下图所示：
<a target="_blank" href="/content/uploadfile/201111/34cc05c5e482ce91cba812fd614308b120111110030630.jpg" id="ematt:64"><img src="/content/uploadfile/201111/thum-34cc05c5e482ce91cba812fd614308b120111110030630.jpg" alt="点击查看原图" border="0"></a>
<div><br></div>
<div>在命令行输入中文文件名也可以正常使用了。</div>
<a target="_blank" href="/content/uploadfile/201111/f64f6c65c77b39de6c0f02edec0a883420111110030705.jpg" id="ematt:65"><img src="/content/uploadfile/201111/f64f6c65c77b39de6c0f02edec0a883420111110030705.jpg" alt="点击查看原图" border="0"></a>
到目前为止，ssh下操作中文文件名的问题就彻底解决了。可以正常输入中文文件名来操作了。
 
下面是vim显示乱码问题，vim一般都是因为文件编码和显示编码的问题引起的乱码。
我们打开文件发现乱码后，采用如下命令

<div id="kindeditor" class="quote">set encoding=utf-8 termencoding=gbk</div>

 

这样以后，vim也可以正常操作中文了。效果图
<a target="_blank" href="/content/uploadfile/201111/5e0ba09c3d78f51517654564ad80598520111110031409.jpg" id="ematt:66"><img src="/content/uploadfile/201111/thum-5e0ba09c3d78f51517654564ad80598520111110031409.jpg" alt="点击查看原图" border="0"></a>
 

<div id="kindeditor" class="note">
<b>网上看到一些资料说是将i18n的LANG设置为zh_CN.UTF-8，然后LC_ALL也是设置为zh_CN.UTF-8</b>
<b>但是我试验了下，效果并不好（也将putty调整为了utf-8）.但是文件名显示还是乱码。需要用ls --show-control-chars来正常显示，输入中文文件名也不正常。</b>
<b>以上只是在putty下使用，也许配合其他ssh客户端效果不太一样。稍后我再添加一些其他客户端效果。</b>
</div>


<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/post/108">CentOS下的Tomcat停止脚本</a><a href="http://xinlogs.com/wow-on-ubuntu-linux">ubuntu8.04 玩魔兽世界 2.4.3(8606)</a><a href="http://xinlogs.com/post/113">mysqldump解决中文乱码问题</a><a href="http://xinlogs.com/monit-to-monitor-services">monit监控linux服务</a><a href="http://xinlogs.com/post/17">如何快速统计服务器访问量</a>
</div>
