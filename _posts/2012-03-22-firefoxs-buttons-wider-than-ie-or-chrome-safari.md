---
layout: post
title: FireFox的按钮比IE和Chrome/Safari宽的解决办法
category:
  - html-css
tags:
  - Firefox
---
{% include JB/setup %}

我讨厌浪费大好的时光编写样式，下面有个很好的例子可以佐证我的想法：

Firefox中的按钮（button）比IE或者Webkit的都宽，为啥？这个小点缀其实藏在浏览器的样式中：

    button::-moz-focus-inner,
    	input[type="reset"]::-moz-focus-inner,
    	input[type="button"]::-moz-focus-inner,
    	input[type="submit"]::-moz-focus-inner,
    	input[type="file"] > input[type="button"]::-moz-focus-inner {
    	padding: 0px 2px 0px 2px;
    }

是的，确实宽了4像素。多谢Firefox。

很幸运，我们可以把它修复！

    button::-moz-focus-inner,
    	input[type="reset"]::-moz-focus-inner,
    	input[type="button"]::-moz-focus-inner,
    	input[type="submit"]::-moz-focus-inner,
    	input[type="file"] > input[type="button"]::-moz-focus-inner {
    	padding: 0px;
    }

这个太easy了：）

我们站点里可能有个class会应用到所有的button上，我把它命名为“formbutton”（不是我选的名字，是我老板8年前选的。这名字不错吧？像史诗一样不朽。）那么，我就可以将上面代码简化为：

    .formbutton::-moz-focus-inner {
    	padding: 0px;
    }

**想了解更多信息的话：**

我是受到这篇文章[《viewing FireFox’s default style sheets》][1]的启发才发现的。细心的你可能会发现，还有另外一个样式表，它是用作键盘导航的。明智地使用你发现新事物的能力，不需要将它删除或者替换掉。

如果你还想了解双冒号是个啥意思，这里还有篇文章：[《double colon notation》][2]。

至今在Webkit或者IE中貌似都还没有等效的伪元素，但是我得承认，自从这个修复了我遇到的问题后，我再也不用到处搜寻解决办法了。

原文地址：<http://stephaniehobson.ca/2011/03/09/firefoxs-buttons-wider-than-ie-or-chrome-safari/>

 [1]: http://www.oppenheim.com.au/2008/07/06/how-to-view-firefoxs-default-internal-css-stylesheet/
 [2]: http://www.evotech.net/blog/2007/05/after-v-after-what-is-double-colon-notation/