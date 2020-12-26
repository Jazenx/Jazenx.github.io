---
title: 前端模块化 Modular
tag: JavaScript
date: 2018-06-12
---

前端模块化的一些总结。

## Why Modular

- 网站正在转变为Web应用程序
- 随着站点变得越来越大，代码复杂性也随之增加
- 需要高度解耦 JS 文件和模块
- 当网站部署的时候，希望在HTTP调用中优化代码

## 一段小的历史

### 匿名函数

最早我们这么写代码：

```js
function foo () {
  ...
}
  
function bar () {
  ...
}
```

这样方式 Global 会被污染，而且容易引起命名冲突。

针对上面的代码，我们可以进行简单的封装，可以使用 NameSpace 模式：

```js
var App = {
  foo: function () {...},
  bar: function () {...}
}
                    
App.foo()
```

通过这种方式，可以减少 Global 上的变量数目，但是本质是对象，一点也不安全。

然后，我们可以可以使用闭包的方式将代码放到闭包里。使用匿名闭包的形式，并且可以往里面引入依赖

```js
var Module = (function($){
    var _$body = $("body");     // we can use jQuery now!
    var foo = function(){
        console.log(_$body);    // 特权方法
    }

    // Revelation Pattern
    return {
        foo: foo
    }
})(jQuery)

Module.foo();
```

这种匿名闭包的方式，其实就是模块模式。

### SCRIPT LOADER

但是只有对代码的封装还是不够的，我们还需要加载

当我们通过 script 标签引入代码的时候，由于加载的原因会产生以下问题

- 难以维护，按照标签内容去找对应文件并修改代码是非常操蛋的。
- 依赖模糊，不同标签之间的代码存在相互依赖的关系，但是从引入代码里无法一眼判断。
- 请求过多，N多标签的引入导致请求会非常多。

针对上面的问题，在远古时代涌现了像 LABjs (Loading And Blocking JavaScript)、YUI3 的模块化处理方案。接着就是我们前端历史主要的几个模块化方案。

## CommonJS

Node 应用由模块组成，采用 CommonJS 模块规范。每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。**在服务器端，模块的加载是运行时同步加载的；在浏览器端，模块需要提前编译打包处理。**

### 特点

- 所有代码都在模块作用域运行，不会污染全局作用域。
- 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了。以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
- 模块加载的顺序，按照其在代码中出现的顺序。

### 语法

- 暴露模块 `module.exports = value`或`exports.xxx = value`

- 引入模块 `require(xxx)` 。如果是第三方模块，xxx 为模块名；如果是自定义模块，xxx 为模块文件路径

**CommonJS暴露的模块到底是什么?** CommonJS规范规定，每个模块内部，module 变量代表当前模块。这个变量是一个对象，它的 exports 属性（即 module.exports ）是对外的接口。**加载某个模块，其实是加载该模块的 module.exports 属性**。

require命令用于加载模块文件。**require 命令的基本功能是，读入并执行一个 JavaScript 文件，然后返回该模块的 exports 对象。如果没有发现指定模块，会报错**。

### 实践

浏览器端实现借助Browserify。

### 加载机制

**CommonJS 模块的加载机制是，输入的是被输出的值的拷贝。也就是说，一旦输出一个值，模块内部的变化就影响不到这个值**。

### 缺陷

CommonJS 规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。

## AMD

**CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数。**

由于 Node.js 主要用于服务器编程，模块文件一般都已经存在于本地硬盘，所以加载起来比较快，不用考虑非同步加载的方式，所以CommonJS 规范比较适用。但是，如果是浏览器环境，要从服务器端加载模块，这时就必须采用非同步模式，因此浏览器端一般采用 AMD 规范。

### 语法

暴露模块

```js
//定义没有依赖的模块
define(function(){
   return 模块
})

//定义有依赖的模块
define(['module1', 'module2'], function(m1, m2){
   return 模块
})
```

引入模块

```js
require(['module1', 'module2'], function(m1, m2){
   使用m1/m2
})
```

### 实践

RequireJS是一个工具库，主要用于客户端的模块管理。它的模块管理遵守AMD规范，**RequireJS的基本思想是，通过define方法，将代码定义为模块；通过require方法，实现代码的模块加载**。

**AMD模块定义的方法非常清晰，不会污染全局环境，能够清楚地显示依赖关系**。AMD模式可以用于浏览器环境，并且允许非同步加载模块，也可以根据需要动态加载模块。

### 缺点

- 执行时机有异议：Reqiurejs 模块加载完毕后是立即执行。

- 模块书写风格有争议：MD 风格下，通过参数传入依赖模块的导出，破坏了 **就近声明** 原则。

- 压缩之后还有 16KB，体积过大

## CMD

CMD规范专门用于浏览器端，模块的加载是异步的，模块使用时才会加载执行。CMD规范整合了CommonJS和AMD规范的特点。在 Sea.js 中，所有 JavaScript 模块都遵循 CMD模块定义规范。

### 语法

定义暴露模块：

```js
//定义没有依赖的模块
define(function(require, exports, module){
  exports.xxx = value
  module.exports = value
})

//定义有依赖的模块
define(function(require, exports, module){
  //引入依赖模块(同步)
  var module2 = require('./module2')
  //引入依赖模块(异步)
    require.async('./module3', function (m3) {
    })
  //暴露模块
  exports.xxx = value
})
```

引入使用模块：

```js
define(function (require) {
  var m1 = require('./module1')
  var m4 = require('./module4')
  m1.show()
  m4.show()
})
```

## ES6 模块化

ES6 模块的设计思想是尽量的静态化，**使得编译时就能确定模块的依赖关系，以及输入和输出的变量**。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。

### 语法

export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。

模块默认输出, 其他模块加载该模块时，import命令可以为该匿名函数指定任意名字。

### 差异

- CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
- CommonJS 模块是运行时加载，ES6 模块是编译时输出接口

ES6 模块的运行机制与 CommonJS 不一样。**ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块**。

## 参考资料

[前端模块化详解(完整版)](https://juejin.im/post/6844903744518389768#heading-4)

[JavaScript 模块化*七日谈*](http://huangxuan.me/js-module-7day/#/)

