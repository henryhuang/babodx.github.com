--- 
categories: []
comments: true
layout: post
title: nginx通过轮询避免php-fpm引起的502错误
---
<span style="font-size:24px;"><b>【问题背景】</b></span>
最近客户的一个网站总是偶尔出现502错误
网站架构采用的就是nginx 1.0.10+php 5.3.8+php-fpm模式
<span style="font-size:24px;">【问题分析】</span>
<span style="color:#e53333;">首先要检查下php-fpm的进程数使用情况</span>

<div id="kindeditor" class="quote">netstat -napo |grep "php-fpm" | wc -l</div>
如果这个查询出来的数量接近了你在php-fpm.conf里设置的数量，说明是进程数量不过用了。果断增加
而我这次碰到的不是这个。继续分析
<span style="color:#e53333;">会不会是php程序执行时间过长造成超时？</span>
这个可以通过我的另外一篇文章来查看
<span style="font-size:18px;"><a href="/find-php-slow-code" target="_blank">php-fpm查找php慢速代码</a></span>
如果是这个问题，我们可以通过修改nginx.conf和php-fpm.conf里面相关的超时设置来解决

<div id="kindeditor" class="note">nginx.conf里面主要是如下<br>fastcgi_connect_timeout 300;<br style="color:#333333;font-family:arial;font-size:14px;line-height:25px;background-color:#ffffff;padding:0px;margin:0px;word-break:break-all;">fastcgi_send_timeout 300;<br style="color:#333333;font-family:arial;font-size:14px;line-height:25px;background-color:#ffffff;padding:0px;margin:0px;word-break:break-all;">fastcgi_read_timeout 300;<br><br>php-fpm.conf里如要是如下
request_terminate_timeout = 10s
</div>
很不幸，我碰到的也不是这个问题
<span style="color:#e53333;">接着我开始分析是不是fastcgi缓存不够？</span>
主要是在nginx.conf配置里修改如下参数


<div id="kindeditor" class="note">
<br>
fastcgi_buffer_size 64k;<br>
fastcgi_buffers 4 64k;<br>
fastcgi_busy_buffers_size 128k</div>
这个我还真不清楚要设置多大合适，网站说这个配置小了，有可能引发502.
<span style="font-size:24px;"><b>【终极解决办法】</b></span>
既然上面的常规办法不能解决，那我就想其他办法了。可以肯定的是502引起肯定是因为php-fpm引发的，也就是nginx将正确的客户端请求发给了后端的php-fpm进程，但是因为php-fpm进程的问题导致不能正确解析php代码。最终返回给了客户端502错误。
我觉得nginx既然upstream可以支持多组后端服务器轮询实现简单的负责均衡，并且可以做简单的健康检查。那我就用这个办法开多组php-fpm服务来实现一个php-fpm池，让nginx在这个php-fpm资源里通过stream轮询。
因为有了健康检查机制，这样就可以在错误到达客户端前换另外一个php-fpm进程重新解析了。
 
<span style="font-size:18px;">主要配置方法</span>

先修改php-fpm.conf来开启多组php-fpm进程
我这里开启了php-cgi_www1和php-cgi_www2两组



``` 
[www1]
listen = /tmp/php-cgi_www1.sock
user = www
group = www
pm = dynamic
pm.max_children = 128
pm.start_servers = 32
pm.min_spare_servers = 32
pm.max_spare_servers = 96
pm.max_requests= 10240
request_terminate_timeout = 10s

[www2]
listen = /tmp/php-cgi_www2.sock
user = www
group = www
pm = dynamic
pm.max_children = 128
pm.start_servers = 32
pm.min_spare_servers = 32
pm.max_spare_servers = 96
pm.max_requests= 10240
request_terminate_timeout = 10s
```

然后重启php-fpm进程，并查看/tmp目录是否已经出现了php-cgi_www1.sock和php-cgi_www2.sock的文件
如果存在这两个文件，说明php-fpm配置ok了。我们继续修改nginx.conf配置
主要在http {} 的配置块内，加入我们要使用的轮询配置



``` 
upstream php_servers{
  server unix:/tmp/php-cgi_www1.sock;
  server unix:/tmp/php-cgi_www2.sock;
}
fastcgi_next_upstream error timeout invalid_header http_500 http_503;
```

接着修改我们原来处理php代码的fastcgi配置



``` 
 location ~ .*\.(php|php5)?$
    {
      fastcgi_pass  unix:/tmp/php-cgi_www.sock;
      fastcgi_index index.php;
      include fastcgi.conf;
    }
```

将其中的fastcgi_pass由原来的php-cgi_www.sock单独的php-fpm进程修改为我们创建的php_servers轮询池



``` 
 location ~ .*\.(php|php5)?$
    {
      fastcgi_pass php_servers;
      fastcgi_index index.php;
      include fastcgi.conf;
    }
```

通过这样的配置，可以肯定的是php-fpm因为采用两组来轮询工作，并且有fastcgi_next_upstream进程简单的健康检查，可以最大限度的避免502错误发生了。
 
 
 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/nginx-epool-events">nginx采用epoll的事件模型，为何效率高。</a><a href="http://xinlogs.com/druapl-url-nginx-rewrite">drupal在nginx 0.8.15下的简洁URL配置</a><a href="http://xinlogs.com/nginx-301-rewrite">nginx通过301统一入口域名</a><a href="http://xinlogs.com/howto-open-rewrite-log">伪静态规则调试技巧</a>
</div>
