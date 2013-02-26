--- 
categories: []
comments: true
layout: post
title: playframework--helloworld程序编写
---
本文参考官方<a href="http://www.playframework.org/documentation/1.0/firstapp">http://www.playframework.org/documentation/1.0/firstapp</a>教程，基本上是翻译
转载请注明<a href="http://www.xinlogs.com/">http://www.xinlogs.com</a>
在安装完playframework框架后，我们就可以编写一个helloworld程序来看看playframework框架是如何工作的了。
我是在C:盘的当前用户目录下，建立了一个playframework目录。在命令窗口，进入这个目录
<img alt="" src="http://xinlogs.com/attachment/1262825988_46530e27.png">
在c:usersbaboplayframework目录下输入下面命令，创建项目
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_125');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_125">
<ol class="dp-xml" style="border-bottom: 0px; border-left: 0px; list-style-type: none; margin-left: 5px; border-top: 0px; border-right: 0px">
<li class="alt"><span><span>play new helloworld </span></span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
然后play框架会提示你输入程序的全名称，这里就输入helloworld即可
<img alt="" src="http://xinlogs.com/attachment/1262826264_788423f9.png">
play new helloworld会在当前目录下，创建一个helloworld目录，用来保存项目的全部文件。
下面运行我们新创建的项目，进入项目的目录helloworld，输入play run
<img alt="" src="http://xinlogs.com/attachment/1262826643_88366d2b.png">
当看到入上图提示，证明项目已经跑在9000端口等我们访问了。让我们打开浏览器输入<a href="http://localhost:9000/">http://localhost:9000</a>访问下看看
<img alt="" src="http://xinlogs.com/attachment/1262826899_34879acd.png">
看到上图显示的默认欢迎界面，说明我们的项目已经成功创建。下面我们看看play框架是如何显示出这个页面的
play框架里，你创建项目以后，程序的入口点在 <b>conf/routes</b> 文件。这个文件定义了程序全部可以被访问的URL地址。如果你打开 <b>conf/routes</b> 文件，你会看到第一个‘route’信息就是如下语句<br>
 
<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_6240');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_6240">
<ol class="dp-xml" style="border-bottom: 0px; border-left: 0px; list-style-type: none; margin-left: 5px; border-top: 0px; border-right: 0px">
<li class="alt"><span><span>GET / Application.index </span></span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
以上语句，告诉play框架，当web服务器收到一个访问 / 路径的的GET请求，就调用Application.index的java方法。实际上Application.index方法是<strong>controllers.Application.index</strong>的简写，因为controllers package 已经被import进来。
一个play框架程序的每个URL都会有入口点，我们叫这些方法Action。定义这些方法的类，我们叫做 <b>'controllers'。</b>
下面我们看看 <b>controllers.Application</b> controller。 打开<b>helloworld/app/controllers/Application.java</b> 文件:
<div class="codeText">
<div class="codeHead">
<span class="lantxt">Java 代码</span><span class="copyCodeText" onclick="copyIdText('code_9784');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_9784">
<ol class="dp-j">
<li class="alt"><span><span class="keyword">package</span><span> controllers; </span></span></li>
    <li> </li>
    <li class="alt">
<span class="keyword">import</span><span> play.mvc.*; </span>
</li>
    <li> </li>
    <li class="alt">
<span class="keyword">public</span><span> </span><span class="keyword">class</span><span> Application </span><span class="keyword">extends</span><span> Controller { </span>
</li>
    <li> </li>
    <li class="alt">
<span class="keyword">public</span><span> </span><span class="keyword">static</span><span> </span><span class="keyword">void</span><span> index() { </span>
</li>
    <li><span>render(); </span></li>
    <li class="alt"><span>} </span></li>
    <li> </li>
    <li class="alt"><span>} </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
我们可以看到Application的父类是 <b>play.mvc.Controller</b> 类。这个类为controllers提供了很多有用的方法, 像<b>render() </b>方法，我们用在index action里。
这个index action 被定义为一个 <b>'public static void' </b>方法. action方法是static的, 因为controller类从来都不会有实例. 总是返回void类型.
默认的index action 很简单: 它调用<b>render() </b>方法告诉Play框架去返回一个模板页面.用一个模板生成http响应是最常见的方法(但不是就这一个种方法)。
模板是在 <b>/app/views</b> 目录下的一个简单的text文件. 因为我们没有指定一个模板, 所以这个action将用一个默认的模板: <b>Application/index.html</b>
让我们看下模板文件的内容，打开<strong>helloworld/app/views/Application/index.html</strong> 文件:
<div class="codeText">
<div class="codeHead">
<span class="lantxt">XML/HTML 代码</span><span class="copyCodeText" onclick="copyIdText('code_8842');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_8842">
<ol class="dp-xml">
<li class="alt"><span><span>#{extends 'main.html' /} </span></span></li>
    <li><span>#{set title:'Home' /} </span></li>
    <li class="alt"> </li>
    <li><span>#{welcome /} </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
模板的内容看起来很简洁。实际上你看到的全是Play标签。它常接近JSP标签。上面的 <b>#{welcome /}</b>标签，就是生成我们在浏览器看到的默认欢迎页面。
这个<strong>#{extends /}</strong> 标签告诉Play框架这个模板继承于另外一个叫 <b>main.html</b>的模板文件。模板继承是一个很棒的概念，在用于创建复杂网页的共用部分重用上。
打开 <b>helloworld/app/views/main.html</b> 模板:
 
<div class="codeText">
<div class="codeHead">
<span class="lantxt">XML/HTML 代码</span><span class="copyCodeText" onclick="copyIdText('code_8740');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_8740">
<ol class="dp-xml">
<li class="alt"><span><span><!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"</span><span class="tag">></span><span> </span></span></li>
    <li> </li>
    <li class="alt">
<span class="tag"><</span><span class="tag-name">html</span><span> </span><span class="attribute">xmlns</span><span>=</span><span class="attribute-value">"http://www.w3.org/1999/xhtml"</span><span> </span><span class="attribute">xml:lang</span><span>=</span><span class="attribute-value">"en"</span><span> </span><span class="attribute">lang</span><span>=</span><span class="attribute-value">"en"</span><span class="tag">></span><span> </span>
</li>
    <li>
<span class="tag"><</span><span class="tag-name">head</span><span class="tag">></span><span> </span>
</li>
    <li class="alt">
<span class="tag"><</span><span class="tag-name">title</span><span class="tag">></span><span>#{get 'title' /}</span><span class="tag"></</span><span class="tag-name">title</span><span class="tag">></span><span> </span>
</li>
    <li>
<span class="tag"><</span><span class="tag-name">meta</span><span> </span><span class="attribute">http-equiv</span><span>=</span><span class="attribute-value">"Content-Type"</span><span> </span><span class="attribute">content</span><span>=</span><span class="attribute-value">"text/html; charset=utf-8"</span><span class="tag">/></span><span> </span>
</li>
    <li class="alt">
<span class="tag"><</span><span class="tag-name">link</span><span> </span><span class="attribute">rel</span><span>=</span><span class="attribute-value">"stylesheet"</span><span> </span><span class="attribute">type</span><span>=</span><span class="attribute-value">"text/css"</span><span> </span><span class="attribute">media</span><span>=</span><span class="attribute-value">"screen"</span><span> </span>
</li>
    <li>
<span class="attribute">href</span><span>=</span><span class="attribute-value">"@{'/public/stylesheets/main.css'}"</span><span> </span><span class="tag">/></span><span> </span>
</li>
    <li class="alt">
<span class="tag"><</span><span class="tag-name">link</span><span> </span><span class="attribute">rel</span><span>=</span><span class="attribute-value">"shortcut icon"</span><span> </span><span class="attribute">type</span><span>=</span><span class="attribute-value">"image/png"</span><span> </span>
</li>
    <li>
<span class="attribute">href</span><span>=</span><span class="attribute-value">"@{'/public/images/favicon.png'}"</span><span> </span><span class="tag">/></span><span> </span>
</li>
    <li class="alt">
<span class="tag"></</span><span class="tag-name">head</span><span class="tag">></span><span> </span>
</li>
    <li>
<span class="tag"><</span><span class="tag-name">body</span><span class="tag">></span><span> </span>
</li>
    <li class="alt"><span>#{doLayout /} </span></li>
    <li>
<span class="tag"></</span><span class="tag-name">body</span><span class="tag">></span><span> </span>
</li>
    <li class="alt">
<span class="tag"></</span><span class="tag-name">html</span><span class="tag">></span><span> </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
看到<strong>#{doLayout /}</strong> 标签了吧，它就是<strong>Application/index.html</strong> 文件内容被插入的位置
<strong>创建一个表单</strong>
下面我们将从一个可以输入我们名字的表单来开始写我们的程序。
编辑 <b>helloworld/app/views/Application/index.html</b> 模板，如下所示
<div class="codeText">
<div class="codeHead">
<span class="lantxt">XML/HTML 代码</span><span class="copyCodeText" onclick="copyIdText('code_8433');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_8433">
<ol class="dp-xml">
<li class="alt"><span><span>#{extends 'main.html' /} </span></span></li>
    <li><span>#{set title:'Home' /} </span></li>
    <li class="alt"> </li>
    <li>
<span class="tag"><</span><span class="tag-name">form</span><span> </span><span class="attribute">action</span><span>=</span><span class="attribute-value">"@{Application.sayHello()}"</span><span> </span><span class="attribute">method</span><span>=</span><span class="attribute-value">"GET"</span><span class="tag">></span><span> </span>
</li>
    <li class="alt">
<span class="tag"><</span><span class="tag-name">input</span><span> </span><span class="attribute">type</span><span>=</span><span class="attribute-value">"text"</span><span> </span><span class="attribute">name</span><span>=</span><span class="attribute-value">"myName"</span><span> </span><span class="tag">/></span><span> </span>
</li>
    <li>
<span class="tag"><</span><span class="tag-name">input</span><span> </span><span class="attribute">type</span><span>=</span><span class="attribute-value">"submit"</span><span> </span><span class="attribute">value</span><span>=</span><span class="attribute-value">"Say hello!"</span><span> </span><span class="tag">/></span><span> </span>
</li>
    <li class="alt">
<span class="tag"></</span><span class="tag-name">form</span><span class="tag">></span><span> </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
我们使用@{...} 符号让Play框架自动生成可以调用Application.sayHello action的URL。现在，刷新下我们的浏览器。
<img alt="" src="http://xinlogs.com/attachment/1262833657_60355058.png">
我们得到了一个错误，因为我们引用的 Application.sayHello action不存在。让我们在helloworld/app/controllers/Application.java文件里创建它。
<div class="codeText">
<div class="codeHead">
<span class="lantxt">Java 代码</span><span class="copyCodeText" onclick="copyIdText('code_2733');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_2733">
<ol class="dp-j">
<li class="alt"><span><span class="keyword">package</span><span> controllers; </span></span></li>
    <li> </li>
    <li class="alt">
<span class="keyword">import</span><span> play.mvc.*; </span>
</li>
    <li> </li>
    <li class="alt">
<span class="keyword">public</span><span> </span><span class="keyword">class</span><span> Application </span><span class="keyword">extends</span><span> Controller { </span>
</li>
    <li> </li>
    <li class="alt">
<span class="keyword">public</span><span> </span><span class="keyword">static</span><span> </span><span class="keyword">void</span><span> index() { </span>
</li>
    <li><span>render(); </span></li>
    <li class="alt"><span>} </span></li>
    <li> </li>
    <li class="alt">
<span class="keyword">public</span><span> </span><span class="keyword">static</span><span> </span><span class="keyword">void</span><span> sayHello(String myName) { </span>
</li>
    <li><span>render(myName); </span></li>
    <li class="alt"><span>} </span></li>
    <li> </li>
    <li class="alt"><span>} </span></li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
我们在action方法里声明了<strong>myName</strong>参数，它将被自动填充上来自表单提交的HTTP <strong>myName</strong> 参数。
现在我们在访问<a href="http://localhost:9000">http://localhost:9000</a>显示如下
<img alt="" src="http://xinlogs.com/attachment/1262842031_8753f4d3.png">
在文本框输入你的名字，然后提交，你会发现一个新的错误。
<img alt="" src="http://xinlogs.com/attachment/1262842309_690736db.png">
这个错误显而易见，Play框架调用sayHello action方法的默认模板，但是这个模板不存在。让我们创建<strong>helloworld/app/views/Application/sayHello.html</strong>文件
<div class="codeText">
<div class="codeHead">
<span class="lantxt">XML/HTML 代码</span><span class="copyCodeText" onclick="copyIdText('code_3443');" style="cursor: pointer">复制内容到剪贴板</span>
</div>
<div id="code_3443">
<ol class="dp-xml">
<li class="alt"><span><span>#{extends 'main.html' /} </span></span></li>
    <li><span>#{set title:'Home' /} </span></li>
    <li class="alt"> </li>
    <li>
<span class="tag"><</span><span class="tag-name">h1</span><span class="tag">></span><span>Hello ${myName ?: 'guest'}!</span><span class="tag"></</span><span class="tag-name">h1</span><span class="tag">></span><span> </span>
</li>
    <li class="alt"> </li>
    <li>
<span class="tag"><</span><span class="tag-name">a</span><span> </span><span class="attribute">href</span><span>=</span><span class="attribute-value">"@{Application.index()}"</span><span class="tag">></span><span>Back to form</span><span class="tag"></</span><span class="tag-name">a</span><span class="tag">></span><span> </span>
</li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
然后刷新浏览器页面，显示如下
<img alt="" src="http://xinlogs.com/attachment/1262842580_8493f286.png">
我们看到了sayHello.html文件里使用了groovy的<strong>'?:'</strong>操作符。当我们<strong>myName</strong>没有没有被填充的时候，它默认用'guest'填充myName。
到这里，我们的HelloWorld程序已经可以正常运行了。但是我们还有几个问题需要解决。
如何显示更好的URL，这个对于SEO还是比较重要的。
如何自定义LayOut
如何对输入的字符进行验证
如何自动测试。
后面我们慢慢实现。
 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/playframework-video">playframework--开发视频</a><a href="http://xinlogs.com/playframework">playframework--play！java on rails框架</a><a href="http://xinlogs.com/playframework-installation-guide">playframework--Installation guide 安装手册【翻译】</a>
</div>
