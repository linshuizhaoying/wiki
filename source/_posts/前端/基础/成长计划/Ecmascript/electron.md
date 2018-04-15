---
title: electron
toc: true
date: 2017-10-30  10:56:50
tags: [前端, electron, js 基础]
---

## electron

### 知识点

electron

### 背景资料

> 使用 JavaScript，HTML 和 CSS 构建跨平台桌面应用程序。基于 Node.js 和 Chromium 开发的，Atom editor 以及很多其他的 apps 就是使用 electron 编写的

> 集成 Node 来授予网页访问底层系统的权限



### electron 入门

全局安装 electron

`npm install electron -g`
 
`yarn add electron -g`

##### 主进程

主进程，通常是一个命名为 main.js 的文件，该文件是每个 electron 应用的入口。它控制了应用的生命周期（从打开到关闭）。它既能调用原生元素，也能创建新的（多个）渲染进程。另外，Node API 是内置其中的。

![imgn](http://haoqiao.qiniudn.com/1460000007503499.png)

#### 渲染进程

> 由于 electron 使用 Chromium 来展示页面，所以 Chromium 的多进程结构也被充分利用。每个 electron 的页面都在运行着自己的进程，这样的进程我们称之为渲染进程。在一般浏览器中，网页通常会在沙盒环境下运行，并且不允许访问原生资源。然而，electron 用户拥有在网页中调用 io.js 的 APIs 的能力，可以与底层操作系统直接交互。

渲染进程是应用的一个浏览器窗口。与主进程不同，它能存在多个（注：一个 electron 应用只能存在一个主进程）并且相互独立（它也能是隐藏的）。主窗口通常被命名为 index.html。它们就像典型的 HTML 文件，但 electron 赋予了它们完整的 Node API。因此，这也是它与浏览器的区别。

![imgn](http://haoqiao.qiniudn.com/1460000007503500.png)

#### 主进程与渲染进程的区别

主进程使用 BroswerWindow 实例创建网页。每个 BroswerWindow 实例都在自己的渲染进程里运行着一个网页。当一个 BroswerWindow 实例被销毁后，相应的渲染进程也会被终止。主进程管理所有页面和与之对应的渲染进程。每个渲染进程都是相互独立的，并且只关心他们自己的网页。由于在网页里管理原生 GUI 资源是非常危险而且容易造成资源泄露，所以在网页面调用 GUI 相关的 APIs 是不被允许的。如果你想在网页里使用 GUI 操作，其对应的渲染进程必须与主进程进行通讯，请求主进程进行相关的 GUI 操作。

![imgn](http://haoqiao.qiniudn.com/1460000007503501.png)

#### 相互通讯

> 由于主进程和渲染进程各自负责不同的任务，而对于需要协同完成的任务，它们需要相互通讯。IPC 就为此而生，它提供了进程间的通讯。但它只能在主进程与渲染进程之间传递信息，即渲染进程之间不能进行。
