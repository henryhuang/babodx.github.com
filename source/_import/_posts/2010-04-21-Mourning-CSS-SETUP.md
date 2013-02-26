--- 
categories: []
comments: true
layout: post
title: 哀悼日网站和bo-blog变灰色设置
---
根据国务院文件，5.19-5.21为全国哀悼日，在此期间，全国和各驻外机构下半旗志哀，停止公共娱乐活动，外交部和我国驻外使领馆设立吊唁簿。5月19日14时28分起，全国人民默哀3分钟，届时汽车、火车、舰船鸣笛，防空警报鸣响。 Admin5与很多草根网站都将整站换成素装。并建议中国所有站点更换为素装。<br>
为方便站点哀悼，特提供css滤镜代码，以表哀悼。以下为全站CSS代码。
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_9157');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_9157">
<ol class="dp-css" style="border-bottom: 0px; border-left: 0px; list-style-type: none; margin-left: 5px; border-top: 0px; border-right: 0px">
<li class="alt"><span><span>html { filter:progid:DXImageTransform.Microsoft.BasicImage(grayscale=1); }  </span></span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
使用方法：这段代码可以变网页为黑白，将代码加到CSS最顶端就可以实现素装。建议全国站长动起来。为在地震中遇难的同胞哀悼。
如果网站没有使用CSS，可以在网页/模板的HTML代码<head>和</head> 之间插入：
<div class="codeText">
<div class="codeHead">
<span class="lantxt">XML/HTML 代码</span><span class="copyCodeText" onclick="copyIdText('code_9247');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_9247">
<ol class="dp-xml">
<li class="alt"><span><span class="tag"><</span><span class="tag-name">style</span><span class="tag">></span><span>  </span></span></li>
    <li>
<span>html{filter:progid:DXImageTransform.Microsoft.BasicImage(</span><span class="attribute">grayscale</span><span>=</span><span class="attribute-value">1</span><span>);}   </span>
</li>
    <li class="alt">
<span class="tag"></</span><span class="tag-name">style</span><span class="tag">></span><span>  </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
有一些站长的网站可能使用这个css 不能生效，是因为网站没有使用最新的网页标准协议
<div class="codeText">
<div class="codeHead">
<span class="lantxt">XML/HTML 代码</span><span class="copyCodeText" onclick="copyIdText('code_7852');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_7852">
<ol class="dp-xml">
<li class="alt"><span><span><!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"</span><span class="tag">></span><span>  </span></span></li>
    <li>
<span class="tag"><</span><span class="tag-name">html</span><span> </span><span class="attribute">xmlns</span><span>=</span><span class="attribute-value">"http://www.w3.org/1999/xhtml"</span><span class="tag">></span><span>  </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
请将网页最头部的<html>替换为以上代码。
有一些网站FLASH动画的颜色不能被CSS滤镜控制，可以在FLASH代码的<object …>和</object>之间插入：
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_3112');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_3112">
<ol class="dp-c">
<li class="alt"><span><span><param value=</span><span class="string">"false"</span><span> name=</span><span class="string">"menu"</span><span>/>   </span></span></li>
    <li>
<span><param value=</span><span class="string">"opaque"</span><span> name=</span><span class="string">"wmode"</span><span>/>  </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
最简单的把页面变成灰色的代码是在head 之间加<br>
 
<div class="codeText">
<div class="codeHead">
<span class="lantxt">XML/HTML 代码</span><span class="copyCodeText" onclick="copyIdText('code_7466');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_7466">
<ol class="dp-xml">
<li class="alt"><span><span class="tag"><</span><span class="tag-name">style</span><span> </span><span class="attribute">type</span><span>=</span><span class="attribute-value">"text/css"</span><span class="tag">></span><span>  </span></span></li>
    <li><span>  </span></li>
    <li class="alt"><span>html {   </span></li>
    <li><span>    FILTER: gray   </span></li>
    <li class="alt"><span>}   </span></li>
    <li>
<span class="tag"></</span><span class="tag-name">style</span><span class="tag">></span><span>  </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
下面再看看如何给上面这些应用到bo-blog的页面中呢。
首先我们到bo-blog的根目录找到我们的template模板目录，在其中找到我们使用的模板所采用的CSS文件。在文件最后加入
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_7962');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_7962">
<ol class="dp-xml">
<li class="alt"><span><span>html { filter:progid:DXImageTransform.Microsoft.BasicImage(</span><span class="attribute">grayscale</span><span>=</span><span class="attribute-value">1</span><span>); }    </span></span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
另在哀悼日或遇难的新闻，所有专题和主题 图片上不能使用红色标题。
