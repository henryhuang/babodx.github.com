--- 
categories: []
comments: true
layout: post
title: "CentOS 5.3下OpenVPN安装和Win7下OpenVPN GUI安装"
---
本来我是在美国的VPS服务器上安装的pptp vpn，这个vpn可以用windows自带的拨号连接，配置也很方便。刚配置好的时候很好用，可以开youtube也可以访问一些被封闭的站点。但是后来家里的歌华有线好像调整了一些路由配置，导致我在家里就不能连接vpn了。单位也不能连接。我用老婆家里的adsl尝试连接正常，用联通3G连接也正常。。。。这个既然是网络的问题，估计个人也解决不了了
最近单位也开始搞起来封锁了，开心、verycd等都被封。。。又不能用pptpd vpn了。。。看来该想想其他办法了。代理尝试了，不管用，看来不是基于域名的限制。
于是就开始尝试采用openvpn了。
参考了
<a href="http://www.throx.net/2008/04/13/openvpn-and-centos-5-installation-and-configuration-guide/">http://www.throx.net/2008/04/13/openvpn-and-centos-5-installation-and-configuration-guide/</a>
<a href="http://www.xiaohui.com/dev/server/20070514-install-openvpn.htm">http://www.xiaohui.com/dev/server/20070514-install-openvpn.htm</a>
<strong>2010年10月14日更新</strong>
加入官方CentOS下,iptables规则修改.保证顺利nat上网
<strong>整体方案</strong>
采用位于美国的CentOS 5.3 Linux服务器搭建openvpn服务器，并通过iptables的nat功能使openvpn服务器当做客户端网关。
客户端安装OpenVPN GUI程序连接服务器
<strong>服务器</strong>
服务器采用位于美国的vps
系统CentOS 5.3
安装openvpn作为vpn服务器软件
<strong>OpenVPN服务器安装</strong>
kernel 需要<strong>支持 tun 设备</strong>, 需要加载 iptables 模块. <br>
检查 tun 是否安装:
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_8743');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_8743">
<ol class="dp-xml">
<li class="alt"><span><span>modinfo tun </span></span></li>
    <li> </li>
    <li class="alt"><span>或者 </span></li>
    <li> </li>
    <li class="alt"><span>find / -name tun.o -print </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
安装需要的相关软件
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_5504');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_5504">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>yum install rpm-build </span></span></li>
    <li><span>yum install autoconf.noarch </span></li>
    <li class="alt"><span>yum install zlib-devel </span></li>
    <li><span>yum install pam-devel </span></li>
    <li class="alt"><span>yum install openssl-devel </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
安装环境准备好后，我们下载需要安装的软件。一共需要下载两个软件openvpn 2.0.9和lzo-1.08-4
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_2756');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_2756">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>wget http://openvpn.net/release/openvpn-2.0.9.tar.gz </span></span></li>
    <li><span>wget http://openvpn.net/release/lzo-1.08-4.rf.src.rpm </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
安装lzo
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_2338');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_2338">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>rpmbuild --rebuild lzo-1.08-4.rf.src.rpm </span></span></li>
    <li><span>rpm -Uvh /usr/src/redhat/RPMS/i386/lzo-*.rpm</span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
安装openvpn
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_6883');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_6883">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>rpmbuild -tb openvpn-2.0.9.tar.gz </span></span></li>
    <li><span>rpm -Uvh /usr/src/redhat/RPMS/i386/openvpn-2.0.9-1.i386.rpm </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
安装后，复制配置文件
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_3327');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_3327">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>cp -r /usr/share/doc/openvpn-2.0.9/easy-rsa/ /etc/openvpn/ </span></span></li>
    <li><span>cp /usr/share/doc/openvpn-2.0.9/sample-config-files/server.conf /etc/openvpn/ </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
配置生成CA脚本需要的配置文件vars
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_1803');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_1803">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt">
<span><span>vi /etc/openvpn/easy-rsa/</span></span><span>vars</span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
打开vars后，找到文件最后的如下内容
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_2532');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_2532">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>export </span><span class="attribute">KEY_COUNTRY</span><span>=</span><span class="attribute-value">KG</span><span> </span></span></li>
    <li>
<span>export </span><span class="attribute">KEY_PROVINCE</span><span>=</span><span class="attribute-value">NA</span><span> </span>
</li>
    <li class="alt">
<span>export </span><span class="attribute">KEY_CITY</span><span>=</span><span class="attribute-value">BISHKEK</span><span> </span>
</li>
    <li>
<span>export </span><span class="attribute">KEY_ORG</span><span>=</span><span class="attribute-value">"OpenVPN-TEST"</span><span> </span>
</li>
    <li class="alt">
<span>export </span><span class="attribute">KEY_EMAIL</span><span>=</span><span class="attribute-value">"me@myhost.mydomain"</span><span> </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
根据自己信息修改上面内容,下面是具体含义
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_1636');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_1636">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>export </span><span class="attribute">KEY_COUNTRY</span><span>=</span><span class="attribute-value">KG</span><span> 设置国家 </span></span></li>
    <li>
<span>export </span><span class="attribute">KEY_PROVINCE</span><span>=</span><span class="attribute-value">NA</span><span> 设置省份 </span>
</li>
    <li class="alt">
<span>export </span><span class="attribute">KEY_CITY</span><span>=</span><span class="attribute-value">BISHKEK</span><span> 设置城市 </span>
</li>
    <li>
<span>export </span><span class="attribute">KEY_ORG</span><span>=</span><span class="attribute-value">"OpenVPN-TEST"</span><span> 设置组织 </span>
</li>
    <li class="alt">
<span>export </span><span class="attribute">KEY_EMAIL</span><span>=</span><span class="attribute-value">"me@myhost.mydomain"</span><span> 设置邮件 </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
设置好后，执行如下命令
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_6937');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_6937">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>cd /etc/openvpn/easy-rsa/ </span></span></li>
    <li><span>. ./vars </span></li>
    <li class="alt"><span>./clean-all </span></li>
    <li><span>注意上面的. ./vars两个点之间有一个空格 </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
建立CA证书
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_5043');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_5043">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>./build-ca </span></span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
生成后，ls keys 可以看到ca.crt ca.key文件
建立服务器密钥
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_1625');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_1625">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>./build-key-server xinlogs </span></span></li>
    <li><span>注意这里的xinlogs是我给密钥起的名字，可以根据个人情况修改 </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
生成Diffie-Hellman文件
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_8054');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_8054">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>./build-dh </span></span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
以上文件都正确生成后，拷贝文件到正确目录
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_3008');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_3008">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>cd /etc/openvpn/easy-rsa/ </span></span></li>
    <li><span>cp keys/ca.crt ../ </span></li>
    <li class="alt"><span>cp keys/dh1024.pem ../ </span></li>
    <li><span>cp keys/xinlogs.key ../ </span></li>
    <li class="alt"><span>cp keys/xinlogs.crt ../ </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
生成客户端密钥
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_2881');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_2881">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>./build-key client-1 </span></span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
这里client-1是客户端密钥的文件名，如果需要创建多个客户端密钥，就修改client-1名字多次生成即可。
修改/etc/openvpn/server.conf配置
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_7600');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_7600">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>local 204.74.212.217 </span></span></li>
    <li><span>#修改local后面ip为服务器地址 </span></li>
    <li class="alt"> </li>
    <li><span>dev tap </span></li>
    <li class="alt"><span>;dev tun </span></li>
    <li><span>#默认是dev tun修改为dev tap,tap是可以路由模式 tun 是以太网隧道模式。具体区别我也不太清楚 </span></li>
    <li class="alt"> </li>
    <li><span>ca ca.crt </span></li>
    <li class="alt"><span>cert xinlogs.crt </span></li>
    <li><span>key xinlogs.key </span></li>
    <li class="alt"><span>#cert后面修改为生成的服务器crt文件xinlogs.crt </span></li>
    <li><span>#key后面修改为生成的服务器key文件xinlogs.key </span></li>
    <li class="alt"> </li>
    <li><span>dh dh1024.pem </span></li>
    <li class="alt"><span>#dh后面修改问生成的dh1024.pem </span></li>
    <li> </li>
    <li class="alt"><span>server 10.8.0.0 255.255.255.0 </span></li>
    <li><span>#server后面基本就用默认的10.8.0.0 255.255.255.0即可 </span></li>
    <li class="alt"> </li>
    <li><span>ifconfig-pool-persist ipp.txt </span></li>
    <li class="alt"><span>#这个默认的ipp.txt就可以 </span></li>
    <li> </li>
    <li class="alt"><span>push "route 10.8.0.0 255.255.255.0" </span></li>
    <li><span>#添加客户端路由 </span></li>
    <li class="alt"><span>push "redirect-gateway" </span></li>
    <li><span>#修改客户端默认路由 </span></li>
    <li class="alt"><span>push "dhcp-option DNS 8.8.8.8" </span></li>
    <li><span>#修改客户端默认dns </span></li>
    <li class="alt"><span>client-to-client </span></li>
    <li><span>#允许连接到vpn的客户端可以互相访问 </span></li>
    <li class="alt"><span>duplicate-cn </span></li>
    <li><span>keepalive 10 120 </span></li>
    <li class="alt"><span>comp-lzo </span></li>
    <li><span>#启用lzo压缩 </span></li>
    <li class="alt"><span>user nobody </span></li>
    <li><span>group nobody </span></li>
    <li class="alt"><span>persist-key </span></li>
    <li><span>persist-tun </span></li>
    <li class="alt"><span>status /var/log/openvpn-status.log </span></li>
    <li><span>log /var/log/openvpn.log </span></li>
    <li class="alt"><span>log-append /var/log/openvpn.log </span></li>
    <li><span>verb 3 </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
<strong>启动停止openvpn</strong>
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_1149');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_1149">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>service openvpn start </span></span></li>
    <li><span>#启动openvpn服务 </span></li>
    <li class="alt"> </li>
    <li><span>service openvpn stop </span></li>
    <li class="alt"><span>#停止openvpn服务 </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
<strong>配置iptables</strong>
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_7010');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_7010">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j SNAT --to-source 214.174.212.217 </span></span></li>
    <li><span>#上面命令最后的214.174.212.217是服务器的公网地址，需要根据自己情况修改 </span></li>
    <li class="alt"><span>service iptables save </span></li>
</ol>
</div>
</div>
确认net.ipv4.ip_forward = 1后，服务器就全部配置完成。
如果是用的CentOS官方版本,不是VPS.那因为它自带的iptables规则限制,还需要在/etc/sysconfig/iptables加入下面语句才可以顺利nat上网
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_5461');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_5461">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>-A RH-Firewall-1-INPUT -i tap+ -j ACCEPT  </span></span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
 
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_3930');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_3930">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>cat /etc/sysctl.conf |grep forward </span></span></li>
    <li><span># Controls IP packet forwarding </span></li>
    <li class="alt">
<span class="attribute">net.ipv4.ip_forward</span><span> = </span><span class="attribute-value">1</span><span> </span>
</li>
    <li> </li>
    <li class="alt"><span>#如果不是1，请用vi修改/etc/sysctl.conf文件 </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
<strong>Win7下Openvpn GUI安装</strong>
从 <a target="_blank" href="http://openvpn.se/">http://openvpn.se</a>下载安装文件
Latest stable release: <strong>1.0.3</strong> with OpenVPN 2.0.9 (2006-10-17)
我们直接下载<a href="http://openvpn.se/files/install_packages/openvpn-2.0.9-gui-1.0.3-install.exe">http://openvpn.se/files/install_packages/openvpn-2.0.9-gui-1.0.3-install.exe</a>
<strong>注意：下载后，别着急双击安装。先右键属性，设置兼容模式为windosxp sp3 并用管理员身份运行。</strong>
<img alt="" src="/attachment/1268635851_23046e9f.png">
然后再运行安装。
<strong>客户端配置</strong>
安装后，将服务器/etc/openvpn/easy-rsa/keys/目录下的ca.crt、client-1.crt和client-1.key三个文件拷贝到C:Program FilesOpenVPNconfig目录下
再将C:Program FilesOpenVPNsample-config目录下的client.ovpn文件拷贝到C:Program FilesOpenVPNconfig目录下。
在开始->所有程序里找到openvpn，进入里面右键点击OpenVPN GUI属性，同样修改兼容模式为windows xp sp3 以管理员运行
修改后，运行openvpn gui程序
正确运行后，电脑的右下角会出现openvpn的图标，右键点击选择Edit Config来修改客户端配置文件
下面是我全部客户端配置文件
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_9401');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_9401">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>############################################## </span></span></li>
    <li><span># Sample client-side OpenVPN 2.0 config file # </span></li>
    <li class="alt"><span># for connecting to multi-client server. # </span></li>
    <li><span># # </span></li>
    <li class="alt"><span># This configuration can be used by multiple # </span></li>
    <li><span># clients, however each client should have # </span></li>
    <li class="alt"><span># its own cert and key files. # </span></li>
    <li><span># # </span></li>
    <li class="alt"><span># On Windows, you might want to rename this # </span></li>
    <li><span># file so it has a .ovpn extension # </span></li>
    <li class="alt"><span>############################################## </span></li>
    <li> </li>
    <li class="alt"><span># Specify that we are a client and that we </span></li>
    <li><span># will be pulling certain config file directives </span></li>
    <li class="alt"><span># from the server. </span></li>
    <li><span>client </span></li>
    <li class="alt"> </li>
    <li><span># Use the same setting as you are using on </span></li>
    <li class="alt"><span># the server. </span></li>
    <li><span># On most systems, the VPN will not function </span></li>
    <li class="alt"><span># unless you partially or fully disable </span></li>
    <li><span># the firewall for the TUN/TAP interface. </span></li>
    <li class="alt"><span>dev tap </span></li>
    <li><span>;dev tun </span></li>
    <li class="alt"> </li>
    <li><span># Windows needs the TAP-Win32 adapter name </span></li>
    <li class="alt"><span># from the Network Connections panel </span></li>
    <li><span># if you have more than one. On XP SP2, </span></li>
    <li class="alt"><span># you may need to disable the firewall </span></li>
    <li><span># for the TAP adapter. </span></li>
    <li class="alt"><span>;dev-node MyTap </span></li>
    <li> </li>
    <li class="alt"><span># Are we connecting to a TCP or </span></li>
    <li><span># UDP server? Use the same setting as </span></li>
    <li class="alt"><span># on the server. </span></li>
    <li><span>;proto tcp </span></li>
    <li class="alt"><span>proto udp </span></li>
    <li> </li>
    <li class="alt"><span># The hostname/IP and port of the server. </span></li>
    <li><span># You can have multiple remote entries </span></li>
    <li class="alt"><span># to load balance between the servers. </span></li>
    <li><span>remote 214.174.212.217 1194 </span></li>
    <li class="alt"><span>;remote my-server-2 1194 </span></li>
    <li> </li>
    <li class="alt"><span># Choose a random host from the remote </span></li>
    <li><span># list for load-balancing. Otherwise </span></li>
    <li class="alt"><span># try hosts in the order specified. </span></li>
    <li><span>;remote-random </span></li>
    <li class="alt"> </li>
    <li><span># Keep trying indefinitely to resolve the </span></li>
    <li class="alt"><span># host name of the OpenVPN server. Very useful </span></li>
    <li><span># on machines which are not permanently connected </span></li>
    <li class="alt"><span># to the internet such as laptops. </span></li>
    <li><span>resolv-retry infinite </span></li>
    <li class="alt"> </li>
    <li><span># Most clients don't need to bind to </span></li>
    <li class="alt"><span># a specific local port number. </span></li>
    <li><span>nobind </span></li>
    <li class="alt"> </li>
    <li><span># Downgrade privileges after initialization (non-Windows only) </span></li>
    <li class="alt"><span>;user nobody </span></li>
    <li><span>;group nobody </span></li>
    <li class="alt"> </li>
    <li><span># Try to preserve some state across restarts. </span></li>
    <li class="alt"><span>persist-key </span></li>
    <li><span>persist-tun </span></li>
    <li class="alt"> </li>
    <li><span># If you are connecting through an </span></li>
    <li class="alt"><span># HTTP proxy to reach the actual OpenVPN </span></li>
    <li><span># server, put the proxy server/IP and </span></li>
    <li class="alt"><span># port number here. See the man page </span></li>
    <li><span># if your proxy server requires </span></li>
    <li class="alt"><span># authentication. </span></li>
    <li><span>;http-proxy-retry # retry on connection failures </span></li>
    <li class="alt"><span>;http-proxy [proxy server] [proxy port #] </span></li>
    <li> </li>
    <li class="alt"><span># Wireless networks often produce a lot </span></li>
    <li><span># of duplicate packets. Set this flag </span></li>
    <li class="alt"><span># to silence duplicate packet warnings. </span></li>
    <li><span>;mute-replay-warnings </span></li>
    <li class="alt"> </li>
    <li><span># SSL/TLS parms. </span></li>
    <li class="alt"><span># See the server config file for more </span></li>
    <li><span># description. It's best to use </span></li>
    <li class="alt"><span># a separate .crt/.key file pair </span></li>
    <li><span># for each client. A single ca </span></li>
    <li class="alt"><span># file can be used for all clients. </span></li>
    <li><span>ca ca.crt </span></li>
    <li class="alt"><span>cert client-1.crt </span></li>
    <li><span>key client-1.key </span></li>
    <li class="alt"> </li>
    <li><span># Verify server certificate by checking </span></li>
    <li class="alt"><span># that the certicate has the nsCertType </span></li>
    <li><span># field set to "server". This is an </span></li>
    <li class="alt"><span># important precaution to protect against </span></li>
    <li><span># a potential attack discussed here: </span></li>
    <li class="alt"><span># http://openvpn.net/howto.html#mitm </span></li>
    <li><span># </span></li>
    <li class="alt"><span># To use this feature, you will need to generate </span></li>
    <li><span># your server certificates with the nsCertType </span></li>
    <li class="alt"><span># field set to "server". The build-key-server </span></li>
    <li><span># script in the easy-rsa folder will do this. </span></li>
    <li class="alt"><span>;ns-cert-type server </span></li>
    <li> </li>
    <li class="alt"><span># If a tls-auth key is used on the server </span></li>
    <li><span># then every client must also have the key. </span></li>
    <li class="alt"><span>;tls-auth ta.key 1 </span></li>
    <li> </li>
    <li class="alt"><span># Select a cryptographic cipher. </span></li>
    <li><span># If the cipher option is used on the server </span></li>
    <li class="alt"><span># then you must also specify it here. </span></li>
    <li><span>;cipher x </span></li>
    <li class="alt"> </li>
    <li><span># Enable compression on the VPN link. </span></li>
    <li class="alt"><span># Don't enable this unless it is also </span></li>
    <li><span># enabled in the server config file. </span></li>
    <li class="alt"><span>comp-lzo </span></li>
    <li> </li>
    <li class="alt"><span># Set log file verbosity. </span></li>
    <li><span>verb 3 </span></li>
    <li class="alt"> </li>
    <li><span># Silence repeating messages </span></li>
    <li class="alt"><span>;mute 20 </span></li>
    <li> </li>
    <li class="alt"><span>route-method exe </span></li>
    <li><span>route-delay 2 </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
其实主要修改的就是如下地方
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_9227');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_9227">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>client </span></span></li>
    <li><span>#说明这个是客户端配置文件 </span></li>
    <li class="alt"> </li>
    <li><span>dev tap </span></li>
    <li class="alt"><span>;dev tun </span></li>
    <li><span>#这个和服务器一样就可以 </span></li>
    <li class="alt"> </li>
    <li><span>remote 214.174.212.217 1194 </span></li>
    <li class="alt"><span>#这个ip要修改为服务器的公网ip地址 </span></li>
    <li> </li>
    <li class="alt"><span>resolv-retry infinite </span></li>
    <li><span>nobind </span></li>
    <li class="alt"><span>persist-key </span></li>
    <li><span>persist-tun </span></li>
    <li class="alt"> </li>
    <li> </li>
    <li class="alt"><span>ca ca.crt </span></li>
    <li><span>cert client-1.crt </span></li>
    <li class="alt"><span>key client-1.key </span></li>
    <li><span>#上面三行一定要根据自己生成的密钥配合 </span></li>
    <li class="alt"> </li>
    <li><span>comp-lzo </span></li>
    <li class="alt"><span>#启用lzo压缩 </span></li>
    <li> </li>
    <li class="alt"><span># Set log file verbosity. </span></li>
    <li><span>verb 3 </span></li>
    <li class="alt"> </li>
    <li> </li>
    <li class="alt"><span>route-method exe </span></li>
    <li><span>route-delay 2 </span></li>
    <li class="alt"><span>#最后这两行win7如果不加上，就不能启动修改路由，导致拨vpn成功，但是不能通过远程服务器做网关上网 </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
这些配置完成后，右键点openvpn gui在桌面右下角图标选择Connect连接
我正确连接后的日志如下
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_3077');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_3077">
<ol class="dp-xml" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>Mon Mar 15 13:03:14 2010 OpenVPN 2.0.9 Win32-MinGW [SSL] [LZO] built on Oct 1 2006 </span></span></li>
    <li><span>Mon Mar 15 13:03:14 2010 IMPORTANT: OpenVPN's default port number is now 1194, based on an official port number assignment by IANA. OpenVPN 2.0-beta16 and earlier used 5000 as the default port. </span></li>
    <li class="alt"><span>Mon Mar 15 13:03:14 2010 WARNING: No server certificate verification method has been enabled. See http://openvpn.net/howto.html#mitm for more info. </span></li>
    <li><span>Mon Mar 15 13:03:14 2010 LZO compression initialized </span></li>
    <li class="alt"><span>Mon Mar 15 13:03:14 2010 Control Channel MTU parms [ L:1574 D:138 EF:38 EB:0 ET:0 EL:0 ] </span></li>
    <li><span>Mon Mar 15 13:03:14 2010 Data Channel MTU parms [ L:1574 D:1450 EF:42 EB:135 ET:32 EL:0 AF:3/1 ] </span></li>
    <li class="alt">
<span>Mon Mar 15 13:03:14 2010 Local Options hash (</span><span class="attribute">VER</span><span>=</span><span class="attribute-value">V4</span><span>): 'd79ca330' </span>
</li>
    <li>
<span>Mon Mar 15 13:03:14 2010 Expected Remote Options hash (</span><span class="attribute">VER</span><span>=</span><span class="attribute-value">V4</span><span>): 'f7df56b8' </span>
</li>
    <li class="alt"><span>Mon Mar 15 13:03:14 2010 UDPv4 link local: [undef] </span></li>
    <li><span>Mon Mar 15 13:03:14 2010 UDPv4 link remote: 204.74.212.217:1194 </span></li>
    <li class="alt">
<span>Mon Mar 15 13:03:16 2010 TLS: Initial packet from 204.74.212.217:1194, </span><span class="attribute">sid</span><span>=</span><span class="attribute-value">3d4cf00a</span><span> 84deb309 </span>
</li>
    <li>
<span>Mon Mar 15 13:03:18 2010 VERIFY OK: </span><span class="attribute">depth</span><span>=</span><span class="attribute-value">1</span><span>, /</span><span class="attribute">C</span><span>=</span><span class="attribute-value">US</span><span>/</span><span class="attribute">ST</span><span>=</span><span class="attribute-value">Beijing</span><span>/</span><span class="attribute">L</span><span>=</span><span class="attribute-value">BEIJING</span><span>/</span><span class="attribute">O</span><span>=</span><span class="attribute-value">xinlogs</span><span>.com/</span><span class="attribute">CN</span><span>=</span><span class="attribute-value">babodx</span><span>/</span><span class="attribute">emailAddress</span><span>=</span><span class="attribute-value">babodx</span><span>@gmail.com </span>
</li>
    <li class="alt">
<span>Mon Mar 15 13:03:18 2010 VERIFY OK: </span><span class="attribute">depth</span><span>=</span><span class="attribute-value">0</span><span>, /</span><span class="attribute">C</span><span>=</span><span class="attribute-value">US</span><span>/</span><span class="attribute">ST</span><span>=</span><span class="attribute-value">Beijing</span><span>/</span><span class="attribute">O</span><span>=</span><span class="attribute-value">xinlogs</span><span>.com/</span><span class="attribute">CN</span><span>=</span><span class="attribute-value">babodx</span><span>/</span><span class="attribute">emailAddress</span><span>=</span><span class="attribute-value">babodx</span><span>@gmail.com </span>
</li>
    <li><span>Mon Mar 15 13:03:20 2010 Data Channel Encrypt: Cipher 'BF-CBC' initialized with 128 bit key </span></li>
    <li class="alt"><span>Mon Mar 15 13:03:20 2010 Data Channel Encrypt: Using 160 bit message hash 'SHA1' for HMAC authentication </span></li>
    <li><span>Mon Mar 15 13:03:20 2010 Data Channel Decrypt: Cipher 'BF-CBC' initialized with 128 bit key </span></li>
    <li class="alt"><span>Mon Mar 15 13:03:20 2010 Data Channel Decrypt: Using 160 bit message hash 'SHA1' for HMAC authentication </span></li>
    <li><span>Mon Mar 15 13:03:20 2010 Control Channel: TLSv1, cipher TLSv1/SSLv3 DHE-RSA-AES256-SHA, 1024 bit RSA </span></li>
    <li class="alt"><span>Mon Mar 15 13:03:20 2010 [babodx] Peer Connection Initiated with 214.174.212.217:1194 </span></li>
    <li>
<span>Mon Mar 15 13:03:21 2010 SENT CONTROL [babodx]: 'PUSH_REQUEST' (</span><span class="attribute">status</span><span>=</span><span class="attribute-value">1</span><span>) </span>
</li>
    <li class="alt"><span>Mon Mar 15 13:03:21 2010 PUSH: Received control message: 'PUSH_REPLY,route 10.8.0.0 255.255.255.0,redirect-gateway,dhcp-option DNS 8.8.8.8,route-gateway 10.8.0.1,ping 10,ping-restart 120,ifconfig 10.8.0.3 255.255.255.0' </span></li>
    <li><span>Mon Mar 15 13:03:21 2010 OPTIONS IMPORT: timers and/or timeouts modified </span></li>
    <li class="alt"><span>Mon Mar 15 13:03:21 2010 OPTIONS IMPORT: --ifconfig/up options modified </span></li>
    <li><span>Mon Mar 15 13:03:21 2010 OPTIONS IMPORT: route options modified </span></li>
    <li class="alt"><span>Mon Mar 15 13:03:21 2010 OPTIONS IMPORT: --ip-win32 and/or --dhcp-option options modified </span></li>
    <li><span>Mon Mar 15 13:03:21 2010 TAP-WIN32 device [本地连接 3] opened: .Global{A958F4F2-14AF-49E6-9FBF-4FC25B8D8786}.tap </span></li>
    <li class="alt"><span>Mon Mar 15 13:03:21 2010 TAP-Win32 Driver Version 8.4 </span></li>
    <li>
<span>Mon Mar 15 13:03:21 2010 TAP-Win32 </span><span class="attribute">MTU</span><span>=</span><span class="attribute-value">1500</span><span> </span>
</li>
    <li class="alt"><span>Mon Mar 15 13:03:21 2010 Notified TAP-Win32 driver to set a DHCP IP/netmask of 10.8.0.3/255.255.255.0 on interface {A958F4F2-14AF-49E6-9FBF-4FC25B8D8786} [DHCP-serv: 10.8.0.0, lease-time: 31536000] </span></li>
    <li><span>Mon Mar 15 13:03:21 2010 Successful ARP Flush on interface [29] {A958F4F2-14AF-49E6-9FBF-4FC25B8D8786} </span></li>
    <li class="alt">
<span>Mon Mar 15 13:03:23 2010 TEST ROUTES: 2/2 succeeded </span><span class="attribute">len</span><span>=</span><span class="attribute-value">1</span><span> </span><span class="attribute">ret</span><span>=</span><span class="attribute-value">1</span><span> </span><span class="attribute">a</span><span>=</span><span class="attribute-value">0</span><span> u/</span><span class="attribute">d</span><span>=</span><span class="attribute-value">up</span><span> </span>
</li>
    <li><span>Mon Mar 15 13:03:23 2010 route ADD 214.174.212.217 MASK 255.255.255.255 192.168.2.1 </span></li>
    <li class="alt"><span>操作完成! </span></li>
    <li><span>Mon Mar 15 13:03:23 2010 route DELETE 0.0.0.0 MASK 0.0.0.0 192.168.2.1 </span></li>
    <li class="alt"><span>操作完成! </span></li>
    <li><span>Mon Mar 15 13:03:23 2010 route ADD 0.0.0.0 MASK 0.0.0.0 10.8.0.1 </span></li>
    <li class="alt"><span>操作完成! </span></li>
    <li><span>Mon Mar 15 13:03:23 2010 route ADD 10.8.0.0 MASK 255.255.255.0 10.8.0.1 </span></li>
    <li class="alt"><span>操作完成! </span></li>
    <li><span>Mon Mar 15 13:03:23 2010 Initialization Sequence Completed </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/centos-install-ruby-rubygems">centos 源码安装ruby和rubygems</a><a href="http://xinlogs.com/wow-on-ubuntu-linux">ubuntu8.04 玩魔兽世界 2.4.3(8606)</a><a href="http://xinlogs.com/vps-dropbox-install">一款Linux下同步备份文件的好软件DROPBOX</a><a href="http://xinlogs.com/post/16">一个很好的开源企业内部IM解决方案</a><a href="http://xinlogs.com/linux-ddos-defender">Linux防御DDOS攻击</a>
</div>
