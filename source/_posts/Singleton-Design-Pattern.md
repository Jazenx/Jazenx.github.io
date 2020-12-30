---
title: 设计模式-单例模式
date: 2020-3-20 17:41:12
tags: Design-Pattern
---
单例设计模式(Singleton Design Pattern)。属于设计模式中的创建型设计模式。

一个类只能创建一个对象或者实例，这个类就是所谓的单例类。这种设计模式就是单例模式。

我们可以使用单例模式在系统中维持一份状态数据，比如 redux 和 vuex 中 store 的实现。除此之外，单例模式还能解决资源访问冲突的问题。

## 实现思路

下面我将 JavaScript 来阐述单例模式的实现思路。主要为饿汉式和懒汉式：

### 饿汉式

为了实现单例模式。我们实现的类在被 new 去创建实例的时候，无论创建多少次，返回总是唯一的那个实例。为此我们让构造函数实现判断是否创建过实例的功能。

在 class 中实现一个静态方法来实现:

```js
class SingleDog {
  constructor() {
	this.instance = new SingleDog()
  }

  woof() {
	console.log('love me')
  }

  static getInstance() {
	return this.instance
  }
}

const a = SingleDog.getInstance()
const b = SingleDog.getInstance()

console.log(a === b) // true
```

使用饿汉式的思路的话，需要我们进行提前初始化实例。将耗时的初始化操作，提前到程序启动的时候完成，这样就能避免在程序运行的时候，再去初始化导致的性能问题。

### 懒汉式

懒汉式相对于饿汉式的优势是支持延迟加载。在我们需要的时候去创建实例。

```js
class SingleDog {
  constructor() {
	this.instance = null
  }

  woof() {
	console.log('love me')
  }

  static getInstance() {
	if(!this.instance) {
	  this.instance = new SingleDog()
	}
	return this.instance
  }
}

const a = SingleDog.getInstance()
const b = SingleDog.getInstance()

console.log(a === b) // true
```



对于其他语言还有如下方式创建。例如 Java。可以自行去了解下。

### 双重检测

饿汉式不支持延迟加载，懒汉式有性能问题，不支持高并发。那么有没有既支持延迟加载、又支持高并发的单例实现方式？有的它就是双重检测方式。实现思路是只要 instance 被创建之后，再调用 getInstance() 函数都不会进入到加锁逻辑中。

### 静态内部类

利用 Java 的静态内部类来实现单例。这种实现方式，既支持延迟加载，也支持高并发，实现起来也比双重检测简单。

### 枚举

基于枚举类型的单例实现。这种实现方式通过 Java 枚举类型本身的特性，保证了实例创建的线程安全性和实例的唯一性。

## 单例模式的缺点

- 单例对 OOP 特性的支持不友好。违背了广义上的抽象特性。
- 单例会隐藏类之间的依赖关系。
- 单例对代码的扩展性不友好。
- 单例对代码的可测试性不友好。
- 单例不支持有参数的构造函数。

## 参考资料

[设计模式之美_设计模式_代码重构-极客时间](https://time.geekbang.org/column/intro/250)
