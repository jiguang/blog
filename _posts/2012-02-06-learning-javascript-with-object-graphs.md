---
layout: post
title: 通过对象图学习JavaScript [之一]
category:
  - JavaScript
tags:
  - 教程
---
{% include JB/setup %}

成为一个高效的JavaScript开发者的秘诀之一就是真正理解这门语言的语义。本文将会通过通俗易懂的图表来解释JavaScript中最基本的核心内容。

**随处可见的引用**

JavaScript中的变量其实一个标签，它引用了内存中某个位置的一个值。这些值可以是字符串、数字和布尔值的原始值。它们也可以是对象（object）或函数（function）。

**本地变量**

在下面这个例子中，我们会在顶级作用域中创建四个本地变量，并将它们指向某个原始值。

    // 我们在顶层作用域创建一些本地变量
    var name = "Tim Caswell";
    var age = 28;
    var isProgrammer = true;
    var likesJavaScript = true;
    // 测试一下看看两个变量是否引用了相同的值
    isProgrammer === likesJavaScript;

输出 => true  
![NewImage][1]  
注意两个布尔值指向的是内存中的同一位置，这是因为原始值是不变的，因此虚拟机（JavaScript解释器 ——[译者][2]注）可以优化它们使所有的引用共享这个原始值的同一个实例。

在这个代码片段中，我们使用 === 来判断两个引用是否指向同一个值，得到的结果是 true。

外面的框代表最外层的封闭作用域。这些变量是最顶级的本地变量，不要把它们跟global/window对象的属性相混淆了。

**对象和原型链**

对象只不过是更多引用的集合，它们指向新创建的对象和原型。它们唯一增添了一点比较特殊的特性就是原型链（Prototype Chains），当你试图访问一个不属于本地对象而属于其父对象的属性时就会用到原型链。

    // 创建一个父对象
    var tim = {
        name: "Tim Caswell",
        age: 28,
        isProgrammer: true,
        likesJavaScript: true
    }
    // 创建一个子对象
    var jack = Object.create(tim);
    // 覆盖一些本地属性
    jack.name = "Jack Caswell";
    jack.age = 4;
    // 通过原型链进行查找
    jack.likesJavaScript;

输出 => true  
![prototypechain][3]  
这里，我们有一个包含四个属性的被 tim 变量所引用的对象，同时我们创建了一个新的对象，该对象继承自第一个对象并且引用为 jack，然后我们覆盖了本地对象的两个属性。

现在，当我们查找jack.likesJavaScript时，起初会找到了jack所引用的对象，然后会继续查找likesJavaScript属性。由于本地对象中并不包含该属性，因此我们查找其父对象并找到了该属性，最后则找到了该属性所指向的true这个值。

**全局对象**

你想知道为什么像[jslint][4]这种工具经常会提示你别忘了在变量前面增加var声明吗？好吧，看看如果丢掉的话会发生什么情况。

    var name = "Tim Caswell";
    var age = 28;
    var isProgrammer = true;
    // 不小心丢掉了var
    likesJavaScript = true;

![novar][5]  
注意，现在likesJavaScript已经是全局对象的一个属性，而不是外层封闭作用域中的一个自由变量了。这种情况只有在混搭几段脚本时才会有问题。但是，在任何真正的程序中都会出现混搭的情况。

请牢记一定要添加这些var声明，这样才能保证你的变量是在当前的作用域及其子对象的作用域中。遵循这个简单的规则将使你受益匪浅。

如果你必须要在全局对象上添点儿东西，那么在浏览器中就明确地使用window.woo，而在node.js中则使用global.goo。

**函数与闭包**

JavaScript并不只是一系列的链式数据结构，它还包含了被称作函数（function）的可执行、可调用的代码。这些函数会创建链式作用域和闭包（closure）。

**可视化的闭包**

函数可以被看作是包含可执行代码及属性的特殊对象。每个函数都有一个特殊的`scope`属性，它代表了函数被定义时的环境。如果一个函数是由另外一个函数返回的，则这个指向旧环境的引用就会在一个“闭包”中被新的函数所终止。

在这个例子中，我们会创建一个简单的工厂方法，它可以生成一个闭包并返回一个函数。

    function makeClosure(name) {
        return function () {
            return name;
        };
    }
    var description1 = makeClosure("Cloe the Closure");
    var description2 = makeClosure("Albert the Awesome");
    console.log(description1());
    console.log(description2());

输出 Cloe the Closure Albert the Awesome

当我们调用description1()时，虚拟机会查找它所引用的函数并执行。由于这个函数会查找一个名为name的本地变量，因此它会在闭包作用域中进行查找。这个工厂方法的好处就是，每个生成的函数都有自己的本地变量空间。

参见这篇“为什么使用闭包（[why use closure][6]）”来获得更多关于闭包及其使用的内容。

**共享的函数和this关键字**

有时由于性能的原因，或者因为就是喜欢这种风格，JavaScript提供了一个this关键字允许你在不同的作用域中依据函数被调用的形式来重用函数对象。

这里我们创建一些对象，它们全部共享同一个函数，这个函数会在内部引用this来展示调用过程中的变化。

    var Lane = {
        name: "Lane the Lambda",
        description: function () {
            return this.name;
        }
    };
    var description = Lane.description;
    var Fred = {
        description: Lane.description,
        name: "Fred the Functor"
    };
    // 从四个不同的作用域调用函数
    console.log(Lane.description());
    console.log(Fred.description());
    console.log(description());
    console.log(description.call({
        name: "Zed the Zetabyte"
    }));

输出：Lane the Lambda Fred the Functor undefined Zed the Zetabyte

在此图中，我们看到即使Fred.description被设置成Lane.description，它实际上也只是引用了函数本身。因此，三个引用都同样对匿名函数拥有所有权。这就是为什么我没有在构造原型上通过“method”来调用函数的缘故，因为这意味着函数与其构造器和它的“类”之间的某种绑定关系。（详见”什么是this“[what is this][7] 获得更多关于this的动态本质的细节）

我很乐于用图表来使这些数据结构可视化，我希望这些内容可以帮助我们这些视觉学习者更好地掌握JavaScript的语义。我曾有过前端开发/设计和服务器端架构的经验。我希望我独特的视角能够帮助那些从设计岗位过来，并想深入学习这门被称作JavaScript的美妙语言的同学。

原文地址：<http://howtonode.org/object-graphs>

 [1]: http://jiguang.github.com/content/uploads/2012/02/NewImage.png "NewImage.png"
 [2]: http://jiguang.github.com "译者"
 [3]: http://jiguang.github.com/content/uploads/2012/02/prototypechain.png "prototypechain.png"
 [4]: http://jslint.com/
 [5]: http://jiguang.github.com/content/uploads/2012/02/novar.png "novar.png"
 [6]: http://howtonode.org/why-use-closure
 [7]: http://howtonode.org/what-is-this