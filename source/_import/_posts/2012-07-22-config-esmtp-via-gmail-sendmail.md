--- 
categories: []
comments: true
layout: post
title: 配置ecshop通过本地esmtp发信
---
<b><span style="font-size:24px;">问题：</span></b>
<span style="font-size:14px;">最近有个客户的ecshop总是卡在下订单的地方。查询了下日志。发现如下</span>



``` 
script_filename = /public_html/quick_buy.php
[0x0a29511c] fgets() /public_html/includes/cls_smtp.php:314
[0x0a295074] get_data() /public_html/includes/cls_smtp.php:230
[0x0a294f5c] auth() /public_html/includes/cls_smtp.php:146
[0x0a294da8] send() ~:299
[0x0a293e80] send_mail() ~:2161
```


<span style="font-size:14px;">发现问题主要是因为在ecshop里的邮件服务器设置了采用其他smtp服务器方式。我估计是到达smtp服务器验证的时间有时候太长，导致这个订单流程失败。</span>
<span style="font-size:14px;">看到ecshop推荐采用本地邮件服务来发信，于是开始考虑采用这个方式。</span>
<span style="font-size:14px;">客户的服务器采用centos的linux服务器，php 5.3.8。</span>
<span style="font-size:14px;">首先说说php的mail函数的条件，需要在php.ini里面指定sendmail_path.</span>
<span style="font-size:14px;">先说下最简单的方式，就是服务器上安装sendmail，然后调用sendmail发送邮件。</span>
<span style="font-size:14px;">如果怕服务器sendmail不正常，最好卸载下再重新安装。</span>



``` 
卸载：
yum remove sendmail
安装
yum install sendmail
```

<span style="font-size:14px;">再设置下php.ini里的sendmail_path=/usr/sbin/sendmail -t -i</span>
<span style="font-size:14px;">这样后，就可以通过ecshop里面的本地邮件服务发信了。不过这种方式有个致命缺点，因为一般我们的服务器都不被大的邮件服务商认可，所以这样发过去的邮件基本都是被拦截后者放入垃圾邮件里。</span>
 
<span style="font-size:24px;"><b>解决方法：</b></span>
<span style="font-size:14px;">还是采用本地邮件服务，不过本地只是一个代理的作用，由本地的代理程序调用gmail的stmp功能发送邮件。</span>
<span style="font-size:14px;">本地安装esmtp程序，用来将我们的邮件通过gmail的帐号发送。这样就可以避免邮件被拦截或者认为是垃圾邮件了。</span>
 
<span style="font-size:14px;">为了一会测试方便，我们先安装一个linux下的邮件客户程序</span>



``` 
yum install mailx
```

<span style="font-size:14px;">安装后，我们就有了mail命令。可以通过下面的命令来发信到babodx@qq.com的邮箱</span>



``` 
echo "测试"|mail -s "ceshi" babodx@qq.com
```

<span style="font-size:24px;">安装esmtp：</span>
<span style="font-size:14px;">为了安装esmtp，我们必须先安装它依赖的一个库文件libesmtp-1.0.6</span>
<span style="font-size:16px;"><b>安装libesmtp-1.0.6</b></span>



``` 
tar zxvf libesmtp-1.0.6.tar.gz
cd libesmtp-1.0.6
./configure
make
make install
```

<span style="font-size:16px;"><b>安装esmtp 1.2</b></span>



``` 
tar jxvf esmtp-1.2.tar.bz2
cd esmtp-1.2
./configure --prefix=/usr/local/webserver/esmtp
make
make install
```

<span style="font-size:small;"><span style="line-height:24px;font-size:14px;">配置esmtp通过gmail帐号发信</span></span>
<span style="font-size:14px;line-height:24px;">vi /usr/local/webserver/esmtp/etc/esmtprc</span>
<span style="font-size:14px;line-height:24px;">内容如下</span>



``` 
identity yourname@gmail.com        
        hostname smtp.gmail.com:587
        username "yourname@gmail.com"
        password "yourpasswd"
        starttls enabled
        default

mda "/usr/bin/procmail -d %T"
```

<span style="font-size:14px;line-height:24px;"><b>mail测试</b></span>
<span style="font-size:14px;line-height:24px;">echo "测试邮件"|mail -s "ceshi" babodx@qq.com</span>
<span style="font-size:14px;line-height:24px;">如果测试通过，我们就再将/usr/sbin/sendmail链接到/usr/local/webserver/esmtp/bin/esmtp。</span>
<span style="font-size:14px;line-height:24px;">ln -s /usr/local/webserver/esmtp/bin/esmtp /usr/sbin/sendmail</span>
<span style="font-size:14px;line-height:24px;">以后就可以在ecshop里面通过本地邮件服务发信了。并且收件人显示的发件人是你设定的gmail帐号。</span>
<span style="font-size:14px;line-height:24px;"><br></span>
<span style="font-size:14px;line-height:24px;"><br></span>
<span style="font-size:14px;line-height:24px;"><br></span>



