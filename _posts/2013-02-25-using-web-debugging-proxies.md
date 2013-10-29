---
layout: post
title: "善用 Web 调试代理工具"
description: "善用 Web 调试代理工具"
category:
  - DevTools
tags: [翻译]
---
{% include JB/setup %}

原文地址：[Using Web Debugging Proxies](http://net.tutsplus.com/tutorials/using-web-debugging-proxies/)

当我们调试前端代码的时候，通常会在检查CSS和JavaScript如何渲染页面上花费大量时间，但是了解网络请求对页面的影响也同等重要。很多情况下由于我们在本地开发，可能会忽略页面的大小、延迟以及脚本的载入和阻塞对网站用户体验的巨大影响。因此，一套得心应手的网络流量检查工具是必不可少的。

幸运的是，所有主流浏览器都提供了可以查看网络流量的调试工具，而且第三方工具比如 Fiddler 和 Charles 不仅允许查看网络请求，而且还提供了可以跟网站交互的扩展功能。

下面我们会对两种类型的工具分别进行介绍。

##基于浏览器的流量嗅探

正如我提过的，每个主流浏览器都有内置的调试工具。它们包括：

* Internet Explorer 的 F12 开发者工具
* Firefox 的 Web开发者工具以及 Firebug 附加组件
* Chrome 的 开发者工具
* Opera 的 Dragonfly
* Safari 的 Web检查器

（[译者](http://44ux.com)注：关于这些工具，还可参见译者的[另一篇文章](http://44ux.com/blog/2012/04/07/smashing-css-sample-chapter2/)。）

每个工具集都有自己独特的功能，并且它们都有搜集网络流量信息的能力。看一下下面几张图，你会发现尽管UI不尽相同，但搜集和显示的数据却非常相像。

![](http://44ux.com/content/uploads/2013/02/debug-proxies-network-ie.png)

![](http://44ux.com/content/uploads/2013/02/debug-proxies-network-chrome.png)

![](http://44ux.com/content/uploads/2013/02/debug-proxies-network-opera.png)

![](http://44ux.com/content/uploads/2013/02/debug-proxies-network-firebug.png)

![](http://44ux.com/content/uploads/2013/02/debug-proxies-network-firefox.png)


最终的结果就是，一个由浏览器下载的资源或数据而产生的网络请求所组成的列表。网络工具可以截获这些请求并将主要的数据展示给你：

* 请求类型（GET，POST 等等）
* 请求的是什么
* URI
* 状态
* 大小
* 完成请求所耗时间

因此，如果我们在 Firebug 中查看结果，可以看到这些请求拉回了主页面以及相关的CSS和JavaScript文件，包括亚马逊AWS上的资源。由于图片的限制，我无法向你展示全部加载内容，不过这里也还返回了图片和Flash swf文件。

##深入研究

有了这些信息后，就可以深入查看特定的请求，确定是否接收到了正确的数据，以及查看为什么会有耗时很长的请求。假设我查看Webtrend的JavaScript文件请求。它耗时1.2秒下载，并且我想看看请求是如何被处理的。我可以展开该请求，看看它是使用gzip压缩（是的）：

![](http://44ux.com/content/uploads/2013/02/debug-proxies-webtrend.png)

和是否被最小化压缩（minified）：

![](http://44ux.com/content/uploads/2013/02/debug-proxies-webtrend-min.png)

本例中的文件并没有被最小化压缩，然后我就可以找开发人员跟进看这样做是否合理。诚然，这只是个2k的文件，不过每个字节其实都很重要，这些信息可以让我们更好地优化网站。

##网络计时

网络延时可以成为致命杀手，尤其是对于那些依赖外部API或者多个脚本文件来实现功能的单页面应用（single-page apps）。大部分浏览器会尽可能地异步加载资源，但是有些资源比如JavaScript文件，可以触发阻塞事件。尽可能地限制这些文件，并以此来优化资源加载至关重要。如果我们看一下这张图，就会发现这个文件花费了1.4秒用来加载：

![](http://44ux.com/content/uploads/2013/02/debug-proxies-load-graphic.png)

当悬停在时间轴上的时候，会出现一个对话框，展示了请求被处理的分解信息：

![](http://44ux.com/content/uploads/2013/02/debug-proxies-load.png)

部分原因是由于它被阻塞了760毫秒。如果这是个普遍存在的问题，你可以尝试使用脚本加载器（比如 [RequireJS](http://requirejs.org/)）来更好地管理脚本加载和依赖关系。

##Ajax请求

由于动态应用无处不在，因此能够查看XHR调用就至关重要了。之前你已经见识过无数的网络请求，试图过滤这些请求并从中找出XHR调用并不高效。因此，大部分工具允许你按请求类型查看。这里我是根据XHR请求过滤的，因此我可以评估请求和响应：

![](http://44ux.com/content/uploads/2013/02/debug-proxies-chrome-xhr.png)

通过深入查看请求信息，我可以获得请求的重要细节，比如请求头、状态、请求方法、cookies，更重要的是可以看到该请求返回的数据：

![](http://44ux.com/content/uploads/2013/02/debug-proxies-chrome-xhr-response.png)

本例中返回的是HTML，不过响应可以是包括文本、JSON或XML在内的任何内容。更棒的是在我遇到任何问题时，都可以充分地查看请求细节。

##Cokkies

Cookies真的非常有用，由于我们会广泛地用到它，因此一个方便地查看Cookie值的方法将使生活变得更轻松。开发者工具即可轻松实现这些，它可以展示哪些Cookies被发送或者接收了。

![](http://44ux.com/content/uploads/2013/02/debug-proxies-cookies.png)

如果你曾经做过服务器端开发而没有客户端工具相伴，你就会知道这有多爽了。

总之，最棒的就是这些功能都内置在你的浏览器中，使开启工具调试和查看细节都变得无比便捷。然而，有时你还需要多一点马力。

##第三方HTTP代理工具

HTTP代理程序如 [Fiddler](http://www.fiddler2.com/) 和 the [Charles Web Debugging Proxy](http://www.charlesproxy.com/) 是基于浏览器的网络流量嗅探工具的老大哥。它们不仅能截获来自浏览器的网络请求，也能截获你机器上其他程序的网络请求，这使得它们在调试方面用途广泛。通常它们也会提供更丰富的特性，比如：

* 带宽限制
* 特定请求的自动响应
* 传输过程中的资源替换
* SSL代理
* 插件生态系统
* 可自定义的脚本
* 记录和回放测试场景

我常用基于Windows的、功能丰富的 Fiddler（免费的！）。由于其强大的特性集合，在微软内部的使用也很广泛。Fiddler的开发者 Eric Lawrence 之前就在微软工作，并且仍然维护着该软件。

如果我们看下界面，就会发现与那些浏览器工具类似的输出。所有的网络请求都按照关键信息显示。

![](http://44ux.com/content/uploads/2013/02/debug-proxies-fiddler.png)

深入查看某个请求，可以看到更多的细节，包括压缩后jQuery源代码：

![](http://44ux.com/content/uploads/2013/02/debug-proxies-fiddler-drill.png)

大部分这种信息也可以通过基于浏览器的工具获取到，但是当你想确定是不是某个库搞垮了你的网站的时候怎么办？你可以直接换掉这些库来进行定位。一个较好的路径就是创建一个Fiddler自动响应器（AutoResponder），拦截并替换掉生产环境中的库。Fiddler会接收这个请求并将其替换为本地文件。下面我们就来一探究竟。

首先，我需要标识出需要替换的URI。在本例中，我看到我的博客主题使用了jQuery v1.2.6。真是疯了，不过在我丢掉它之前，需要看一下jQuery v1.8.3是否能如预期一样正常工作。

![](http://44ux.com/content/uploads/2013/02/debug-proxies-jquery-126.png)

我点击 jQuery v1.2.6 这条记录，在 Fiddler 的右栏中，选择“AutoResponder”页卡并选择“Enable automatic responses”。可以直接将URI拖拽到规则编辑器中。你会发现规则是从比较URI开始的。如果匹配，它将以你选择的某个事件作为响应。

![](http://44ux.com/content/uploads/2013/02/debug-proxies-fiddler-autorule1.png)

由于我想测试的是 jQuery 1.8.3，我希望这条规则可以用我本地的jQuery副本替换生产环境的版本。

![](http://44ux.com/content/uploads/2013/02/debug-proxies-fiddler-autorule2.png)

保存这条规则并刷新我的页面。最终结果是，尽管URI可能看上去相同，检查结果可以验证 jQuery v1.8.3 实际上已经嵌入了。这样我就可以在不对网站进行任何更改的情况下直接测试了。

![](http://44ux.com/content/uploads/2013/02/debug-proxies-fiddler-autorule3.png)

从调试的角度来讲，这种功能无比实用，尤其是当你想定位一个藏在老版本框架或库中的bug时候。

##附加组件生态系统

Fiddler从它的[附件组件生态系统](http://www.fiddler2.com/Fiddler2/extensions.asp)中获益良多，这个系统通过[iFiddlerExtension接口](http://www.fiddler2.com/fiddler/dev/IFiddlerExtension.asp)为Fiddler扩展功能。目前有如下功能的附加组件：

* 压力测试
* 安全审计
* 流量对比用来对比概况
* JavaScript格式化


对于它本身来说，Fiddler拥有无数特性，实在无法在本文一一列举。这就是为啥有个[330页的书](http://www.fiddler2.com/book/)来教你如何用好它的原因了。这本书只有10美元，即可让你从内到外掌握这个伟大的工具。

##OSX 和 Linux

如果你在用OSX或者Linux，那么最佳选择就是Charles Web调试代理工具。该工具支持广泛并且商业化成熟，每一分钱都花得很值得。我曾寻找过专注于web开发的替代品，而Charles却脱颖而出。

界面跟Fiddler很类似，不过它提供了两种不同的方式查看网络流量：

![](http://44ux.com/content/uploads/2013/02/debug-proxies-charles-struct.png)

![](http://44ux.com/content/uploads/2013/02/debug-proxies-charles-seq.png)

样式完全随你，我更倾向于结构化视图，因为感觉它更有条理，不过搜寻特定的URI就稍有不便。

与Fiddler类似，Charles也提供了自动响应功能，叫做“Map Local…”，可以通过鼠标右击某个URI呼出。该功能允许你选择一个本地文件进行工作。

当刷新页面后，我的jQuery v1.2.6就被我本机上的jQuery v1.9替换掉了。

![](http://44ux.com/content/uploads/2013/02/debug-proxies-charles-autoresp.png)

Charles另外一个极好的特性就是，它可以抑制网络请求来模拟特定的带宽速度。仍记得使用56K猫时的那些令人捉急的日子，使用这个功能可以让你尽情追忆似水年华：

![](http://44ux.com/content/uploads/2013/02/debug-proxies-charles-throttle.png)

Chares也提供了一个跨平台的界面，因此也可以在Windows上工作。

##该用哪一个

所有这些工具我每时每刻都在用，因为我需要测试每个主流的浏览器，有了这些功能着实可以使问题定位更容易些。当然，是选择基于浏览器的嗅探工具，还是基于应用程序的代理工具完全取决于你的调试需要。

> 如果你只需要检查某些流量并查看结果，那么基于浏览器的嗅探工具可能是你的最佳选择。

另一方面，如果你需要精确控制URI如何响应，或者希望可以灵活地创建自定义脚本，那么像Fiddler或Charles这种工具才是你需要的。令人欣慰的是，我们有稳定的备选方案可以帮助我们实现这些功能，尤其是当项目复杂度日益增加时，也不会感到无力。

























