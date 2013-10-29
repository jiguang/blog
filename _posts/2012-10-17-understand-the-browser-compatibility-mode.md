---
layout: post
title: "深入理解浏览器兼容性模式"
description: "深入理解浏览器兼容性模式"
category:
  - html-css
tags: [Browser, HTML5]
---
{% include JB/setup %}

**UPDATE:** 本文已被 [CSDN](http://www.csdn.net/article/2012-10-22/2811049-Understanding-the-browser-compatibility-) 转载，可惜木有稿费，还把作者链接去掉了...

关于各种浏览器模式，网上已经有许多文档和资料了，但是很少有能够完全将几个概念阐述清楚的。大部分的资料稍显过时，有些内容可能已经不再适用了。本文中[笔者](http://44ux.com)将尽可能将几个概念阐述清楚，并去掉一些过时的内容，仅保留必要的干货。

想必你一定知道浏览器有个标准（Standards）模式和一个怪异（Quirks）模式，或许你还听说过有个“准标准（Almost Standards）”模式。而当你打开 Internet Explorer 的时候，又看到了什么浏览器模式、文档模式，还有什么兼容性视图等等...

这些都是什么？啥是浏览器模式，啥是文档模式？标准模式和准标准的模式有什么区别？IE9兼容性视图和真正的IE9有什么区别？什么情况下会触发这些模式，又该怎样才能检测到浏览器当前处于哪种模式中呢？本文将详细为你解答这些疑问。

**三种模式**

首先我们要知道，为什么会有这么多模式。其实这是个历史遗留问题，在[浏览器大战][1]时期，网景浏览器（Netscape Navigator）和微软的IE浏览器（Microsoft Internet Explorer）对网页分别有不同的实现方式，那个时候的网页要针对这两种浏览器分别开发不同的版本。而到了 [W3C](http://www.w3.org/) 制定标准之后，这些浏览器就不能继续使用这种页面了，因而会导致大部分现有站点都不能使用。基于这个原因，浏览器才引入两种模式来处理一些遗留的站点。

现在的浏览器排版引擎支持三种模式：怪异（Quirks）模式、准标准（Almost Standards）和标准（Standards）模式。在怪异模式中，排版引擎会模拟 网景4 和 Windows 中的 IE5 的行为；在完全标准的模式中，会尽量执行 HTML 和 CSS 规范所指定的行为；而在准标准模式中，则只包含很少的一部分怪异模式中的行为。

那么所谓标准模式，就一定都“标准”吗？答案当然是否定的，因为各个浏览器厂商实现标准的阶段不同，所以各个浏览器的“标准模式”之间也会有很大的不同。

Firefox、Safari、Chrome、Opera (自 7.5 以后)、 IE8 和 IE9 都有一个准标准模式。那么既然标准模式都不那么标准，准标准的模式肯定就更不标准了。[最初的准标准模式][2]只会[影响表格中的图像][3]，而后来各个浏览器又或多或少地进行了修改。那么什么情况下会触发准标准模式呢？是的，正如你所想到的，某些 [DOCTYPE][4] 会触发准标准模式，例如：

    &quot;-//W3C//DTD XHTML 1.0 Transitional//EN&quot;
    &quot;-//W3C//DTD XHTML 1.0 Frameset//EN&quot;
    &quot;-//W3C//DTD HTML 4.01 Transitional//EN&quot;
    &quot;-//W3C//DTD HTML 4.01 Frameset//EN&quot;
    &quot;http://www.ibm.com/data/dtd/v11/ibmxhtml1-transitional.dtd&quot;

一个完整的 DOCTYPE 例子如下：

    &lt;!DOCTYPE HTML PUBLIC &quot;-//W3C//DTD HTML 4.01 Transitional//EN&quot;
                          &quot;http://www.w3.org/TR/html4/loose.dtd&quot;&gt;

如果在 [Firefox](http://www.mozilla.org/en-US/firefox/new/) 中插入这种 DOCTYPE，并在页面中插入一个空的 `span` 标签，那么在 [Firebug](https://getfirebug.com/) 中查看元素的布局就会发现不同：

准标准模式中元素的 line-height 被忽略了，元素既没有宽度也没有高度：

![Almost Standards](http://44ux.com/content/uploads/2012/10/almost-standards.png)

标准模式中元素仍然保留了 line-height，拥有 18px 的高度：

![Standards](http://44ux.com/content/uploads/2012/10/full-standards.png)

在 Firefox 浏览器中，使用鼠标右键 -> 查看页面信息 可以看到当前浏览器运行在何种模式（只能看到“混杂模式”和“标准规范模式”两种表示）：

![mozilla-standard-mode](http://44ux.com/content/uploads/2012/10/mozilla-standard-mode.png)

有位大神 [Henri Sivonen](http://hsivonen.iki.fi/author/) 曾写过一篇文章叫做 [Activating Browser Modes with Doctype](http://hsivonen.iki.fi/doctype/)，里面包含了一个完整的表格，展示了各种 DOCTYPE 设置将会使浏览器以何种方式渲染。这里还有一篇 [秦歌](http://dancewithnet.com) 的译文 [《用doctype激活浏览器模式》](http://dancewithnet.com/2009/06/14/activating-browser-modes-with-doctype/)。

鉴于目前一些最新版本的浏览器已经放弃了准标准模式，所以关于准标准模式的细节这里就不再赘述了，感兴趣的同学可以详细阅读以下资料：

* [Gecko's "Almost Standards" Mode](https://developer.mozilla.org/en-US/docs/Gecko's_Almost_Standards_Mode?redirectlocale=en-US&redirectslug=Gecko%27s_%22Almost_Standards%22_Mode)

* [Line Height Calculations in Almost Standards Mode](http://msdn.microsoft.com/en-us/library/ff405794%28v=vs.85%29)

* [Images, Tables, and Mysterious Gaps](https://developer.mozilla.org/en-US/docs/Images,_Tables,_and_Mysterious_Gaps)

* [almost-standards test](http://meyerweb.com/eric/css/tests/almost-standards.html)

* [DOCTYPE Switches support in Opera Presto 2.10](http://www.opera.com/docs/specs/doctype/)

那么，既然这么多的 DOCTYPE 都会触发非标准的模式，那么如何才能触发标准模式呢？对了！要使用 [HTML5 DOCTYPE][5]，即：

    &lt;!DOCTYPE html&gt;

注意：如果文档中没有包含 DOCTYPE 或者包含了一个无法识别的 DOCTYPE，则浏览器就会进入怪异模式。

下面简单说一下怪异模式。怪异模式有许多“怪异”的行为，主要是为了兼容那些遗留的古老页面而保留的模式。不同浏览器的怪异模式也不尽相同，它们都有自己的实现方式。怪异模式与标准模式的差异主要体现在 [盒模型（box model）](http://www.w3.org/TR/CSS2/box.html)、表格单元格高度的处理等。例如 IE 的怪异模式中，元素的 width 包含了 padding 和 border，而标准模式中 padding 和 border 并不属于宽度的一部分。

若想详细了解浏览器在怪异模式下的行为，可以参看下面两篇文章。不过不建议在这上面花太多的精力，这是个历史遗留问题，而且我们也尽量保证新开发的页面不要进入到怪异模式：

* [Mozilla Quirks Mode Behavior](https://developer.mozilla.org/en-US/docs/Mozilla_Quirks_Mode_Behavior)

* [What happens in Quirks Mode?](http://www.cs.tut.fi/~jkorpela/quirks-mode.html)

* [Compatability Mode Test](http://hixie.ch/tests/adhoc/compat/mozilla/001.cgi?DOCTYPE=%3C!DOCTYPE+HTML%3E&MODE=full&EXPECT=standards%20mode)

**小结：** 至此我们需要了解，浏览器有三种运行模式，即标准模式、准标准模式和怪异模式，要使用 `<!DOCTYPE html>` 来正确地触发标准模式。千万不要丢掉 DOCTYPE 声明，因为这会导致浏览器进入怪异模式。


**IE的浏览器模式**

IE8有4种模式：IE5.5怪异模式、IE7标准模式、IE8 准标准模式和IE8标准模式，而IE9有7种模式: IE5.5怪异模式、IE7标准模式、IE8准标准模式、IE8标准模式、IE9准标准模式、IE9标准模式、XML模式。

其中 XML模式 是针对 XML 文档的，这里不打算阐述，细节可以看这篇文章[Defining Document Compatibility](http://msdn.microsoft.com/en-us/library/cc288325(v=vs.85\).aspx) 中有详细阐述。

在 IE8 及以后的的 IE 浏览器中，支持 `X-UA-Compatible` 头，可以通过在服务器端设置 HTTP 头，或者在页面中插入 `<meta>` 标签来实现：

    HTTP:
    Header set X-UA-Compatible "IE=8"

    Meta:
    &lt;meta http-equiv=&quot;X-UA-Compatible&quot; content=&quot;IE=EmulateIE7&quot; /&gt;

这种方法主要是防止老的页面在较新的浏览器中显示不正常的情况的， 比如上面的代码可以强制 IE8 以上版本的浏览器以IE7的模式进行渲染。

注意，不要在新开发的网页中使用这种技术，这种技术只应该作为新老网页更替过程中的过渡方案。由于目前新开发的网页都是尽量支持最新版本的浏览器的，所以这种技术也会慢慢被淘汰，感兴趣的同学可以详细阅读 <a href="http://msdn.microsoft.com/en-us/library/cc288325(VS.85).aspx" target="_blank">微软的这篇文档</a>。

**小结：** 这里我们需要知道有这种方式可以强制浏览器以某种模式运行，但只应作为过渡方案，不应在新开发的网页中使用。


**IE9 兼容性视图 与 IE9 标准视图**

如果你使用的是 IE9，那么按下 F12 键就会出现开发者工具，上面有两个下拉菜单：浏览器模式 和 文档模式。那么什么是浏览器模式？什么又是文档模式？二者有何区别？

浏览器模式用于切换IE针对该网页的默认文档模式、对不同版本浏览器的条件注释解析、以及发送给网站服务器的用户代理（User-Agent）字符串的值。网站可以根据浏览器返回的不同用户代理字符串判断浏览器的版本和及安装的功能，这样就可以根据不同的浏览器返回不同的页面内容了。

文档模式用于指定IE的页面排版引擎（Trident）以哪个版本的方式来解析并渲染网页代码。切换文档模式会导致网页被刷新，但不会更改用户代理字符串中的版本号，也不会从服务器重新下载网页。切换浏览器模式的同时，浏览器也会自动切换到相应的文档模式。

一言以蔽之，浏览器模式会影响服务器端对客户端浏览器版本的判断，对条件注释也有影响；而文档模式会影响IE的排版引擎，对网页渲染会有影响，对 CSS hack 也会产生影响。因此，通过条件注释可以判断浏览器模式，而使用 CSS hack 可以判断文档模式。

如果我们使用一句简单的 JavaScript 语句来查看用户代理（User-Agent）字符串的值，则可以看到 IE9 兼容性视图 与 IE9 的区别：

    &lt;script type=&quot;text/javascript&quot;&gt;
        alert('UA:'+navigator.userAgent);
    &lt;/script&gt;

输出结果如下所示，注意其中的 MSIE 版本号已经不同。判断浏览器模式就是判断 User-Agent 中的版本号，即 MSIE 后面的数值：

{% highlight javascript %}
// IE9
UA:Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E; Tablet PC 2.0)

// IE9 兼容性视图
UA:Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.1; Trident/5.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E; Tablet PC 2.0)
{% endhighlight %}

话说 IE9 兼容性视图 是模拟IE7的行为，那么 IE9 兼容性视图 与 IE7 有没有区别呢？肯定是有区别的，即使是 IE9 中的 IE7标准模式，与原生的IE7在渲染上也是有区别的，具体我们暂不去深究。

那么既然 IE9 兼容性视图 的版本号跟 IE7 相同，如何才能判断当前是 IE9 兼容性视图，还是纯正的 IE7 呢？其实很简单，只需要判断浏览器的用户代理（User-Agent）字符串中是否包含 Trident 即可。首先检测 MSIE 的版本号是否为 7.0，然后再判断是否含有 Trident 字串，若包含则为 IE9 兼容性视图，否则则为纯正的 IE7。

**小结：** 至此，你应该了解了什么是浏览器模式、什么是文档模式以及它们之间的区别了，另外还了解了 IE9 兼容性视图 与 IE9 以及 IE7 的区别。


**控制默认的渲染方式**

当 Internet Explorer 9 遇到未包含 `X-UA-Compatible` 标头的网页时，它将使用 `<!DOCTYPE>` 指令来确定如何显示该网页。 如果该指令丢失或未指定基于标准的文档类型，则 Internet Explorer 9 将以 IE5 模式（怪异模式）来显示该网页。

如果 `&lt;!DOCTYPE&gt;` 指令指定了基于标准的文档类型，则 Internet Explorer 9 将以 IE9 模式显示该网页，但出现以下情况时除外：

<ul>
    <li>为该网页启用了兼容性视图。</li>
    <li>该网页是在 Intranet 区域中加载的，并且已将 Internet Explorer 9 配置为使用兼容性视图来显示 Intranet 区域中的网页。</li>
    <li>已将 Internet Explorer 9 配置为使用兼容性视图来显示所有网站。</li>
    <li>已将 Internet Explorer 9 配置为使用<a href="http://msdn.microsoft.com/en-us/library/dd567845(v=VS.85).aspx" target="_blank">兼容性视图列表（其实是个黑名单，其中指定了一组始终使用兼容性视图显示的网站）</a>。</li>
    <li>已使用开发人员工具覆盖在该网页中指定的设置。</li>
    <li>该网页遇到了页面布局错误，并且已将 Internet Explorer 9 配置为，通过在兼容性视图中重新打开网页来自动从此类错误中恢复。</li>
</ul>


此外，可以使用下面的注册表项来控制 Internet Explorer 对未包含 X-UA-Compatible 标头的页面的处理方式。

    HKEY_LOCAL_MACHINE (or HKEY_CURRENT_USER)
         SOFTWARE
              Microsoft
                   Internet Explorer
                        Main
                             FeatureControl
                                  FEATURE_BROWSER_EMULATION
                                       iexplore.exe = (DWORD)

其中 `DWORD` 值必须等于下列值之一：

<table class="table" style="margin:10px 0;font-size:12px;">
	<tbody><tr><td>值</td><td>说明</td></tr>
		<tr><td>7000</td><td>包含基于标准的 &lt;!DOCTYPE&gt; 指令的页面将以 IE7 模式显示。</td></tr>
		<tr><td>8000</td><td>包含基于标准的 &lt;!DOCTYPE&gt; 指令的页面以 IE8 模式显示。</td></tr>
		<tr><td>8888</td><td>页面始终以 IE8 模式显示，而不考虑 &lt;!DOCTYPE&gt; 指令。 （这可绕过前面列出的例外情况。）</td></tr>
	</tbody>
</table>

关于IE浏览器确定文档模式的整个流程，可以参看这篇文章 [How IE8 Determines Document Mode][6]，文中详细阐述了整个流程与内部机制。

**小结：** 仍然坚持使用 `<!DOCTYPE html>`，可最大程度减小发生错误的几率。


**文档模式的检测**

在 JavaScript 中可以通过 `documentMode` 来检测文档模式，在 IE6 和 IE7 中是使用 `compatMode` 来确定文档模式的，这个属性自 IE8 开始已经被 `documentMode` 所替代。

那么，如果需要兼容 IE6 和 IE7 的话（必须的 ...），则相应的检测代码大致如下：

{% highlight javascript %}
engine = null;
if (window.navigator.appName == "Microsoft Internet Explorer")
{
   // This is an IE browser. What mode is the engine in?
   if (document.documentMode) // IE8 or later
      engine = document.documentMode;
   else // IE 5-7
   {
      engine = 5; // Assume quirks mode unless proven otherwise
      if (document.compatMode)
      {
         if (document.compatMode == "CSS1Compat")
            engine = 7; // standards mode
      }
      // There is no test for IE6 standards mode because that mode
      // was replaced by IE7 standards mode; there is no emulation.
   }
   // the engine variable now contains the document compatibility mode.
}
{% endhighlight %}

IE6 和 IE7 中的 `compatMode` 有两个可能的值“CSS1Compat”和“BackCompat ”，分别对应了 IE6 和 IE7 中的标准模式和怪异模式。上面的代码首先假定是怪异模式，然后再试图推翻假设。这里没有包含“IE6 标准模式”，因为它已经被 IE7标准模式 所替代，没有模拟的情况。

这里要注意，不同的文档模式对 JavaScript 也有[一些影响][8]，我们不必去深究不同文档模式对 JavaScript 有何种不同影响，只需要在编码时进行特定的 [特性检测][7] 即可。

**小结：** 一般情况下是没必要进行文档模式检测的，对于样式兼容我们可以写 CSS hack，而对于 JavaScript 来说，则更加推荐特性检测，而不是检测浏览器本身。


**浏览器模式与文档模式之间的关系**

浏览器模式可以决定页面默认的文档模式，但文档模式可能会受其他因素影响而改变，如上文所述。如果浏览器模式与文档模式设置不同的话，会不会有什么影响呢？

我们已经知道浏览器模式主要用于标识浏览器本身，原则上不会对页面渲染产生影响。但是我们又知道，浏览器模式可以影响条件注释，所以如果你的页面中有条件注释的话，那么浏览器模式的变化就会影响到页面渲染。

服务器端只能通过浏览器模式所标识的版本来确定客户端浏览器的版本，如果你将浏览器模式标识为IE9，但文档模式选择为IE7标准的话，就可能会有问题。不过这还要看服务器端是否有针对不同浏览器的处理策略，如果服务器端并未对不同浏览器的输出做差异化处理的话，那么这两个模式选项不同就不会有问题。

**小结：** 如果服务器端对不同浏览器的输出做了差异化处理，那么浏览器模式和文档模式不一致就可能产生问题。


**结语**

本文参考了大量现有文献，详细阐述了各种模式的区别以及它们之间的关系。相信通过上面的叙述，你已经能够区分这些浏览器模式或者文档模式以及它们之间的关系了，每节的结论在小结中已有阐述，希望能够对你有所帮助。


**参考文献**

<p><a href="http://en.wikipedia.org/wiki/Browser_wars">http://en.wikipedia.org/wiki/Browser_wars</a></p>

<p><a href="http://meyerweb.com/eric/thoughts/2008/01/24/almost-target/">http://meyerweb.com/eric/thoughts/2008/01/24/almost-target/</a></p>

<p><a href="https://developer.mozilla.org/en-US/docs/Images,_Tables,_and_Mysterious_Gaps">https://developer.mozilla.org/en-US/docs/Images,_Tables,_and_Mysterious_Gaps</a></p>

<p><a href="http://en.wikipedia.org/wiki/Document_Type_Declaration">http://en.wikipedia.org/wiki/Document_Type_Declaration</a></p>

<p><a href="http://dev.w3.org/html5/spec/#the-doctype">http://dev.w3.org/html5/spec/#the-doctype</a></p>

<p><a href="http://blogs.msdn.com/b/ie/archive/2010/03/02/how-ie8-determines-document-mode.aspx">http://blogs.msdn.com/b/ie/archive/2010/03/02/how-ie8-determines-document-mode.aspx</a></p>

<p><a href="http://kangax.github.com/cft/">http://kangax.github.com/cft/</a></p>

<p><a href="http://blogs.msdn.com/b/ie/archive/2011/03/24/ie9-s-document-modes-and-javascript.aspx">http://blogs.msdn.com/b/ie/archive/2011/03/24/ie9-s-document-modes-and-javascript.aspx</a></p>

<p><a href="http://msdn.microsoft.com/en-us/library/cc288325(v=vs.85).aspx">http://msdn.microsoft.com/en-us/library/cc288325(v=vs.85).aspx</a></p>

<p><a href="https://developer.mozilla.org/en-US/docs/Quirks_Mode_and_Standards_Mode">https://developer.mozilla.org/en-US/docs/Quirks_Mode_and_Standards_Mode</a></p>

<p><a href="https://developer.mozilla.org/en-US/docs/Gecko's_Almost_Standards_Mode">https://developer.mozilla.org/en-US/docs/Gecko's_Almost_Standards_Mode</a></p>

<p><a href="http://blogs.msdn.com/b/ie/archive/2009/02/16/just-the-facts-recap-of-compatibility-view.aspx">http://blogs.msdn.com/b/ie/archive/2009/02/16/just-the-facts-recap-of-compatibility-view.aspx</a></p>

<p><a href="http://www.quirksmode.org/css/quirksmode.html">http://www.quirksmode.org/css/quirksmode.html</a></p>

<p><a href="http://hsivonen.iki.fi/doctype/">http://hsivonen.iki.fi/doctype/</a></p>

<p><a href="http://blogs.msdn.com/b/ie/archive/2011/03/24/ie9-s-document-modes-and-javascript.aspx">http://blogs.msdn.com/b/ie/archive/2011/03/24/ie9-s-document-modes-and-javascript.aspx</a></p>

<p><a href="https://developer.mozilla.org/en-US/docs/Mozilla_Quirks_Mode_Behavior">https://developer.mozilla.org/en-US/docs/Mozilla_Quirks_Mode_Behavior</a></p>

<p><a href="https://developer.mozilla.org/en-US/docs/Gecko's_Almost_Standards_Mode">https://developer.mozilla.org/en-US/docs/Gecko's_Almost_Standards_Mode</a></p>

<p><a href="http://meyerweb.com/eric/css/tests/almost-standards.html">http://meyerweb.com/eric/css/tests/almost-standards.html</a></p>

<p><a href="http://www.opera.com/docs/specs/doctype/">http://www.opera.com/docs/specs/doctype/</a></p>



[1]: http://en.wikipedia.org/wiki/Browser_wars
[2]: http://meyerweb.com/eric/thoughts/2008/01/24/almost-target/
[3]: https://developer.mozilla.org/en-US/docs/Images,_Tables,_and_Mysterious_Gaps
[4]: http://en.wikipedia.org/wiki/Document_Type_Declaration
[5]: http://dev.w3.org/html5/spec/#the-doctype
[6]: http://blogs.msdn.com/b/ie/archive/2010/03/02/how-ie8-determines-document-mode.aspx
[7]: http://kangax.github.com/cft/
[8]: http://blogs.msdn.com/b/ie/archive/2011/03/24/ie9-s-document-modes-and-javascript.aspx

