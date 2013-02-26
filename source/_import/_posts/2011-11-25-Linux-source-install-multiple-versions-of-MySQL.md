--- 
categories: []
comments: true
layout: post
title: Linux源码安装MySQL多版本共存
---
       原来我的服务器上是MySQL 5.5.16。本来是用的挺稳定。但是当时编译的时候由于手误将DDEFAULT_COLLATION=utf8-general_ci \弄错啦。。。应该是utf8_general_ci。导致很多程序用的时候报错，或者需要修改。
最近发现MySQL更新到了5.5.18了，就打算迁移到新数据库正好修正原来问题。
为了不影响访问，就打算在5.5.16运行的情况下，继续安装好5.5.18.等顺利转移到5.5.18后，再关闭原来MySQL5.5.16版本。
也就有了今天这篇文章。在Linux下从源码安装多版本或者多个MySQL数据库。
<b>重点：多版本或者多个MySQL同时运行，主要就是多个MySQL需要运行在不同目录，不同的端口，采用不同的数据库路径。</b> 
<b>步骤</b>
<b>1、首先是下载需要安装的MySQL</b>

下载mysql 5.5.16

<div id="kindeditor" class="quote">wget http://dev.mysql.com/get/Downloads/MySQL-5.5/mysql-5.5.16.tar.gz/from/http://mysql.mirrors.pair.com/</div>

<span class="Apple-style-span" style="font-weight:normal;">下载mysql 5.5.18</span>

<div id="kindeditor" class="quote">wget <span class="Apple-style-span" style="font-size:12px;line-height:18px;background-color:#ffead4;">http://dev.mysql.com/get/Downloads/MySQL-5.5/mysql-5.5.16.tar.gz/from/http://mysql.mirrors.pair.com/</span>
</div>

<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><b>2、安装支持程序</b></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">原来版本的MySQL源码安装都是采用./configure和make这些。但是新版本的都是用cmake来弄了。所以如果系统中没有的，需要先安装cmake</span>


<div id="kindeditor" class="quote">yum install cmake</div>

<div style="font-size:12px;line-height:18px;"><br></div>
<div style="font-size:12px;line-height:18px;"><b>3、创建MySQL用户和用户组</b></div>



<div id="kindeditor" class="quote">
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">/usr/sbin/groupadd mysql</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">/usr/sbin/useradd -g mysql mysql</span>
</div>

<div style="font-size:12px;line-height:18px;"><br></div>
<div style="font-size:12px;line-height:18px;"><b>4、解压、编译、安装MySQL</b></div>
<div style="font-size:12px;line-height:18px;">解压</div>
<div>
<div></div>
<div id="kindeditor" class="quote">
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">tar zxvf mysql-5.5.16.tar.gz</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">cd mysql-5.5.16</span></div>
</div>
<div></div>
<div style="font-size:12px;line-height:18px;"><br></div>
</div>
<div style="font-size:12px;line-height:18px;">编译参数</div>
<div>
<div></div>
<div id="kindeditor" class="quote">
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">cmake . \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DCMAKE_INSTALL_PREFIX=/usr/local/webserver/mysql5516/ \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DMYSQL_DATADIR=/data0/mysql/3306/data \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DMYSQL_UNIX_ADDR=/data0/mysql/3306/mysqld.sock \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DWITH_MYISAM_STORAGE_ENGINE=1 \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DWITH_INNOBASE_STORAGE_ENGINE=1 \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DWITH_BLACKHOLE_STORAGE_ENGINE=0 \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DWITH_ARCHIVE_STORAGE_ENGINE=0 \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DWITHOUT_PARTITION_STORAGE_ENGINE=1 \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DENABLED_LOCAL_INFILE=1 \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DMYSQL_TCP_PORT=3306 \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DEXTRA_CHARSETS=all \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DDEFAULT_CHARSET=utf8 \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DDEFAULT_COLLATION=utf8_general_ci \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DWITH_DEBUG=0 \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DWITH_EMBEDDED_SERVER=1 \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DWITH_SSL=system \</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">  -DWITH_READLINE=1</span></div>
</div>
<div></div>
<b>注意：编译参数里有几项需要根据自己具体情况设置。我这里将MySQL5.5.16运行于3306端口，并安装在mysql5516目录。</b>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;background-color:#ffead4;">-DCMAKE_INSTALL_PREFIX=/usr/local/webserver/mysql5516/   设置mysql的安装目录</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;background-color:#ffead4;">-DMYSQL_DATADIR=/data0/mysql/3306/data 设置mysql数据库文件的位置</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;background-color:#ffead4;">-DMYSQL_UNIX_ADDR=/data0/mysql/3306/mysqld.sock 设置mysql启动后sock文件位置</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;background-color:#ffead4;">-DMYSQL_TCP_PORT=3306 设置mysql的监听端口</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><b>以上这些设置需要根据不同版本的mysql相应修改。不然多个版本同时监听3306端口，肯定冲突了。</b></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><b><br></b></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">编译</span>


<div id="kindeditor" class="quote">make</div>

<div style="font-size:12px;line-height:18px;">安装</div>
<div style="font-size:12px;line-height:18px;">
<div id="kindeditor" class="quote">make install</div>
</div>
<div style="font-size:12px;line-height:18px;"><br></div>
<b>5、配置目录权限、创建目录、建立初始数据。</b>
配置目录权限


<div id="kindeditor" class="quote">chmod +w /usr/local/webserver/mysql5516
chown -R mysql:mysql /usr/local/webserver/mysql5516
cd ../
</div>

<div>创建相关目录</div>
<div>
<div></div>
<div id="kindeditor" class="quote">
<div>mkdir -p /data0/mysql/3306/data/</div>
<div>mkdir -p /data0/mysql/3306/binlog/</div>
<div>mkdir -p /data0/mysql/3306/relaylog/</div>
<div>chown -R mysql:mysql /data0/mysql/</div>
</div>
<div></div>
</div>
<div>建立初始数据</div>
<div>
<div>
<div id="kindeditor" class="quote">/usr/local/webserver/mysql5516/scripts/mysql_install_db --basedir=/usr/local/webserver/mysql --datadir=/data0/mysql/3306/data --user=mysql</div>
</div>
</div>
<div><br></div>
<b>6、创建My.cnf配置文件和Mysql启动脚本</b>
创建my.cnf配置文件


<div id="kindeditor" class="quote">cp support-files/my-small.cnf /usr/local/webserver/mysql5516/my.cnf</div>

<b>注意：原来mysql默认的配置文件放在/etc/my.cnf。现在多个版本同时运行，需要每个版本都将配置文件单独放在自己的安装目录内。</b>
<b><br></b>
创建MySQL启动脚本


<div id="kindeditor" class="quote">cp support-files/mysql.server /etc/init.d/mysql5516</div>

<b>注意：原来mysql默认采用mysqld脚本，现在每个mysql需要有自己的启动脚本，并通过修改启动脚本来为每个mysql指定采用的配置文件。</b>
<b><br></b>
修改/etc/init.d/mysql5516，用来指定采用的配置文件位置。




``` 
# Try to find basedir in /etc/my.cnf 
```



``` 
 conf=/etc/my.cnf
```

 在文件的213和214行内容如上，我们需要修改conf=/etc/my.cnf为我们指定的my.cnf文件位置。
在我们这里是



``` 
conf=/usr/local/webserver/mysql5516/my.cnf
```


修改mysqld5516权限为可执行

<div id="kindeditor" class="quote">chmod +x mysqld5516</div>


 
<b>7、启动mysql并配置为开机运行</b>
启动

<div id="kindeditor" class="quote">service mysqld5516 start</div>

开机运行

<div id="kindeditor" class="quote">chkconfig mysqld5516 on</div>

<b>以上是MySQL 5.5.16的安装。下面5.5.18版本的安装</b>

<div style="font-size:12px;line-height:18px;"><br></div>
<div style="font-size:12px;line-height:18px;">5.5.18版本的安装基本和MySQL 5.5.16版本一样。我们因为要两个数据库同时运行。所以再给cmake的参数时候，需要安装上面注意的内容设定5.5.18的安装目录为/usr/local/webserver/mysql5518 再调整运行端口为3307</div>
<div style="font-size:12px;line-height:18px;">具体的cmake参数如下</div>
<div style="font-size:12px;line-height:18px;">
<div style="background-color:#ffead4;"></div>
</div>
<div id="kindeditor" class="quote">
<div style="font-size:12px;line-height:18px;">
<div style="background-color:#ffead4;">cmake . \</div>
<div style="background-color:#ffead4;">  -DCMAKE_INSTALL_PREFIX=/usr/local/webserver/mysql5518/ \</div>
<div style="background-color:#ffead4;">  -DMYSQL_DATADIR=/data0/mysql/3307/data \</div>
<div style="background-color:#ffead4;">  -DMYSQL_UNIX_ADDR=/data0/mysql/3307/mysqld.sock \</div>
<div style="background-color:#ffead4;">  -DWITH_MYISAM_STORAGE_ENGINE=1 \</div>
<div style="background-color:#ffead4;">  -DWITH_INNOBASE_STORAGE_ENGINE=1 \</div>
<div style="background-color:#ffead4;">  -DWITH_BLACKHOLE_STORAGE_ENGINE=0 \</div>
<div style="background-color:#ffead4;">  -DWITH_ARCHIVE_STORAGE_ENGINE=0 \</div>
<div style="background-color:#ffead4;">  -DWITHOUT_PARTITION_STORAGE_ENGINE=1 \</div>
<div style="background-color:#ffead4;">  -DENABLED_LOCAL_INFILE=1 \</div>
<div style="background-color:#ffead4;">  -DMYSQL_TCP_PORT=3307 \</div>
<div style="background-color:#ffead4;">  -DEXTRA_CHARSETS=all \</div>
<div style="background-color:#ffead4;">  -DDEFAULT_CHARSET=utf8 \</div>
<div style="background-color:#ffead4;">  -DDEFAULT_COLLATION=utf8_general_ci \</div>
<div style="background-color:#ffead4;">  -DWITH_DEBUG=0 \</div>
<div style="background-color:#ffead4;">  -DWITH_EMBEDDED_SERVER=1 \</div>
<div style="background-color:#ffead4;">  -DWITH_SSL=system \</div>
</div>
<div style="font-size:12px;line-height:18px;"></div>
</div>
<div style="font-size:12px;line-height:18px;"><br></div>
<div style="font-size:12px;line-height:18px;">还需要为mysql5518配置相应权限和建立3307端口的相关目录。因为上面已经有具体步骤，这里只是罗列下命令，不具体说明</div>
<div style="font-size:12px;line-height:18px;">
<div id="kindeditor" class="quote">chmod +w /usr/local/webserver/mysql5518
chown -R mysql:mysql /usr/local/webserver/mysql5518
cd ../
</div>



<div></div>
<div id="kindeditor" class="quote">
<div>mkdir -p /data0/mysql/3307/data/</div>
<div>mkdir -p /data0/mysql/3307/binlog/</div>
<div>mkdir -p /data0/mysql/3307/relaylog/</div>
<div>chown -R mysql:mysql /data0/mysql/</div>
</div>
<div></div>

</div>
<div style="font-size:12px;line-height:18px;">最后的启动脚本也要修改为调用5518目录下的my.cnf配置。 而且my.cnf的[mysqld]段也要指定运行端口为3307</div>
<div style="font-size:12px;line-height:18px;">下面是我mysql5518目录下的my.cnf中的一些需要修改的项</div>
<div style="font-size:12px;line-height:18px;"><b>注意：下面文件并不是完整的my.cnf，只是将需要修改的列出来了</b></div>
<div>
<div></div>
<div id="kindeditor" class="quote">
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">[client]</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">port    = 3307</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">socket  = /data0/mysql/3307/mysql.sock</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">[mysqld]</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">port    = 3307</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">socket  = /data0/mysql/3307/mysql.sock</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">basedir = /usr/local/webserver/mysql5518</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">datadir = /data0/mysql/3307/data</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">log-error = /data0/mysql/3307/mysql_error.log</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">pid-file = /data0/mysql/3307/mysql.pid</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">log-bin = /data0/mysql/3307/binlog/binlog</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">relay-log-index = /data0/mysql/3307/relaylog/relaylog</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">relay-log-info-file = /data0/mysql/3307/relaylog/relaylog</span></div>
<div><span class="Apple-style-span" style="font-size:12px;line-height:18px;">relay-log = /data0/mysql/3307/relaylog/relaylog</span></div>
</div>
<div></div>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">这样弄完后，我服务器上就可以同时运行mysql5516与mysql5518两个版本的数据库了。</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">启动、关闭、重启mysql 5.5.16全部采用service mysqld5516脚本来控制。</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;">启动、关闭、重启mysql 5.5.18全部采用service mysqld5518脚本来控制。</span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><br></span>
<span class="Apple-style-span" style="font-size:12px;line-height:18px;"><b>总结：同时运行多个版本数据库，可以方便我们升级数据库或者对不同版本进行测试。</b></span>
</div>

</div>

<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/post/113">mysqldump解决中文乱码问题</a><a href="http://xinlogs.com/disable-mysql-query-log">开启或者关闭MYSQL的query log(查询日志)</a><a href="http://xinlogs.com/ruby_import_dbf_to_mysql">ruby将DBF文件导入MYSQL库</a><a href="http://xinlogs.com/post/24">Mysql设置my.cnf来记录下全部sql请求</a><a href="http://xinlogs.com/mysql-error-index-is-slower">Mysql错误索引比没索引还慢</a>
</div>
