--- 
categories: []
comments: true
layout: post
title: emlog支付插件发布
---
今天看到支付宝可以采用http://me.alipay.com/用户名的形式创建自己的收款页面。以前要想写支付宝的付款插件，需要先申请一个商家用户，拿到分配你的密钥才可以。现在可以通过创建收款主页来直接收款，对于程序开发方便了很多（不过这个方式只能简单的收款，并不能用于商品的订单，没有回调函数不能知道是否收款成功等内容）。
而且看到wordpress貌似已经有了这样的插件可以用来发布一个收款按钮，于是我也心血来潮写了一个。主要还是为了学习如何写一个emlog的插件。看着官方的插件开发文档，参考着公告插件的代码来写的这个支付宝收款插件。
 
<b>主要功能：</b>
1、在博客的侧边栏显示出一个支付宝的收款按钮，基本也就是用来让人家自愿捐款用。如下图：
<a target="_blank" href="/content/uploadfile/201111/dc52c595a54d30a8c75037440df4329320111120123853.jpg" id="ematt:74"><img src="/content/uploadfile/201111/thum-dc52c595a54d30a8c75037440df4329320111120123853.jpg" width="420" height="117" title="点击查看原图" alt="点击查看原图" border="0"></a>
 
2、后台可以方便的设置用来收款的支付宝收款主页地址。
<a target="_blank" href="/content/uploadfile/201111/a893fefaf93a1b8b2ed0a350417b7d6120111120124028.jpg" id="ematt:75"><img src="/content/uploadfile/201111/thum-a893fefaf93a1b8b2ed0a350417b7d6120111120124028.jpg" width="420" height="155" title="点击查看原图" alt="点击查看原图" border="0"></a>
 
 
 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/bo-blog-to-emlog">微博从bo-blog迁移到了emlog。</a><a href="http://xinlogs.com/mod-template-emidream">修改EMiDream模板</a><a href="http://xinlogs.com/emlog-optimize-via-minify">emlog访问速度优化之minify整合css与js文件</a>
</div>
