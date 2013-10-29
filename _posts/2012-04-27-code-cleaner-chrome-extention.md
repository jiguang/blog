---
layout: post
title: 代码复用工具 Code Cleaner Chrome 扩展版
category:
  - DevTools
tags:
  - Code Cleaner
---
{% include JB/setup %}

**UPDATE:** 最近 Chrome 应用商店经常打不开，故强烈推荐使用 <a href="http://44ux.com/demo/code-cleaner/index.htm" target="_blank">Code Cleaner 在线版</a> 并加入书签 ;)

在实际开发的过程中，尤其是一些大型项目的开发中，经常会遇到代码复用的问题，很多代码是可复用的，但并不一定已经形成复用组件。这时就需要手动拷贝原始代码，由于手动拷贝的原始代码含有大量无用信息，所以清理工作就变得很重要。

Code Cleaner 就是这样一个代码清理工具，它可以方便地清理代码中的冗余信息，清理后可得到简洁可复用的DOM结构，非常适合团队复用代码。如果一段代码经常会用到，后续再继续整理到组件库即可。

下面是该扩展的简介：

Code Cleaner helps web developers get clear DOM code, when some module on page can be used again, you can use the ‘Elements Panel’,then right click mouse, choose ‘Copy as HTML’, then paste the code into Code Cleaner, custom your own option, there you go!

Code Cleaner 可以帮助Web开发人员快速获取简洁的DOM结构，当某个模块极具复用性时，可以直接在开发工具的Elements面板右键拷贝HTML，然后粘贴到Code Cleaner中进行处理，即可得到清洁可复用的代码。

V 1.0 版特性：

**注意：该扩展只支持最新版本Chrome（19.0.1084.9以上版本），低版本用户若想使用本扩展，请更新Chrome浏览器（请原谅我如此之挫）。**

* 清除 id, class, script, idinline-style, table, Link, img, script, br, html-comment, blank  
* 清除自定义属性（自动补全）  
* 仅保留常见属性  
* 自定义正则匹配  
* 替换全部文本

屏幕截图：

![2](http://jiguang.github.com/content/uploads/2012/04/screenshot.jpg)

这里是下载地址：[Chrome商店英文版][3]    [Chrome商店中文版][4]    [Code Cleaner 在线版](http://44ux.com/demo/code-cleaner/index.htm)

使用方法：

- 安装完成后该扩展会出现在“开发人员工具”中，如截图所示；

- 使用“Elements”面板，或直接在页面上点击鼠标右键“审查元素”；

- 在“Elements”面板中选中元素，点击右键“Copy as HTML”；

- 点击“Code Cleaner”选项卡，将代码粘贴到文本域中，根据自己的需要选择要清理的部分，直到满意为止。

 

如果大家有什么建议或者批评指正，欢迎在这里留言与我交流，感谢支持！

 [3]: https://chrome.google.com/webstore/detail/ajnfhahbkopfgiheliocnmeobejfdlfe?hl=en-US "Chrome商店英文版"
 [4]: https://chrome.google.com/webstore/detail/ajnfhahbkopfgiheliocnmeobejfdlfe "Chrome商店中文版"