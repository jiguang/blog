---
layout: post
title: JavaScript随机排序的一种实现
category:
  - JavaScript
tags: [ ]
---
{% include JB/setup %}

开发中经常会用到的dom元素随机排序的一种实现方式，并附有简单例子，代码有注释，很容易懂的。

DEMO截图：

![Screenshot][1]

    <h1>Random Card</h1>
    <ul id="container">
        <li class="pk" style="background:red;">Red</li>
        <li class="pk" style="background:green;">Green</li>
        <li class="pk" style="background:pink;">Pink</li>
        <li class="pk" style="background:purple;">Purple</li>
        <li class="pk" style="background:blue;">Blue</li>
        <li class="pk" style="background:orange;">Orange</li>
    </ul>
    
    <button id="shuffle">开始随机洗牌</button>

    function shuffle(){
        // 获取列表项
        var temp = document.getElementsByTagName('li');
    
        // 创建新数组用来保存列表项
        var list = new Array();
    
        // 为每个列表项分配一个随机数属性，并将其存入新数组
        for(var i=0, j=temp.length; i<j; i++){
            temp[i].order = Math.random();
            list.push(temp[i]);
        }
    
        // 对新数组按其随机数属性进行排序
        list = list.sort(function(a,b){
            return a.order - b.order;
        });
    
        var container = document.getElementById('container');
        container.style.display = 'none'; // offline
        container.innerHTML = '';
    
        for(; list.length>0; ){
            container.appendChild(list.pop());
        }
    
        container.style.display = 'block';
    }
    
    document.getElementById('shuffle').onclick = function(){
        shuffle();
    };

[点击这里查看DEMO][2]

补充1：经wikixie同学提醒，随机的代码也可改为：

    list = list.sort(function(a,b){
            return Math.random() - Math.random();
    });

补充2：经[猫哥][3]提醒，补充1所示代码有可能导致概率不等。具体原因为：各个浏览器对sort()方法实现方式不同，使用两次随机差值进行比较，可能导致各项机会不均等。具体大家可以深入了解JavaScript中sort()的底层实现方式。

这里是[WebKit源码][4]中的实现方式，有兴趣同学可以研究一下：

    // "Min" sort. Not the fastest, but definitely less code than heapsort
        // or quicksort, and much less swapping than bubblesort/insertionsort.
        for (unsigned i = 0; i < length - 1; ++i) {
            JSValue iObj = thisObj->get(exec, i);
            if (exec->hadException())
                return JSValue::encode(jsUndefined());
            unsigned themin = i;
            JSValue minObj = iObj;
            for (unsigned j = i + 1; j < length; ++j) {
                JSValue jObj = thisObj->get(exec, j);
                if (exec->hadException())
                    return JSValue::encode(jsUndefined());
                double compareResult;
                if (jObj.isUndefined())
                    compareResult = 1; // don't check minObj because there's no need to differentiate == (0) from > (1)
                else if (minObj.isUndefined())
                    compareResult = -1;
                else if (callType != CallTypeNone) {
                    MarkedArgumentBuffer l;
                    l.append(jObj);
                    l.append(minObj);
                    compareResult = call(exec, function, callType, callData, jsUndefined(), l).toNumber(exec);
                } else
                    compareResult = (jObj.toUStringInline(exec) < minObj.toUStringInline(exec)) ? -1 : 1;
    
                if (compareResult < 0) {
                    themin = j;
                    minObj = jObj;
                }
            }
            // Swap themin and i
            if (themin > i) {
                thisObj->methodTable()->putByIndex(thisObj, exec, i, minObj, true);
                if (exec->hadException())
                    return JSValue::encode(jsUndefined());
                thisObj->methodTable()->putByIndex(thisObj, exec, themin, iObj, true);
                if (exec->hadException())
                    return JSValue::encode(jsUndefined());
            }
        }

 [1]: http://jiguang.github.com/content/uploads/2012/04/screenshot.png
 [2]: http://jiguang.github.com/demo/random-sort.html
 [3]: http://yekai.net
 [4]: http://svn.webkit.org/repository/webkit/trunk/Source/JavaScriptCore/runtime/ArrayPrototype.cpp