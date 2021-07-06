---
title: HTML/CSS案例四
date: 2020-08-04 18:20:56
categories: 
- 前端
tags:
- 前端
- HTML
- CSS
---

这个是利用 C3 的 transform 来做一些 3D 动画

![image-20200804182347608](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200804182347608.png)



```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        @keyframes rotation {
            0% {
                transform: rotateY(0);
            }

            100% {
                transform: rotateY(360deg);
            }
        }

        body {
            perspective: 800px;
        }

        section {
            position: relative;
            width: 300px;
            height: 200px;
            margin: 250px auto;
            transform-style: preserve-3d;
            animation: rotation 10s linear infinite;
            background: url("dog.png") no-repeat;
        }

        section div {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url("cat.png") no-repeat;
        }

        section div:nth-child(1) {
            transform: translateZ(300px);
        }

        section div:nth-child(2) {
            transform: rotateY(60deg) translateZ(300px);
        }

        section div:nth-child(3) {
            transform: rotateY(120deg) translateZ(300px);
        }

        section div:nth-child(4) {
            transform: rotateY(180deg) translateZ(300px);
        }

        section div:nth-child(5) {
            transform: rotateY(240deg) translateZ(300px);
        }

        section div:nth-child(6) {
            transform: rotateY(300deg) translateZ(300px);
        }
    </style>
</head>

<body>
    <section class="box">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
    </section>
</body>
```

