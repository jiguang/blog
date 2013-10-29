---
layout: post
title: JavaScript动态插入样式与结构
category:
  - JavaScript
tags: [ ]
---
{% include JB/setup %}

JavaScript动态插入样式有两种情况，一种为插入link元素用于包含外部文件，另一种为插入style元素用于指定嵌入样式。

第一种情况很简单，假设要在文档中插入如下元素：

    &lt;link rel=&quot;stylesheet&quot; type=&quot;text/css&quot; href=&quot;style.css&quot; /&gt;

那么使用这样的代码即可：

    var link = document.getElementById("link");
    link.rel = "stylesheet";
    link.type = "text/css";
    link.href = "style.css";
    var head = document.getElementByTagName("head")[0];
    head.appendChild(link);

不过要注意，必须将link元素添加到head而不是body中，才能保证在所有浏览器中行为一致。可以将这段代码封装为以下函数：

    function loadStyle(url){
    	var link = document.getElementById("link");
    	link.rel = "stylesheet";
    	link.type = "text/css";
    	link.href = url;
    	var head = document.getElementByTagName("head")[0];
    	head.appendChild(link);
    }

另一种动态插入样式的方式是使用style元素：

    &lt;style type=&quot;text/css&quot; &gt;
    body{background-color: red}
    &lt;/style&gt;

如果按照相同的逻辑，下面的代码应该是有效的：

    var style = document.createElement("style");
    style.type = "text/css";
    style.appendChild(document.createTextNode("body{background-color: red}"));
    var head = document.getElementsByTagName("head")[0];
    head.appendChild(style);

不过事实上，这段代码只能在Firefox、Safari、Chrome和Opera中运行，在IE中则会报错。IE会将style当做一个特殊的节点，不允许访问其子节点。解决这个问题的方法就是访问元素的styleSheet属性，该属性又有一个cssText属性，可以接受CSS代码。代码如下：

    var style = document.createElement("style");
    style.type = "text/css";
    try{
    	style.appendChild(document.createTextNode("body{background-color:red} "));
    } catch(ex){
    	style.styleSheet.cssText = "body{background-color:red} ";
    }
    var head = document.getElementByTagName("head")[0];
    head.appendChild(style);

整理成通用的函数则为：

    function loadStyleString(css){
    	var style = document.createElement("style");
    	style.type = "text/css";
    	try{
    		style.appendChild(document.createTextNode(css));
    	} catch(ex){
    		style.styleSheet.cssText = css;
    	}
    	var head = document.getElementByTagName("head")[0];
    	head.appendChild(style);
    }

那么，上面介绍的这些都是只插入样式的情况，如果样式和结构一起插入的话会有发生什么情况呢？例如，我们需要向页面中插入如下代码：

    &lt;div class=&quot;foo&quot;&gt;This is a test&lt;/div&gt;

我们可能首先会想到innerHTML，直接将整段代码作为某个容器的innerHTML插入，但是你会发现在所有现代浏览器中都没问题，但是在IE8以下样式没有生效。这应该是浏览器渲染方式的问题，只有使用JavaScript异步加载样式和结构时才会出现，如果由后台吐出就不会有这个问题。如果想了解更多浏览器的工作原理，请看Ghost的这篇[《浏览器工作原理浅析》][1]。

打开调试工具可以发现在dom结构中并没有生成style标签。如前所示，这一步会有兼容性问题，可以按照前面的代码进行处理，将style元素添加到head中即可解决IE中这个问题。

    // 方案1：单独提取style将其插入到head中
    code.replace(regExp, function(match, cssText){
    
    var head = document.getElementsByTagName('head')[0],
    	style = document.createElement('style'),
    	rules = document.createTextNode(cssText);
    
    	style.type = 'text/css';
    	if(style.styleSheet)
    		style.styleSheet.cssText = rules.nodeValue;
    	else
    		style.appendChild(rules);
    	head.appendChild(style);
    
    	return '';
    });

另外，经过测试[笔者][2]发现还有另外一种解决方案，即将style元素放到dom结构下方，然后就可以直接使用innerHTML了。那么我们可以写一段脚本，将代码中的style部分提取出来，并将其挪到dom结构的下方：

    var regExp = /<style([\s\S]*?)</style>/ig;
    
    // 方案2：将style放到代码底部
    if(code.match(regExp)[0]){
    	code = code.replace(regExp,'') + code.match(regExp)[0];
    	document.getElementById(container).innerHTML = code;
    }

那么如何才能知道innerHTML操作的有没有成功呢？如果你使用类库的话，可以检测IE浏览器版本小于等于8。当然，还有更简洁一点的办法，如果innerHTML插入style成功了，则在dom结构中会多出一个style节点。那么我们可以计算插入前后页面中style节点的个数来判断是否成功。可以通过以下一个简单的函数来计算style节点个数：

    // 计算页面内style节点的数目
    function countStyleSheets(){
    	return document.getElementsByTagName('style').length;
    }

然后在执行innerHTML插入操作后再比较一下：

    if(countStyleSheets() === original){}

这样就可以根据判断结果来决定是否需要额外的操作，在需要使用JavaScript同时插入style和dom时可以采用这种方式。

[查看DEMO][3]

 [1]: http://www.cssforest.org/blog/index.php?id=195
 [2]: http://jiguang.github.com "笔者"
 [3]: http://jiguang.github.com/demo/apply-custom-code.html