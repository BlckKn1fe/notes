---
title: HTML基础笔记
date: 2020-05-26 03:16:36
categories: 
- 前端
tags:
- 前端
- HTML

---

>笔记内容全部来源于：https://www.bilibili.com/video/BV14J4114768

# HTML基础

## 标签的关系

包含关系：

```html
<head>
    <title></title>
</head>
```

并列关系：

```html
<head> </head>
<body> </body>
```



## 基本要素

| 标签名            | 定义     | 说明                           |
| ----------------- | -------- | ------------------------------ |
| \<html\>\</html\>   | HTML标签 | 也叫根标签                     |
| \<head\>\</head\>   | 文档头部 | head 标签中必须要设置title标签 |
| \<title\>\</title\> | 文档标题 | 这就是显示该网站叫什么的       |
| \<body\>\</body\>   | 文档主题 | 基本全部内容都在 body 中       |



## 其他基本要素

文档类型声明标签，其本身不是 HTML 标签，主要告诉浏览器 HTML 版本

```html
<!DOCTYPE html>
```

lang 的话是告诉浏览器页面是什么语种，主要给搜索引擎看的，一般有 en，zh-CN等语种

```html
<html lang="en">
```

charset 是告诉浏览器是哪种字符编码的，其中有一些比较常见的有 GB2312（简体中文），BIG5（繁体中文），GBK（国标包含简体和繁体），UTF-8（万国码）

```html
<meta charset="UTF-8" />
```



## 常见标签

根据标签的语义，在合适的地方给一个最为合理的标签，可以让页面结构清晰明了。



1. 标题标签：一行文字独立显示，且会加粗，和 MarkDown 一个意思

	```html
	<h1></h1>
	<h2></h2>
	...
	<h6></h6>
	```
2. 段落标签：简单来说就是在其中的文字会成为一个段落，起会根据浏览器的实际大小而换行

	```html
	<p></p>
	```


3. 换行标签：强制在特定的位置进行换行，**单标签**

	```html
	<br />
	```


4. 文字格式化标签：

	| 语义   | 标签                               | 说明          |
	| :----- | :--------------------------------- | :------------ |
	| 加粗   | \<strong\>\</strong\> 或者 \<b\>\</b\> | 推荐用 strong |
	| 斜体   | \<em\>\</em\> 或者 \<i\>\</i\>         | 推荐用 em     |
	| 删除线 | \<del\>\</del\> 或者 \<s\>\</s\>       | 推荐用 del    |
	| 下划线 | \<ins\>\</ins\> 或者\ <u>\</u\>       | 推荐用 ins    |


5. \<div\>和 \<span\> 标签都是用来布局的，其中 div 可以看做大盒子，独占一横行；而 span 表示一横行中更多小的盒子，横向跨度，具体细分。

	```html
	<div></div>
	<span></span>
	```


6. 图像标签，**单标签**，url 为路径

	```html
	<img src="url" />
	```

	图片标签中，还有其他的一些属性可以添加

	| 属性   | 属性值   | 说明                       |
	| ------ | -------- | -------------------------- |
	| src    | 图片路径 | 必须属性                   |
	| alt    | 文本     | 当图片不能显示的时候的提示 |
	| title  | 文本     | 鼠标放到图片上，显示的文字 |
	| width  | 像素     | 图片宽度                   |
	| height | 像素     | 图片高度                   |
	| border | 像素     | 图片边框粗细               |


7. 超链接标签：通过点击某个图片或者文本可以跳转到下一个页面，url 可以放内部链接，外部链接，或者一个文件；target 决定了跳转页面的形式，默认是不会跳转到一个新的标签页面，设置成  `_blank` 之后会在点击之后跳转到一个新的页面。当不确定，目标网页还没做好的情况下，可以把 url 写成  `#`  来暂时代替。

	```html
	<a href="url" target="弹出方式"> 文本或图像等 </a>
	```

	跳转功能（锚点链接）：这里其实是给一个标签附上一个 `ID` 属性，之后在 href 的位置按照 `#ID` 的格式定位到这个标签位置，（注意加井号），点击该链接，会直接跳到那个标签的位置。和百度百科中，点击某一个链接，会直接跳转到目标内容功能是一样的。

	```html
	<h1 id="title"> 标题 </h1>
	<h2 id="intro"> 个人简介 </h2> 
	    
	<a href="#title"> 跳转到标题 </a>
	<a href="#intro"> 跳转到个人简介 </a>
	```



8. 特殊符号：太多了... 懒得打了，直接截图

	 ![image-20200527181850978](R:/mdImages/image-20200527181850978.png)

	  

## 表格标签

### 基本标签

表格标签，table 标签标示一整个表格，tr 表示的则为一整行，td 的话就表示一行里一个个小格，每个单元格中基本可以放所有元素。

```html
<table>
    <!-- 第一行 -->
    <tr>
        <td>单元格内的文字</td>
    </tr>
    
    <!-- 第二行 -->
    <tr>
        <td>单元格内的文字</td>
    </tr> 
    
    ...
    ...
    
</table>
```

一个表格中的第一行，一般都是表头，决定了下面几行内容是什么，所以一般第一行不是用 `td` 标签，而是用 `th` 标签（table head），表头单元格，其修饰的文字会加粗并且居中。

### 其他属性

| 属性名       | 属性值              | 描述                                      |
| ------------ | ------------------- | ----------------------------------------- |
| align        | left, center, right | 规定表格相对周围元素的对齐方式            |
| border       | 1 或者 “”           | 规定表格单元是否拥有边框                  |
| cellpadding  | 像素值              | 规定单元边沿与其内容之间的空白，默认为1px |
| cellspacing  | 像素值              | 规定单元格之间的空白，默认为2px           |
| width/height | 像素值或百分比      | 规定表格宽度/高度                         |

### 表头区 / 主体区

表格分为表头和主体两部分，一个是 `thead` 一个是 `tbody` ，这两个表示一大块区域，其中 `thead` 中必须有 `tr` 标签。

```html
<table>
    <!-- thead 包含的都是头部部分 -->
    <thead>
        <tr>
            <th>姓名</th>
            <th>年龄</th>
        </tr>
    </thead>
    <!-- tbody 包含的都是主体部分 -->
    <tbody>
        <tr>
            <td>张三</td>
            <td>23</td>
        </tr>
        
        <tr>
            <td>李四</td>
            <td>24</td>
        </tr>
    </tbody>
</table>
```



### 合并单元格

合并单元格主要分三个步骤：

1. 确定是跨行还是跨列，横跨列，竖跨行
2. 账号到目标单元格，使用合适的合并方式
3. 删除多余单元格

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <table align="center" border="1" cellpadding="10" cellspacing="0" width="500">
        <thead>
            <tr>
                <th colspan="4"> 个人简介 </th>
                <th colspan="2"> 其他 </th>
            </tr>
            
        </thead>

        <tbody>
            <tr>
                <td rowspan="2" width="50">照片：</td>
                <td rowspan="2" width="60"></td>
                <td width="50">爱好：</td>
                <td></td>
                <td width="50"> 证书: </td>
                <td></td>
            </tr>
            <tr>
                <td>性别：</td>
                <td></td>
                <td width="50">荣誉</td>
                <td></td>
            </tr>
        </tbody>
    </table>
</body>
</html>
```

效果图如下：

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200527202428321.png )

## 列表标签

主要有三种不同类型列表：无序列表，有序列表，和自定义列表

### 无序列表（重点）

无序列表中，各个项之间没有熟悉怒之分；`<ul>` 之间只能放 `<li>` 标签，且 `<li>` 相当于一个容器，可以容纳所有元素

```html
<ul>
    <li>项目 1</li>
    <li>项目 2</li>
    <li>项目 3</li>
</ul>
```



### 有序列表（了解）

（和无序基本没差别，只是各项之间是有先后顺序关系的）



### 描述列表

这个列表一般都是在第一层级里用一个概括性的词，用来形容下面几个项的，其标签为 `<dl>` ，全称为 `Description Lists` ，而其下一层的标签 `<dt>` 全称为 `Description Term` ，再下一级为 `<dd>` 全称为 `Definition` 

```html
<dl>
    <dt>联系我们</dt>
    <dd>官方微博</dd>
    <dd>微信公众号</dd>
    <dd>联系电话</dd>
    <dd>在线客服</dd>
</dl>
```





## 表单

### 表单基本信息

一个表单由表单域，表单控件（元素），和提示信息组成；表单主要是用来收集用户信息用的。

```html
<form action="url" method="提交方式" name"表单域名称">
    各种表单元素控件
</form>
```



常用属性：

| 属性   | 属性值     | 作用                                               |
| ------ | ---------- | -------------------------------------------------- |
| action | url 地址   | 用于指定接收并处理表单数据的服务器程序的 url 地址  |
| method | get / post | 用于设置表单数据的提交方式                         |
| name   | 名称       | 用于指定表单的名称，以区别同一个页面中的多个表单域 |

### 表单元素

有 input 输入表单元素，select 下拉表单元素，textarea 表单域元素

#### input

input 即让用户输入信息，input 标签有一个 type 属性，不同的 type 属性会有不同的控件，比如文办字段，复选框，按钮，单选按钮等等，input 是个**单标签**！

```html
<input type="属性值" />
```

input 主要属性：

| 属性值   | 描述                                                       |
| -------- | ---------------------------------------------------------- |
| button   | 定义可点击的按钮（多数情况下，用于 JS 启动脚本）           |
| checkbox | 定义复选框                                                 |
| file     | 定义输入字段和 “浏览” 按钮，供文件上传                     |
| hidden   | 定义隐藏的输入字段                                         |
| image    | 定义图像形式的提交按钮                                     |
| password | 定义密码字段，该字段中的字符被掩码                         |
| radio    | 定义单选按钮                                               |
| reset    | 定义重置按钮，重置按钮会清楚表单中所有数据                 |
| submit   | 定义提交按钮，提交按钮会把表单数据发送到服务器中           |
| text     | 定义单行的输入字段，用户可在其中输入文本，默认宽度20个字符 |

input 其他属性：

| 属性      | 属性值       | 描述                                  |
| --------- | ------------ | ------------------------------------- |
| name      | 由用户自定义 | 定义 input 元素的名称                 |
| value     | 由用户自定义 | 规定 input 元素的值                   |
| checked   | checked      | 规定此 input 元素首次加载时应当被选中 |
| maxlength | 正整数       | 规定输入字段中的字符的最大长度        |

其中 value 主要是给后台人员使用的；name 为表单元素的名字，单选和复选记得要有同样的 name；checked 若被添加的话，当页面被打开的时候会被默认勾选上。

```html
<form>
    
    <!-- text 文本框 用户可以在里面输入任何文字 -->
    用户名：<input type="text" name="username" value="请输入用户名"> <br>
    
    <!-- password 密码框 用户看不见输入的密码 -->
    密码：<input type="password" name="pwd"> <br>

    <!-- radio 单选按钮 实现多选一 -->
    <!-- name 是表单元素名字 如果想实现多选一的话 需要让这几个标签有相同的名字 -->
    <!-- 单选和复选可以设置 checked 属性 使其默认被勾选 -->
    性别：男 <input type="radio" name="sex" value="男"> 
    女 <input type="radio" name="sex" value="女"> <br>
    
    <!-- checkbox 复选框 实现多选 -->
    爱好：吃饭 <input type="checkbox" name="hobby" value="吃饭"> 
    睡觉 <input type="checkbox" name="hobby" value="睡觉"> 
    打豆豆 <input type="checkbox" name="hobby" value="打豆豆" checked="checked"> <br>

    <!-- 提交按钮会把表单域内的信息提交给服务器，reset则把表单重置成默认的样子 -->
    <input type="submit" value="注册"> <input type="reset" value="清空">

    <!-- 普通按钮  后期配合JS使用 -->
    <input type="button" value="获取短信验证码">
    
    
    
</form>
```



#### select

两点要说明，第一个是 `option` 标签记得给 `value` 值，第二个是有一个属性是 `selected`

```html
<select>
    <option>选项1</option>
    <option>选项2</option>
    <option>选项3</option>
    ...
    ...
</select>
```



#### textarea

这个基本就可以理解为一个大一点的文本框，里面就是用来输入文字的

```html
<textarea row="30" col="5">文本内容</textarea>
```

也可以加 ID 和 name 属性，一般 row 和 col 属性在开发中用不到，会用 css 来调整



### \<lable\>标签

```` <lable>``` 标签主要用于绑定一个表单元素，当点击 ```<lable>``` 标签的文本时，也会选择到对应元素上，提高用户体验；lable 里的 ```for``` 要跟一个明确的 ```id```

```html
<lable for="sex">男</lable>
<input type="radio" name="sex" id="sex" />
````
