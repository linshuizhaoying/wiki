---
title: HTML5
toc: true
date: 2017-07-10  19:02:31
tags: [前端,Html,HTML5]
---

## HTML5新特性

* 1.绘画 canvas; 用于媒介回放的 video 和 audio 元素; 

* 2.本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失; 

* 3. 语意化更好的内容元素，比如 article、footer、header、nav、section; 

* 4. 表单控件，calendar、date、time、email、url、search; 

* 5. 新的技术webworker, websocket, Geolocation;


## 详细划分

### 离线

Application cache
Local storage
Indexed DB
在线/离线事件
离线应用
manifest文件
  
>前置知识
 
```

web应用程序的本地缓存是通过每个页面的manifest文件来管理的，它是一个文本文件，以清单的形式列举了需要缓存或不需要被缓存的资源文件的文件名称

真正运行或测试离线应用时，需要让服务器支持text/cache-manifest

```

### 存储
Application cache
Local storage
Indexed DB

### 连接
Web sockets

Server-sent事件

### 文件访问
File Api
File System
FileWriter
ProgressEvents

### 语义
Media
structural

### 国际化
Link relation

### 属性
form类型
microdata

### 音频和视频
Html5 Video
Web Audio
WebRTC
Video track
3D和图形
Canvas 2D
3D css转换
WEBGL
SVG

### 展示
Css3 2D/3D 变换
转换transition
WebFonts

### 性能
Web Worker
HTTP caching
         
### 其它
触控和鼠标
Shadow DOM
CSS masking


### 兼容

IE8/IE7/IE6支持通document.createElement方法产生的标签， 可以利用这一特性让这些浏览器支持HTML5新标签， 浏览器支持新标签后，还需要添加标签默认的样式。

使用polyfill



