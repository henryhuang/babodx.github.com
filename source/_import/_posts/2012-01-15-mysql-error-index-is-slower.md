--- 
categories: []
comments: true
layout: post
title: Mysql错误索引比没索引还慢
---
今天一个客户反映网站某个页面很慢。初步分析是数据库查询造成的延时。
打开数据库的慢查询日志，监控到如下语句

<div id="kindeditor" class="annotate"> # Time: 120115  1:09:37
# User@Host: sq_ttt[sq_ttt] @ localhost []
# Query_time: 3.895267  Lock_time: 0.000131 Rows_sent: 1  Rows_examined: 168835
use sq_ttt;
SET timestamp=1326560977;
SELECT A.*,A.aid AS id FROM p8_article A  WHERE A.yz=1  AND A.fid IN (31)  AND A.ispic=1 ORDER BY A.list DESC LIMIT 0,1;
</div>

这个查询用了3.8s时间。
于是进入数据库开始了这次分析。。。
 
首先看下表结构


<div id="kindeditor" class="annotate">CREATE TABLE `p8_article` (
  `aid` mediumint(7) unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(150) NOT NULL DEFAULT '',
  `smalltitle` varchar(100) NOT NULL DEFAULT '',
  `fid` mediumint(7) unsigned NOT NULL DEFAULT '0',
  `mid` mediumint(5) NOT NULL DEFAULT '0',
  `fname` varchar(50) NOT NULL DEFAULT '',
  `hits` mediumint(7) NOT NULL DEFAULT '0',
  `pages` smallint(4) NOT NULL DEFAULT '0',
  `comments` mediumint(7) NOT NULL DEFAULT '0',
  `posttime` int(10) NOT NULL DEFAULT '0',
  `list` int(10) NOT NULL DEFAULT '0',
  `uid` mediumint(7) NOT NULL DEFAULT '0',
  `username` varchar(30) NOT NULL DEFAULT '',
  `author` varchar(30) NOT NULL DEFAULT '',
  `copyfrom` varchar(100) NOT NULL DEFAULT '',
  `copyfromurl` varchar(150) NOT NULL DEFAULT '',
  `titlecolor` varchar(15) NOT NULL DEFAULT '',
  `fonttype` tinyint(1) NOT NULL DEFAULT '0',
  `picurl` varchar(150) NOT NULL DEFAULT '0',
  `ispic` tinyint(1) NOT NULL DEFAULT '0',
  `yz` tinyint(1) NOT NULL DEFAULT '0',
  `yzer` varchar(30) NOT NULL DEFAULT '',
  `yztime` int(10) NOT NULL DEFAULT '0',
  `levels` tinyint(2) NOT NULL DEFAULT '0',
  `levelstime` int(10) NOT NULL DEFAULT '0',
  `keywords` varchar(100) NOT NULL DEFAULT '',
  `jumpurl` varchar(150) NOT NULL DEFAULT '',
  `iframeurl` varchar(150) NOT NULL DEFAULT '',
  `style` varchar(15) NOT NULL DEFAULT '',
  `template` varchar(255) NOT NULL DEFAULT '',
  `target` tinyint(1) NOT NULL DEFAULT '0',
  `ip` varchar(15) NOT NULL DEFAULT '',
  `lastfid` mediumint(7) NOT NULL DEFAULT '0',
  `money` mediumint(7) NOT NULL DEFAULT '0',
  `buyuser` text NOT NULL,
  `passwd` varchar(32) NOT NULL DEFAULT '',
  `allowdown` varchar(150) NOT NULL DEFAULT '',
  `allowview` varchar(150) NOT NULL DEFAULT '',
  `editer` varchar(30) NOT NULL DEFAULT '',
  `edittime` int(10) NOT NULL DEFAULT '0',
  `begintime` int(10) NOT NULL DEFAULT '0',
  `endtime` int(10) NOT NULL DEFAULT '0',
  `description` text NOT NULL,
  `lastview` int(10) NOT NULL DEFAULT '0',
  `digg_num` mediumint(7) NOT NULL DEFAULT '0',
  `digg_time` int(10) NOT NULL DEFAULT '0',
  `forbidcomment` tinyint(1) NOT NULL DEFAULT '0',
  `ifvote` tinyint(1) NOT NULL DEFAULT '0',
  `heart` varchar(255) NOT NULL DEFAULT '',
  `htmlname` varchar(100) NOT NULL DEFAULT '',
  PRIMARY KEY (`aid`),
  KEY `list` (`list`,`fid`,`yz`,`ispic`) USING BTREE
) ENGINE=MyISAM AUTO_INCREMENT=176567 DEFAULT CHARSET=gbk
</div>


 
然后我们采用explain看看是否使用了索引


<div id="kindeditor" class="annotate">mysql> explain SELECT A.list,A.aid AS id FROM p8_article A  WHERE A.yz=1  AND A.fid IN (31)  AND A.ispic=1 ORDER BY A.list DESC LIMIT 0,1;
+----+-------------+-------+-------+---------------+------+---------+------+------+-------------+
| id | select_type | table | type  | possible_keys | key  | key_len | ref  | rows | Extra       |
+----+-------------+-------+-------+---------------+------+---------+------+------+-------------+
|  1 | SIMPLE      | A     | index | NULL          | list | 9       | NULL |    1 | Using where |
+----+-------------+-------+-------+---------------+------+---------+------+------+-------------+
1 row in set (0.00 sec)
</div>
可以发现使用了list索引，这个查询类型是index
注意：用explain分析的时候，这个type的速度顺序如下：

<b><span style="color:#000000;">system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL</span></b>
<b><span style="color:#000000;">一般我们优化，最少要到range。</span></b>
<b><span style="color:#000000;"><br></span></b>
<span style="color:#000000;">不过今天的问题没有这样简单，因为当我删除索引后，发现即使不用索引也比这个快。</span>
<span style="color:#000000;">下面是我删掉全部索引的结果</span>


<div id="kindeditor" class="annotate">
<span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">mysql> SELECT A.list,A.aid AS id FROM p8_article A  WHERE A.yz=1  AND A.fid IN (31)  AND A.ispic=1 ORDER BY A.list DESC LIMIT 0,1;</span>
<span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">+------------+------+</span>
<span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">| list       | id   |</span>
<span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">+------------+------+</span>
<span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">| 1323887041 | 1312 |</span>
<span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">+------------+------+</span>
<span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">1 row in set (0.31 sec)</span>
</div>

<div style="color:#000000;font-family:Georgia;font-size:12px;letter-spacing:2px;line-height:20px;">用explain查看没有索引的状态如下</div>
<div>
<div></div>
<div id="kindeditor" class="annotate">
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">mysql> explain SELECT A.list,A.aid AS id FROM p8_article A  WHERE A.yz=1  AND A.fid IN (31)  AND A.ispic=1 ORDER BY A.list DESC LIMIT 0,1;</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">+----+-------------+-------+------+---------------+------+---------+------+--------+-----------------------------+</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">| id | select_type | table | type | possible_keys | key  | key_len | ref  | rows   | Extra                       |</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">+----+-------------+-------+------+---------------+------+---------+------+--------+-----------------------------+</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">|  1 | SIMPLE      | A     | ALL  | NULL          | NULL | NULL    | NULL | 175175 | Using where; Using filesort |</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">+----+-------------+-------+------+---------------+------+---------+------+--------+-----------------------------+</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">1 row in set (0.00 sec)</span></div>
</div>
<div></div>
我们可以看到type是ALL，Extra里面还用了filesort.但即使这样，查询时间才<span class="Apple-style-span" style="background-color:#e7e7e7;">0.31 sec</span>
可以看到加了索引后，反而慢了。这也是我今天想说的一个问题：<span style="color:#e53333;"><b>错误的索引可能会更慢！</b></span>
<span style="color:#e53333;"><b><br></b></span>
<span style="color:#e53333;"><b><span style="color:#000000;">调整索引</span></b></span>
<span style="color:#e53333;"><span style="color:#000000;">我们删掉list索引后，尝试重新建立这个复合索引。</span></span>


<div id="kindeditor" class="annotate">mysql> create index list on p8_article(yz,fid,ispic,list);
Query OK, 175175 rows affected (2.80 sec)
Records: 175175  Duplicates: 0  Warnings: 0
</div>

这次索引的区别就在于顺序的改变，将list放在了最后。这个可以参考mysql联合索引的最左前缀。
这样在看看explain的状态




</div>
<div id="kindeditor" class="annotate">
<div>

mysql> explain SELECT SQL_NO_CACHE A.list,A.aid AS id FROM p8_article A  WHERE A.yz=1  AND A.fid IN (31)  AND A.ispic=1 ORDER BY A.list DESC LIMIT 0,1;
+----+-------------+-------+------+---------------+------+---------+-------------------+------+-------------+
| id | select_type | table | type | possible_keys | key  | key_len | ref               | rows | Extra       |
+----+-------------+-------+------+---------------+------+---------+-------------------+------+-------------+
|  1 | SIMPLE      | A     | ref  | list          | list | 5       | const,const,const |   37 | Using where |
+----+-------------+-------+------+---------------+------+---------+-------------------+------+-------------+
1 row in set (0.00 sec)


</div>
<div style="color:#000000;font-family:Georgia;font-size:12px;letter-spacing:2px;line-height:20px;"></div>
</div>
<div style="color:#000000;font-family:Georgia;font-size:12px;letter-spacing:2px;line-height:20px;">type已经是ref了，Extra也不在使用filesort了。</div>
<div style="color:#000000;font-family:Georgia;font-size:12px;letter-spacing:2px;line-height:20px;">再实际运行查询，看看时间上的变化</div>
<div>
<div></div>
</div>
<div id="kindeditor" class="annotate">
<div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">mysql> SELECT SQL_NO_CACHE A.list,A.aid AS id FROM p8_article A  WHERE A.yz=1  AND A.fid IN (31)  AND A.ispic=1 ORDER BY A.list DESC LIMIT 0,1;</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">+------------+------+</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">| list       | id   |</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">+------------+------+</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">| 1323887041 | 1312 |</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">+------------+------+</span></div>
<div><span class="Apple-style-span" style="font-size:12px;letter-spacing:2px;line-height:20px;">1 row in set (0.00 sec)</span></div>
</div>
<div style="color:#000000;font-family:Georgia;font-size:12px;letter-spacing:2px;line-height:20px;"></div>
</div>
<div style="color:#000000;font-family:Georgia;font-size:12px;letter-spacing:2px;line-height:20px;">为了怕有query_cache的原因，所以特意加入了SQL_NO_CACHE。</div>
现在显示查询时间为0.00 sec 基本可以忽略不计了。而经过索引的调整，网页的速度也确实得到了改善。
 
<b>总结：索引错误的使用反而会拖慢查询，甚至比不用还慢。一定要根据最左前缀建立合适的索引，才可以起到加速查询的作用！</b>


 

<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/eMule-VPN-HighID">通过VPN获取eMule的HighID</a><a href="http://xinlogs.com/disable-mysql-query-log">开启或者关闭MYSQL的query log(查询日志)</a><a href="http://xinlogs.com/ruby_import_dbf_to_mysql">ruby将DBF文件导入MYSQL库</a><a href="http://xinlogs.com/post/22">一个在windows下很好用的MySQL工具Navicat</a><a href="http://xinlogs.com/squid-cache-php-script">squid缓存php页面</a>
</div>
