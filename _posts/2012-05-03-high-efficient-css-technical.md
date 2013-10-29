---
layout: post
title: 高效CSS开发核心要点
category:
  - html-css
tags:
  - 性能优化
  - 技巧
  - 语义化
---
{% include JB/setup %}

本文参考了业界规范及主流趋势，详尽整理了CSS开发中需要注意的要点以及关乎性能的一些问题，希望对您有所帮助，也可收藏作为参考。

**1. 基本原则**

**1.1 把CSS放在HTML页面头部**

由于浏览器需要在所有的样式表加载完成后才能开始渲染页面，样式表加载完成之前页面会一直显示空白，因此需要将样式表放在头部。

`@import` 相当于把 `<link>` 标签放在页面的底部，因此从优化性能的角度考虑，应避免使用 `@import`。

**1.2 避免使用 CSS Expressions**

Expression 只有 IE 支持，而且他的执行比大多数人想象的要频繁的多。不仅页面渲染和改变大小 (resize) 时会执行，页面滚动 (scroll) 时也会执行，甚至连鼠标在页面上滑动时都会执行。在 expression 里面加上一个计数器就会知道，expression 的执行上相当频繁的。鼠标的滚动很容易就会使 expression 的执行次数超过 10000。

**1.3 CSS简写**

1.3.1 16进制颜色值简写

    color: #eebbcc;

    color: #ebc;

1.3.2 属性值简写

     margin-top: 2px;
     margin-right: 5px;
     margin-bottom: 2em;
     margin-left: 15px;     ----->>     margin: 2px 5px 2em 15px; 
    
     border-width: 1px;
     border-style: solid;
     border-color: #000     ----->>     border: 1px solid #000 
    
     font-style: italic;
     font-variant: small-caps;
     font-weight: bold;
     font-size: 1em;
     line-height: 140%;
     font-family: sans-serif;  ----->>  font: italic small-caps bold 1em/140% sans-serief 
    
     background-color: #f00;
     background-image: url(background.gif);
     background-repeat: no-repeat;
     background-attachment: fixed;
     background-position: 0 0;   ----->>background: #f00 url(background.gif) no-repeat fixed 0 0 
    
     list-style-type: square;
     list-style-position: inside;
     list-style-image: url(image.gif)  ----->> list-style: square inside url(image.gif)

**1.4 尽量抽取相似部分**

    .class1{position: absolute; left: 20px; top: 30px;}
    .class2{position: absolute; left: 20px; top: 30px;}
    .class3{position: absolute; left: 20px; top: 30px;}
    .class4{position: absolute; left: 20px; top: 30px;}
    .class5{position: absolute; left: 20px; top: 30px;}
    .class6{position: absolute; left: 20px; top: 30px;} 
    
     -------------------->>>>>>> 
    
     .class1 .class2 .class3 .class4 .class5 .class6{
     	Position: absolute; left: 20px; top: 20px;
     }

[jiguang.github.com版权所有][1]

**2. 关键要点**

**2.1 只使用小写**

    <!-- Not recommended -->
    <A HREF="/">Home</A>
    
    <!-- Recommended -->
    <img src="google.png" alt="Google"/>

**2.2 不要有多余的空格（划线处）**

    <!-- Not recommended -->
    <p>What?_
    
    <!-- Recommended -->
    <p>Yes please.

**2.3 使用utf8编码**

    html中：
    <meta charset="utf-8"> 
    
    css中：
    @charset "utf-8";

**2.4 使用[html5][2]文档类型**

    <!DOCTYPE html>

**2.5 验证HTML与CSS文档**

[验证HTML][3]

[验证CSS][4]

**2.6 使文档语义化**

    <!-- Not recommended -->
    <div onclick="goToRecommendations();">All recommendations</div>
    
    <!-- Recommended -->
    <a href="recommendations/">All recommendations</a>

**2.7 多媒体（多终端）兼容**

    <!-- Not recommended -->
    <img src="spreadsheet.png"/>
    
    <!-- Recommended -->
    <img src="spreadsheet.png" alt="Spreadsheet screenshot."/>

**2.8 不要使用实体引用**

    <!-- Not recommended -->
    The currency symbol for the Euro is “&eur;”.
    
    <!-- Recommended -->
    The currency symbol for the Euro is “€”.

**2.9 使用带有语义的id和class**

    /* Not recommended: meaningless */
    #yee-1901 {}
    
    /* Not recommended: presentational */
    .button-green {}
    .clear {}
    
    /* Recommended: specific */
    #gallery {}
    #login {}
    .video {}
    
    /* Recommended: generic */
    .aux {}
    .alt {}

**2.10 省略零值的单位**

    margin: 0;
    padding: 0;

**2.11 省略起始的零**

    font-size: .8em;

**2.12 尽量避免CSS hacks**

尝试换种解决方案

**2.13 为末尾的声明添加分号**

尽管省略末尾分号可以省略一个字节，但是非常不利于团队维护，得不偿失

    /* Not recommended */
    .test {
      display: block;
      height: 100px
    }
    
    /* Recommended */
    .test {
      display: block;
      height: 100px;
    }

**2.14 选择器的效率**

浏览器是“从右往左”来分析 class 的，对于下面的规则

    #god &gt; li {font-weight: bold}

浏览器会先查找页面上所有的“li”节点，然后再去做进一步的判断：如果它的父节点的 id 为“god”，则匹配成功。由此可知，CSS 选择器的匹配远比我们想象的要慢的多，CSS 的性能问题不容忽视。

**2.15 后代选择器**

    #toc li {font-weight: bold}

这个效率比之前的“child selector”效率更慢，而且要慢很多。浏览器先便利所有的“li”节点，然后步步上溯其父节点，直到 DOM 结构的根节点（document），如果有某个节点的 id 为“toc”，则匹配成功，否则继续查找下一个“li”节点。

**2.16 尽量避免全局选择器**

    [hidden="true"] { ... } /* A universal rule */

这里的匹配规则很明显：查找页面上的所有节点，如果有节点存在“hidden”属性，并且其属性值为“true”，则匹配成功。这是最耗时耗力的匹配，页面上的所有节点都需要进行匹配运算，这种规则应尽量避免。

是用星号也一样

    #god li *

先找到页面上的所有元素，再匹配祖先中包含li的元素，然后在这些元素中再查找父元素的id为god的元素。

因此，对于全局选择器，只建议一种用法：

    * { margin: 0; padding: 0; /* etc. */ }

**2.17 避免tag+id或者class+id**

    button#goButton {...};-----&gt;&gt;#goButton
    .fundation#testIcon {...};-----&gt;&gt;#testIcon

**2.18 关于tag+class**

    button.indented {...}-----&gt;&gt;.button-indented {...}

程序员们经常会给某个 Class 前面加上标签名称（Tag Name），以更精确且快速的定位该节点，但是这样往往效率更差。因为页面上的 class 在全局范围内来讲应该是唯一的，用唯一的 Class 名称来定位一个节点往往比组合定位更加快捷。事实上，这种做法也可以避免由于开发修改页面元素的类型（Tag）而导致的样式失效的情况，做到样式与元素的分离，两者独立维护。[jiguang.github.com版权所有][1]

**2.19 尽量减少规则数量**

可以考虑将层级关系写到一个class中，不过在层级变动时就比较麻烦了

    Span[mailfolder=&quot;true&quot;] &gt; table &gt; tr &gt; td.columnClass {...}

    -------------------&gt;&gt;&gt;&gt;&gt;&gt;&gt;
    
    .span-mailfolder-tbl-tdCol {...}

**2.20 避免过长的class命名**

可以考虑缩写

    ocHeroImage
    ocEmailAddress

如果觉得难以理解，可以加入连字符或注释

    oc-HeroImage
    oc-EmailAddress

尽管有语义化方面的考虑，但命名还是尽量短一些，只要易于辨认即可

    heroImg
    emailAddr

**2.21 文件名中不应有空格**

a) 有空格的文件名会被Google当成两个关键字，可能带来更多的搜索结果，引来更多流量，是好事  
b) 有空格意味着不能省略引号，多了两个字节  
c) 空格会被浏览器自动转换为%20，老的浏览器可能不支持，如果将%20硬编码到URL中则每个实例中都多了两个字符

    input {background: url("/images/shadow background.gif");}

**2.22 省略URI的引号**

    @import url(//www.google.com/css/go.css);

**2.23 尽量避免后代选择器**

    treehead treerow treecell {...} -----&gt;&gt; treehead &gt; treerow &gt; treecell {...}

Descendant 选择器是耗时相对高的选择器，通常来讲，它在 CSS 里的使用应该是尽量避免的，如果能用 child 选择器替代就应该尽量这样去做。

**2.24 充分利用继承机制**

    Color
     font
     letter-spacing
     line-height
     list-style
     text-align
     text-indent
     text-transform
     white-space
     word-spacing 
    
     #bookmark  &gt; .menu-left {list-style-image: url(blah)}

     ------------&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;
    
     #bookmark  {list-style-image: url(blah)}

**2.25 发布之前一定要进行压缩**

可使用[YUI Compressor][5]或类似软件进行压缩后再发布。

[jiguang.github.com版权所有][1]

**3. 高级技巧**

**3.1 省略嵌入资源的协议**

即可避免混合内容问题（mixed content issues）也可以缩减文件大小

    &lt;!-- Not recommended --&gt;
    &lt;script src=&quot;http://www.google.com/js/gweb/analytics/autotrack.js&quot;&gt;&lt;/script&gt;

    &lt;!-- Recommended --&gt;

    &lt;script src=&quot;//www.google.com/js/gweb/analytics/autotrack.js&quot;&gt;&lt;/script&gt;
    /* Not recommended */
    .example {
      background: url(http://www.google.com/images/example);
    }
    
    /* Recommended */
    .example {
      background: url(//www.google.com/images/example);
    }

**3.2 省略可选的标签**

[HTML5规范][6]中指定了一些可以省略的标签，可以缩减文件大小

    &lt;!-- Not recommended --&gt;
    &lt;!DOCTYPE html&gt;
    &lt;html&gt;
      &lt;head&gt;
        &lt;title&gt;Spending money, spending bytes&lt;/title&gt;
      &lt;/head&gt;
      &lt;body&gt;
        &lt;p&gt;Sic.&lt;/p&gt;
      &lt;/body&gt;
    &lt;/html&gt;

    &lt;!-- Recommended --&gt;
    &lt;!DOCTYPE html&gt;
    &lt;title&gt;Saving money, saving bytes&lt;/title&gt;
    &lt;p&gt;Qed.

**3.3 文件结构**

由于图像等资源一般只有CSS文件使用，故可将图像文件夹放到CSS文件同级目录，这样就可使用相对路径，节省字节数

    input {background: url("images/shadow background.gif");}

**3.4 文件夹命名**

通常文件夹会被命名为其所代表的资源的复数形式

    images
    assets
    fonts

其实大可不必，使用单数形式可以节省许多字节，尤其是当每个项目的结构都类似时

    img
    asset
    font

对于下面一段包含58字节的代码：

    input {background: url("/images/shadow background.gif");}

优化后变为52字节，即10%的压缩：

    input {background: url(img/shadow-background.gif);}

如果在使用缩写，则可进一步缩减：

    input {background: url(img/shadow-bg.gif);}

如果一个项目有几百行这样的代码，那么就会节省几百字节了。如果站点访问量超大，则可节省数目客观的带宽资源。

**3.5 末尾声明的分号**

前面提过，去掉末尾声明的分号可以节省字节，但不利于维护。可以考虑在压缩发布阶段去掉。

    .clear {clear:both;}
    
    .clear {clear:both}

**3.6 背景色简写**

背景色简写也可节省字节，但要慎用，因为省略掉的默认属性会覆盖前面的属性。

    background-color: blue;
    
    background: blue;

**3.7 滤镜简写**

    selector {
    	-ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=65)";
    	filter: progid:DXImageTransform.Microsoft.Alpha(Opacity=65);
    }

filter的版本是IE8以下的，–ms-filter的版本是IE9以上的，YUI会将其压缩为：

    selector {
    	-ms-filter:"alpha(opacity=65)";
    	filter:alpha(opacity=65);
    }

由于YUI的广泛使用极其社区开发着的强大力量，说明这种写法已经是经过深度测试的写法，可以放心使用。

**3.8 Gzip压缩与CSS书写**

[Goolge建议][7]按照字母顺序书写CSS规则，方便维护。其他公司也有自己的规范。从Gzip压缩角度，只要整个文档中的书写顺序保持一致即可，可以提高Gzip压缩比率。

    background: fuchsia;
    border: 1px solid;
    -moz-border-radius: 4px;
    -webkit-border-radius: 4px;
    border-radius: 4px;
    color: black;
    text-align: center;
    text-indent: 2em;

**3.9 更少的请求比更小的尺寸重要**

文件可以适当的合并，只保留基本的层级即可

    &lt;link rel=&quot;stylesheet&quot; href=&quot;http://css.somedomain.com/reset.css&quot; type=&quot;text/css&quot; /&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;http://css.somedomain.com/global.css&quot; type=&quot;text/css&quot; /&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;http://css.somedomain.com/home.css&quot; type=&quot;text/css&quot; /&gt;

[jiguang.github.com版权所有][1]

**参考资料：**

<http://en.wikipedia.org/wiki/Css>

<http://www.w3.org/TR/CSS21/>

[http://www.ibm.com/developerworks/cn/web/1109\_zhouxiang\_optcss/][8]

<http://www.w3.org/TR/css3-color/#html4>

<http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml>

<http://www.w3.org/International/tutorials/tutorial-char-enc/en/all.html>

[Pro CSS for High Traffic Websites][9]

[http://www.ibm.com/developerworks/cn/web/1009\_chengfu\_dojocss/][10]

 [1]: http://jiguang.github.com "From jiguang.github.com"
 [2]: http://jiguang.github.com/index.php/tag/html5/ "html5"
 [3]: http://validator.w3.org/nu/
 [4]: http://jigsaw.w3.org/css-validator/
 [5]: http://yuilibrary.com/projects/yuicompressor
 [6]: http://www.whatwg.org/specs/web-apps/current-work/multipage/syntax.html#syntax-tag-omission
 [7]: http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml
 [8]: http://www.ibm.com/developerworks/cn/web/1109_zhouxiang_optcss/
 [9]: http://www.amazon.com/Pro-CSS-High-Traffic-Websites/dp/1430232889/ref=sr_1_1?ie=UTF8&qid=1336017836&sr=8-1
 [10]: http://www.ibm.com/developerworks/cn/web/1009_chengfu_dojocss/