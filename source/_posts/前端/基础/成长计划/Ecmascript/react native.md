---
title: react native
toc: true
date: 2018-02-23  21:20:40
tags: [前端, react native, RN]
---

# 简介

React Native使你能够在Javascript和React的基础上获得完全一致的开发体验，构建世界一流的原生APP。

使用React Native，你可以使用标准的平台组件，例如iOS的UITabBar或安卓的Drawer。 这使你的app获得平台一致的视觉效果和体验，并且获得最佳的性能和流畅性。

# RN的运行原理

![Object-C 与 RN](http://haoqiao.qiniudn.com/6039087-13814ffff21ce4eb.png)

`JavaScript 是一种单线程的语言，它不具备自运行的能力，因此总是被动调用，Objective-C 创建了一个单独的线程，这个线程只用于执行 JavaScript 代码，而且 JavaScript 代码只会在这个线程中执行。`

>在 React Native 中，Objective-C 和 JavaScript 的交互都是通过传递 ModuleId、MethodId 和 Arguments 进行的。Objective-C 和 JavaScript 两端都保存了一份配置表，里面标记了所有 Objective-C 暴露给 JavaScript 的模块和方法。这样，无论是哪一方调用另一方的方法，实际上传递的数据只有 ModuleId、MethodId 和 Arguments 这三个元素，它们分别表示类、方法和方法参数，当 Objective-C 接收到这三个值后，就可以通过 runtime 唯一确定要调用的是哪个函数，然后调用这个函数。Objective-C 和 JavaScript 的交互总是由Objective-C发起的。Object-C与js的交互是通过各端的Bridge和ModuleConfig来进行的，实际过程可分为两个阶段：初始化阶段和方法调用阶段。


一般情况下，Objective-C 会定时、主动的调用JS放到MessageQueue 中的方法，实际上（由于卡顿或某些特殊原因），JavaScript 也可以主动调用 Objective-C 的方法，目前，React Native 的逻辑是，如果消息队列中有等待 Objective-C 处理的逻辑，而且 Objective-C 超过 5ms 都没有来取走，那么 JavaScript 就会主动调用 Objective-C 的方法。



