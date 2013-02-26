--- 
categories: []
comments: true
layout: post
title: 监控tomcat内存占用
---
最近单位给客户部署的一台服务器，主要跑的JAVA应用，一个物业OA系统。经常因为Tomcat内存占用过高而停止响应。
观察了一下，每次停止响应时候，Tomcat进程占用内存超过1.2G
初步怀疑是程序不够完善有内存泄露的地方，但是一时半会也找不到具体代码哪里有问题。
我就写了一个shell不断监控Tomcat进程内存占用，当发现内存占用大于800M，我就自动重启一次Tomcat。
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_2300');" style="cursor:pointer;">复制内容到剪贴板</span> <div id="code_2300">
<ol class="dp-c" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>#!/bin/sh   </span></span></li>
    <li>
<span>MEM=`ps aux|grep tomcat|grep startup|awk </span><span class="string">'{print $6}'</span><span>`   </span>
</li>
    <li class="alt">
<span>#</span><span class="func">echo</span><span> </span><span class="vars">$MEM</span><span>  </span>
</li>
    <li>
<span class="keyword">if</span><span> [ </span><span class="vars">$MEM</span><span> -gt 800000 ]; then   </span>
</li>
    <li class="alt">
<span>   </span><span class="func">echo</span><span> </span><span class="string">"tomcat mem too large,restart tomcat!"</span><span>  </span>
</li>
    <li><span>   service tomcat restart   </span></li>
    <li class="alt"><span>fi   </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
注意：如果service tomcat restart不能正常重启Tomcat，可以参考我前面写的<a href="/admin/post/108">CentOS下的Tomcat停止脚本</a><div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/centos_VPN_Howto">CentOS 5.3架设VPN和619错误排除</a><a href="http://xinlogs.com/post/11">CentOS里的Xen配置pygrub</a><a href="http://xinlogs.com/Tomcat-reboot-problems">解决Linux下Tomcat不能重启和停止问题</a><a href="http://xinlogs.com/Centos-install-mudos">CENTOS下安装MUDOS跑侠客行</a><a href="http://xinlogs.com/monitor-network-connections-of-linux">linux网络连接数量监控</a>
</div>
