---
layout: post
title: 如何用CSS3美化菜单
category:
  - html-css
tags:
  - CSS3
  - 翻译
---
{% include JB/setup %}

快速提示：为每个菜单项添加图像，并在鼠标悬停时滑出。

![菜单项添加图像][2]

[查看DEMO][2]    [下载源码][3]

在这个叫做“[提示与技巧][4]”的分类中，我们会介绍一些关于Web开发和Web设计的简短有趣的方法。在今天的小提示中，我们会展示如何为菜单添加一个优美的悬停效果。主要的想法就是当鼠标悬停在菜单项时右侧滑出一个图片。

每个菜单项（在本例中是一个无序列表项）会包含一个含有两个 **span** 的锚点和一个图像：

    &lt;ul class=&quot;mh-menu&quot;&gt;
        &lt;li&gt;
            &lt;a href=&quot;#&quot;&gt;
                &lt;span&gt;Art Director&lt;/span&gt;
                &lt;span&gt;Henry James&lt;/span&gt;
            &lt;/a&gt;
            &lt;img src=&quot;images/1.jpg&quot; alt=&quot;image01&quot;/&gt;
        &lt;/li&gt;
        &lt;!-- ... --&gt;
    &lt;/ul&gt;

我们会把 **.mh-menu li a** 设置成 **display:block** 并将背景设置为 **rgba(255,255,255, 0.8)** ，当鼠标悬停到列表项时，我们再把背景色设置成浅蓝色 **rgba(225,239,240, 0.4)**：

    .mh-menu li:hover a{
        background: rgba(225,239,240, 0.4);
    }

第二个 **span** 在鼠标悬停时也会改变背景色，但我们希望不同的列表项能够显示不同的颜色，因此我们首先添加一个颜色渐变，然后使用 **nth-child** 选择器获取元素（[有用的:nth-child秘方][5]  ——[译者][6]注。）：

    .mh-menu li a span:nth-child(2){
        /*...*/
        transition: color 0.2s linear;
    }
    .mh-menu li:nth-child(1):hover span:nth-child(2){
        color: #ae3637;
    }
    .mh-menu li:nth-child(2):hover span:nth-child(2){
        color: #c3d243;
    }
    .mh-menu li:nth-child(3):hover span:nth-child(2){
        color: #d38439;
    }
    .mh-menu li:nth-child(4):hover span:nth-child(2){
        color: #8e7463;
    }

由于图像要滑到右侧，因此，首先它的 **left** 值必须是 **0px**，同时我们也为它的透明度添加一个渐变，它会从初始值 **0** 渐变到 **1** ：

    .mh-menu li img{
        position: absolute;
        z-index: 1;
        left: 0px;
        top: 0px;
        opacity: 0;
        transition: left 0.4s ease-in-out, opacity 0.6s ease-in-out;
    }
    .mh-menu li:hover img{
        left: 300px;
        opacity: 1;
    }

瞧，这样我们就有了一个很棒的滑出的效果了！

注意要确保锚点的 z-index 值比图像高一点儿，这样图像才会在锚点的下方滑出而不是在它的上面。

另外，我们还可以使背景色在悬浮的时候不透明，例如完全是白色（[demo 2][7]），或者为每个子元素设置不同的颜色（[demo 3][8]）。

本文中的演示由  [Bartosz Kosowski][9]  ([CC BY-NC 3.0][10]) 提供。

[查看DEMO][2]    [下载源码][3]

原文地址：[http://tympanus.net/codrops/2012/01/22/how-to-spice-up-your-menu-with-css3/][11]

 [1]: http://jiguang.github.com/content/uploads/2012/01/CSS3MenuHoverEffect.jpg
 [2]: http://jiguang.github.com/go.php?url=http://tympanus.net/TipsTricks/CSS3MenuHoverEffect/
 [3]: http://tympanus.net/TipsTricks/CSS3MenuHoverEffect/CSS3MenuHoverEffect.zip
 [4]: http://jiguang.github.com/index.php/category/tips-and-tricks/
 [5]: http://jiguang.github.com/go.php?url=http://www.qianduan.net/useful-nth-child-recipies.html
 [6]: http://jiguang.github.com "译者"
 [7]: http://jiguang.github.com/go.php?url=http://tympanus.net/TipsTricks/CSS3MenuHoverEffect/index2.html
 [8]: http://jiguang.github.com/go.php?url=http://tympanus.net/TipsTricks/CSS3MenuHoverEffect/index3.html
 [9]: http://jiguang.github.com/go.php?url=http://www.bartoszkosowski.com/
 [10]: http://jiguang.github.com/go.php?url=http://creativecommons.org/licenses/by-nc/3.0/
 [11]: http://jiguang.github.com/go.php?url=http://tympanus.net/codrops/2012/01/22/how-to-spice-up-your-menu-with-css3/