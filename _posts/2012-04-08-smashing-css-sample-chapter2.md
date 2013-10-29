---
layout: post
title: "【TEAP早期试读】Firebug快速入门教程"
category:
  - html-css
tags:
  - Smashing CSS
  - 翻译
---
{% include JB/setup %}

>**图灵社区按**：
> TEAP是什么？TEAP是Turingbook Early Access Program的简称，即早期试读，它公布的是图灵在途新书未经编辑的内容。一本书的翻译周期约为3到6个月，如果在翻译过程中，译者就能与读者进行沟通和交流，对整本书的翻译品质是有帮助的。通过TEAP，读者可以提前阅读将来才能出版的内容，译者也能收获宝贵的反馈意见，改进翻译，提高质量。

> **简介：**本书原名为 [Smashing CSS](http://www.ituring.com.cn/book/924)，中文暂定名为《精彩绝伦的CSS》，本文节选自书中第一章。

> **作者：** Eric A. Meyer 从1993年开始就投身于web开发领域，是HTML、CSS和Web标准方向国际上公认的专家。他是复杂螺旋顾问公司（Complex Spiral Consulting）的创始人，该公司为美国在线（America On-Line）、苹果（Apple）、奥多比（Adobe）、微软（Microsoft）、前进保险（Progressive Insurance），宣威-威廉斯（Sherwin-Williams）等许多公司提供咨询服务。他是微格式（micro- formats）运动的联合发起者，并且是An Event Apart网站开发者系列会议的联合创办人（和Jeffrey Zeldman一起），他还是5本最畅销的CSS和网页设计书籍的作者。


无论做什么事情，工具都能起到很大的辅助作用，创建网页或者应用亦是如此。对于CSS来说，既有可以帮助我们书写CSS的工具，也有使用CSS构建的工具来辅助我们进行开发。甚至有的工具可以使我们的浏览器支持更多原生并不支持的CSS特性，你不仅是开发者，还是个能工巧匠！相信本章介绍的这些工具绝对会撑爆你的工具箱。

**FIREBUG**

Firebug（见图1-1）是任何网页开发人员的工具箱中不可或缺的两个工具之一（关于另外一个，请参考1.2节“Web Developer Toolbar”）。它是火狐浏览器（Firefox）中的一个完全免费的扩展，如果你用的是其他浏览器，你也可以接着往下看，因为你一样可以使用Firebug！

![Firebug][2]

   [2]: http://44ux.com/content/uploads/2012/02/firebug.png (firebug.png)

图1-1：Firebug主页

要想安装Firebug，可以在火狐浏览器中访问getfirebug.com，然后点击安装按钮（就在网页的右上方）开始安装，安装完成后重启火狐浏览器即可。现在准备开始你的神奇之旅吧！

我没法在这么短的篇幅中涵盖Firebug的全部功能。实际上，即使整个一章的篇幅都未必够用，我这里只讲一些重点。

图1-2中所示的HTML选项卡左侧展示的是文档的结构，点击箭头可以展开或收缩文档的子结构。注意在该选项卡中，当把鼠标悬停在某个元素名上时，该元素会在页面中高亮显示。最神奇的是，它还可以通过彩色区域和代码来展示元素的内边距（padding）和外边距（margin）。例如，本例中的内容区域为浅蓝色，内边距是淡紫色而外边距是浅黄色。具体是什么颜色其实没那么重要，因为在页面上可以很直观地看到效果。

![Checklayout][3]

   [3]: http://44ux.com/content/uploads/2012/02/checklayout.png (checklayout.png)

图1-2：通过Firebug查看元素的布局

在HTML选项卡的右侧，可以通过点击样式（Style）选项卡来查看应用在当前元素上的CSS（见图1-3）。这里不仅包含了你自己写的样式，还包含了浏览器自身的内建样式。例如，你可以看一下html.css和quirk.css这两个文件的内容，这些就是内建样式（这些样式称为“用户代理样式”，可以通过点击样式选项卡，在弹出的菜单中选择是否显示用户代理样式）。

有一点需要注意的是，Firebug有时候会显示一些像-moz-background-clip这种未曾声明过的属性，如果确定没有明确声明那些属性，基本上就可以忽略掉。另外，如果你使用的是简写形式的属性，它也会自动扩展成独立的属性，也就是说像这样的代码：

    font: 1em "Andale Mono", "Courier New", Courier, monospace;


在Firebug中就会变成这样：

    font-family: "Andale Mono","Courier New",Courier, monospace;
    font-size: 1em;
    font-size-adjust: none;
    font-stretch: normal;
    font-style: normal;
    font-variant: normal;
    font-weight: normal;
    line-height: normal;


![Styletab][4]

   [4]: http://44ux.com/content/uploads/2012/02/styletab.png (styletab.png)

图1-3：在独立的Firebug窗口中样式选项卡全部展开

尽管刚开始可能有点儿令人困惑，但是这种呈现方式并不赖，因为它可以提醒你在这些简写的背后还隐藏着一些东西（更多的简写属性详见第二章）。

另外一个需要注意的是，样式选项卡中列出的规则是按特殊性（specificity）降序排列的，也就是说，第一个是应用在当前元素上最特殊的规则（优先级最高），第二个是特殊性稍低一点的规则，依此类推（更多关于特殊性的内容，详见第二章“特殊性”部分）。

通过随时单击鼠标右键并在弹出的菜单中选择审查元素（Inspect Element）选项可以查看任何元素（如图1-4所示），也可以点击Firebug图标旁边带箭头的方框图标来切换到审查模式。此时，随着鼠标在页面上的移动，当前元素会显现出轮廓，单击元素即可进行查看。

在Firebug中单击任何声明左侧的空白处都可以禁用该声明，这在通过禁用声明的方式测试各个属性之间的相互作用时非常有用。在图1-5中还可以看到，当鼠标悬停在颜色值上时会出现一个小色块用来展示该值对应的颜色。

![Inspectelement][5]

   [5]: http://44ux.com/content/uploads/2012/02/inspectelement.png (inspectelement.png)

图1-4：右键菜单中的审查元素选项

![Disabledstyle][6]

   [6]: http://44ux.com/content/uploads/2012/02/disabledstyle.png (disabledstyle.png)

图1-5：禁用的样式和悬浮的颜色框

如图1-6所示，在Firebug的样式选项卡中也可以查看元素的计算样式。这就意味着，无论你是否曾经声明过，它都会把所有已知的应用在该元素上的CSS属性展示出来。记住，所有的CSS属性都有默认值。在该视图下可以查看全部的默认值，例如当你想知道浏览器应用在标题上的行高（line-height）的确切像素值时，该视图就变得非常有用了。

![Computedstyle][7]

   [7]: http://44ux.com/content/uploads/2012/02/computedstyle.png (computedstyle.png)

图1-6：计算样式

通过查看元素的盒模型（box model）部分，可以精确地查看元素的尺寸大小，如元素的宽高、内边距和外边距等（如图1-7所示），这些都是用像素来表示的。更酷的是当鼠标悬停在该面板中的框上时，页面上就会出现沿着元素外框的上边缘和左边缘放置的像素尺。

在图1-8中我们可以很明显地看到，Firebug还有许多其他的功能，诸如可以编辑元素的属性值（比如类）或者元素本身的内容、添加或编辑CSS属性和值等等。通过在Firebug界面中随时单击鼠标左键或者右键可以自行探索Firebug的功能，看看你还能做点儿什么。

![Layouttab][8]

   [8]: http://44ux.com/content/uploads/2012/02/layouttab.png (layouttab.png)

图1-7：布局选项卡

![Editcss][9]

   [9]: http://44ux.com/content/uploads/2012/02/editcss.png (editcss.png)

图1-8：在样式选项卡下飞速编辑CSS

需要注意，当在样式选项卡下查看元素的CSS时，是无法看到任何伪元素（pseudo-elements）相关的规则影响元素的。作为例子，如果我们使用了这样一个选择器p:first-letter的话，那么当查看p元素时就无法看到这条规则，伪类（pseudo-classes）是可以看到的但是伪元素不能。而如果使用了包含生成内容的清除浮动（clearfix）解决方案，那就很有挑战性了（见第四章“清除浮动”部分）。

如果你不使用Firefox进行开发但却想一睹Firebug的风采，那么访问这个页面[getfirebug.com/lite.html][10]（如图1-9所示）并且按照页面上的指引进行安装，就可以使Firebug运行在Internet Explorer、Opera或者Safari等浏览器中，以此来适应你的开发环境。可以把Firebug精简版链接到测试页面进行测试，或者把它保存到书签栏存成一个书签小程序（bookmarklet）（这也是我所推荐的方式）。

   [10]: http://getfirebug.com/lite.html

![Firebuglite][11]

   [11]: http://44ux.com/content/uploads/2012/02/firebuglite.png (firebuglite.png)

图1-9：Firebug精简版在IE浏览器上运行

这个版本的Firebug并不像火狐扩展版的功能那么多，因此才有“精简版”一说，但是它仍然很好很强大。

>Hi！我是本书的译者[姬光][13]，如果你对本书或者译文有任何建议，欢迎在此留言与编辑和我进行交流，也欢迎光顾我的个人博客（[44ux.com][14]）与我交流：）

   [13]: http://www.ituring.com.cn/users/85358 (姬光)
   [14]: http://www.44ux.com (姬光的博客)