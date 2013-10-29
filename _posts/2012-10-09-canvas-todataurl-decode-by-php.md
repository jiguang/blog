---
layout: post
title: Canvas toDataURL decoded by PHP
category:
  - JavaScript
tags: [canvas, PHP]
---
{% include JB/setup %}

我们知道 Canvas 的 [toDataURL](https://developer.mozilla.org/en-US/docs/DOM/HTMLCanvasElement) 方法可以生成 base64 编码的 dataURL 形式的图片到页面中，例如：

{% highlight javascript %}
function test() {
     var canvas = document.getElementById("canvas");
     var url = canvas.toDataURL();

     var newImg = document.createElement("img");
     newImg.src = url;
     document.body.appendChild(newImg);
}
{% endhighlight %}

那么，如何将生成后的图片保存下来呢？

如果在客户端的话，最简单的方式就是鼠标右键->另存为，那么在服务器端如何保存呢？

假设服务器端使用的是PHP，那么可以使用PHP的 [base64_decode](http://php.net/manual/en/function.base64-decode.php) 方法，不过有几点需要注意：

1，需要将空格转换为加号：

{% highlight php %}
$encodedData = str_replace(' ','+',$encodedData);
{% endhighlight %}

2，需要去掉前面的前缀：

{% highlight php %}
$encodedData = preg_replace('/^data:image\/(png|jpg);base64,/','',$encodedData);
{% endhighlight %}

3，前两步都可以在客户端使用 JavaScript 完成，然后将处理完的数据 post 给 PHP 页面，再调用 base64_decode 即可：

{% highlight php %}
$decocedData = base64_decode($encodedData);
{% endhighlight %}

4，解码后，可以将图片直接显示出来：

{% highlight php %}
header("Content-type: image/png");
echo $decocedData;
{% endhighlight %}