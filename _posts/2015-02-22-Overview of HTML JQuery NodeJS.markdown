---
layout:       post
title:        "Overview of HTML JQuery NodeJS"
subtitle:     "Overview of HTML JQuery NodeJS"
date:         2015-02-22 12:00:00
author:       "Gary"
header-img:   "img/post-bg-android.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - HTML
    - JQuery
    - NodeJS
---
## HTML
- HTML (**H**yper **T**ext **M**arkup **L**anguage)
- HTML markup language


```HTML
<!-- header -->
<h1>This is a heading</h1>
<h2>This is a heading</h2>
<h3>This is a heading</h3>
 
<!-- paragraph -->
<p>This is a paragraph.</p>
<p>This is another paragraph.</p>
 
<!-- html link -->
<a href="http://www.w3school.com.cn">This is a link</a>
 
<!-- img -->
<img src="w3school.jpg" width="104" height="142" />
 
<!-- newline -->
<br />
 
<!-- css link -->
<link rel="stylesheet" type="text/css" href="mystyle.css">
 
<!-- js link -->
<link rel="stylesheet" type="text/css" href="mystyle.css">
 
<style type="text/css">
body {background-color: red}
p {margin-left: 20px}
</style>
 
<!-- table -->
<table border="1">
<!-- table row-->
<tr>
<!-- table data-->
<td>row 1, cell 1</td>
<td>row 1, cell 2</td>
</tr>
<tr>
<td>row 2, cell 1</td>
<td>row 2, cell 2</td>
</tr>
</table>
 
<!-- ordered list-->
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
 
<!-- unordered list-->
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
 
<select name="cars">
<option value="volvo">Volvo</option>
<option value="saab">Saab</option>
<option value="fiat">Fiat</option>
<option value="audi">Audi</option>
</select>
 
<!-- value indicates value to submit-->
 
<!-- property-->
<!-- align -->
<h1 align="center"> 
<!-- background color -->
<body bgcolor="yellow">
```

## JQuery
###### Basic Syntax
```javascript
$(selector).action()
```

###### Selection
```javascript
// Hide Current Cotent
$(this).hide() 
// Hide Current Paragraph
$("p").hide() 
// Hide All Elements which has a class equals to test
$(".test").hide() 
// Hide All Elements which has a id equals to test
$("#test").hide() 

$("p.intro")
//选取所有 id="demo" 的 <p> 元素
$("p#demo")
//选取所有带有 href 属性的元素
$("[href]") 
//选取所有带有 href 值等于 "#" 的元素
$("[href='#']") 
//选取所有带有 href 值不等于 "#" 的元素
$("[href!='#']") 
//选取所有 href 值以 ".jpg" 结尾的元素
$("[href$='.jpg']") 
$("p").css("background-color","red");
// 每个 <ul> 的第一个 <li> 元素
$("ul li:first")   
```

###### Document Ready Function
```javascript
$(document).ready(function(){
 
//--- jQuery functions go here ----
 
});
```

###### Event
```javascript
// 将函数绑定到文档的就绪事件（当文档完成加载时）
$(document).ready(function)

// 触发或将函数绑定到被选元素的点击事件
$(selector).click(function)

// 触发或将函数绑定到被选元素的双击事件
$(selector).dblclick(function) 

// 触发或将函数绑定到被选元素的获得焦点事件
$(selector).focus(function)

// 触发或将函数绑定到被选元素的鼠标悬停事件
$(selector).mouseover(function)
```



###### AJAX
```javascript
// jQuery load()
$(selector).load(URL, data, callback);
/*
必需的 URL 参数规定您希望加载的 URL。
可选的 data 参数规定与请求一同发送的查询字符串键/值对集合。
可选的 callback 参数是 load() 方法完成后所执行的函数名称。
 
Callback 可选参数
responseTxt - 包含调用成功时的结果内容
statusTXT - 包含调用的状态
xhr - 包含 XMLHttpRequest 对象
*/
```

###### HTTP
```javascript
$.get(URL, callback);
 
// 必需的 URL 参数规定您希望请求的 URL。
// 可选的 callback 参数是请求成功后所执行的函数名。
 
$.post(URL, data, callback);
 
// 必需的 URL 参数规定您希望请求的 URL。
// 可选的 data 参数规定连同请求发送的数据。
// 可选的 callback 参数是请求成功后所执行的函数名。
```

###### No Confilct
```javascript
var jq = $.noConflict();
jq(document).ready(function(){
  jq("button").click(function(){
    jq("p").text("jQuery 仍在运行！");
  });
});
```

## NodeJS
```javascript
// check the version of nodejs
// node -v
 
// execute js file
// node xx.js
 
// console 
console.log('Hello World');
 
// import module
var http = require("http");
 
 
/*package management
 
    local installation
        npm install <package>
 
    global installation
        npm install <package> -g
     
    uninstall package
         npm uninstall <package>
 
    update package
        npm update <package>
     
    search package
        npm search <package>
     
    create package
        npm init
     
    clear cache
        npm cache clear
     
    add user
        npm adduser
 
    publish package
        npm publish
     
    unpublish package
        npm unpublish <package>
 
    help
        npm help
 
    update npm self
        npm install npm -g
 
    check the package list 
        npm ls
*/
 
 
/* package description
name          包名
version       包的版本号
description   包的描述
homepage      包的官网 url 
author        包的作者姓名
contributors  包的其他贡献者姓名
dependencies  依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下
repository    包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上
main          是一个模块ID，它是一个指向你程序的主要项目。就是说，如果你包的名字叫 express，然后用户安装它，然后require("express")
keywords      关键字
*/
 
 
// REPL Read Eval Print Loop
 
// enter REPL
// node
 
// variable
// if user doesnot use var , it will print out
var somewhat_variable = somewhat_value;
 
// _
// just like python ,you can get the last reuslt with _
 
// .help                list current command
// .break .clear        退出多行表达式
// .save filename       save file
// .load filename       load file
 
 
// node callback function
var fs = require('fs');
 
fs.readFile('somewhat-txtfile.txt', function(err, data){
    if(err) return console.error(err);
    console.log(data.toString());
});
 
/*
Node.js 是单进程单线程应用程序，但是通过事件和回调支持并发，所以性能非常高。
Node.js 的每一个 API 都是异步的，并作为一个独立线程运行，使用异步函数调用，并处理并发。
Node.js 基本上所有的事件机制都是用设计模式中观察者模式实现。
Node.js 单线程类似进入一个while(true)的事件循环，直到没有事件观察者退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数.
*/
 
// import events package
var events = require('events');
// create eventEmitter 
var eventEmitter = new events.eventEmitter();
 
// binding event and event handler
eventEmitter.on('event_name', eventHandler);
 
// trigger event
eventEmitter.emit('event_name');
 
/*
Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。
Node.js 里面的许多对象都会分发事件：
一个net.Server对象会在每次有新连接时分发一个事件， 
一个fs.readStream对象会在文件被打开的时候发出一个事件。 
所有这些产生事件的对象都是 events.EventEmitter 的实例。
*/
 
 
 
// 为指定事件添加一个监听器到监听器数组的尾部
// addListener(event, listener)
 
// on(event, listener)
server.on('connection', function (stream) {
  console.log('someone connected!');
});
 
// once(event, listener)
// 为指定事件注册一个单次监听器，即 监听器最多只会触发一次，触发后立刻解除该监听器。
server.once('connection', function (stream) {
  console.log('Ah, we have our first user!');
});
 
// removeListener(event, listener)
// 移除指定事件的某个监听器，监听器 必须是该事件已经注册过的监听器。
var callback = function(stream) {
  console.log('someone connected!');
};
server.on('connection', callback);
server.removeListener('connection', callback);
 
// removeAllListeners([event])
// 移除所有事件的所有监听器， 如果指定事件，则移除指定事件的所有监听器。
 
// setMaxListeners(n)
// 默认情况下， EventEmitters 如果你添加的监听器超过 10 个就会输出警告信息。 setMaxListeners 函数用于提高监听器的默认限制的数量。
 
// listeners(event)
// 返回指定事件的监听器数组。
 
// emit(event, [arg1], [arg2], [...])
// 按参数的顺序执行每个监听器，如果事件有注册监听返回 true，否则返回 false。
 
// error event
 
// EventEmitter 定义了一个特殊的事件 error，它包含了错误的语义，我们在遇到 异常的时候通常会触发 error 事件。
// 当 error 被触发时，EventEmitter 规定如果没有响 应的监听器，Node.js 会把它当作异常，退出程序并输出错误信息。
// 我们一般要为会触发 error 事件的对象设置监听器，避免遇到错误后整个程序崩溃。例如：
var events = require('events'); 
var emitter = new events.EventEmitter(); 
emitter.emit('error'); 
 
 
 
 
// Buffer
 
// JavaScript 语言自身只有字符串数据类型，没有二进制数据类型。但在处理像TCP流或文件流时，必须使用到二进制数据。
// 因此在 Node.js中，定义了一个 Buffer 类，该类用来创建一个专门存放二进制数据的缓存区。
// 在 Node.js 中，Buffer 类是随 Node 内核一起发布的核心库。
// Buffer 库为 Node.js 带来了一种存储原始数据的方法，可以让 Node.js 处理二进制数据，
// 每当需要在 Node.js 中处理I/O操作中移动的数据时，就有可能使用 Buffer 库。原始数据存储在 Buffer 类的实例中。
// 一个 Buffer 类似于一个整数数组，但它对应于 V8 堆内存之外的一块原始内存。
 
 
// create 10 byte buffer
var buf = new Buffer();
 
// create array buffer
var buf = new Buffer([10, 20, 30, 40, 50]);
 
// create a buffer by string
var buf = new Buffer("www.baidu.com", "utf-8")
// support "ascii", "utf8", "utf16le", "ucs2", "base64", "hex" decode
 
 
/* write to buffer
 
buf.write(string[, offset[, length]][, encoding])
string     写入缓冲区的字符串
offset     缓冲区开始写入的索引值，默认为 0 
length     写入的字节数，默认为 buffer.length
encoding   使用的编码。默认为 'utf8' 
*/
 
 
/* read from buffer
buf.toString([encoding[, start[, end]]])
 
encoding   使用的编码。默认为 'utf8' 
start      指定开始读取的索引位置，默认为 0
end        结束位置，默认为缓冲区的末尾
*/
 
/* buffer to json
buf.toJSON()
*/
 
var buf = new Buffer("www.baidu.com");
var json = buf.toJSON();
 
console.log(json);
 
/* combine buffer
Buffer.concat(list[, totalLength])
 
list         用于合并的 Buffer 对象数组列表
totalLength  指定合并后Buffer对象的总长度
*/
 
 
/* buffer compare
buf.compare(otherBuffer);
 
return a integer, 表示 buf 在 otherBuffer 之前，之后或相同。
*/
 
/* buffer copy
 
buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]])
 
targetBuffer   要拷贝的 Buffer 对象。
targetStart    数字, 可选, 默认: 0
sourceStart    数字, 可选, 默认: 0
sourceEnd      数字, 可选, 默认: buffer.length
 
*/
 
 
// module system
 
// ./ reresent current directory
var hello = require("./hello");
hello.word();
 
// use exprots to exports object to other module
exports.word = function() {
    console.log("Hello world");
}
 
// use module to export the whole file into one object
module.exports = function Hello() {
    var name;
    name = "Hello World";
    this.Set_Name()
}
 
// global variable
// __filename   current filepath
// __dirnamee   current dir name
// SetTimeout(cb, ms) 
// clearTimeout(t)
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
var t = setTimeout(printHello, 2000);
 
// 清除定时器
clearTimeout(t);
 
// setInterval(cb, ms)
 
// console
// process
 
 
 
// util
var util = require('util');
 
function Base() {
    this.name = "base";
    this.base = 1991;
    this.foo = function () {
        console.log('hello' + this.name);
    }
}
 
Base.prototype.showName = function () {
    console.log(this.name);
};
 
function Sub(){
    this.name = 'Sub';
}
 
util.inherits(Sub, Base);
var objBase = new Base();
 
 
util.isArray(obj);
 
util.isRegExp(obj);
 
util.isDate(obj);
```