---
title: this
toc: true
date: 2017-07-13  12:51:28
tags: [前端,Javascript,this]
---


## 前置

>this总是指向函数的直接调用者（而非间接调用者）

this在运行时绑定，上下文取决于函数调用时各种条件，和函数声明的位置没关系，只取决于函数的调用方式


## 绑定规则

隐式绑定,对象属性引用链中只有最后一层在调用位置起作用

```

function foo() { console.log(this.a) }
var obj2 = { a: 42, foo: foo }
var obj1 = { a: 2, obj2: obj2 }
var obj3 = { a: 233, foo: foo }
obj3.foo() //2333 obj1.obj2.foo() //42

```

隐式丢失

```

function foo() { console.log(this.a) }

function doFoo(fn) { fn() }
var obj3 = { a: 233, foo: foo }
var a = "windows"
doFoo(obj3.foo) // windows obj3.foo() //233

```

显示绑定 call apply

new 绑定
创建一个新对象
新对象被执行[prototype]链接

新对象会绑定到函数调用的this
返回这个新对象

优先级从高到低：new,显示,隐式,默认


*在全局运行上下文中（在任何函数体外部），this指代全局对象

` console.log(this===window);// true
`



## call与apply,bind的区别


>call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 this 的指向

>call, apply方法区别是,从第二个参数起, call方法参数将依次传递给借用的方法作参数, 而apply直接将这些参数放到一个数组中再传递, 最后借用方法的参数列表是一样的.

>在知道调用函数的参数数量时，使用 call() 的性能会优于 apply()。

>主要在实现的过程中 apply() 需要完成额外的操作（判断第二个参数类数组的长度，etc.）更严谨的说法是，当有this指向或者执行参数时，call的性能要明显优于apply。


 bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this.

传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数


```

var foo = { bar: 1, eventBind: function() {
   $('.someClass').on('click', function(event) { 
    console.log(this.bar); }.bind(this));
   }
 }


```

bind 是返回对应函数，便于稍后调用；apply 、call 则是立即调用



```

var obj = { x: 81, };
var foo = {
    getX: function() {
        return this.x;
    }
}
console.log(foo.getX.bind(obj)()); //81
console.log(foo.getX.call(obj)); //81 
console.log(foo.getX.apply(obj)); //81


```

* 实现BIND函数

基本原理就是使用 apply() 与闭包，返回包含 apply() 的闭包使得 apply() 绑定指定作用域，但并未执行。


```

Function.prototype.bind = Function.prototype.bind || function (context) {
  var self = this;
  return function () {
    return self.apply(context, arguments);
  }
}


```


## 知识点
- this 的指向如何确定
- call, bind, apply
- bind 的 polyfill 写法

## 初级前端的 this 指向问题
面试过程中考察基础有一道比问题：“this 的指向如何确定？”

如何解答这道题目？

我会这样回到：

## 从作用域说起

作用域分为动态作用域与词法作用域，它们主要的区别是：

1. 词法作用域是在写代码或者说定义的时候确定的，词法作用域关注函数在何处声明
2. 动态作用域是在运行的时候确定的，而动态作用域关注函数从何处调用。

**this 就可以类比成 js 中的动态作用域**

`this` 是在运行时进行绑定的，并不是在编写时绑定，它的上下文取决于函数调用的各种条件。`this` 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。

### 根据优先级判断 this 值指向

如果遇到 `this` 的判断问题，可按照下面的顺序来判断：

#### 函数是否在 new 中调用（new 绑定）？如果是的话，this 绑定的是新创建的对象。
{% codeblock lang:js %}
function Foo (something) {
  this.a = something
}

let bar = new Foo(3)
console.log(bar.a) // 3
{% endcodeblock %}

#### 函数是否通过 call, apply, bind 绑定？如果是的话，this 绑定的是指定的对象。

先看 `call`
{% codeblock lang:js %}
function foo () {
  console.log(this.a)
}
const obj = {
  a: 2
}
foo.call(obj) // 2
{% endcodeblock %}

接着看 `bind`
{% codeblock lang:js %}

function foo (sth) {
  return this.a + sth
}
let obj = {
  a: 2
}

let bar = foo.bind(obj)

let b = bar(3)

console.log(b) // 5
{% endcodeblock %}

当说到这里的时候，**可以提一下 `call`、`apply`、`bind` 的区别**，这里介绍一篇文章[call apply bind 区别](http://www.jianshu.com/p/56a9c2d11adc)

#### 函数是否在某个上下文对象中调用？如果是的话，`this` 绑定的就是指定的对象。
{% codeblock lang:js %}
const obj = {
  a: 1,
  foo: function () {
    console.log(this.a)
  }
}
obj.foo() // 1
{% endcodeblock %}

#### 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 `undefined`，否则绑定到全局对象。

非严格模式：
{% codeblock lang:js %}
var a = 678

function foo () {
  console.log(this.a)
}

var bar = foo() // 678
{% endcodeblock %}

严格模式下：
{% codeblock lang:js %}
'use strict'
var a = ''abc

function foo () {
  console.log(this.a)
}

var bar = foo() // VM3254:2 Uncaught SyntaxError: Unexpected identifier
{% endcodeblock %}

### 特殊的箭头函数

上面四条可以基本涵盖所有正常的函数，但是，`ES6` 中介绍了一种无法使用这些规则的特殊函数类型，箭头函数。

箭头函数是使用 “胖箭头” 的操作符 `=>` 定义的。箭头函数不使用 `this` 的四种标准规则，而是根据外层（函数或者全局）作用域来决定 `this`。

{% codeblock lang:js %}
function foo() {
  return (a) => {
    // this继承自 foo()
    console.log(this.a)
  }
}
var obj1 = {
  a: 2
}

var obj2 = {
  a: 2
}

var bar = foo.call(obj1)
bar.call(obj2) // 2
{% endcodeblock %}

`foo()` 内部创建的箭头函数会捕获调用时 `foo()` 的 `this`。由于 `foo()` 的 `this` 绑定到 `obj1`，`bar`（引用箭头函数）的 `this` 也会绑定到 `obj1`，箭头函数的绑定无法被修改（`new` 也不行！）

以上，是初级前端考察 `this` 值指向的一般解答。那么中级前端呢？

## 中级前端的 this 指向问题
对于中级前端，一般就是直接上手写代码了，对于 this 值得指向，有一道比较经典的笔试题：“模拟实现 ES5 中原生 bind 函数”。

这道面试考察到以下几个点：
1. `bind` 函数
2. 闭包
3. 考察代码的风格以及编写能力

### 初级的实现
对于这道题目，最简单的解答版本如下：
{% codeblock lang:js %}
Function.prototype.bind = function (context) {
  var me = this
  var args = Array.prototype.slice(arguments)
  return function () {
    me.apply(context, args.slice(1))
  }
}
{% endcodeblock %}

这个版本呢，就是利用 `apply` 来做模拟，将第一个参数（context）以外的其他参数，作为提供给原函数的预设参数。

这个版本的写法基本满足了 polyfill 的需求，但是，
1. 假如调用 bind 的不是一个函数呢？
2. 假如这个浏览器本身就支持 bind 函数呢？

针对第一个问题，我们可以加一个判断：
{% codeblock lang:js %}
Function.prototype.bind = function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable')
  }
}
{% endcodeblock %}

{% codeblock lang:js %}
// 嗅探进化版本
Function.prototype.bind = Function.prototype.bind || function (context) { .... }
{% endcodeblock %}

### 更好的颗粒化
上面的写法实现了最基本的 bind，也做了嗅探处理，但是，**这种写法直接预设了参数丢失的情况**。

假如在返回的函数中，想要传递参数呢？

改进的方法如下：
{% codeblock lang:js %}
Function.prototype.bind  = Function.prototype.bind || function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable')
  }
  var me = this
  var outterArgs = Array.prototype.slice(arguments, 1)
  return function () {
    var innerArgs = Array.prototype.slice(arguments, 1)
    var finalArgs = outterArgs.concat(innerArgs)
    return me.apply(context, finalArgs)
  }
}
{% endcodeblock %}

### 构造函数的处理
对于上面的代码，polyfill 完成了 70% 了，还有一个情况，没有进行处理，那就是：bind 返回的函数如果作为构造函数，搭配 new 关键字出现的话，我们的绑定 this 就需要“被忽略”。

对于 new 关键字的情况，我们需要处理一下原型链了。

{% codeblock lang:js %}
Function.prototype.bind = Function.prototype.bind || function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable')
  }
  var me = this
  var outterArgs = Array.prototype.slice(arguments, 1)
  var F = function () {}
  F.prototype = this.prototype
  var bound = function () {
    var innerArgs = Array.prototype.slice(arguments, 1)
    var finalArgs = outterArgs.concat(innerArgs)
    return me.apply(this instanceof F ? this : context || this, finalArgs)
  }
  bound.prototype = F.prototype
  return bound
}
{% endcodeblock %}

对此，bind 的 polyfill 写法已经基本完成了。

上面的写法基本跟[MDN 给出的 polyfill](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) 一致了。

好了，关于 bind 的 polyfill 基本就到这里了。

