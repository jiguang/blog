---
layout: post
title: Firefox 10内置的Web开发者工具
category:
  - DevTools
tags:
  - Firebug
  - Firefox
post_format: [ ]
---
{% include JB/setup %}

在刚刚发布的[Firefox 10][1]中新增了多个内置的Web开发工具，可以说目前来看基本都是鸡肋。对于习惯了[Firebug][2]的开发者来说，Firefox的这些工具简直太小儿科了！不过不能因为开始很鸡肋，就对它不理不睬，我们还是希望能够不断地有更加出色的Web开发工具出现的。

[笔者][3]也是带着好奇+折腾没够的心理，想看一看这货究竟有没有点儿长处。先来看看Page Inspector的截图吧：

![][5]

图1：Firefox内置开发者工具截图

怎么样？很熟悉吧？

Firefox 10内置的这个工具目前只能查看HTML结构和CSS属性，对于HTML结构只能修改标签内的属性值，而不能修改文本内容。

对于CSS属性查看器，可以像Firebug一样增添属性或者修改属性值，也可以显示属性之间相互覆盖的情况。

这货不就是个精简版Firebug吗？是的！看样子是要跟Firebug抢饭碗啊，不过目前来看，这条路还长着呢。

那么，它究竟有没有点儿长处呢？经过[笔者][3]仔细查找，终于找到值得一提的一点长处了。在查看CSS的面板中，共有两个标签：Rules和Properties。

在Properties标签下可以查看或搜索应用在当前被检查的元素上的CSS属性，当鼠标悬浮到某个属性上时会出现一个小问号图标，点击此图标会直接跳到MDN上关于该属性的文档。因此，当你对某个属性不太熟悉时，可以直接点击这里查看文档，方便学习之用，这勉强算是一点儿优点吧。

另外，在Properties标签的搜索框旁边有个选项“Only user styles”，可以选择是否只显示用户样式。如果取消该选项，就可以看到许多浏览器内置的样式了，可以通过它来查看某些内置样式的值。

好了，我实在找不出其他有用的地方了！这里还有一个[官方的视频][5]，里面介绍的有些东西在Firefox 10里还没有，可以简单看一下。

对于Firefox本身，大家也可以关注一下它为开发者提供了哪些新的东西：[Firefox 10 for developers][6] 。

在前沿技术方面，Firefox 10还为开发者提供了全屏API，开发者可以开发以全屏模式运行的web应用。Firefox 10增加了对WebGL3D绘图标准的支持，并且支持CSS 3 3D Transform。这里有个原来只能在webkit运行的[经典例子][7]，现在也可以在Firefox 10运行了，不过看上去有点儿慢。

另外值得一提的是：Firefox 10明显改善了插件的兼容性问题，Mozilla能够自动检测服务器上插件的兼容性，在用户升级到Firefox 10时，此前的插件中的绝大部分都将标记为兼容插件。这就意味着自Firefox 10以后，插件开发者们就不用随着Firefox的每次升级而手动更改兼容版本号了。

有人说Firebug要危险了，不过[笔者][3]并不这么认为。Firebug的社区是非常强大而活跃的，还有很多坚决拥护Firebug的开发者，所以Firefox想把Firebug比下去还欠些火候。不过话说回来，如果Firefox真的能够开发出比Firebug更优秀更好用的产品，那么也不妨一试。

 [1]: http://www.mozilla.org/en-US/firefox/features/
 [2]: http://getfirebug.com/
 [3]: http://jiguang.github.com
 [4]: http://jiguang.github.com/content/uploads/2012/02/firefox_developer_tool.png
 [5]: http://player.youku.com/player.php/sid/XMzQ3NzM0MTI4/v.swf
 [6]: https://developer.mozilla.org/en/Firefox_10_for_developers
 [7]: http://www.webkit.org/blog-files/3d-transforms/poster-circle.html