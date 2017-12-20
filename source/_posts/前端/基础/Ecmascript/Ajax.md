---
title: Ajax
toc: true
date: 2017-07-12  10:58:50
tags: [前端,Javascript,Ajax]
---

## 概述

`ajax的全称： Asynchronous Javascript And XML。 `

` 异步传输 + js + xml`

ajax应用主要特点是使用脚本控制http和web服务器进行数据交换， 不会导致页面重载

### 特点

1. 改善的用户体验, 无刷新更新数据。 - AJAX提供的更丰富的用户体验是其主要优点 

2. 减少带宽的使用并增加速度 - AJAX使用客户端脚本来和web服务器通讯， 用JavaScript来交互数据。 使用AJAX能减少网路负载和带宽使用并且只获得你所需的数据。 这样能给你更快的接口和更低的响应时间

3. 支持异步处理


### 缺点

1. 安全问题, 跨站点脚本攻击、 SQL注入攻击 

2. 违背URL和资源定位的初衷 

3. 对搜索引擎支持较弱


## 原理

XMLHttpRequest基本用法

XMLHttpRequest readystate四个值： 

0: uninitialized， 还没调用open() 

1: open， 调用了open()， 但还没调用send()
 
2: sent， 调用了send()， 但请求还没返回 

3: receiving， 收到了部分响应数据 

4: complete， 收到了所有的响应数据， 并且可用了 xhr.open(method, url, isAsyn);

{% codeblock lang:js %}

function createXHR() {
    return new XMLHttpRequest();
}
var xhr = createXHR();
xhr.onreadystatechange = function() {
    if (xhr.readyState == 4) {
        var status = xhr.status;
        if ((status》 = 200 && status《 300) || status == 304) { alert(xhr.responseText); } else { alert('Request was unsuccessful: ' + xhr.status); }
    }
};
xhr.open('get', 'example.txt', true);
xhr.send(null);

{% endcodeblock %}


* 原生ajax的请求总结为一下六个步骤

1.创建XHR对象

2.调用open()方法创建请求

3.调用send()方法发送请求

4.onreadychange捕获请求的状态码

5.判断状态码是否成功

6.调用ajax的responseText属性返回数据

## json与jsonp的区别

>jsonp全名叫做json with padding， 很形象， 就是把json对象用符合js语法的形式包裹起来以使其它网站可以请求得到， 也就是将json数据封装成js文件

>json是理想的数据交换格式， 但没办法跨域直接获取， 于是就将json包裹(padding) 在一个合法的js语句中作为js文件传过去。 这就是json和jsonp的区别， json是想要的东西， jsonp是达到这个目的而普遍采用的一种方法， 当然最终获得和处理的还是json。 所以说json是目的， jsonp只是手段。 json总会用到， 而jsonp只有在跨域获取数据才会用到。

>理解了json和jsonp的区别之后， 其实ajax里的跨域获取数据就很好理解和实现了， 同源时候并没有什么特别的， 直接取就行， 跨域时候需要拐个弯来达到目的


```

脚本创建一个 <script> 元素， 地址指向第三方的API网址， 
形如： 
< script src = "http://www.example.net/api?param1=1&param2=2" > < /script> 

并提供一个回调函数来接收数据（函数名可约定，或通过地址参数传递）。 
第三方产生的响应为json数据的包装（故称之为jsonp，即json padding），形如： callback({"name":"hax","gender":"Male"}) 
这样浏览器会调用callback函数，并传递解析后json对象作为参数。
本站脚本可在callback函数里处理所传入的数据。

```

{% codeblock lang:js %}

  < script type = "text/javascript" >
    /* js 解析返回的格式为 json */
    function telljson() {
        // 创建 xmlhttpRequest 对象
        var xmlhttpRequest = new XMLHttpRequest();
        //请求URL
        var url = "http://localhost:18080/servlet/Servlet3?aa=10";
        // 将请求过程绑定到 open 方法
        xmlhttpRequest.open("POST", url, true);
        // 发送请求
        xmlhttpRequest.send(url);
        // readstate 就是一个xmlhttprequest 对象的一个属性用来记录服务端响应的状态
        var readstate = xmlhttpRequest.readyState;
        alert("请求准备状态：" + readstate);
        // status 服务器执行的状态
        var status = xmlhttpRequest.status;
        alert("请求发送结果" + status);
        // responseText 对象为xmlhttpRequest 对象的一个属性，用来以字符串的方式存储服务器端返回的值。
        var text = xmlhttpRequest.responseText;
        alert("json text: " + text);
        // 获取json 返回值
        // 那边传的是json对象的格式的一个字符串，在前台首先将字符串转化为一个json格式的js对象
        var json = eval("(" + text + ")");
        // 通过eval() 方法将json格式的字符串转化为js对象，并进行解析获取内容
        alert("age:" + json.age + "age1:" + json.age1);
    };
   < /script>

{% endcodeblock %}


* JSONP 的缺点以及安全隐患

>它只支持GET请求而不支持POST等其它类型的HTTP请求；它只支持跨域HTTP请求这种情况，不能解决不同域的两个页面之间如何进行JavaScript调用的问题。

>JSONP是一种脚本注入(Script Injection)行为，所以有一定的安全隐患。


## Ajax的兼容性问题

1. document.getElementById替代document.all（ie适用）

2. 集合[]替代()（ie适用）

3. target替代srcElement;parentNode替代parentElement（parentNode ie适用）

4,node.parentNode.removeChild(node)替代
removeNode(this)（ie适用）

5. DOMMouseScroll替代onmousewheel;-e.detail替代event.wheelDelta

6. addEventListener替代attachEvent;removeEventListener替代detachEvent

7. e.preventDefault()替代event.returnValue=false;e.stopPropagation()替代event.cancelBubble=true

8. style.top、style.left等严格检查"px"单位（加"px" ie适用）

9. style="-moz-opacity:0.9"替代style="filter:alpha(opacity=90)";无其它filter

10. style.cursor="pointer"替代style.cursor="hand"（ie适用）

11. title替代alt（ie适用）

12. 所有的空间在引用时都要这样引用：document.getElementById（“XX”）





