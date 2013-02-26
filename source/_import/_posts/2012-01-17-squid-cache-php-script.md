--- 
categories: []
comments: true
layout: post
title: squid缓存php页面
---
这两天在看squid反向代理加速web应用方面的内容。
总体来看squid的cache都是缓存一些静态内容，例如图片、css、js等文件。而动态内容是不能缓存的。那有没有办法让squid缓存php文件呢？答案是肯定的。
首先用squid反向代理后，我们访问一个test.html的静态页面。


<div id="kindeditor" class="annotate">[root@localhost ~]# curl -I http://xinlogs.com:3128/test.html
HTTP/1.0 200 OK
Content-Type: text/html
Content-Length: 77
Accept-Ranges: bytes
Server: Tengine/1.2.1
Date: Tue, 17 Jan 2012 12:38:36 GMT
Last-Modified: Tue, 17 Jan 2012 01:29:47 GMT
X-Cache: HIT from squid.xinlogs.com
Via: 1.1 squid.xinlogs.com:3128 (squid/2.7.STABLE9)
Connection: close
</div>

可以看到<span class="Apple-style-span" style="font-size:12px;line-height:18px;color:#000000;">X-Cache: HIT from squid.xinlogs.com，成功命中缓存。说明是被squid缓存的内容直接给我们了。</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;color:#000000;">而访问一个动态页面呢？</span>


<div id="kindeditor" class="annotate">
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">[root@localhost ~]# curl -I http://xinlogs.com:3128/test.php</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">HTTP/1.0 200 OK</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Server: Tengine/1.2.1</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Date: Tue, 17 Jan 2012 12:40:52 GMT</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Content-Type: text/html</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Vary: Accept-Encoding</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">X-Powered-By: PHP/5.3.8</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">X-Cache: MISS from squid.xinlogs.com</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Via: 1.1 squid.xinlogs.com:3128 (squid/2.7.STABLE9)</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Connection: close</span>
</div>

<span class="Apple-style-span" style="font-size:12px;line-height:18px;">通过</span><span class="Apple-style-span" style="font-size:12px;line-height:18px;color:#000000;">X-Cache: MISS from squid.xinlogs.com，可以看到是未命中，也就是从squid后端的nginx请求到的内容。</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">查了写资料，发现之所以不缓存动态脚本，主要是php文件没有输出</span><span class="Apple-style-span" style="font-size:12px;line-height:18px;color:#000000;">Last-Modified: Tue, 17 Jan 2012 01:29:47 GMT这行头信息。</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">下面我们构造一个可以输出头信息的php文件test2.php来测试下。</span>



``` 
<?php
$now = time();
$generatedAt = gmdate("D, d M Y H:i:s T",$now);
//print_r($generatedAt);
$lastModified = gmdate("D, d M Y 00:00:00 T",$now);
$expiresAt = gmdate("D, D M Y H:i:s T",strtotime($lastModified)+86400);
$maxAge=strtotime($expiresAt)-strtotime($generatedAt);
header('Last-modified: '.$lastModified);
header('Cache-control: max-age=' . $maxAge);
echo "Test".time();
```

<span class="Apple-style-span" style="font-size:12px;line-height:18px;">这里我们通过header函数来构造了Last-Modified和Cache-control头信息。</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">再让我们通过curl -I来查看下是否可以缓存这个test2.php文件</span>


<div id="kindeditor" class="annotate">
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">[root@localhost ~]# curl -I http://xinlogs.com:3128/test2.php</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">HTTP/1.0 200 OK</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Server: Tengine/1.2.1</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Date: Tue, 17 Jan 2012 12:46:08 GMT</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Content-Type: text/html</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Vary: Accept-Encoding</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">X-Powered-By: PHP/5.3.8</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Last-Modified: Tue, 17 Jan 2012 00:00:00 GMT</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Cache-Control: max-age=-1169168</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Age: 60</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Content-Length: 14</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">X-Cache: HIT from squid.xinlogs.com</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Via: 1.1 squid.xinlogs.com:3128 (squid/2.7.STABLE9)</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">Connection: close</span>
</div>



<span class="Apple-style-span" style="font-size:12px;line-height:18px;">通过上面信息，可以看到test2.php已经被缓存了。</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">总结：通过在php脚本里输出header头信息，还是可以让squid缓存上php文件的。</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><b>但是目前还有一些问题没有解决：</b></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><b>通过IE浏览器访问可以正常命中，通过chrome或者firefox浏览器每次访问都是如下未命中</b></span>
1326794201.750      1 210.56.223.167 TCP_REFRESH_MISS/200 355 GET <a href="http://xinlogs.com:3128/test2.php" target="_blank" style="word-wrap:break-word;color:#336699;font-family:Tahoma, 'Microsoft Yahei', Simsun;font-size:14px;line-height:21px;">http://xinlogs.com:3128/test2.php</a> - FIRST_UP_PARENT/xinlogs text/html
<span class="Apple-style-span" style="font-size:14px;line-height:21px;"><br></span>
<span class="Apple-style-span" style="font-size:14px;line-height:21px;">而且不同访问方式的第一次访问也都是未命中，比如wget -S第一次访问未命中，如果换了curl -I再访问，还是未命中。</span>
<span class="Apple-style-span" style="font-size:14px;line-height:21px;">尝试在squid.conf文件里加入了reload-into-ims on选项，但是无效。</span>
<span class="Apple-style-span" style="font-size:14px;line-height:21px;">以上问题估计还需要看看是不是需要配合nginx的设置来解决，因为静态文件是不存在这些问题的。</span>

 
<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/php-eaccelerator-vs-fastcgi-cache">php eaccelerator vs fastcgi_cache性能比较</a><a href="http://xinlogs.com/check-slow-php-script">如何找到执行慢的php程序</a><a href="http://xinlogs.com/eMule-VPN-HighID">通过VPN获取eMule的HighID</a><a href="http://xinlogs.com/update-115disk-urls">批量更换115网盘链接</a><a href="http://xinlogs.com/find-php-slow-code">php-fpm查找php慢速代码</a>
</div>
