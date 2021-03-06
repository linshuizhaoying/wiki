---
title: Webpack
toc: true
date: 2017-07-12  20:32:59
tags: [前端,Javascript,Webpack]
---

# webpack 简介

> webpack 是一个现代 JavaScript 应用程序的模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图( dependency graph )，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

# webpack 原理

![webpack整体流程图](http://haoqiao.qiniudn.com/webpack%E6%95%B4%E4%BD%93%E6%B5%81%E7%A8%8B%E5%9B%BE.jpg)

## webpack 核心概念

* entry 一个可执行模块或库的入口文件。
* chunk 多个文件组成的一个代码块，例如把一个可执行模块和它所有依赖的模块组合和一个 chunk 这体现了 webpack的打包机制。
* loader 文件转换器，例如把 es6 转换为 es5，scss 转换为 css。
* plugin 插件，用于扩展 webpack 的功能，在 webpack 构建生命周期的节点上加入扩展 hook 为 webpack 加入功能。


## webpack构建流程

从启动 webpack 构建到输出结果经历了一系列过程，它们是：

1. 解析 webpack 配置参数，合并从 shell 传入和 webpack.config.js 文件里配置的参数，生产最后的配置结果。

2. 注册所有配置的插件，好让插件监听 webpack 构建生命周期的事件节点，以做出对应的反应。

3. 从配置的 entry 入口文件开始解析文件构建 AST 语法树，找出每个文件所依赖的文件，递归下去。

4. 在解析文件递归的过程中根据文件类型和 loader 配置找出合适的 loader 用来对文件进行转换。

5. 递归完后得到每个文件的最终结果，根据 entry 配置生成代码块 chunk。

6. 输出所有 chunk 到文件系统。

**需要注意的是，在构建生命周期中有一系列插件在合适的时机做了合适的事情，比如 UglifyJsPlugin 会在 loader 转换递归完后对结果再使用 UglifyJs 压缩覆盖之前的结果。**


# webpack 热更新原理

![webpack热更新总结](http://haoqiao.qiniudn.com/webpack%E7%83%AD%E6%9B%B4%E6%96%B0%E6%80%BB%E7%BB%93.jpg)

webpack 在最初的版本是没有热更新的，后来是因为有了这个需求，社区开发了热更新的模块 `webpack-hot-middleware`。

> webpack-hot-middleware/client 发现通信是用  window.EventSource 实现

> EventSource 是 HTML5 中 Server-sent Events 规范的一种技术实现。EventSource 接口用于接收服务器发送的事件。它通过HTTP连接到一个服务器，以text/event-stream 格式接收事件, 不关闭连接。通过 EventSource 服务端可以主动给客户端发现消息，使用的是 HTTP协议，单项通信，只能服务器向浏览器发送； 与 WebSocket 相比轻量，使用简单，支持断线重连。

整个流程归纳如下:


1. Webpack编译期，为需要热更新的 entry 注入热更新代码(EventSource通信)

2. 页面首次打开后，服务端与客户端通过 EventSource 建立通信渠道，把下一次的 hash 返回前端

3. 客户端获取到hash，这个hash将作为下一次请求服务端 hot-update.js 和 hot-update.json的hash

4. 修改页面代码后，Webpack 监听到文件修改后，开始编译，编译完成后，发送 build 消息给客户端

5. 客户端获取到hash，成功后客户端构造hot-update.js script链接，然后插入主文档

6. hot-update.js 插入成功后，执行 hotAPI 的 createRecord 和 reload 方法，获取到 Vue 组件的 render 方法，重新 render 组件， 继而实现 UI 无刷新更新。



# webpack 本地开发怎么解决跨域的

总所周知，webpack 中修改配置开启代理就能进行本地跨域调试。

webpack-dev-server 的配置

```

在webpack的配置文件（ webpack.config.js ）中进行配置


module.exports = {
 ...

 // webpack-dev-server 的配置
 devServer: {
 historyApiFallback: true,
 hot: true,
 inline: true,
 progress: true,
 port: 3000,
 host: '10.0.0.9',
 proxy: {
 '/test/*': {
 target: 'http://localhost',
 changeOrigin: true,
 secure: false
 }
 }
 },

 ...
};

```

`webpack-dev-server 使用 http-proxy-middleware 去把请求代理到一个外部的服务器`

## Webpack 打包出来的文件如何拆包

>webpack 在打包过程中会针对不同的资源类型使用不同的loader处理，然后将所有静态资源整合到一个bundle里，以实现所有静态资源的加载。webpack最初的主要目的是在浏览器端复用符合CommonJS规范的代码模块，而CommonJS模块每次修改都需要重新构建(rebuild)后才能在浏览器端使用。

通过 `CommonsChunkPlugion` 将各个模块区分打包。


### 如何定位webpack打包速度慢的原因

我们首先需要定位webpack打包速度慢的原因，才能因地制宜采取合适的方案。我们可以在终端中输入：

` webpack --profile --json > stats.json `

然后将输出的json文件到如下两个网站进行分析

https://github.com/webpack/analyse

http://alexkuz.github.io/webpack-chart/

# Webpack 优化 

1）：拆包，限制构建范围，减少资源搜索时间，无关资源不要参与构建

2）：使用增量构建而不是全量构建

3）：从webpack存在的不足出发，优化不足，提升效率

## webpack打包优化

### 1.减小打包文件体积

webpack+react的项目打包出来的文件经常动则几百kb甚至上兆，究其原因有：

import css文件的时候，会直接作为模块一并打包到js文件中
所有js模块 + 依赖都会打包到一个文件
React、ReactDOM文件过大
针对第一种情况，我们可以使用 extract-text-webpack-plugin，但缺点是会产生更长时间的编译，也没有HMR，还会增加额外的HTTP请求。对于css文件不是很大的情况最好还是不要使用该插件。

针对第二种情况，我们可以通过提取公共代码块，这也是比较普遍的做法：

 `new webpack.optimize.CommonsChunkPlugin('common.js');`
 
通过这种方法，我们可以有效减少不同入口文件之间重叠的代码，对于非单页应用来说非常重要。

针对第三种情况，我们可以把React、ReactDOM缓存起来：

```
 entry: {
        vendor: ['react', 'react-dom']
},

new webpack.optimize.CommonsChunkPlugin('vendor','common.js'),
    
```

我们在开发环境使用react的开发版本，这里包含很多注释，警告等等。部署上线的时候可以通过 webpack.DefinePlugin来切换生产版本。

当然，我们还可以将React 直接放到CDN上，以此来减少体积。


### 2.代码压缩


### 3.`happypack`

happypack 的原理是让loader可以多进程去处理文件

### 4.缓存与增量构建 

# Webpack 常用的插件

## 热模块替换Hot Module Replacement

热模块替换可以让模块在没有页面刷新的情况下实时更新代码改动结果；

## CommonsChunkPlugin

CommonsChunkPlugin是一个可以用来提取公共模块的插件.

## ExtractTextWebpackPlugin

分离css单独打包



 
# 资料

> (细说 webpack 之流程篇)[http://taobaofed.org/blog/2016/09/09/webpack-flow/]

> (Webpack 热更新实现原理分析)[https://zhuanlan.zhihu.com/p/30623057]

> (webpack proxy)[http://webpack.github.io/docs/webpack-dev-server.html#proxy]

> (webpack-dev-server)[https://github.com/liangklfangl/webpack-dev-server]


## 概述


`webpack提供的是一个前端模块化的方案`

>requirejs是一种在线"编译" 模块的方案，相当于在页面上加载一个AMD 解释器，以便于浏览器能够识别 define、exports、module，而这些东西就是用于模块化的关键。 而browserify / webpack，则是一个预编译模块的方案。它是预编译的，，不需要在浏览器中加载解释器。另外，你在本地直接写JS，不管是 AMD / CMD / ES6 风格的模块化，它都能认识，并且编译成浏览器认识的JS

## 原理

>所有资源统一入口
webpack是通过js来获取和操作其他文件资源的，比如webpack想处理less, 但是它并不会直接从本地的文件夹中直接通过路径去读取css文件，而且通过执行入口js文件，如果入口 文件中，或者入口文件相关联的js文件中含有 require(xx.less) 这个less文件，那么它就会通过 对应的loader去处理这个less文件


总结:

* 每个文件都是一个资源，可以用require导入js
* 每个入口文件会把自己所依赖(即require)的资源全部打包在一起，一个资源多次引用的话，只会打包一份
* 对于多个入口的情况，其实就是分别独立的执行单个入口情况，每个入口文件不相干(可用CommonsChunkPlugin优化)

>在入口文件中，对每个require资源文件进行配置一个id, 也 就是说，对于同一个资源,就算是require多次的话，它的id也是一样的，所以无论在多少个文件中 require，它都只会打包一分

## tree shaking

即从模块包中排除未使用的 exports 项

tree-shaking 消灭没有用到的代码

不管是 rollup 还是 webpack 2，tree-shaking 都是因为 ES6 modules 的静态特性才得以实现的

1. 只能作为模块顶层的语句出现，不能出现在 function 里面或是 if 里面。（ECMA-262 15.2)
2. import 的模块名只能是字符串常量。(ECMA-262 15.2.2)
3. 不管 import 的语句出现的位置在哪里，在模块初始化的时候所有的 import 都必须已经导入完成。换句话说，ES6 imports are hoisted。(ECMA-262 15.2.1.16.4 - 8.a)
4. import binding 是 immutable 的，类似 const。比如说你不能 import { a } from './a' 然后给 a 赋值个其他什么东西。(ECMA-262 15.2.1.16.4 - 12.c.3)

这些设计虽然使得灵活性不如 CommonJS 的 require，但却保证了 ES6 modules 的依赖关系是确定 (deterministic) 的，和运行时的状态无关，从而也就保证了 ES6 modules 是可以进行可靠的静态分析的

正是基于这个基础上，才使得 tree-shaking 成为可能（这也是为什么 rollup 和 webpack 2 都要用 ES6 module syntax 才能 tree-shaking）

## Webpack 怎么提取公共模块

common chunk plugins

顾名思义，Common Chunks 插件的作用就是提取代码中的公共模块，然后将公共模块打包到一个独立的文件中去，以便在其它的入口和模块中使用。

