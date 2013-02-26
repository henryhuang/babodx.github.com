--- 
categories: []
comments: true
layout: post
title: 解决Linux下Tomcat不能重启和停止问题
---
我的Tomcat 5.5.28安装在CentOS 5.3 64bit系统上<br>
每次重启都停住，只要要等5-10分钟才可以。<br>
系统reboot或者shutdown也都卡在关闭tomcat的过程中<br>
后来发现直接用/usr/local/tomcat/bin/shutdown.sh一样存在问题，但是如果Tomcat什么项目都不加载，却正常。<br>
我怀疑是tomcat加载的lib或者jar文件一多，重启和关闭就会慢的巨慢。。。甚至10分钟以上。<br>
为了解决这个问题，我修改了/etc/init.d/目录下的tomcat文件，让关闭或者重启的时候，直接通过kill命令杀掉tomcat进程。<br>
下面是我/etc/init.d/tomcat文件<br><br>
 
<div class="codeText">
<div class="codeHead">
<span class="lantxt">C/C++ 代码</span><span class="copyCodeText" onclick="copyIdText('code_7437');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_7437">
<ol class="dp-cpp">
<li class="alt"><span><span class="preprocessor">#!/bin/bash </span></span></li>
    <li><span class="preprocessor"># </span></li>
    <li class="alt"><span class="preprocessor"># Startup script for the tomcat </span></li>
    <li><span class="preprocessor"># </span></li>
    <li class="alt"><span class="preprocessor"># chkconfig: 345 80 15 </span></li>
    <li><span class="preprocessor"># description: Tomcat is a Servlet+JSP Engine. </span></li>
    <li class="alt"> </li>
    <li><span class="preprocessor"># Source function library. </span></li>
    <li class="alt"><span>. /etc/rc.d/init.d/functions </span></li>
    <li> </li>
    <li class="alt"> </li>
    <li><span>export JAVA_HOME=/usr/local/jdk1.6.0_16 </span></li>
    <li class="alt"><span>export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar </span></li>
    <li><span>export PATH=$PATH:$JAVA_HOME/bin </span></li>
    <li class="alt"> </li>
    <li><span>export CATALINA_BASE=/usr/local/tomcat </span></li>
    <li class="alt"><span>export CATALINA_HOME=/usr/local/tomcat </span></li>
    <li><span>export CATALINA_TMPDIR=/usr/local/tomcat/temp </span></li>
    <li class="alt"><span>export JRE_HOME=/usr/local/jdk1.6.0_16 </span></li>
    <li> </li>
    <li class="alt"><span>start(){ </span></li>
    <li>
<span class="keyword">if</span><span> [ -z $(/sbin/pidof java) ]; then </span>
</li>
    <li class="alt">
<span>echo </span><span class="string">"Starting tomcat"</span><span> </span>
</li>
    <li><span>/usr/local/tomcat/bin/startup.sh </span></li>
    <li class="alt"><span>touch /var/lock/subsys/tomcat </span></li>
    <li>
<span class="keyword">else</span><span> </span>
</li>
    <li class="alt">
<span>echo </span><span class="string">"tomcat allready running"</span><span> </span>
</li>
    <li><span>fi </span></li>
    <li class="alt"><span>} </span></li>
    <li> </li>
    <li class="alt"> </li>
    <li><span>stop(){ </span></li>
    <li class="alt">
<span class="keyword">if</span><span> [ ! -z $(/sbin/pidof java) ]; then </span>
</li>
    <li>
<span>echo </span><span class="string">"Shutting down tomcat"</span><span> </span><span>
    <font color="#000000"><span class="preprocessor">#下面就是关闭的核心代码了，用ps获取tomcat进程id，直接kill掉 </span></font>
    </span>
</li>
    <li class="alt">
<span>ps aux|grep tomcat|grep startup|awk </span><span class="string">'{print $2}'</span><span>|xargs kill -9 </span>
</li>
    <li><span class="preprocessor">#/usr/local/tomcat/bin/shutdown.sh </span></li>
    <li class="alt">
<span>until [ -z $(/sbin/pidof java) ]; </span><span class="keyword">do</span><span> :; done </span>
</li>
    <li><span>rm -f /var/lock/subsys/tomcat </span></li>
    <li class="alt">
<span class="keyword">else</span><span> </span>
</li>
    <li>
<span>echo </span><span class="string">"tomcat not running"</span><span> </span>
</li>
    <li class="alt"><span>fi </span></li>
    <li><span>} </span></li>
    <li class="alt"> </li>
    <li> </li>
    <li class="alt">
<span class="keyword">case</span><span> </span><span class="string">"$1"</span><span> in </span>
</li>
    <li><span>start) </span></li>
    <li class="alt"><span>start </span></li>
    <li><span>;; </span></li>
    <li class="alt"><span>stop) </span></li>
    <li><span>stop </span></li>
    <li class="alt"><span>;; </span></li>
    <li><span>restart) </span></li>
    <li class="alt"><span>stop </span></li>
    <li><span>start </span></li>
    <li class="alt"><span>;; </span></li>
    <li><span>status) </span></li>
    <li class="alt"><span>/usr/local/tomcat/bin/catalina.sh version </span></li>
    <li><span>;; </span></li>
    <li class="alt"><span>*) </span></li>
    <li>
<span>echo </span><span class="string">"Usage: $0 {start|stop|restart|status}"</span><span> </span>
</li>
    <li class="alt"><span>esac </span></li>
    <li> </li>
    <li class="alt"><span>exit 0 </span></li>
    <li> </li>
    <li class="alt"> </li>
</ol>
</div>
<link href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css" type="text/css" rel="stylesheet">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/centos-install-ruby-rubygems">centos 源码安装ruby和rubygems</a><a href="http://xinlogs.com/ssh-login-protection-by-denyhosts">通过denyhosts保护ssh登陆</a><a href="http://xinlogs.com/post/17">如何快速统计服务器访问量</a><a href="http://xinlogs.com/post/20">ubuntu下arp攻击防御和反击！</a><a href="http://xinlogs.com/php-eaccelerator-vs-fastcgi-cache">php eaccelerator vs fastcgi_cache性能比较</a>
</div>
