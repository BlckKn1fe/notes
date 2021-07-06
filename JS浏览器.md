---
title: JS浏览器
date: 2020-08-10 00:11:49
categories: 
- 前端
tags:
- 前端
- JavaScript
---

> 这里笔记内容都是参考廖雪峰对于 JavaScript 教程而进行有针对性的提炼的
>
> https://www.liaoxuefeng.com/wiki/1022910821149312/
>
> 有部分内容来自菜鸟教程
>
> https://www.runoob.com/ajax/ajax-tutorial.html



目前主流的浏览器分这么几种：

- IE 6~11：国内用得最多的IE浏览器，历来对W3C标准支持差。**从IE10开始支持ES6标准；**
- Chrome: Google 出品的基于 Webkit 内核浏览器，内置了非常强悍的 JavaScript 引擎——V8。由于 Chrome一经安装就时刻保持自升级，所以不用管它的版本，最新版早就支持ES6了；
- Safari: Apple 的 Mac 系统自带的基于 Webkit 内核的浏览器，从OS X 10.7 Lion自带的6.1版本开始支持ES6，目前最新的 OS X 10.11 El Capitan自带的 Safari 版本是9.x，早已支持ES6；
- Firefox: Mozilla 自己研制的Gecko内核和JavaScript引擎 OdinMonkey。早期的Firefox按版本发布，后来终于聪明地学习 Chrome 的做法进行自升级，时刻保持最新；
- 移动设备上目前iOS和Android两大阵营分别主要使用 Apple 的 Safari 和 Google 的 Chrome，由于两者都是Webkit 核心，结果 HTML5 首先在手机上全面普及（桌面绝对是 Microsoft 拖了后腿），对 JavaScript 的标准支持也很好，最新版本均支持ES6。



# 各种对象

JavaScript可以获取浏览器提供的很多对象，并进行操作。（这里应该就是 BOM了，全称 Browser Object Model）

 ## window

`window`对象有`innerWidth`和`innerHeight`属性，可以获取浏览器窗口的内部宽度和高度。内部宽高是指除去菜单栏、工具栏、边框等占位元素后，用于显示网页的净宽高。还有一个`outerWidth`和`outerHeight`属性，可以获取浏览器窗口的整个宽高。

兼容性：IE<=8不支持。



## navigator

`navigator`对象表示浏览器的信息，最常用的属性包括：

- navigator.appName：浏览器名称；
- navigator.appVersion：浏览器版本；
- navigator.language：浏览器设置的语言；
- navigator.platform：操作系统类型；
- navigator.userAgent：浏览器设定的`User-Agent`字符串。
- ...

***请注意***，`navigator`的信息可以很容易地被用户修改，所以 JavaScript 读取的值不一定是正确的。



## screen

- screen.width：屏幕宽度，以像素为单位；
- screen.height：屏幕高度，以像素为单位；
- screen.colorDepth：返回颜色位数，如8、16、24。



## location

因为 location 理解成网站访问的 “地址” 即可

`location`对象表示当前页面的URL信息。例如，一个完整的URL：

```
http://www.example.com:8080/path/index.html?a=1&b=2#TOP
```

可以用`location.href`获取。要获得URL各个部分的值，可以这么写：

```
location.protocol; // 'http'
location.host; // 'www.example.com'
location.port; // '8080'
location.pathname; // '/path/index.html'
location.search; // '?a=1&b=2'
location.hash; // 'TOP'
```

要加载一个新页面，可以调用`location.assign()`。如果要重新加载当前页面，调用`location.reload()`方法非常方便。注意这里的 assign() 好像是针对当前的 url 来说的！



## document

`document`对象表示当前页面。由于HTML在浏览器中以DOM形式表示为树形结构，`document`对象就是整个DOM树的根节点。

`document`的`title`属性是从HTML文档中的`<title>xxx</title>`读取的，但是可以动态改变

```
document.title = '努力学习JavaScript!';
```

`document`对象提供的`getElementById()`和`getElementsByTagName()`可以按ID获得一个DOM节点和按Tag名称获得一组DOM节点

```
var drinks = document.getElementsByTagName('dt');
```

`document`对象还有一个`cookie`属性，可以获取当前页面的Cookie。



# DOM

DOM 全称 **D**ocument **O**bject **M**odel，DOM 的定义如下：

>*"The W3C Document Object Model (DOM) is a platform and language-neutral interface that allows programs and scripts to dynamically access and update the content, structure, and style of a document."*

主要学习 HTML DOM，这东西用一句话概括就是：**The HTML DOM is a standard for how to get, change, add, or delete HTML elements.**



## 获取/更新

这个 Element 其实代表的是 DOM 这个 Tree 中的一个 Element Node

```javascript
var node = document.getElementById();
node.value;
```

还可以通过：

```javascript
document.getElementsByTagName()
document.getElementsByClassName()
```

获取到这个 DOM 节点之后可以做很多操作，比如**修改其内容**

```javascript
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置文本为abc:
p.innerHTML = 'ABC'; // <p id="p-id">ABC</p>
```

用 `innerHTML ` 时要注意，是否需要写入HTML。如果写入的字符串是通过网络拿到了，要注意对字符编码来避免XSS攻击。

第二种是修改 `innerText` 或 `textContent ` 属性，这样可以自动对字符串进行HTML编码，保证无法设置任何HTML标签：

```javascript
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置文本:
p.innerText = '<script>alert("Hi")</script>';
// HTML被自动编码，无法设置一个<script>节点:
// <p id="p-id">&lt;script&gt;alert("Hi")&lt;/script&gt;</p>
```

`innerText` 不返回隐藏元素的文本，而 `textContent `返回所有文本。

通过 DOM 还能**修改 CSS 样式**：

>https://www.w3schools.com/jsref/dom_obj_style.asp

```javascript
 获取<p id="p-id">...</p>
var p = document.getElementById("p-id");
// 设置CSS:
p.style.color = "#ff0000";
p.style.fontSize = "20px";
p.style.paddingTop = "2em";
```

还能 DOM 操作修改图片：

```javascript
document.getElementById("myImage").src = "landscape.jpg";
```



## 插入

当**节点为空**，例如，`<div></div>`，那么，直接使用`innerHTML = '<span>child</span>'`就可以修改DOM节点的内容，相当于“插入”了新的DOM节点。如果这个**DOM节点不是空的**，那就不能这么做，因为`innerHTML`会直接替换掉原来的所有子节点。

有两个办法可以插入新的节点。一个是使用`appendChild`，把一个子节点添加到父节点的最后一个子节点。

```javascript
var
    js = document.getElementById('js'),
    list = document.getElementById('list');
list.appendChild(js);
```

还可以**创建一个新的节点**：

```javascript
var
    list = document.getElementById('list'),
    haskell = document.createElement('p');
haskell.id = 'haskell';
haskell.innerText = 'Haskell';
list.appendChild(haskell);
```

**动态创建一个节点**然后添加到DOM树中，可以实现很多功能。举个例子，动态地给文档添加了新的CSS定义：

```javascript
var d = document.createElement('style');
d.setAttribute('type', 'text/css');
d.innerHTML = 'p { color: red }';
document.getElementsByTagName('head')[0].appendChild(d);
```

还可以插在具体的某一个元素的前面，就不展开写了：

```javascript
parentElement.insertBefore(newElement, referenceElement);
```



## 删除

删除有俩办法，第一个办法是**先获取到要删除的 Node：**

```javascript
var elmnt = document.getElementById("p1");
elmnt.remove();
```

但是老的浏览器会不支持，需要用父级 Node 去删除子级 Node，所以最最最好的办法就是这样：

```javascript
child.parentNode.removeChild(child);  // W3C 官方给出的
```

注意到删除后的节点虽然不在文档树中了，但其实它还在内存中，可以随时再次被添加到别的位置，remove 会把被删除的 Node 返回。

当我们用如下代码删除子节点时：

```javascript
var parent = document.getElementById('parent');
parent.removeChild(parent.children[0]);
parent.removeChild(parent.children[1]); // <-- 浏览器报错
```

浏览器报错：`parent.children[1]`不是一个有效的节点。原因就在于，当`<p>First</p>`节点被删除后，`parent.children`的节点数量已经从2变为了1，索引`[1]`已经不存在了。

因此，删除多个节点时，要注意`children`属性时刻都在变化。





## Navigation

![image-20200810221004551](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200810221004551.png)

因为 DOM 其实是一个**树状结构**，所以每一个 Element 其实是看做一个 Node，然后一个 Node 有它的 parent，有 sibling 等等：

- `parentNode`
- `childNodes[*nodenumber*]`
- `firstChild`
- `lastChild`
- `nextSibling`
- `previousSibling`

下面这俩用来获取根节点

- `document.body` - The body of the document  // 只获取 Body 标签部分
- `document.documentElement` - The full document  // 从头到尾



The `nodeName` property specifies the name of a node.

- nodeName is read-only
- nodeName of an element node is the same as the tag name
- nodeName of an attribute node is the attribute name
- nodeName of a text node is always #text
- nodeName of the document node is always #document



The `nodeValue` property specifies the value of a node.

- nodeValue for element nodes is `null`
- nodeValue for text nodes is the text itself
- nodeValue for attribute nodes is the attribute value



The `nodeType` property is read only. It returns the type of a node.

![image-20200810163241510](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200810163241510.png)



## Event

这个事件非常的关键，让网页动起来就靠它了！有很多很多的 event，先贴一个连接来：

> https://www.w3schools.com/jsref/dom_obj_event.asp

有很多很多的 Event，比如鼠标点击 ```onclick``` ，其后面可以跟一个 function，或者直接对 element 进行修改：

```html
<h1 onclick="this.innerHTML = 'Ooops!'">Click on this text!</h1>
```

或者：

```html
<h1 onclick="changeText(this)">Click on this text!</h1>

<script>
function changeText(id) {
  id.innerHTML = "Ooops!";
}
```

再或者：

```html
<button onclick="displayDate()">Try it</button>
```

还有在网页一加载的时候就可以执行的 event: `onload` and `onunload` events，只要某个东西一加载，事件就开始执行；

还有 ```onchange``` ，就是当某个 element 修改之后会执行，比如在输入框里，输入完所有内容变大写；

还有 onmouseover 和 onmouseout ，鼠标经过会执行事件；

还有 onmousedown 和 onmouseup，这俩一般一起使用，鼠标点下去一个事件，松开之后又一个



还有一种正确的添加 event 的姿势，可以无限的给一个 element 进行添加，甚至可以添加同一种类型的 event：

```javascript
element.addEventListener(event, function, useCapture);
```

第一个参数为时间类型，参考上面的链接；第二个是传入一个 function 的名字。这里的 function 可以是匿名函数

```javascript
element.addEventListener("click", function(){ alert("Hello World!"); });
```

或者

```javascript
element.addEventListener("click", myFunction);

function myFunction() {
  alert ("Hello World!");
}
```

再或者

```javascript
element.addEventListener("click", function(){ myFunction(p1, p2); });
```

最后这个情况是用一个匿名函数去 call 一个需要传参的函数，这样似乎好像是就没办法 remove 这个 listener 了...



然后说一个很关键的参数，就是第三个参数

事件的运作模式分为 Bubbling 和 Capturing，一句话简单概括的话就是：Bubbling  是先从里到外，而 Capturing 是从外到里。当两个 element 都被添加相同的 event 的时候，事件同时触发的话，bubbling 是从里层盒子先执行，然后是外层，capturing 的话就是反过来。

```html
<!DOCTYPE html>
<html>

<head>
    <style>
        #myDiv1,
        #myDiv2 {
            background-color: coral;
            padding: 50px;
        }

        #myP1,
        #myP2 {
            background-color: white;
            font-size: 20px;
            border: 1px solid;
            padding: 20px;
        }
    </style>
    <meta content="text/html; charset=utf-8" http-equiv="Content-Type">
</head>

<body>

    <h2>JavaScript addEventListener()</h2>

    <div id="myDiv1">
        <h2>Bubbling:</h2>
        <p id="myP1">Click me!</p>
    </div><br>

    <div id="myDiv2">
        <h2>Capturing:</h2>
        <p id="myP2">Click me!</p>
    </div>

    <script>
        document.getElementById("myP1").addEventListener("click", function () {
            alert("You clicked the white element!");
        }, false);

        document.getElementById("myDiv1").addEventListener("click", function () {
            alert("You clicked the orange element!");
        }, false);

        document.getElementById("myP2").addEventListener("click", function () {
            alert("You clicked the white element!");
        }, true);

        document.getElementById("myDiv2").addEventListener("click", function () {
            alert("You clicked the orange element!");
        }, true);
    </script>

</body>

</html>
```

这个例子非常好！

有些老版本这个没法用，就要用  `attachEvent()` 和 `detachEvent()` （参数第一个是 event，似乎要写详细的 event 名字，第二个传 function）



## Collections / Node Lists

The `getElementsByTagName()` method returns an `HTMLCollection` object.

The `length` property defines **the number of elements** in an `HTMLCollection`

**An HTMLCollection is NOT an array!**

An HTMLCollection may look like an array, but it is not.



A `NodeList` object is almost the same as an `HTMLCollection` object.

**All browsers** return a **NodeList** object for the property  `childNodes`. 

**Most browsers** return a **NodeList** object for the method  `querySelectorAll()`.

**不同点：**

**HTMLCollection items can be accessed by their name, id, or index number.**

**NodeList items can only be accessed by their index number.**

**Only the NodeList object can contain attribute nodes and text nodes.**

第三个很关键！只有 NodeList 才能包含属性节点和文本节点！



# 操作表单

用JavaScript操作表单和操作DOM是类似的，因为表单本身也是DOM树。

不过表单的输入框、下拉框等可以接收用户输入，所以用JavaScript来操作表单，可以获得用户输入的内容，或者对一个输入框设置新的内容。

## 获取值

如果我们获得了一个`<input>`节点的引用，就可以直接调用`value`获得对应的用户输入值：

```javascript
// <input type="text" id="email">
var input = document.getElementById('email');
input.value; // '用户输入的值'
```

这种方式**可以应用于** `text`、`password`、`hidden`以及`select`。

对于**单选框和复选框**，`value`属性返回的永远是HTML预设的值，而我们需要获得的实际是用户是否“勾上了”选项，所以应该用`checked`判断



设置值就不摘录了...



## 提交表单

第一个办法是用给 button 一个 onclick 事件，然后直接调用一个function 来提交表单，但是这个办法扰乱了浏览器对form的正常提交。浏览器默认点击`<button type="submit">`时提交表单，或者用户在最后一个输入框按回车键。因此，第二种方式是响应`<form>`本身的`onsubmit`事件，在提交form时作修改：

```html
<!-- HTML -->
<form id="test-form" onsubmit="return checkForm()">
    <input type="text" name="test">
    <button type="submit">Submit</button>
</form>

<script>
function checkForm() {
    var form = document.getElementById('test-form');
    // 可以在此修改form的input...
    // 继续下一步:
    return true;
}
</script>
```

安全考虑，提交表单时不传输明文口令，而是口令的MD5，同时为了美观考虑，可以像下面这么写（思路参考即可，实际会用 Ajax 来传。）

```html
<!-- HTML -->
<form id="login-form" method="post" onsubmit="return checkForm()">
    <input type="text" id="username" name="username">
    <input type="password" id="input-password">  <!-- 注意这里没 name 的属性 -->
    <input type="hidden" id="md5-password" name="password">
    <button type="submit">Submit</button>
</form>

<script>
function checkForm() {
    var input_pwd = document.getElementById('input-password');
    var md5_pwd = document.getElementById('md5-password');
    // 把用户输入的明文变为MD5:
    md5_pwd.value = toMD5(input_pwd.value);
    // 继续下一步:
    return true;
}
</script>
```

没有 `name `属性的 `<input> `的数据不会被提交！



# 操作文件

在HTML表单中，可以上传文件的唯一控件就是 `<input type="file">`

当一个表单包含`<input type="file">` 时，表单的 `enctype `必须指定为 `multipart/form-data`，`method `必须指定为 `post`，浏览器才能正确编码并以 `multipart/form-data `格式发送表单的数据。出于安全考虑，浏览器只允许用户点击 `<input type="file"> `来选择本地文件。

```javascript
var f = document.getElementById('test-file-upload');
var filename = f.value; // 'C:\fakepath\test.png'
if (!filename || !(filename.endsWith('.jpg') || filename.endsWith('.png') || filename.endsWith('.gif'))) {
    alert('Can only upload image file.');
    return false;
}
```



随着HTML5的普及，新增的File API允许JavaScript读取文件内容，获得更多的文件信息。

HTML5的File API提供了`File`和`FileReader`两个主要对象，可以获得文件信息并读取文件。

```javascript
var
    fileInput = document.getElementById('test-image-file'),
    info = document.getElementById('test-file-info'),
    preview = document.getElementById('test-image-preview');
// 监听change事件:
fileInput.addEventListener('change', function () {
    // 清除背景图片:
    preview.style.backgroundImage = '';
    // 检查文件是否选择:
    if (!fileInput.value) {
        info.innerHTML = '没有选择文件';
        return;
    }
    // 获取File引用:
    var file = fileInput.files[0];
    // 获取File信息:
    info.innerHTML = '文件: ' + file.name + '<br>' +
                     '大小: ' + file.size + '<br>' +
                     '修改: ' + file.lastModifiedDate;
    if (file.type !== 'image/jpeg' && file.type !== 'image/png' && file.type !== 'image/gif') {
        alert('不是有效的图片文件!');
        return;
    }
    // 读取文件:
    var reader = new FileReader();
    reader.onload = function(e) {
        var
            data = e.target.result; // 'data:image/jpeg;base64,/9j/4AAQSk...(base64编码)...'            
        preview.style.backgroundImage = 'url(' + data + ')';
    };
    // 以DataURL的形式读取文件:
    reader.readAsDataURL(file);
});
```





# Ajax

Ajax 全称 Asynchronous Javascript And XML，最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。

![image-20200812155743704](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200812155743704.png)

基本实例结构：

```html
<div id="myDiv"><h2>使用 AJAX 修改该文本内容</h2></div>
<button type="button" onclick="loadXMLDoc()">修改内容</button>

<head>
<script>
function loadXMLDoc()
{
    .... AJAX 脚本执行 ...
}
</script>
</head>
```

## 请求

简单的 GET 请求的代码实例：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script>
function loadXMLDoc()
{
	var xmlhttp;
	if (window.XMLHttpRequest)
	{
		// IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
		xmlhttp=new XMLHttpRequest();
	}
	else
	{
		// IE6, IE5 浏览器执行代码
		xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
	}
	xmlhttp.onreadystatechange=function()
	{
		if (xmlhttp.readyState==4 && xmlhttp.status==200)
		{
			document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
		}
	}
	xmlhttp.open("GET","/try/ajax/demo_get.php",true);
	xmlhttp.send();
}
</script>
</head>
<body>

<h2>AJAX</h2>
<button type="button" onclick="loadXMLDoc()">请求数据</button>
<div id="myDiv"></div>

</body>
</html>
```

上面 GET 请求的这个 php 文件有自己写好的方法，会根据调用来返回适当的数据。open 方法有三个参数

- 第一个为请求的类型
- 第二个为请求文件的在服务器的位置
- 第三个为是否异步

如果希望通过 GET 方法发送信息，向 URL 添加信息：

```javascript
xmlhttp.open("GET","/try/ajax/demo_get2.php?fname=Henry&lname=Ford",true);
xmlhttp.send();
```

## 响应

获取到服务器返回的信息的话使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性。其中第一个就直接使用就可以，第二个的话需要先获取到 XML 对象：

> 这里使用这个 xml 文件样本 [点击查看](https://www.runoob.com/try/demo_source/cd_catalog.xml)

````javascript
xmlDoc=xmlhttp.responseXML;
txt="";
x=xmlDoc.getElementsByTagName("ARTIST");
for (i=0;i<x.length;i++)
{
    txt=txt + x[i].childNodes[0].nodeValue + "<br>";
}
document.getElementById("myDiv").innerHTML=txt;
````



## readyState

当请求被发送到服务器时，我们需要执行一些基于响应的任务。每当 readyState 改变时，就会触发 onreadystatechange 事件。

![image-20200812165931810](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200812165931810.png)

0：请求未初始化（还没有调用 open()）。

1：请求已经建立，但是还没有发送（还没有调用 send()）。

2：请求已发送，正在处理中（通常现在可以从响应中获取内容头）。

3：请求在处理中；通常响应中已有部分数据可用了，但是服务器还没有完成响应的生成。

4：响应已完成；您可以获取并使用服务器的响应了。

这个 onreadystateChange 要进行函数重写，根据 XMLHTTPRequest 的状态和 status 的情况来决定下一步要对页面进行什么样的操作。

**注意：** onreadystatechange 事件被触发 4 次（0 - 4）, 分别是： 0-1、1-2、2-3、3-4，对应着 readyState 的每个变化。



# 异步

异步就是不同的步调（时间）去执行一些东西，主要是通过设置时间来进行一些操作



## setTimeOut

设定多少秒之后执行回调函数：

```javascript
setTimeOut(() => {
    console.log("一秒后执行");
}, 1000);
```

这个 TimeOut 是可以被提前取消的，使用 ```clearTimeOut``` 来实现：

```javascript
var timer = setTimeOut(() => {
    console.log("一秒后执行");
}, 1000);

clearTimeOut(timer);
```



## setInterval

这个是设定每隔多少秒调用一次回调函数

```javascript
var interval = setInterval(() => {
    let date = new Date();
    console.log(`${date.getHours()}:${date.getMinutes()}`);
}, 1000)
```

同样，这个也是可以终止的：

```javascript
clearInterval(interval);
```



## promise

promise 允许我们创建自定义的异步操作，一般它用来执行比较耗时的操作，首先先创造一个 promise

```javascript
var promise = new Promise((resolve) => {
  setTimeout(() => {
    resolve("执行成功");
  }, 2000);
});

```

```then``` 用来获取到 promise 执行之后的结果，其内容为 resolve 内的内容：

```javascript
promise.then(value => console.log(value));
```

promise 还可以捕获异常，在创建 promise 的时候需要多添加一个参数 ```reject``` ：

```javascript
var promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("执行失败");
  }, 2000);
});

promise.catch((error) => console.log(error));
// promise.then((value) => console.log(value));
```

上面如果执行失败的话，可以用 ```catch``` 来捕获异常

promise 的 then 可以链式调用，同时一般 catch 放在最后一个 then 的后面：

```javascript
new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(1);
	}, 1000);
})
	.then((value) => {
		console.log(value);
		return value + 10;
	})
	.then((value) => {
		console.log(value);
		return new Promise((resolve) => resolve(value + 20));
	})
	.then((value) => {
		console.log(value);
	})
	.catch((error) => {
		console.log(error);
	});
```

以上出现 reject 或者抛出异常，都会被 catch 到。



## 多个promise同时执行

使用 Promise.all() 可以让多个 promise 同时执行，并且在最后返回一个大的 promise，然后会返回一个结果数组，分别对应每个 promise 返回的值。

```javascript
'use strict';
var p1 = new Promise((resolve) => {
	setTimeout(() => {
		resolve(1);
	}, 1000);
});

var p2 = new Promise((resolve) => {
	setTimeout(() => {
		resolve(2);
	}, 2000);
});

var p3 = new Promise((resolve) => {
	setTimeout(() => {
		resolve(3);
	}, 500);
});

Promise.all([p1, p2, p3]).then((values) => {
	console.log(values);  // [ 1, 2, 3 ]
});

```



## async / await

这俩是，且不完全是 promise 的语法糖：

```javascript
'use strict';
async function async1() {
	setTimeout(() => {
		console.log('async 1 执行完毕');
	}, 1000);
}

async1();
```

await 必须在带 async 的函数中使用，使用期间使用 try / catch  来捕获异常：

```javascript
'use strict';
async function async1() {
	let result = await async2();
	try {
		let result2 = await async3();
	} catch (e) {
		console.log(e);
	}
	console.log(result);
}

async function async2() {
	return new Promise((resolve) => {
		setTimeout(() => {
			resolve(10);
		}, 1000);
	});
}

async function async3() {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			reject('执行出错');
		}, 500);
	});
}

async1();

```

