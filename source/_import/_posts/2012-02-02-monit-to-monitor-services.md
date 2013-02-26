--- 
categories: []
comments: true
layout: post
title: monit监控linux服务
---
最近负责的两台服务器需要监控nginx和memcached服务，防止网站访问异常。
我用monit来解决这个需求。
monit是一款linux下的开源软件，可以负责监控系统的服务、进程、文件等内容，并设置一定的条件下执行特定的action。
我们可以通过配置，让monit来检查网站的状态和memcached的状态，发现异常的时候，自动重启服务并邮件通知我们。
我配置了30s检查一次，出现问题发送邮件到我的gmail邮箱。我手机马上就能收到邮件提醒，非常效率。
 
安装配置步骤：
下载monit

<div id="kindeditor" class="quote">wget http://mmonit.com/monit/dist/monit-5.3.2.tar.gz</div>

 
解压并安装

<div id="kindeditor" class="quote">
<span style="color:#000000;">tar zxvf </span><span class="Apple-style-span" style="font-size:12px;line-height:18px;color:#000000;">monit-5.3.2.tar.gz</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;color:#000000;">cd </span><span class="Apple-style-span" style="font-size:12px;line-height:18px;color:#000000;">monit-5.3.2</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">./configure --prefix=/usr/local/monit</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">make</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">make install</span>
</div>

<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">配置</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">先将默认的配置文件拷贝到/etc目录下，然后编辑monitrc文件</span>

<div id="kindeditor" class="quote">cp monitrc /etc/</div>

<span class="Apple-style-span" style="font-size:12px;line-height:18px;">下面是我的一些配置：</span>

<div id="kindeditor" class="quote">
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">set daemon  30   #设置30s检查一次</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">set logfile /var/log/monit.log   #设定日志文件</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">#设定邮件</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">set mailserver smtp.gmail.com PORT 587 USERNAME "yourname@gmail.com" PASSWORD "yourpassword" USING tlsv1 </span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">#设定邮件格式</span>

<span class="Apple-style-span" style="font-size:12px;line-height:18px;">set mail-format {</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">      from: monit@huaximall.com</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">      subject: monit alert --  $EVENT $SERVICE</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">      message: $EVENT Service $SERVICE</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">                 Date:        $DATE</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">                 Action:      $ACTION</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">                 Host:        xinlogs.com</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">                 Description: $DESCRIPTION</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">            Your faithful employee,</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">            Monit</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">}</span>

<span class="Apple-style-span" style="font-size:12px;line-height:18px;">#设定提醒超时</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">set alert babodx@gmail.com with reminder on 3 cycles</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">#设定检查nginx服务</span>

<span class="Apple-style-span" style="font-size:12px;line-height:18px;">check process nginx with pidfile /usr/local/webserver/nginx-1.0.11/nginx.pid</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">  start program = "/usr/local/webserver/nginx-1.0.11/sbin/nginx"</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">  stop program = "/usr/bin/killall nginx"</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">  if failed host xinlogs.com port 80 protocol http</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">     and request "/t.html"</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">     then restart</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">  if 2 restarts within 3 cycles then timeout</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">#设定检查memcached服务</span>

<span class="Apple-style-span" style="font-size:12px;line-height:18px;">check process memcached with pidfile /var/run/memcache.pid</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">      start program = "/usr/local/memcached/bin/memcached -d -m 1024 -u root -p 11211 -c 1024 -P /var/run/memcache.pid"</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">      stop program = "/bin/kill -9 `cat /var/run/memcache.pid`; rm /var/run/memcached.pid"</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">      if failed host 127.0.0.1 port 11211 protocol MEMCACHE then restart</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">      if cpu > 60% for 2 cycles then alert</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">      if cpu > 98% for 5 cycles then restart</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">      if 2 restarts within 3 cycles then timeout</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">      group cache</span>



</div>


<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">下面再贴出来提醒的邮件样子给大家看看</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">这个是检查到nginx被终止后的提醒邮件</span>

<div id="kindeditor" class="quote">Does not exist Service nginx<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">                Date:        Thu, 02 Feb 2012 11:31:50<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">                Action:      restart<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">                Host:        xinlogs.com<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">                Description: process is not running<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);"><br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">           Your faithful employee,<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">           Monit</div>

<span class="Apple-style-span" style="font-size:12px;line-height:18px;">下面是自动恢复nginx服务后的提醒邮件</span>

<div id="kindeditor" class="quote">Exists Service nginx<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">                Date:        Thu, 02 Feb 2012 11:32:28<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">                Action:      alert<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">                Host:        xinlogs.com<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">                Description: process is running with pid 6894<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);"><br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">           Your faithful employee,<br style="color:#222222;font-family:arial, sans-serif;font-size:14px;line-height:normal;background-color:rgba(255, 255, 255, 0.917969);">           Monit</div>


<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">有了这些就可以安心了，万一网站访问异常了。monit会尝试自动恢复的，而且第一时间通知我们。因为我的vps在美国，所以发送到gmail很及时。手机再设置个邮件提醒功能，基本就ok了。</span>

<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span><div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/linux-ddos-defender">Linux防御DDOS攻击</a><a href="http://xinlogs.com/post/20">ubuntu下arp攻击防御和反击！</a><a href="http://xinlogs.com/vps-dropbox-install">一款Linux下同步备份文件的好软件DROPBOX</a><a href="http://xinlogs.com/OpenVPN-Install">CentOS 5.3下OpenVPN安装和Win7下OpenVPN GUI安装</a><a href="http://xinlogs.com/Centos-install-mudos">CENTOS下安装MUDOS跑侠客行</a>
</div>
