---
title: Node
toc: true
date: 2017-07-11  20:56:16
tags: [前端,Javascript,Node]
---

# 前置知识

## 基础

>Node是采用单线程，异步式I/O，事件驱动式的让javascript运行在服务器端的开发平台

>Node.js在执行中会维护一个事件队列，每个异步式I/O请求完成后会被推送到事件队列，等待程序进程处理。
>利用事件循环的处理能力:Node.js适用于应用程序处理大量并发的I/O。


## 核心模块

### EventEmitter
>EventEmitter是node中一个实现观察者模式的类，主要功能是监听和发射消息，用于处理多模块交互问题

>所有能触发事件的对象都是EventEmitter类的实例。 这些对象开放了一个eventEmitter.on()函数，允许将一个或多个函数附加到会被对象触发的命名事件上

{% codeblock lang:js %}

实现一个eventEmitter
           var util = require('util');  
           var EventEmitter = require('events').EventEmitter;  

           function MyEmitter() {  
               EventEmitter.call(this);  
           } // 构造函数  

           util.inherits(MyEmitter, EventEmitter); // 继承  

           var em = new MyEmitter();  
           em.on('hello', function(data) {  
               console.log('收到事件hello的数据:', data);  
           }); // 接收事件，并打印到控制台  
           em.emit('hello', 'EventEmitter传递消息真方便!');

{% endcodeblock %}



### Buffer

>Buffer对象是Node处理二进制数据的一个接口。它是Node原生提供的全局对象

>JavaScript比较擅长处理字符串，对于处理二进制数据（比如TCP数据流），就不太擅长。Buffer对象就是为了解决这个问题而设计的。

### Stream

>stream是基于事件EventEmitter的数据管理模式．由各种不同的抽象接口组成，主要包括可写，可读，可读写，可转换等几种类型

* Stream是Node中一个非常重要的概念，被大量对象实现，尤其是Node中的I/O操作

* Stream是一个抽像的接口，一般不会直接使用，需要实现内部的某些抽象方法(例如_read、_write、_transform)

* Stream是EventEmitter的子类，实际上Stream的数据传递内部依然是通过事件(data)来实现的

* Stream分为四种：readable、writeable、Duplex、transform


### http模块

{% codeblock lang:js %}

* 实现一个简单的http服务器
     * 代码
          * var http = require('http'); // 加载http模块  

                http.createServer(function(req, res) {  
                    res.writeHead(200, {'Content-Type': 'text/html'}); // 200代表状态成功, 文档类型是给浏览器识别用的  
                    res.write('<meta charset="UTF-8"> <h1>我是标题啊！</h1> <font color="red">这么原生，初级的服务器，下辈子能用着吗?!</font>'); // 返回给客户端的html数据  
                    res.end(); // 结束输出流  
                }).listen(3000); // 绑定3ooo, 查看效果请访问 http://localhost:3000
     * 加载http模块，创建服务器，监听端口

{% endcodeblock %}



### cluster 模块

>CPU都是多核的背景下.
它可以通过一个父进程管理子进程的方式来实现集群的功能。
>每个worker进程通过使用child_process.fork()函数，基于IPC（Inter-Process Communication，进程间通信），实现与master进程间通信。

>当worker使用server.listen（...）函数时 ，将参数序列传递给master进程。如果master进程已经匹配workers，会将传递句柄给工人。如果master没有匹配好worker，那么会创建一个worker，再传递并句柄传递给worker。

## Nodejs中的Stream和Buffer有什么区别

* buffer

>为数据缓冲对象，是一个类似数组结构的对象，可以通过指定开始写入的位置及写入的数据长度，往其中写入二进制数据
 
* stream

>是对buffer对象的高级封装，其操作的底层还是buffer对象，stream可以设置为可读、可写，或者即可读也可写，在nodejs中继承了EventEmitter接口，可以监听读入、写入的过程

## node有哪些定时功能

* process.nextTick
* setTimeout/clearTimeout
* setInterval/clearInterval
* setImmediate/clearImmediate,

>优先级
>在每一轮循环检查中，idle观察者先于I/O观察者，I/O观察者先于check观察者。
  
* process.nextTick
  每次调用，只会将回调函数放入队列，在下一轮tick时取出执行。时间复杂度o(1) 属于IDLE观察者

* setTimeout
 setTimeout需要使用红黑树，且after设置为0，其实会被node强制转换为1，存在性能上的问题，建议替换为setImmediate。时间复杂度o(lg(n))

* setImmediate
 将函数延迟执行，属于check观察者。

## node中的异步和同步怎么理解

>同步与异步

>1. 同步和异步关注的是消息通信机制 (synchronous communication/ asynchronous communication)。

>2. 所谓同步，就是在发出调用时，
    1. 在没有得到结果之前，调用不返回。
    2. 一旦调用返回，即得到返回值。
>换句话说，就是由调用者主动等待这个调用的结果。

>3. 异步则是相反，调用在发出之后，调用就直接返回，但没有返回结果。

>换句话说，当一个异步过程调用发出后，调用者不会立刻得到结果。而是在调用发出后，被调用者通过状态、通知来通知调用者，或通过回调函数处理这个调用。

* node是单线程的，异步是通过一次次的循环事件队列来实现的．

* 同步则是说阻塞式的IO,这在高并发环境会是一个很大的性能问题，所以同步一般只在基础框架的启动时使用，用来加载配置文件，初始化程序什么的

## express


### 特点
* 精简
* 可扩展
* 可以配合多页和单页应用

express中如何获取路由的参数

* users/:name使用req.params.name来获取;
* req.body.username则是获得表单传入参数username
* express路由支持常用通配符 ?, +, *, and ()

## Mongodb

>NoSQL（Not Only SQL）泛指非关系型数据库

>MongoDB数据库是面向文档的数据库，该数据库是非阻塞型数据库，内部查询支持javascript。

### 和SQL区别

* SQL数据存在特定结构的表中；而NoSQL则更加灵活和可扩展，存储方式可以省是JSON文档、哈希表或者其他方式
* 在SQL中，必须定义好表和字段结构后才能添加数据，例如定义表的主键(primary key)，索引(index),触发器(trigger),存储过程(stored procedure)等。表结构可以在被定义之后更新，但是如果有比较大的结构变更的话就会变得比较复杂。 在NoSQL中，数据可以在任何时候任何地方添加，不需要先定义表。
* 1. SQL数据库被称为关系型数据库（RDBMS），而NoSQL数据库被称为非关系型数据库或分布式数据库。  

  2. SQL数据库是基于表的数据库，而NoSQL数据库则有基于文档的，键值对的，图形的或基于列式存储的数据库。  

  3. SQL数据库的数据结构必须事先先定义好，而NoSQL数据库的数据是动态无结构的。  

  4. SQL数据库的负载能力是以增加硬件配置的垂直扩展方式来增加的，而NoSQL数据库的负载能力可以通过增加数据库服务器的数量来增加（属于水平扩展）。  

  5.对于复杂的查询：SQL非常的擅长，而NoSQL则不擅长


### mongoose是什么？有支持哪些特性?

>mongoose是mongodb的文档映射模型．主要由Schema, Model和Instance三个方面组成．  

* Schema就是定义数据类型，  
* Model就是把Schema和js类绑定到一起，  
  Instance就是一个对象实例．  
* 常见mongoose操作有,save, update, find. findOne, findById, static方法等


## koa

* 处理客户端请求的函数。内部封装一个NEXT回调函数。

* 中间件（Middleware） 是一个函数，它可以访问请求对象（request object (req)）, 响应对象（response object (res)）, 和 web 应用中处于请求-响应循环流程中的中间件，一般被命名为 next 的变量。  

  如果当前中间件没有终结请求-响应循环，则必须调用 next() 方法将控制权交给下一个中间件，否则请求就会挂起。

完美解决了异步组合问题，并解决了异步异常捕获问题（并且 koa 框架在最外层 catch 了异常，应用不会因为异常而中断进程，所以不需要 forever 来重启应用）



## 阅读node的http模块和Stream模块源码(?)

## libuv的底层实现(?)

