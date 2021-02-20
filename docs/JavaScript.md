## JavaScript 部分

### 1.引起内存泄漏的操作有哪些

1.全局变量引起

2.闭包引起

3.dom 清空，事件未清除

4.子元素存在引用

5.被遗忘的计时器

参考：

1. [【译】JavaScript 内存泄漏问题](http://octman.com/blog/2016-06-28-four-types-of-leaks-in-your-javascript-code-and-how-to-get-rid-of-them/)

2. [JavaScript 常见的内存泄漏原因](https://juejin.im/entry/58158abaa0bb9f005873a843)

### 2.如何实现 ajax 请求

1.通过实例化一个 XMLHttpRequest 对象得到一个实例，调用实例的 open 方法为这次
ajax 请求设定相应的 http 方法、相应的地址和以及是否异步，当然大多数情况下我们都是选异步，
以异步为例，之后调用 send 方法 ajax 请求，这个方法可以设定需要发送的报文主体，然后通过
监听 readystatechange 事件，通过这个实例的 readyState 属性来判断这个 ajax 请求的状态，其中分为 0,1,2,3,4 这四种
状态，当状态为 4 的时候也就是接收数据完成的时候，这时候可以通过实例的 status 属性判断这个请求是否成功

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'aabb.php', true);
xhr.send(null);
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4) {
    if (xhr.status == 200) {
      console.log(xhr.responseText);
    }
  }
};
```

### 3.简要介绍 ES6

ES6 在变量的声明和定义方面增加了 let、const 声明变量，有局部变量的概念，赋值中有比较吸引人的结构赋值，同时 ES6 对字符串、
数组、正则、对象、函数等拓展了一些方法，如字符串方面的模板字符串、函数方面的默认参数、对象方面属性的简洁表达方式，ES6 也
引入了新的数据类型 symbol，新的数据结构 set 和 map,symbol 可以通过 typeof 检测出来，为解决异步回调问题，引入了 promise 和
generator，还有最为吸引人了实现 Class 和模块，通过 Class 可以更好的面向对象编程，使用模块加载方便模块化编程，当然考虑到
浏览器兼容性，我们在实际开发中需要使用 babel 进行编译。

### 4.对 js 原型的理解

我们知道在 es6 之前，js 没有类和继承的概念，js 是通过原型来实现继承的。在 js 中一个构造函数默认自带有一个 prototype 属性，
这个的属性值是一个对象，同时这个 prototype 对象自带有一个 constructor 属性，这个属性指向这个构造函数，同时每一个实例
都有一个**proto**属性指向这个 prototype 对象，我们可以将这个叫做隐式原型，我们在使用一个实例的方法的时候，会先检查
这个实例中是否有这个方法，没有则会继续向上查找这个 prototype 对象是否有这个方法，刚刚我们说到 prototype 是一个对象，
那么也即是说这个是一个对象的实例，那么这个对象同样也会有一个**proto**属性指向对象的 prototype 对象。

### 5.对 js 模块化的理解

在 ES6 出现之前，js 没有标准的模块化概念，这也就造成了 js 多人写作开发容易造成全局污染的情况，以前我们可能会采用立即执行
函数、对象等方式来尽量减少变量这种情况，后面社区为了解决这个问题陆续提出了 AMD 规范和 CMD 规范，这里不同于 Node.js 的
CommonJS 的原因在于服务端所有的模块都是存在于硬盘中的，加载和读取几乎是不需要时间的，而浏览器端因为加载速度取决于网速，
因此需要采用异步加载，AMD 规范中使用 define 来定义一个模块，使用 require 方法来加载一个模块，现在 ES6 也推出了标准的模块
加载方案，通过 export 和 import 来导出和导入模块。

### 6.如何实现一个 JS 的 AMD 模块加载器

AMD 是解决 JS 模块化的规范，实现这样的一个模块加载器的关键在于解决每个模块依赖的解析。首先我们需要有一个模块的入口，也就是主模块，比如我们使用
一个 use 方法作为入口，之后以数组的形式列出了主模块的依赖，这时候我们要想到的是如何解析这一个一个的依赖，也就是如何解析出一个个 js 文件的绝对地址，
我们可以制定一个规则，如默认为主模块的路径为基准，也可以像 requirejs 一样使用一个 config 方法来指定一个 baseurl 和为每一个模块指定一个 path，最后就是
模块的问题，我们需要暴露一个 define 方法来定义模块，也就是模块名，依赖以及每个模块的各自代码。其中每个模块的代码都应该在依赖加载完之后执行，这就是一个
回调函数，模块的依赖、回调函数、状态、名字、模块导出等可以看做是一个模块的属性，因此我们可以使用一个对象来保存所有的模块，然后每个模块的各个属性存放在一个对象中。
最后我们来考虑一下模块加载的问题，上面我们说到 use 方法，use 方法的逻辑就是遍历依赖，然后对每个模块进行加载，也就是解析地址然后使用插入 script，我们假设
使用 loadModule 方法来加载依赖，那么这个函数的逻辑就应该是检查我们的模块是否已经加载过来判断是否需要加载，如果这个模块还有依赖则调用 use 方法继续解析，模块依赖中我们
还没有提到的问题就是每个模块的依赖是需要被传进模块里来使用的，解决方法就是每个模块的 callback 方法执行后的返回的 export 记录下来然后使用 apply 之类的方法将这些参数传递进去。
大致就是这样子的。

参考：

1. [动手实现一个 AMD 模块加载器(一)](https://github.com/huruji/blog/issues/13)

2. [动手实现一个 AMD 模块加载器(二)](https://github.com/huruji/blog/issues/16)

3. [动手实现一个 AMD 模块加载器(三)](https://github.com/huruji/blog/issues/17)

### 7.简要介绍事件代理，以及什么时候使用，事件代理发生在事件处理流程的哪个阶段，有什么好处？

事件代理就是说我们将事件添加到本来要添加事件的父节点，将事件委托给父节点来触发处理函数，这通常会在
这通常会使用在大量的同级元素需要添加同一类事件的时候，比如一个动态的非常多的列表，需要为每个列表项都添加
点击事件，这时可以使用事件代理，通过判断 e.target.nodeName 来判断发生的具体元素，从而判断是否是在
列表项中触发，这样的好处是可以减少事件绑定，同时动态的 DOM 结构仍然可以监听。事件代理发生在冒泡阶段。

参考：

1. [事件代理](http://www.bubuko.com/infodetail-2290096.html)

2. [浅析 JavaScript 的事件代理和委托](https://yq.aliyun.com/articles/185645)

### 8.使用 new 操作符实例化一个对象的具体步骤

1.构造一个新的对象

2.将构造函数的作用域赋给新对象（也就是说 this 指向了新的对象）

3.执行构造函数中的代码

4.返回新对象

### 9.js 如何判断网页中图片加载成功或者失败

使用 onload 事件运行加载成功，使用 onerror 事件判断失败

### 10.递归和迭代的区别是什么，各有什么优缺点？

程序调用自身称为递归，利用变量的原值推出新值称为迭代，递归的优点
大问题转化为小问题，可以减少代码量，同时应为代码精简，可读性好，
缺点就是，递归调用浪费了空间，而且递归太深容易造成堆栈的溢出。迭代的好处
就是代码运行效率好，因为时间只因循环次数增加而增加，而且没有额外的空间开销，
缺点就是代码不如递归简洁

参考：

[深究递归和迭代的区别、联系、优缺点及实例对比](http://blog.csdn.net/laoyang360/article/details/7855860)

### 11.策略模式是什么，说一下你的理解？

策略模式就是说我们将一系列的算法封装起来，使其相互之间可以替换，封装的算法具有一定的独立性，不会随客户端的变化而变化，比较常见的使用常见就是类似于
表单验证这种多场景的情况，我们使用策略模式就可以避免使用一堆的 if...else。

### 12.什么是事件循环（EVENT LOOP）？

我们常常说 js 是单线程的，是指 js 执行引擎是单线程的，除了这个单线程，还有一个
任务队列，在执行 js 代码的过程中，执行引擎遇到注册的延时方法，如定时器，DOM 事件，
会将这些方法交给相应的浏览器模块处理，当这些延时方法有触发条件去触发的时候，
这些延时方法会被添加至任务队列，而这些任务队列中的方法只有 js 的主线程空闲了才会执行，
这也就是说我们常常用的定时器定的时间参数只是一个触发条件，具体多少时间后执行其实还需要看
js 主线程空闲与否

参考：

[【转向 Javascript 系列】从 setTimeout 说事件循环模型](http://www.alloyteam.com/2015/10/turning-to-javascript-series-from-settimeout-said-the-event-loop-model/)

[深入浅出 Javascript 事件循环机制(上)](https://zhuanlan.zhihu.com/p/26229293)

[深入浅出 JavaScript 事件循环机制(下)](https://zhuanlan.zhihu.com/p/26238030)

[并发模型与事件循环](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/EventLoop)

### 13.原生 JS 操作 DOM 的方法有哪些？

获取节点的方法 getElementById、getElementsByClassName、getElementsByTagName、
getElementsByName、querySelector、querySelectorAll,对元素属性进行操作的 getAttribute、
setAttribute、removeAttribute 方法，对节点进行增删改的 appendChild、insertBefore、replaceChild、removeChild、
createElement 等

### 14.typeof 操作符返回值有哪些，对 undefined、null、NaN 使用这个操作符分别返回什么

typeof 的返回值有 undefined、boolean、string、number、object、function、symbol。对 undefined
使用返回 undefined、null 使用返回 object，NaN 使用返回 number

### 15.实现一个类型判断函数，需要鉴别出基本类型、function、null、NaN、数组、对象？

只需要鉴别这些类型那么使用 typeof 即可，要鉴别 null 先判断双等判断是否为 null，之后使用 typeof 判断，如果是 obejct 的话，再用 Array.isArray 判断
是否为数组，如果是数字再使用 isNaN 判断是否为 NaN,（需要注意的是 NaN 并不是 JavaScript 数据类型，而是一种特殊值）如下：

```javascript
function type(ele) {
  if (ele === null) {
    return null;
  } else if (typeof ele === 'object') {
    if (Array.isArray(ele)) {
      return 'array';
    } else {
      return typeof ele;
    }
  } else if (typeof ele === 'number') {
    if (isNaN(ele)) {
      return NaN;
    } else {
      return typeof ele;
    }
  } else {
    return typeof ele;
  }
}
```

### 16.javascript 做类型判断的方法有哪些？

typeof、instanceof 、 Object.prototype.toString()(待续)

### 17.JavaScript 严格模式下有哪些不同？

- 不允许不使用 var 关键字去创建全局变量，抛出 ReferenceError
- 不允许对变量使用 delete 操作符，抛 ReferenceError
- 不可对对象的只读属性赋值，不可对对象的不可配置属性使用 delete 操作符，不可为不可拓展的对象添加属性，均抛 TypeError
- 对象属性名必须唯一
- 函数中不可有重名参数
- 在函数内部对修改参数不会反映到 arguments 中
- 淘汰 arguments.callee 和 arguments.caller
- 不可在 if 内部声明函数
- 抛弃 with 语句

参考：

1.[javascript 高级程序设计](https://book.douban.com/subject/10546125/)

### 18.setTimeout 和 setInterval 的区别，包含内存方面的分析？

setTimeout 表示间隔一段时间之后执行一次调用，而 setInterval 则是每间隔一段时间循环调用，直至 clearInterval 结束。
内存方面，setTimeout 只需要进入一次队列，不会造成内存溢出，setInterval 因为不计算代码执行时间，有可能同时执行多次代码，
导致内存溢出。

参考：

[JS 中 settimeout 和 setinterval 函数的区别](https://my.oschina.net/u/3636678/blog/1499852)

[setTimeout() 和 setInterval() 本质区别在哪里？](https://segmentfault.com/q/1010000005989491)

### 19.同源策略是什么？

同源策略是指只有具有相同源的页面才能够共享数据，比如 cookie，同源是指页面具有相同的协议、域名、端口号，有一项不同就不是同源。
有同源策略能够保证 web 网页的安全性。

参考：

[前端必备 HTTP 技能之同源策略详解](http://www.jianshu.com/p/beb059c43a8b)

[浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)

[浏览器的同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)

### 20.ES6 之前 JavaScript 如何实现继承？

ES6 之前的继承是通过原型来实现的，也就是每一个构造函数都会有一个 prototype 属性，然后如果我们调用一个实例的方法或者属性，首先会在自身寻找，然后在
构造函数的 prototype 上寻找，而 prototype 本质上就是一个实例，因此如果 prototype 上还没有则会往 prototype 上的构造函数的 prototype 寻找，因此实现继承
可以让构造函数的 prototype 是父级的一个实例就是以实现继承。

### 21.如何阻止事件冒泡和默认事件？

标准的 DOM 对象中可以使用事件对象的 stopPropagation()方法来阻止事件冒泡，但在 IE8 以下中 IE 的事件对象通过设置事件对象的 cancelBubble 属性为 true 来阻止冒泡；
默认事件的话通过事件对象的 preventDefault()方法来阻止，而 IE 通过设置事件对象的 returnValue 属性为 false 来阻止默认事件。

### 22.addEventListener 有哪些参数？

有三个参数，第一个是事件的类型，第二个是事件的回调函数，第三个是一个表示事件是冒泡阶段还是捕获阶段捕获的布尔值，true 表示捕获，false 表示冒泡

### 23.介绍一下 Promise，底层如何实现？

参考：

[面试精选之 Promise](https://juejin.cn/post/6844903625609707534)

[promise 底层实现](https://www.cnblogs.com/zhouyideboke/p/12875355.html)

[介绍下 promise 的特性,优缺点，内部如何实现的，动手实现一个 promise (一)](https://www.cnblogs.com/James-net/archive/2020/11/12/13958533.html)

### 24.如何实现懒加载？

懒加载就是根据用户的浏览需要记载内容，也就是在用户即将浏览完当前的内容时进行继续加载内容，这种技术常常用来加载图片的时候使用。我们判断用户是否即将浏览到底部之后进行在家内容
这时候可能会需要加载大量的内容，可以使用 fragment 来优化一下，因为大部分是使用滑动和滚轮来触发的，因此很有可能会不断触发，可以使用函数节流做一个优化，防止用户不断触发。

### 25.函数节流是什么？

函数节流就是让一个函数无法在很短的时间间隔内连续调用，而是间隔一段时间执行，这在我们为元素绑定一些事件的时候经常会用到，比如我们
为 window 绑定了一个 resize 事件，如果用户一直改变窗口大小，就会一直触发这个事件处理函数，这对性能有很大影响。

[什么是函数节流？](http://www.alloyteam.com/2012/11/javascript-throttle/)

### 26.浏览器内核有哪些？分别对应哪些浏览器？

常见的浏览器内核有 Trident、Gecko、WebKit、Presto，对应的浏览器为 Trident 对应于 IE，Gecko 对应于火狐浏览器，Webkit 有 chrome 和 safari，Presto
有 Opera。

### 27.什么是深拷贝，什么是浅拷贝？

浅拷贝是指仅仅复制对象的引用，而不是复制对象本身；深拷贝则是把复制对象所引用的全部对象都复制一遍。

### 28.原生 js 字符串方法有哪些？

简单分为获取类方法，获取类方法有 charAt 方法用来获取指定位置的字符，获取指定位置字符的 unicode 编码的 charCodeAt 方法，
与之相反的 fromCharCode 方法，通过传入的 unicode 返回字符串。查找类方法有 indexof()、lastIndexOf()、search()、match()
方法。截取类的方法有 substring、slice、substr 三个方法，其他的还有 replace、split、toLowerCase、toUpperCase 方法。

### 29.原生 js 字符串截取方法有哪些？有什么区别？

js 字符串截取方法有 substring、slice、substr 三个方法，substring 和 slice 都是指定截取的首尾索引值，不同的是传递负值的时候
substring 会当做 0 来处理，而 slice 传入负值的规则是-1 指最后一个字符，substr 方法则是第一个参数是开始截取的字符串，第二个是截取的字符数量，
和 slice 类似，传入负值也是从尾部算起的。

### 30.SVG 和 Canvas 的区别？

svg 绘制出来的 bai 每一个图形元素都是独立 du 的 DOM 节点，zhi 可方便后期绑定事件或修改，而 daocanvas 输出的是一整 zhuan 幅画布<br />
svg 输出的图形是矢量的，后期可以修改参数来自由放大缩小，无失真，canvas 输出标量画布，就像一张图片一样

### 31.介绍一下 ES6 的暂时性死区和块级作用域

ES6 块级作用域 {} for if while<br />
块级作用域 ：只跟 let 或者 const 声明的变量起作用；对于 var 没有块级作用域的限制
<br />
暂时性死区<br />
用 var 声明过的变量 ，会有变量提升，给一个默认值是 undefined
用 let 和 const 声明的变量， 不会进行变量提升，但是他也会对里边的带 let 和 const 的变量进行预览,这时 ，浏览器不会再往上级作用域查找相应变量

### 32.请介绍一下装饰者模式，并实现

在不改变元对象的基础上，对这个对象进行包装和拓展（包括添加属性和方法），从而使这个对象可以有更复杂的功能。

### 33.介绍一下职责链模式？

将一个流程进行分解，让这个流程在多个对象中进行传递，由最后一个对象完成这个流程。通过职责链模式能够将流程进行分解，从而解耦。

### 34.请说一下实现 jsonp 的实现思路？

jsonp 的原理是使用 script 标签来实现跨域，因为 script 标签的的 src 属性是不受同源策略的影响的，因此可以使用其来跨域。一个最简单的 jsonp 就是创建一个 script 标签，设置 src 为相应的 url，在 url 之后添加相应的 callback，格式类似于
url?callback=xxx，服务端根据我们的 callback 来返回相应的数据，类似于 res.send(req.query.callback + '('+ data + ')')，这样就实现了一个最简单的 jsonp

参考：

[动手实现一个 JSONP]()

[jsonp 的原理与实现](https://segmentfault.com/a/1190000007665361)

[fetch-jsonp 源码](https://github.com/camsong/fetch-jsonp/blob/master/src/fetch-jsonp.js)
