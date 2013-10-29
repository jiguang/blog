---
layout: post
title: 性能黄金法则
category:
  - JavaScript
tags:
  - 性能优化
  - 翻译
---
{% include JB/setup %}

原文地址：<http://www.stevesouders.com/blog/2012/02/10/the-performance-golden-rule/>

昨天我在[Google Ventures][1]为他们的一些投资公司做了个研讨会。我不知道听众会有多少关于性能优化的背景知识，因此我从2007年的第一个演示开始，回顾了几乎跟性能优化相关的所有内容，真的是很怀旧啊。话说距离我开始谈论[《高性能网站建设指南》][2]的最佳实践已经很多年了，我重新审视了这些早期的提示，比如[减少HTTP请求][3]，和[添加Expires头][4]，还有[压缩组件][5]。

不过我还需要回顾得更远一些，想到在还没有[Velocity][6]和[WPO][7]之前，我或许还得澄清一下为什么我会如此关注前端性能。我找到了当时包含性能黄金法则的幻灯片：

> 80-90%的最终用户响应时间都花在前端上。  
> 从这里开始。

还有一些其他相关的幻灯片展示了一些流行的网站分别花在后端和前端的时间，但是数据已经很旧并且很有限了，因此我决定更新一下，下面是我的成果。

首先是一个瀑布图，它展示了前后端的划分。这个瀑布图是[LinkedIn][8]的。这里“后端”的时间是指从服务器返回第一个字节到客户端所花费的时间。它通常包含大部分的后端处理：数据库查询、远程web服务调用、拼接HTML等等。其余的是“前端”的时间，它包含了显而易见的前端阶段，诸如执行JavaScript代码以及渲染页面等。它同时也包含了下载页面上所有相关资源的时间。我把这些划分到前端时间里是因为，有许多切实可行的办法可以减少这个时间，比如 [异步加载脚本][9]，[合并脚本和样式表][3]以及[域名分散][10]（即通过多个域名实现并行下载的策略  ——[译者][11]注）等。

![Golden waterfall][12]

对于排名前十位的网站分析结果显示，平均在前端花费的时间占比为76%，略低于黄金法则中提出的80-90%的值。不过别忘了，这些网站的前端都经过了高度的优化，并且其中两个是载入资源非常少的搜索页面（而不是结果页面）。

![Golden top10][13]

对于排名10000左右的10个网站进行的分析，可以得到一个更典型的视图。平均在前端花费的时间占比为92%，高于排名前10的76%，甚至高于黄金法则中建议的80-90%。

![Golden 9990][14]

为了使与会者接受这个法则，我展示了他们自己网站的前后端花费时间占比，得到的结果为前端占比84%。这有助于使他们的认可我的理论，即前端的性能才是最难最有挑战的，也是最应该给予关注的。

![Golden startups][15]

后来我想起来我在[HTTP Archive][16]上还有关于网站耗时的信息。不过我一般不展示这些信息，因为我认为真正的用户度量应该更准确一些，不过我计算了被抓取到的50000个网站的前后端耗时占比，结果前端占比为87%。

![Top50ksite][17]

能够获取这些比2007年更新的信息来验证性能黄金法则真是太好了，而且它也显示了前端性能优化越来越受重视了。如果你担心可用性和可扩展性，那就关注一下后端。但是如果你担心载入网站时用户等待的时间太久，那么关注前端才是王道。

 [1]: http://www.googleventures.com/
 [2]: http://stevesouders.com/hpws/
 [3]: http://developer.yahoo.com/blogs/ydn/posts/2007/04/rule_1_make_few/
 [4]: http://developer.yahoo.com/blogs/ydn/posts/2007/05/high_performanc_2/
 [5]: http://developer.yahoo.com/blogs/ydn/posts/2007/07/high_performanc_3/
 [6]: http://velocityconf.com/velocity2012
 [7]: http://www.stevesouders.com/blog/2010/05/07/wpo-web-performance-optimization/
 [8]: http://www.linkedin.com/
 [9]: http://www.stevesouders.com/blog/2009/04/27/loading-scripts-without-blocking/
 [10]: http://www.stevesouders.com/blog/2009/05/12/sharding-dominant-domains/
 [11]: http://jiguang.github.com "译者"
 [12]: http://jiguang.github.com/content/uploads/2012/02/golden-waterfall.png "golden-waterfall.png"
 [13]: http://jiguang.github.com/content/uploads/2012/02/golden-top10.png "golden-top10.png"
 [14]: http://jiguang.github.com/content/uploads/2012/02/golden-9990.png "golden-9990.png"
 [15]: http://jiguang.github.com/content/uploads/2012/02/golden-startups.png "golden-startups.png"
 [16]: http://httparchive.org/
 [17]: http://jiguang.github.com/content/uploads/2012/02/top50ksite1.png "top50ksite.png"