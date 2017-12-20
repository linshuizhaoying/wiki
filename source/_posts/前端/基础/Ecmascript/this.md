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

