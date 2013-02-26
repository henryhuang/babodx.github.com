--- 
categories: []
comments: true
layout: post
title: "开启或者关闭MYSQL的query log(查询日志)"
---
最近在整理服务器的时候，发现mysql的查询日志文件占用了很多空间。这个查询日志文件部分内容如下，随着查询增加，会越来越大
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_3583');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_3583">
<ol class="dp-xml" style="border-bottom: 0px; border-left: 0px; list-style-type: none; margin-left: 5px; border-top: 0px; border-right: 0px">
<li class="alt"><span><span>Tcp port: 3306 Unix socket: /tmp/mysql.sock </span></span></li>
    <li><span>Time Id Command Argument </span></li>
    <li class="alt"><span>100404 8:36:49 1 Connect <a href="mailto:zzz@localhost">zzz@localhost</a> on </span></li>
    <li><span>1 Init DB zzz </span></li>
    <li class="alt"><span>1 Query SET NAMES 'utf8' </span></li>
    <li><span>1 Query SELECT * FROM `boblog_counter` LIMIT 0,1 </span></li>
    <li class="alt">
<span>1 Query SELECT `blogid`,`pubtime`,`edittime`,`blogalias` FROM `boblog_blogs` WHERE `property`</span><span class="tag"><</span><span class="tag-name">2</span><span> ORDER BY `pubtime`DESC LIMIT 0, 1500 </span>
</li>
    <li><span>1 Quit </span></li>
    <li class="alt"><span>100404 8:38:01 2 Connect admin@localhost on </span></li>
    <li><span>2 Init DB zzz </span></li>
    <li class="alt"><span>2 Query SET NAMES 'utf8' </span></li>
    <li>
<span>2 Query SET </span><span class="attribute">sql_mode</span><span>=</span><span class="attribute-value">''</span><span> </span>
</li>
    <li class="alt"><span>2 Query SELECT * FROM `cmseasy_settings` WHERE `tag`='table-fieldset' ORDER BY 1 DESC limit 1 </span></li>
    <li><span>2 Init DB zzz </span></li>
    <li class="alt"><span>2 Query SET NAMES 'utf8' </span></li>
    <li>
<span>2 Query SET </span><span class="attribute">sql_mode</span><span>=</span><span class="attribute-value">''</span><span> </span>
</li>
    <li class="alt">
<span>2 Query SELECT * FROM `cmseasy_user` WHERE userid</span><span class="tag">></span><span>0 ORDER BY 1 DESC limit 1 </span>
</li>
    <li><span>2 Init DB zzz </span></li>
    <li class="alt"><span>2 Query SET NAMES 'utf8' </span></li>
    <li>
<span>2 Query SET </span><span class="attribute">sql_mode</span><span>=</span><span class="attribute-value">''</span><span> </span>
</li>
    <li class="alt"><span>2 Query SELECT count(typeid) as rec_sum FROM `cmseasy_type` </span></li>
    <li><span>2 Query SELECT * FROM `cmseasy_type` ORDER BY `order`,1 limit 1000 </span></li>
    <li class="alt"><span>2 Init DB zzz</span></li>
    <li><span>2 Query SET NAMES 'utf8' </span></li>
    <li class="alt">
<span>2 Query SET </span><span class="attribute">sql_mode</span><span>=</span><span class="attribute-value">''</span><span> </span>
</li>
    <li>
<span>2 Query SELECT count(1) as rec_sum FROM `cmseasy_friendlink` WHERE state</span><span class="tag">></span><span>0 and </span><span class="attribute">linktype</span><span>=</span><span class="attribute-value">1</span><span> </span>
</li>
    <li class="alt">
<span>2 Query SELECT * FROM `cmseasy_friendlink` WHERE state</span><span class="tag">></span><span>0 and </span><span class="attribute">linktype</span><span>=</span><span class="attribute-value">1</span><span> ORDER BY listorder asc,id limit 20 </span>
</li>
    <li><span>2 Init DB zzz</span></li>
    <li><span>2 Query SET NAMES 'utf8' </span></li>
    <li>
<span>2 Query SET </span><span class="attribute">sql_mode</span><span>=</span><span class="attribute-value">''</span><span> </span>
</li>
    <li class="alt">
<span>2 Query SELECT count(1) as rec_sum FROM `cmseasy_friendlink` WHERE state</span><span class="tag">></span><span>0 and </span><span class="attribute">linktype</span><span>=</span><span class="attribute-value">1</span><span> </span>
</li>
    <li>
<span>2 Query SELECT * FROM `cmseasy_friendlink` WHERE state</span><span class="tag">></span><span>0 and </span><span class="attribute">linktype</span><span>=</span><span class="attribute-value">1</span><span> ORDER BY listorder asc,id limit 20 </span>
</li>
    <li class="alt"><span>2 Init DB zzz </span></li>
    <li><span>2 Query SET NAMES 'utf8' </span></li>
    <li class="alt">
<span>2 Query SET </span><span class="attribute">sql_mode</span><span>=</span><span class="attribute-value">''</span><span> </span>
</li>
    <li>
<span>2 Query SELECT * FROM `cmseasy_templatetag` WHERE </span><span class="attribute">name</span><span>=</span><span class="attribute-value">'å~E¬å~O¸ç®~@ä»~K'</span><span> ORDER BY 1 DESC limit 1 </span>
</li>
    <li class="alt"><span>2 Init DB zzz </span></li>
    <li><span>2 Query SET NAMES 'utf8' </span></li>
    <li class="alt">
<span>2 Query SET </span><span class="attribute">sql_mode</span><span>=</span><span class="attribute-value">''</span><span> </span>
</li>
    <li><span>2 Query Describe cmseasy_archive </span></li>
    <li class="alt">
<span>2 Query SELECT count(1) as rec_sum FROM `cmseasy_archive` WHERE typeid in (2) and </span><span class="attribute">checked</span><span>=</span><span class="attribute-value">1</span><span> and (state IS NULL or state</span><span class="tag"><</span><span class="tag">></span><span>'-1') </span>
</li>
    <li>
<span>2 Query SELECT * FROM `cmseasy_archive` WHERE typeid in (2) and </span><span class="attribute">checked</span><span>=</span><span class="attribute-value">1</span><span> and (state IS NULL or state</span><span class="tag"><</span><span class="tag">></span><span>'-1') ORDER BY aid desc limit 4 </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
其实记录的都是mysql执行的一些select语句，对于正常运行的服务器，我觉得基本没有必要保留这些日志。
关键是怎么关闭，这个日志的开启与否，可以通过mysql里提交如下命令查看
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_8683');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_8683">
<ol class="dp-xml" style="border-bottom: 0px; border-left: 0px; list-style-type: none; margin-left: 5px; border-top: 0px; border-right: 0px">
<li class="alt"><span><span>show variables like 'log'; </span></span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
如果现实 log 的值是 ON就代码开启，如果是OFF就代表关闭
我们可以通过my.cnf配置文件进行配置
在my.cnf文件里，加入general-log = 0就关闭了这个查询日志。如果是general-log = 1就开启了这个查询日志。<br>
 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/post/23">如何建立两个表间的外键</a><a href="http://xinlogs.com/post/113">mysqldump解决中文乱码问题</a><a href="http://xinlogs.com/update-115disk-urls">批量更换115网盘链接</a><a href="http://xinlogs.com/HeidiSQL">HeidiSQL一款开源的MySQL图形管理软件</a><a href="http://xinlogs.com/mysql-error-index-is-slower">Mysql错误索引比没索引还慢</a>
</div>
