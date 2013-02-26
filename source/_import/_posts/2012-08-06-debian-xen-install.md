--- 
categories: []
comments: true
layout: post
title: debian6.0.5(squeeze)安装配置xen-4.0虚拟化
---
<b><span style="font-size:24px;">系统安装</span></b>
<span style="font-size:18px;">我是通过http://mirrors.163.com下载的</span>     debian-6.0.5-amd64-CD-1.iso
<div>最小安装有一张CD就可以了，安装的是<span style="font-size:18px;">debian6.0.5(squeeze) 64位版本</span>
</div>
<div><br></div>
<div><span style="font-size:18px;">安装没啥好说的，最小化安装，除了基本系统就多选择了一个ssh server。</span></div>
<div><br></div>
<div>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><span style="font-size:18px;"> </span><div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">安装好系统后，进入系统首先设置网卡，不要用dhcp选择static模式</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">编辑/etc/network/interfaces文件如下</span></div>
</div>
<div>

``` 
# The loopback network interfaceauto lo

iface lo inet loopback



# The primary network interface

#allow-hotplug eth0

auto eth0

iface eth0 inet static

  address 192.168.1.10

  netmask 255.255.255.0

  broadcast 192.168.1.255

  network 192.168.1.0

  gateway 192.168.1.1
```

<br>
</div>
<div>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">我这里只有一个eth0网卡，采用192.168.1.10的IP地址</span></div>
</div>
<div><br></div>
<div>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><span style="font-size:18px;"> </span><div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">接下来修改apt的源，采用mirrors.163.com提供的服务器，这样速度比较快</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">修改/etc/apt/sources.list</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">内容如下</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;">
<div id="kindeditor" class="quote">
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><div style="font-size:medium;">
<span style="background-color:#f7f7f7;font-size:13px;line-height:19px;">deb </span><a href="http://mirrors.163.com/debian/" style="font-size:13px;line-height:19px;">http://mirrors.163.com/debian/</a><span style="background-color:#f7f7f7;font-size:13px;line-height:19px;"> squeeze main non-free contrib</span><br>
</div>
<div style="font-size:medium;">
<span style="background-color:#f7f7f7;font-size:13px;line-height:19px;">deb </span><a href="http://mirrors.163.com/debian/" style="font-size:13px;line-height:19px;">http://mirrors.163.com/debian/</a><span style="background-color:#f7f7f7;font-size:13px;line-height:19px;"> squeeze-proposed-updates main non-free contrib</span><span style="background-color:#f7f7f7;font-size:13px;line-height:19px;"><br></span>
</div>
<div style="font-size:medium;">
<span style="background-color:#f7f7f7;font-size:13px;line-height:19px;">deb-src </span><a href="http://mirrors.163.com/debian/" style="font-size:13px;line-height:19px;">http://mirrors.163.com/debian/</a><span style="background-color:#f7f7f7;font-size:13px;line-height:19px;"> squeeze main non-free contrib </span><span style="background-color:#f7f7f7;font-size:13px;line-height:19px;"><br></span>
</div>
<div style="font-size:medium;">
<span style="font-size:13px;background-color:#f7f7f7;line-height:19px;">deb-src </span><a href="http://mirrors.163.com/debian/" style="font-size:13px;line-height:19px;">http://mirrors.163.com/debian/</a><span style="font-size:13px;background-color:#f7f7f7;line-height:19px;"> squeeze-proposed-updates main non-free contrib</span>
</div>
<div><span style="font-size:13px;background-color:#f7f7f7;line-height:19px;"><br></span></div>
</div>
 <span style="line-height:19px;font-size:18px;">执行apt-get update更新源</span>
</div>
<span style="line-height:19px;font-size:18px;">经过这些设置，系统基本就是一个干净的最小系统。并且采用163的服务器作为软件源。</span>
<span style="line-height:19px;"><br></span>
<span style="line-height:19px;font-size:24px;"><b>安装XEN-4.0</b></span>
<span style="font-size:medium;"><span style="line-height:27px;font-size:18px;">下面开始安装xen-4.0的相关软件包和配置</span></span>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?-->apt-get install xen-hypervisor-4.0-amd64 xen-linux-system-2.6.32-5-xen-amd64 xen-utils* xenwatch xen-tools
<span style="font-size:18px;line-height:21px;">需要下66.2M的软件包</span>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><span style="font-size:18px;"> </span><span style="font-size:18px;line-height:21px;">安装后，再次启动选择带XEN 4.0 amd64的内核启动项</span>
<span style="font-size:18px;line-height:21px;color:#e53333;">注意：（默认的启动项虽然带xen但是不带4.0选项，启动后不能正确启动xend）</span>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;line-height:21px;color:#000000;">也可以修改/etc/default/grub里面的选项，默认从这个内核启动</span></div>
<span style="font-size:18px;line-height:21px;color:#000000;">修改GRBU_DEFAULT=4</span>
<span style="font-size:14px;line-height:21px;color:#000000;"><br></span>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;line-height:21px;">配置xen的网络</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;line-height:21px;">修改/etc/xen/xend-config.sxp文件</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:14px;line-height:21px;"><br></span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;line-height:21px;">打开</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;">
<span style="font-size:18px;">(network-script network-bridge)</span><span style="font-size:14px;line-height:21px;"><br></span><span style="font-size:18px;"> </span>
</div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">采用桥接方式 </span></div>


<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">重启后，系统会将物理网卡修改为peth0，而eth0为桥接的网卡</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><br></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">brctl show 显示如下</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">root@node1:~# brctl show</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;text-align:-webkit-auto;"><span style="font-size:18px;">bridge name     bridge id                    STP enabled     interfaces</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;text-align:-webkit-auto;"><span style="font-size:18px;">eth0                 8000.001c42d8fe43     no                    peth0</span></div>
<div><br></div>

<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><b><span style="font-size:24px;">通过xen-create-image安装debian虚拟系统</span></b>
xen-create-image --hostname=vm01 --size=2G --swap=128M --ide \ 
--ip=192.168.1.21 --netmask=255.255.255.0 --gateway=192.168.1.1 \
--force --dir=/vm --memory=128M -arch=i386 \
--kernel=/boot/vmlinuz-2.6.32-5-xen-amd64 \
--dist=squeeze --mirror=http://mirrors.163.com/debian/ --passwd \
--install-method=debootstrap
<br>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="line-height:19px;font-size:18px;">安装以后，需要调整vm01.cfg文件，否则无法启动</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="line-height:19px;font-size:18px;">首先调整磁盘的相关配置</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">root        = '/dev/xvda2 ro'</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">disk        = [</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">                  'file:/vm/domains/vm01/disk.img,xvda2,w',</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">                  'file:/vm/domains/vm01/swap.img,xvda1,w',</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">              ]</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><br></div>
<span style="font-size:18px;">将原来的hda2修改为xvda2 </span>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">然后是调整网卡的相关配置</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">vif         = [ 'ip=192.168.1.21,mac=00:16:3E:9B:1A:90,bridge=eth0' ]</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">主要是加上bridge=eth0</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><br></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">这样就可以通过xm create vm01.cfg启动虚拟系统了</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><br></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;background-color:#000000;color:#ffffff;">root@node1:~# xm list</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;background-color:#000000;color:#ffffff;">Name                                        ID   Mem VCPUs      State   Time(s)</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;background-color:#000000;color:#ffffff;">Domain-0                                   0   879     1               r-----     26.6</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;background-color:#000000;color:#ffffff;">vm01                                         1   128     1                -b----      1.7</span></div>
<span style="font-size:18px;background-color:#000000;color:#ffffff;">root@node1:~#  </span>
<span style="font-size:18px;"><br></span>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><div style="font-family:Arial;font-size:medium;line-height:normal;">启动后，dom0下面会出现一个vif的虚拟网卡</div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="background-color:#000000;color:#ffffff;">root@node1:~# brctl show</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="background-color:#000000;color:#ffffff;">bridge name     bridge id                      STP enabled     interfaces</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="background-color:#000000;color:#ffffff;">eth0                 8000.001c42d8fe43         no                      peth0</span></div>
<span style="background-color:#000000;color:#ffffff;">                                                                                                vif1.0 </span>
<span style="background-color:#000000;color:#ffffff;"><br></span>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">如果在guest系统只能ping到dom0的eth0，而不能ping到外网</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">可以检查下</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;background-color:#000000;color:#ffffff;">root@node1:~# sysctl net.ipv4.ip_forward</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><br></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;"><span style="background-color:#000000;color:#ffffff;">net.ipv4.ip_forward = 0</span> </span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><br></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">只要打开这个ip_forward就可以了</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><br></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">修改/etc/sysctl.conf里面的net.ipv4.ip_forward=1</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">或者</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;background-color:#000000;color:#ffffff;">echo 1>/proc/sys/net/ipv4/ip_forward</span></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><br></div>
<div style="font-family:Arial;font-size:medium;line-height:normal;"><span style="font-size:18px;">也可以用命令临时生效</span></div>
<span style="font-size:18px;background-color:#000000;color:#ffffff;">sysctl -w net.ipv4.ip_forward=1 </span>
<span style="font-size:18px;background-color:#000000;color:#ffffff;"><br></span>
<b><span style="font-size:18px;">如果您的系统是CentOS，请参考我以前的文章。</span></b>
<h3 class="t" style="margin:0px;padding:0px;list-style:none;font-weight:normal;font-size:medium;font-family:arial;line-height:18px;"><a href="/post/2" target="_blank"><em style="font-style:normal;color:#cc0000;">CentOS虚拟化</em>服务的配置</a></h3>





<!--?xml version="1.0" encoding="UTF-8" standalone="no"?-->
</div>
<!--?xml version="1.0" encoding="UTF-8" standalone="no"?--><div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/post/13">CentOS 5.3 安装虚拟化后的内存识别问题.</a><a href="http://xinlogs.com/post/5">ubuntu 9.04安装xen</a><a href="http://xinlogs.com/post/10">怎么mount一个xen的img映像。转载</a><a href="http://xinlogs.com/post/11">CentOS里的Xen配置pygrub</a><a href="http://xinlogs.com/post/8">ubuntu 9.04 server 安装VMware Tools解决share folders问题</a>
</div>
