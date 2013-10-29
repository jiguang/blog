---
layout: post
title: 消极接近（Negative Proximity）
category:
  - html-css
tags:
  - 翻译
---
{% include JB/setup %}

CSS的后代选择器有个很细微的特性可能很多人都不曾注意到，因为它很少出现：该选择器没有元素接近（element proximity）的概念。下面是关于这个原则的一个经典范例：

    body h1 {color: red;}
    html h1 {color: green;}

给定这样的样式后，所有的h1将会是绿色而不是红色。这是因为两个选择器具有相同的特殊性（specificity），因此最后一个获胜。而body元素在文档中比html元素更接近h1的事实完全被忽略了。CSS没有衡量文档树中元素接近程度的机制，如果让我打赌的话，我会赌永远都不会有这种机制。

我之所以提起这个是因为，当使用否定伪类（[negation pseudo-class][1]）时它可能会给你带来麻烦。考虑下面的代码：

    div:not(.one) p {font-weight: bold;}
    div.one p {font-weight: normal;}
    
    &lt;div class=&quot;one&quot;&gt;
      &lt;div class=&quot;two&quot;&gt;
        &lt;p&gt;Hi there!&lt;/p&gt;
      &lt;/div&gt;
    &lt;/div&gt;

给定这样的样式，段落（p）并**不会**被加粗。这是因为两条规则都可以匹配，因此最后一个会赢。段落内的文本将会是正常的粗细。

“啊！”你哭了…“可是第一个规则具有更高的特殊性，所以不用管什么顺序它也能赢啊！”你是不是这么想的？但结果却是，否定伪类并不算做一个伪类，它就像通用选择器（univseral selector）一样，根本不会贡献任何特殊性：

> 否定选择器中包含的选择器同其它的选择器计算方式相同，但否定本身并不算作伪类。
> 
> ——Selectors Level 3, section 9: [Calculating a selector’s specificity][2]

如果交换两条规则的顺序，那么就可以得到加粗的段落了，因为“其他方面相等的规则，后出现的获胜”。然而，在下面的情况下，它却不能使你得到一个红背景红字体的段落：

    div:not(.one) p {color: red;}
    div.one p {background: red;}
    
    &lt;div class=&quot;one&quot;&gt;
      &lt;div class=&quot;two&quot;&gt;
        &lt;p&gt;Hi there!&lt;/p&gt;
      &lt;/div&gt;
    &lt;/div&gt;

这个段落是一个不含有one这个类（class）的div的子元素，但它同时也是一个含有one这个类的div的后代元素，两条规则都起作用了。

（感谢[Stephanie Hobson][3]第一个向我提及此事）

原文链接：<http://meyerweb.com/eric/thoughts/2012/03/07/negative-proximity/>

 

 [1]: http://www.w3.org/TR/css3-selectors/#negation
 [2]: http://w3.org/TR/css3-selectors/#specificity
 [3]: http://stephaniehobson.ca/