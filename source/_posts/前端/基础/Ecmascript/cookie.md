---
title: cookie
toc: true
date: 2018-02-25  08:48:04
tags: [前端, cookie, cookie 操作]
---

# 如何实现cookie跨域访问

> 一般情况下
> Cookie是不可跨域的。域名http://www.google.com颁发的Cookie不会被提交到域名http://www.baidu.com/去。这是由Cookie的隐私安全机制决定的。隐私安全机制能够禁止网站非法获取其他网站的Cookie。
>正常情况下，同一个一级域名下的两个二级域名如http://www.helloweenvsfei.com和images.helloweenvsfei.com也不能交互使用Cookie，因为二者的域名并不严格相同。如果想所有helloweenvsfei.com名下的二级域名都可以使用该Cookie，需要设置Cookie的domain参数

因此产生了一些黑科技的做法。

其实说到底这些做法是将 cookie 视为一个字符串数据。然后依靠常规的跨域做法将这个字符串暴露给别的域名。

## JS跨域的 JSONP 解决方案

## iframe跨域

在Ａ系统下成功登录后，利用JS动态创建一个隐藏的iframe，通过iframe的src属性将Ａ域下的cookie值作为

```

var   _frm   =   document.createElement("iframe");  
_frm.style.display="none";  
 _frm.src="http://b.com/b.jsp?test_cookie=xxxxx";  
 document.body.appendChild(_frm);   

```

在Ｂ系统的b.jsp页面中来获取Ａ系统中所传过来的cookie值，并将所获取到值写入cookie中，这样就简单的实现了cookie跨域的访问。

## Nginx设置反向代理也可实现


