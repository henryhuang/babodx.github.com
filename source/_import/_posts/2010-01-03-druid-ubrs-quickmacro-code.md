--- 
categories: []
comments: true
layout: post
title: 野D带黑上，后台挂机按键精灵代码
---
本文章原创，转载请注明出处 鑫的方向 <a href="http://www.xinlogs.com">http://www.xinlogs.com</a>
前面写了一篇<a href="http://www.xinlogs.com/post/60/">德鲁伊黑上带刷详细图文攻略 1.0版本</a>，很多朋友想要带刷时候使用的后台代码。
为了帮人帮到底，和小站的人气。这里放出代码。
代码是运行在按键精灵里的。在winxp+按键精灵7和win7+按键精灵7下测试通过
如果转载，请注明 鑫的方向 <a href="http://www.xinlogs.com/">http://www.xinlogs.com</a>

<div class="codeText">
<span class="copyCodeText" onclick="copyIdText('code_5539');" style="cursor: pointer">复制内容到剪贴板</span>
<div id="code_5539">
<ol class="dp-vb">
<li class="alt"><span><span>Plugin hwnd=Window.GetKeyFocusWnd() </span></span></li>
    <li><span>Rem start </span></li>
    <li class="alt">
<span class="keyword">For</span><span> 30 </span>
</li>
    <li><span>Plugin Bkgnd.KeyPress(hwnd,70) </span></li>
    <li class="alt"><span>Delay 300 </span></li>
    <li><span>EndFor </span></li>
    <li class="alt"><span>Delay 1000 </span></li>
    <li><span>Plugin Bkgnd.KeyDown(hwnd,16) </span></li>
    <li class="alt"><span>Plugin Bkgnd.KeyPress(hwnd,87) </span></li>
    <li><span>Plugin Bkgnd.KeyUp(hwnd,16) </span></li>
    <li class="alt"><span>Delay 500 </span></li>
    <li>
<span class="keyword">For</span><span> 30 </span>
</li>
    <li class="alt"><span>Plugin Bkgnd.KeyPress(hwnd,70) </span></li>
    <li><span>Delay 300 </span></li>
    <li class="alt"><span>EndFor </span></li>
    <li><span>Delay 1000 </span></li>
    <li class="alt"><span>Plugin Bkgnd.KeyDown(hwnd,18) </span></li>
    <li><span>Plugin Bkgnd.KeyPress(hwnd,68) </span></li>
    <li class="alt"><span>Plugin Bkgnd.KeyUp(hwnd,18) </span></li>
    <li><span>Delay 500 </span></li>
    <li class="alt">
<span class="keyword">For</span><span> 30 </span>
</li>
    <li><span>Plugin Bkgnd.KeyPress(hwnd,70) </span></li>
    <li class="alt"><span>Delay 300 </span></li>
    <li><span>EndFor </span></li>
    <li class="alt"><span>Delay 1000 </span></li>
    <li>
<span class="keyword">For</span><span> 8 </span>
</li>
    <li class="alt"><span>Plugin Bkgnd.KeyDown(hwnd,16) </span></li>
    <li><span>Plugin Bkgnd.KeyPress(hwnd,82) </span></li>
    <li class="alt"><span>Plugin Bkgnd.KeyUp(hwnd,16) </span></li>
    <li><span>EndFor </span></li>
    <li class="alt"><span>Delay 500 </span></li>
    <li><span>Goto start </span></li>
    <li class="alt"> </li>
</ol>
</div>
<link rel="stylesheet" type="text/css" href="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/insertcode.css">
<script language="javascript" src="http://www.xinlogs.com/editor/fckeditor/editor/plugins/insertcode/excute.js" type="text/javascript"></script>
</div>
<strong>代码说明：</strong>
上面是我自己使用的代码，我的快捷键设置和大家不一样，所以别人使用，请注意修改快捷键。
精灵之火（野性） Shift+R
树皮 Shift+W
狂暴回复 Alt+D
横扫+重殴 宏 F （其实就是个AOE宏，需要不断切换目标，保证重殴可以放出来就可以）
以上就是我带刷的代码，基本保证装备不红，人物不死。
 <div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/post/60">德鲁伊黑上带刷详细图文攻略 1.0版本</a><a href="http://xinlogs.com/wow-1-70-Express-Upgrade">国服3.13 德鲁伊带小号快速升级（1-70全程带）</a><a href="http://xinlogs.com/wow-on-ubuntu-linux">ubuntu8.04 玩魔兽世界 2.4.3(8606)</a>
</div>
