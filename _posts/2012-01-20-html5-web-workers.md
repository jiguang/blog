---
layout: post
title: "JavaScript并行运算新机遇——WebWorkers的神奇魔法"
category:
  - JavaScript
tags:
  - HTML5
  - Web Workers
  - 翻译
---
{% include JB/setup %}

原文链接：<http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers.aspx>

很明显[html5][1]的应用是用JavaScript写的，但是跟其他的开发环境相比（例如一些原生的），JavaScript一直有个很严重的局限性：它的所有执行进程都在同一个线程里。

这对于如今像i5/i7这种动辄就8个CPU的多核处理器就有些麻烦了，即使最新的ARM手机处理器也都是双核或者4核。如果顺利的话，我们有望看到HTML5为Web开发提供一个应对这些又新又强劲的处理器的途径，让我们可以拥抱一个Web应用开发的新时代。

![原文插图1][2]

## 在没有 Workers 之前

这个JavaScript的局限性意味着一个长时间运行的进程会冻结主窗口。我们常说我们被“UI 线程”阻塞。这是由于主线程在处理所有的可视化元素及其相关任务：绘制，刷新，动画，用户输入事件等等。

我们都知道这个线程过载的严重后果：页面冻结并且用户不能再与你的应用进行交互了。当然，这时的用户体验那是相当差了，并且用户可能决定关掉这个Tab或者整个浏览器，你可能不希望看到这发生在你的app上。

为了避免发生这种情况，浏览器已经引入一种保护机制，当一个脚本有可能长时间运行时，可以对用户进行警告。

悲剧的是，这种机制并不能正确分辨究竟一段脚本是编写有问题，还是它确实需要更多的时间来完成它的工作。尽管如此，由于它阻塞了UI线程，所以还是让你知道有可能现在有错误发生的比较好。下面是一些消息的例子（从Firefox 5 和 IE9 获得）：

![原文插图][3]

![原文插图][4]

迄今为止，由于以下两个原因，那些问题已经很少发生了：

1.  HTML和JavaScript已经与原来的使用方式不同了，而且使用的目的也不同了，因为其他的技术可以完成多线程的任务。与本地的应用相比，网站也没有提供更丰富的体验。
2.  总有一些其他的办法可以或多或少地解决这些并发问题。

那些办法都是Web开发者所熟知的。例如，通过 setTimeout（） 和 setInterval（）方法，我们可以试图模拟并行任务。通过 XMLHttpRequest对象，也可以异步地处理HTTP请求，避免了从远程服务器载入资源时冻结UI。最后，应用DOM事件写出的应用程序给人一种错觉，让人误以为几个事件在同时发生。真的是错觉吗？是的！

为了更好的理解其原理，让我们来看一段伪代码并且看看在浏览器内部究竟发生了什么：

    &lt;script type=&quot;text/javascript&quot;&gt;
    function init(){
    { piece of code taking 5ms to be executed }
    A mouseClickEvent is raised
    { piece of code taking 5ms to be executed }
    setInterval(timerTask,&quot;10&quot;);
    { piece of code taking 5ms to be executed }
     }
    function handleMouseClick(){
    piece of code taking 8ms to be executed
    }
    function timerTask(){
    piece of code taking 2ms to be executed
    }
    &lt;/script&gt;

让我们为这段代码建立一个模型。这个图表向我们展示了一个时间段内浏览器内部究竟发生了什么：

![原文插图][5]

这个图表很好地诠释了我们的任务的非并行本质。5毫秒后，用户产生一个鼠标点击事件。然而，由于init()方法仍然在执行，并且独占了主线程，所以这个事件不能被立即处理。点击事件将被保存并且延迟处理。

从5毫秒到10毫秒之间：init()方法在这5毫秒中仍然执行，然后在10毫秒时请求调用 timerTask() 这个方法。这个方法理论上应该在20毫秒的时间点执行。

从10毫秒到15毫秒之间：init()方法仍然需要5毫秒来完成运行。这与15毫秒时的黄色区块相对应。由于我们冻结了主线程，所以浏览器现在才可以继续进行刚才保存的请求。

从15毫秒到23毫秒之间：浏览器开始执行handleMouseClick（）方法，该方法执行了8毫秒（蓝色区块）。

从23毫秒到25毫秒之间：作为一个副作用，在20毫秒时间点就应该执行的timerTask（）方法被稍微平移了3毫秒。而其他的时间点，被当作没有代码占用CPU。

注意：这个例子和上面的图表（通过特征监测机制判断使用SVG或者PNG）是受到这篇文章的启发：[HTML5 Web Workers Multithreading in JavaScript][6]

所有这些提示并没有解决我们最初的问题：所有东西都在主UI线程里执行。

此外，即使JavaScript还没有被用来开发像其他“高级语言”一样的应用，它仍然在随着HTML5和其相关技术所提供的新的可能而改变。因此给JavaScript赋予更多新的能力，使之能够建立新一代的能够处理并行任务的应用，就变得更加重要了。这就是为什么我们有了Web Workers。

## Web Workers 或者 如何在UI线程执行代码

[Web Workers APIs][7]定义了一个在后台运行脚本的方法。你可以执行一些存活在主页面之外的线程而不影响页面的绘制性能。然而，同样的方式，我们知道不是所有的算法都能并行执行的，也不是所有的JavaScript代码都能从Workers中受益。Ok，唠叨的够多了，让我们看看这些著名的Workers。

## 我的第一个Web Worker

由于Web Workers将在一个独立的线程里执行，你必须把代码从主页面中分离出来，放到独立的文件中。完成这些后，你需要实例化一个Worker对象来调用它们：

    var myHelloWorker = new Worker('helloworkers.js');

然后你就可以给它发送一条信息来开启Worker（因此也开启了一个窗口之外的线程）：

    myHelloWorker.postMessage();

的确，Web Workers和主页面通过消息进行通信。这些消息可以是一般的字符串或者JSON对象。为了演示简单的消息发送，我们来review一个非常基础的例子。这个例子会发送一个字符串给worker，将其与worker联系起来。首先，将下面代码放到“ helloworker.js”文件中：

    function messageHandler(event) {
    // Accessing to the message data sent by the main page var messageSent = event.data;
    // Preparing the message that we will send back
    var messageReturned = "Hello " + messageSent + " from a separate thread!";
    // Posting back the message to the main page this.postMessage(messageReturned);
    }
    // Defining the callback function raised when the main page will call us
    this.addEventListener('message', messageHandler, false);

我们只在“helloworkers.js”中定义了一小段将在另一个线程执行的代码。它可以从你的主页面接收消息，在上面完成一些任务，并且向你的主页面返回一个消息。然后我们需要在主页面编写一个接收者。下面是处理消息的页面：

    &lt;!DOCTYPE html&gt;
    &lt;html&gt;
    &lt;head&gt;
    &lt;title&gt;Hello Web Workers&lt;/title&gt;
    &lt;/head&gt;
    &lt;body&gt;
    &lt;div id=&quot;output&quot;&gt;&lt;/div&gt;
    &lt;script type=&quot;text/javascript&quot;&gt;
    // Instantiating the Worker
    var myHelloWorker = new Worker('helloworkers.js');
    // Getting ready to handle the message sent back
    // by the worker myHelloWorker.addEventListener(&quot;message&quot;, function (event) {
    document.getElementById(&quot;output&quot;).textContent = event.data;
    }, false);
    // Starting the worker by sending a first message
    myHelloWorker.postMessage(&quot;David&quot;);
    // Stopping the worker via the terminate() command myHelloWorker.terminate();
    &lt;/script&gt;
    &lt;/body&gt;
    &lt;/html&gt;

运行的结果将是：”Hello David from a separate thread!”，你被打动了，有木有？

你要注意worker会一直存活直到你终止它。

既然没有自动垃圾收集，那么控制它们的状态就全靠你自己了。并且你要记住，初始化一个worker会消耗一定的内存…而且也不要忽略冷启动时间。要想停止一个worker，有两种可能的方式：

1.  从主调用页面调用terminate（）命令。
2.  在worker内部通过调用close（）命令。

**演示：**你可以在浏览器中测试这个稍微增强了一点的例子：
<http://david.blob.core.windows.net/html5/HelloWebWorkers_EN.htm>

## 通过JSON发送消息

当然，大多数时候我们会发送更加结构化的数据给Workers。（顺便说一下，Web Workers也可以通过[Message channels][8]进行通讯）

但是使用JSON格式是唯一可以给worker发送结构化消息的方法。幸运的是，浏览器现在支持worker的程度已经与原生支持JSON的程度一样好了。他们真是太好了！

让我们拿出之前的代码例子。我们打算增加一个WorkerMessage类型的对象。这种类型将被用来向Web Workers发送一些带参数的命令。

我们使用下面这个简化版的HelloWebWorkersJSON_EN.htm页面：

    &lt;!DOCTYPE html&gt;
    &lt;html&gt;
    &lt;head&gt;
    &lt;title&gt;Hello Web Workers&lt;/title&gt;
    &lt;/head&gt;
    &lt;body&gt;
    &lt;div id=&quot;output&quot;&gt;&lt;/div&gt;
    &lt;script type=&quot;text/javascript&quot;&gt;
    // Instantiating the Worker
    var myHelloWorker = new Worker('helloworkers.js');
    // Getting ready to handle the message sent back
    // by the worker
    myHelloWorker.addEventListener(&quot;message&quot;, function (event) {
    document.getElementById(&quot;output&quot;).textContent = event.data;
    }, false);
    // Starting the worker by sending a first message
    myHelloWorker.postMessage(&quot;David&quot;);
    // Stopping the worker via the terminate() command
    myHelloWorker.terminate();
    &lt;/script&gt;
    &lt;/body&gt;
    &lt;/html&gt;

我们使用一种非侵入式的JavaScript方法来帮助我们分离表现层和逻辑层。然后绑定的逻辑就存在于HelloWebWorkersJSON_EN.js文件中：

    // HelloWebWorkersJSON_EN.js associated to HelloWebWorkersJSON_EN.htm
    // Our WorkerMessage object will be automatically
    // serialized and de-serialized by the native JSON parser
    function WorkerMessage(cmd, parameter) {
    this.cmd = cmd; this.parameter = parameter;
    }
    // Output div where the messages sent back by the worker will be displayed
    var _output = document.getElementById("output");
    /* Checking if Web Workers are supported by the browser */
    if (window.Worker) {
    // Getting references to the 3 other HTML elements
    var _btnSubmit = document.getElementById("btnSubmit");
    var _inputForWorker = document.getElementById("inputForWorker");
    var _killWorker = document.getElementById("killWorker");
    // Instantiating the Worker
    var myHelloWorker = new Worker('helloworkersJSON_EN.js');
    // Getting ready to handle the message sent back
    // by the worker
    myHelloWorker.addEventListener("message", function (event) {
    _output.textContent = event.data;
    }, false);
    // Starting the worker by sending it the 'init' command
    myHelloWorker.postMessage(new WorkerMessage('init', null));
    // Adding the OnClick event to the Submit button
    // which will send some messages to the worker
    _btnSubmit.addEventListener("click", function (event) {
    // We're now sending messages via the 'hello' command
    myHelloWorker.postMessage(new WorkerMessage('hello', _inputForWorker.value));
    }, false);
    // Adding the OnClick event to the Kill button
    // which will stop the worker. It won't be usable anymore after that.
    _killWorker.addEventListener("click", function (event) {
    // Stopping the worker via the terminate() command
    myHelloWorker.terminate();
    _output.textContent = "The worker has been stopped.";
    }, false);
    } else {
    _output.innerHTML = "Web Workers are not supported by your browser. Try with IE10: <a href=\"http://ie.microsoft.com/testdrive\">download the latest IE10 Platform Preview</a>";
    }

再次说明，这个例子是非常基础的。但是，它可以帮助你理解背后的逻辑。当然，没人能阻止你发送一些可以被人工智能或者物理引擎处理的游戏元素。

**演示：**你可以在这儿测试JSON的例子：
<http://david.blob.core.windows.net/html5/HelloWebWorkersJSON_EN.htm>

## 浏览器支持

Web Workers刚刚出现在IE10平台预览版。Firefox（3.6以上），Safari（4.0以上），Chrome和Opera11也都支持。然而，这些浏览器的手机版并不支持。如果你想获得更详尽的浏览器支持列表，可以看看这里：<http://caniuse.com/#search=worker>

为了实时地了解你的代码的支持情况，请使用**特性监测机制**。（你不应该使用神马用户代理嗅探！）

为了帮助你实现，这里有2个可用的解决方案。第一个是用这样一小段代码，你自己简单地测试特性：

    /* Checking if Web Workers are supported by the browser */ if (window.Worker) { // Code using the Web Workers }

第二个是使用著名的[Modernizr][9]库（现在已经原生的移到了ASP.NET的MVC3项目模版中）。然后，只要用下面这样一段代码：

    &lt;script type=&quot;text/javascript&quot;&gt;
     var divWebWorker = document.getElementById(&quot;webWorkers&quot;);
    if (Modernizr.webworkers) {
    divWebWorker.innerHTML = &quot;Web Workers ARE supported&quot;;
    } else {
    divWebWorker.innerHTML = &quot;Web Workers ARE NOT supported&quot;;
    }
    &lt;/script&gt;

例如，这里就是你的浏览器支持情况：Web Workers **are not** supported inside your browser.（原文页面对当前浏览器支持情况进行监测并将结果展示在这里。[译者][10]注。）

这将使你的应用产生两个版本。如果Web Workers不被支持，你就正常执行你的JavaScript代码。如果是在大多数现代浏览器中，Web Workers是被支持的，你就可以推送一些JavaScript代码给workers用来加强你的应用的性能。这样你就不必中断任何事情或者仅仅为最新的浏览器单独建立一个版本了。它在全部浏览器中都能工作，只是性能稍有差别。

## Worker不能访问的元素（Worker不能干什么）

与其看看你用Workers不能干什么，不如让我们了解一下你只能用worker干点儿什么：

（下面是几个表格）

Method Description
void close(); Terminates the worker thread.
void importScripts(urls); A comma-separated list of additional JavaScript files.
void postMessage(data); Sends a message to or from the worker thread.

Attributes Type Description
location WorkerLocation Represents an absolute URL, including protocol, host, port, hostname, pathname, search, and hash components.
navigator WorkerNavigator Represents the identity and onLine state of the user agent client.
self WorkerGlobalScope The worker scope, which includes the WorkerLocation and WorkerNavigator objects.

Event Description
onerror A runtime error occurred.
onmessage Message data received.

Method Description
void clearInterval(handle); Cancels a timeout identified by handle.
void clearTimeout(handle); Cancels a timeout identified by handle.
long setInterval(handler, timeout value, arguments); Schedules a timeout to be run repeatedly after the specified number of milliseconds. Note that you can now pass additional arguments directly to the handler. If handler is a DOMString, it is compiled as JavaScript. Returns a handle to the timeout. Clear with clearInterval.
long setTimeout(handler, timeout value, arguments); Schedules a timeout to run after the specified number of milliseconds. Note that you can now pass additional arguments directly to the handler. If handler is a DOMString, it is compiled as JavaScript. Returns a handle to the timeout. Clear with clearTimeout.

**注意：**这些表格是从MSDN文档中引用的：[HTML5 Web Worker][11]

总之，你**没有操作DOM的权限**。这有一个非常好的图表作为总结：

![原文配图][12]

例如，既然你在worker中没有对window对象的操作权限，你就不能操作本地存储（Local Storage，反正看起来也不像线程安全的）。那些限制对于在其他环境中使用多线程操作的开发者来说或许看起来过于严格了。然而，最大的优点是我们不会陷入我们经常遇到的问题：死锁，竞争条件等。对于这些，在Web Workers中我们都不用考虑。这使得当使用Web Workers在一些特殊的场景中允许一些有趣的性能增强时变得非常好用。

## 错误处理与调试

处理Web Workers的错误非常容易。你只需要用与注册OnMessage事件同样方法注册一个OnError事件即可：



 

    myWorker.addEventListener("error", function (event) { _output.textContent = event.data; }, false);

这是Web Worker能给你的帮助你调试代码的最好的原生支持了…不过这个非常有限不是吗？

## 通过F12开发工具获得更好的调试体验

为了突破这些局限，**IE10在它的脚本调试器中为你提供了一个直接调试Web Workers代码的功能**，就像调试其他脚本一样。

对此，你需要通过F12健运行开发者工具栏，并且点击“脚本“页卡。你应该还看不到与你的worker相关的JS文件。但是一旦点击”开始调试“按钮，它就应该神奇地出现了：

![原文配图][13]

下一步就是像调试你以往的JavaScript代码一样调试你的worker了！

![原文配图][14]

IE10是目前唯一支持这样调试的浏览器。如果你想了解更多关于这个特性的细节，你可以读一下这篇文章：[Debugging Web Workers in IE10][15]

## 一个用来模拟console.log()的有趣方法

最后，你要知道在worker中是不能使用console对象的。因此，如果你需要通过.log()方法来跟踪worker内部发生了什么，它将不会工作，因为console对象没有定义。幸好，我找到一个有趣的方法，通过MessageChannel：[console.log() for Web Workers][16].可以**模拟console.log()**行为。该方法在IE10，Chrome和Opera中工作良好，但是在Firefox中还不行，因为Firefox还不支持MessageChannel。

**注意：**为了使这个链接中的例子在IE10下能工作，你需要把下面这行代码：

    console.log.apply(console,args); // Pass the args to the real log

修改成：

    console.log.apply(console, args); // Pass the args to the real log

然后，你应该可以得到这样的结果：

![原文配图][17]

**例子：**如果你想使用这个console.log()模拟，请到这里：<http://david.blob.core.windows.net/html5/HelloWebWorkersJSONdebug.htm>

## 用例分析与如何识别潜在的候选者（使用场景）

-Web Workers 用在什么场景？

当你在网上查找Web Workers的例子时，你总会找到一类的例子：强化的数学/科学计算。然后你会看到一些JavaScript光线跟踪，分形，素数之类的东西。虽然是理解Workers工作方式的很好的例子，但是很少能给我们一些关于如何在”真实的世界“中的应用中使用它们具体观点。

确实，我们上面所看到的这些Web Workers自身的不足缩小了能使用Web Workers的有趣场景的范围。尽管如此，如果你花点儿时间仔细想想，你就会发现一些新的有趣用途：

*   **图像处理** 通过使用从&lt；canvas&gt；或者`&lt;video&gt;`元素中获取的数据。你可以把图像分割成几个不同的区域并且把它们推送给并行的不同Workers。这样你就会在新一代的多核处理器中受益。你受益越多，你运行的就越快。
*   **大量数据** 检索，你需要在调用 XMLHTTPRequest后处理大量的数据。如果处理这些数据所需的时间长短非常重要，你最好在Web Worker中来做这些，避免冻结UI线程。这样你可以保持一个可交互的应用。
*   **背景数据分析：**由于在使用Web Workers的时候，我们有更多潜在的CPU可用时间，我们现在可以考虑一下JavaScript中的新应用场景。例如，我们可以想像在不影响UI体验的情况下实时处理用户输入。利用这样一种可能，我们可以想像一个像Word（Office Web Apps 套装）一样的应用：当用户打字时后台在词典中进行查找，帮助用户自动纠错等等。
*   **针对本地数据的并发请求**。IndexDB 将提供本地存储（Local Storage）所不能提供给我们的特性：一个针对Web Workers的线程安全的存储环境。

此外，如果你想转到视频游戏的世界，你可以考虑推送人工智能或者物理引擎的数据到Web Workers。例如，我做了这样一个小实验：[On Web Workers, GWT, and a New Physics Demo ][18]，该实验使用[Box2D physic engine][19]和Workers。对于你的人工智能引擎，这也意味着你可以使用同样的时间帧来处理更多的数据（例如在棋类游戏中预测更多的步数）。

我的一些同事或许会说唯一的限制就是你的想象力！

但是一般来说，只要你不需要DOM，任可能影响用户体验的耗时的JavaScript代码都是一个使用Web Workers很好的候选。然而，使用Workers时你还需要注意以下三点：

1.  worker的初始化时间和通讯时间不应该比自身的处理时间长。
2.  使用多个Workers时的内存消耗。
3.  代码块之间的依赖关系，你可能需要一些同步的逻辑。并行没那么简单我的朋友！

从我们的角度，我们最近发布了一个演示，叫做[Web Workers Fountains][20]:

![原文配图][21]

这个例子展示了一些颗粒效果（喷泉）并且对每个喷泉使用一个Web Worker来尽可能快地计算这些粒子。每个Worker的结果汇总后显示在`&lt;canvas&gt;`元素中。Web Workers也可以在通过 Message Channels 在它们之间交换信息。在该例中，这被用来询问每个Workers何时改变喷泉的颜色。我们之后循环这组颜色数组：红色，橙色，黄色，绿色，蓝色，紫色和粉色，这都归功于Message Channels。如果你对细节感兴趣，请跳到 Demo3.js 文件中的 LightManager() 函数部分。

而且，你可以在随意在 Internet Exploer 10 中运行这个例子，非常好玩！

## 如何识别你代码中的热点

为了追踪代码的瓶颈并且识别代码中的哪部分可以发送给Web Workers，你可以使用IE9/10中提供的F12工具栏里面的脚本探查器。它可以帮助你识别你代码中的热点。然而，识别一个热点并不意味着你已经找到一个适合Web Workers的好候选。为了更好地理解这些，我们来review两个有趣的案例。

## 案例1：`&lt;canvas&gt;`中的速读动画演示

这个演示是从 [IE Test Drive][22]获取的，并且可以直接在这儿找到：[Speed Reading][23]。该例试图使用`&lt;canvas&gt;`来尽可能快地显示字符。其目的是强调你的浏览器执行硬件加速层的质量。但是除此之外，把一些操作分割成线程能获得更好的性能吗？我们需要做一些分析来验证一下。

如果你在IE9/10中运行这个例子，你可以同时在几秒之内打开探查器。下面是你得到的结果：

![enter image description here][24]

如果你降序排列那些比较耗时的方法，你会清楚地看到那些最先出现的方法：DrawLoop()，Draw()和drawImage()。如果你双击Draw这一行，就会跳到这个方法对应的代码。你会看到几个这种类型的调用：



 

    surface.drawImage(imgTile, 0, 0, 70, 100, this.left, this.top, this.width, this.height);

这里surface对象引用了一个`&lt;canvas&gt;`元素。

通过简短的分析，我们可以得到一个初步的结论，这个例子通过drawImage()方法花费了大部分的时间在Canvas内部绘图。由于Web Worker无法获取`&lt;canvas&gt;`元素，我们无法将这个耗时的任务分离到其他的线程中（例如我们可以想像一些并发处理`&lt;canvas&gt;`元素的方法）。这个例子就不是一个很好的用Web Workers处理并行的候选。

但是它很好地说明了你应该落实的操作过程。如果经过一些探查工作后，你发现耗时的脚本的主要部分与DOM对象紧密耦合，那么Web Workers就没办法帮你的Web app增强性能了。

## 案例2：`&lt;canvas&gt;`元素中的光线追踪

我们再举一个简单的例子帮助理解。我们拿一个这样的光线追踪的例子进行说明：[Flog.RayTracer Canvas Demo][25]。光线追踪使用一些CPU密集型的数学计算，据此来模拟光线的路径。这个主意是用来模拟一些诸如反射，折射，材质等效果的。

当我们运行脚本探察器时，你应该得到类似这样的结果：

![原文配图][26]

再一次的，如果我们降序排列这些方法，有2个方法明显地占用了大多数时间：renderScene()和getPixelColor()。

getPixelColor()方法的目的是计算当前的像素。光线追踪是一个像素一个像素的渲染场景。这个getPixelColor()方法之后再调用rayTrace()方法接管渲染阴影，环境光等等操作。这是我们的应用的核心部分。并且如果你review一下rayTrace()这个方法的代码，你就会发现它是100%原汁原味的JavaScript。这些代码没有任何DOM依赖。好吧，我相信你懂的：这个例子非常适合并行处理。此外，我们很容易将图像渲染拆分到几个线程中（也因此可能在几个CPU中），由于每个像素的计算之间没有同步进行的必要。每个像素的操作与它们的邻居是独立的，因为在本例中没有使用抗锯齿。

这样一来，如果我们发现一些使用Web Workers实现光线追踪的例子就不会奇怪了，比如这个：<http://nerget.com/rayjs-mt/rayjs.html>

使用IE10探查这个光线追踪例子，我们可以看出不使用Worker和使用4个Worker的显著区别：

![enter image description here][27]

在第一个截图中，processRenderCommand()方法几乎占据了全部可用CPU，并且场景绘制耗时2.854秒。

使用4个Web Workers之后，processRenderCommand()方法在4个并行的线程中执行。我们甚至可以在右侧一栏看到它们的Worker Id。这次场景绘制耗时1.473秒。受益是真实存在的：场景绘制快了一倍！

## 结论

关于Web Workers，没有什么神奇的或者新的概念用来回顾/构建你的JavaScript代码使之可以并行执行的。你需要将代码中的加强部分独立出来。它需要与你页面中的其他逻辑相对独立，避免等待同步的任务执行。并且最重要的部分是：代码不能跟DOM有耦合。如果所有这些条件都具备了，那就考虑一下Web Workers。它们绝对可以提高你的Web app的总体性能！

## 附加资源

这有一些有趣的附加资源，有兴趣可以读一下：

*   [Web Workers: Background JavaScript Makes Web Apps Faster][28]
*   [An Introduction to HTML 5 Web Workers][29]
*   [A Deeper Look at HTML 5 Web Workers][30]
*   [A very nice illustration of Web Workers][31]
*   [Official W3C Web Workers Spec][32]
*   [Web Workers in IE10: Developer Guide][33]

 [1]: http://jiguang.github.com/index.php/tag/html5/ "html5"
 [2]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image001.jpg
 [3]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image002.jpg
 [4]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image003.jpg
 [5]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image004.jpg
 [6]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://xebee.xebia.in/2010/11/02/multithreading-in-javascript-with-web-workers/
 [7]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://www.whatwg.org/specs/web-apps/current-work/complete/workers.html
 [8]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://www.w3.org/TR/webmessaging/#messagechannel
 [9]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://www.modernizr.com/
 [10]: http://laserji.com/
 [11]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://msdn.microsoft.com/fr-fr/ie/hh272905.aspx#_HTML5WebWorker
 [12]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image007.jpg
 [13]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image008-1.jpg
 [14]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image009-1.jpg
 [15]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://blogs.msdn.com/b/ie/archive/2011/07/12/debugging-web-workers-in-ie10.aspx
 [16]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://www.davidflanagan.com/2011/01/consolelog-for.html
 [17]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image010.jpg
 [18]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://extremelysatisfactorytotalitarianism.com/blog/?p=932
 [19]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://box2d-js.sourceforge.net/
 [20]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://ie.microsoft.com/testdrive/Graphics/WorkerFountains/Default.html
 [21]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image011.jpg
 [22]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://ie.microsoft.com/testdrive/
 [23]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://ie.microsoft.com/testdrive/Performance/SpeedReading/Default.html
 [24]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image013.jpg
 [25]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://labs.flog.co.nz/raytracer/
 [26]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image014.jpg
 [27]: http://www.codeproject.com/KB/solution-center/HTML5-Web-Workers/image015.jpg
 [28]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://blogs.msdn.com/b/ie/archive/2011/07/01/web-workers-in-ie10-background-javascript-makes-web-apps-faster.aspx
 [29]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://cggallant.blogspot.com/2010/08/introduction-to-html-5-web-workers.html
 [30]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://cggallant.blogspot.com/2010/08/deeper-look-at-html-5-web-workers.html
 [31]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://wearehugh.com/public/2010/08/html5-web-workers/
 [32]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://www.w3.org/TR/workers/
 [33]: http://ad.doubleclick.net/click%3bh%3dv8/3bf2/3/0/%2a/f%3b247991221%3b0-0%3b1%3b65699690%3b5046-1/3%3b44798311/44816099/1%3b%3b~sscs%3d%3fhttp://msdn.microsoft.com/en-us/ie/hh272905.aspx#_HTML5WebWorker