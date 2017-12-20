---
title: JS代码片段
toc: true
date: 2017-07-12  10:58:50
tags: [前端,Javascript,代码片段]
---

## 用原生js实现复选框选择以及全选非全选功能

{% codeblock lang:html %}

<input type="checkbox" id="checkItem">全选/全不选 

 
<h3>爱好</h3> 
<input type="checkbox" name="item">看书 

 
<input type="checkbox" name="item">听音乐 

 
<input type="checkbox" name="item">打球 

 
<input type="checkbox" name="item">散步 

 
<input type="checkbox" name="item">写代码 

 
    var checkItem = document.getElementById("checkItem").onclick = function() {
        var itemsElement = document.getElementsByName("item");
        for (var i = 0; i < itemsElement.length; i++) {
            var itemElement = itemsElement[i];
            if (this.checked) {
                itemElement.checked = "checked";
            } else {
                itemElement.checked = null;
            }
        }
    }


{% endcodeblock %}



## 实现一个querySelectorAll的功能，函数长这样querySelect(el, className)

{% codeblock lang:js %}

function querySelect(el,className){
    var children = el.children;
    var result = [];
    if(el.classList.contains(className)){
        result.push(el);
    }
    for(var i; i<children.length; i++){
        var child = children[i];
        var arr = querySelect(child,className);
        result.push.apply(result, arr);
    }
    return result;
}

{% endcodeblock %}

{% jsfiddle cfofrs4g %}

## 实现一个暴露内部变量，而且外部可以访问修改的函数（get和set，闭包实现）

{% codeblock lang:js %}

var person = function(){    
    //变量作用域为函数内部，外部无法访问    
    var name = "default";       

    return {    
       getName : function(){    
           return name;    
       },    
       setName : function(newName){    
           name = newName;    
       }    
    }    
}();    

print(person.name);//直接访问，结果为undefined    
print(person.getName());    
person.setName("abruzzi");    
print(person.getName());    

得到结果如下：  

undefined  
default  
abruzzi

{% endcodeblock %}

## 对于页面上的任意元素，当鼠标指针位于其上时，在元素旁边显示该元素的class属性（可以使用任意库）

{% codeblock lang:html %}

<!doctype html> 
<html> 
<head> 
    <meta charset="utf-8"> 
    <title></title> 
    <style>
      .tip{
        position: absolute;
        background-color: red;
        color: white;
      }
    </style>
</head>
<body>
    <h1></h1>
    Hello, this is a snippet.
    <p class="p">hover op</p>

    <div class="test">hover test</div>

    <div class="ul">
      <h1>hover ul</h1>
        <li class="li">
               hover li
        </li>
    </div>

    <div class="tip">vvv</div>
</body>
<script src="http://libs.baidu.com/jquery/1.9.1/jquery.min.js">
</script>

<script>
  /*
  对于页面上的任意元素，当鼠标指针位于其上时，在元素旁边显示该元素的class属性（可以使用任意库）

   */
    document.addEventListener("mouseover",function(e){
     console.log(e)
     var tip = document.querySelector('.tip');
     tip.innerHTML  = e.target.className;
     tip.style.left = e.x + "px";
     tip.style.top = e.y + "px";

    },false)
    

</script>
</html>

{% endcodeblock %}


{% jsfiddle ek9cm2nv %}

## 写一个递归。就是每隔5秒调用一个自身，一共100次

{% codeblock lang:js %}

(function () {
  var z = 0
  var j = 0
  var timer = setTimeout(function() { 
    if (j < 100){
    timer = setTimeout(arguments.callee, 5000)
    z++;
    console.log(z)
    }
  }, 5000)
})()  

{% endcodeblock %}


## 原生JS实现addClass,removeClass,toggleClass.

{% codeblock lang:js %}

<style type="text/css">  
    div.testClass{  
        background-color:gray;  
    }  
</style>  
  
<script type="text/javascript">  
function hasClass(obj, cls) {  
    return obj.className.match(new RegExp('(\\s|^)' + cls + '(\\s|$)'));  
}  
  
function addClass(obj, cls) {  
    if (!this.hasClass(obj, cls)) obj.className += " " + cls;  
}  
  
function removeClass(obj, cls) {  
    if (hasClass(obj, cls)) {  
        var reg = new RegExp('(\\s|^)' + cls + '(\\s|$)');  
        obj.className = obj.className.replace(reg, ' ');  
    }  
}  
  
function toggleClass(obj,cls){  
    if(hasClass(obj,cls)){  
        removeClass(obj, cls);  
    }else{  
        addClass(obj, cls);  
    }  
}  
  
function toggleClassTest(){  
    var obj = document. getElementById('test');  
    toggleClass(obj,"testClass");  
}  
</script>  
  
<body>  
    <div id = "test" style = "width:250px;height:100px;">  
        sssssssssssss  
    </div>  
    <input type = "button" value = "toggleClassTest" onclick = "toggleClassTest();" />  
</body>

{% endcodeblock %}

{% jsfiddle 2s3mzjw9 %}


## js轮播实现思路

定义一个轮播组件，给用户使用时需要提供哪些对外API和属性

```

interval 自动轮播间隔
direction 从左到右还是从右到左
pause   如果设置为“悬停”，请暂停鼠标滚轮上的旋转木马循环，并在鼠标滚轮上恢复轮播的循环。
如果设置为null，则在盘旋盘上悬停不会暂停。
keyboard 是否接受键盘事件

而且它可以作为触发器进行事件触发，比如：

.carousel('prev')

Cycles to the previous item.

.carousel('next')

Cycles to the next item.

.carousel('pause')

Stops the carousel from cycling through items.

```

## 日历

{% codeblock lang:html %}

body,html{padding: 0;margin: 0;font-size: 14px;color:#000;}
table {border-collapse: collapse;width: 100%;table-layout: fixed;}
td,th {border: 1px solid #e1e1e1;padding: 0;height: 20px;line-height: 20px;text-align: center;}
.current{color:red;}

<table>
    <thead>
        <tr><th>一</th><th>二</th><th>三</th><th>四</th><th>五</th><th>六</th><th>日</th></tr>
    </thead>
    <tbody>
        <tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
        <tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
        <tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
        <tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
        <tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
        <tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
    </tbody>
</table>

// 获取指定年-月 有多少天
function getDaysInOneMonth(year, month){  
  month = parseInt(month, 10);  
  var d= new Date(year, month, 0);  
  return d.getDate();  
}  
// return Mon Tue Wed Thu Fri Sat Sun
// 获取第一天星期几
function getWeek(year,month){
    
    var day = new Date(year,month-1,1)
    var map = {
      "Mon" : 1,
      "Tue" : 2,
      "Wed" : 3,
      "Thu" : 4,
      "Fri" : 5,
      "Sat" : 6,
      "Sun" : 7,
    }
    return map[day.toString().split(" ")[0]]
}
function calendar(year, month) {
  var tds = document.querySelectorAll('tbody td'), i;
  var start = getWeek(year,month) -1
  var j = 0
  var d = new Date()
    for(i = 0; i < tds.length; ++i) {
        if(i >= start && j<getDaysInOneMonth(year,month)){
           j++
           tds[i].innerHTML = j;
           if(j == d.getDate()){
               tds[i].className = "current"
           }
        }
    }
}
calendar(2017,4);

{% endcodeblock %}

{% jsfiddle 95toou8h %}

## 编写一个元素拖拽的插件

{% codeblock lang:html %}



<body>
    <div id="box">
        <div id="head">我是可拖拽区域</div>
        <div id="body">我是主体部分</div>
    </div>
</body>
<script>
// target 拖拽目标id， moveTarget 移动目标id
// how to use
// drag(targetId, moveTargetId)
// targetId: 触发拖拽操作的区域id
// moveTargetId: 拖拽需要移动的区域id
// ----- 注意 ------
// 会给moveTarget节点添加user-select属性，值为none

var drag = function(target, moveTarget) {
  target = document.getElementById(target);
  target.style.cursor = 'all-scroll';
  moveTarget = document.getElementById(moveTarget);

  //拖拽区域可以选中文字会产生影响，所以设置无法选中文字
  ['-moz-','-webkit-','-ms-','-khtml-',''].map(function(prefix) {
    moveTarget.style[prefix + 'user-select'] = 'none';
  });

  // 设置position:absolute
  moveTarget.style.position = 'absolute';


  var fnDown = function(e) {
    e = e || window.event;
    var disX = e.clientX - moveTarget.offsetLeft,
        disY = e.clientY - moveTarget.offsetTop;

    // 防止如果其他功能也设置了onmousemove和onmouseup,先保存起来，drag结束再重新恢复
    var previousOnmousemove = document.onmousemove,
        previousOnmouseup = document.onmouseup;

    document.onmousemove = function(e) {
      e = e || window.event;
      fnMove(e, disX, disY);
    };

    document.onmouseup = function() {
      console.log('mouseup');
      document.onmousemove = previousOnmousemove;
      document.onmouseup = previousOnmouseup;
    };
    console.log('mousedown');
  };

  var fnMove = function(e, disX, disY) {
    var l = e.clientX - disX,
        t = e.clientY - disY,
        // 这里踩了一个大坑，以为document.documentElement.clientHeight和document.body.clientHeight是一样的
        // 都是获取浏览器视窗的高度。但不一样，
        // document.documentElement.clientHeight获取浏览器窗口高度
        // document.body.clientHeight 获取body的高度，这时如果body高度比较低就会出错了
        // 所以把document.documentElement.clientHeight放到前面
        winW = document.documentElement.clientWidth||document.body.clientWidth,
            winH = document.documentElement.clientHeight||document.body.clientHeight,
        maxW = winW - moveTarget.offsetWidth,
        maxH = winH - moveTarget.offsetHeight;

        // console.log(document.documentElement.clientHeight + ' ' + document.body.clientHeight);

    if (l < 0) {
      l = 0;
    } else if (l > maxW) {
      l = maxW;
    }
    if (t < 0) {
      t = 0;
    } else if (t > maxH) {
      t = maxH;
    }

    moveTarget.style.left = l + 'px';
    moveTarget.style.top = t + 'px';
  };
  target.onmousedown = fnDown;
};
</script>
<script>
drag('head', 'box');
</script>

#box {
        width: 400px;
        height: 300px;
        background-color: lightgray;
        position: absolute;
        top: 100px;
        left: 400px;
    }
    
    #head {
        text-align: center;
        line-height: 50px;
        height: 50px;
        background-color: rgb(196, 94, 227);
    }
    
    #body {
        text-align: center;
        margin-top: 10px;
    }

{% endcodeblock %}

{% jsfiddle wnwtdLbn %}

## 编写一个contextmenu的插件

{% codeblock lang:html %}

html:

<!doctype html>
<html>

<head>
    <meta charset="utf-8">
    <title>Context Menu</title>
    <style>
    /* Page */
    
    html {
        width: 100%;
        height: 100%;
        background: radial-gradient(circle, #fff 0%, #a6b9c1 100%) no-repeat;
    }
    
    .container {
        left: 0;
        margin: auto;
        position: absolute;
        top: 20%;
        width: 100%;
        text-align: center;
    }
    
    h1,
    h2 {
        color: #555;
    }
    /* Menu */
    
    .menu {
        position: absolute;
        width: 200px;
        padding: 2px;
        margin: 0;
        border: 1px solid #bbb;
        background: #eee;
        background: -webkit-linear-gradient(to bottom, #fff 0%, #e5e5e5 100px, #e5e5e5 100%);
        background: linear-gradient(to bottom, #fff 0%, #e5e5e5 100px, #e5e5e5 100%);
        z-index: 100;
        border-radius: 3px;
        box-shadow: 1px 1px 4px rgba(0, 0, 0, .2);
        opacity: 0;
        -webkit-transform: translate(0, 15px) scale(.95);
        transform: translate(0, 15px) scale(.95);
        transition: transform 0.1s ease-out, opacity 0.1s ease-out;
        pointer-events: none;
    }
    
    .menu-item {
        display: block;
        position: relative;
        margin: 0;
        padding: 0;
        white-space: nowrap;
    }
    
    .menu-btn {
        display: block;
        color: #444;
        font-family: 'Roboto', sans-serif;
        font-size: 13px;
        cursor: pointer;
        border: 1px solid transparent;
        white-space: nowrap;
        padding: 6px 8px;
        border-radius: 3px;
    }
    
    button.menu-btn {
        background: none;
        line-height: normal;
        overflow: visible;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        width: 100%;
        text-align: left;
    }
    
    a.menu-btn {
        outline: 0 none;
        text-decoration: none;
    }
    
    .menu-text {
        margin-left: 25px;
    }
    
    .menu-btn .fa {
        position: absolute;
        left: 8px;
        top: 50%;
        -webkit-transform: translateY(-50%);
        transform: translateY(-50%);
    }
    
    .menu-item:hover > .menu-btn {
        color: #fff;
        outline: none;
        background-color: #2E3940;
        background: -webkit-linear-gradient(to bottom, #5D6D79, #2E3940);
        background: linear-gradient(to bottom, #5D6D79, #2E3940);
        border: 1px solid #2E3940;
    }
    
    .menu-item.disabled {
        opacity: .5;
        pointer-events: none;
    }
    
    .menu-item.disabled .menu-btn {
        cursor: default;
    }
    
    .menu-separator {
        display: block;
        margin: 7px 5px;
        height: 1px;
        border-bottom: 1px solid #fff;
        background-color: #aaa;
    }
    
    .menu-item.submenu::after {
        content: "";
        position: absolute;
        right: 6px;
        top: 50%;
        -webkit-transform: translateY(-50%);
        transform: translateY(-50%);
        border: 5px solid transparent;
        border-left-color: #808080;
    }
    
    .menu-item.submenu:hover::after {
        border-left-color: #fff;
    }
    
    .menu .menu {
        top: 4px;
        left: 99%;
    }
    
    .show-menu,
    .menu-item:hover > .menu {
        opacity: 1;
        -webkit-transform: translate(0, 0) scale(1);
        transform: translate(0, 0) scale(1);
        pointer-events: auto;
    }
    
    .menu-item:hover > .menu {
        -webkit-transition-delay: 300ms;
        transition-delay: 300ms;
    }
    </style>
</head>
<menu class="menu">
    <li class="menu-item">
        <button type="button" class="menu-btn">
            <i class="fa fa-folder-open"></i>
            <span class="menu-text">Open</span>
        </button>
    </li>
    <li class="menu-item disabled">
        <button type="button" class="menu-btn">
            <span class="menu-text">Open in New Window</span>
        </button>
    </li>
    <li class="menu-separator"></li>
    <li class="menu-item">
        <button type="button" class="menu-btn">
            <i class="fa fa-reply"></i>
            <span class="menu-text">Reply</span>
        </button>
    </li>
    <li class="menu-item">
        <button type="button" class="menu-btn">
            <i class="fa fa-star"></i>
            <span class="menu-text">Favorite</span>
        </button>
    </li>
    <li class="menu-item submenu">
        <button type="button" class="menu-btn">
            <i class="fa fa-users"></i>
            <span class="menu-text">Social</span>
        </button>
        <menu class="menu">
            <li class="menu-item">
                <button type="button" class="menu-btn">
                    <i class="fa fa-comment"></i>
                    <span class="menu-text">Comment</span>
                </button>
            </li>
            <li class="menu-item submenu">
                <button type="button" class="menu-btn">
                    <i class="fa fa-share"></i>
                    <span class="menu-text">Share</span>
                </button>
                <menu class="menu">
                    <li class="menu-item">
                        <button type="button" class="menu-btn">
                            <i class="fa fa-twitter"></i>
                            <span class="menu-text">Twitter</span>
                        </button>
                    </li>
                    <li class="menu-item">
                        <button type="button" class="menu-btn">
                            <i class="fa fa-facebook-official"></i>
                            <span class="menu-text">Facebook</span>
                        </button>
                    </li>
                    <li class="menu-item">
                        <button type="button" class="menu-btn">
                            <i class="fa fa-google-plus"></i>
                            <span class="menu-text">Google Plus</span>
                        </button>
                    </li>
                    <li class="menu-item">
                        <button type="button" class="menu-btn">
                            <i class="fa fa-envelope"></i>
                            <span class="menu-text">Email</span>
                        </button>
                    </li>
                </menu>
            </li>
        </menu>
    </li>
    <li class="menu-separator"></li>
    <li class="menu-item">
        <button type="button" class="menu-btn">
            <i class="fa fa-download"></i>
            <span class="menu-text">Save</span>
        </button>
    </li>
    <li class="menu-item">
        <button type="button" class="menu-btn">
            <i class="fa fa-edit"></i>
            <span class="menu-text">Rename</span>
        </button>
    </li>
    <li class="menu-item">
        <button type="button" class="menu-btn">
            <i class="fa fa-trash"></i>
            <span class="menu-text">Delete</span>
        </button>
    </li>
</menu>
<div class="container">
    <h1>Context Menu</h1>
    <h2>(right-click anywhere)</h2>
</div>

<script src="./two.js">
    

</script>
</html>


js:

var menu = document.querySelector('.menu');

function showMenu(x, y){
    menu.style.left = x + 'px';
    menu.style.top = y + 'px';
    menu.classList.add('show-menu');
}

function hideMenu(){
    menu.classList.remove('show-menu');
}

function onContextMenu(e){
    e.preventDefault();
    showMenu(e.pageX, e.pageY);
    document.addEventListener('mousedown', onMouseDown, false);
}

function onMouseDown(e){
    hideMenu();
    document.removeEventListener('mousedown', onMouseDown);
}

document.addEventListener('contextmenu', onContextMenu, false);

{% endcodeblock %}


{% jsfiddle v0qgpoxd %}

## 一个字符串hello(world(ni(hao(shijie)))),找出第n个括号中的内容

```

str = "hello(world(ni(hao(shijie))))".match(/([^()]+)/gi);

str[n]

(?:[a-z]*\(){3}([a-z]*\)){3}

```

## 懒加载

>对于图片过多的页面，为了加速页面加载速度，所以很多时候我们需要将页面内未出现在可视区域内的图片先不做加载， 等到滚动到可视区域后再去加载
>当访问一个页面的时候，先把img元素或是其他元素的背景图片路径替换成loading图片地址（这样就只需请求一次） 等到一定条件（这里是页面滚动到一定区域），用实际存放img地址的laze-load属性的值去替换src属性，即可实现’懒加载’。//即使img的src值为空，浏览器也会对服务器发送请求。所以平时做项目的时候，如果img没有用到src，就不要出现src这个属性


{% codeblock lang:js %}

var num = document.getElementsByTagName('img').length;
    var img = document.getElementsByTagName("img");
    var n = 0; //存储图片加载到的位置，避免每次都从第一张图片开始遍历
    lazyload(); //页面载入完毕加载可是区域内的图片
    window.onscroll = lazyload;

    function lazyload() { //监听页面滚动事件
        var seeHeight = document.documentElement.clientHeight; //可见区域高度
        var scrollTop = document.documentElement.scrollTop || document.body.scrollTop; //滚动条距离顶部高度
        for (var i = n; i < num; i++) {
            if (img[i].offsetTop ... seeHeight + scrollTop) {
                if (img[i].getAttribute("src") == "default.jpg") {
                    img[i].src = img[i].getAttribute("data-src");
                }
                n = i + 1;
            }
        }
    }

{% endcodeblock %}

[js实现一个图片懒加载插件](http://www.zyy1217.com/2017/03/20/js实现一个图片懒加载插件/)

[高性能滚动 scroll 及页面渲染优化原理与实例](http://www.zyy1217.com/2017/03/16/高性能滚动%20scroll%20及页面渲染优化/)

## 给定一个元素获取它相对于视图窗口的坐标(?)



## 实现一个常见的移动端三栏布局。上下定高且固定fix，中间栏自适应高度且可以滚动(?)

## 百度的自动补全的搜索框，需要实现用鼠标和键盘上下键都能选取补全列表中的item(?)

仿百度实时下拉搜索框,源码[在这里](https://github.com/fyuanfen/fyuanfen.github.io/tree/master/searchlist)

## 瀑布流(?)

>瀑布流布局主要分为两部分：
 1) 数据块排列，
算法步骤简述下: 
初始化时，对容器中已有数据块元素进行第一次计算，需要用户给定: 
a，容器元素 — 以此获取容器总宽度；
b，列宽度；
最终列数取的是容器宽度/列宽度； 
获得列数后，需要保存每个列的当前高度，这样在添加每个数据块时，才知道起始高度是多少；
依次取容器中的所有数据块，先寻找当前高度最小的某列，之后根据列序号，确定数据块的left，top值，left 为所在列的序号乘以列宽，top 为所在列的当前高度，最后更新所在列的当前高度加上这个数据块元素的高度，至此，插入一个元素结束。

>异步加载数据，前面讲的是如何对容器中已有元素进行排列，但很多情况下，还需要不断加载新数据块，仅包含两个步骤: 绑定滚动事件，并确定预加载线高度值，即滚动到哪个高度后，需要去加载数据，其实这个就是列的最小高度值，这样当前滚动值和最小高度值比较一下即可判断出来，是否要触发加载数据


## 直播点赞按钮的冒泡功能如何实现(?)

## 聊天室的实现(?)

## 实现新闻订阅功能(?)


## 实现一个前端模板引擎(?)

## 去除代码里面所有的空格和换行(?)

## node.js实现CORS(?)

## 100行简单实现AMD异步模块加载器（JS设计模式这本书上有样例代码）(?)

## flex怎么实现一个直径100px的圆放在屏幕中间(?)

## web socket 心跳(?)

## 前端路由

实现方式
Hash
History
原理
以 hash 形式（也可以使用 History API 来处理）为例，当 url 的 hash 发生变化时，触发 hashchange 注册的回调，回调中去进行不同的操作，进行不同的内容的展示

思路
* init 监听浏览器 url hash 更新事件
* route 存储路由更新时的回调到回调数组routes中，回调函数将负责对页面的更新
* refresh 执行当前url对应的回调函数，更新页面
利用HTML5新特性
需要注意的是：pushState()和replaceState()方法存在安全方面的限制，本地测试是无效的，会报错，可以简单放到任何服务端测试，或者使用http-server开启简单服务器
代码
{% codeblock lang:js %}


function Router() {
    this.routes = {};
    this.currentUrl = '';
}
Router.prototype.route = function(path, callback) {
    this.routes[path] = callback || function(){};
};
Router.prototype.refresh = function() {
    this.currentUrl = location.hash.slice(1) || '/';
    this.routes[this.currentUrl]();
};
Router.prototype.init = function() {
    window.addEventListener('load', this.refresh.bind(this), false);
    window.addEventListener('hashchange', this.refresh.bind(this), false);
}
window.Router = new Router();
window.Router.init();



      <ul> 
          <li><a href="#/">turn white</a></li> 
          <li><a href="#/blue">turn blue</a></li> 
          <li><a href="#/green">turn green</a></li> 
      </ul> 


var content = document.querySelector('body');
// change Page anything
function changeBgColor(color) {
    content.style.backgroundColor = color;
}
Router.route('/', function() {
    changeBgColor('white');
});
Router.route('/blue', function() {
    changeBgColor('blue');
});
Router.route('/green', function() {
    changeBgColor('green');
});



<script>
 var currentPage = 5; // prefilled by server！！！！
 function go(d) {
     setupPage(currentPage + d);
     history.pushState(currentPage, document.title, '?x=' + currentPage);
 }
 onpopstate = function(event) {
     setupPage(event.state);
 }
 function setupPage(page) {
     currentPage = page;
     document.title = 'Line Game - ' + currentPage;
     document.getElementById('coord').textContent = currentPage;
     document.links[0].href = '?x=' + (currentPage+1);
     document.links[0].textContent = 'Advance to ' + (currentPage+1);
     document.links[1].href = '?x=' + (currentPage-1);
     document.links[1].textContent = 'retreat to ' + (currentPage-1);
 }
</script>



{% endcodeblock %}


## History API


### history.pushState


### history.replaceState


### 相同之处是两个 API 都会操作浏览器的历史记录，而不会引起页面的刷新。  不同之处在于，pushState会增加一条新的历史记录，而replaceState则会替换当前的历史记录。


