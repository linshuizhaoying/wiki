---
title: ES6
toc: true
date: 2017-07-13  12:51:28
tags: [前端,Javascript,ES6]
---

## Iterator

>它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署Iterator接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）
一是为各种数据结构，提供一个统一的、简便的访问接口；
二是使得数据结构的成员能够按某种次序排列；
三是ES6创造了一种新的遍历命令for...of循环，Iterator接口主要供for...of消费

Iterator的遍历过程是这样的。   
   （1）创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。  
   （2）第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员。   
  （3）第二次调用指针对象的next方法，指针就指向数据结构的第二个成员。   
  （4）不断调用指针对象的next方法，直到它指向数据结构的结束位置。

{% codeblock lang:js %}

var it = makeIterator(['a', 'b']);  

       it.next() // { value: "a", done: false }  
       it.next() // { value: "b", done: false }  
       it.next() // { value: undefined, done: true }  
       function makeIterator(array) {  
         var nextIndex = 0;  
         return {  
           next: function() {  
             return nextIndex < array.length ?  
               {value: array[nextIndex++], done: false} :  
               {value: undefined, done: true};  
           }  
         };  
       }

{% endcodeblock %}


## let

let可以将变量绑定到所在的任意作用域{…}中,它为声明的变量隐式劫持了所在的块作用域，也就是花括号中的块，每进入一次花括号就生成了一个块级域


 let 的暂存死区与错误

* 重复Let定义同一个变量会抛出错误
* 在let定义之前引用会抛出引用错误
 let的作用域是块，而var的作用域是函数

*TDZ (暂时性死区)。指代码中的变量还没有初始化而不能被引用的情况。
     * {  
       a = 2; ERROR  
       let a;  

       }
* 原理：var 会变量提升，let 不会。

{% codeblock lang:js %}

function varTest() {  
    var x = 1;  
    if (true) {  
      var x = 2;  // 同样的变量!  
      console.log(x);  // 2  
    }  
    console.log(x);  // 2  
  }  

  function letTest() {  
    let x = 1;  
    if (true) {  
      let x = 2;  // 不同的变量  
      console.log(x);  // 2  
    }  
    console.log(x);  // 1  
  }

{% endcodeblock %}


## 箭头函数

>用于将函数内部的`this`延伸到上一层作用域中。即上一次的上下文会穿透到内层。  

>箭头对上下文的绑定是强制的，无法通过apply或call方法改变。

>使用了块语句的箭头函数不会自动返回值，你需要使用 return 语句将所需值返回


## symbol

`Symbol 是JavaScript ES6 的原始数据类型`

`它能避免命名冲突的风险`

调用Symbol()创建一个新的symbol，它的值与其它任何值皆不相等

JavaScript中最常见的对象检查的特性会忽略symbol键。例如，for-in循环只会遍历对象的字符串键，symbol键直接跳过，Object.keys(obj)和Object.getOwnPropertyNames(obj)也是一样。但是symbols也不完全是私有的：用新的APIObject.getOwnPropertySymbols(obj)就可以列出对象的symbol键。另一个新的API，Reflect.ownKeys(obj)，会同时返回字符串键和symbol键

## for of

* for-of 循环语句通过方法调用来遍历各种 集合。数组、Maps 对象、Sets 对象
* 不会遍历自定义属性，不会遍历普通对象
* 可以正确响应 break、continue 和 return 语句
* 支持类数组遍历, 支持字符串字符遍历

## generators

* Generator 函数是一个状态机，封装了多个内部状态，执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态
* 两个特征。  
  一是，function关键字与函数名之间有一个星号；  
  二是，函数体内部使用yield语句，定义不同的内部状态（yield在英语里的意思就是“产出”）

## promise

Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息

对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：Pending（进行中）、Resolved（已完成，又称 Fulfilled）和Rejected（已失败）。

只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变

一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从Pending变为Resolved和从Pending变为Rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。就算改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的

Promise新建后就会立即执行

Promise是一个对象，充当异步操作和回调函数之间的中介。

     * var promise = new Promise(function(resolve, reject) {  
         // 异步操作的代码  

         if (/* 异步操作成功 */){  
           resolve(value);  
         } else {  
           reject(error);  
         }  
       });
* 每一个异步函数立刻返回一个Promise对象，每个对象指定回调函数，在异步任务完成后调用。
* Promise有三种状态:pending 未完成,resolved 已完成,rejected 失败。最终状态只有成功和失败。通过then方法添加两个回调函数处理resolved状态和rejected状态，一旦状态改变就调用。

     * po.then(function(value) {  
         // success  
       }, function(value) {  
         // failure  
       });

* 优点在于流程清晰，让回调函数变成规范链式写法，可以为多个异步操作指定同一个回调，可以为多个回调指定同一个错误处理方法。如果一个任务已经完成，再往上添加回调，会立刻执行。无须担心错过某个事件。

{% codeblock lang:js %}

var promise = new Promise(function(resolve, reject) {  
    // ... some code  

    if (/* 异步操作成功 */){  
      resolve(value);  
    } else {  
      reject(error);  
    }  
  });  

  promise.then(function(value) {  
    // success  
  }, function(error) {  
    // failure  
  });  


  function timeout(ms) {  
    return new Promise((resolve, reject) => {  
      setTimeout(resolve, ms, 'done');  
    });  
  }  

  timeout(100).then((value) => {  
    console.log(value);  
  });

* Promise对象实现的Ajax操作
     * var getJSON = function(url) {  
         var promise = new Promise(function(resolve, reject){  
           var client = new XMLHttpRequest();  
           client.open("GET", url);  
           client.onreadystatechange = handler;  
           client.responseType = "json";  
           client.setRequestHeader("Accept", "application/json");  
           client.send();  

           function handler() {  
             if (this.readyState !== 4) {  
               return;  
             }  
             if (this.status === 200) {  
               resolve(this.response);  
             } else {  
               reject(new Error(this.statusText));  
             }  
           };  
         });  

         return promise;  
       };  

       getJSON("/posts.json").then(function(json) {  
         console.log('Contents: ' + json);  
       }, function(error) {  
         console.error('出错了', error);  
       });
 
* 用法
     * 加载图片
          * var preloadImage = function (path) {  
              return new Promise(function (resolve, reject) {  
                var image = new Image();  
                image.onload  = resolve;  
                image.onerror = reject;  
                image.src = path;  
              });  
            };
     * Ajax
          * function search(term) {  
              var url = 'http://example.com/search?q=' + term;  
              var xhr = new XMLHttpRequest();  
              var result;  

              var p = new Promise(function (resolve, reject) {  
                xhr.open('GET', url, true);  
                xhr.onload = function (e) {  
                  if (this.status === 200) {  
                    result = JSON.parse(this.responseText);  
                    resolve(result);  
                  }  
                };  
                xhr.onerror = function (e) {  
                  reject(e);  
                };  
                xhr.send();  
              });  

              return p;  
            }  

            search("Hello World").then(console.log, console.error);

{% endcodeblock %}


## 模板字符串

>模板字符串中嵌入变量，需要将变量名写在${}之中。大括号内部可以放入任意的JavaScript表达式，可以进行运算，以及引用对象属性还能调用函数

```

var x = 1;  
var y = 2;  

`${x} + ${y} = ${x + y}`  
       // "1 + 2 = 3"  

`${x} + ${y * 2} = ${x + y * 2}`  
       // "1 + 4 = 5"  

var obj = {x: 1, y: 2};  
`${obj.x + obj.y}`  
       // 3  

function fn() {  
         return "Hello World";  
       }  

`foo ${fn()} bar`  
// foo Hello World bar

```


## 标签

{% codeblock lang:js %}

   var a = 5;  
       var b = 10;  

       function tag(strings, ...values) {  
         console.log(strings[0]); // "Hello "  
         console.log(strings[1]); // " world "  
         console.log(strings[2]); // ""  
         console.log(values[0]);  // 15  
         console.log(values[1]);  // 50  

         return "Bazinga!";  
       }  

       tag`Hello ${ a + b } world ${ a * b}`;  
       // "Bazinga!"

{% endcodeblock %}


## Array.from()

>Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）。

Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组

{% codeblock lang:js %}

    * Array.from(arrayLike,x =>x *x);// 等同于   

       Array.from(arrayLike).map(x =>x *x);  

       Array.from([1,2,3],(x)=>x *x)// [1, 4, 9]

* let arrayLike ={'0':'a','1':'b','2':'c',length:3};// ES5的写法   
  var arr1 =[].slice.call(arrayLike);// ['a', 'b', 'c'] // ES6的写法
   
  let arr2 =Array.from(arrayLike);// ['a', 'b', 'c']
* 只要是部署了Iterator接口的数据结构，Array.from都能将其转为数组
* 如果参数是一个真正的数组，Array.from会返回一个一模一样的新数组,而且这两个数组不会引用同一份数据。a != b

{% endcodeblock %}



## Array.of()

>Array.of方法用于将一组值，转换为数组


{% codeblock lang:js %}

 Array.of(3,11,8)// [3,11,8]   

       Array.of(3)// [3]   

       Array.of(3).length// 1
  function ArrayOf(){  
    return [].slice.call(arguments);  
  }

{% endcodeblock %}


## 数组实例的find()和findIndex()

* find方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组

* 数组实例的findIndex方法的用法与find方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1

* indexOf方法无法识别数组的NaN成员，但是findIndex方法可以借助Object.is方法做到

{% codeblock lang:js %}

   * [NaN].indexOf(NaN)  
       // -1  

       [NaN].findIndex(y => Object.is(NaN, y))  
       // 0

{% endcodeblock %}


## entries()，keys()和values()

>keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历

{% codeblock lang:js %}

 * for (let index of ['a', 'b'].keys()) {  
         console.log(index);  
       }  
       // 0  
       // 1  

       for (let elem of ['a', 'b'].values()) {  
         console.log(elem);  
       }  
       // 'a'  
       // 'b'  

       for (let [index, elem] of ['a', 'b'].entries()) {  
         console.log(index, elem);  
       }  
       // 0 "a"  
       // 1 "b"

{% endcodeblock %}


## rest参数

>用于获取函数的多余参数，这样就不需要使用arguments对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中

{% codeblock lang:js %}

* // arguments变量的写法  
  function sortNumbers() {  
    return Array.prototype.slice.call(arguments).sort();  
  }  

  // rest参数的写法  
  const sortNumbers = (...numbers) => numbers.sort();
* function push(array, ...items) {  
    items.forEach(function(item) {  
      array.push(item);  
      console.log(item);  
    });  
  }  

  var a = [];  
  push(a, 1, 2, 3)

{% endcodeblock %}


## 扩展运算符(…)

>rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列，用于展开数组。


{% codeblock lang:js %}


     * function push(array, ...items) {  
         array.push(...items);  
       }  

       function add(x, y) {  
         return x + y;  
       }  

       var numbers = [4, 38];  
       add(...numbers) // 42
* 替代数组的apply方法
     * // ES5的写法  
       function f(x, y, z) {  
         // ...  
       }  
       var args = [0, 1, 2];  
       f.apply(null, args);  

       // ES6的写法  
       function f(x, y, z) {  
         // ...  
       }  
       var args = [0, 1, 2];  
       f(...args);
     * // ES5的写法  
       Math.max.apply(null, [14, 3, 77])  

       // ES6的写法  
       Math.max(...[14, 3, 77])  

       // 等同于  
       Math.max(14, 3, 77);
     * // ES5的写法  
       var arr1 = [0, 1, 2];  
       var arr2 = [3, 4, 5];  
       Array.prototype.push.apply(arr1, arr2);  

       // ES6的写法  
       var arr1 = [0, 1, 2];  
       var arr2 = [3, 4, 5];  
       arr1.push(...arr2);
* 合并数组
     * // ES5  
       [1, 2].concat(more)  
       // ES6  
       [1, 2, ...more]  

       var arr1 = ['a', 'b'];  
       var arr2 = ['c'];  
       var arr3 = ['d', 'e'];  

       // ES5的合并数组  
       arr1.concat(arr2, arr3);  
       // [ 'a', 'b', 'c', 'd', 'e' ]  

       // ES6的合并数组  
       [...arr1, ...arr2, ...arr3]  
       // [ 'a', 'b', 'c', 'd', 'e' ]

{% endcodeblock %}


## name 属性

函数的name属性，返回该函数的函数名。

```

function foo(){}foo.name// "foo"

```

## Object.is()

>它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致

```

   * Object.is('foo', 'foo')  
       // true  
       Object.is({}, {})  
       // false

```

##  Object.assign()

>方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性

>Object.assign方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

{% codeblock lang:js %}

   * var target = { a: 1 };  

       var source1 = { b: 2 };  
       var source2 = { c: 3 };  

       Object.assign(target, source1, source2);  
       target // {a:1, b:2, c:3}

{% endcodeblock %}


## 属性遍历

* for...in
     * for...in循环遍历对象自身的和继承的可枚举属性（不含Symbol属性）
* Object.keys(obj)
     * Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含Symbol属性）
* Object.getOwnPropertyNames(obj)
     * Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（不含Symbol属性，但是包括不可枚举属性）
* Object.getOwnPropertySymbols(obj)
     * Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有Symbol属性
* Reflect.ownKeys(obj)
     * Reflect.ownKeys返回一个数组，包含对象自身的所有属性，不管是属性名是Symbol或字符串，也不管是否可枚举

## __proto__属性

* 用来读取或设置当前对象的prototype对象

* __proto__前后的双下划线，说明它本质上是一个内部属性，而不是一个正式的对外的 API，只是由于浏览器广泛支持，才被加入了 ES6。标准明确规定，只有浏览器必须部署这个属性，其他运行环境不一定需要部署

* __proto__调用的是Object.prototype.__proto__

## Object.setPrototypeOf()

>Object.setPrototypeOf方法的作用与__proto__相同，用来设置一个对象的prototype对象，返回参数对象本身


```

* let proto = {};  
       let obj = { x: 10 };  
       Object.setPrototypeOf(obj, proto);  

       proto.y = 20;  
       proto.z = 40;  

       obj.x // 10  
       obj.y // 20  
       obj.z // 40

```

## Object.getPrototypeOf()

用于读取一个对象的原型对象

## 解构赋值

* 解构赋值允许你使用类似数组或对象字面量的语法将数组和对象的属性赋给各种 变量
* [ variable1, variable2, ..., variableN ] = array;
* 解构赋值允许指定默认值

```
  let[x,y ='b']=['a'] // x='a', y='b'
  let[x,y ='b'] = ['a',undefined];// x='a', y='b'

```


* 对象的结构赋值

对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。


对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者


{% codeblock lang:js %}

* var [foo, [[bar], baz]] = [1, [[2], 3]];  
   console.log(foo);  
   // 1  
   console.log(bar);  
   // 2  
   console.log(baz);  
   // 3
* var [,,third] = ["foo", "bar", "baz"];  
   console.log(third);  
   // "baz"
* var [head, ...tail] = [1, 2, 3, 4];   

  console.log(tail); // [2, 3, 4]

{% endcodeblock %}

## Set 和 Map

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

    
* 方法
     * add(value)：添加某个值，返回Set结构本身
     * delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
     * has(value)：返回一个布尔值，表示该值是否为Set的成员。
     * clear()：清除所有成员，没有返回值
* 遍历
     * keys()：返回键名的遍历器
     * values()：返回键值的遍历器
     * entries()：返回键值对的遍历器
     * forEach()：使用回调函数遍历每个成员

{% codeblock lang:js %}

 * const s = new Set();  

       [2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));  

       for (let i of s) {  
         console.log(i);  
       }  
       // 2 3 5 4  
       // 例一  
       var set = new Set([1, 2, 3, 4, 4]);  
       [...set]  
       // [1, 2, 3, 4]  

       // 例二  
       var items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);  
       items.size // 5  

       // 例三  
       function divs () {  
         return [...document.querySelectorAll('div')];  
       }  

       var set = new Set(divs());  
       set.size // 56  

       // 类似于  
       divs().forEach(div => set.add(div));  
       set.size // 56

{% endcodeblock %}


>ES6提供了Map数据结构。
它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。
也就是说，Object结构提供了“字符串—值”的对应，Map结构提供了“值—值”的对应，是一种更完善的Hash结构实现。如果你需要“键值对”的数据结构

{% codeblock lang:js %}

 * var m = new Map();  
       var o = {p: 'Hello World'};  

       m.set(o, 'content')  
       m.get(o) // "content"  

       m.has(o) // true  
       m.delete(o) // true  
       m.has(o) // false  

       var map = new Map([  
         ['name', '张三'],  
         ['title', 'Author']  
       ]);  

       map.size // 2  
       map.has('name') // true  
       map.get('name') // "张三"  
       map.has('title') // true  
       map.get('title') // "Author"

{% endcodeblock %}


