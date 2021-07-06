---
title: DOM/BOM操作
date: 2020-09-03 08:14:08
categories: 
- 前端
tags:
- 前端
- JavaScript
- DOM
- BOM
---

> 所有笔记内容源于 https://www.bilibili.com/video/BV1k4411w7sV



# ### DOM ###

# 获取元素

## 基础

通过 id 获取：

```html
document.getElementById();
```



通过标签名：

```
document.getElementsByTagName();
```

此方法返回的是带有该标签的**对象的集合（collection）**，注意这不是个 Array



通过父级获取父级下的指定标签对象集合：

```javascript
var ol = document.getElementById("ol");
var lis = ol.getElementsByTagName("li");
```



查看对象内的属性和方法：

```javascript
console.dir();  // 里面放对象
```



## H5新增

（H5新增）通过类名获取某些对象集合：

```javascript
 var boxs = document.getElementsByClassName('box');
```



（H5新增）querySelector：

如果选择的是一个集合的话，注意它只返回一个集合中的第一个对象

```javascript
var firstBox = document.querySelector('.box');  // 类选择

var nav = document.querySelector('#nav');  // Id 选择

var li = document.querySelector('li');  // 标签选择
```



（H5新增）querySelectorAll：

这个比上一来说，就会返回集合

```javascript
 var allBox = document.querySelectorAll('.box');

var lis = document.querySelectorAll('li');
```



## 获取特殊标签

获取 body 元素：

```javascript
var bodyEle = document.body;
```



获取 html 元素：

```javascript
var htmlEle = document.documentElement;
```



# 事件基础

JavaScript使我们有能力创建动态页面，而事件是可以被JavaScript侦测到的行为。网页中的每个元素都可以产生某些可以触发JavaScript的事件，例如，我们可以在用户点击某按钮时产生一个事件，然后去执行某些操作。

事件是有三部分组成 事件源 事件类型 事件处理程序  我们也称为事件三要素

(1) 事件源，事件被触发的对象

(2) 事件类型，如何触发，什么事件，比如鼠标点击（onclick），还是鼠标经过，还是键盘按下

(3) 事件处理程序，通过一个函数赋值的方式完成



```html
<body>
    <button id="btn">唐伯虎</button>
    <script>
        var btn = document.getElementById('btn');
        btn.onclick = function() {
            alert('点秋香');
        }
    </script>
</body>
```



## 常见鼠标事件

![image-20200903084454171](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200903084454171.png)



## 通过事件修改内容

修改一个标签内的内容：

```html
<body>
    <button>显示当前系统时间</button>
    <div>某个时间</div>

    <script>
        var btn = document.querySelector('button');
        var div = document.querySelector('div');
        btn.onclick = function() {
            div.innerHTML = getDate();
        }

        function getDate() {
			// ...
        }
    </script>
</body>
```



# 操作元素

## innerText 和 innerHTML 区别

写什么内容，就会显示什么内容，如果写了一些其他的标签，比如字体加粗，则不会识别同时空格和换行也会去掉

```javascript
element.innerText
```



在 innerText 的基础上会识别 HTML 的标签，同时保留空格和换行

```javascript
element.innerHTML
```



## 常见属性修改

修改图片的 src 和 title：

```html
<body>
    <button id="ldh">刘德华</button>
    <button id="zxy">张学友</button> <br>
    <img src="images/ldh.jpg" alt="" title="刘德华">

    <script>
        // 修改元素属性  src
        // 1. 获取元素
        var ldh = document.getElementById('ldh');
        var zxy = document.getElementById('zxy');
        var img = document.querySelector('img');
        // 2. 注册事件  处理程序
        zxy.onclick = function() {
            img.src = 'images/zxy.jpg';
            img.title = '张学友思密达';
        }
        ldh.onclick = function() {
            img.src = 'images/ldh.jpg';
            img.title = '刘德华';
        }
    </script>
</body>
```

还可以修改 href，id，alt 等等



## 操作表单

常见属性：type，value，checked，selected，disabled

```html
<body>
    <button>按钮</button>
    <input type="text" value="输入内容">
    <script>
        // 1. 获取元素
        var btn = document.querySelector('button');
        var input = document.querySelector('input');
        // 2. 注册事件 处理程序
        btn.onclick = function() {
            // 表单里面的值 文字内容是通过 value 来修改的
            input.value = '被点击了';
            // 如果想要某个表单被禁用 不能再点击 disabled  我们想要这个按钮 button禁用
            this.disabled = true;
            // this 指向的是事件函数的调用者 btn
        }
    </script>
</body>
```



## 修改 CSS 样式

修改元素大小，颜色，位置，可以用

```javascript
element.style  // 产生的是行内样式，权重比较高
```

```html
<body>
    <div></div>
    <script>
        // 1. 获取元素
        var div = document.querySelector("div");
        // 2. 注册事件 处理程序
        div.onclick = function () {
            // div.style里面的属性 采取驼峰命名法
            this.style.backgroundColor = "purple";
            this.style.width = "250px";
        };

    </script>
</body>
```



通过类来修改，可以一次修改比较复杂的，注意如果直接单独写一个类名的话，会覆盖之前的，如果要保留之前的类名的话，要注意都写在一起，注意先后顺序

```javascript
element.className
```

```html
<body>
    <div class="first">文本</div>
    <script>
        // 1. 使用 element.style获得修改元素样式，如果样式比较少 或者 功能简单的情况下使用
        var test = document.querySelector('div');
        test.onclick = function() {
            // 2. 我们可以通过，修改元素的className更改元素的样式，适合于样式较多或者功能复杂的情况
            // 3. 如果想要保留原先的类名，我们可以这么做，多类名选择器
            // this.className = 'change';
            this.className = 'first change';
        }
    </script>
</body>
```



## 获取自定义属性值

获取 HTML 元素自带的一些属性值：

```javascript
var d = document.querySelector('div');
console.log(d.id)
```

获取自定义的属性值，比如 index-number：（程序员自己自定义的）

```javascript
var d = document.querySelector('div');
console.log(d.getAttriibute('id')) // 这里可以填写自定义属性值
```



## 修改自定义属性值

修改本身自带的属性，直接通过 ```.``` 来修改：

```javascript
div.id = 'test';
div.className = 'navs';
```

修改自定义属性的话，通过 ```setAttribute``` 和 ```removeAttribute``` 来修改

```javascript
div.setAttribute('index', 2);
div.setAttribute('class', 'footer'); 
div.removeAttribute('index');
```



## 自定义属性规范

H5 新规范要求，自定义属性的命名方式都要以 ```data-``` 开头，比如：

```html
<div data-index = "1"></div>
<script>
	element.setAttribute("data-time", 23);
</script>
```

H5 新增了一个可以获取到自定义属性集合的方法,  dataset 是一个集合里面存放了所有以data开头的自定义属性：（存在巨大兼容性问题，IE 11以上才支持）

```javascript
console.log(div.dataset.index);
console.log(div.dataset['time']);
```

如果自定义属性里面有多个-链接的单词，我们获取的时候采取驼峰命名法：

```html
<div getTime="20" data-index="2" data-list-name="andy"></div>
<script>
    console.log(div.dataset.listName);
    console.log(div.dataset['listName']);
</script>
```



# 节点操作

网页中的所有内容都是节点(标签、属性、文本、注释等），在DOM中，节点使用node来表示。
HTML DOM树中的所有节点均可通过JavaScript进行访问，所有HTML元素(（节点）均可被修改，也可以
创建或删除。

![image-20200908104156252](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200908104156252.png)

一般地，节点至少拥有nodeType(节点类型)、nodeName(节点名称）和nodeValue(节点值）这三个
基本属性，元素节点操作的在开发中比较多

| nodeType 值 | 类型     |
| ----------- | -------- |
| 1           | 元素节点 |
| 2           | 属性节点 |
| 3           | 文本节点 |



## 层级

**父节点操作**：

```javascript
element.parentNode
```

```html
<body>
    <div class="demo">
        <div class="box">
            <span class="erweima">×</span>
        </div>
    </div>

    <script>
        // 1. 父节点 parentNode
        var erweima = document.querySelector('.erweima');
        // 得到的是离元素最近的父级节点(亲爸爸),如果找不到父节点就返回为 null
        console.log(erweima.parentNode);
    </script>
</body>
```



**子节点操作**：

```javascript
element.childNode  // （标准）这里包含元素节点和文本节点等等，比如回车
```

如果只想要获得里面的元素节点，则需要专门处理，需要循环，判断 nodeType，然后选出 type 是 1 的节点。所以我们一般不提倡使用 childNodes

选择所有子元素：

```javascript
element.children  // （非标准）
```

parentNode.children 是一个只读属性，返回所有的子元素节点。它只返回子元素节点，其余节点不返
回（这个是我们重点掌握的）。虽然children是一个非标准，但是得到了各个浏览器的支持，因此我们可以放心使用，**这个方法返回的是一个 Collection**



**首尾节点**：

这个不管是元素节点，还是文本节点都会获取到

```javascript
element.firstChild
element.lastChild
```

这个是获取第一个元素节点，找不到返回 null，IE 9以上才支持

```javascript
element.firstElementChild
element.lastElementChild
```

因为兼容性问题，一般会用 ```element.children[0]``` 的方式得到第一个节点



**兄弟节点**：

这个就是获取同一父级下的同一层级的兄弟节点，如果没有则返回 null，还是只支持 IE 9以上

```html
<body>
    <div>我是div</div>
    <span>我是span</span>
    <script>
        var div = document.querySelector('div');
        // 1.nextSibling 下一个兄弟节点 包含元素节点或者 文本节点等等
        console.log(div.nextSibling);
        console.log(div.previousSibling);
        // 2. nextElementSibling 得到下一个兄弟元素节点
        console.log(div.nextElementSibling);
        console.log(div.previousElementSibling);
    </script>
</body>
```

为了解决兼容性问题，可以自己封装一个函数：

```javascript
function getNextElementSibling(element) {
    let el = element;
    while (el = el.nextSibling) {
        if (el.nodeType === 1) {
            return el;
        }
    }
    return null;
}
```





## 下拉菜单案例

HTML 和 CSS 的代码就不写了，直接上 JS 代码，学习思路

```javascript
// 1. 获取元素
var nav = document.querySelector('.nav');
var lis = nav.children; // 得到4个小li
// 2.循环注册事件
for (var i = 0; i < lis.length; i++) {
    lis[i].onmouseover = function() {
        this.children[1].style.display = 'block';
    }
    lis[i].onmouseout = function() {
        this.children[1].style.display = 'none';
    }
}

```





## 创建节点

**创建**：

```javascript
document.createElement("tagName")
```

document.createElement() 方法创建由 tagName 指定的HTML元素。因为这些元素原先不存在，是根据我们的需求动态生成的，所以我们也称为动态创建元素节点。



**添加**：

```javascript
element.appendChild(child)  // element 是父级， child 是子级
```

这种添加方式，是一直在最后面添加，还有一种方法可以添加到特定子元素的前面：

```javascript
element.insertBefore(child, 指定元素)
```



## 评论案例

```html
<body>
    <textarea name="" id=""></textarea>
    <button>发布</button>
    <ul>

    </ul>
    <script>
        // 1. 获取元素
        var btn = document.querySelector('button');
        var text = document.querySelector('textarea');
        var ul = document.querySelector('ul');
        // 2. 注册事件
        btn.onclick = function() {
            if (text.value == '') {
                alert('您没有输入内容');
                return false;
            } else {
                // (1) 创建元素
                var li = document.createElement('li');
                // 先有li 才能赋值
                li.innerHTML = text.value;
                // (2) 添加元素
                ul.insertBefore(li, ul.children[0]);
            }
        }
    </script>
</body>
```



## 删除节点

通过父级来删除子节点：

```javascript
node.removeChild(child);  // 获取 child 可以通过 node.children[index] 的方式来获取
```

还可以直接获取到要删除的就节点，直接删除，但是存在一定兼容性问题：

```javascript
child.remove()
```

所以 W3C 给出一个完美的解决办法：

```javascript
child.parentNode.removeChild(child);
```



## 复制节点

复制当前节点，并返回一个新的节点：

```javascript
node.cloneNode()
```

小括号内可以传参数，当为 false 的时候，为浅拷贝，仅拷贝该节点，并不会拷贝该节点内的内容，反之则会把节点内的内容也一并复制过来。



## 动态创建元素区别

```
document.write();
element.innerHTML;
document.createElement();
```

第一个如果在页面加载完之后再调用的话，则会导致页面的重绘，页面内原有的内容会被覆盖掉。比如我有一个 button，点击的时候触发 documen.write()，那么它会让整个页面内容变成新 write 的内容，**这个很少使用！**



第二个和第三个在创建单个，或者少量的节点的时候，没什么区别。前者是靠拼接字符串然后识别 HTML 代码实现，后者直接创建一个节点。但是当创建的很多很多的时候，如果是在循环中拼接字符串的话第二个效率很低，第三个效率很高！第三个的可读性也会更好一点，但是通过稍微复杂一点的方式（数组拼接），可以让第二种 innerHTML 的方式更快：

```javascript
function fn() {
    var inner = document.querySelector('.inner');
    var arr = [];
    for (var i = 0; i < 100; i++) {
        arr.push('<a href="#">百度</a>');
    }
    inner.innerHTML = arr.join('');
}
```



# 事件

之前提到的那些事件，都是很基础的事件添加方式，具有唯一性，如果后面添加了一个同样的事件，则会覆盖掉之前添加的事件，所以有新增的添加事件的方法，其为 W3C 推荐的方式

## 注册事件

添加事件方式：

```javascript
element.addEventListener(type, listener[, useCapture]);
```

该方法具有兼容性问题，IE 9 之前不支持，改可以用。注意上面这个，如果 listener 采用匿名函数的方式添加进去的话，就没办法删除掉这个事件，所以这里一般都会提前声明好这个函数。

- type：事件类型

  > 直接参考这个：https://www.w3schools.com/jsref/dom_obj_event.asp

- listener：事件处理函数，事件发生，调用该函数

```javascript
element.attachEvent();
```

这个办法可以对同一元素同一时间注册多个监听器，会按照注册顺序依次执行



## 删除事件

传统注册删除事件的方式：

```javascript
element.onclick = null;
```

通过添加监听器的方式删除：

```javascript
element.removeEventListener(type, listener[, useCapture]);
```



## 事件流

当事件发生的时候是分为 **捕获阶段** 和 **冒泡阶段** 的，捕获阶段是时间从最顶级往下传播，而冒泡则是从下层往外层传播

捕获阶段：

```html
<body>
    <div class="father">
        <div class="son">son盒子</div>
    </div>
    <script>
        // dom 事件流 三个阶段
        // 1. JS 代码中只能执行捕获或者冒泡其中的一个阶段。
        // 2. onclick 和 attachEvent（ie） 只能得到冒泡阶段。
        // 3. 捕获阶段 如果addEventListener 第三个参数是 true 那么则处于捕获阶段  
        // document -> html -> body -> father -> son
        var son = document.querySelector('.son');
        son.addEventListener('click', function() {
            alert('son');
        }, true);
        var father = document.querySelector('.father');
        father.addEventListener('click', function() {
            alert('father');
        }, true);
    </script>
</body>
```

浏览器弹出对话框顺序先为 father，后为 son，说的通俗一点就是先外后内，事件先让 father 接受到，执行之后在传给下面的 son

冒泡阶段：

```html
<body>
    <div class="father">
        <div class="son">son盒子</div>
    </div>
    <script>
        // 4. 冒泡阶段 如果addEventListener 第三个参数是 false 或者 省略 那么则处于冒泡阶段  
        // son -> father ->body -> html -> document
        var son = document.querySelector('.son');
        son.addEventListener('click', function() {
            alert('son');
        }, false);
        var father = document.querySelector('.father');
        father.addEventListener('click', function() {
            alert('father');
        }, false);
        document.addEventListener('click', function() {
            alert('document');
        })
    </script>
</body>
```

这个的话，就是先 son 先触发，然后向外传播

在实际开发中很少用捕获，更多用冒泡，但是还是有一些事件是没有冒泡的，比如 onblur, onfocus, onmouseenter, onmouseleave



## 事件对象

事件对象是在添加事件的时候，系统自己创建生成的，想要获取到事件对象，给回调函数添加一个形参即可：

```javascript
element.addEventListener('click', function(e) {  // e 为 event 缩写
    console.log(e);
})
```

IE 678 不兼容它，在可以用 ```e = e || window.event``` 来处理

![image-20200910112757793](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200910112757793.png)

1. e.target 是返回触发该事件的对象，而  this 是返回绑定该事件的对象，最简单的例子就是给 ul 添加一个事件，点击 li 也会触发，那么 e.target 返回的是 li，但是 this 返回的是 ul；IE 678 用的是 e.srcElement



## 阻止默认行为

这个可以让点击链接不跳转，点击提交按钮不提交等等

```javascript
var a = document.querySelector('a');
a.addEventListener('click', fn);
function fn(e) {
    e.preventDefault();
}
```



## 阻止冒泡传播

事件冒泡本身的特性，会带来的坏处，也会带来的好处，使用 ```event.preventPropagation()``` 来实现

```html
<body>
    <div class="father">
        <div class="son">son儿子</div>
    </div>
    <script>
        // 常见事件对象的属性和方法
        // 阻止冒泡  dom 推荐的标准 stopPropagation() 
        var son = document.querySelector('.son');
        son.addEventListener('click', function(e) {
            alert('son');
            e.stopPropagation(); // stop 停止  Propagation 传播
            e.cancelBubble = true; // 非标准 cancel 取消 bubble 泡泡
        }, false);
        
        
		// father 这没阻止冒泡，所以点了 father 之后也会传播到 document
        var father = document.querySelector('.father');
        father.addEventListener('click', function() {
            alert('father');
        }, false);
        document.addEventListener('click', function() {
            alert('document');
        })
    </script>
</body>
```



## 事件委托

事件委托实际上就是利用了冒泡机制，比如 ul 下有多个 li，如果给 ul 添加了一个 click 的事件监听，那么当点击任何一个 li 的时候，事件通过冒泡传递到了 ul，触发，然后执行对应函数。通过 event.target 可以获取到引发该事件的 li，这样就可以避免多次循环添加多次事件，减少 DOM 操作次数。

```html
<body>
    <ul>
        <li>知否知否，点我应有弹框在手！</li>
        <li>知否知否，点我应有弹框在手！</li>
        <li>知否知否，点我应有弹框在手！</li>
        <li>知否知否，点我应有弹框在手！</li>
        <li>知否知否，点我应有弹框在手！</li>
    </ul>
    <script>
        // 事件委托的核心原理：给父节点添加侦听器， 利用事件冒泡影响每一个子节点
        var ul = document.querySelector("ul");
        ul.addEventListener("click", function (e) {
            var children = ul.querySelectorAll("li");
            for (let i = 0; i < children.length; i++) {
                children[i].style.backgroundColor = null;
            }
            e.target.style.backgroundColor = "pink";
            alert('知否知否，点我应有弹框在手！');
        });
    </script>
</body>
```



## 常见鼠标事件

![image-20200910134036435](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200910134036435.png)



简单介绍两个有关鼠标的事件，可以禁止在网页右键和选中文字：

```html
<body>
    我是一段不愿意分享的文字
    <script>
        // 1. contextmenu 我们可以禁用右键菜单
        document.addEventListener('contextmenu', function(e) {
                e.preventDefault();
            })
            // 2. 禁止选中文字 selectstart
        document.addEventListener('selectstart', function(e) {
            e.preventDefault();
        })
    </script>
</body>
```



## 鼠标事件对象

鼠标事件对象里，也有很多的属性可以获取到

![image-20200910134738643](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200910134738643.png)

```html
<body>
    <script>
        // 鼠标事件对象 MouseEvent
        document.addEventListener('click', function(e) {
            // 1. client 鼠标在可视区的x和y坐标
            console.log(e.clientX);
            console.log(e.clientY);
            console.log('---------------------');

            // 2. page 鼠标在页面文档的x和y坐标
            console.log(e.pageX);
            console.log(e.pageY);
            console.log('---------------------');

            // 3. screen 鼠标在电脑屏幕的x和y坐标
            console.log(e.screenX);
            console.log(e.screenY);

        })
    </script>
</body>
```



## 键盘事件

某个键盘按键被松开时触发：

```javascript
document.addEventListener('keyup', function() {
    console.log("我弹起了");
})
```

某个键盘按键被按下时触发：

```javascript
document.addEventListener('keydown', function() {
    console.log("我弹起了");
})
```

某个键盘按键被按下时触发：（与上面的区别是，这个不能识别功能键，比如 Ctrl）

```javascript
document.addEventListener('keypress', function() {
    console.log("我弹起了");
})
```



## 键盘事件对象

这个和鼠标事件对象一样，这个对象会包含一些按下的按键的信息，不过这个对象的兼容性很有问题，但是还是可以通过 keycode 来进行补救，有 ASCII 来一一对应

```javascript
document.addEventListener('keypress', function(e) {
    console.log(e.keycode);
})
```

这里要注意一个事：

```keyup``` 和 ```keydown``` 两个事件不区分大小写，a 和 A 的 keycode 都是 65

```keypress``` 会区分大小写，a 是 97，A 是是 65



## 搜索框放大案例

注意事项：这里不能用 ```keydown``` 事件，因为文字进入到文本框是在事件触发之后；同时也不能用 ```keypress``` 因为不识别功能键，比如删除内容

```html
<body>
    <div class="search">
        <div class="con">123</div>
        <input type="text" placeholder="请输入您的快递单号" class="jd">
    </div>
    <script>
        // 快递单号输入内容时， 上面的大号字体盒子（con）显示(这里面的字号更大）
        // 表单检测用户输入： 给表单添加键盘事件
        // 同时把快递单号里面的值（value）获取过来赋值给 con盒子（innerText）做为内容
        // 如果快递单号里面内容为空，则隐藏大号字体盒子(con)盒子
        var con = document.querySelector('.con');
        var jd_input = document.querySelector('.jd');
        jd_input.addEventListener('keyup', function() {
                // console.log('输入内容啦');
                if (this.value == '') {
                    con.style.display = 'none';
                } else {
                    con.style.display = 'block';
                    con.innerText = this.value;
                }
            })
            // 当我们失去焦点，就隐藏这个con盒子
        jd_input.addEventListener('blur', function() {
                con.style.display = 'none';
            })
            // 当我们获得焦点，就显示这个con盒子
        jd_input.addEventListener('focus', function() {
            if (this.value !== '') {
                con.style.display = 'block';
            }
        })
    </script>
</body>
```



# ### BOM ###

BOM ( BrowserObjectModel )即浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心
对象是window。
BOM由一系列相关的对象构成，并且每个对象都提供了很多方法与属性。BOM缺乏标准，JavaScript语法的标准化组织是ECMA，DOM的标准化组织是W3C，BOM最初是Netscape浏览器标准的一部分。

![image-20200912233843715](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200912233843715.png)





# Window 常见事件



## 窗口加载

窗口加载事件是等待一个文档中全部的内容加载完毕之后，才会被触发的，这样就可以把 JS 操作元素的代码写在 HTML 代码之前，或者引用外部 JS 文件，而不用担心所操作的元素还没生成。

通常使用 addEventListener 的方式来添加事件，可以添加多个事件：

```javascript
window.addEventListener("load", function() {});
```



还有一个与之类似的事件：（IE9 以上支持）

```javascript
document.addEventListener("DOMContentLoaded", function() {});
```

这个事件触发是在仅当 DOM 加载完成的时候，不包括 CSS 样式表，图片，flash 等



## 调整窗口大小

当窗口大小发生变化的时候，事件就会触发，通常用于实现响应式布局：

```javascript
 window.addEventListener('resize', function() {});
```

调整的小案例：

```html
<body>
    <script>
        window.addEventListener('load', function() {
            var div = document.querySelector('div');
            window.addEventListener('resize', function() {
                console.log(window.innerWidth);

                console.log('变化了');
                if (window.innerWidth <= 800) {
                    div.style.display = 'none';
                } else {
                    div.style.display = 'block';
                }

            })
        })
    </script>
    <div></div>
</body>
```



# 定时器

## 使用方法

**设置定时器**

定时一段时间之后，执行某一个函数：（延时为毫秒，延迟参数可省略）

```javascript
var timer = window.setTimeOut(function() {}, 200);  // window 可省略
```

通过一个页面有多个定时器的时候，要给他们起好名字



**清除定时器**

参数为要清除的计时器的标识符

```javascript
clearTimeOut(timer)
```



**重复定时器**

```setInterval()``` 是每隔一段时间，就执行回调函数，反复执行：（使用和 setTimeOut 差不多）

```javascript
setInterval(function() {}, 1000);
```



**清除重复定时器**

```
clearInterval(timer)
```

注意，一般要清除的计时器，都会是一个全局变量，记得要声明在添加事件之外



## 倒计时秒杀案例

```html
<body>
    <div>
        <span class="hour"></span>
        <span class="minute"></span>
        <span class="second"></span>
    </div>
    <script>
        // 1. 获取元素 
        var hour = document.querySelector('.hour'); // 小时的黑色盒子
        var minute = document.querySelector('.minute'); // 分钟的黑色盒子
        var second = document.querySelector('.second'); // 秒数的黑色盒子
        var inputTime = +new Date('2019-5-1 18:00:00'); // 返回的是用户输入时间总的毫秒数
        countDown(); // 我们先调用一次这个函数，防止第一次刷新页面有空白 
        // 2. 开启定时器
        setInterval(countDown, 1000);

        function countDown() {
            var nowTime = +new Date(); // 返回的是当前时间总的毫秒数
            var times = (inputTime - nowTime) / 1000; // times是剩余时间总的秒数 
            var h = parseInt(times / 60 / 60 % 24); //时
            h = h < 10 ? '0' + h : h;
            hour.innerHTML = h; // 把剩余的小时给 小时黑色盒子
            var m = parseInt(times / 60 % 60); // 分
            m = m < 10 ? '0' + m : m;
            minute.innerHTML = m;
            var s = parseInt(times % 60); // 当前的秒
            s = s < 10 ? '0' + s : s;
            second.innerHTML = s;
        }
    </script>
</body>
```



# Location对象

统─资源定位符(Uniform Resource Locator,URL)是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。

![image-20200913201911706](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200913201911706.png)

这里面 ```href``` 和 ```search``` 很重要！



它自带有一些方法：

![image-20200913204505512](R:/mdImages/image-20200913204505512.png)



# Navigator 对象

navigator对象包含有关浏览器的信息，它有很多属性，我们最常用的是userAgent，该属性可以返回由客户机发送服务器的user-agent头部的值。



***请注意***，`navigator`的信息可以很容易地被用户修改，所以 JavaScript 读取的值不一定是正确的。



# History对象

window对象给我们提供了一个history对象，与浏览器历史记录进行交互。该对象包含用户（在浏览器窗口中)）访问过的URL。

![image-20200913210930487](R:/mdImages/image-20200913210930487.png)

这个 back 和 forward 很少用，做 OA 企业系统的时候会用到（Office Automation）



# Offset

offset翻译过来就是偏移量,我们使用offset系列相关属性可以动态的得到该元素的位置(偏移)、小等。

1. 获得元素距离带有定位父元素的位置
2. 获得元素自身的大小(宽度高度)
3. 注意:返回的数值都不带单位

![image-20200922175155822](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200922175155822.png)



用 offset 和 style 来获取到元素的宽高的区别：

![image-20200922180141085](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200922180141085.png)





# Client

我们使用client系列的相关属性来获取元素可视区的相关信息，通过client系列的相关属性可以动态的得到该元素的边框大小、元素大小等。

![image-20200929004015053](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200929004015053.png)



# Scroll

scroll 和 client 很相似，client 返回的是设定的高度宽度等，但是 scroll 返回的是实际的高度宽度

![image-20200929175242721](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200929175242721.png)



比如说当一个 box 里的内容太多了，超出盒子本身大小，这时候 scroll 就返回的是实际的大小

![image-20200929175317318](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200929175317318.png)

其中 scrollTop 用的比较多，当把 overflow 设置成了 auto 的时候，超出的部分是可以通过滚动条来调整的；当滚动条拉到最下面的时候，被 scroll 上去的那部分的**元素**的距离，就是 scrollTop

![image-20200929180608326](R:/mdImages/image-20200929180608326.png)

scrollTop 准确的说是**元素本身**从上边框的下沿到滚上去的内容的顶部的距离，他和 window.pageYOffset 不一样，后者是页面被卷上去的距离

scroll 这还有一个 scroll 事件，当滚动条发生滚动就会触发：

```html
<body>
    <div>
        我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 		   我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 		  我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 我是内容 
    </div>
    <script>
        // scroll 系列
        var div = document.querySelector('div');
        console.log(div.scrollHeight);
        console.log(div.clientHeight);
        // scroll滚动事件当我们滚动条发生变化会触发的事件
        div.addEventListener('scroll', function() {
            console.log(div.scrollTop);
        })
    </script>
</body>
```

1. 声明了DTD，使用document.documentElement.scrollTop

2. 未声明DTD，使用document.body.scrollTop

3. 新方法window.pageYoffset和window.pagexoffset，IE9开始支持

（DTD 就是 HTML 页面第一行那个声明）

可以封装一个函数，来解决兼容性问题：

```javascript
function getScroll() {
    return {
        left: window.pageXOffset || document.documentElement.scrollLeft || document.body.scrollLeft || 0,
        top: window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0
    };
}
```



# 以上三点的小节

![image-20200929183349339](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200929183349339.png)

1. offset系列经常用于获得元素位置 offsetLeft offsetTop

2. client经常用于获取元素大小 clientWidth clientHeight
3. scroll经常用于获取滚动距离 scrollTop scrollLeft
4. 注意页面滚动的距离通过 window.pageYoffset 获得



# mouseover 和 mouseenter

mouseover 会发生事件冒泡，比如一个大的父盒子里有一个小的子盒子， 当鼠标移动到父盒子上的时候，事件会被触发；当鼠标移动到子盒子上的时候，因为子盒子没有绑定事件，这个鼠标移动到上面的事件就不会被捕获，该事件会继续冒泡，然后让父盒子捕获到，然后再次触发。

![image-20200929184017038](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200929184017038.png)

比如当鼠标移动到粉色区域的时候，事件会被触发一次；从粉色移动到紫色区域的时候又会触发一次。

而 mouseenter 就不会发生这样的事，事件不会冒泡，通常 mouseleave 和 mouseenter 搭配使用。