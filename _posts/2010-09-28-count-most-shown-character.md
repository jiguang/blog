---
title: JavaScript笔试题：统计出现最多的字符次数
layout: post
category:
  - JavaScript
tags: [ ]
---
{% include JB/setup %}

一小段代码，经常出现在面试笔试题中的：统计一个字符串中出现最多的字符的次数，可以是英文或者数字。

应用正则表达式的全局匹配，可以匹配出字符出现的次数，比较这些次数，将最大的保存并返回。代码如下：

    var countMost = function(str){
        if(!str) return;
    
        var _count = 0, _temp = 0, _reg, _char;
    
        for(var i = 0; i<str.length; i++){
            _reg = new RegExp(str.charAt(i),'g');
            _temp = str.match(_reg).length;
    
            if(_temp > _count){
                _count = _temp;
                _char = str.charAt(i);
            }
        }
    
        return_count;
        //return {count:_count, char:_char};
    };

注释掉的代码可以一并返回出现最多次数的字符。