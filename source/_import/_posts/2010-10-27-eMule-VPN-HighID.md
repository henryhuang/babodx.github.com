--- 
categories: []
comments: true
layout: post
title: 通过VPN获取eMule的HighID
---
一般单位里，我们都是通过单位网关上网。这种情况下打开eMule肯定获取到的是LowID（低ID）。那么既然我们可以通过VPN来翻墙，有没有办法通过VPN来获取到eMule的HighID（高ID）呢？
答案是肯定的：可以。
只要我们将VPN服务器的外网IP上的对应端口映射到我们VPN客户端获取到的IP端口上就可以。
我的VPN服务器，就是跑在我这台博客服务器上的openvpn server。笔记本采用openvpn gui客户端连接。
我笔记本分到的ip 10.8.0.2  
vpn server的外网ip：204.74.212.217
vpn server的tap0 ip地址：10.8.0.1
下面是我的iptables配置：
加入如下两条规则
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_1143');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_1143">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>iptables -t nat -A PREROUTING -p tcp -d 204.74.212.217 --dport 13242 -j DNAT --to 10.8.0.2:13242   </span></span></li>
    <li><span>iptables -t nat -A PREROUTING -p udp -d 204.74.212.217 --dport 11324 -j DNAT --to 10.8.0.2:11324   </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
其中13242和11324端口，要和自己安装的emule端口对应。这个可以在emule的选项-》连接界面查看。
以上配置后，还不能获取高ID的话，基本都是windows防火墙原因了。
修改win7 防火墙
在“高级安全Windows防火墙”中的“入站规则”和“出站规则”中，都允许eMule的通过。
然后断开eMule连接，重新连接一次。就可以获取高ID了。<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/squid-cache-php-script">squid缓存php页面</a><a href="http://xinlogs.com/accelerate-app-store-download">快速提升app store下载速度</a><a href="http://xinlogs.com/php-eaccelerator-vs-fastcgi-cache">php eaccelerator vs fastcgi_cache性能比较</a><a href="http://xinlogs.com/check-slow-php-script">如何找到执行慢的php程序</a><a href="http://xinlogs.com/mysql-error-index-is-slower">Mysql错误索引比没索引还慢</a>
</div>
