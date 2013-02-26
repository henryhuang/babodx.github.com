--- 
categories: []
comments: true
layout: post
title: nginx采用epoll的事件模型，为何效率高。
---
以前就知道在linux下nginx采用epoll事件模型，处理效率高。但是一直不知道具体为什么，今天查看了下文档，了解了原因。<br>首先nginx支持一下这些事件模型（才考nginx的wiki）<br>Nginx支持如下处理连接的方法（I/O复用方法），这些方法可以通过use指令指定。 <ul>
<li>
<strong>select</strong> - 标准方法。 如果当前平台没有更有效的方法，它是编译时默认的方法。你可以使用配置参数 --with-select_module 和 --without-select_module 来启用或禁用这个模块。 </li>
<li>
<strong>poll</strong> - 标准方法。 如果当前平台没有更有效的方法，它是编译时默认的方法。你可以使用配置参数 --with-poll_module 和 --without-poll_module 来启用或禁用这个模块。 </li>
<li>
<strong>kqueue</strong> - 高效的方法，使用于 FreeBSD 4.1+, OpenBSD 2.9+, NetBSD 2.0 和 MacOS X. 使用双处理器的MacOS X系统使用kqueue可能会造成内核崩溃。 </li>
<li>
<strong>epoll</strong> - 高效的方法，使用于Linux内核2.6版本及以后的系统。在某些发行版本中，如SuSE 8.2, 有让2.4版本的内核支持epoll的补丁。 </li>
<li>
<strong>rtsig</strong> - 可执行的实时信号，使用于Linux内核版本2.2.19以后的系统。默认情况下整个系统中不能出现大于1024个POSIX实时(排队)信号。这种情况对于高负载的服务器来说是低效的；所以有必要通过调节内核参数 /proc/sys/kernel/rtsig-max 来增加队列的大小。可是从Linux内核版本2.6.6-mm2开始， 这个参数就不再使用了，并且对于每个进程有一个独立的信号队列，这个队列的大小可以用 RLIMIT_SIGPENDING 参数调节。当这个队列过于拥塞，nginx就放弃它并且开始使用 poll 方法来处理连接直到恢复正常。 </li>
<li>
<strong>/dev/poll</strong> - 高效的方法，使用于 Solaris 7 11/99+, HP/UX 11.22+ (eventport), IRIX 6.5.15+ 和 Tru64 UNIX 5.1A+. </li>
<li>
<strong>eventport</strong> - 高效的方法，使用于 Solaris 10. 为了防止出现内核崩溃的问题， 有必要安装 <a rel="nofollow" href="http://sunsolve.sun.com/search/document.do?assetkey=1-26-102485-1" title="http://sunsolve.sun.com/search/document.do?assetkey=1-26-102485-1" class="external text">这个</a> 安全补丁。 </li>
</ul>在linux下面，只有epoll是高效的方法。<br><br>下面再来看看epoll到底是如何高效的<br>Epoll是<a href="http://www.hudong.com/wiki/Linux%C3%A5%C2%86%C2%85%C3%A6%20%C2%B8" target="_blank" title="Linux内核" class="innerlink">Linux内核</a>为处理大批量句柄而作了改进的<a href="http://www.hudong.com/wiki/poll" target="_blank" title="poll" class="innerlink">poll</a>。要使用epoll只需要这三个系统调用：epoll_create(2)， epoll_ctl(2)， epoll_wait(2)。它是在2.5.44内核中被引进的(epoll(4) is a new API introduced in Linux kernel 2.5.44)，在2.6内核中得到广泛应用。<br><br>epoll的优点<br><ul><li>支持一个进程打开大数目的<a href="http://www.hudong.com/wiki/socket" target="_blank" title="socket" class="innerlink">socket</a>描述符(FD)</li></ul>select 最不能忍受的是一个进程所打开的FD是有一定限制的，由FD_SETSIZE设置，默认值是2048。对于那些需要支持的上万连接数目的IM服务器来说显然太少了。这时候你一是可以选择修改这个宏然后重新编译内核，不过资料也同时指出这样会带来网络效率的下降，二是可以选择多进程的解决方案(传统的Apache方案)，不过虽然linux上面创建进程的代价比较小，但仍旧是不可忽视的，加上进程间数据同步远比不上线程间同步的高效，所以也不是一种完美的方案。不过 epoll则没有这个限制，它所支持的FD上限是最大可以打开文件的数目，这个数字一般远大于2048,举个例子,在1GB内存的机器上大约是10万左右，具体数目可以cat /proc/sys/fs/file-max察看,一般来说这个数目和系统内存关系很大。<ul><li>IO效率不随FD数目增加而线性下降</li></ul>传统的select/poll另一个致命弱点就是当你拥有一个很大的socket集合，不过由于网络延时，任一时间只有部分的socket是"活跃"的，但是select/poll每次调用都会线性扫描全部的集合，导致效率呈现线性下降。但是epoll不存在这个问题，它只会对"活跃"的socket进行操作---这是因为在内核实现中epoll是根据每个fd上面的callback函数实现的。那么，只有"活跃"的socket才会主动的去调用 callback函数，其他idle状态socket则不会，在这点上，epoll实现了一个"伪"AIO，因为这时候推动力在os内核。在一些 benchmark中，如果所有的socket基本上都是活跃的---比如一个高速LAN环境，epoll并不比select/poll有什么效率，相反，如果过多使用epoll_ctl,效率相比还有稍微的下降。但是一旦使用idle connections模拟<a href="http://www.hudong.com/wiki/WAN" target="_blank" title="WAN" class="innerlink">WAN</a>环境,epoll的效率就远在select/poll之上了。<ul><li>使用<a href="http://www.hudong.com/wiki/mmap" title="mmap" class="innerlink">mmap</a>加速内核与用户空间的消息传递。</li></ul>这点实际上涉及到epoll的具体实现了。无论是select,poll还是epoll都需要内核把FD消息通知给用户空间，如何避免不必要的内存拷贝就很重要，在这点上，epoll是通过内核于用户空间mmap同一块内存实现的。而如果你想我一样从2.5内核就关注epoll的话，一定不会忘记手工 mmap这一步的。<ul><li>内核微调</li></ul>这一点其实不算epoll的优点了，而是整个linux平台的优点。也许你可以怀疑linux平台，但是你无法回避linux平台赋予你微调内核的能力。比如，内核<a href="http://www.hudong.com/wiki/TCP/IP" target="_blank" title="TCP/IP" class="innerlink">TCP/IP</a>协议栈使用内存池管理sk_buff结构，那么可以在运行时期动态调整这个内存pool(skb_head_pool)的大小--- 通过echo XXXX>/proc/sys/net/core/hot_list_length完成。再比如listen函数的第2个参数(TCP完成3次握手的数据包队列长度)，也可以根据你平台内存大小动态调整。更甚至在一个数据包面数目巨大但同时每个数据包本身大小却很小的特殊系统上尝试最新的<a href="javascript:linkredwin('NAPI');" title="NAPI" class="link_red">NAPI</a>网卡驱动架构。<br><br><strong>以上这些epoll内容，参考epoll_互动百科<br><br></strong>在我128M的vps上，我查看了一下，file-max的数量已经达到11945<br>应该说确实比apache的方式要好，而且资源占用也少。<div id="related_log" style="font-size:12px">
<b>相关日志：</b><a href="http://xinlogs.com/druapl-url-nginx-rewrite">drupal在nginx 0.8.15下的简洁URL配置</a><a href="http://xinlogs.com/nginx-301-rewrite">nginx通过301统一入口域名</a><a href="http://xinlogs.com/howto-open-rewrite-log">伪静态规则调试技巧</a><a href="http://xinlogs.com/Solve-nginx-502-mistakes">nginx通过轮询避免php-fpm引起的502错误</a>
</div>
