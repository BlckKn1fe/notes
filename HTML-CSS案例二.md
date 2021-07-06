---
title: HTML/CSS案例二
date: 2020-07-07 02:01:25
categories: 
- 前端
tags:
- 前端
- HTML
- CSS
---

这里简单的模仿了学成网，代码有点长，先放 HTML 代码，分块放

## Header

```html
<!-- 头部导航栏 -->
<div class="header w">
    <!-- 导航栏中的log -->
    <div class="logo">
        <img src="imgs/logo.png" alt="">
    </div>
    
    <!-- 导航 -->
    <div class="nav">
        <ul>
            <li><a href="#">首页</a></li>
            <li><a href="#">课程</a></li>
            <li><a href="#">职业规划</a></li>
        </ul>
    </div>
    
    <!-- 搜索栏 -->
    <div class="search">
        <input type="text" value="输入关键词">
        <button></button>
    </div>
    
    <!-- 用户 -->
    <div class="user">
        <div class="avatar">
        </div>
        BlackKnife
    </div>
</div>
```



## Banner

```html
<!-- Banner -->
<div class="banner">

    <div class="w">
        <!-- 副导航栏 -->
        <div class="subnav">
            <ul>
                <li><a href="">前端开发<span>></span></a></li>
                <li><a href="">前端开发<span>></span></a></li>
                <li><a href="">前端开发<span>></span></a></li>
                <li><a href="">前端开发<span>></span></a></li>
                <li><a href="">前端开发<span>></span></a></li>
                <li><a href="">前端开发<span>></span></a></li>
                <li><a href="">前端开发<span>></span></a></li>
                <li><a href="">前端开发<span>></span></a></li>
                <li><a href="">前端开发<span>></span></a></li>
            </ul>
        </div>
        
        <!-- 课程表 -->
        <div class="course">
            <h2>我的课程</h2>
            <!-- 课程表 -->
            <div class="bd">
                <ul>
                    <li>
                        <h4>继续学习 程序语言设计</h4>
                        <p>正在学习-使用对象</p>
                    </li>
                    <li>
                        <h4>继续学习 程序语言设计</h4>
                        <p>正在学习-使用对象</p>
                    </li>
                    <li>
                        <h4>继续学习 程序语言设计</h4>
                        <p>正在学习-使用对象</p>
                    </li>
                </ul>
                <a href="#" class="more">全部课程</a>
            </div>
        </div>
    </div>
</div>
```



## 推荐

```html
<!-- 精品推荐 -->
<div class="goods w">
    <h4>精品推荐</h4>
    <ul>
        <li><a href="#">JQuery</a></li>
        <li><a href="#">Spark</a></li>
        <li><a href="#">Java</a></li>
        <li><a href="#">MySQL</a></li>
        <li><a href="#">JavaScript</a></li>
        <li><a href="#">Python</a></li>
    </ul>
    <a href="" class="mod">修改兴趣</a>
</div>
```



## Body

```html
<!-- 视频内容 -->
<div class="box w">
    <div class="box-hd w">
        <h3>精品推荐</h3>
        <a href="#">查看全部</a>
    </div>

    <!-- 核心板块 -->
    <div class="box-bd">
        <ul class="clearfix">
            <li>
                <img src="imgs/cover.png" alt="">
                <h4>Android Hybrid APP 开发实战 H5+原生</h4>
                <p><span>高级</span> · 1125人在学习</p>
            </li>
            <li>
                <img src="imgs/cover.png" alt="">
                <h4>Android Hybrid APP 开发实战 H5+原生</h4>
                <p><span>高级</span> · 1125人在学习</p>
            </li>
            <li>
                <img src="imgs/cover.png" alt="">
                <h4>Android Hybrid APP 开发实战 H5+原生</h4>
                <p><span>高级</span> · 1125人在学习</p>
            </li>
            <li>
                <img src="imgs/cover.png" alt="">
                <h4>Android Hybrid APP 开发实战 H5+原生</h4>
                <p><span>高级</span> · 1125人在学习</p>
            </li>
            <li>
                <img src="imgs/cover.png" alt="">
                <h4>Android Hybrid APP 开发实战 H5+原生</h4>
                <p><span>高级</span> · 1125人在学习</p>
            </li>
            <li>
                <img src="imgs/cover.png" alt="">
                <h4>Android Hybrid APP 开发实战 H5+原生</h4>
                <p><span>高级</span> · 1125人在学习</p>
            </li>
            <li>
                <img src="imgs/cover.png" alt="">
                <h4>Android Hybrid APP 开发实战 H5+原生</h4>
                <p><span>高级</span> · 1125人在学习</p>
            </li>
            <li>
                <img src="imgs/cover.png" alt="">
                <h4>Android Hybrid APP 开发实战 H5+原生</h4>
                <p><span>高级</span> · 1125人在学习</p>
            </li>
            <li>
                <img src="imgs/cover.png" alt="">
                <h4>Android Hybrid APP 开发实战 H5+原生</h4>
                <p><span>高级</span> · 1125人在学习</p>
            </li>
            <li>
                <img src="imgs/cover.png" alt="">
                <h4>Android Hybrid APP 开发实战 H5+原生</h4>
                <p><span>高级</span> · 1125人在学习</p>
            </li>
        </ul>
    </div>
</div>
```



## Footer

```html
<!-- footer -->
<div class="footer">
    <div class="w">
        <!-- 左半部部分 -->
        <div class="left">
            <div class="logo"><img src="imgs/logo.png" alt=""></div>
            <div class="copyright">学成在线致力于普及中国最好的教育它与中国一流大学和机构合作提供在线课程。<br>
                © 2017年XTCG Inc.保留所有权利。-沪ICP备15025210号</div>
            <a href="#" class="app">下载APP</a>
        </div>
        
		<!-- 右半部部分 -->
        <div class="right">
            <dl>
                <dt>合作伙伴</dt>
                <dd><a href="#">合作机构</a></dd>
                <dd><a href="#">合作导师</a></dd>
            </dl>

            <dl>
                <dt>新手指南</dd>
                <dd><a href="#">如何注册</a></dd>
                <dd><a href="#">如何选课</a></dd>
                <dd><a href="#">如何拿到毕业证</a></dd>
                <dd><a href="#">学分是什么</a></dd>
                <dd><a href="#">考试未通过怎么办</a></dd>
            </dl>

            <dl>
                <dt><a>关于学成网</dt>
                <dd><a href="#">关于</a></dd>
                <dd><a href="#">管理团队</a></dd>
                <dd><a href="#">工作机会</a></dd>
                <dd><a href="#">客户服务</a></dd>
                <dd><a href="#">帮助</a></dd>
            </dl>
        </div>
    </div>
</div>
```

## CSS

这特别长...

```css
* {
    margin: 0;
    padding: 0;
}

a {
    text-decoration: none;
}

body {
    background-color: #f3f5f7;

}

li {
    list-style: none;
}

.clearfix:before,
.clearfix:after {
    content: "";
    display: table;
}

.clearfix:after {
    clear: both;
}

.clearfix {
    *zoom: 1;
}

.w {
    width: 1200px;
    margin: 0px auto;
}

.header {
    height: 42px;
    margin: 30px auto;
}

.header .logo {
    float: left;
    width: 195px;
    height: 42px;
}

.header .nav {
    float: left;
    margin-left: 55px;
}

.header ul li {
    float: left;
    margin: 0px 15px;
}

.header ul li a {
    height: 42px;
    line-height: 42px;
    display: block;
    padding: 0px 10px;
    font-size: 18px;
    color: #050505;
}

.header ul li a:hover {
    color: #00a4ff;
    border-bottom: 2px solid #00a4ff;
}

.header .search {
    float: left;
    width: 412px;
    height: 42px;
    margin-left: 60px;
}

.header .search input {
    float: left;
    height: 40px;
    width: 345px;
    border: 1px solid #00a4ff;
    border-right: 0;
    color: #bfbfbf;
    padding-left: 15px;
}

.header .search button {
    float: left;
    width: 50px;
    height: 42px;
    border: 0;
    background-image: url(imgs/search.png);
}

.header .user {
    float: left;
    line-height: 42px;
    margin-left: 40px;
}

.header .user .avatar {
    margin: 5px 10px 0 0;
    background-color: skyblue;
    float: left;
    height: 32px;
    width: 32px;
    border-radius: 16px;
    background-image: url(imgs/avatar.png);
}

.banner {
    background-color: #1c036c;
    height: 421px;
}

.banner .w {
    height: 421px;
    background-image: url(imgs/banner.png);
    background-position: center top;
    background-repeat: no-repeat;
}

.banner .subnav {
    float: left;
    padding-top: 10px;
    height: 411px;
    width: 200px;
    background-color: rgba(0, 0, 0, 0.3);
}

.banner .subnav ul li {
    height: 45px;
    line-height: 45px;
    padding: 0 20px;
}

.banner .subnav a {
    color: #fff;
    font-size: 14px;
}

.banner .subnav ul li a span {
    float: right;
}

.banner .subnav a:hover {
    color: #00a4ff;
}

.banner .course {
    float: right;
    width: 230px;
    height: 300px;
    background-color: #fff;
    margin-top: 50px;
}

.banner .course h2 {
    height: 48px;
    line-height: 48px;
    text-align: center;
    background-color: #9bceea;
    font-size: 18px;
    color: #fff;
}

.banner .course .bd {
    padding: 0 15px;
}

.banner .course ul li {
    padding: 12px 0;
    border-bottom: 1px solid #ccc;
}

.banner .course h4 {
    font-size: 16px;
    color: #4e4e4e;
}

.banner .course p {
    font-size: 12px;
    color: #a5a5a5;
}

.banner .course .more {
    display: block;
    height: 38px;
    width: 198px;
    margin-top: 5px;
    text-align: center;
    line-height: 38px;
    border: 1px solid #00a4ff;
    color: #00a4ff;
    font-weight: 700;
}

.goods {
    height: 60px;
    line-height: 60px;
    margin-top: 10px;
    background-color: #fff;
    box-shadow: 0 2px 3px 3px rgba(0, 0, 0, 0.1);
}

.goods h4 {
    float: left;
    margin-left: 30px;
    color: #00a4ff;
    font-size: 16px;
    font-weight: 700;
}

.goods ul {
    float: left;
    margin-left: 30px;
}

.goods ul li {
    float: left;
}

.goods ul li a {
    padding: 0 35px;
    border-left: 1px solid gray;
    font-size: 16px;
    color: #050505;
}

.goods .mod {
    float: right;
    font-size: 14px;
    color: #00a4ff;
    margin-right: 22px;
}

.box {
    margin-top: 30px;
    padding-bottom: 20px;
}

.box-hd {
    height: 45px;
}

.box .box-hd h3 {
    float: left;
    font-size: 20px;
    color: #494949;
}

.box .box-hd a {
    float: right;
    margin-right: 20px;
    font-size: 12px;
    color: #a5a5a5;
}

.box .box-bd ul {
    width: 1215px;
}

.box .box-bd ul li {
    float: left;
    margin-right: 15px;
    margin-bottom: 15px;
    width: 228px;
    height: 272px;
    background-color: #fff;
}

.box .box-bd ul li img {
    width: 100%;
}

.box .box-bd ul li h4 {
    margin: 25px 20px 20px 25px;
    font-size: 14px;
    font-weight: 400;
    color: #050505;
}

.box .box-bd ul li p {
    font-size: 12px;
    margin-left: 25px;
    color: #b8b8b8;
}

.box .box-bd ul li span {
    color: #ff7c2d;
}

.footer {
    height: 415px;
    background-color: #fff;
}

.footer .w {
    padding-top: 40px;
}

.footer .left {
    float: left;
    margin-left: 20px;
}

.footer .logo {
    margin-bottom: 25px;
}

.footer .copyright {
    margin-bottom: 20px;
    font-size: 12px;
    color: #666666;

}

.footer .app {
    display: inline-block;
    height: 33px;
    width: 118px;
    text-align: center;
    line-height: 33px;
    color: #00a4ff;
    border: 1px solid #00a4ff;
}

.footer .right {
    float: right;
    margin-right: 30px;
}

.footer dl {
    float: right;
    margin-left: 100px;
}

.footer dl dt {
    font-size: 16px;
    margin-bottom: 8px;
}

.footer dl dd {
    margin-bottom: 5px;
}

.footer dl dd a {
    font-size: 12px;
    color: #050505;
}
```

