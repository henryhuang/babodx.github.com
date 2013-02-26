--- 
categories: []
comments: true
layout: post
title: 伪静态规则调试技巧
---
最近在威客网上接了个任务，帮助调整伪静态规则。WEB服务是apache的，主要就是修改rewrite规则。
伪静态规则写起来比较麻烦，关键是正则表达是写完总要测试，而我们不知道这个rewrite该怎么测试。我们需要准确知道服务器是怎么匹配我们的规则的，匹配后又发生了什么。这也就是我今天要写的调试技巧。
目前我们采用的WEB Server主要是apache和nginx（Windows下的IIS我不用，只有基于Linux的）。
其实所谓技巧就是通过打开rewrite的日志功能，然后每次访问后，我们查看rewrite日志来分析我们的规则是否正确。
 
<b>APACHE开启rewrite办法</b>
在httpd.conf文件里加入


<div id="kindeditor" class="quote">RewriteLog "/xampp/apache/logs/RewriteLog.txt" 
RewriteLogLevel 9
</div>

<div>我这里没用Linux下的apache，直接在我windows下的xampp环境里测试的</div>
<div><br></div>
<div>访问后的rewrite日志文件如下</div>
<div>
<div></div>
</div>
<div id="kindeditor" class="quote">
<div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (3) [perdir C:/xampp/htdocs/] strip per-dir prefix: C:/xampp/htdocs/q-123.html -> q-123.html</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (3) [perdir C:/xampp/htdocs/] applying pattern '^.*$' to uri 'q-123.html'</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (4) [perdir C:/xampp/htdocs/] RewriteCond: input='C:/xampp/htdocs/q-123.html' pattern='!-f' => matched</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (4) [perdir C:/xampp/htdocs/] RewriteCond: input='C:/xampp/htdocs/q-123.html' pattern='!-d' => matched</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (2) [perdir C:/xampp/htdocs/] rewrite 'q-123.html' -> 'index.php?q-123.html'</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (3) split uri=index.php?q-123.html -> uri=index.php, args=q-123.html</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (3) [perdir C:/xampp/htdocs/] add per-dir prefix: index.php -> C:/xampp/htdocs/index.php</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (2) [perdir C:/xampp/htdocs/] trying to replace prefix C:/xampp/htdocs/ with /</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (5) strip matching prefix: C:/xampp/htdocs/index.php -> index.php</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (4) add subst prefix: index.php -> /index.php</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6ba00d8/initial] (1) [perdir C:/xampp/htdocs/] internal redirect with /index.php [INTERNAL REDIRECT]</div>
<div>127.0.0.1 - - [01/Dec/2011:15:38:38 +0800] [localhost/sid#775148][rid#6bdc840/initial/redir#1] (3) [perdir C:/xampp/htdocs/] strip per-dir prefix: C:/xampp/htdocs/index.php -> index.php</div>
</div>
<div></div>
</div>
<div>还是很详细的，用来分析我们的rewrite规则是否生效和是否有错误足够了！</div>
<div><br></div>
<b>Nginx开启rewrite日志办法</b>
在nginx.conf的配置文件里，加入如下选项内容

<div id="kindeditor" class="quote">rewrite_log on;</div>

<br>
<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/druapl-url-nginx-rewrite">drupal在nginx 0.8.15下的简洁URL配置</a><a href="http://xinlogs.com/Solve-nginx-502-mistakes">nginx通过轮询避免php-fpm引起的502错误</a><a href="http://xinlogs.com/apache-VirtualHost-Configure">apache下多虚拟主机的配置与管理</a><a href="http://xinlogs.com/nginx-301-rewrite">nginx通过301统一入口域名</a><a href="http://xinlogs.com/nginx-epool-events">nginx采用epoll的事件模型，为何效率高。</a>
</div>
