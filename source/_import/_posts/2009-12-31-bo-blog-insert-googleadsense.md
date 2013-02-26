--- 
categories: []
comments: true
layout: post
title: "bo-blog 2.1.1如何加入google广告条"
---
刚开始使用的bo-blog的时候也不知道如何加入google广告条，经过这几天的咕哝，现在已经成功在右边和下面加入。方法记录如下
<strong>右侧边栏加入google广告</strong>
进入后台管理->常规管理->模块配置
点击左侧的新建/编辑项目 如图
<img alt="" src="http://xinlogs.com/attachment/1262226738_418266e6.png">
然后在手动添加项目中选择侧边模块，确定。如图
<img alt="" src="http://xinlogs.com/attachment/1262226860_83688c76.png">
确认后，进入到新增/编辑项目页面
项目名称--这个是在后台编辑的时候，给自己分辨模块用的，随意命名。我的叫做googleadsense，注意：只能字母和数字
项目描述--随意填写
抬头文字--显示在首页侧边栏上，我的叫做广告栏，根据个人情况写
栏目内容--这里写入google的广告代码就可以了
这个办法最容易，不用修改模板。如果加入后，首页显示google广告栏，里面内容是白的，不要着急，过段时间就可以显示了。我第一个广告大约在30分钟后，才正确显示出来。
<strong>博客底部加入google广告</strong>
底部加入广告，也可以用上面类似的办法，通过加入底部模块实现。但是我尝试了，不好对齐，而且有的模板会破坏底部风格。所以我没有用添加底部模块的办法。
我采用直接修改模板的elements.php文件方法。
到bo-blog的template目录下，找到自己使用的模板文件夹，我用的neowin2，所以对应目录就是neowin2
在模板目录下，打开elements.php文件。找到文件里的displayfooter位置，在displayfooter段里面加入google广告代码。入下所示
<div class="codeText">
<div class="codeHead">
<span class="lantxt">PHP 代码</span><span class="copyCodeText" onclick="copyIdText('code_8194');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_8194">
<ol class="dp-c">
<li class="alt"><span><span class="vars">$elements</span><span>[</span><span class="string">'displayfooter'</span><span>]=<<<eot </span></span></li>
    <li>
<span><div </span><span class="keyword">class</span><span>=</span><span class="string">"clear"</span><span>> </div> </span>
</li>
    <li class="alt"><span></div> </span></li>
    <li>
<span><div </span><span class="keyword">class</span><span>=</span><span class="string">"content-box-b"</span><span>> </span>
</li>
    <li class="alt">
<span><div </span><span class="keyword">class</span><span>=</span><span class="string">"content-box-b-r"</span><span>> </span>
</li>
    <li>
<span><div </span><span class="keyword">class</span><span>=</span><span class="string">"content-box-b-l"</span><span>></div> </span>
</li>
    <li class="alt"><span></div> </span></li>
    <li><span></div> </span></li>
    <li><span>/* 下面为google广告代码 */ </span></li>
    <li class="alt"><span><center> </span></li>
    <li>
<span><script type=</span><span class="string">"text/javascript"</span><span>><!-- </span>
</li>
    <li class="alt">
<span>google_ad_client = </span><span class="string">"pub-2759888315234430"</span><span>; </span>
</li>
    <li>
<span class="comment">/* 728x90, ... 09-12-21 */</span><span> </span>
</li>
    <li class="alt">
<span>google_ad_slot = </span><span class="string">"9531615827"</span><span>; </span>
</li>
    <li><span>google_ad_width = 728; </span></li>
    <li class="alt"><span>google_ad_height = 90; </span></li>
    <li><span class="comment">//--> </span></li>
    <li class="alt"><span></script> </span></li>
    <li>
<span><script type=</span><span class="string">"text/javascript"</span><span> </span>
</li>
    <li class="alt">
<span>src=</span><span class="string">"http://pagead2.googlesyndication.com/pagead/show_ads.js"</span><span>> </span>
</li>
    <li><span></script> </span></li>
    <li class="alt"><span></center> </span></li>
    <li class="alt"><span>/*google广告代码结束*/ </span></li>
    <li>
<span><div id=</span><span class="string">"footer"</span><span>> </span>
</li>
    <li class="alt">
<span><div id=</span><span class="string">"footer-r"</span><span>> </span>
</li>
    <li>
<span><div id=</span><span class="string">"footer-l"</span><span>> </span>
</li>
    <li class="alt">
<span><div id=</span><span class="string">"footer-misc"</span><span>> </span>
</li>
    <li>
<span>Skin By <a href=</span><span class="string">"http://www.neowin.net/"</span><span> target=</span><span class="string">"_blank"</span><span>>Neowin</a><br /> </span>
</li>
    <li class="alt">
<span>Transplanted By <a href=</span><span class="string">"http://blog.meiu.cn/"</span><span> target=</span><span class="string">"_blank"</span><span>>Lingter</a> </span>
</li>
    <li><span></div> </span></li>
    <li class="alt">
<span><div id=</span><span class="string">"footer-copyright"</span><span>> </span>
</li>
    <li><span>{section_foot_components} </span></li>
    <li class="alt">
<span><div id=</span><span class="string">"processtime"</span><span>> </span>
</li>
    <li><span></div> </span></li>
    <li class="alt"><span></div> </span></li>
    <li>
<span><div </span><span class="keyword">class</span><span>=</span><span class="string">"clear"</span><span>></div> </span>
</li>
    <li class="alt"><span></div> </span></li>
    <li><span></div> </span></li>
    <li class="alt"><span></div> </span></li>
    <li><span></div> </span></li>
    <li class="alt"><span>eot; </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
上面的代码就是在博客下面，footer信息的上面，加入一条google广告。效果可以参考<a href="http://www.xinlogs.com">www.xinlogs.com</a>的样子
