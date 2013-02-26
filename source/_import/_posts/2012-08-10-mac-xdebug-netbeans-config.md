--- 
categories: []
comments: true
layout: post
title: mac下配置php调试环境
---
<span style="font-size:18px;">已经用了mac有2个月了，基本网络管理和开发工作都在mac下面了。</span>
<span style="font-size:18px;">下面说下如何配置mac下面的php调试环境</span>
<span style="font-size:18px;">我的环境：macbook air 2012款   Mac OS X 10.8 系统</span>
<span style="font-size:18px;">php环境：XAMPP 1.7.3</span>
<span style="font-size:medium;"><span style="line-height:27px;">开发工具：一般用sublime text 2，调试或者项目开发用netbeans</span></span>
转载请注明：来源http://xinlogs.com
 
<span style="font-size:medium;"><span style="line-height:27px;">下面说说如何安装调试环境，主要就是xdebug的安装和配置</span></span>
<span style="font-size:medium;"><span style="line-height:27px;">这里需要提下mac下面的安装包管理工具homebrew</span></span>
<span style="font-size:medium;"><span style="line-height:27px;">我就是用brew来安装xdebug的。homebrew是一个用ruby开发的，类似macport的工具。可以在线安装软件，就像linux下的apt-get或者yum一样。</span></span>
<span style="font-size:medium;"><span style="line-height:27px;"><br></span></span>
<span style="font-size:medium;"><span style="line-height:27px;">安装homebrew</span></span>
<span style="font-size:medium;"><span style="line-height:27px;">需要系统可以支持ruby，通过ruby -v命令可以查看ruby版本。mac os 自带ruby</span></span>
<span style="font-size:medium;"><span style="line-height:27px;">直接在终端里输入</span></span>








<span class="s1" style="background-color:#000000;color:#ffffff;">ruby <(curl -fsSk <a href="https://raw.github.com/mxcl/homebrew/go"><span class="s2" style="background-color:#000000;color:#ffffff;">https://raw.github.com/mxcl/homebrew/go</span></a>)</span>

<span style="font-size:18px;">上面命令执行完，brew命令就可以执行了。</span>
<span style="font-size:18px;">先运行brew doctor测试下环境是否完整，如果需要，还要添加Command Line Tools。</span>

<span style="font-size:18px;">添加Command Line Tools</span>
<span style="font-size:18px;">打开Xcode，在preferences->downloads里面下载并安装Command Line Tools</span>
<a target="_blank" href="/content/uploadfile/201208/b7bd19d4120ebf0aeec8e81f597afd4820120810025014.png" id="ematt:78"><img src="/content/uploadfile/201208/thum-b7bd19d4120ebf0aeec8e81f597afd4820120810025014.png" width="420" height="296" title="Command Line Tools安装" alt="Command Line Tools安装" border="0"></a>

<span style="font-size:18px;">一切正确的话，执行brew doctor显示如下</span>
<span style="background-color:#000000;color:#ffffff;font-size:18px;">babomatoMacBook-Air:~ babo$ brew doctor</span>
<span style="background-color:#000000;color:#ffffff;font-size:18px;">Your system is raring to brew.</span>
<div><br></div>
<div>安装好brew后，先执行brew update更新下软件包的源，然后再加载两个formula的资源</div>
<div><span style="font-size:18px;background-color:#000000;color:#ffffff;">brew update</span></div>
<div>
<span style="font-size:18px;background-color:#000000;color:#ffffff;"> </span><span style="font-size:18px;background-color:#000000;color:#ffffff;"> </span><span style="font-size:18px;background-color:#000000;color:#ffffff;"> </span><span style="font-size:18px;background-color:#000000;color:#ffffff;"> </span><span style="font-size:18px;background-color:#000000;color:#ffffff;"> </span><span style="font-size:18px;background-color:#000000;color:#ffffff;"> </span><span style="font-size:18px;background-color:#000000;color:#ffffff;">brew tap josegonzalez/homebrew-php</span>
<span style="font-size:18px;background-color:#000000;color:#ffffff;"> </span><span style="font-size:18px;background-color:#000000;color:#ffffff;">brew tap homebrew/dupes</span>
<span style="font-size:medium;"><span style="line-height:27px;">完成后，就可以通过brew search和brew install来查找并安装软件了</span></span>
<span style="font-size:medium;"><span style="line-height:27px;background-color:#000000;color:#ffffff;">brew install php53-xdebug</span></span>
<span style="color:#ffffff;font-size:medium;"><span style="line-height:27px;color:#000000;">安装好xdebug后，还需要配置php.ini才可以正常使用。</span></span>
<span style="font-size:medium;"><span style="line-height:27px;">先用brew info查看xdebug相关信息</span></span>
<span style="font-size:medium;"><span style="line-height:27px;">

``` 
babomatoMacBook-Air:~ babo$ brew info php53-xdebug
php53-xdebug: stable 2.2.1, HEAD
http://xdebug.org
Depends on: autoconf
/usr/local/Cellar/php53-xdebug/2.2.1 (4 files, 360K) *
https://github.com/josegonzalez/homebrew-php/commits/master/Formula/php53-xdebug.rb
==> Options
--without-config-file
	Do not add ext-xdebug.ini to /usr/local/etc/php/5.3/conf.d
==> Caveats
To finish installing xdebug for PHP 5.3:
  * /usr/local/etc/php/5.3/conf.d/ext-xdebug.ini was created,
    do not forget to remove it upon extension removal.
  * Restart your webserver.
  * Write a PHP page that calls "phpinfo();"
  * Load it in a browser and look for the info on the xdebug module.
  * If you see it, you have been successful!
```

</span></span><span style="font-size:medium;"><span style="line-height:27px;">通过信息我们知道xdebug版本是 stable 2.2.1</span></span>
<span style="font-size:medium;"><span style="line-height:27px;">配置文件在</span></span><span style="line-height:27px;">/usr/local/etc/php/5.3/conf.d/ext-xdebug.ini</span>
<span style="line-height:27px;background-color:#ffffff;font-size:18px;">我们xampp的配置文件在</span>/Applications/XAMPP/xamppfiles/etc/php.ini
<span style="line-height:normal;">将xdebug的配置加入到</span>/Applications/XAMPP/xamppfiles/etc/php.ini
<span style="line-height:normal;">在文件最下面加入如下信息</span>
<span style="line-height:normal;">

``` 
[xdebug]
zend_extension="/usr/local/Cellar/php53-xdebug/2.2.1/xdebug.so"
        xdebug.remote_enable=1 
        xdebug.remote_host=localhost 
        xdebug.remote_port=9000 
        xdebug.remote_handler=dbgp 
```

</span><span style="line-height:normal;">重启xampp的apache，然后访问phpinfo查看是否成功加载xdebug</span>
<span style="line-height:normal;"><a target="_blank" href="/content/uploadfile/201208/fa1135a0a91f2b16a940677ba17e5ee420120810030534.png" id="ematt:79"><img src="/content/uploadfile/201208/thum-fa1135a0a91f2b16a940677ba17e5ee420120810030534.png" width="420" height="226" title="xdebug配置信息" alt="xdebug配置信息" border="0"></a><br></span>


<span style="font-size:18px;background-color:#000000;color:#ffffff;"><br></span>
<span style="font-size:18px;color:#000000;">配置netbeans可以调用xdebug</span>
<span style="font-size:18px;color:#000000;">首先将netbeans的偏好设置里的php5解释器设置为xampp的php。不要用系统自带的</span>
<span style="font-size:18px;color:#000000;"><a target="_blank" href="/content/uploadfile/201208/a7bf682822d31105489112d2407faad120120810030918.png" id="ematt:80"><img src="/content/uploadfile/201208/thum-a7bf682822d31105489112d2407faad120120810030918.png" width="420" height="128" title="netbeans设置php解释器" alt="netbeans设置php解释器" border="0"></a><br></span>
<span style="font-size:18px;">再设置调试的端口</span>
<span style="font-size:18px;"><a target="_blank" href="/content/uploadfile/201208/b72398a82969d697e91341de4795ea9a20120810031012.png" id="ematt:81"><img src="/content/uploadfile/201208/thum-b72398a82969d697e91341de4795ea9a20120810031012.png" width="420" height="361" title="netbeans设置xdebug端口" alt="netbeans设置xdebug端口" border="0"></a><br></span>
<span style="font-size:18px;">这些都设置好，就可以调试php代码了</span>
<span style="font-size:medium;"><span style="line-height:27px;">最后放一张我调试的截图</span></span>
<span style="font-size:medium;"><span style="line-height:27px;"><a target="_blank" href="/content/uploadfile/201208/252bb5eba38b1183421ad606648254a920120810031405.png" id="ematt:82"><img src="/content/uploadfile/201208/thum-252bb5eba38b1183421ad606648254a920120810031405.png" alt="点击查看原图" border="0"></a><br></span></span>
</div>

<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/mac-vnc-rdp-over-ssh">mac下用ssh转发端口</a><a href="http://xinlogs.com/update-115disk-urls">批量更换115网盘链接</a><a href="http://xinlogs.com/php-performance-debugging-tools">php性能调试工具</a><a href="http://xinlogs.com/squid-cache-php-script">squid缓存php页面</a><a href="http://xinlogs.com/find-php-slow-code">php-fpm查找php慢速代码</a>
</div>
