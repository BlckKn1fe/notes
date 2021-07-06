---
title: BootStrap快速入门
date: 2020-11-01 19:55:23
categories: 
- 前端
tags: 
- Java
- CSS
- 框架
---



Bootstrap 是美国 Twitter 公司的设计师 Mark Otto 和 Jacob Thornton合作基于 HTML、CSS、JavaScript 开发的简洁、直观、强悍的前端开发框架，使得 Web 开发更加快捷。



# 模版

直接使用以下模板来使用 BootStrap 框架：

```HTML
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- Bootstrap -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" rel="stylesheet">

    <!-- HTML5 shim 和 Respond.js 是为了让 IE8 支持 HTML5 元素和媒体查询（media queries）功能 -->
    <!-- 警告：通过 file:// 协议（就是直接将 html 页面拖拽到浏览器中）访问页面时 Respond.js 不起作用 -->
    <!--[if lt IE 9]>
      <script src="https://cdn.jsdelivr.net/npm/html5shiv@3.7.3/dist/html5shiv.min.js"></script>
      <script src="https://cdn.jsdelivr.net/npm/respond.js@1.4.2/dest/respond.min.js"></script>
    <![endif]-->
          <!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
    <script src="https://cdn.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"></script>
    <!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/js/bootstrap.min.js"></script>
      
  </head>
  <body>
      <h1>
          Hello World!
      </h1>
  </body>
</html>
```

（以上通过使用 CDN 的方式来在线获取到相对应的 CSS 文件和 JS 文件，也可以通过下载的方式在本地引用）



# 棚格系统（Grid System)

Grid System 把屏幕或者叫视口（viewport）最多分成了 12 列，我们可以手动的规定一个元素占这 12 份的多少；通过 Grid System 可以快速的实现页面的布局。**首先，Grid System 是必须在容器当中的**：

```html
<header class="container-fluid">
	<!-- Grid System Content -->
</header>
```

其中 ```container-fluid``` 代表沾满整个宽度（100% 宽度），而 ```container``` 是一个固定的宽度，在两边会留白。

通过 ```row``` 这个预定义类来表示 “行” 的概念，这一行被分为最多 12 份，超出的部分会到下一行。起内容所占份数，是我们自己来设定，比如我们想三等分的布局内容，则要按以下方式：

```html
<header class="container-fluid">
	<div class="row">
      <div class="col-md-4">.col-md-1</div>
      <div class="col-md-4">.col-md-1</div>
      <div class="col-md-4">.col-md-1</div>
    </div>
</header>
```

其中 ```col-**``` 的内容，表示其适配的设备像素大小，比如按照 ```col-md-3``` 去设置 4 张图片，其在电脑端显示的内容就会是水平的，但是在手机端就会变成纵向排列。如果想让其在手机端按照其他方式进行排列，则需要添加更多的预定义类，比如以下代码，表示的是在电脑端和平板端，按照四等分在一行内显示，而在手机端按照二等分，分成两行去显示：

```HTML
<div class="container">
    <div class="row">
        <div class="col-xs-6 col-sm-3"></div>
        <div class="col-xs-6 col-sm-3"></div>
        <div class="col-xs-6 col-sm-3"></div>
        <div class="col-xs-6 col-sm-3"></div>
    </div>
</div>
```

值得一说的是，Grid System 的前缀是向上兼容，但是向下不兼容的，比 ```col-xs-3``` 在任何设备上，都会将内容均匀四等分的显示。

下面是文档中的一说明表：



![image-20201101202314080](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20201101202314080.png)



这里附上一个可以在线生成 Grid 的一个网站：

> https://www.layoutit.com/build

这个网站简直就是一键式布局... 傻瓜又高效



# 使用

BootStrap 提供了很多已经写好的 CSS 样式、组件和插件，我们只需要写上对应的类名就可以实现其展示的效果；其每一个样式在官网文档都有展示，可以对照样例进行修改，学习。



**下面以按钮 Button 来举例说明怎么使用 BootStrap**



为 `<a>`、`<button>` 或 `<input>` 元素添加按钮类（button class）即可使用 Bootstrap 提供的样式，注意一点，尽可能使用 ```button``` 来实现，其兼容性和跨浏览器的能力更强。

```html
<a class="btn btn-default" href="#" role="button">Link</a>
<button class="btn btn-default" type="submit">Button</button>
<input class="btn btn-default" type="button" value="Input">
<input class="btn btn-default" type="submit" value="Submit">
```

这些都是 default 样式，后面的 ```btn-*``` 是可以修改并且修改一些样式的：

```html
<!-- Standard button -->
<button type="button" class="btn btn-default">（默认样式）Default</button>

<!-- Provides extra visual weight and identifies the primary action in a set of buttons -->
<button type="button" class="btn btn-primary">（首选项）Primary</button>

<!-- Indicates a successful or positive action -->
<button type="button" class="btn btn-success">（成功）Success</button>

<!-- Contextual button for informational alert messages -->
<button type="button" class="btn btn-info">（一般信息）Info</button>

<!-- Indicates caution should be taken with this action -->
<button type="button" class="btn btn-warning">（警告）Warning</button>

<!-- Indicates a dangerous or potentially negative action -->
<button type="button" class="btn btn-danger">（危险）Danger</button>

<!-- Deemphasize a button by making it look like a link while maintaining button behavior -->
<button type="button" class="btn btn-link">（链接）Link</button>
```

这些按钮还可以设置**激活状态，禁用状态等等**，可以在官网上随用随查。



# 遇到的问题

这里记录下在使用 BootStrap 中遇到的一些问题，以及解决方法。

## 版心

如果想使用自己定义的版心，则不可以使用 fixed width，否则其自动响应的功能就会失效！

```css
.w {
    width: 90%;  /* 这样写是可以的 */
}
```

