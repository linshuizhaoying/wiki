---
title: cookie
toc: true
date: 2017-07-10  19:02:31
tags: [前端,Html,Cookie]
---

## 概述

### 时效期

cookie默认的有效期很短，只能持续在web浏览器会话期间，一旦用户关闭浏览器，保存的数据就丢失了。因此要明确告诉浏览器cookie的有效期是多长，一旦设置了有效期，浏览器就会将cookie存储在一个文件中，直到过了指定有效期才会删除。 

### 浏览器在发送cookie时会发送哪几个部分[面试考点]

> expires 

expires 选项用来设置 Cookie 什么时间内有效，expires 其实是 Cookie 失效日期。
expires 必须是 GMT 格式的时间

> max-age

expires 是 http/1.0 协议中的选项，在新的 http/1.1 协议中 expires 已经由 max-age 选项代替，两者的作用都是限制 Cookie 的有效时间。expires 的值是一个时间点 (Cookie 失效时刻 = expires)，而 max-age 的值是一个以秒为单位时间段  (Cookie 失效时刻 = 创建时刻 + max-age)

>domain 和 path

name、domain 和 path 可以标识一个唯一的 Cookie。domain 和 path 两个选项共同决定了 Cookie 何时被浏览器自动添加到请求头部中发送出去。

如果没有设置这两个选项，则会使用默认值。domain 的默认值为设置该 Cookie 的网页所在的域名，path 默认值为设置该 Cookie 的网页所在的目录。

> Cookie 的作用域和作用路径

只要满足作用域和作用路径，请求就会带上 Cookie，就算端口号不一样。

在子路径内可以访问访问到父路径的 Cookie，反过来就不行。

> secure
secure选项用来设置cookie只在确保安全的请求中才会发送。当请求是HTTPS或者其他安全协议时，包含secure选项的cookie才能被发送至服务器。

### response头的set-cookie字段
![imgn](http://haoqiao.qiniudn.com/E290D12E-EFDD-4B7E-AC55-7D31F49CD4FD.png)

* 一个 Set-Cookie 字段只能设置一个 Cookie，当你要想设置多个 Cookie，需要添加同样多的 Set-Cookie 字段
* 服务端可以设置 Cookie 的所有选项：expires、domain、path、secure、HttpOnly

#### 作用

通过使用Set－Cookie头，一个HTTP服务器可以传递name/value键值对以及相对应的元数据（所谓的cookies）到user agent。当user agent向服务器发送后续请求时，user agent会根据元数据和其他信息来决定是否要在Cookie头中返回name/value键值对。

为了存储状态，源服务器在HTTP响应中包含了一个Set-Cookie头。在后续的请求中，user agent将回传一个Cookie请求头到源服务器。Cookie头包含了user agent在前面Set-Cookie头中包含的cookie。源服务器可以选择忽略Cookie头或将Cookie用于应用所定义的目的。

为了`移除一个cookie`，服务器要返回一个把过期时间设置在过去的Set-Cookie字段。服务器只有在Set-Cookie头中Path和Domain属性与创建cookie时相符时，才能成功删除cookie。


##  cookie操作

{% codeblock Javascript Array Syntax lang:js %}

var CookieUtil = {
    get: function(name) {
        var cookieName = encodeURIComponent(name) + '=',
            cookieStart = document.cookie.indexOf(cookieName),
            cookieValue = null;
        if (cookieStart》 - 1) {
            var cookieEnd = document.cookie.indexOf(';', cookieStart);
            if (cookieEnd == -1) {
                cookieEnd = document.cookie.length;
            }
            cookieValue = decodeURIComponent(document.cookie.substring(cookieStart + cookieName.length, cookieEnd));
        }
        return cookieValue;
    },
    set: function(name, value, expires, path, domain, secure) {
        var cookieText = encodeURIComponent(name) + '=' +
            encodeURIComponent(value);
        if (expires instanceof Date) {
            cookieText += '; expires=' + expires.toGMTString();
        }
        if (path) {
            cookieText += '; path=' + path;
        }
        if (domain) {
            cookieText += '; domain=' + domain;
        }
        if (secure) { //secure在这里是布尔值
            cookieText += '; secure';
        }
        document.cookie = cookieText;
    },
    unset: function(name, path, domain, secure) {
        this.set(name, '', new Date(0), path, domain, secure);
    }

};

CookieUtil.set("name", "Nicholas");
CookieUtil.set("book", "Professional JavaScript");
//读取 cookie 的值
console.log(CookieUtil.get("name")); //"Nicholas"
console.log(CookieUtil.get("book")); //"Professional JavaScript"

{% endcodeblock %}

>cookie的名/值 键中是不允许包含分号，逗号，空白符，因此用encodeURLComponent编码

## 缺点

>Cookie会被附加在每个HTTP请求中，所以无形中增加了流量
>由于在HTTP请求中的Cookie是明文传递的，所以安全性成问题
>Cookie的大小限制在4KB左右。对于复杂的存储需求来说是不够用的

