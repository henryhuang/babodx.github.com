--- 
categories: []
comments: true
layout: post
title: ruby将DBF文件导入MYSQL库
---
单位招生项目，需要向MySQL灌入一些数据测试。因为原始数据都是DBF文件，就是foxpro数据库。
下面是用ruby代码实现的导入功能。
<div class="codeText">
<div class="codeHead">
<span class="lantxt">Ruby 代码</span><span class="copyCodeText" onclick="copyIdText('code_5393');" style="cursor:pointer;">复制内容到剪贴板</span>
</div>
<div id="code_5393">
<ol class="dp-rb" style="border-bottom:0px;border-left:0px;list-style-type:none;margin-left:5px;border-top:0px;border-right:0px;">
<li class="alt"><span><span>require </span><span class="string">'rubygems'</span><span>  </span></span></li>
    <li>
<span>require </span><span class="string">'dbf'</span><span>  </span>
</li>
    <li class="alt">
<span>require </span><span class="string">'mysql2'</span><span>  </span>
</li>
    <li>
<span>require </span><span class="string">'iconv'</span><span>  </span>
</li>
    <li class="alt">
<span class="variable">@the_year</span><span>=</span><span class="string">"2011"</span><span>  </span>
</li>
    <li>
<span class="variable">@start_time</span><span>=</span><span class="builtin">Time</span><span>.now   </span>
</li>
    <li class="alt">
<span class="variable">@major_list</span><span>=[]   </span>
</li>
    <li>
<span>client = Mysql2::Client.</span><span class="keyword">new</span><span>(</span><span class="symbol">:host</span><span> => </span><span class="string">"localhost"</span><span>, </span><span class="symbol">:username</span><span> => </span><span class="string">"root"</span><span>,</span><span class="symbol">:password</span><span>=></span><span class="string">"dxzddp"</span><span>,</span><span class="symbol">:database</span><span>=></span><span class="string">"enroll"</span><span>)</span><span class="comment">#创建数据库连接 </span><span>  </span>
</li>
    <li class="alt">
<span>results = client.query(</span><span class="string">"SELECT major_id FROM major WHERE the_year='#{@the_year}'"</span><span>, </span><span class="symbol">:as</span><span> => </span><span class="symbol">:array</span><span>)   </span>
</li>
    <li>
<span>results.</span><span class="keyword">each</span><span> </span><span class="keyword">do</span><span> |row|   </span>
</li>
    <li class="alt">
<span>  </span><span class="variable">@major_list</span><span>.push(row[0])   </span>
</li>
    <li>
<span class="keyword">end</span><span>  </span>
</li>
    <li class="alt">
<span>p </span><span class="variable">@major_list</span><span>  </span>
</li>
    <li>
<span>table = DBF::Table.</span><span class="keyword">new</span><span>(</span><span class="string">"d:/ss.dbf"</span><span>)   </span>
</li>
    <li class="alt">
<span>  table.</span><span class="keyword">each</span><span> </span><span class="keyword">do</span><span> |record|   </span>
</li>
    <li>
<span>      </span><span class="variable">@sfzh</span><span>=record.sfzh   </span>
</li>
    <li class="alt">
<span>      </span><span class="variable">@name</span><span>=Iconv.iconv(</span><span class="string">"utf-8"</span><span>, </span><span class="string">"gb18030"</span><span>, record.xm)   </span>
</li>
    <li>
<span>      </span><span class="variable">@sfzh</span><span>=record.sfzh   </span>
</li>
    <li class="alt">
<span>      </span><span class="variable">@ksh</span><span>=record.ksh   </span>
</li>
    <li>
<span>      </span><span class="variable">@major</span><span>=</span><span class="variable">@major_list</span><span>.choice   </span>
</li>
    <li class="alt">
<span>      </span><span class="variable">@major2</span><span>=</span><span class="variable">@major_list</span><span>.choice   </span>
</li>
    <li>
<span>      </span><span class="variable">@major2</span><span>=</span><span class="variable">@major_list</span><span>.choice </span><span class="keyword">while</span><span> </span><span class="variable">@major2</span><span>==</span><span class="variable">@major</span><span>  </span>
</li>
    <li class="alt">
<span>      </span><span class="variable">@to_student_info</span><span>=</span><span class="string">"insert into student_info (student_no,access_pwd,the_year) VALUES ('#{@sfzh}','e10adc3949ba59abbe56e057f20f883e','#{@the_year}')"</span><span>  </span>
</li>
    <li>
<span>      </span><span class="variable">@to_candidate</span><span>=</span><span class="string">"insert into candidate (candidate_name,id_card_no,candidate_no,state,has_submitted,the_year) VALUES ('#{@name}','#{@sfzh}','#{@ksh}','A','Y','#{@the_year}')"</span><span>  </span>
</li>
    <li class="alt">
<span>      </span><span class="variable">@to_candidate_ideal</span><span>=</span><span class="string">"insert into candidate_ideal (candidate_id,first_major,second_major,allow_adjustment,is_living_in_college) VALUES (last_insert_id(),'#{@major}','#{@major2}','Y','Y')"</span><span>  </span>
</li>
    <li>
<span>      </span><span class="variable">@to_candidate_score</span><span>=</span><span class="string">"insert into candidate_score (candidate_id) VALUES (last_insert_id())"</span><span>  </span>
</li>
    <li class="alt">
<span>      </span><span class="variable">@to_contact_info</span><span>=</span><span class="string">"insert into contact_info (candidate_id) VALUES (last_insert_id())"</span><span>  </span>
</li>
    <li>
<span>      </span><span class="variable">@to_school_info</span><span>=</span><span class="string">"insert into school_info (candidate_id) VALUES (last_insert_id())"</span><span>  </span>
</li>
    <li class="alt">
<span>      </span><span class="keyword">begin</span><span>  </span>
</li>
    <li>
<span>      client.query(</span><span class="variable">@to_student_info</span><span>)   </span>
</li>
    <li class="alt">
<span>      client.query(</span><span class="variable">@to_candidate</span><span>)   </span>
</li>
    <li>
<span>      client.query(</span><span class="variable">@to_candidate_ideal</span><span>)   </span>
</li>
    <li class="alt">
<span>      client.query(</span><span class="variable">@to_candidate_score</span><span>)   </span>
</li>
    <li>
<span>      client.query(</span><span class="variable">@to_contact_info</span><span>)   </span>
</li>
    <li class="alt">
<span>      client.query(</span><span class="variable">@to_school_info</span><span>)   </span>
</li>
    <li>
<span>      </span><span class="keyword">rescue</span><span> Mysql2::Error=>e   </span>
</li>
    <li class="alt"><span>          puts e.to_s   </span></li>
    <li>
<span>  </span><span class="keyword">end</span><span>  </span>
</li>
    <li class="alt">
<span class="keyword">end</span><span>  </span>
</li>
    <li>
<span>client=</span><span class="keyword">nil</span><span> </span><span class="keyword">if</span><span> client   </span>
</li>
    <li class="alt">
<span>p </span><span class="builtin">Time</span><span>.now-</span><span class="variable">@start_time</span><span>  </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
大体思路就是
1、建立数据库连接
2、查询招生专业major
3、打开dbf文件
4、每读取一行，想Mysql数据库写入一行，随机生成报考专业
5、关闭数据库连接
6、打印出用时<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/post/22">一个在windows下很好用的MySQL工具Navicat</a><a href="http://xinlogs.com/HeidiSQL">HeidiSQL一款开源的MySQL图形管理软件</a><a href="http://xinlogs.com/bo-blog-to-emlog">微博从bo-blog迁移到了emlog。</a><a href="http://xinlogs.com/post/112">Mysql设置不区分表名大小写</a><a href="http://xinlogs.com/bbsxp6-to-phpwind8">bbsxp 6.0数据导入phpwind 8.7方法</a>
</div>
