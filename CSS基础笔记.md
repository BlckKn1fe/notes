---

title: CSS基础笔记
date: 2020-06-28 01:51:40
categories: 
- 前端
tags:
- 前端
- CSS
---

> 笔记内容全部来源于：https://www.bilibili.com/video/BV14J4114768

## 基本语法

CSS主要由两个部分构成：选择器以及一条或多条说明；一般情况，css 会写在 head 标签之间

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200530064603169.png )

```css
<style>
    p {
        color : red;
        font-size: 12px;
    }
</style>

<p>
    测试 CSS
</p>
```



## 代码风格

1. 样式格式书写

   **紧凑格式：**

```css
h3 { color: deeppink; font-size: 20px;}
```

   **展开格式：**（推荐）

```css
h3 { 
    color: deeppink; 
    font-size: 20px;
}
```


2. 大小写：没特殊要求，全小写

3. 空格规范：选择器和大括号之间有一个空格，属性的冒号后有一个空格 （这不就是我个人写空格的强迫症么...)



## 基础选择器

### 标签选择器

这里的标签就是 HTML 里的各种标签名，会对某一种标签内的内容，进行统一修改

### 类选择器

可以单独选择一个或者某几个标签，定义一个类，然后这个类可以去应用到多个标签上；在标签中用 `class` 属性来调用

```css
.类名 {
    属性1: 属性值;
    ...
}
```

代码例子：定义一个 red 类，然后应用到第一个 `<li>` 标签上

```html
.red {
    color: red;
}

<body>
	<ul>
        <li class="red">乐事</li>
        <li>妙脆角</li>
        <li>三只松鼠</li>
    </ul>
</body>
```

一个标签可以指定多个类名，类名需要用多个空格隔开；这里需要注意一点就是，当添加多个类名的时候，可能会出现类与类之间出现修改样式重复的时候，这种情况下会选择最后面那个来代替之前的。

```css
<html>
<head>
    <style>
        .green {
            width: 100px;
            height: 100px;
            font-size: 36px;
            /* 背景颜色 */
            background-color: aquamarine;
        }

        .yellow {
            width: 150px;
            height: 100px;
            background-color: yellow;
        }
    </style>
</head>
<body>
    <div class="green"> 绿色 </div>
    <div class="yellow"> 黄色 </div>
    <div class="green yellow"> 绿色 </div>
</body>
</html>
```

这里的 yello 并没有设置字体大小，当给最后一个 `div` 标签添加类的时候，会出现只改变了 box 的颜色和 box 的大小，并没有修改之前 green 设置好的字体大小。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200530072814944.png )

### ID选择器

这里实用上和类选择器基本一样，只是前面要用 `#` 来选择是哪一个具体的标签要进行修改。ID 选择器则选择某一个 HTML 标签中的 ID 属性，这个 ID 属性是唯一的，不允许被重复使用，全局唯一；ID 选择器的优先级高于类选择器。

### 通配符选择器

特殊情况会用到，后期会讲到



### 小节

| 基础选择器   | 作用                     | 特点                   | 使用情况       |
| ------------ | ------------------------ | ---------------------- | -------------- |
| 标签选择器   | 可以选择出所有相同的标签 | 不能差异化选择         | 较多           |
| 类选择器     | 可以选择一个或多个标签   | 可以根据需求选择       | 非常多         |
| ID 选择器    | 一次只能选择一个标签     | 全局唯一 / 指定        | 一般和 JS 搭配 |
| 通配符选择器 | 选择所有标签             | 选择太多，有部分不需要 | 特殊情况使用   |



## 字体属性

### 字体

一个 font-family 中之所以用多个字体是考虑到兼容性问题，当第一个字体没有的时候，以此往后找，如果都没有则使用系统默认字体。Chrome 默认是微软雅黑。

```css
h2 {
    font-family: 'Time New Roman', Times, serif;
}
```



### 字体大小

使用 font-size 属性可以修改文字大小，Chrome 默认的文字大小是 16px，标题标签比较特殊，需要单独修改字体大小。

```css
p {
    font-size: 20px;
}
```



### 文字样式

**粗细：**

可以通过多种方式修改字体的粗细，可以用 normal，bold，bolder，或者数字来设置，其中 400 和 normal 是等价的；实际开发中更推荐使用数字的方式

```css
.bold {
    font-weight: 600;
}
```



**其他样式：**

```css
p {
    font-style: italic; /* 斜体 */
}
em {
    font-style: normal /* 让斜体不倾斜 */
}
```



### 属性复合写法

在对一个标签中的字体进行多方面的修改的时候，可能会写成下面这个样子：

```css
div {
    font-style: italic;
    font-weight: 800;
    font-size: 16px;
    font-family: "Microsoft Yahei";
}
```

这样写了好多行，这些设置可以用一行写出，遵循 

``` css
font: font-style font-weight font-size/font-height font-family
```



的先后顺序进行填写顺序，必须按照顺序来写；这样的写法里，```font-size``` 和 ```font-family``` 必须要有

```css
div {
    font: italic 800 16px "Microsoft Yahei";
}
````



## 文本属性

### 对齐文本

通过 `text-align` 属性可以修改文本的对齐情况，属性可以改为 left，center，和 right

```css
div {
    text-align: center;
}
```



### 装饰文本

通过 `text-decoration` 属性规定添加到文本的装饰；可以添加下划线，删除线，和上划线等；属性值可以选择 none，underline，overline，和 line-through 等。在这个 none 用的最多，一般是用来取消 `<a>` 标签带来的下划线的

```css
div {
    text-decoration: underline;
}
```



### 文本缩进

通过 `text-indent` 属性来指定文本的第一行进行缩进，通常用在段落标签里。通常，单位用 em 会多一点，因为 em 会根据当前文字大小情况来进行调整。

```css
div {
    text-indent: 10px;
}
```



### 行间距

通过 `line-height` 属性可以调整文字之间的行间距，先强调一下行间距的概念：

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200531073338653.png )



```css
p {
    line-height: 26px;
}
```



## CSS引入方式

1. 内部样式表（嵌入式引用）：之前用到的一直都是内部样式表，`style` 标签理论上可以放在任何地方，但是一般都放在 `head` 标签中，它方便控制单一的 html 页面，代码结构相对清晰。


2. 行内样式表：如果只是想对单一的标签内的内容进行简单的修改的话，可以直接在标签上加一个 `style` ，然后进行修改，并没有体现结构和样式分离：

```html
/* CSS文件里只有样式没有 style 标签 */
<p style="color: red; font-size: 12px"> 行内样式表 </p>
```


3. 外部样式表：简单来说就是单独写一个 CSS 文件，然后进行引用。`rel` 定义了当前文档和被连接文档之间的关系，这里使用为 “stylesheet”，表示这是一个样式表文件；`href` 则为路径。

```css
div {
    color: pink;
}
```

```html
<head>
    <link rel="stylesheet" href="样式01.css">
</head>
<div>
    樱花的颜色
</div>
```



| 样式表     | 优点                 | 缺点                 | 使用情况 | 控制范围     |
| ---------- | -------------------- | -------------------- | -------- | ------------ |
| 行内样式表 | 书写方便，权重高     | 结构样式混写         | 较少     | 控制一个标签 |
| 内部样式表 | 部分结构和样式相分离 | 结构样式没有彻底分离 | 较多     | 控制一个页面 |
| 外部样式表 | 完全实现结构样式分离 | 需要引入             | 最多     | 控制多个页面 |



## emmet语法

开局直接用 ! 生成基本结构或者输入标签名直接 tab 键生成标签，都属于 emmet 语法规范中的。emmet 可以快速生成标签，生成多个相同的标签，有父子级关系的标签，兄弟关系等等，见代码示范：

```html
<!-- 通过 div*3 实现 -->
<div></div>
<div></div>
<div></div>

<!-- 通过 ul>li 实现 -->
<ul>
    <li></li>
</ul>

<!-- 通过 div+p+ul>li 实现 -->
<div></div>
    <p></p>
    <ul>
        <li></li>
    </ul>

<!-- 通过.类名或者#ID的方式实现，默认生成 div，也可以制定标签加 .类名或#ID-->
<div class="nav"></div>
<div id="prospector"></div>
<ul class="deco"></ul>
<p id="two"></p>

<!-- $为自增幅，以下通过 .demo$*5 实现 -->
<div class="demo1"></div>
<div class="demo2"></div>
<div class="demo3"></div>
<div class="demo4"></div>
<div class="demo5"></div>
```

emmet 也可以实现快速生成 CSS 的代码：

```css
.test {
     /* 以下三个值需要输入每个词的首字母即可 */
    text-align: center;
    line-height: 26px;
    text-decoration: underline;
}
```



## CSS复合选择器

### 后代选择器

后代选择器就是准确选定父标签中的子标签，比如 ol 下的 li，和 ul 下的 li，这俩都是 li，如果只是对 li 标签添加样式的话，就都会生效，所以可以使用后代选择器。（不仅可以是儿子关系，只要是后代就行）而且，如果出现好几个同样的父亲辈的，可以通过类选择器来逐一修改。

```css
/* 这里有一点需要注意的，就是，ol后面的所有的li都会变，就算层级不一样也会。
 * 比如 div 下 a 嵌套一个 p，然后再嵌套一个 a，那么如果是用 div a {} 
 * 来给样式的话，那么所有的 a 标签的内容都会发生改变 
 */
ol li {
    color: pink;
}

ol li a {
    color: red;
}

.nav li a {
    color: blue;
}
/* 可以准确选择到具体某一个标签内容 */
<ol>
    <li>我是 ol 的孩子</li>
    <li>我是 ol 的孩子</li>
    <li><a href = "#">我是 ol 的孙子</a></li>
</ol>
/* 通过类选择器 */
<ol class = "nav">
    <li>我是 ol 的孩子</li>
    <li>我是 ol 的孩子</li>
    <li><a href = "#">我是 ol 的孙子</a></li>
</ol>
```



### 子选择器

只能选择最近一级的元素，父级可以是 `.类名` 也可以是标签名

```html
<head>
    <style>
        div > a {
            color: red;
        }
    </style>
</head>

<div>
    <a>我是亲儿子</a>  <!-- 只有这个会被修改 -->
    <P>
        <a href = "#">我是孙子</a>
    </P>
</div>
```



### 并集选择器

这个简单一句话就是，所有类型的选择器都可以用逗号链接在一起

```css
div, .nav p, .edge>a {
    color: red;
}
```



### 伪类选择器

这个是在其他的选择器基础上，做一些效果的，比如点击，鼠标 hover等等，有 link，visited，hover，active等等。值得一提的是，**这个也可以用并集选择器！**

```css
a:hover {
    font-size: 24px;
}
```

这里单独说一下 focus，意思就是对光标所在处的表单进行修饰。

```css
/* 把获得光标的 input 表单选择出来 */
input:focus {
    background-attachment: red;  /* 这个是修改文字框的 */
    background-color: skyblue;  /* 这个是修改背景的 */
    color: pink;  /* 这个是文字的 */
}
```



## 元素选择模式

HTML 元素一般分为 **块元素** 和 **行内元素** 两种类型，块元素独占一行。

### 块元素特点

- 自己独占一行
- 高度，宽度，内边距，外边距都可以控制
- 宽度默认是容器（父级宽度）的100%，父级就是上一级标签
- 是一个容器以及盒子，可以放行内元素或块元素

**特别注意：文字元素中不能加入块元素，比如 p 标签 和 h 标签里不能放 div 标签**



### 行内元素特点

常见行内标签有 \<a\>, \<strong\>, \<em\>, \<i\>, \<del\>, \<ins\> 等等，其中最有代表性的是 \<span\>，有的地方也叫行内元素为内联元素。

- 相邻元素在同一行上，一行可以有多个
- 高，宽直接设置是无效的
- 默认宽度就是它本身内容的宽度
- 行内元素只能容纳文本，或者其他的行内元素

注意：特殊情况 \<a\> 里可以放块级元素，需要给 \<a\> 转换一下块级模式。



### 行内块元素

行内块元素就是，又有块元素特点，又有行内元素的特点，在资料中一般没有对其明确的解释。常见的有 \<image>，\<input>，\<td>等等
- 和其他行内块元素在同一行上，且之间有空隙，一行可以显示多个
- 默认宽度就是内容宽度
- 内外边距，宽度高度都可以设置

### 元素显示模式转换

简单理解就是，让其中一种模式的元素具有另外一种模式的特性，比如想要增加 \<a> 的触发范围


行内元素赋予块元素特性：
```html
<head>
    <style>
        a {
            width: 150px;
            height: 50px;
            display: block;  /* 赋予块元素特性 */
        }
    </style>
</head>

<body>
    <a> href="#"> 增大触发范围 </a>
</body>
```

块元素赋予块元素特性：
```html
<head>
    <style>
        a {
            width: 150px;  /* 会提示删除 */
            height: 50px;
            display: inline;  /* 赋予块元素特性 */
        }
    </style>
</head>

<body>
    <a href="#"> 块元素 -> 行内元素 </a>
</body>
```

还可以改成行内块元素，用 ```inline-block```  关键字，这里就不写了



### 简单的侧边栏案例

这里参考的是小米商城的侧边栏，我觉得还不错，记过来了

```html
<head>
    <style>
        a {
            display: blcok;  /* 先改成块内元素 */
            width: 230px;
            height: 40px;
            background-color: #55585a;
            font-size: 14;
            color: #fff;
            line-height: 40px;  /* 文字垂直居中 */
            text-decoration: none;  /* 去除下划线 */
            text-indent: 2em;
        }

        a:hover {
            background-color: #ff6700;
        }
    </style>
</head>

<body>
    <a href="#"> 电脑 手机 </a>
    <a href="#"> 电视 合适 </a>
    <a href="#"> 笔记本 平板 </a>
    <a href="#"> 出行 穿戴 </a>
</body>

```

### 单行文字垂直居中原理

这个其实利用了盒子模型的特点，见下图就完事了，文字没有图来的直观一点，它就是靠上下空气把文字挤到中间的。如果这个文字行高高于盒子高度，则文字会偏下，反之偏上。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200627172222.png)


## CSS 背景

### 背景颜色
通过 ```background-color``` 来设置背景颜色，提一嘴，可以设置 ```transparent``` 把背景颜色设置成透明色，不是白色，默认不设置的话就是透明


### 背景图片
实际开发中，**长用于 logo，装饰，或者全局背景图，优点是便于控制位置**。（精灵图）通过 ```background-image: url();``` 来设置，默认为 none。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200627173503.png)

默认插入背景图的话，是平铺样式，平铺即铺满盒子模型。这个背景图片还可以修改其显示的样式。通过 ``` background-repeat``` 来修改。可以修改的属性有 reapeat，no-repeat，repeat-x，和 repeat-y，分别代表横纵平铺，不平铺，在横向平铺，在纵向平铺。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200627173820.png)

虽然盒子很大，但是可以规定其平铺样式，沿着 Y 和沿着 X 就不在这里写了。



### 背景图片位置
这个最好的理解方式就是，改变一个图片在一个盒子里的位置，通过 ```background-position``` 来修改，第一个参数是 x 第二个属性 y。可以是方位词，也可以是具体值。

| 参数值   | 说明                                      |
| -------- | ----------------------------------------- |
| length   | 百分数，或者由浮点数字和单位组成的长度值  |
| position | top，center，bottom，left，right 方位名词 |

**注意，这俩可以混合使用。**（不过不管怎么用，一定注意第一个是 X 第二个是 Y 就没那么多麻烦事了）



### 背景附着

这个就是之前自己想知道的一个东西，页面往下滚动的时候可以设置背景图片是跟着滚动而滚动还是不动，通过 ```bakcground-attachment``` 来实现，可以修改两个参数：
```scroll``` 就是滚动；```fixed``` 就是不动



### 背景复合写法

这个就和之前的 ```font``` 那个复合写法一样，在写背景复合属性的时候是没有先后顺序的，但是一般会按照
```background: 背景颜色 图片url 平铺 滚动 位置``` 的顺序来写



### 背景颜色半透明

通过 ```background: rgba(0, 0, 0, 0.3)``` 来设置，最后的 a 是 alpha 的缩写，数值越小透明度越高，反之越低。这个只会修改背景颜色的透明度，盒子内的内容不会受到影响。

### 背景小节

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200627202303.png)



## CSS三大特性

### 层叠性

层叠性其实自己之前实验过，就是对同一个标签进行同样属性的设置，比如这样
```html
<head>
    <style>
        div {
            color: yellow;
            font-size: 16px;
        }
        
        div {
            color: pink;
        }
    </style>
</head>

<body>
    <div>
        测试字体
    </div>
</body>

```

这里，都对 div 进行了修饰，但是只会保留最后一个。只有**冲突**的属性才会采取**就近原则**，而其他的不会。比如这里在第一个选择器中，添加了字体大小，这个字体大小不会消失。


### 继承性

这个个人感觉就是层级问题，越高等级的标签，会影响到下级标签，比如在高层级标签进行了一些文字上的修饰，那么，下面的标签里的文字内容也会被影响
```html
<head>
	<style>
        div {
            color: pink;
            font-size: 16px;
            line-height: 48px;
        }
    </style>
</head>

<body>
    <div>
        <p>
            内层标签内容
        </p>
    </div>
</body>

```

在这里，\<p> 标签的样式，会继承 \<div> 标签的样式，注意！这里只会继承一些文字的样式，比如：text-，font-，line-，以及 color 属性，其他的不会继承。（目前学到的好像是这样，如果有错误回来再改）

行高的继承有点讲究，行高设置成倍数的话，后面继承的文字，就会根据自己的实际文字大小去自动调整行高。
```html
<head>
    <style>
        body {
            font: 12px/1.5 'Microsoft Yahei'  /* 这里这个 1.5 代表的是当前文字大小的 1.5 倍行高 */
        }
        div {
            font-size: 14px;
        }
        p {
            font-siize: 16px;
        }
    </style>
</head>

<body>
    <div>
        div 里的文字  <!-- 由于重新设置了 div 的文字大小，所以只会继承 1.5 倍行高，行高为 21-->
    </div>
    <p>
        p 里的文字  <!-- 这里的行高就是 24 -->
    </p>
</body>
```



### 优先级

先放一段代码
```html
<head>
    <style>
        div {
            color: blue;
        }
        .nav {
            color: pink;
        }
    </style>
</head>

<body>
    <div class = "nav">
        测试文字
    </div>
</body>
```

这里是类选择器要优先于标签选择器，各种选择器之间存在优先级关系，如下表。

| 选择器                   | 权重    |
| ------------------------ | ------- |
| 继承 或者 *              | 0,0,0,0 |
| 元素选择器（标签选择器） | 0,0,0,1 |
| 类选择器 伪类选择器      | 0,0,1,0 |
| ID 选择器                | 0,1,0,0 |
| 行内样式 style=""        | 1,0,0,0 |
| !important               | 无穷大  |

!important 使用方法如下

```css
div {
    color: pink!important;
}
```


优先级的一些注意事项
- 各选择器之间的权重其实是按照一种位比较，从左往右的顺序来比的
- **继承的权重是0**，比如给 body 一个 id，然后对这个 id 进行修饰，其 body 内有一 div 标签，如果单独对 div 进行修饰的话，那么冲突的部分 div 有效，而 body 的无效。
- a 标签有点特殊，其默认的颜色是蓝色，比如 body 设置了 color 是 pink，然后 body 里有一个 a 标签，但是这个 a 标签的样式不随着 body走

### 权重叠加
权重的每一位之间的叠加，是不会进位的。在复合选择器中就会出现权重的叠加，比如
```css
.nav ul li  /* 权重为 0,0,1,2 */
a:hover  /* 权重为 0,0,1,1 */
```



## CSS 盒子模型

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200627231932.png)

这里先简单认识一个盒子模型，border 就是盒子的边框，content 为盒子里的内容，padding 就是文字和盒子边框之间的距离，叫内边距，margin 即为外边距，是控制盒子和盒子之间距离的东西

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200627232150.png)



### 边框

边框的粗细可以通过 ```border-width``` 来设置，样式通过 ```border-style``` 来设置，同时还可以设置边框颜色，通过 ```border-color``` 来设置。

这个 style 可以给很多参数：

| 参数   | 说明                                |
| ------ | ----------------------------------- |
| none   | 和 borer-width 无关                 |
| hidden | 隐藏，IE不支持                      |
| dotted | 点状                                |
| dashed | 虚线                                |
| solid  | 实线                                |
| double | 双线变量，两线距离等于 border-width |
| groove | 根据 border-color 画 3D 凹槽        |
| ridge  | 根据 border-color 画菱形边框        |
| inset  | 根据 border-color 画 3D 凹边        |
| outset | 根据 border-color 画 3D 凸边        |


![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200627233823.png)


对 border-style 给参数的时候：

- 给一个参数代表给四个边一个样式
- 给四个参数代表分别给上-右-下-左边框样式
- 给两个参数代表给上下-左右边框样式
- 给三个参数代表给上-左右-下边框样式


### 边框复合写法

格式：
```border: 5px solid pink```
这个写法还不错，第一个参数是宽度，第二个是样式，第三个是颜色。
border 还可以单独设置单个边框：
```border-top: 10px dashed purple```

### 边框合并
```border-collapse: collaps``` 
比如在用 \<table> 标签的时候，就会出现两个边框重叠在一起，然后显得边框有点粗的情况，这时候就可以用 border-collapse: collapse; 来实现边框的合并。
这里多说两句，table 的很多样式以后就要单独抽取出来写，以下代码来做示范：

```css
/* 设置表格大小 */
table {
    weight: 500px;
    height: 250px;
}
/* 表头宽一点 */
table th {
    height: 30px;
}
/* 设置 table，td 和 th 的边框 */
table,
td, th {
    border: 1px solid black;  /* 1px 宽度的黑色实线边框 */
    border-collapse: collapse; /* 合并边框 */
    font-size: 14px;
    text-align: center;
}
```



### 内边距

这个内边距使用的时候和边框有一个毛病，会改变盒子本身的大小，比如外面加了边框为 20px，然后 div 本身长宽 200，那加完边框就是 240。

**内边距也有这个问题，盒子会改变形状。**

**\*\* 但是没有设置 width/heiht 值的话，是不会改变盒子本身大小的! *\***

其使用方法和 border 基本一样，也分上下左右边距，也可以用复合写法
padding-top，padding-right，padding-bottom 和 padding-left。
**内外边距有巧妙的用法，其可以在只设置高度的情况下，用内边距去撑开一个盒子，这样保证内容之间的距离相等**，下面给一个导航栏案例。

```html
<head>
    <style>
        div {
            height: 41px;  /* 设置盒子高度 */
            border-top: 3px solid coral;  /* 设置上边距，呈现出一个长条 */
            border-bottom: 1px solid #edeef0;  /* 同上 */
            background-color: #fff;  /* 设置整个导航栏颜色 */
            line-height: 41px;  /* 垂直居中 */
        }

        .nav a {
            display: inline-block;  /* 讲起改变成行内块元素，增大点击面积 */
            height: 41px;   /* 设置成盒子高度一样打 */
            font-size: 14px;
            text-decoration: none;
            color: #4c4c4c;
            padding: 0 20px;  /* 核心：变成行内元素快后，用 padding 撑开宽度 */
        }

        .nav a:hover {
            color: #ff8500;
            background-color: #eee;
        }
    </style>
</head>

<body>
    <div clas="nav">
        <a href="#">首页</a>
        <a href="#">移动客户端</a>
        <a href="#">新闻</a>
        <a href="#">朋友圈</a>
    </div>
</body>
```



### 外边距（Margin）

关键词 ```margin``` ，也可以同时设置上下左右，其顺序和内边距，边框什么的，是一样的。

需要注意一点，margin 填写的参数出了具体数值以外，可以用 ```auto``` 标签，auto 可以来实现居中对齐，就是网页内容无论缩小还是放大，内容都会一直居中，**只有块元素有效**。

文字的居中对齐是 ```text-align``` 属性调增，其对**行内元素**和**行内块元素**都起效。



### 嵌套元素塌陷

当两个块元素（兄弟关系）相遇的时候，如果在同一个链接的地方，或者在同一个方向添加 margin 的时候，他们不会合并，他们只会选择较大值的一个，这种现象叫坍塌。针对情况，有三个解决方案：（**暂时解决方案**）

- 给父元素添加边框
- 给父元素添加内边距
- 给父元素添加 ```overflow:hidden```



### 清除内外边距

这个有时候是为了布局而考虑的，有很多标签有自己的内外边距，不同浏览器渲染出来的默认的也会不一样，所以干脆就直接全都清楚掉，统一设置统一管理。

```css
* {
    margin: 0px;
    padding: 0px;
}
```



## 圆角边框

语法：

```css
border-radius: length; /* length 可以写百分比 */
```

实现原理就是圆的半径，去贴合方形的四个角，取圆和方形连在一起的那一部分。**这个数值也可以填四个数值，两个数值，和三个数值**。从左上角开始，其对应顺序和 padding 和 margin 类似。也可以单独设置，通过 ```border-top/bottom-xxx``` 来设置。



## 盒子阴影

当鼠标放在一个区域的时候，产生的边缘的阴影就是盒子阴影效果。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200705011939.png)



语法：

```css
box-shadow: h-shadow v-shadow [blur] [spread] [color] [inset]
```

| 值       | 描述                                    |
| -------- | --------------------------------------- |
| h-shadow | [必须] 水平阴影的位置，允许负值         |
| v-shadow | [必须] 垂直阴影的位置，允许负值         |
| blur     | [可选] blur 管的是阴影的模糊程度        |
| spread   | [可选] spread 管影子的大小              |
| color    | [可选] 阴影颜色 rgba(0, 0, 0, 0.3)      |
| inset    | [可选] 将外部阴影（outset）改为内部阴影 |

**注意：**

- outset 默认，不可以往上写
- 影子不会影响布局



## 文字阴影

语法：

```css
text-shadow: h-shadow v-shadow [blur] [color]
```

参数就不赘述解释了



## 浮动

float 属性用于创建浮动框，将其移动到一边，直到左边缘或右边缘触及包含块或另一个浮动框的边缘。

传统也布局三种方式：

- 普通流（标准流）：按照标签规定好的默认方式排列

  ![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200705023043.png)

- 浮动：最典型的是，可以让多个块级元素在一行内排列显示。

- 定位

实际开发中，一个页面基本都包含了这三种布局方式。

**网页开发第一原则：多个块级元素纵向排列找标准流，多个块级元素横向排列找浮动。**

**网页开发第二准则：先设置盒子大小，之后设置盒子的位置。**



### 使用

语法：

```css
选择器 { float: 属性;}
```

| 属性值 | 描述           |
| ------ | -------------- |
| none   | 不浮动（默认） |
| left   | 向左浮动       |
| right  | 向右浮动       |



### 浮动特性

**一、**脱离标准流（脱标），脱离标准流的控制（浮）移动到指定位置（动），浮动的盒子不再保留原先的位置，标准流元素会占据原来浮动元素的位置。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200705025650.png)

给第一个 div 添加浮动之后，第二个 div 就会窜上来，然后实现第一个 div 浮在第二个 div 上面的效果。



**二、**多个脱标的元素，会按照顶端对齐横向排列，无关盒子大小。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200705030351.png)

（特别提示：多个浮动的元素，会随着浏览器大小调整而自动换行）



**三、**浮动元素具有行内块元素特性，任何元素都可以浮动。如果块级的盒子没有设置宽度，默认宽度和父级一样宽，**但是添加浮动之后**，它的大小会根据内容来决定。浮动的盒子中间是没有缝隙的，紧挨着的。



**主动主要配合标准流的父级搭配使用，就是先给一个大盒子往里装。**



### 注意事项

- 先用标准流的父级元素排列上下位置，之后内部子元素采取浮动排列左右位置
- 一个盒子里如果有多个盒子，若其中一个盒子浮动，则其他的兄弟也应浮动
- 浮动的盒子只会影响浮动盒子后面的标准流，不会影响前面的标准流



### 清除浮动

有时候父级元素的高度如果定死的话，就只能装一定数量的子盒子，实际情况中，一个父级盒子里可能会装很多很多子盒子，是变化的。如果不给父盒子高度，直接让子盒子变浮动的话，就会出现子盒覆盖在父盒子上，而且父盒子高度基本为 0，其下方的其他标准流盒子，也会在浮动的子盒子下方。

 ![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200705150150.png)



语法：

```css
选择器 {clear: 属性值;}
```

| 属性值 | 描述                   |
| ------ | ---------------------- |
| left   | 不允许左侧有浮动元素   |
| right  | 不允许左右侧有浮动元素 |
| both   | 不允许两侧有浮动元素   |

实际工作中，几乎都只用 both 属性，清除浮动的策略，是**闭合浮动**

清除浮动方法：（后三个重点）

#### 额外标签法

也叫隔墙法，是 W3C 推荐的做法

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200705162854.png)

这里我设置了一个大的 box，背景颜色为灰色，同时在浮动元素下面加入一个 footer，然后给个灰色边框，footer 在高度上撑开父级 box 盒子。此时，浮动元素在其上方，不受 box 父级盒子约束，且浮动元素不会撑开 box 父级盒子。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200705163101.png)

给 footer 加入 clear 之后，其浮动元素自动撑开 box 父级盒子。所以这里实现原理应该是这样的：clear 禁止了浮动元素和标准流元素重叠在一起，使 footer 往下挪，不和浮动元素重叠。然后在 footer 下移的过程中，也无形中撑开了盒子。所以这个隔墙法，只需要在最后加入一个空白 div 盒子，给予其 clear 属性，让其不断往下挪动，直到不和浮动元素重叠为止，恰好起到了让父级 box 的高度根据浮动元素数量自动调节的效果。（秒啊

优点：通俗易懂，书写方便

缺点：添加许多无意义标签，结构化较差

注意：要求这个新的空标签，必须是块级元素（因为要整个横向隔开）



#### 父级添加 overflow 属性

这个用法简单，直接在父级元素块里添加 overflow 就可以，但是我感觉这个不太行，如果一个 box 里放了浮动元素之后，若还想在这个 box 里放其他的块级元素，这块级元素就不会继续向下移动，而是保持位置不变。

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200705164448.png)

它只是单纯的撑开了 box 的大小，但是这个 footer 没动。后面会单独讲 overflow，暂时只做了解。

优点：书写简单

缺点：溢出隐藏



#### 父级添加 after 伪元素

这个最终的实现效果，感觉和 overflow 是一样的，只是单纯的撑开了父级的 box，但是后面的块级元素并没有往下挪动。使用方法就是，哪一个父级元素需要清除浮动，就直接在 class 里加 clearfix 就可以了

```css
.clearfix:after {
    content: "";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}

.clearfix {  /* IE 6、7 专有*/
    *zoom: 1;
}
```

优点：没有增加标签，结构更简单，使用直接复制粘贴过来即可

缺点：照顾低版本浏览器、

代表网站：百度，淘宝，网易



#### 父级添加双伪元素

```css
.clearfix:before, .clearfix:after {
    content: "";
    display: table;
}

.clearfix:after {
    clear: both;
}

.clearfix {  /* IE 6、7 专有*/
    *zoom: 1;
}
```

优点：代码更简洁

缺点：照顾低版本浏览器

代表网站：小米，腾讯等



## PS切图

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200705171509.png)

1. 图层切图：要么单选，要么多选，然后邮件导出
2. 切图：左面有个工具，切哪一块导出哪一块
3. 利用工具：第三方 cutterman 工具



## CSS属性书写顺序

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200705174740.png)



示例：

![image-20200705175119363](R:/mdImages/image-20200705175119363.png)



## 学成在线案例

先确定页面的布局结构，确定版心，分块来慢慢搭建，该案例只会总结一些注意事



### Header

这个位置，一开始选择在版心盒子中（不给高度），装四个 box，然后再做位置的调整，但是这样实际操作起来，在水平对齐这一块上就不好做。所以最后选择一个固定高度的盒子，把他们再装进去。

另外在做圆角头像这，可以先把一个 div 通过 border-radius 变成圆形，然后修改背景图片就行了，也方便进行对齐等操作。



### Banner

实际开发中，不会直接使用 a 标签来做链接，而是通过 li 包含链接 (li+a) 的做法，li+a 的语义更清晰，是有条理的列表型内容。如果直接用 a，搜索引擎容易辨别为由堆砌关键字的嫌疑，从而影响网站排名

另外在这多插一嘴，凡是涉及到布局的，优先调整外层盒子的属性，能把一个盒子内的内容做细分就尽可能细分，这样在整体的位置摆放和对单一内容的调整就会灵活的多。



### 主体

主体这块，最大的感受就是，慢慢来，分块来，绝对没问题。涉及到浮动的内容，感觉还是挺好用的。



## 定位

定位可以让盒子自由的在某个盒子里移动位置或者固定在屏幕中的某个位置，并且可以压住其他盒子。一些标准流或浮动无法快速实现，此时就会需要定位。



### 定位组成

定位由 ```定位模式 + 边偏移``` 组成

定位模式：指定元素在文档中红的定位方式，通过 ```position``` 属性来设置

| 值       | 语义     |
| -------- | -------- |
| static   | 静态定位 |
| relative | 相对定位 |
| absolute | 绝对定位 |
| fixed    | 固定定位 |



边偏移：决定了元素的最终位置，有 top，bottom，left 和 right 四个属性



### 定位模式

**静态定位 static（了解）**

语法：

```css
选择器 { position: static; }
```

静态定位**按照标准流**特性摆放位置，没有边偏移，静态定位在布局时很少用到



**相对定位 relative（重要）**

语法：

```css
选择器 { position: relative; }
```



相对定位是元素在**移动的时候**，是**相对它原来的定位**来说的。有相对定位的盒子，移动之后，其最开始位置的盒子空间保留，其他的盒子不会挪过去，一句话就是不脱标，其典型应用是给绝对定位当爹。



**绝对定位 absolute（重要）**

当不存在父级或者父级没有定位的话，则其以浏览器为准定位。（Document 文档）

当有多个父级的时候，以最近的有定位的父级为准。当一个盒子设定为绝对定位的时候，会脱离标准流，可以在父级盒子里浮动，且不会干扰到其他的盒子，会让出其原先的位置。



**固定定位 fixed（重要）**

固定定位是元素固定于浏览器可视区域位置的，说简单点就是滚动页面它也不会跟着动。固定定位的元素，不会占有原先的位置，就也和浮动差不多，脱标了。

固定定位有一个小的使用技巧，就是它可以让某一个元素提着版心对齐，比如 “回到顶部” 这样的的按钮，其实现方式为，往左或者往右走网页一半的距离，可以用 left / right 属性，然后可以在用 margin-left / right 来走版心的一半。



**粘性定位 sticky（了解）**

这个定位模式挺好玩的，就是 relative 和 fixed 的两个混合，其本身会占有一开始自己所在的位置，同时随着滚动条的移动，也会固定在一个地方，有点像一些固定在顶部的导航栏这样的效果，想让粘性定位有效果，必须给至少一个边偏移。

但是其兼容性不好，现在很多网站这样的效果是通过 JS 来实现的。



### 叠放次序

在使用定位布局的时候，会出现盒子重叠的情况，这时候想自由设定的话，可以用 ```z-index``` 来设置，就是说白了就有点像设置图层，谁在上面谁在下面的道理一样。

数值越大的，在上面显示的优先级越高。如果属性都一样的话，越在后面的越在前面。



### 绝对定位/固定定位居中

这个和版心的思路差不多，就是先 left / right 给个 50%，然后多出的部分在用用 margin 来减回去，就能实现水平或者垂直居中了。



### 定位特性

行内块元素添加 absolute 或者 fixed，可以直接设置高度和宽度；

块级元素添加 absolute 或者 fixed，如果不给宽度和高度，默认大小和内容的大小一致；

浮动元素，绝对定位的元素都不会触发外边距合并问题。



浮动定位会压住下面的标准流的盒子，但是不会压住盒子内的文字或者图片；

绝对 / 固定定位则会压住下面所有的标准流的内容。

浮动之所以不会压住文字，是因为浮动最开始想做文字环绕效果的。



### 小节

![image-20200719104123240](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200719104123240.png)



![image-20200719113732430](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200719113732430.png)





## 元素显示与隐藏

### display

之前提到的 ```display``` 属性就是来控制元素的显示和隐藏的，其不仅仅可以改变元素的显示模式。当属性为 none 的时候，即隐藏对象；之前学到的 ```display: block`` 不仅仅是转换为块级元素，同时也有显示元素的含义。

**display 隐藏元素后，不再占有原来的位置**



### visibility

visibility 也是可以设置一个元素可见还是隐藏的，与 display 不同的一点是，其隐藏之后，会继续占有原来的位置。属性可以设置 visible 或者 hidden。



### overflow

overflow 规定了如果内容溢出所在盒子的时候，应该怎么处理。

| 属性    | 描述                                 |
| ------- | ------------------------------------ |
| visible | 默认属性，没什么效果                 |
| hidden  | 隐藏超出部分                         |
| scroll  | 不管超出还是没超出，都会加一个滚动条 |
| auto    | 超出则显示滚动条，不超出则不显示     |

如果有定位的盒子，要慎重使用 overflow: hidden 因为它会隐藏多余的部分



## 精灵图

一个网页中往往会应用很多小的背景图像作为修饰,当网页中的图像过多时,服务器就会频繁地接收和发送请求图片,造成服务器请求压力过大,这将大大降低页面的加载速度。

因此,为了有效地减少服务器接收和发送请求的次数,提高页面的加载速度,出现了CSS精灵技术(也称
CSS Sprites、CSS 雪碧)。

核心原理：将网页中的一些小背景图像整合到一-张大图中, 这样服务器只需要一次请求就可以了

其实现最主要依靠 ```background-position``` 来实现。先测量好自己想要的图片的大小，然后创建一个和图片大小一致的盒子，并且添加大的精灵图背景图片。通过移动图片的位置让这个盒子刚好能显示到背景图片特定的额小图标处。

注意一点：向左和向上移动，其值都为负数，相反为正数。



## 字体图标

字体图标使用场景:主要用于显示网页中通用、常用的一些小图标。

精灵图是有诸多优点的,但是缺点很明显。

1.图片文件还是比较大的。

2.图片本身放大和缩小会失真.

3.一旦图片制作完毕想要更换非常复杂。

此时,有一种技术的出现很好的解决了以上问题,就是**字体图标iconfont**。字体图标可以为前端工程师提供一种方便高效的图标使用方式， **展示的是图标,本质属于字体。**

**轻量级**： 一个图标字体要比一系列的图像要小。一旦字体加载了，标就会马上渲染出来,减少了服务器请求

**灵活性**：本质其实是文字,可以很随意的改变颜色、产生阴影、透明效果、旋转等

**兼容性**：几乎支持所有的浏览器,请放心使用

**遇到结构和样式简单的小图标，用字体图标合适一点，反之就用精灵图。**



推荐网站：

icomoon字库：https://icomoon.io/

阿里 iconfont 字库：https://www.iconfont.cn/



### 引用

1. 把下载包里的 fonts 文件夹放入页面根目录下

   **不同浏览器所支持的字体格式是不一样的**,字体图标之所以兼容,就是因为包含了主流浏览器支持的字体文件。

   TureType(.tt)格式 ttf 字体是Windows和Mac的最常见的字体,支持这种字体的浏览器有IE9+、Firefox3.5+、
   Chrome4+、Safari3+、 Opera10+、 iOs Mobile、Safari4.2+ ;

   Web Open Font Format(.woff) 格式 woff 字体,支持这种字体的浏览器有IE9+、Firefox3.5+、 Chrome6+.
   Safari3.6+、Opera11.1+ ;

   Embedded Open Type(.eot) 格式 eot 字体是 IE 专用字体,支持这种字体的浏览器有 IE4+ ;

   SVG(.svg)格式 .svg 字体是基于 SVG 字体渲染的一种格式,支持这种字体的浏览器有Chrome4+、Safari3.1+、Opera10.0+、iOS Mobile Safari3.2+ 

   

2. 进行 CSS 字体声明，这在下载的包里有 style.css 文件，把里面的一部分内容复制粘贴到 HTML 的 style 标签里即可。

  

3. 复制对应的图标的小框，然后对其 font-family 属性进行修改，改成 CSS 字体声明里的那个



### 追加

以 icomoon 举例，它每次下载之后，会跟着一个 selection.json 的文件，可以先在网页 import 这个 json 文件，然后获取到之前选择的图标，再此基础上进行更多的选择，然后下载，在去替换掉之前的字体图标文件。



## 三角形做法

先弄一个盒子出来，然后设置宽高都是 0，然后给一个宽度大一点的就可以了。每一个边框对应一个三角形，想做一个三角形的时候只需要把其他三个边颜色变成 transparent 即可。

```css
div {
    width: 0;
    height: 0;
    line-height: 0;
    font-size: 0;
    border: 50px solid transparent;
    border-left-color: pink;
}
```

然后通过定位的方式，把三角移动到自己想要的位置。



## 鼠标样式

当时鼠标在一些特定的地方的时候，会显示不一样的鼠标样式，通过 ```cursor``` 属性来进行修改

| 属性值      | 描述         |
| ----------- | ------------ |
| default     | 默认         |
| pointer     | 小手         |
| move        | 移动（十字） |
| text        | 文本         |
| not-allowed | 进制         |



## 移除文字省略

想实现这个效果，需要满足三个条件

```css
{
    /* 先强制在一行内显示文本 */
    white-space: nowrap;
    /* 超出的部分隐藏 */
    overflow: hidden;
    /* 文字用省略号代替超出的部分 */
    text-overflow: ellipsis;
}
```

还可以实现多行文本的溢出显示省略号可以实现，就是第一行正常换行，到第二行写不下的时候才会用省略号代替。其有一兼容性问题，使用与 WebKit 浏览器以及移动端

```css
{
    /* 超出的部分隐藏 */
    overflow: hidden;
    /* 文字用省略号代替超出的部分 */
    text-overflow: ellipsis;
    /* 弹性伸缩盒子模型显示 */
    display: -webkit-box;
    /* 显示在一个块级元素显示的文本行数 */
    -webkit-line-clamp: 2;
    /* 设置或检索伸缩盒子对象的子元素的排列方式 */
    -webkit-box-orient: vertical;
}
```

**更推荐让后台人员来做这个效果,因为后台人员可以设置显示多少个字，操作更简单。**



## 常见布局技巧

### Margin负值

当用 ```ul>li``` 创建横排的栏目的时候，添加浮动他们会有边框贴在一起然后出现一个粗边框的情况，难看，可以利用 margin 负值的技巧来让两个一样像素大小的边框重合在一起，实现了 1 + 1 = 1 的效果。

```html
<style>
    li {
        float: left;
        list-style: none;
        width: 150px;
        height: 200px;
        border: 1px solid skyblue;
        margin-left: -1px;
    }
</style>

<body>
    <ul>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</body>
```

它的实现原理：浏览器渲染每一个 HTML 结构的时候，是选中一个结构，然后去找 CSS 的样式。第一个 li 标签浮动之后，确实也往左移动了 1px，而第二个 li 过来的时候，是先浮动，和第一个 li 贴上，出现一个 2px 的边框之后，再向左移动 1px。

但是这样会有一个缺点，即当前盒子的左边框会一直压在左面盒子的右边框上，如果有一些鼠标 hover 时的边框效果的时候，就会显示不出来。

![image-20200721105530045](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200721105530045.png)

这种时候就需要提高盒子层级，鼠标经过某个盒子的时候提高当前盒子的层级，如果盒子本身没有有定位，则加相对定位（保留位置）， 如果有定位,则加 z- index。**相对定位会压住标准流和浮动的元素。**



### 文字环绕

浮动最开始的设计就是为了问题环绕而存在的，其实现过程很简单也很实用，比如想做出下面图中的效果，就可以使用浮动

![image-20200721110553398](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200721110553398.png)

一个大盒子里，左面用一个盒子装好图片然后给个浮动属性，它就会贴过去，右面的文字就会自动实现环绕，超出部分会换行显示。



### 三角形巧用

三角形不仅仅可以产生等腰的三角形，还可以让三角形拉长，变成一个自己需要的样子。

![image-20200721111858504](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200721111858504.png)

这里就是利用边框，砍掉两个不要的三角形，然后另外一个不要的变成透明色，就可以实现

```css
{
    width: 0px;
    height; 0px;
    /* 只保留右边的框有颜色 */
    border-color: transparent red transparent transparent;
    border-style: solod;
    border-width: 100px 50px 0 0;
}
```



### CSS 初始化

不同浏览器对有些标签的默认值是不同的,为了消除不同浏览器对HTML文本呈现的差异,照顾浏览器的兼容，我们需要对CSS初始化
简单理解: CSS初始化是指重设浏览器的样式。 (也称为CSS reset )每个网页都必须首先进行CSS初始化，这里我们以东css初始化代码为例。

```css
/* 把我们所有标签的内外边距清零 */
* {
    margin: 0;
    padding: 0
}
/* em 和 i 斜体的文字不倾斜 */
em,
i {
    font-style: normal
}
/* 去掉li 的小圆点 */
li {
    list-style: none
}

img {
    /* border 0 照顾低版本浏览器 如果 图片外面包含了链接会有边框的问题 */
    border: 0;
    /* 取消图片底侧有空白缝隙的问题 */
    vertical-align: middle
}

button {
    /* 当我们鼠标经过button 按钮的时候，鼠标变成小手 */
    cursor: pointer
}

a {
    color: #666;
    text-decoration: none
}

a:hover {
    color: #c81623
}

button,
input {
    /* "\5B8B\4F53" 就是宋体的意思 这样浏览器兼容性比较好 */
    font-family: Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif
}

body {
    /* CSS3 抗锯齿形 让文字显示的更加清晰 */
    -webkit-font-smoothing: antialiased;
    background-color: #fff;
    font: 12px/1.5 Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;
    color: #666
}

.hide,
.none {
    display: none
}

/* 清除浮动 */
.clearfix:after {
    visibility: hidden;
    clear: both;
    display: block;
    content: ".";
    height: 0
}

.clearfix {
    *zoom: 1
}
```

