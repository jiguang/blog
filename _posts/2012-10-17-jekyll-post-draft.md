---
layout: post
title: Jekyll 中如何保存文章草稿
category:
 - html-css
tags: [Jekyll, Github]
---
{% include JB/setup %}

当我们写文章博客时，经常需要保存未完成的草稿，但是 [Jekyll][1] 生成站点时，默认会遍历 `_post` 目录下的所有 `.md` 类型的文件。其实，通过一点简单的设置，就可以达到保存草稿的目的。

首先我们来熟悉一下 Jekyll 中使用的 [YAML][2] 格式的头信息，任何包含 YAML 头信息的文件都会被 Jekyll 特殊处理，这些头信息必须在文件内容的最顶端，用三个中划线分隔：

    ---
    layout: post
    title: Blogging Like a Hacker
    ---

当使用 UTF-8 编码时，要确保文件是无 BOM 格式的，尤其是 Windows 用户，否则还可能导致[Jekyll在Windows下生成_site目录为空][3]的错误。

Jekyll 中有以下几个预定义的全局变量：

<table class="table" style="font-size:12px;margin:10px 0;">
<tbody>
<tr>
<td> 变量 </td>
		<td> 描述 </td>
	</tr>
<tr>
<td> <code>layout</code> </td>
		<td> 如果设置了，则指定了所使用的布局文件，使用不带后缀名的布局文件名作为值。布局文件必须放在<code>_layouts</code> 目录中。</td>
	</tr>
<tr>
<td> <code>permalink</code> </td>
		<td> 如果你希望当前文章的永久链接不同于默认的 /year/month/day/title.html 形式，则可设置该变量，它将作为最后生成文章的 <span class="caps">URL</span>。</td>
	</tr>
<tr>
<td> <code>published</code> </td>
		<td> 如果你不想让某篇文章在生成的站点中出现，可将此变量设置为 false。</td>
	</tr>
<tr>
<td> <code>category</code>/<code>categories</code> </td>
		<td>可以指定一个或多个该文章所属的类别，可以以  <a href="http://en.wikipedia.org/wiki/YAML#Lists" target="_blank">YAML 列表</a>的形式指定。</td>
	</tr>
<tr>
<td> <code>tags</code> </td>
		<td> 与文章类别类似，也可以为文章指定一个或多个标签，同样适用 <span class="caps">YAML</span> 列表或空格分隔的字符串。 </td>
	</tr>
</tbody>
</table>

任何其他非预定义的变量，都将在转换时被发送到 [Liquid][4] 模板引擎中，例如设置文章标题的话可以在布局模板中使用下面的代码进行设置：

    <title>{{ page.title }}</title>

现在我们已经熟悉了 Jekyll 中的这些配置选项的作用了，那么保存草稿的话就是将 `published` 属性设置为 `false` 即可，这个文件就不会生成文章页面了：

    ---
    layout: post
    title: Blogging Like a Hacker
    published: false
    ---

不过需要注意的是，只要你的文件提交到了 [Github][5]，那么还是有可能被人看到的（如果你的粉丝经常关注你所提交的代码的话）。所以保存草稿时还是要酌情处理，看草稿阶段是否能够见人接客。


[1]: https://github.com/mojombo/jekyll
[2]: http://yaml.org/
[3]: http://44ux.com/blog/2012/10/10/invalid-byte-sequence-in-gbk/
[4]: http://liquidmarkup.org/
[5]: http://github.com/



