---
layout: post
title: "Sizzle 各版本中数组去重方法 Sizzle.uniqueSort 的演变"
description: "Sizzle 各版本中数组去重方法 Sizzle.uniqueSort 的演变"
category:
 - JavaScript
tags: []
---
{% include JB/setup %}

关于 JavaScript 中的数组去重，网上有许多文章，这里就不再赘述。今天恰好有机会用到 [jQuery](http://jquery.com/) 中的 `$.unique`，所以就翻看了一下源码整理下来。

我们知道 jQuery 是使用 [Sizzle](http://sizzlejs.com/) 作为选择器的基础类库的，其中也包含了基本的排序方法 `Sizzle.uniqueSort`，而 jQuery 的 `unique` 方法则为这个方法的一个别名：

{% highlight javascript %}
jQuery.unique = Sizzle.uniqueSort;
{% endhighlight %}

在 jQuery 1.7 以前 `Sizzle.uniqueSort` 的实现方式如下：

{% highlight javascript %}
Sizzle.uniqueSort = function( results ) {
	if ( sortOrder ) {
		hasDuplicate = baseHasDuplicate;
		results.sort( sortOrder );

		if ( hasDuplicate ) {
			for ( var i = 1; i < results.length; i++ ) {
				if ( results[i] === results[ i - 1 ] ) {
					results.splice( i--, 1 );
				}
			}
		}
	}

	return results;
};
{% endhighlight %}

该方法首先对数组进行排序，然后循环比较数组中的当前项与前一项，若全等，则删除当前项，并将索引置回。若不等，则继续在数组中前进。

在 jQuery 1.8 中 `Sizzle.uniqueSort` 的实现方式如下：

{% highlight javascript %}
Sizzle.uniqueSort = function( results ) {
	var elem,
		i = 1;

	hasDuplicate = baseHasDuplicate;
	results.sort( sortOrder );

	if ( hasDuplicate ) {
		for ( ; (elem = results[i]); i++ ) {
			if ( elem === results[ i - 1 ] ) {
				results.splice( i--, 1 );
			}
		}
	}

	return results;
};
{% endhighlight %}

相对于 jQuery 1.7 中的实现，该方法不再访问数组的 `length` 属性，提高了运行效率。每次以 `elem = results[i]` 赋值后是否存在为循环判定条件。

在 2012-10-09 [提交到 Sizzle 的代码](https://github.com/jquery/sizzle/commit/2a7c8b352198b3f42a1a54ff0097565499f5b6e4)中，[笔者](http://44ux.com)发现其实现方法又有些变化：

{% highlight javascript %}
// October 09, 2012 Fix jQuery #12671: quasi-array duplicate removal in oldIE
Sizzle.uniqueSort = function( results ) {
	var elem,
		duplicates = [],
		i = 1,
		j = 0;

	hasDuplicate = baseHasDuplicate;
	results.sort( sortOrder );

	if ( hasDuplicate ) {
		for ( ; (elem = results[i]); i++ ) {
			if ( elem === results[ i - 1 ] ) {
				j = duplicates.push( i );
			}
		}
		while ( j-- ) {
			results.splice( duplicates[ j ], 1 );
		}
	}

	return results;
};
{% endhighlight %}

提交的注释是修复老版IE中准数组（quasi-array）去重的问题，至于这个 oldIE 估计是指 IE6 以下。

与这部分代码同时提交的还有[测试用例的更改](https://github.com/gibson042/sizzle/commit/5f2d508cfdeaf71a00f5b0dd8bda34b0246f5ec5)：

{% highlight javascript %}
deepEqual( Sizzle.uniqueSort( arrDup ).slice( 0 ), [ el1, el2 ], "Duplicates array" );
deepEqual( Sizzle.uniqueSort( objDup ).slice( 0 ), [ el1, el2 ], "Duplicates quasi-array" );
{% endhighlight %}

这种方法的好处是可以兼容类数组的对象，如果你每次都能保证是对真正的数组排序的话，就不需要做这种处理了。

该提交目前还没有体现在最新的 jQuery 版本中，估计下一个小版本就会包含了。




