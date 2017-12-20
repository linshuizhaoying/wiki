---
title: Webpack
toc: true
date: 2017-07-12  20:32:59
tags: [前端,Javascript,Webpack]
---

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

