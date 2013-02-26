--- 
categories: []
comments: true
layout: post
title: "php eaccelerator vs fastcgi_cache性能比较"
---
<div>【原创】转载请注明出处 http://xinlogs.com</div>
<div>最近放假一直在家里测试php如何优化性能。</div>
<div>今天比较了下默认php、eaccelerator和fastcgi_cache的性能。</div>
<div><br></div>
<div>先介绍下我的测试环境：</div>
<div>这次是在我VPS上做的测试：cpu 为1颗Intel(R) Core(TM) i7-2600 CPU @ 3.40GHz，内存256M</div>
<div>系统为centos 5.6 32bit</div>
<div>php版本 5.3.8</div>
<div>web服务采用Tengine/1.2.1，其实就是nginx</div>
<div><br></div>
<div>测试文件一个采用phpinfo.php，这个就是phpinfo()函数输出页面。另外一个就是采用我博客首页http://xinlogs.com</div>
<div>注：我的博客采用emlog 4.1</div>
<div>下面进入我的测试。。。。</div>
<div><br></div>
<div>php先用默认状态，不加载eaccelerator模块。</div>
<div>测试phpinfo.php,连续访问3000次，采用10个并发请求。</div>
<div></div>
<div id="kindeditor" class="quote">
<div>ab -n 3000 -c 10 http://xinlogs.com/phpinfo.php</div>
<div>Server Software:        Tengine/1.2.1</div>
<div>Server Hostname:        xinlogs.com</div>
<div>Server Port:            80</div>
<div><br></div>
<div>Document Path:          /phpinfo.php</div>
<div>Document Length:        54365 bytes</div>
<div><br></div>
<div>Concurrency Level:      10</div>
<div>Time taken for tests:   7.557223 seconds</div>
<div>Complete requests:      3000</div>
<div>Failed requests:        0</div>
<div>Write errors:           0</div>
<div>Total transferred:      163608000 bytes</div>
<div>HTML transferred:       163095000 bytes</div>
<div>Requests per second:    396.97 [#/sec] (mean)</div>
<div>Time per request:       25.191 [ms] (mean)</div>
<div>Time per request:       2.519 [ms] (mean, across all concurrent requests)</div>
<div>Transfer rate:          21141.76 [Kbytes/sec] received</div>
<div><br></div>
<div>Connection Times (ms)</div>
<div>              min  mean[+/-sd] median   max</div>
<div>Connect:        0    0   0.0      0       2</div>
<div>Processing:     9   24  52.3     25    2432</div>
<div>Waiting:        9   24  52.2     25    2431</div>
<div>Total:          9   24  52.3     25    2432</div>
<div><br></div>
<div>Percentage of the requests served within a certain time (ms)</div>
<div>  50%     25</div>
<div>  66%     28</div>
<div>  75%     29</div>
<div>  80%     29</div>
<div>  90%     31</div>
<div>  95%     32</div>
<div>  98%     34</div>
<div>  99%     36</div>
<div> 100%   2432 (longest request)</div>
</div>
<div></div>
<div><br></div>
<div>可以看到每秒大约处理396.97 [#/sec] (mean)。99%响应时间在36ms以内</div>
<div>我又测试了几次，基本每秒处理在390到450浮动，响应时间在45ms以内吧。</div>
<div><br></div>
<div>再来看看我博客首页的测试情况</div>
<div></div>
<div id="kindeditor" class="quote">
<div>ab -n 3000 -c 10 http://xinlogs.com/</div>
<div>Server Software:        Tengine/1.2.1</div>
<div>Server Hostname:        xinlogs.com</div>
<div>Server Port:            80</div>
<div><br></div>
<div>Document Path:          /</div>
<div>Document Length:        21494 bytes</div>
<div><br></div>
<div>Concurrency Level:      10</div>
<div>Time taken for tests:   52.787368 seconds</div>
<div>Complete requests:      3000</div>
<div>Failed requests:        2970</div>
<div>   (Connect: 0, Length: 2970, Exceptions: 0)</div>
<div>Write errors:           0</div>
<div>Total transferred:      64990449 bytes</div>
<div>HTML transferred:       64432449 bytes</div>
<div>Requests per second:    56.83 [#/sec] (mean)</div>
<div>Time per request:       175.958 [ms] (mean)</div>
<div>Time per request:       17.596 [ms] (mean, across all concurrent requests)</div>
<div>Transfer rate:          1202.31 [Kbytes/sec] received</div>
<div><br></div>
<div>Connection Times (ms)</div>
<div>              min  mean[+/-sd] median   max</div>
<div>Connect:        0    0   0.0      0       0</div>
<div>Processing:     9  175 661.8    127   18091</div>
<div>Waiting:        9  174 661.9    126   18091</div>
<div>Total:          9  175 661.8    127   18091</div>
<div><br></div>
<div>Percentage of the requests served within a certain time (ms)</div>
<div>  50%    127</div>
<div>  66%    149</div>
<div>  75%    166</div>
<div>  80%    178</div>
<div>  90%    199</div>
<div>  95%    210</div>
<div>  98%    226</div>
<div>  99%    348</div>
<div> 100%  18091 (longest request)</div>
</div>
<div></div>
<div>可以看到每秒处理56.83 [#/sec] (mean)请求，99%的响应时间在348ms内</div>
<div>多次测试后，基本保持在每秒处理55-60个请求，响应时间基本在350ms内。</div>
<div><br></div>
<div>接着我开启php的eaccelerator模块,开启后的phpinfo信息如下：</div>
<div></div>
<div id="kindeditor" class="quote">
<div>This program makes use of the Zend Scripting Language Engine:</div>
<div>Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies</div>
<div>    with eAccelerator v0.9.6.1, Copyright (c) 2004-2010 eAccelerator, by eAccelerator</div>
</div>
<div></div>
<div><br></div>
<div>我们再次测试phpinfo.php文件</div>
<div></div>
<div id="kindeditor" class="quote">
<div>ab -n 3000 -c 10 http://xinlogs.com/phpinfo.php</div>
<div><br></div>
<div>Server Software:        Tengine/1.2.1</div>
<div>Server Hostname:        xinlogs.com</div>
<div>Server Port:            80</div>
<div><br></div>
<div>Document Path:          /phpinfo.php</div>
<div>Document Length:        56845 bytes</div>
<div><br></div>
<div>Concurrency Level:      10</div>
<div>Time taken for tests:   7.128081 seconds</div>
<div>Complete requests:      3000</div>
<div>Failed requests:        0</div>
<div>Write errors:           0</div>
<div>Total transferred:      171333080 bytes</div>
<div>HTML transferred:       170819225 bytes</div>
<div>Requests per second:    420.87 [#/sec] (mean)</div>
<div>Time per request:       23.760 [ms] (mean)</div>
<div>Time per request:       2.376 [ms] (mean, across all concurrent requests)</div>
<div>Transfer rate:          23472.94 [Kbytes/sec] received</div>
<div><br></div>
<div>Connection Times (ms)</div>
<div>              min  mean[+/-sd] median   max</div>
<div>Connect:        0    9   7.9      9      40</div>
<div>Processing:     2   13  13.6     12     620</div>
<div>Waiting:        0    9  13.2      8     603</div>
<div>Total:         12   23  13.6     23     623</div>
<div><br></div>
<div>Percentage of the requests served within a certain time (ms)</div>
<div>  50%     23</div>
<div>  66%     29</div>
<div>  75%     31</div>
<div>  80%     31</div>
<div>  90%     32</div>
<div>  95%     34</div>
<div>  98%     37</div>
<div>  99%     39</div>
<div> 100%    623 (longest request)</div>
</div>
<div></div>
<div>从结果来看，加了eAccelerator后，phpinfo.php的测试结果并不是很快。每秒处理420.87请求，偶尔还会在380左右。并没有明显变化。</div>
<div><br></div>
<div>再来看看博客首页的测试。</div>
<div></div>
<div id="kindeditor" class="quote">
<div>ab -n 3000 -c 10 http://xinlogs.com/</div>
<div>Server Software:        Tengine/1.2.1</div>
<div>Server Hostname:        xinlogs.com</div>
<div>Server Port:            80</div>
<div><br></div>
<div>Document Path:          /</div>
<div>Document Length:        21488 bytes</div>
<div><br></div>
<div>Concurrency Level:      10</div>
<div>Time taken for tests:   23.177888 seconds</div>
<div>Complete requests:      3000</div>
<div>Failed requests:        2970</div>
<div>   (Connect: 0, Length: 2970, Exceptions: 0)</div>
<div>Write errors:           0</div>
<div>Total transferred:      64929588 bytes</div>
<div>HTML transferred:       64371588 bytes</div>
<div>Requests per second:    129.43 [#/sec] (mean)</div>
<div>Time per request:       77.260 [ms] (mean)</div>
<div>Time per request:       7.726 [ms] (mean, across all concurrent requests)</div>
<div>Transfer rate:          2735.67 [Kbytes/sec] received</div>
<div><br></div>
<div>Connection Times (ms)</div>
<div>              min  mean[+/-sd] median   max</div>
<div>Connect:        0    0   1.0      0      23</div>
<div>Processing:     4   76 582.2     31   13655</div>
<div>Waiting:        4   76 582.2     31   13655</div>
<div>Total:          4   76 582.2     31   13655</div>
<div><br></div>
<div>Percentage of the requests served within a certain time (ms)</div>
<div>  50%     31</div>
<div>  66%     46</div>
<div>  75%     55</div>
<div>  80%     58</div>
<div>  90%     69</div>
<div>  95%     80</div>
<div>  98%    100</div>
<div>  99%    129</div>
<div> 100%  13655 (longest request)</div>
</div>
<div></div>
<div>从博客首页的测试可以发现，用了eAccelerator的效果还是很明显的。每秒处理请求129.43 [#/sec] (mean)，99%的响应时间在129ms内完成。</div>
<div>这个结果比不用eaccelerator的情况下，高了1倍多。</div>
<div><br></div>
<div>个人认为eAccelerator因为减少了php每次编译时间，所以当php程序需要调用很多类，或者调用其他php文件的时候。这个优化就体现出来了。就想我上面的测试，博客首页的加载确实快了，每秒处理的请求也多了。但是想phpinfo.php这样只有一个文件，并且只是简单的输出phpinfo信息的页面，eaccelerator起到的优化就很少了。</div>
<div><br></div>
<div>下面我们再来测试下fastcgi_cache有多快，这个就是缓存，完全不是优化php执行速度了，而是直接缓存上结果，下次直接调用缓存。</div>
<div><br></div>
<div>我们先用curl请求2次phpinfo.php 确定已经缓存上，再来测试。</div>
<div></div>
<div id="kindeditor" class="quote">
<div>-bash-3.2# curl -I http://xinlogs.com/phpinfo.php</div>
<div>HTTP/1.1 200 OK</div>
<div>Server: Tengine/1.2.1</div>
<div>Date: Thu, 19 Jan 2012 07:09:23 GMT</div>
<div>Content-Type: text/html</div>
<div>Connection: keep-alive</div>
<div>Vary: Accept-Encoding</div>
<div>X-Powered-By: PHP/5.3.8</div>
<div>X-Cache: HIT</div>
<div><br></div>
<div>ab -n 3000 -c 10 http://xinlogs.com/phpinfo.php</div>
<div>Server Software:        Tengine/1.2.1</div>
<div>Server Hostname:        xinlogs.com</div>
<div>Server Port:            80</div>
<div><br></div>
<div>Document Path:          /phpinfo.php</div>
<div>Document Length:        58255 bytes</div>
<div><br></div>
<div>Concurrency Level:      10</div>
<div>Time taken for tests:   0.656969 seconds</div>
<div>Complete requests:      3000</div>
<div>Failed requests:        0</div>
<div>Write errors:           0</div>
<div>Total transferred:      175320000 bytes</div>
<div>HTML transferred:       174765000 bytes</div>
<div>Requests per second:    4566.43 [#/sec] (mean)</div>
<div>Time per request:       2.190 [ms] (mean)</div>
<div>Time per request:       0.219 [ms] (mean, across all concurrent requests)</div>
<div>Transfer rate:          260605.90 [Kbytes/sec] received</div>
<div><br></div>
<div>Connection Times (ms)</div>
<div>              min  mean[+/-sd] median   max</div>
<div>Connect:        0    0   0.0      0       0</div>
<div>Processing:     0    1   1.1      1       4</div>
<div>Waiting:        0    1   0.9      1       4</div>
<div>Total:          0    1   1.1      1       4</div>
<div><br></div>
<div>Percentage of the requests served within a certain time (ms)</div>
<div>  50%      1</div>
<div>  66%      2</div>
<div>  75%      2</div>
<div>  80%      3</div>
<div>  90%      3</div>
<div>  95%      3</div>
<div>  98%      4</div>
<div>  99%      4</div>
<div> 100%      4 (longest request)</div>
</div>
<div></div>
<div><br></div>
<div><br></div>
<div>看到什么是速度了吧。。。每秒处理请求 4566.43 [#/sec] (mean)，响应时间4ms以内完成。</div>
<div>这个就是fastcgi_cache的速度了.</div>
<div><br></div>
<div>也许一个phpinfo.php页面不能说明问题，我们再来看看博客首页的情况</div>
<div><br></div>
<div></div>
<div id="kindeditor" class="quote">
<div>ab -n 3000 -c 10 http://xinlogs.com/</div>
<div><br></div>
<div>Server Software:        Tengine/1.2.1</div>
<div>Server Hostname:        xinlogs.com</div>
<div>Server Port:            80</div>
<div><br></div>
<div>Document Path:          /</div>
<div>Document Length:        21485 bytes</div>
<div><br></div>
<div>Concurrency Level:      10</div>
<div>Time taken for tests:   0.578070 seconds</div>
<div>Complete requests:      3000</div>
<div>Failed requests:        0</div>
<div>Write errors:           0</div>
<div>Total transferred:      65055000 bytes</div>
<div>HTML transferred:       64455000 bytes</div>
<div>Requests per second:    5189.68 [#/sec] (mean)</div>
<div>Time per request:       1.927 [ms] (mean)</div>
<div>Time per request:       0.193 [ms] (mean, across all concurrent requests)</div>
<div>Transfer rate:          109900.19 [Kbytes/sec] received</div>
<div><br></div>
<div>Connection Times (ms)</div>
<div>              min  mean[+/-sd] median   max</div>
<div>Connect:        0    0   0.0      0       0</div>
<div>Processing:     0    1   0.8      2       3</div>
<div>Waiting:        0    1   0.8      2       3</div>
<div>Total:          0    1   0.8      2       3</div>
<div>WARNING: The median and mean for the processing time are not within a normal deviation</div>
<div>        These results are probably not that reliable.</div>
<div>WARNING: The median and mean for the waiting time are not within a normal deviation</div>
<div>        These results are probably not that reliable.</div>
<div>WARNING: The median and mean for the total time are not within a normal deviation</div>
<div>        These results are probably not that reliable.</div>
<div><br></div>
<div>Percentage of the requests served within a certain time (ms)</div>
<div>  50%      2</div>
<div>  66%      2</div>
<div>  75%      2</div>
<div>  80%      2</div>
<div>  90%      2</div>
<div>  95%      2</div>
<div>  98%      2</div>
<div>  99%      3</div>
<div> 100%      3 (longest request)</div>
</div>
<div></div>
<div>再看看首页，因为采用了fastcgi_cache，每秒处理请求5189.68 [#/sec] (mean)，响应时间3ms内完成。</div>
<div><br></div>
<div>性能比较表格(主要提供每秒处理数做比较)</div>
<div><br></div>
<div>                    请求文件长度    php默认     eAccelerator加速模式      fastcgi_cache模式</div>
<div>phpinfo.php     58255 bytes     396.97          420.87                          4566.43 </div>
<div>博客首页        21485 bytes      56.83           129.43                           5189.68 </div>
<div><br></div>
<div><b><span style="font-size:18px;">总结：</span></b></div>
<div>上面的测试虽然不是很严谨，但是通过这些已经可以发现fastcgi_cache的优势了，通过缓存php结果，大幅提升请求处理能力。</div>
<div>一般程序再通过eaccelerator模式加速后，可以提升2-3倍性能，而fastcgi_cache缓存的性能提升大约在10倍。</div>
<div><br></div>
<div>不过fastcgi_cache的应用也有它的弊端，就是要通过程序控制好刷新和不缓存内容。不然登陆后的用户都显示一个界面，或者发布的消息迟迟不能显示在首页都是它要解决的问题。</div>
<div><br></div>
<div>一般用户还是建议采用eaccelerator模块加速，因为这样基本无需修改代码，最容易实现。fastcgi_cache很难在不调整代码的情况下使用，因为他会缓存全部php文件，至少我的博客在启用了fastcgi_cache后，后台不正常。</div>
<div><br></div>
<div><br></div>
<div><br></div>
<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/check-slow-php-script">如何找到执行慢的php程序</a><a href="http://xinlogs.com/linux-ddos-defender">Linux防御DDOS攻击</a><a href="http://xinlogs.com/accelerate-app-store-download">快速提升app store下载速度</a><a href="http://xinlogs.com/wow-on-ubuntu-linux">ubuntu8.04 玩魔兽世界 2.4.3(8606)</a><a href="http://xinlogs.com/find-php-slow-code">php-fpm查找php慢速代码</a>
</div>
