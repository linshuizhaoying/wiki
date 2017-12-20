---
title: 
date: 2017-07-08 01:55:57
banner:
tags:
categories:
---
# 关于该Wiki

- 支持多级分类
- 方便修改和更新内容
- 部署简单
- 分类目录可展开和收缩
- 展开分类时可查看该分类下所有文章 / 词条的标题
- 每篇文章 / 词条能添加多个分类 / 标签
- Wiki 可支持引用内部链接
- 使用 Markdown 书写文章 / 词条
- 支持全文搜索（可搜索内容和标题）

# Demo

## 文章开头

开启`toc: true` 文章右上角有个小目录


```

---
title: Title
toc: false
date: 2017-05-06 00:37:50
tags: [前端]
---


```

### 一些MarkDown标记

版本使用

### 使用版本


- **gulp** : v3.9.1
- **gulp-imagemin** : v3.1.1


```

- **gulp** : v3.9.1
- **gulp-imagemin** : v3.1.1

```


### 引用内容

>
> #### Title
>
> Type: `Attr`
>

```

>
> #### Title
>
> Type: `Attr`
>

```




#### 代码

```bash

function test(){
	var a="";
	let b = ""
}

```

```

bash

function test(){
	var a="";
	let b = ""
}

```



```

{% codeblock Javascript Array Syntax lang:js http://j.mp/pPUUmW MDN Documentation %}
var arr1 = new Array(arrayLength);
var arr2 = new Array(element0, element1, ..., elementN);
{% endcodeblock %}

```

{% codeblock Javascript Array Syntax lang:js http://j.mp/pPUUmW MDN Documentation %}
var arr1 = new Array(arrayLength);
var arr2 = new Array(element0, element1, ..., elementN);
{% endcodeblock %}




### 参考资料

> - [haoqiao](http://haoqiao.me)

```

> - [haoqiao](http://haoqiao.me)

```




### 表格

|  排序方法  |     平均时间      |     最坏时间      |     辅助空间      | 稳定性  |
| :----: | :-----------: | :-----------: | :-----------: | :--: |
|  冒泡排序  |   $O(n^2)$    |   $O(n^2)$    |    $O(1)$     |  稳定  |
| 简单选择排序 |   $O(n^2)$    |   $O(n^2)$    |    $O(1)$     |  稳定  |


```

|  排序方法  |     平均时间      |     最坏时间      |     辅助空间      | 稳定性  |
| :----: | :-----------: | :-----------: | :-----------: | :--: |
|  冒泡排序  |   $O(n^2)$    |   $O(n^2)$    |    $O(1)$     |  稳定  |
| 简单选择排序 |   $O(n^2)$    |   $O(n^2)$    |    $O(1)$     |  稳定  |


```


### Html


#### jsfiddle

{% jsfiddle fv61zk81 %}

```

{% jsfiddle fv61zk81 %}

```

### codepen


