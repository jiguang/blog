---
title: 如何清除各种浏览器的DNS缓存
layout: post
category:
  - DevTools
tags: [ ]
---
{% include JB/setup %}

在Web开发调试网页的时候，经常会被各种缓存搞得焦头烂额，虽然有[Fiddler][1]等得力工具，但是开发起来还是不够方便，而且Fiddler有时候真的很慢。下面就介绍几个原生的方法，通过一些设置禁止DNS缓存，经过测试Firefox和IE的还比较好用，IE在刷新后可以直接展示最新的页面，但FF貌似每次需要几秒钟后刷新才是最新的页面。

首先介绍一下基本知识，关于DNS与Hosts的概念，请看百度百科：

DNS <http://baike.baidu.com/view/22276.htm>

Hosts <http://baike.baidu.com/view/597330.htm>

对于Windows系统的DNS服务，也是有缓存的，可以通过下面的方法清除：

1. 控制面板 – 管理工具 – 服务   重启DNS Client 和 DHCP client 服务

2. cmd - ipconfig /flushdns

![pic1][2]

那么，如何清除各种浏览器的DNS缓存呢？ 下面有几种方法可以进行尝试：

对于全部浏览器：

解决方案：重启浏览器（废话！）

对于Chrome：

解决方案：访问:  <chrome://net-internals/#dns>  有清空按钮，反应较慢，要等一会儿才能切换过来

对于IE：

解决方案：在注册表的 HKEY\_CURRENT\_USER\Software\Microsoft\Windows\CurrentVersion\InternetSettings\ 中创建 DnsCacheTimeout 和 ServerInfoTimeOut ，类型为 DWORD 。都指定为 5 ，浏览器就的 dns cache 过期时间就是 5s 。

即：

[HKEY\_CURRENT\_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings]

“DnsCacheEnabled”=dword:00000000

“DnsCacheTimeout”=dword:00000000

“ServerInfoTimeOut”=dword:00000000

微软相关文档：<http://support.microsoft.com/kb/318803>

注意：Windows 2000 以前的 Microsoft 操作系统不包含客户端DNS缓存功能

对于Firefox：

解决方案1：安装插件

DNS Flusher：<https://addons.mozilla.org/zh-CN/firefox/addon/dns-flusher/>  每次需要点击一次才生效

HostAdmin：<https://addons.mozilla.org/zh-CN/firefox/addon/hostadmin/>   即时生效，但列出了hosts文件里全部内容，对于我们复杂的环境不好用，个人开发比较合适

![pic2][3]

解决方案2：

打开firefox，输入 “about:config”（点击 “I’ll be careful, i promise” ）.

右键单击空白处，选择 “New” -> “Integer”

输入 “**network.dnsCacheExpiration**” 作为属性名，设置 “0″作为整数值

再次右键单击空白处，选择 “New” -> “Integer”

这次输入 “**network.dnsCacheEntries**” 作为属性名，设置 “0″作为整数值

Done, 你已经禁止了dns缓存

这几种方法重在折腾，经过实验IE的修改注册表的方法最好用，切换Hosts后直接刷新即可。Firefox的插件比较好用。

 [1]: http://fiddler2.com/fiddler2/
 [2]: http://jiguang.github.com/content/uploads/2012/02/6cddab8b977758a240b3fa31151d1520.png
 [3]: http://jiguang.github.com/content/uploads/2012/02/d5196a59909937fdb4936b0d1991dbde.png