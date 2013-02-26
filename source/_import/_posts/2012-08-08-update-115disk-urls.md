--- 
categories: []
comments: true
layout: post
title: 批量更换115网盘链接
---
<span style="font-size:18px;">今天115网盘关闭了对外分享链接的功能。我的客户有人是用115网盘作为服务器资源存储用的，网站上下载资源都是采用115网盘地址来发布的。这样直接造成全部资源都无法下载了，我临时写了个php脚本来更新全部的115网盘链接到本地服务器地址。</span>
 
<b><span style="font-size:24px;">主要思路：</span></b>
<span style="font-size:18px;">首先将网盘里的资源全部下载到服务器，然后导出一个网盘地址和文件实际名字的列表</span>
<span style="font-size:18px;">如下所示：</span>
<div id="kindeditor" class="quote">http://115.com/file/e78cpfjd#Dark_Room.rar<br>
http://115.com/file/e78cpwkh#Dark_Energy.rar</div>
<span style="font-size:18px;">可以看到前面是网盘下载的url，＃号后面是实际的文件名。</span>
<span style="font-size:18px;">我将这个列表保存在一个文件里，便于php脚本获取。</span>
<span style="font-size:18px;">然后编写一个php脚本，把文件里每行的url取出来，到数据库里把这个下载链接替换为我们服务器上的地址。主要是采用mysql的replace函数实现</span>
 
脚本如下：


``` 
#!/usr/bin/php
<?php
print("=======fix_url v1=========\n");
print("=======write by babodx@gmail.com=======\n");


function update_url($url) {
    list($url_115,$url_tv1926)=explode("#",$url);
    $url_tv1926="http://xiazai.tv1926.com/".$url_tv1926;
    print("update:");
    print($url_115);
    print(">>");
    $link = mysql_connect('localhost', 'username', 'passwd');
    if (!$link) {
        die('Could not connect: ' . mysql_error());
    }

    mysql_select_db('my_database', $link) or die ('Can\'t use my_database : ' .         mysql_error());

    $sql="update p8_article_content_101 set     my_801=replace(my_801,'".$url_115."','".$url_tv1926."')";
    mysql_query("set names gbk");
    $result = mysql_query($sql)
    or die("Invalid query: " . mysql_error());

    mysql_close($link);
    print($url_tv1926);
    print("\n");
}

if (is_null($argv[1])) {
    print("argv1 is null\n");
    exit();
}
$file_name=$argv[1];
print("get files name...\n");
$file_names=array();
$handle = @fopen($file_name, "r");
if ($handle) {
    while (!feof($handle)) {
        $buffer = fgets($handle);
        $tmp_file_name=trim($buffer);
        array_push($file_names,$tmp_file_name);

    }
    fclose($handle);
}

foreach ($file_names as $file_url) {
    update_url($file_url);
}
print("over!");



?>
```


<span style="font-size:18px;">如果觉得这个脚本有用，可以从网盘下载（网盘的脚本添加了注释）</span>
http://babo.qjwm.com/down_4261688.html<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/post/24">Mysql设置my.cnf来记录下全部sql请求</a><a href="http://xinlogs.com/check-slow-php-script">如何找到执行慢的php程序</a><a href="http://xinlogs.com/post/23">如何建立两个表间的外键</a><a href="http://xinlogs.com/disable-mysql-query-log">开启或者关闭MYSQL的query log(查询日志)</a><a href="http://xinlogs.com/post/22">一个在windows下很好用的MySQL工具Navicat</a>
</div>
