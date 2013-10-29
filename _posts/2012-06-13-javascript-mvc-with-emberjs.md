---
layout: post
title: JavaScript MVC with Ember.js
category:
  - JavaScript
tags:
  - Ember.js
  - MVC
  - 教程
---
{% include JB/setup %}

最近两年 JavaScript 的 MVC 框架尤为火爆，JavaScript 应用的开发已经越来越接近传统软件开发了，各种软件工程的思想以及设计模式都逐渐应用在前端开发中。关于 JavaScript 的 MVC 框架，当前最流行的当属[Backbone.js][1]了，它有着强大而活跃的社区支持以及丰富的文档资源，可称得上是当下的王者。但它的缺点也很明显，由于它过于轻量，所以很多功能都只能自己开发，它的轻量既可以说是优点也可以说是缺点，还是看能否满足你的业务需要了。

本文即将介绍的 [Ember.js][2] 算得上是后起的新秀，不过它本身不是个新东西，它的前身是苹果公司出品的 [SproutCore][3]，后来由原 SproutCore Team 的成员开发（或者说重命名）而产生了 Amber.js，再后来又改为 Ember.js。

Ember.js 本身的功能非常强大，直接集成了模板功能，以及其他很多非常有用的功能。那么有的同学会问了，为什么选用Ember.js呢？它好在哪里呢？请大家先看一下InfoQ的这篇文章[《12种JavaScript MVC框架的比较》][4]，看完后可以对 JavaScript MVC 框架的现状有一定了解。网上还有一些专门将 Backbone.js 和 Ember.js 进行比较的文章[《EmberJS: Initial Impressions (Compared To Backbone)》][5]，这说明它们还有的拼。

个人认为，Ember.js 对于刚接触 MVC 的同学比较容易上手一点，抽象的层次更高一些，而且它提供的一些自动更新模板以及数据绑定等功能真的非常好用。缺点就是目前完全没有中文资料，主要是靠官方的文档，也有老外同学写些这方面的文章。不过[笔者][6]相信这个库马上就会崛起，所以在这里给大家做个简单介绍，后续如果时间允许的话，会将 Ember.js 的文档翻译整理出来，方便大家学习。

**什么是MVC**

[MVC][7]模式（三层架构模式）（Model-View-Controller）是[软件工程][8]中的一种[软件架构][9]模式，把软件系统分为三个基本部分：模型（Model）、视图（View）和控制器（Controller）。

**关于Ember.js**

Ember（之前是SproutCore 2.0）是竞争者中的新宠。它是从 SproutCore2.0 中抽取分离其核心特性并转变成一个更加紧凑的模型框架，更加适合web应用的开发。其优点是丰富的模板系统，拥有可编写的视图和UI绑定。缺点是由于太新，相关文档跟不上，目前几乎没有中文资料。

**快速开始**

Ember.js 依赖于[jQuery][10]，要想使用Ember.js需要先引入jQuery。在Ember.js的[官方网站][2]上，有个[Starter-Kit][11]是专门针对初学者的开发包，里面包含了Ember.js所需的文件以及一个html文件，可以在libs/app.js中书写自定义的代码进行调试。

如果是自己新建的目录和文件，那么需要分别下载Ember.js和jQuery的文件然后引入：

{% highlight html %}
    &lt;script src=&quot;js/libs/jquery.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;js/libs/ember-0.9.8.1.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;js/app.js&quot;&gt;&lt;/script&gt;
{% endhighlight %}

引入文件之后，可以使用jQuery的所有方法，结合Ember.js的方法开发大型App了。下面介绍一些Ember.js的基本功能，以窥一斑。

首先，建立任何App都要先声明一个命名空间：

{% highlight javascript %}
    // add a namespace, Em === Ember
    var App = Em.Application.create();
{% endhighlight %}

该命名空间可以保证各个App不发生冲突，同时它也有时间代理的功能，可以捕获所有冒泡的事件。如果你的App只是页面中的一部分，那么也可以指定一个根元素，这样事件就只会冒泡到该根元素：

{% highlight javascript %}
    window.App = Ember.Application.create({
    	rootElement: '#sidebar'
    });
{% endhighlight %}

下面介绍几个简单的例子。

**1. 自动更新模板**

JS中的对象App.president的值fullName已经和视图绑定，当按下按钮更新对象属性时，视图也会自动更新。

{% highlight javascript %}
    &lt;script type=&quot;text/x-handlebars&quot;&gt;
        The President of the United States is {{App.president.fullName}}.
    &lt;/script&gt;
    &lt;p&gt;&lt;button id=&quot;btn1&quot;&gt;TEST&lt;/button&gt;&lt;/p&gt;

    // Example 1
    App.president = Em.Object.create({
        firstName: &quot;Barack&quot;,
        lastName: &quot;Obama&quot;,
        fullName: function() {
            return this.get('firstName') + ' ' + this.get('lastName');
            // Tell Ember that this computed property depends on firstName
            // and lastName
        }.property('firstName', 'lastName')
    });
    $('#btn1').click( function() {
        App.president.set('lastName', 'Obamaomao');
    });
{% endhighlight %}

**2. 延迟使用的模板**

有的时候我们不希望模板在页面载入时立即显示，那么可以把它放到script标签中，并增添data-template-name属性来为模板命名，之后就可以在Ember.js中控制模板的填充和显隐了。

{% highlight javascript %}
    &lt;div id=&quot;temp-wrap&quot; style=&quot;border: 3px solid #c22;width:300px;&quot;&gt;
        &lt;!-- Insert into here --&gt;
    &lt;/div&gt;

    &lt;script type=&quot;text/x-handlebars&quot; data-template-name=&quot;info&quot;&gt;
        User: Albert Hofmann
        &lt;div&gt;
            &lt;b&gt;Posts:&lt;/b&gt; {{posts}}
            &lt;br&gt;
            &lt;b&gt;Hobbies:&lt;/b&gt; {{hobbies}}
        &lt;/div&gt;
    &lt;/script&gt;

    // Example 2
    App.InfoView = Em.View.extend({
        templateName: 'info',
        posts: 25,
        hobbies: &quot;Riding bicycles&quot;
    });
    App.InfoView.create().appendTo('#temp-wrap');
    
    // or
    // var info = App.InfoView.create();
    // info.appendTo('#temp-wrap');
{% endhighlight %}

**3. 绑定（Bindings）**

一个对象中的属性可以与另外一个对象的属性进行绑定，这样其中一个更新时，另外一个也随之更新。只需要使用Binding字符串，将Binding加在绑定的属性字符串末尾即可。

{% highlight javascript %}
    &lt;p&gt;Binding Test: &lt;button id=&quot;btn2&quot;&gt;TEST&lt;/button&gt;&lt;/p&gt;

    // Example 3
    App.country = Ember.Object.create({
        // Ending a property with 'Binding' tells Ember to
        // create a binding to the presidentName property.
        presidentNameBinding: 'App.president.name'
    });
    // Later, after Ember has resolved bindings...
    $('#btn2').click( function() {
        alert(App.country.get('presidentName'));  // &quot;Barack Obama&quot;
    });
{% endhighlight %}

**4. 事件处理**

{% highlight javascript %}
    &lt;script type=&quot;text/x-handlebars&quot;&gt;
    {{#view App.ClickableView}}
        This is a clickable area!
    {{/view}}
    &lt;/script&gt;

    // Example 4
    App.ClickableView = Ember.View.extend({
        click: function(evt) {
            alert(&quot;ClickableView was clicked!&quot;);
        }
    });
{% endhighlight %}

目前暂且介绍这几个，详细的可以查看[官方文档][12]，如果看E文比较有困难的话，敬请关注本站，稍后如时间允许将放出中文文档。

这里有一些相关资料，感兴趣的同学可以继续深入研究一下：

[http://www.adobe.com/devnet/[html5][14]/articles/flame-on-a-beginners-guide-to-emberjs.html][14]
<http://emberjs.com/documentation/>  
<http://www.andymatthews.net/read/2012/03/07/Getting-Started-With-EmberJS>  
<http://ngauthier.com/2012/02/playing-with-ember.html>  
<http://emberjs.tumblr.com/>

下面是关于本文的一个PPT：

**[Java script+mvc+with+emberjs][14]** View more [presentations][15] from [ji guang][16].

这里有本文介绍的代码制成的DEMO：

[查看DEMO][17]

 [1]: http://backbonejs.org/
 [2]: http://emberjs.com/
 [3]: http://sproutcore.com/
 [4]: http://www.infoq.com/cn/news/2012/05/js-mvc-framework
 [5]: http://lostechies.com/derickbailey/2012/02/21/emberjs-initial-impressions-compared-to-backbone/
 [6]: http://jiguang.github.com "笔者"
 [7]: http://zh.wikipedia.org/wiki/MVC
 [8]: http://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6%E5%B7%A5%E7%A8%8B
 [9]: http://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6%E6%9E%B6%E6%9E%84
 [10]: http://jquery.com/
 [11]: https://github.com/downloads/emberjs/starter-kit/starter-kit.0.9.8.1.zip
 [12]: http://emberjs.com/documentation/
 [13]: http://www.adobe.com/devnet/html5/articles/flame-on-a-beginners-guide-to-emberjs.html
 [14]: http://www.slideshare.net/jiguang/java-scriptmvcwithemberjs "Java script+mvc+with+emberjs"
 [15]: http://www.slideshare.net/
 [16]: http://www.slideshare.net/jiguang
 [17]: http://jiguang.github.com/demo/ember/index.html