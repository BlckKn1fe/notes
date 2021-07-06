# JQuery

笔记整理自 

>https://www.bilibili.com/video/BV1J4411877



# 版本说明

目前jQuery有三个大版本：
**1.x**：兼容ie678,使用最为广泛的，官方只做BUG维护，功能不再新增。因此一般项目来说，使用 1.x 版本就可以了
最终版本：1.12.4 (2016年5月20日)

**2.x**：不兼容ie678，很少有人使用，官方只做BUG维护，功能不再新增。如果不考虑兼容低版本的浏览器可以使用2.x
最终版本：2.2.4 (2016年5月20日)

**3.x**：不兼容ie678，只支持最新的浏览器。除非特殊要求，一般不会使用3.x版本的，很多老的jQuery插件不支持这个版本。目前该版本是官方主要更新维护的版本。

最新版本：3.2.1（2017年3月20日）

**jquery.js**：开发版本，主要给程序员看源码的，有更好的阅读体验

**jquery.min.js**：生产版本，内容没有缩进，只是加速了加载速度

# 快速开始

```jsx
// 在 head 中引入
<script type="text/javascript" src="../js/jquery-3.5.1.js"></script>
```

注意：

- 自己的 JS 代码要写在 head 标签后面
- 或者写在 ```$function(){}``` 中

获取对象：

```jsx
// 返回一个 JQuery 对象
const object = $("选择器");
```

# JQuery 对象和 JS 对象

JQuery 对象本质也是对 JS 对象的一种封装，其提供了很多方便的操作。

JS 对象和 JQuery 对象不通用

示例：

```jsx
const divs = document.getElementsByTagName("div");
console.log(divs.length);
for (let i = 0; i < divs.length; i++) {
	divs[i].innerHTML = "kkk";
}

const $divs = $("div");
console.log($divs.length);
$divs.html("ccc")
```

JQuery 封装之后的 JS 对象，是存在数组里的，即使是把一个 JS 对象封装成 JQuery 对象，也是会存在一个数组里的。JQuery 对象和 JS 对象是可以互相转换的。

JS → JQuery：（还是以上面代码作示例）

```jsx
// 通过 $() 封装
const divs = document.getElementsByTagName("div");
console.log(divs.length);
for (let i = 0; i < divs.length; i++) {
	$(divs[i]).html("kkk");
}
```

JQuery → JS：

```jsx
// 1. 通过索引
const $divs = $("div");
console.log($divs.length);
$divs[0].innerHTML = "sss";

// 2. 通过 Object.get(index) 的方式
const $divs = $("div");
console.log($divs.length);
$divs.get(1).innerHTML = "sss";

```

# 入口函数

因为浏览器解析 HTML 文件顺序的原因，如果只是单纯把 JS 代码写在 Head 标签前面，那么有很多代码是没法正确执行的，因为 DOM 文档还没有被生成出来。所以，用普通 JS 的方法来解决这个问题的话，是使用 `window.onload = function() {}` 来解决，而 JQuery 的话是通过：

```jsx
$(function() {});
```

JQuery 的入口函数和普通的入口函数有一个很重要的区别：

JQuery 这面可以多次定义，但是普通的只能定义一次！

# 添加事件

通过 `object.click()` 方法来添加

```jsx
let btn = $("#btn");
btn.click(() => {
    console.log("点击按钮");
});
```

# 样式控制

DOM 操作的办法是通过设置 style 来控制样式，JQuery 有一个 `css()` 方法

```jsx
let btn = $("#btn");
btn.click(() => {
    $("#div1").css("backgroundColor", "blue");
});
```

# 选择器

选择器太多了，分类来一个个说

## 基本选择器

1. 标签选择器

    ```jsx
    $("html标签名");  // 获得所有匹配的标签名元素
    ```

2. id 选择器

    ```jsx
    $("#id");  // 获取指定 id 的元素
    ```

3. 类选择器

    ```jsx
    $(".class");  // 获取指定 class 属性的元素
    ```

4. 并集选择器

    ```jsx
    $("选择器1,  选择器2, ...")  // 获取多个选择器中的所有元素
    ```

## 层级选择器

1. 后代选择器

    ```jsx
    $("A B ...")  // A 元素内的所有 B 元素
    ```

2. 子选择器

    ```jsx
    $("A > B")  // A 元素内的最近一层的 B 元素
    ```

## 属性选择器

1. 属性名称选择器

    ```jsx
    $("A[属性名]")  // 包含指定属性的选择器
    ```

2. 属性选择器

    ```jsx
    $("A[属性名='值']")  // 包含指定属性等于指定值的选择器
    ```

3. 复合选择器

    ```jsx
    $("A[属性名='值'][]...")  // 包含多条属性条件的选择器
    ```

其中匹配属性值的时候，可以通过：

- ^= 来选择以特定值开始的属性名
- $= 来选择以特定值结束的属性名
- *= 来选择含有特定值的属性名

## 过滤选择器

1. 首元素选择器

    ```jsx
    :first  // 获取选择的元素中的第一个元素
    ```

2. 尾元素选择器

    ```jsx
    :last  // 获取选择的元素中的最后一个元素
    ```

3. 非元素选择器

    ```jsx
    :not(selector)  // 不包括指定内容的元素
    ```

4. 偶数选择器

    ```jsx
    :even  // 偶数，从 0 开始计数
    ```

5. 奇数选择器

    ```jsx
    :add  // 奇数，从 0 开始计数
    ```

6. 等于索引选择器

    ```jsx
    :eq(index)  // 指定索引元素
    ```

7. 大于索引选择器

    ```jsx
    :gt(index)  // 大于指定索引元素
    ```

8. 小于选择器

    ```jsx
    :lt(index)  // 小于指定索引元素 
    ```

9. 标题选择器

    ```jsx
    :header  // 获得标题（h1 ~ h6） 元素，固定写法
    ```

## 表单过滤选择器

1. 可用元素选择器

    ```jsx
    :enabled  // 获得可用选择器
    ```

2. 不可用元素选择器

    ```jsx
    :disabled  // 获得不可用元素
    ```

3. 勾选选择器

    ```jsx
    :checked  // 获得单选 / 复选框选中的元素
    ```

4. 选中选择器

    ```jsx
    :selected  // 获得下拉框选中的元素
    ```

# DOM 操作

## 内容操作

1. 通过 `html()` 方法，获得标签体，比如 

    ```html
    <div id="mydiv"><p><a href="#">标题标签</a></p></div>

    <script>
    	var tag = $("#mydiv").html();
    </script>

    ```

    上面获取到的内容为 `<p><a href="#">标题标签</a></p>`

    同时，在 html() 方法中传入参数的话，则替换掉标签体内容，基本和 innerHTML 一样

2. 通过 `text()` 方法获取标签体内的内容，比如

    ```html
    <div id="mydiv"><p><a href="#">标题标签</a></p></div>
    <script>
    	var text = $("#mydiv").text();
    </script>
    ```

    这个获取到的内容是 ”标题标签“ 四个字；但是如果传参的话，修改的是整个标签体，会把 p 标签和 a 标签替换掉

3. 通过 `val()` 方法来获取输入内容

    ```html
    <input id="myinput" type="text" name="hello" value="hello world"/><br/>

    <script>
    	var val = $("#myinput").val();
    </script>
    ```

## 属性操作

1. 通用属性操作

    ```jsx
    // 获取 / 设置元素的属性
    $("selector").attr();  

    // 删除属性
    $("selector").removeAttr();  

    /* 下面操作固有属性，上面操作自定义属性 */

    // 获取设置元素的属性
    $("selector").prop();  

    // 删除属性
    $("selector").removeProp();  
    ```

2. 对 class 属性操作

    ```jsx
    // 添加 class 属性值
    $("selector").addClass("className");

    // 删除 class 属性值
    $("selector").removeClass("className");

    // 切换 class 属性值，如果存在该 class，则删除掉，否则添加
    $("selector").toggleClass("className");
    ```

## CRUD 操作（全部为 jQ 对象）

1. `append()`：把一个元素追加到该元素内部，并追加在末尾

    ```jsx
    // 在 o1 对象内部的末尾追加对象 o2
    o1.append(o2);
    ```

2. `prepend()`：把一个元素追加到该元素内部，并追加在开头

    ```jsx
    // 在 o1 对象内部的开头追加对象 o2
    o1.prepend(o2);
    ```

3. `appendTo()`：把该元素追加到目标元素内部，并添加在末尾

    ```jsx
    // 把对象 o1 添加到 o2 对象内部的末尾
    o1.appendTo(o2);
    ```

4. `prependTo()`：把该元素追加到目标元素内部，并添加在开头

    ```jsx
    // 把对象 o1 添加到 o2 对象内部的开头
    o1.prependTo(o2);

    ```

    ---

    以上都是子父级的操作，下面都是平级操作

    ---

5. `after(`)：把一个元素添加到该元素的后面，**这俩元素平级，兄弟关系**

    ```jsx
    // 在 o1 对象后面添加对象 o2
    o1.after(o2);
    ```

6. `before()`：把一个元素添加到该元素的前面，**这俩元素平级，兄弟关系**

    ```jsx
    // 在 o1 对象前面添加对象 o2
    o1.before(o2);
    ```

7. `insertAfter()`：把该对象添加到目标对象的后面

    ```jsx
    // 在 o2 后面添加 o1
    o1.insertAfter(o2);
    ```

8. `insertBefore()`：把该对象添加到目标对象的前面

    ```jsx
    // 在 o2 前面添加 o1
    o1.insertBefore(o2);
    ```

9. `remove()`：删除该对象

    ```jsx
    // 删除对象 o
    o.remove()
    ```

10. `empty()`：清空该元素的所有子元素，并保留当前对象以及属性节点

    ```jsx
    o.empty()
    ```

# 动画

有三种方式来显示和隐藏元素

## 默认显示和隐藏（斜角收回去）

1. `show([speed, [easing], [fn]])：`

    参数：

    - speed：动画速度。预定义值有 slow，normal，和 fast；也可以传毫秒值
    - easing：指定切换效果，默认为 swing
    - fn：动画完成时执行目标函数，每个元素执行一次

    ```jsx
    #("selector").show("slow", "swing", () => { /* fn */});
    ```

2. `hide([speed, [easing], [fn]])：`

    （参数同上）

    ```jsx
    #("selector").hide("normal", "linear", () => { /* fn */});
    ```

3. toggle([speed, [easing], [fn]])：

    （参数同上）

    ```jsx
    #("selector").toggle("fast", "swing", () => { /* fn */});
    ```

## 滑动显示和隐藏（上下拉窗帘）

1. `slideDown([speed, [easing], [fn]])：`

    ```jsx
    #("selector").slideDown("fast", "swing", () => { /* fn */});
    ```

2. `slideUp([speed, [easing], [fn]])：`

    ```jsx
    #("selector").slideUp("slow", "swing", () => { /* fn */});
    ```

3. `slideToggle([speed, [easing], [fn]])：`

    ```jsx
    #("selector").slideToggle("normal", "swing", () => { /* fn */});
    ```

## 淡入淡出显示和隐藏

1. `fadeIn([speed, [easing], [fn]])：`

    ```jsx
    #("selector").fadeIn("normal", "swing", () => { /* fn */});
    ```

2. `fadeOut([speed, [easing], [fn]])：`

    ```jsx
    #("selector").fadeOut("normal", "swing", () => { /* fn */});
    ```

3. `fadeToggle([speed, [easing], [fn]])：`

    ```jsx
    #("selector").fadeToggle("normal", "swing", () => { /* fn */});
    ```

    # 遍历

    介绍 JS 和 jQuery 的遍历方式

    ## JS 遍历

    for(初始值; 条件; 步长)

    ```jsx
    // 以城市列表举例
    $(function() {
    	var cities = $("#city li");
    	for (var i = 0; i < cities.length; i++) {
    		console.log(i + " : " + cities[i].innnerHTML);
    	}	
    })
    ```

    ## jQ 遍历

    1. `object.each(callback)`

        这里的 object 必须为 jQ 对象

        ```jsx
        var cities = $("#city li");
        // 其中 index 和 element 起的名字不重要，顺序重要
        // element 为 JS 对象
        cities.each(function([index], [element]) {
        	console.log(this.innerHTML);
        	console.log(index + " : " + element.innerHTML);
        })
        ```

        想要断掉循环，需要通过 return 来实现

        - return false 等价于 break
        - return true 等价于 continue

    2. `$.each(object, [callback])`

        这里的 object 可以为 JS 数组对象

        ```jsx
        var cities = $("#city li");
        $.each(cities, function([index], [element]) {
        	console.log($(this).html());
        })
        ```

    3. `for .. of` （jQuery 3.0 之后）

        ```jsx
        var cities = $("#city li");
        for (li of cities) {
        	console.log($(li).html());
        }
        ```

# 事件绑定

1. 标准绑定方式

    o 为 jQ 对象，event 为事件类型，例如 click，mouseover 等

    ```jsx
    // 普通写法
    o.event(function() {/* callback function */});

    // 链式编程
    o.event(function() {
    	/* callback function */
    }).event(function() {
    	/* callback function */
    });
    ```

2. on/off 绑定/解绑

    ```jsx
    // 绑定
    $("selector").on("eventName", function() { /* callback function */});

    // 解绑
    $("selector").off("eventName", function() { /* callback function */});
    ```

3. 事件切换 toggle

    1.9 版本该方法被移除，使用 jQuery Migrate 插件恢复

    `toggle(fn1, fn2, [fn3, ...])`

# 插件

插件机制可以增强 jQuery 的功能，有两种实现方式

1. `$.fn.extend(object)`

    这个是对 jQ 对象的一个增强，比如使用 `$("selector")`获取到了一个 jQ 对象，增强之后，这个对象可以调用一些自定义方法。使用格式如下：

    ```javascript
    // 增加自定义方法
    $.fn.extend({
    	fn1:function() {},
    	fn2:function() {},
    	...
    });

    // 调用自定义方法
    $("selector").fn1();
    $("selector").fn2();
    ```

2. `$.extend(object)`

    $ 符号代表 jQuery 对象本身，通过这样的方法增强的话，直接调用 `$.fu()`就可以直接使用

    ```jsx
    // 增加自定义全局方法
    $.extend({
    	fn:function() {};
    });

    // 使用定义的全局方法
    $.fn();
    ```