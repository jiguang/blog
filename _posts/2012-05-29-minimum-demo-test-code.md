---
layout: post
title: 最小化DEMO测试代码
category:
  - JavaScript
tags:
  - demo
---
{% include JB/setup %}

在制作页面Demo的时候，为了方便测试效果，经常要写些测试代码。对于常用的功能可以封装成函数直接使用，或者先引入个jQuery类库，待进入开发阶段时再删除。

本文只提供了几个代码片段，可以直接放在页面中测试Demo，目前只包括显示隐藏和简单的Tab，后续会继续整理更多简化的Demo测试代码。

当然，本文中号称“最简”也不一定是最简，可能还有更简单的写法，只是超出了[笔者][1]的能力范围了，还望路过大侠不吝赐教：）

最简添加删除class：

    // for test
    function $(id){ return document.getElementById(id); }
    
    // add or remove class
    $('test1').onclick = function(){
        $('test2').className = /\s*on$/.test($('test2').className) ? 'test_class' : 'test_class on';
    };

最简显示隐藏：

    // for test
    function $(id){ return document.getElementById(id); }
    
    // display none or block
    $('test1').onclick = function(){
        $('test2').style.display = $('test2').style.display == 'none' ? 'block' : 'none';
    };

最简Tab切换：

    <style>
            ul li{cursor: pointer;}
            div{display: none;}
            .on{border: 1px solid #ddd;}
            .on div{display: block;}
    </style>
    
    <ul id="x">
        <li>Tab1<div>content1</div></li>
        <li>Tab2<div>content2</div></li>
        <li>Tab3<div>content3</div></li>
    </ul>
    
    <script type="text/javascript">
    document.getElementById('x').onclick = function(e){
        var w=window.event,t=(e||w).target||w.srcElement,c='className',x=this.x;
        x?x[c]='':0;this.x=t;t[c]='on';
    };
    </script>



目前只整理了这几个，也是我平时用到最多的，欢迎补充！

 [1]: http://jiguang.github.com "笔者"