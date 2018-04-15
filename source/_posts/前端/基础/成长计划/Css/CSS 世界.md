---
title: CSS 世界
toc: true
date: 2018-03-19  12:56:10
tags: [前端,Css,基础]
---

## css 流体布局下额宽度分离原则

css 中的 width属性 不与 影响宽度的 padding/border/margin 属性共存。

不能出现

```

.box{ width: 100px; border: 1px solid;}

```

而用 

```

.father{ width:100px; }
.son { margin: ...; padding: ...; }

```

## box-sizing 的初衷

解决替换元素宽度自适应问题。

[demo](http://demo.cssworld.cn/3/2-9.php)

让 `textarea` `input` 的100%自适应父容器宽度。




