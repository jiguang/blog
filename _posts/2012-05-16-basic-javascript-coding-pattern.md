---
layout: post
title: JavaScript基本编码模式
category:
  - JavaScript
tags: [ ]
---
{% include JB/setup %}

无论编写任何程序都会接触到编码风格，设计模式等概念，编码风格一般侧重于书写规范，而设计模式则偏向于程序架构设计。本文中[笔者][1]整理的这些“模式”包含了编写JavaScript代码时一些常用的方法或者小技巧，可以帮助初学JavaScript的同学迅速提升代码质量。当然，在此之前首先要保证规范的书写习惯，在此之上可以再将本文介绍的知识点运用到代码编写中，则可以大大提升代码质量。

下面是[笔者][1]整理的一些点，没有什么逻辑顺序，想到哪儿写到哪儿了，不足之处欢迎大家补充指正。

1. 变量定义

    // 一般写法
    var a = 0;
    var b = 1;
    var c = 'xxx';
    
    // 推荐写法
    var a = 0,
        b = 1,
        c = 'xxx';

2. 尽量使用字面量

    // 一般写法
    var obj = new Object();
            obj.a = 'aa';
            obj.b = 'bb';
            obj.c = 'cc';
    
    var arr = new Array();
    
    // 推荐写法
    var obj = {
            a: 'aa',
            b: 'bb'
        };
    
    var arr = [];
    
    function getXX(index){
        return ['aa', 'bb', 'xx', 55, 'xxb'](index);
    }
    
    function getMessage(code){
        return {
            404: 'xxx',
            500: 'xxx'
        }[code];
    }

3. 正则字面量

    var regex = new RegExp('someting');
    
    // 当正则表达式可能变化时才使用构造函数
    var cls = 'someclass',
        regex = new RegExp(cls + '\\s*', 'ig');   // only for dynamic regexs
    
    // 其他情况均使用字面量
    var regex = /someting/ig;

4. 设置默认值

    // Default values
    var arg = arg || 'default';  // fallback
    
    document.getElementById('test').onclick = function(event){
        var event = event || window.event;
    };
    
    function getX(a){
        return a+1 || 'default';
    }
    

5. 条件判断

    // Conditions
    answer = obj &#038;&#038; obj.xx &#038;&#038; obj.xx.xxx;
    
    // 连续判断
    if(obj &#038;&#038; obj.xx &#038;&#038; obj.xx.xxx){
        // do something
    }
    
    if(obj.xx){
        // do something
    }
    
    if(!obj){
        // do something
    }
    
    // 使用全等判断
    if(a === b){
        // do something
    }
    
    // 尽量不检测浏览器，仅检测要使用的特性是否支持
    if(document.getElementById){
        // ability detect
    }
    

6. 三元操作符

    // Ternary
    check ? value1 : value2;
    
    // 三元操作符更简洁
    var foo = (condition) ? value1 : value2;
    
    function xx(){
        if(condition){
            return value1;
        }else{
            return value2;
        }
    }
    
    function xx(){
        return (condition) ? value1 : value2;
    }
    
    // 格式化三元操作符
    foo = predicate ? "one" :
          predicate ? "two" :
                      "default";   // format

7. 插入迭代值

    // Insert iteration
    var name = value[i];
        i++;
    
    // 直接将迭代值插入
    var name = value[i++];

8. DOM操作

    // DOM Operation
    el.style.display = 'none'; // offline
        // operation
    el.style.display = 'block';
    
    // 使用文档碎片操作更好
    var fragment = document.createDocumentFragment();  // better
    
    el.innerHTML = '';  // fast remove all children, but may leaks memory
    el.innerHTML = 'xxx';  // ok, use it!
    
    // 小心处理NodeList
    var images = document.getElementsByTagName('img');  // be careful! dynamic list

9. 事件代理

    // 使用事件代理，在更外层的元素上监听事件
    document.getElementById('list').onclick = function(evt){
        var evt = evt || window.event,
            target = evt.target || evt.srcElement;
    
        if(target.id === 'btn1'){
            // do something
        }
    }

10. 命名空间

    // An Object as a Namespace
    var MYAPP = {};
        MYAPP.dom.get = function(id){};
        MYAPP.style.css = function(el, style){};
    
    MYAPP.namespace('event');

11. 链式操作

    // Chaining operation: return this
    function setValue(el, value){
        el.value = value;
        return this;
    }
    
    var obj = new MYAPP.dom.Element('span');
        obj.setText('hello')
        .setStyle('color', 'red')
        .setStyle('font', 'Verdana');

12. 私有作用域

    // Function
    (function(){
        var _private = 'cant see me';
    
    })();
    
    (function($){
         $('#xxb').click(function(){ });
    })(jQuery);

13. 配置对象

    // Configure Object
    function foo(id, conf, null , null){
        // do somethin
    }
    
    foo('bar', {
        key1 : 1,
        key2 : 2
    });
    

14. 类型转换

    // Type Conversion
    +'010' === 10;
    Number('010') === 10;
    parseInt('010', 10) === 10;
    10 + '' === '10';
    
    +new Date() // timestamp
    +new Date;
    

15. 扩展原型

    // 仅在需要向前兼容时才使用，其他情况不建议扩展原型对象
    Array.prototype.forEach = function(){
        // only for forward compatible
    };

16. 循环优化

    // 缓存
    for(var i=0, j = document.getElementsByTagName('a').length; i<j; i++){
        // always cache
    }
    
    // 变量提升
    var i = 0, j = xx.length;
    for(; i<j; i++){
        // hoisting
    }
    
    // 始终检测是否自有属性，避免不必要的原型链查找
    for(var i in foo){
        if(foo.hasOwnProperty(i)){
            console.log(i);  // avoid prototype chain searching
        }
    }
    
    // 如果不在乎顺序，可采用逆序循环
    for(var i=xx.length; i>0; i--){
        // maybe faster
    }
    
    // 据说是最快的
    while(i--){
        // maybe fastest
    }

17. 尽量使用新特新

    Array.forEach();
    getElementsByClassName();
    querySlectorAll();
    
    // 首先检测是否支持新特性，能用就用
    if(document.getElementsByClassName){
        // use
    }else{
        // your implementations
    }

18. 惰性载入

    // 只判断一次，再次调用该函数则无需判断
    function lazyDef(){
        if(condition1){
            lazyDef = function(){ };
        }else if(condition2){
            lazyDef = function(){ };
        }
        return lazyDef();
    }

19. 私有函数与公共方法

    var MYAPP = {};
    
    MYAPP.dom = (function(){
        var _setStyle = function(el, prop, value){
            console.log('setStyle');
        };
    
        return {
            setStyle: _setStyle
        };
    })();
    
    // 当 MYAPP.dom.setStyle 不慎被覆写时，_setStyle在内部仍然可用

20. 调试

    // 尽量使用，可以传入多个参数，最后输出拼接后的字符串
    console.log('xx','xx','...');
    console.dir(someObj);
    console.dirxml(someDom);
    console.time('timer');
    console.warn('xxx');
    
    // 封装可以保证不小心发布出去也不会导致问题，但报错时行号可能有问题
    function msg(msg){
        if(console &#038;&#038; console.log){
            console.log(msg);   // wrong line number
        }
    }

基本上目前想到的只有这些，欢迎大家补充讨论：）

 [1]: http://jiguang.github.com "笔者"