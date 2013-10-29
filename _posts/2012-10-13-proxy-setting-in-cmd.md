---
layout: post
title: CMD 和 Git 中的代理设置
category:
 - DevTools
tags: [Note, Git]
---
{% include JB/setup %}

前些天在公司配置 [Jekyll][4] 的时候用到 CMD 的代理设置，这里记录一下，另附上 Git 的代理设置，以备查阅。

**CMD 设置代理**

在 [cmd][1] 环境下设置代理可能不是很常用，但是某些情况下还是可能会用到，比如公司的电脑只能通过设置代理访问外网，而你需要在 cmd 环境下使用 `gem` 命令更新文件时。

当然，如果你使用某些代理软件为所有通讯设置了代理，那就不需要这些设置了。

为 cmd 设置代理很简单，首先打开 cmd （win + R，输入 cmd，然后按 enter 键），然后输入如下命令：

{% highlight bash %}
set http_proxy=http://proxy.yourname.com:8080
{% endhighlight %}

其中 `http://proxy.yourname.com` 是你的代理服务器地址，而 `8080` 是端口号，如果有则设置。另外，如果你的代理服务器要求用户名和密码的话，那么还需要：

{% highlight bash %}
set http_proxy_user=&lt;&amp;#20320;&amp;#30340;&amp;#29992;&amp;#25143;&amp;#21517;&gt;
set http_proxy_pass=&lt;&amp;#20320;&amp;#30340;&amp;#23494;&amp;#30721;&gt;
{% endhighlight %}

设置完成后，就可以在 `cmd` 下正常使用网络了。

**Git 设置代理**

[Git][3] 的代理设置也非常简单，一句话就搞定了：

{% highlight bash %}
git config --global http://proxy.yourname.com:8080
{% endhighlight %}

如果需要用户名密码的话，则设置：

{% highlight bash %}
git config –global http.proxy http://user:password@proxy.yourname.com:8080
{% endhighlight %}

其中 `user` 和 `password` 分别为你的用户名和密码。

设置完成后，可以通过如下命令来查看设置是否生效：

{% highlight bash %}
git config –get –global http.proxy
{% endhighlight %}

如果某一天你不喜欢她了，需要删除代理设置，那么可以使用：

{% highlight bash %}
git config --system (或 --global 或 --local) --unset http.proxy
{% endhighlight %}

来删除设置。

PS：有些同学可能使用的 Git 的客户端，比如界面很上流的 [Github for Windows][2]，里面可能并没有代理设置的选项，不过别着急，这些客户端一般在底层都是调用的命令行工具，所以同样按照上述步骤进行设置即可。

[1]: http://baike.baidu.com/view/65856.htm
[2]: http://windows.github.com/
[3]: http://44ux.com/tags.html#Git-ref
[4]: https://github.com/mojombo/jekyll
