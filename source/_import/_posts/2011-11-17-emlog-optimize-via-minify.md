--- 
categories: []
comments: true
layout: post
title: emlog访问速度优化之minify整合css与js文件
---
我们知道网站的访问速度和很多因素相关，但是今天这里只是优化我们能力范围内的首页文件加载数量与大小。
如果首页文件很大，肯定加载慢。还有一个因素就是首页如果加载很多css和js文件，因为在加载过程中会建立很多次连接从而降低访问首页的速度。
下面是我博客默认情况下的加载css和js文件的数量
<a target="_blank" href="/content/uploadfile/201111/608b080a399c83bbd679f8afef089cfc20111117063223.jpg" id="ematt:68"><img src="/content/uploadfile/201111/thum-608b080a399c83bbd679f8afef089cfc20111117063223.jpg" width="420" height="43" title="点击查看原图" alt="点击查看原图" border="0"></a>
<a target="_blank" href="/content/uploadfile/201111/f6893b6f67d76ff8c173d31adb40201f20111117063249.jpg" id="ematt:69"><img src="/content/uploadfile/201111/thum-f6893b6f67d76ff8c173d31adb40201f20111117063249.jpg" alt="点击查看原图" border="0"></a>
从图上可以看到加载了4个css文件和5个js文件。大小分别为54.2kb和45.5kb。
 
下面我们的主要工具出场：<b>minify</b>
这个程序是纯php编写的开源程序，用来将我们网站的css和js文件进行整合与压缩。这样我们就可以通过整合css与js文件并压缩大小从而提高首页加载速度。
 
首先将minify下载，我这里采用的是minify_2.1.3版本。将下载后的min目录放到网站的根目录。
接着我们改造我们的程序，让css与js文件通过minify程序合并压缩。
 
我是自己写了一个类，我起名为merge.php用来调用minify整合。下面是类代码



``` 
<?php
/**
 * JS和CSS文件合并类
 *
 * @copyright (c) xinlogs.com All Rights Reserved
 * $Id: merge.php 2011-11-16 09:54:58Z  xinlogs.com$
 */

class Merge {
  //定义两个数组用来存储全部css和js文件的位置
  static private $css_list=array();
  static private $js_list=array();
  static private $js_code=array();
  //定义addCssFile方法，用来添加css文件到数组
  static function addCssFile($addFilePath) {
    array_push(self::$css_list,$addFilePath);
  }
  //定义getCssList方法，用来返回css文件字符串
  static function getCssList(){
    return implode(",",self::$css_list);
  }
  //定义addJsFile方法，用来添加js文件到数组
  static function addJsFile($addFilePath) {
    array_push(self::$js_list,$addFilePath);
  }
  //定义getJsList用来返回js文件字符串
  static function getJsList(){
    return implode(",",self::$js_list);
  }
  //加入js代码
  static function addJsCode($addJsCode)
  {
    array_push(self::$js_code,$addJsCode);
  }

  static function includejscode(){
    return implode(" ",self::$js_code);
  }
  //用来返回加入CSS代码的语句
  static function includecss() {
    $css="<link href=".BLOG_URL."min/?f=".Merge::getCssList()." rel=\"stylesheet\" type=\"text/css\" />";
    return $css;
  }
  //用来返回插件js代码的语句
  static function includejs() {
    $js="<script src=".BLOG_URL."min/?f=".Merge::getJsList()." type=\"text/javascript\"></script>";
    return $js;
  }
}
```

将merge.php放到网站include/lib目录下，emlog就会自动加载这个类了。
 
接着我们修改我们的模板与插件，让所有css与js文件通过Merge这个类来处理。
一般css与js文件都是在模板的header.php文件内加载的，这里以默认模板为例说明
我们打开header.php文件，然后修改css文件加载的语句。将原来加载css的语句，采用Merge::addcssfile方法代替


<div id="kindeditor" class="quote">Merge::addCssFile("content/templates/default/main.css");</div>

<div>最后在doAction后面统一输出我们合并后的css文件。</div>
<div>
<div></div>
<div id="kindeditor" class="quote">
<div><?php doAction('index_head'); ?></div>
<div><?php echo Merge::includecss();?></div>
</div>
<div></div>
</div>
<div><br></div>

我们再来处理js文件，我是将模板的js文件调整到了fooder.php里加载，下面以这个footer.php为例说明
首先将加载js文件的语句，替换为Merge::addJsFile方法

<div id="kindeditor" class="quote"><?php Merge::addJsFile("include/lib/js/common_tpl.js");?></div>

然后在doAction后面再统一调用合并后的js文件

<div id="kindeditor" class="quote"><?php doAction('index_footer'); ?>

<?php echo Merge::includejs();?>
<?php echo Merge::includejscode();?>

</div>


 
以上只是针对模板的修改，我的这个博客还采用了高亮代码的插件，这个插件会调用一些js与css文件。所以下面我还修改这个插件的代码，使它也通过merge类来合并输出css与js。
这个插件主要修改drp_highlighter.php文件，在<span class="Apple-style-span" style="font-size:12px;line-height:18px;background-color:#ffead4;">drp_highlighter_index_head方法里加入css的引入</span>


<div id="kindeditor" class="quote">function drp_highlighter_index_head() {
  Merge::addCssFile("content/plugins/drp_highlighter/css/editor.css");
}
</div>

 
在<span class="Apple-style-span" style="font-size:12px;line-height:18px;background-color:#ffead4;">drp_highlighter_index_footer方法里加入对js文件与代码的引入。</span>


<div id="kindeditor" class="quote">function drp_highlighter_index_footer(){
        include(EMLOG_ROOT.'/content/plugins/drp_highlighter/config.php');
        Merge::addJsFile("content/plugins/drp_highlighter/js/jquery.js");
        Merge::addJsFile("content/plugins/drp_highlighter/js/jquery.highlighter.src.js");
        $jscode="
<script type='text/javascript'>
$(document).ready(function(){
        var option={
                dir:'".BLOG_URL."content/plugins/drp_highlighter/js/syntaxhighlighter/',
                name:'code',
                showControls:true,
                skin:'".constant('highlighter')."'
        };
        $.SyntaxHighlighter(option);
});
</script>";
        Merge::addJsCode($jscode);
}
</div>

 
最后在文件最后面找到注册action的代码，和上面方法名修改对应上。


<div id="kindeditor" class="quote">addAction('adm_sidebar_ext', 'drp_highlighter_adm_sidebar');
addAction('index_head','drp_highlighter_index_head');
addAction('index_footer', 'drp_highlighter_index_footer');
addAction('adm_writelog_head','drp_highlighter_adm_head');
</div>




 
下面是我整合css与js代码后的效果
<a target="_blank" href="/content/uploadfile/201111/8f685fc5bfcc86915423e98a6c8132a420111117065631.jpg" id="ematt:71"><img src="/content/uploadfile/201111/thum-8f685fc5bfcc86915423e98a6c8132a420111117065631.jpg" width="420" height="42" title="点击查看原图" alt="点击查看原图" border="0"></a>
css文件降低为3个，其中有两个为新浪微博的代码，这个不好整合了。。。
<a target="_blank" href="/content/uploadfile/201111/e552b0dde56885bff97f4addbf7bb34720111117065803.jpg" id="ematt:72"><img src="/content/uploadfile/201111/thum-e552b0dde56885bff97f4addbf7bb34720111117065803.jpg" width="420" height="43" title="点击查看原图" alt="点击查看原图" border="0"></a>
js文件数降低为3个，其中一个是微博的，一个是百度统计的。我们自己的代码已经整合到一个请求里调用了。
 
 

 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/emlog-alipay-pulgin">emlog支付插件发布</a><a href="http://xinlogs.com/bo-blog-to-emlog">微博从bo-blog迁移到了emlog。</a><a href="http://xinlogs.com/mod-template-emidream">修改EMiDream模板</a>
</div>
