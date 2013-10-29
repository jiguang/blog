---
layout: post
title: "Ember.js 中文文档"
category:
  - JavaScript
tags:
  - Ember.js
  - 翻译
---
{% include JB/setup %}

前段时间我写了[《JavaScript MVC with Ember.js》][1]，里面提到有时间时要将文档翻译出来，但诺言一直没有兑现，最近终于把它赶了出来，希望对学习Ember.js的人有所帮助。

译文比较匆忙，故保留了英文原文，以便遇到晦涩之处时可以参照原文。

下面是Ember.js的官方网站以及中文文档的地址：

EmberJs 官方网站：<http://emberjs.com/>

EmberJs Github地址：<https://github.com/emberjs/ember.js> （欢迎Fork指正错误）

EmberJs 中文版文档访问地址：<http://ued.github.com/emberjs-doc-cn/>

这里的文档并不包含API文档，是官方Doc的中文译本，全人工翻译。

下面复制一些内容，方便大家预览：

### Ember.js 是什么? {#toc_what-is-ember-js}

Ember is a JavaScript framework for creating ambitious web applications that eliminates boilerplate and provides a standard application architecture.

Ember 是一个旨在创建非凡web应用的JavaScript框架，它消除了样板（boilerplate）并提供了标准的应用程序架构。

#### 消除样板 {#toc_eliminate-boilerplate}

There are some tasks that are common to every web application. For example, taking data from the server, rendering it to the screen, then updating that information when it changes.

每个web应用中都会有些常规的任务。例如，从服务器取数据、将数据渲染到屏幕、然后当数据变更时更新信息。

Since the tools provided to do this by the browser are quite primitive, you end up writing the same code over and over. Ember.js provides tools that let you focus on your app instead of writing the same code you’ve written a hundred times.

由于浏览器能够提供的完成该任务的工具太过原始，致使你一遍一遍地书写相同的代码。Ember.js提供的工具可以让你专注于你的app，而不是重复地编写已经写过无数次的代码。

Because we’ve built dozens of applications ourselves, we’ve gone beyond the obvious low-level event-driven abstractions, eliminating much of the boilerplate associated with propagating changes throughout your application, and especially into the DOM itself.

因为我们自己已经创建过数十个应用，我们已经远远超越了明显低级的事件驱动（event-driven）的抽象概念，消除了大量贯穿于应用当中的、与传播变化相关联的样板，尤其是DOM本身。

To help manage changes in the view, Ember.js comes with a templating engine that will automatically update the DOM when the underlying objects change.

为了帮助你管理视图（view）中的变更，Ember.js内置了一个模板引擎，当底层的对象发生改变时它可以自动地更新DOM。

For a simple example, consider this template of a Person:

作为一个简单的例子，考虑这个Person的模板:

    {{person.name}} is {{person.age}}.

As with any templating system, when the template is initially rendered, it will reflect the current state of the person. To avoid boilerplate, though, Ember.js will also update the DOM automatically for you if the person’s name or age changes.

与其他任何的模板系统类似，模板初始渲染时会影响到person的当前状态。然而，为了避免样板，当person的name更新时Ember.js也会为你自动更新DOM。

You specify your template once, and Ember.js makes sure it’s always up to date.

一次性指定模板，Ember.js就会确保它实时更新。

#### 提供架构 {#toc_provides-architecture}

Since web applications evolved from web pages, which were nothing more than static documents, browsers give you just enough rope to hang yourself with.

由于web应用是由web页面演变而来，其不过是静态文档而已，浏览器已赋予你充分的能力去做任何想做的事。

Ember makes it easy to divide your application into models, views, and controllers, which improves testability, makes code more modular, and helps new developers on the project quickly understand how everything fits together. The days of callback spaghetti are over.

Ember可以让你很容易地将应用划分为模型（models），视图（views）和控制器（controllers），从而提高了可测性，使代码更加模块化，并且帮助项目中的新手快速理解各部分的结合原理。意面式的回调（callback spaghetti）时代结束了。（[译者][2]注：[Spaghetti code][3]指具有复杂控制流程的代码。）

Ember also supplies built-in support for state management, so you’ll have a way to describe how your application moves through various nested states (like signed-out, signed-in, viewing-post, and viewing-comment) out of the box.

Ember同样提供了内置的对状态管理的支持，从而可以描述你的应用程序在各种不同的嵌套状态（比如登入登出，浏览文章，查看评论）间的流转。

 [1]: http://jiguang.github.com/index.php/2012/06/javascript-mvc-with-emberjs/
 [2]: https://twitter.com/laserji
 [3]: http://en.wikipedia.org/wiki/Spaghetti_code