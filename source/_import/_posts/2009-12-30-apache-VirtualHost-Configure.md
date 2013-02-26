--- 
categories: []
comments: true
layout: post
title: apache下多虚拟主机的配置与管理
---
当一台服务器想给多个域名提供web服务的时候，我们可以使用apache虚拟主机配置。
apache的配置文件在centos系统下，默认放在/etc/httpd/conf目录下的httpd.conf文件里。
虚拟主机的配置如下
<div class="codeText">
<div class="codeHead"><span class="copyCodeText" onclick="copyIdText('code_8025');" style="cursor: pointer">复制内容到剪贴板</span></div>
<div id="code_8025">
<ol class="dp-cpp">
<li class="alt"><span><span class="preprocessor">#############TEST VirtualHost </span></span></li>
    <li><span><VirtualHost *:80> </span></li>
    <li class="alt"><span>ServerAdmin <a href="mailto:babodx@gmail.com">babodx@gmail.com</a></span></li>
    <li>
<span>DocumentRoot </span><span class="string">"/home/babo"</span><span> </span>
</li>
    <li class="alt"><span>ServerName <a href="http://www.xinlogs.com">www.xinlogs.com</a> </span></li>
    <li><span>DirectoryIndex index.html </span></li>
    <li class="alt"><span>ErrorLog logs/www.xinlogs.com_error_log </span></li>
    <li><span>CustomLog logs/www.xinlogs.com-access_log common </span></li>
    <li class="alt"> </li>
    <li class="alt"><span></VirtualHost> </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
如果我们apache给10个或更多的域名提供web服务，这样的话，我们的httpd.conf就会有很多<VirtualHost *:80>这样的配置段落，看起来很长，管理起来也很麻烦。而且一个apache同时给几十个web域名提供虚拟主机，完全没有问题。那该如何写配置文件便于我们管理呢？
<strong>解决办法</strong>
为了管理方便，配置文件的结构清晰。
我们完全可以将每个虚拟主机的配置文件放在独立文件中，这样apache的主要配置文件httpd.conf看上去很简洁。
只要在最后加入
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_3505');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_3505">
<ol class="dp-xml">
<li class="alt"><span><span>Include conf/vhost_*.conf </span></span></li>
</ol>
</div>
</div>
 
然后再conf目录里，每个虚拟主机的文件只要以vhost_开头，后面可以用自己的域名加.conf后缀。比如有个test.com的域名要做web。
我们就可以直接在conf目录创建一个vhost_test.com.conf文件，文件里写入虚拟主机配置，比如
<div class="codeText">
<div class="codeHead"><span class="copyCodeText" onclick="copyIdText('code_7220');" style="cursor: pointer">复制内容到剪贴板</span></div>
<div id="code_7220">
<ol class="dp-xml">
<li class="alt"><span><span>#############TEST VirtualHost </span></span></li>
    <li>
<span class="tag"><</span><span class="tag-name">VirtualHost</span><span> *:80</span><span class="tag">></span><span> </span>
</li>
    <li class="alt"><span>ServerAdmin <a href="mailto:babodx@gmail.com">babodx@gmail.com</a></span></li>
    <li><span>DocumentRoot "/home/test.com" </span></li>
    <li class="alt"><span>ServerName test.com </span></li>
    <li><span>DirectoryIndex index.html </span></li>
    <li class="alt"><span>ErrorLog logs/test.com_error_log </span></li>
    <li><span>CustomLog logs/test.com-access_log common </span></li>
    <li class="alt"> </li>
    <li>
<span class="tag"></</span><span class="tag-name">VirtualHost</span><span class="tag">></span><span> </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
通过这种配置结构，我们管理再多的虚拟主机也不用怕。只要修改conf目录下对应虚拟主机的配置文件即可。<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/howto-open-rewrite-log">伪静态规则调试技巧</a>
</div>
