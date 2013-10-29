---
layout: post
title: 通过对象图学习JavaScript 【之二】
category:
  - JavaScript
tags:
  - 教程
  - 翻译
---
{% include JB/setup %}

由于我的[第一篇文章][1]里通过图解描述JavaScript语义的方式大受欢迎，因此我决定尝试用这种方法来讲解一些高级内容。在本文中，我会讲解三种常用的创建对象的技术，它们分别是：构造器（constructor）加原型（prototype）的方式、纯原型的方式以及对象工厂（object factory）的方式。

我的目的是希望能够帮助大家理解每种技术的优缺点，并理解其运行机理。

**经典的JavaScript构造器**

首先我们通过原型来创建一个简单的构造器。这是在原生的JavaScript中最接近类（class）的一种方式。它非常强大而有效，但是我们并不能奢望它像其他包含类的语言一样强大。

    //长方形
    function Rectangle(width, height) {
    this.width = width;
    this.height = height;
    }
    Rectangle.prototype.getArea = function getArea() {
    return this.width * this.height;
    };
    Rectangle.prototype.getPerimeter = function getPerimeter() {
    return 2 * (this.width + this.height);
    };
    Rectangle.prototype.toString = function toString() {
    return this.constructor.name + " a=" + this.getArea() + " p=" + this.getPerimeter();
    };
    //正方形
    function Square(side) {
    this.width = side;
    this.height = side;
    }
    Square.prototype.__proto__ = Rectangle.prototype;
    Square.prototype.getPerimeter = function getPerimeter() {
    return this.width * 4;
    };
    //测试
    var rect = new Rectangle(6, 4);
    var sqr = new Square(5);
    console.log(rect.toString())
    console.log(sqr.toString())

现在我们新定义一个叫做Square的类对象，它继承自Rectangle。为了实现继承，构造器的prototype必须继承自父构造器的prototype。这里我们覆盖了getPerimeter使其更加高效，顺便展示一下如何来覆盖函数。

    function Square(side) {
    this.width = side;
    this.height = side;
    }
    Square.prototype.__proto__ = Rectangle.prototype;
    Square.prototype.getPerimeter = function getPerimeter() {
    return this.width * 4;
    };

用法就很简单了，只要给每个都创建一个实例（instance）并在实例上调用函数即可。

    var rect = new Rectangle(6, 4);
    var sqr = new Square(5);
    console.log(rect.toString())
    console.log(sqr.toString())

输出：  
Rectangle a=24 p=20 Square a=25 p=20

下图是生成的数据结构，虚线表示对象的继承。

![classic][2]

注意，虽然它们都是继承自Rectangle.prototype的对象，但在rect实例和Square.prototype之间还是有一点小区别。如果你仔细研究的话，会发现JavaScript不过是一系列相互关联的对象而已。唯一特殊的对象就是函数（function）了，在函数中可以接受参数并且可以包含可执行的代码，函数还可以指向作用域（scope）。

**纯原型对象**

再看刚才的例子，这次我们不使用构造函数，而只使用纯原型继承。

我们来定义一个Rectangle原型来作为构建其他对象的基础。

    var Rectangle = {
    name: "Rectangle",
    getArea: function getArea() {
    return this.width * this.height;
    },
    getPerimeter: function getPerimeter() {
    return 2 * (this.width + this.height);
    },
    toString: function toString() {
    return this.name + " a=" + this.getArea() + " p=" + this.getPerimeter();
    }
    };

现在我们来定义一个名为Square的子对象，并且覆盖一些属性来改变它的某些行为。

    var Square = {
    name: "Square",
    getArea: function getArea() {
    return this.width * this.width;
    },
    getPerimeter: function getPerimeter() {
    return this.width * 4;
    },
    };
    Square.__proto__ = Rectangle;

为了创建这些原型的实例，首先我们简单地创建一个继承自原型对象的新对象，然后再手动设置一些局部状态。

    var rect = Object.create(Rectangle);
    rect.width = 6;
    rect.height = 4;
    var square = Object.create(Square);
    square.width = 5;
    console.log(rect.toString());
    console.log(square.toString());

输出：  
Rectangle a=24 p=20 Square a=25 p=20

下面是生成的对象图：

![graph][3]

这个方法没有构造器+原型的方法那么强大，但是通常更容易理解一点，因为它没有那么拐弯抹角。当然了，如果你之前使用的语言包含纯原型继承，那么你会很高兴地发现在JavaScript中也是可以实现的。

**对象工厂**

我最喜欢的创建对象的方法之一就是使用工厂函数。它的不同之处在于，你不必定义包含所有共享函数的原型对象，然后再创建这些对象的实例，每次只需要简单地调用一个可以返回新对象的函数即可。

这个例子是一个超简单的MVC系统。控制器（controller）函数接受作为参数的模型（model）和视图（view）对象并且输出一个新的控制器对象。所有状态都通过作用域保存在闭包中。

    function Controller(model, view) {
    view.update(model.value);
    return {
    up: function onUp(evt) {
    model.value++;
    view.update(model.value);
    },
    down: function onDown(evt) {
    model.value--;
    view.update(model.value);
    },
    save: function onSave(evt) {
    model.save();
    view.close();
    }
    };
    }

若想使用该函数，只需要传入所需的参数调用函数即可。注意一下我们是如何用它来作为事件处理函数（setTimeout）而不用事先将函数绑定到对象上的。由于它（该函数）在内部不使用this关键字，因此就没有必要搞乱this的值了。

    var on = Controller(
    // 内嵌模拟的模型
    {
    value: 5,
    save: function save() {
    console.log("Saving value " + this.value + " somewhere");
    }
    },
    // 内嵌模拟的视图
    {
    update: function update(newValue) {
    console.log("View now has " + newValue);
    },
    close: function close() {
    console.log("Now hiding view");
    }
    }
    );
    setTimeout(on.up, 100);
    setTimeout(on.down, 200);
    setTimeout(on.save, 300);

    // 输出
    View now has 5
    View now has 6
    View now has 5
    Saving value 5 somewhere
    Now hiding view

下面是这段代码生成的对象图。注意我们是通过函数隐藏的`scope`属性来访问传入的两个匿名对象的，或者换句话说，我们通过工厂函数创建的闭包可以访问到model和view。

![mvc][4]

**结论**

这里面有太多我想探索的细节了，不过我更喜欢保持文章的简短易读。如果大家有需求的话，我会再写第三篇文章来讲解如何使用ruby风格的mixin以及其他一些高级内容。

原文链接：<http://howtonode.org/object-graphs-2>

 [1]: http://jiguang.github.com/index.php/2012/02/learning-javascript-with-object-graphs/
 [2]: http://jiguang.github.com/content/uploads/2012/02/classic.png "classic.png"
 [3]: http://jiguang.github.com/content/uploads/2012/02/graph.png "graph.png"
 [4]: http://jiguang.github.com/content/uploads/2012/02/mvc.png "mvc.png"