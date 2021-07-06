---
title: HTML/CSS案例一
date: 2020-07-04 23:51:25
categories: 
- 前端
tags:
- 前端
- HTML
- CSS
---
以下为模仿小米商城中某一商品栏的内容进行的模仿，成品结果大概如下图：

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20200704235256.png)

代码如下：
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            background-color: #f5f5f5;
        }

        a {
            text-decoration: none;
            color: #333
        }

        .box {
            width: 300px;
            height: 415px;
            background-color: #fff;
            margin: 100px auto;
        }

        .box img {
            width: 100%;
        }

        .review {
            height: 70px;
            font-size: 14px;
            padding: 0px 30px;
            margin-top: 30px;
        }

        .appraise {
            padding: 0px 30px;
            margin-top: 20px;
            font-size: 12px;
            color: #b0b0b0;
        }

        .info {
            font-size: 14px;
            margin-top: 15px;
            padding: 0px 30px;
        }

        .info h4 {
            display: inline-block;
            font-weight: 400;
        }

        .info span {
            color: #ff6700
        }

        .info em {
            font-style: normal;
            color: #ebe4e0;
            margin: 0 5px 0 15px;
        }
    </style>
</head>

<body>
    <div class="box">
        <a href="#"><img src="img/1.jpg" alt=""></a>
        <p class="review">评论信息...评论信息...评论信息...评论信息...评论信息...评论信息...</p>
        <div class="appraise">来自 黑刀 的评价</div>
        <div class="info">
            <h4><a href="#">Redmi AirDots真无线蓝...</a></h4>
            <em>|</em>
            <span>99.9元</span>
        </div>
    </div>
</body>

</html>
```

注意事项：
- 标签具有语义，合适的地方用合适的标签，如 p 是段落文字，h 为（产品）标题等等
- 类名多不要紧，可以更精确定位每一个盒子
- padding 和 margin 混着用没问题，灵活使用
- 多操作，多利用工具练习布局思路