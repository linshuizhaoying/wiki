---
title: 深入React技术栈
toc: true
date: 2017-09-17  13:36:18
tags: [前端,阅读,读书笔记,读后感]
---

## 事件系统

React基于Virtual DOM实现了一个SyntheticEvent(事件层) 我们所定义的事件处理器会接收到一个SyntheticEvent对象的实例。

> 事件处理程序通过 合成事件（SyntheticEvent）的实例传递，SyntheticEvent 是浏览器原生事件跨浏览器的封装。SyntheticEvent 和浏览器原生事件一样有 stopPropagation()、preventDefault() 接口，而且这些接口所有浏览器兼容。

所有事件绑定到最外层。

访问原生事件对象通过 `nativeEvent`

必须用驼峰形式写事件的属性名 比如 `onClick={...}`



### 合成事件的实现机制

**`事件委派`**

事件代理机制:

并不会直接绑定到真实节点。所有事件绑定到结构最外层。

使用统一的`事件监听器`。

`事件监听器`维护了一个映射来保存所有的组件内部的事件监听和处理函数。

组件挂载或卸载时，只是在`事件监听器`上插入和删除一些对象。

当事件发生时，统一的事件监听器去处理。在映射中找到对应处理。简化了事件处理和回收机制，效率提升。


**`自动绑定`**

在React组件，每个方法的上下文自动绑定该组件实例。自动绑定this到当前组件。

当使用ES6 classes或者纯函数。这种自动绑定消失，我们需要自己去绑定。

`bind方法` 可以传递参数


```
onClick={this.handleClick.bind(this,'arg')}

```

`双冒号语法` 不传递参数


```
onClick={::this.handleClick}

```

`构造器内声明`


```

handleClick(e){
  this.handleClick=this.handleClick.bind(this)
}


onClick={this.handleClick}

```


`箭头函数`


```
 
 const handleClick = (e) {
   ...
 }
 
 onClick={this.handleClick}

```




## CSS Modules

用 compose 来组合样式

```

.base{}

.normal {
  compose:base;
}


import styles from './Button.css'
<button class={styles.normal}> click </button>

=>

<button class="button--base--abc123 button--normal--sss"> click </button>

```

# 解读React源码


# React应用架构

## React MiddleWare

Redux 借鉴了Koa的MiddleWare思想。


## Redux 中的组件


|   | 展示型组件 | 容器型组件 |
| --- | --- | --- |
| 目的 | 长什么样子(标签，样式) | 干什么用(获取数据，更新状态) |
| 是否感知redux | 否 | 是 |
| 获取数据 | 从this.props | 从redux状态树中获取 |
| 要改变数据 | 调用从props中传入的action creator | 直接分发action |
| 实际创建于 | 开发者自身 | 由React Redux创建 |
|  |  |  |



