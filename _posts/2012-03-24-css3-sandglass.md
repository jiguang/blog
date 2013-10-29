---
layout: post
title: CSS3实现倒计时沙漏
category:
  - html-css
tags:
  - CSS3
---
{% include JB/setup %}

这是一个[css3][1]实现的小沙漏，没什么实际意义，就是玩一玩，用到了CSS3的两个属性：transform 和 transition。为了简单起见，代码中没有把带所有浏览器前缀的代码都写出来，只写了-webkit-的。

先看一下效果图：

![Sandglass][2]

[查看DEMO][3]

下面是具体代码，没什么难度：

    &lt;!DOCTYPE html&gt;
    &lt;html&gt;
    &lt;head&gt;
        &lt;title&gt;CSS3 Sandglass&lt;/title&gt;

    &lt;/head&gt;
    &lt;body&gt;
    
    ## CSS3 10秒倒计时沙漏 - Webkit only @jiguang.github.com
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    </body>
    </html>

鼠标hover上去触发，没有使用JS，修改倒计时的时间只需要改掉 -webkit-transition 中的两个10s即可。目前由于鼠标移出后会恢复原来的状态，所以会有点儿奇怪，感觉像是水被抽回去的效果。点击这里[查看DEMO][3]。

 [1]: http://jiguang.github.com/index.php/tag/css3/ "css3"
 [2]: http://jiguang.github.com/content/uploads/2012/03/sandglass.png "sandglass.png"
 [3]: http://jiguang.github.com/demo/css3-sandglass.html