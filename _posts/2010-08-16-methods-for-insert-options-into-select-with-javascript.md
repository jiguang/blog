---
title: JavaScript动态添加Option的几种方式
layout: post
category:
  - JavaScript
tags: [ ]
---
{% include JB/setup %}

在处理表单的时候，经常会有这样的需求：给定一定的数据来生成某个select的option，或者更进一步，某些option或许预先选中或者有高亮显示。

下面我们就来温习一下几种option的创建方式。这个需求要求某些选项要预先有高亮显示，可以通过添加class来处理。

首先定义一个高亮的class，为了简单处理，只定义红色高亮：

    <style type="text/css">
    .hot-games{
        background-color: #f00;
    }
    </style>

然后预先定义一些用来生成选项的数据：

    <script type="text/javascript">
    var gameOptions = {
        "options": [
            {
                "name":"魔兽世界",
                "value":"50007361",
                "isHot":"false"},
            {
                "name":"龙之谷",
                "value":"50007937",
                "isHot":"false"},
            {
                "name":"永恒之塔",
                "value":"50007385",
                "isHot":"true"},
            {
                "name":"奇迹世界",
                "value":"50007800",
                "isHot":"false"}
        ]
    };
    </script>

这里是select元素，需要制定id和name属性：

    &lt;select id="cat" name="cat" style="width:130px;"&gt;&lt;/select&gt;

大致有三种方案可以增加option，一种是通过new Option() 构造函数，第二种是通过select.add()函数，第三种是通过增加innerHTML。下面比较一下三种方式的异同：

首先获取到select元素：

    var gameCat = document.getElementById('cat');

第一种方案，通过new Option()构造函数，该函数属0级DOM，所有浏览器都支持：

    //方案1:new Option()构造函数
    for(var i = 0, j = gameOptions.options.length; i<j; i++){
        gameCat.options[i] = new Option(gameOptions.options[i].name, gameOptions.options[i].value);
    
        if(gameOptions.options[i].isHot == 'true'){
            gameCat.options[i].className = 'hot-games';
        }
    }

new Option(text, value)第一个参数为显示的文字，第二个为value值，如果想设置其他属性需要单独操作。

第二种方案，通过document.createElement（）来创建选项，然后再设置选项的属性。在这里w3c标准是可以设置option的 label属性即为显示文本，但事实证明FF并不支持，只是给option增加了一个label属性。而w3c中定义的readonly的属性text，在FF中却可以设置。那么是否需要判断浏览器类型呢？当然不用，该方案代码如下：

    //方案2:add()
    for(var i = 0, j = gameOptions.options.length; i<j; i++) {
        var option = document.createElement('option');
    
        try{
            //二级DOM中该属性为readonly，但FF确可写，且可显示为选项内容
            option.text = gameOptions.options[i].name;
        }catch(e){
            //IE支持label，可以直接显示为选项的文字
            option.label = gameOptions.options[i].name;
        }
    
        option.value = gameOptions.options[i].value;
        option.className = (gameOptions.options[i].isHot=='true')?'hot-games':'';
    
        //如果不传第二个参数，FF下会报错
        gameCat.add(option,null);
    }

这里要注意的add()函数的第二个参数，该参数为before，可以指定选项插到哪个选项之前，如果为null则插到最后。如果不指定这个参数在IE系不会有问题，FF下会报错，提示Not enough arguments,参数不足，所以最好传个null先。

第三种方案，比较少用于select元素上，IE下是无效的，但是动态添加其他元素的孩子时经常用到。当然也可以设置元素属性后 appendChild(),同方案二。这里要先把select隐藏掉，这样可以减少浏览器重绘次数，对于display=’none’的元素的操作不会引起重绘或回流。再改变完之后再显示出来：

    //方案3：innerHTML
    //IE无效，FF可用
    gameCat.style.display = 'none';
    for(vari=0, j = gameOptions.options.length; i<j; i++) {
        var isHot = (gameOptions.options[i].isHot == 'true')?'':'';
        gameCat.innerHTML += '&lt;option value=' + gameOptions.options[i].value + isHot + '&gt;' + gameOptions.options[i].name + '&lt;/option&gt;';
    }
    gameCat.style.display = 'block';

这三种方式最常用的就是第一种了，没有兼容性问题，其他两种只是在此作为例子，展示创建option的方法。