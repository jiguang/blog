---
title: JavaScript实现可定制图片的进度条
layout: post
category:
  - JavaScript
tags: [ ]
---
{% include JB/setup %}

目前关于js进度条的应用，多数都是动态载入，或者纯js+css生成的朴素的进度条。有些时候我们需要在页面中展示一个静态的进度即可，比如网盘剩余空间等等，此时最好只给定一个进度的参数，便可生成全部的进度条。为了美观考虑，[笔者][1]尝试采用图片实现进度条，这种进度条的效果依赖于图片的样式和定制的参数，可以很方便地修改进度条的样式。在展示进度时，只需要给相应的html标签里加一个参数即可。下面是基于YUI实现的代码：

    var D=YAHOO.util.Dom, E=YAHOO.util.Event;
    
    //可定制的进度条初始化函数，长度可任意，高度依赖背景图片能展示的高度范围
    var initProcess=function( classname, width, height, imgurl){
        if(!classname)return;
    
        //处理误传入单位的情况
        if(width||height){
            width=width.replace('px','');
            height=height.replace('px','');
        }
    
        //处理未定义宽高的情况
        width=width||100;
        height=height||20;
    
        //获取所有进度条容器
        var containers=D.getElementsByClassName(classname);
        if(containers){
    
            //设置容器基本参数
            D.setStyle(containers,'display','none');
            D.setStyle(containers,'width', width+'px');
            D.setStyle(containers,'height', height+'px');
    
            //初始化进度
            for(vari=0, j=containers.length; i<j; i++){
                var inner=document.createElement('div');
    
                D.setStyle(inner,'background','url('+imgurl+') repeat 0 50% #f50');
    
                if(D.getAttribute(containers[i],'data-value')){
                    var value=parseInt(D.getAttribute(containers[i],'data-value'));
    
                    if(value&&value>=0&&value<=100){
                        D.setStyle(inner,'width', (width/100)*value + 'px');
                        D.setStyle(inner,'height', height+'px');
                    }
                }
                containers[i].appendChild(inner);
            }
            D.setStyle(containers,'display','block');
        }
    };
    
    E.onDOMReady(function(){
        //修改参数可以不同样式展示进度条
        initProcess('process-bar','200','20','bar.png');
    });

这个例子只是千万种解决方案中的一种，根据你的情况可以酌情使用。

 [1]: http://44ux.com "笔者"