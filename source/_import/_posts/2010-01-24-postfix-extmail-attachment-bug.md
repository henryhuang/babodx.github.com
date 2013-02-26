--- 
categories: []
comments: true
layout: post
title: postfix+extmail的附件大小显示和限制问题解决办法
---
最近在给一个公司部署了postfix+<span class="t_tag" onclick="tagshow(event)" href="tag.php?name=extmail">extmail</span>的<span class="t_tag" onclick="tagshow(event)" href="tag.php?name=%E9%82%AE%E4%BB%B6">邮件</span><span class="t_tag" onclick="tagshow(event)" href="tag.php?name=%E7%B3%BB%E7%BB%9F">系统</span>后，遇到了附件大小限制问题。<br><br>
ext<span class="t_tag" onclick="tagshow(event)" href="tag.php?name=mail">mail</span>默认设置是5M，但是由于extmail的统计大小方法问题，一般附件限制比设置的小了1/3。也就是你设置了5M，也就能到3M多。<br><br>
这里先说下大小限制的配置。<br><br>
一、修改extmail的<span class="t_tag" onclick="tagshow(event)" href="tag.php?name=web">web</span>mail.cf文件<br><br>
SYS_MESSAGE_SIZE_LIMIT = 5242880 注意：以位为单位为5M字节。 <br><br>
SYS_MESSAGE_SIZE_LIMIT = xxx<br><br>
二、修改/etc/postfix/main.cf文件，<br><br>
message_size_limit = xxx<br><br><br>
三、重启postfix即可，service postfix restrart 和重启apache，service httpd restart<br><br><br><strong>如何解决附件限制大小和系统文件大小相差1/3呢？</strong>
首先是修改/var/www/extsuite/extmail/libs/Ext/App目录下的Compose.pm文件<br>
 
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_4811');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_4811">
<ol class="dp-xml">
<li class="alt"><span><span>sub is_oversize {   </span></span></li>
    <li>
<span>    my $</span><span class="attribute">self</span><span> = </span><span class="attribute-value">shift</span><span>;   </span>
</li>
    <li class="alt">
<span>    my $</span><span class="attribute">sys</span><span> = $self-</span><span class="tag">></span><span>{sysconfig};   </span>
</li>
    <li>
<span>    my $</span><span class="attribute">maxsize</span><span> = $sys-</span><span class="tag">></span><span>{SYS_MESSAGE_SIZE_LIMIT};   </span>
</li>
    <li class="alt">
<span>    my $</span><span class="attribute">tsize</span><span> = </span><span class="attribute-value">shift</span><span>;   </span>
</li>
    <li><span>  </span></li>
    <li class="alt">
<span>    return 0 unless defined $maxsize and $maxsize </span><span class="tag">></span><span> 0;   </span>
</li>
    <li>
<span>    return 0 unless defined $tsize and $tsize </span><span class="tag">></span><span> 0;   </span>
</li>
    <li class="alt"><span>  </span></li>
    <li>
<span>    if ($tsize </span><span class="tag">></span><span>= $maxsize) {   </span>
</li>
    <li class="alt"><span>        return $maxsize;   </span></li>
    <li><span>    }   </span></li>
    <li class="alt"><span>    0;   </span></li>
    <li><span>}   </span></li>
</ol>
</div>
<link href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css" type="text/css" rel="stylesheet">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
<br>
注意到上面的if ($tsize >= $maxsize)了吧，修改为if ($tsize >= 1.29*$maxsize)<br><br>
这样修改后，基本extmail的附件限制大小就和系统显示的文件大小一样。<br><br>
目前还有一个文件，就是我们上传一个1M的内容，显示出来的附件大小大约是1.3M，这个问题需要修改一个human函数解决。<br><br>
在/var/www/extsuite/extmail/libs/Ext目录下，打开Utils.pm文件<br><br>
找到如下函数<br><br>
 
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_6657');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_6657">
<ol class="dp-xml">
<li class="alt"><span><span>sub human_size {   </span></span></li>
    <li>
<span>    my $</span><span class="attribute">s</span><span> = $_[0];   </span>
</li>
    <li class="alt">
<span>    if($s</span><span class="tag"><</span><span class="tag-name">1024</span><span>) {   </span>
</li>
    <li><span>        return sprintf("%0.2f", $s/1024)."K";   </span></li>
    <li class="alt">
<span>    }elsif($s</span><span class="tag"><</span><span class="tag-name">1024</span><span>*1024) {   </span>
</li>
    <li><span>        return sprintf("%0.1f", $s/1024)."K";   </span></li>
    <li class="alt"><span>    }else {   </span></li>
    <li><span>        return sprintf("%0.1f", $s/(1024*1024))."M";   </span></li>
    <li class="alt"><span>    }   </span></li>
    <li><span>}   </span></li>
</ol>
</div>
<link href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css" type="text/css" rel="stylesheet">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
将函数修改如下<br>
 
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_7785');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_7785">
<ol class="dp-xml">
<li class="alt"><span><span>sub human_size {   </span></span></li>
    <li>
<span>    my $</span><span class="attribute">s</span><span> = $_[0];   </span>
</li>
    <li class="alt">
<span>    if($s</span><span class="tag"><</span><span class="tag-name">1024</span><span>) {   </span>
</li>
    <li><span>        return sprintf("%0.2f", $s/1024/1.36)."K";   </span></li>
    <li class="alt">
<span>    }elsif($s</span><span class="tag"><</span><span class="tag-name">1024</span><span>*1024) {   </span>
</li>
    <li><span>        return sprintf("%0.1f", $s/1024/1.36)."K";   </span></li>
    <li class="alt"><span>    }else {   </span></li>
    <li><span>        return sprintf("%0.1f", $s/(1024*1024*1.36))."M";   </span></li>
    <li class="alt"><span>    }   </span></li>
    <li><span>}   </span></li>
</ol>
</div>
<link href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css" type="text/css" rel="stylesheet">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
这样就解决了extmail的附件大小显示和限制问题。<br><br>
目前还有一个遗留问题，就是在folder.htm模板界面里显示的邮件大小还是比系统文件大1/3。这个不知道怎么修改了。找到了对应函数，但是它直接显示的多少M或者多少K，用除1.36会出现问题。。。。这个期待<span class="t_tag" onclick="tagshow(event)" href="tag.php?name=%E9%AB%98%E6%89%8B">高手</span>解决了。
