---
layout: post
title: Jekyll在Windows下生成_site目录为空的解决方案
category:
  - html-css
tags: [Markdown, Jekyll]
---
{% include JB/setup %}

Mac 下安装 [Jekyll][1] 非常简单，几乎不会遇到问题，但是一到了 Windows 就各种纠结。如果你不知道什么是 Jekyll 和 Github Pages，可以看一下[阮老师的这篇文章][2]。

本文讲述的是 \_site 目录生成内容为空的解决方案，前提是你已经在 Windows 下[安装][3]好了 Jekyll。

**基本症状：**

在 cmd 中运行：

{% highlight bash %}
jekyll --server
{% endhighlight %}

此时命令会很快结束，但是查看站点目录中的 \_site 目录却没有内容。

**解决方案：**

首先使用：

{% highlight bash %}
jekyll --server --no-auto
{% endhighlight %}

命令查看 Ruby 报错（也可以将 \_config.yml 中的 `auto: true` 改为 `auto: false`），可能会出现一坨错误，其中可能包含`invalid byte sequence in GBK`字样。

那么很明显这是个编码问题，请仔细查看报错的文件位置，如果是 Ruby 本身的问题，那么有两种解决方案：

1. Ruby代码的编码格式有问题：

解决方案是在该ruby文件头声明正确的编码

{% highlight bash %}
# encoding: GB2312
{% endhighlight %}

2. 读取的文件编码不对：

解决方案：Ruby 在 window 中默认的文件读取编码是GBK, 因此只要在打开文件时改变编码即可。

{% highlight bash %}
File.open("scroll.js", 'r:utf-8')
{% endhighlight %}

如果报错位置是在 Jekyll 的目录中，例如：

{% highlight bash %}
C:\Ruby193\lib\ruby\gems\1.9.1\gems\jekyll-0.11.2\lib\jekyll\convertible.rb:27:in `read_yaml`: invalid byte sequence in US-ASCII (ArgumentError)
{% endhighlight %}

那么我们可以看到，在 convertible.rb 文件中第27行出现错误（具体行号见实际报错提示），那么我们找到这一行：

{% highlight bash %}
self.content = File.read(File.join(base, name))
{% endhighlight %}

将其修改为：

{% highlight bash %}
self.content = File.read(File.join(base, name), :encoding => "utf-8")
{% endhighlight %}

即可解决。同时，一定要确保你的文章要保存为 [UTF-8 无 BOM 格式][4]才行。

**UPDATE:**也有些同学在 Win7 下还需要设置环境变量：

{% highlight bash %}
export LC_ALL=zh_CN.UTF-8
export LANG=zh_CN.UTF-8
{% endhighlight %}

在 gitbash 下直接执行即可。


[1]: https://github.com/mojombo/jekyll
[2]: http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html
[3]: https://github.com/mojombo/jekyll/wiki/install
[4]: http://wenwen.soso.com/z/q118420256.htm



