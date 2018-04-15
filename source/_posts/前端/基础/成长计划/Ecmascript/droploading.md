---
title: 下拉加载优化
toc: true
date: 2017-10-30  10:56:50
tags: [前端, electron, JS 基础]
---

## 下拉加载优化

### 知识点

现在有一个下拉加载更多的列表，当加载的次数非常多时，页面的 DOM 结构会变得非常复杂，会不会带来性能问题？怎么优化？

### 背景资料

> 下拉刷新的实现逻辑上是很直接的，通过节流或者防抖动的方式去监听滑动事件，判断边界，触发加载事件。

高级一点 UI 交互的还会先加载默认的占位内容，等真实数据到达替换。

普通的[加载例子](http://www.caijinfeng.com/temp/pull/examples/index.html)


无限滚动的思路：

`隐藏不需要显示的 DOM 元素`

`通过懒加载（设置一个合理阈值，在用户滚动到页面底部之前，先进行提前加载）`

`计算显示屏大小，移除过时的，添加新元素`

`DOM 回收（对于需要产生的大量 DOM 节点（比如我们下拉加载的信息内容）不是主动用 createElement 的方式创建，而是回收利用那些已经移出视窗，暂时不会被需要的 DOM 节点）`

### 实战

先看 **DOM 回收** 的例子[infinite-list](https://github.com/roeierez/infinite-list)

![imgn](http://haoqiao.qiniudn.com/infinite-list.gif)

这个是利用 **DOM 缓存** 来实现的无限加载。

写法简单的例子还是 **减少页面元素，保存页面元素数量一定。**

直接来看这个 demo

![imgn](http://haoqiao.qiniudn.com/infinite-vue-list.gif)

原理和[代码](https://github.com/hejianxian/vue-list)直接可以阅读参考文献里的内容。






### 参考

> [设计无限滚动下拉加载，实践高性能页面真谛](https://segmentfault.com/a/1190000008518315)

> [infinite-scroller](https://developers.google.com/web/updates/2016/07/infinite-scroller)

> [Vue.js 一个超长列表无限滚动加载的解决方案](https://juejin.im/entry/5819993fbf22ec0068aab054)

