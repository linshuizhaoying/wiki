---
title: javascript异步编程
toc: true
date: 2017-07-12  10:58:50
tags: [前端,Javascript,异步编程]
---

## 回调函数

## setTimeout 函数


## generator

* 当你在执行一个函数的时候，你可以在某个点暂停函数的执行，并且做一些其他工作，然后再返回这个函数继续执行，甚至是携带一些新的值，然后继续执行。

* 当我们调用一个生成器函数的时候，它并不会立即执行， 而是需要我们手动的去执行迭代操作（next方法）。也就是说，你调用生成器函数，它会返回给你一个迭代器。迭代器会遍历每个中断点
 
{% codeblock lang:js %}

* function* foo () {    
              var index = 0;  
              while (index ... 2) {  
                yield index++; //暂停函数执行，并执行yield后的操作  
              }  
            }  

var bar =  foo(); // 返回的其实是一个迭代器  

            console.log(bar.next());    // { value: 0, done: false }    
            console.log(bar.next());    // { value: 1, done: false }    
            console.log(bar.next());    // { value: undefined, done: true }




{% endcodeblock %}


## Promises对象


Promise 对象用于异步调用返回值的集中处理。 Promise 对象表示一个现在、将来或永不可用的值。
* 特点
     * 对象的状态不受外界影响。
Promise对象代表一个异步操作，有三种状态：Pending（进行中）、Resolved（已完成，又称 Fulfilled）和Rejected（已失败）。
只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
     * 就算改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的

Promise 本质是一个状态机。每个 promise 只能是 3 种状态中的一种：pending、fulfilled 或 rejected。状态转变只能是 pending -> fulfilled 或者 pending -> rejected。状态转变不可逆。
then 方法可以被同一个 promise 调用多次。
then 方法必须返回一个 promise。规范里没有明确说明返回一个新的 promise 还是复用老的 promise（即 return this），大多数实现都是返回一个新的 promise，而且复用老的 promise 可能改变内部状态，这与规范也是相违背的。
值穿透。

{% codeblock lang:js %}

 * var myFirstPromise = new Promise(function(resolve, reject){  
           //当异步代码执行成功时，我们才会调用resolve(...), 当异步代码失败时就会调用reject(...)  
           //在本例中，我们使用setTimeout(...)来模拟异步代码，实际编码时可能是XHR请求或是HTML5的一些API方法.  
           setTimeout(function(){  
               resolve("成功!"); //代码正常执行！  
           }, 250);  
       });  

       myFirstPromise.then(function(successMessage){  
           //successMessage的值是上面调用resolve(...)方法传入的值.  
           //successMessage参数不一定非要是字符串类型，这里只是举个例子  
           console.log("Yay! " + successMessage);  
       });


{% endcodeblock %}

## 一个promise有多个then，如果第一个then出错，后面的还会执行吗，如何捕获异常。 如果第一个then出错了，我还想要后面的继续执行，应该怎么做？promise的catch方法回调函数出错会发生什么

不会执行。then里面抛出新异常。后面接catch。或者调用链最后用catch，然后具体错误具体分析。

catch方法返回的还是一个 Promise 对象，因此后面还可以接着调用then方法。

如果后面没有catch，导致这个错误不会被捕获，也不会传递到外层。如果后面接着catch，会被捕获处理。





## async


## await


## 考题

### 1.代码运行结果

{% codeblock lang:js %}

(function test() {
    setTimeout(function() {console.log(4)}, 0);
    new Promise(function executor(resolve) {
        console.log(1);
        for( var i=0 ; i<10000 ; i++ ) {
            i == 9999 && resolve();
        }
        console.log(2);
    }).then(function() {
        console.log(5);
    });
    console.log(3);
})()


{% endcodeblock %}




1. 当前task运行，执行代码。首先setTimeout的callback被添加到tasks queue中；
2. 实例化promise，输出 1; promise resolved；输出 2;
3. promise.then的callback被添加到microtasks queue中；
4. 输出 3;
5. 已到当前task的end，执行microtasks，输出 5;
6. 执行下一个task，输出4。

>1. 每个浏览器环境，至多有一个event loop。
2. 一个event loop可以有1个或多个task queue。
3. 一个task queue是一列有序的task，用来做以下工作：Events task，Parsing task， Callbacks task， Using a resource task， Reacting to DOM manipulation task等。

>每个(task source对应的)task queue都保证自己队列的先进先出的执行顺序，但event loop的每个turn，是由浏览器决定从哪个task source挑选task。这允许浏览器为不同的task source设置不同的优先级，比如为用户交互设置更高优先级来使用户感觉流畅。

>micro-task在ES2015规范中称为Job。 其次，macro-task代指task。

>Event Loop（事件循环）是怎么来处理task和microtask的

>1. 每个线程有自己的事件循环，所以每个web worker有自己的，所以它才可以独立执行。然而，所有同属一个origin的windows共享一个事件循环，所以它们可以同步交流。
2. 事件循环不间断在跑，执行任何进入队列的task。
3. 一个事件循环可以有多个task source，每个task source保证自己的任务列表的执行顺序，但由浏览器在（事件循环的）每轮中挑选某个task source的task。
4. tasks are scheduled，所以浏览器可以从内部到JS/DOM，保证动作按序发生。在tasks之间，浏览器可能会render updates。从鼠标点击到事件回调需要schedule task，解析html，setTimeout这些都需要。
5. microtasks are scheduled，经常是为需要直接在当前脚本执行完后立即发生的事，比如async某些动作但不必承担新开task的弊端。microtask queue在回调之后执行，只要没有其它JS在执行中，并且在每个task的结尾。microtask中添加的microtask也被添加到microtask queue的末尾并处理。microtask包括mutation observer callbacks和promise callbacks。



