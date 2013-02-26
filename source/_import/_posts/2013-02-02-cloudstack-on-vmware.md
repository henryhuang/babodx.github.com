--- 
categories: []
comments: true
layout: post
title: "cloudstack on vmware"
---
很久没写博客了，总是犯懒。。。。
最近在玩cloudstack和python、django啥的。
上周参加了北京举行的cloudstack技术沙龙，收获很大。这两天在自己笔记本上开始折腾cloudstack玩，写下自己的方法和大家分享下。
我主要是分享下如何通过vmware的嵌套虚拟化，在一台自己的笔记本上跑起来cloudstack。这样手边没有服务器的朋友也可以先看看cloudstack到底什么样子了。
 
因为cloudstack还是很吃资源的，我先说下我的硬件吧。
我的笔记本 CPU i7-3520M  内存4G  装的64位 win7
<b><span style="color:#e53333;">主要是cpu支持虚拟化技术，并且内存越大越好</span></b>
<span style="color:#e53333;"><br></span>
本来我是用的virtualBOX的，因为devcloud2需要virtualbox来跑。但是virtualbox不支持嵌套虚拟化，所以无法胜任cloudstack的运行环境。我换了vmware workstation 9.
 
本来cloudstack文档里的最小配置是2台机器，一台跑管理程序和NFS存储，一台跑虚拟化。但是社区里发布了一个一键安装版本，将这些都整合到一个CentOS 6.3的系统下了，用的kvm的虚拟化技术。
 
下面说说我的步骤：
首先在vmware里创建一个虚拟机，Guest operating system选择Linux下的Red Hat Enterprise Linux 6 64-bit。
虚拟机的内存我给分配了2936M，这个数值是vmware建议的最大分配内存了（宿主4G内存情况下）。
cpu我分配了2个core
注意：Processors设置的时候，已经要勾选下面的Virtualize Intel VT-x/EPT or AMD-V/RVI选项。
<a target="_blank" href="/content/uploadfile/201302/551ddf6c4c81911782d77b19904f993820130202154739.png" id="ematt:83"><img src="/content/uploadfile/201302/thum-551ddf6c4c81911782d77b19904f993820130202154739.png" alt="点击查看原图" border="0"></a>
通过上面截图可以看到Device里面没用的设备也全部删掉，只保留必要的设备。
网卡配置为Host-only模式
<a target="_blank" href="/content/uploadfile/201302/7fda662296e64d81cfcf046f2d4b3c7520130202154946.png" id="ematt:84"><img src="/content/uploadfile/201302/thum-7fda662296e64d81cfcf046f2d4b3c7520130202154946.png" alt="点击查看原图" border="0"></a>
然后我们还要修改下win7下面vmnet1的网卡IP设置，因为一键安装的cloudstack默认使用192.168.16.10这个IP。而这个IP关联到很多配置不能随意修改，所以我们只能修改本机的IP来访问192.168.16这个网段了。
<a target="_blank" href="/content/uploadfile/201302/d1102bc485d7cd75563589177ad2129120130202155209.png" id="ematt:85"><img src="/content/uploadfile/201302/d1102bc485d7cd75563589177ad2129120130202155209.png" alt="点击查看原图" border="0"></a>
这样基本环境就配置好。
然后放入一键安装光盘或者加载ISO来安装cloudstack就可以了。光盘启动后只要选择默认的第一项就可以等待了，全部自动安装，大约20-30分钟就完成了。
系统自动重启后，再次进入系统。我们就可以通过win7下的浏览器访问http://192.168.16.10:8080/client这个管理界面了。默认的用户名密码为admin/password
<a target="_blank" href="/content/uploadfile/201302/082ff4faf6c50e9a1205e8dd4b347af420130202155701.png" id="ematt:86"><img src="/content/uploadfile/201302/thum-082ff4faf6c50e9a1205e8dd4b347af420130202155701.png" alt="点击查看原图" border="0"></a>
进入后会见到如下界面
<a target="_blank" href="/content/uploadfile/201302/d3dd13e89e43747a64442db622b74a1e20130202160055.png" id="ematt:87"><img src="/content/uploadfile/201302/thum-d3dd13e89e43747a64442db622b74a1e20130202160055.png" alt="点击查看原图" border="0"></a>
 
主机警报里会出现一个关于nfs的警报，常规警报里也会出现一些。我这个截图已经跑了一个实例了，所以默认安装的可能和这个截图不太一样。
什么都不需要设置，包括cidr也不需要设置了。
全局设置就保持默认的就可以了
<a target="_blank" href="/content/uploadfile/201302/b123b12819b166c56188cdbdcf191cc920130202160251.png" id="ematt:88"><img src="/content/uploadfile/201302/thum-b123b12819b166c56188cdbdcf191cc920130202160251.png" alt="点击查看原图" border="0"></a>
去基础架构界面看下，两台系统VM启动正常就可以了
<a target="_blank" href="/content/uploadfile/201302/ab4009e8e76a12b3949dc62d8e70535020130202160434.png" id="ematt:89"><img src="/content/uploadfile/201302/thum-ab4009e8e76a12b3949dc62d8e70535020130202160434.png" alt="点击查看原图" border="0"></a>
<span style="color:#e53333;"><b>注意：如果没有启动实例的时候，虚拟路由器是0.</b></span>
<span style="color:#e53333;"><span style="color:#000000;">接下来我们就可以部署一个自己的虚拟机实例了，这个一键安装里自带了一个centos的模版。</span></span>
<span style="color:#e53333;"><span style="color:#000000;">通过实例界面一步一步创建就可以。不要分配太大的内存，要不会因为资源不足导致创建失败，毕竟我们是在vmware嵌套的虚拟环境下弄的。</span></span>
<span style="color:#e53333;"><span style="color:#000000;"><a target="_blank" href="/content/uploadfile/201302/2a7501861773785cf0a6ef27a412472920130202160822.png" id="ematt:90"><img src="/content/uploadfile/201302/thum-2a7501861773785cf0a6ef27a412472920130202160822.png" alt="点击查看原图" border="0"></a><br></span></span>
<span style="color:#e53333;"><span style="color:#000000;">创建后，如果一切正常就会看到新创建的实例已经启动了。如果有错误，可以查看/var/log/cloud/management/management-server.log日志文件。</span></span>
状态会有Creating->Starting->Running
<a target="_blank" href="/content/uploadfile/201302/04b91acda0a968ff2a09a2195f703b2d20130202161715.png" id="ematt:91"><img src="/content/uploadfile/201302/thum-04b91acda0a968ff2a09a2195f703b2d20130202161715.png" alt="点击查看原图" border="0"></a>
<span style="color:#e53333;"><span style="color:#000000;">我也只是刚刚弄好一个最最简单的环境，cloudstack很强大涉及的知识也很广。如果要在实际生产中使用，还要熟悉很多方面的内容。慢慢学习了。。。。</span></span>
大家如果对cloudstack感兴趣，可以加入www.cloudstack-china.org获取更多资料和帮助。
也可以加入cloudstack-china的qq群（<span class="lname" style="font-size:10pt;line-height:1.5;">群   号:</span><span id="qid" class="value text-cursor" style="font-size:10pt;line-height:1.5;">236581725）</span>
<span class="value text-cursor" style="font-size:10pt;line-height:1.5;">最后感谢qq群和邮件列表内回答我问题的朋友，是他们帮我解决了实例无法启动的问题和告诉我如何查看日志等。</span>
 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/post/13">CentOS 5.3 安装虚拟化后的内存识别问题.</a><a href="http://xinlogs.com/post/2">CentOS虚拟化服务的配置</a><a href="http://xinlogs.com/post/11">CentOS里的Xen配置pygrub</a><a href="http://xinlogs.com/post/10">怎么mount一个xen的img映像。转载</a><a href="http://xinlogs.com/post/4">安装和使用xen-shell</a>
</div>
