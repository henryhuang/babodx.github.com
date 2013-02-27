--- 
categories: ['Hardware','Mobile']
comments: true
layout: post
title: 诺基亚E63刷机
---
前几天老婆的E63手机总是死机，一直在跟我抱怨。。。本来想给她买个iphone，结果老婆说不喜欢，还是喜欢她红色的E63.

我想想手机买了1年多了，也没有更新过，不如刷个新系统看看吧。于是就开始搜索资料、准备软件。

具体刷机步骤参考
[http://nokia.zol.com.cn/188/297_1876549.html](http://nokia.zol.com.cn/188/297_1876549.html)

**诺基亚刷机方法**

主要是官方刷新，还是用第三方软件刷新

官方的在线刷新就是用官方公布的软件直接更新手机的系统。但是官方更新只限于行货手机，因为官方是根据你手机的CODE号码来为你更新手机系统的，如果你手机的CODE是外国的，那刷新后，就没有中文系统了。所以这个不适合大多数水货手机

我查看了一下，老婆手机的CODE是0569944: 马来西亚的。用官方的估计不行，于是就开始考虑第三方软件。

发现比较著名的就是凤凰刷机工具了。

其实简单的说“凤凰刷机”就是刷机软件的一种，而非官方的刷机软件而已，由于其在性能上有官方刷机软件很多不能完成的任务，因而收到的机友的广泛追捧。

**准备工作**

首先是下载凤凰刷机软件

参考这里[http://nokia.zol.com.cn/1/18_299.html](http://nokia.zol.com.cn/1/18_299.html)

我下载的就是[凤凰刷机2009.34.7.40015最新XX版09.10.30发布](http://nokia.zol.com.cn/1/18_299.html)

下载好凤凰刷机软件后，就是备份手机存储卡上的内容和联系人了。我是直接用读卡器把存储卡拷贝到电脑，然后用诺基亚PC套件导出的联系人。

这些都弄好了，就可以进入刷机步骤了。

**刷机步骤**

安装好凤凰刷机软件，并打开

选择Tools->Image file download

![选择下载img](http://farm9.staticflickr.com/8103/8512516328_851a5902d7.jpg)

然后在Product code内输入你想刷的系统的code码，我这里输入的0569941，新加坡的版本

输入完成后，点download等待下载，大小在60-80M。

下载完成后，点cancel就可以了。

系统下载好了，我们就该向手机内刷了。用USB的数据线连接手机。在手机上选择PC套件模式。

然后选择凤凰2009的菜单file---manage connection，跳出菜单NO CONECTION，再点“NEW”按键。

![new按钮](http://farm9.staticflickr.com/8508/8511405473_51583b1644_z.jpg) 

按NEW键之后会出现下图：

![new截图](http://farm9.staticflickr.com/8100/8511406351_68da4dfd26_z.jpg)

选择USB后，按下一步，直到完成。这样连接里会才会出现USB和NO CONNECTIONS两个选择，一个是USB连接，一个是没有连接，如果是USB连接，表示手机正常。选择USB，点击“APPLY”按钮,然后点击Cancel，连接完成

![usb1截图](http://farm9.staticflickr.com/8226/8512518914_dfa06ce9ed_z.jpg)
![usb2截图](http://farm9.staticflickr.com/8506/8512513418_7c9ca6b164_z.jpg)

菜单File ,open Product，系统会自动调出诺基亚各个型号的RM菜单，你选择自己的机型即可，比如诺基亚E63为RM-437，找到后点OK。

![ok截图](http://farm9.staticflickr.com/8368/8512513018_73ca2419cd_z.jpg)

![ok截图2](http://farm9.staticflickr.com/8386/8512512776_a4c75f2239_z.jpg)

选择RM-437的画面，在出现的提示框里找RM-437，选中，然后点OK，选择normal.

凤凰2009软件菜单Flashing-----Fireware Update,出现下面画面

![update截图](http://farm9.staticflickr.com/8240/8511400983_580969c8e8_z.jpg)

这里有一个非常关键的地方需要注意,就是上图选择RM-437之后,要注意看Product CODE:显示的CODE是多少,如果是欧版的CODE(比如0582481),那就需要你选择以下亚太的CODE的系统,选择你刚才下载的比如<font color="#000000" size="2">0569941</font>的系统. 点击下图右边的小方块(小方块上有省略号的那个)

想刷400.21.013的朋友，如果下载了400的固件，但是在这里没有找到400的话，可以来这里看下

凤凰菜单栏进入：TOOLS-Options-product locetion（下图）

![locetion图](http://farm9.staticflickr.com/8107/8512512350_980a17dfbe_z.jpg)

红色线标注的栏目原来是空白的，这里需要设置下路径，点右边的“add”添加路径如图所示，然后点“apply”确认后退出。

哈哈，看下图出现了400.21.013了

![截图](http://farm9.staticflickr.com/8368/8511400657_7362511b0f_z.jpg)

点击"Refurbish"开始刷机，在进入这个程序之后，无论计算机屏幕右下角出现什么提示，均不要搭理它，直到提示成功。

![截图](http://farm9.staticflickr.com/8371/8511400551_ca21a4f98f_z.jpg) 

到这里就算完成了。哈哈

以上内容基本都是从下面两篇帖子上转过来的。如果有什么不明白，可以直接去看原帖。我只是记录下，省的以后刷机还要找资料。

* [原创][教程]诺基亚E63用凤凰2009成功刷机
* 0300 凤凰2009成功刷 E63最新固件 400.21.013
 
