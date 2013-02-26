--- 
categories: []
comments: true
layout: post
title: win7安装oracle10g完美解决方案
---
最近由于项目需要，要求安装oracle10g数据库进行开发。在安装的时候碰到一些问题，这里记录下解决办法。
我的系统是win7 中文32位旗舰版。
数据库是oracle10g
<strong>问题1</strong>
在安装的时候，首先是系统检查。因为默认oracle是不支持win7系统的，所以我们需要修改下文件，让它支持。
具体办法：
找到database\stage\prereq\db\refhost.xml文件，在文件里添加如下内容
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_2083');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_2083">
<ol class="dp-xml" style="border-bottom: 0px; border-left: 0px; list-style-type: none; margin-left: 5px; border-top: 0px; border-right: 0px">
<li class="alt"><span><span class="comments"><!--Microsoft Windows 7--></span><span>    </span></span></li>
    <li>
<span class="tag"><</span><span class="tag-name">OPERATING_SYSTEM</span><span class="tag">></span><span>    </span>
</li>
    <li class="alt">
<span class="tag"><</span><span class="tag-name">VERSION</span><span> </span><span class="attribute">VALUE</span><span>=</span><span class="attribute-value">"6.1"</span><span class="tag">/></span><span>    </span>
</li>
    <li>
<span class="tag"></</span><span class="tag-name">OPERATING_SYSTEM</span><span class="tag">></span><span>   </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
<strong>问题2</strong>
上面只是检查系统是否符合要求，这个时候如果运行setup.exe还有可能出现错误提示。这个时候只要我们右键点setup.exe文件，然后选择“兼容性”标签，勾选上“以兼容模式运行这个程序”并在列表里选择“windows xp(service pack 3)”。然后记得安装的时候选择基本安装模式，不要用高级模式。高级模式到后面还是不能通过。
