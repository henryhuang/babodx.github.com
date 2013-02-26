--- 
categories: []
comments: true
layout: post
title: "drupal在nginx 0.8.15下的简洁URL配置"
---
最近在学习drupal，发现这个程序比xoops、joomla都灵活，对应的学习曲线也高。反正安装后，根本不知道真没设置。呵呵
下面先在nginx上配置了rewrite规则，让drupal支持简洁的URL

<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_6531');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_6531">
<ol class="dp-c">
<li class="alt"><span><span>server </span></span></li>
    <li><span>{ </span></li>
    <li class="alt"><span>listen 80; </span></li>
    <li><span>server_name drupal.xinlogs.com; </span></li>
    <li class="alt"><span>index index.html index.htm index.php; </span></li>
    <li><span>root /data0/htdocs/drupal; </span></li>
    <li class="alt"> </li>
    <li><span><span class="preprocessor">#limit_conn crawler 20; </span></span></li>
    <li class="alt"><span>location = / { </span></li>
    <li><span>index index.php; </span></li>
    <li class="alt"><span>} </span></li>
    <li> </li>
    <li class="alt"><span>location / { </span></li>
    <li><span>index index.php index.html; </span></li>
    <li class="alt"> </li>
    <li>
<span class="keyword">if</span><span> (!-f $request_filename) { </span>
</li>
    <li class="alt"><span>rewrite ^(.*)$ /index.php?q=$1 last; </span></li>
    <li>
<span class="keyword">break</span><span>; </span>
</li>
    <li class="alt"><span>} </span></li>
    <li> </li>
    <li class="alt">
<span class="keyword">if</span><span> (!-d $request_filename) { </span>
</li>
    <li><span>rewrite ^(.*)$ /index.php?q=$1 last; </span></li>
    <li class="alt">
<span class="keyword">break</span><span>; </span>
</li>
    <li><span>} </span></li>
    <li class="alt"><span>} </span></li>
    <li> </li>
    <li class="alt"><span>location ~ .*.(php|php5)?$ </span></li>
    <li><span>{ </span></li>
    <li class="alt"><span>fastcgi_pass unix:/tmp/php-cgi.sock; </span></li>
    <li><span><span class="preprocessor">#fastcgi_pass 127.0.0.1:9000; </span></span></li>
    <li class="alt"><span>fastcgi_index index.php; </span></li>
    <li><span>include fcgi.conf; </span></li>
    <li class="alt"><span>} </span></li>
    <li> </li>
    <li class="alt"><span>location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$ </span></li>
    <li><span>{ </span></li>
    <li class="alt"><span>expires 30d; </span></li>
    <li><span>} </span></li>
    <li class="alt"> </li>
    <li><span>location ~ .*.(js|css)?$ </span></li>
    <li class="alt"><span>{ </span></li>
    <li><span>expires 1h; </span></li>
    <li class="alt"><span>} </span></li>
    <li> </li>
    <li class="alt">
<span>log_format drupal_access </span><span class="string">'$remote_addr - $remote_user [$time_local] "$request" '</span><span> </span>
</li>
    <li>
<span class="string">'$status $body_bytes_sent "$http_referer" '</span><span> </span>
</li>
    <li class="alt">
<span class="string">'"$http_user_agent" $http_x_forwarded_for'</span><span>; </span>
</li>
    <li><span>access_log /data1/logs/drupal_access.log drupal_access; </span></li>
    <li class="alt"><span>} </span></li>
</ol>
</div>
<link href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css" type="text/css" rel="stylesheet">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/nginx-epool-events">nginx采用epoll的事件模型，为何效率高。</a><a href="http://xinlogs.com/nginx-301-rewrite">nginx通过301统一入口域名</a><a href="http://xinlogs.com/Solve-nginx-502-mistakes">nginx通过轮询避免php-fpm引起的502错误</a><a href="http://xinlogs.com/howto-open-rewrite-log">伪静态规则调试技巧</a>
</div>
