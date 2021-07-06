---
title: JS快速入门
date: 2020-07-11 05:02:30
categories: 
- 前端
tags:
- 前端
- JavaScript
---



> 这里笔记内容都是参考廖雪峰对于 JavaScript 教程而进行有针对性的提炼的
>
> https://www.liaoxuefeng.com/wiki/1022910821149312/



# 数据类型

## 比较运算符

JavaScript允许对任意数据类型做比较：

```javascript
false == 0; // true
false === 0; // false
```

有两种比较运算符：

第一种是`==`比较，**它会自动转换数据类型再比较**，很多时候，会得到非常诡异的结果；

第二种是`===`比较，**它不会自动转换数据类型**，如果数据类型不一致，返回`false`，如果一致，再比较。

另一个例外是`NaN`这个特殊的Number与所有其他值都不相等，包括它自己：

```javascript
NaN === NaN; // false
```

唯一能判断`NaN`的方法是通过`isNaN()`函数：

```javascript
isNaN(NaN); // true
```

JavaScript把`null`、`undefined`、`0`、`NaN`和空字符串`''`视为`false`，其他值一概视为`true`，因此上述代码条件判断的结果是`true`。

## null 和 undefined

在JavaScript中，还有一个和`null`类似的`undefined`，它表示“未定义”。

JavaScript的设计者希望用`null`表示一个空的值，而`undefined`表示值未定义。事实证明，这并没有什么卵用，区分两者的意义不大。大多数情况下，我们都应该用`null`。`undefined`仅仅在判断函数参数是否传递的情况下有用。



## 数组和对象

数组是一组按顺序排列的集合，集合的每个值称为元素。JavaScript的数组可以包括任意数据类型。例如：

```javascript
[1, 2, 3.14, 'Hello', null, true];
```

另一种创建数组的方法是通过`Array()`函数实现：

```
new Array(1, 2, 3); // 创建了数组[1, 2, 3]
```

然而，出于代码的可读性考虑，强烈建议直接使用`[]`。



## 对象

JavaScript的对象是一组由 **键-值** 组成的无序集合，例如：

```javascript
var person = {
    name: 'Bob',
    age: 20,
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
};
```

要获取一个对象的属性，我们用`对象变量.属性名`的方式：

```javascript
person.name; // 'Bob'
person.zipcode; // null
```



## strict 模式

如果一个变量没有通过`var`申明就被使用，那么该变量就自动被申明为全局变量：

```javascript
i = 10; // i现在是全局变量
```

为了修补JavaScript这一严重设计缺陷，ECMA在后续规范中推出了strict模式，在strict模式下运行的JavaScript代码，强制通过`var`申明变量，未使用`var`申明变量就使用的，将导致运行错误。

启用strict模式的方法是在JavaScript代码的第一行写上：

```javascript
'use strict';
```

不支持strict模式的浏览器会把它当做一个字符串语句执行，支持strict模式的浏览器将开启strict模式运行JavaScript。



# 字符串

字符串就是用 `''` 或 `""` 括起来的字符表示。如果`'`本身也是一个字符，那就可以用`""`括起来，比如 `"I'm OK"` 包含的字符是`I`，`'`，`m`，空格，`O`，`K`这6个字符。

可转义 ASCII 字符和 Unicode 字符：

```javascript
'\x41'; // ASCII 完全等同于 'A'
'\u4e2d\u6587'; // Unicode 完全等同于 '中文'
```



## 多行字符串

由于多行字符串用`\n`写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用反引号 *`\* ... \*`* 表示：

```javascript
`这是一个
多行
字符串`;
```

*注意：反引号在键盘的ESC下方，数字键1的左边：*



## 模板字符串

要把多个字符串连接起来，可以用 `+` 号连接

如果有很多变量需要连接，用 `+` 号就比较麻烦。ES6新增了一种模板字符串，表示方法和上面的多行字符串一样，但是它会自动替换字符串中的变量：

```javascript
var name = '小明';
var age = 20;
var message = `你好, ${name}, 你今年${age}岁了!`;
alert(message);
```



## 操作字符串

### 先重点说两个

要获取字符串某个指定位置的字符，使用类似Array的下标操作，索引号从0开始，超出范围的索引不会报错，但一律返回undefined

```javascript
var s = 'Hello, world!';

s[0]; // 'H'
s[6]; // ' '
s[7]; // 'w'
s[12]; // '!'
s[13]; // undefined 超出范围的索引不会报错，但一律返回undefined
```

**需要特别注意的是，字符串是不可变的**，如果对字符串的某个索引赋值，**不会有任何错误，但是，也没有任何效果**：

```javascript
var s = 'Test';
s[0] = 'X';
alert(s); // s仍然为'Test'
```



JavaScript为字符串提供了一些常用方法，**注意，调用这些方法本身不会改变原有字符串的内容，而是返回一个新字符串**

### toUpperCase

`toUpperCase()`把一个字符串全部变为大写：

```javascript
var s = 'Hello';
s.toUpperCase(); // 返回'HELLO'
```

### toLowerCase

`toLowerCase()`把一个字符串全部变为小写：

```javascript
var s = 'Hello';
var lower = s.toLowerCase(); // 返回'hello'并赋值给变量lower
lower; // 'hello'
```

### indexOf

`indexOf()`会搜索指定字符串出现的位置：

```javascript
var s = 'hello, world';
s.indexOf('world'); // 返回7
s.indexOf('World'); // 没有找到指定的子串，返回-1
```

### substring

`substring()`返回指定索引区间的子串：

```javascript
var s = 'hello, world'
s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
s.substring(7); // 从索引7开始到结束，返回'world'
```



# 数组

JavaScript的 `Array` 可以包含任意数据类型，并通过索引来访问每个元素。要取得`Array`的长度，直接访问`length`属性。**注意**，直接给`Array`的`length`赋一个新的值会导致`Array`大小的变化

```javascript
var arr = [1, 2, 3];
arr.length; // 3
arr.length = 6;
arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
arr.length = 2;
arr; // arr变为[1, 2]
```



`Array`可以通过索引把对应的元素修改为新的值，因此，对`Array`的索引进行赋值会直接修改这个`Array`，**注意**，如果通过索引赋值时，索引超过了范围，同样会引起`Array`大小的变化**

```javascript
var arr = [1, 2, 3];
arr[5] = 'x';
arr; // arr变为[1, 2, 3, undefined, undefined, 'x']
```

在编写代码时，不建议直接修改`Array`的大小，访问索引时要确保索引不会越界。



## slice

`slice()`就是对应String的`substring()`版本，它截取`Array`的部分元素，然后返回一个新的`Array`，左闭右开。如果不传参数的话，则会切整个数组，利用这个可以进行复制

```javascript
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
var aCopy = arr.slice();
aCopy; // ['A', 'B', 'C', 'D', 'E', 'F', 'G']
aCopy === arr; // false
```

（为什么上面会返回 false ）



## unshift和shift

如果要往`Array`的头部添加若干元素，使用`unshift()`方法，`shift()`方法则把`Array`的第一个元素删掉：

```javascript
var arr = [1, 2];
arr.unshift('A', 'B'); // 返回Array新的长度: 4
arr; // ['A', 'B', 1, 2]
arr.shift(); // 'A'
arr; // ['B', 1, 2]
arr.shift(); arr.shift(); arr.shift(); // 连续shift 3次
arr; // []
arr.shift(); // 空数组继续shift不会报错，而是返回undefined
arr; // []
```



## sort

`sort()`可以对当前`Array`进行排序，它会直接修改当前`Array`的元素位置，直接调用时，按照默认顺序排序：

```javascript
var arr = ['B', 'C', 'A'];
arr.sort();
arr; // ['A', 'B', 'C']
```

（还有个 reverse就不说了，sort 后续还会说）



## splice

`splice()`方法是修改`Array`的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素：

```javascript
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

第一个参数是起始位置，第二个参数是从起始位置开始要删除的元素个数，后续就是从起始位置开始要添加的元素



## concat

`concat()`方法把当前的`Array`和另一个`Array`连接起来，并返回一个新的`Array`，**注意**，`concat()`方法并没有修改当前`Array`，而是返回了一个新的`Array`。

实际上，`concat()`方法可以接收任意个元素和`Array`，并且自动把`Array`拆开，然后全部添加到新的`Array`里：

```javascript
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```



## join

`join()`方法是一个非常实用的方法，它把当前`Array`的每个元素都用指定的字符串连接起来，然后返回连接后的字符串：

```javascript
var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
```

如果`Array`的元素不是字符串，将自动转换为字符串后再连接。



# 对象

JavaScript的对象是一种无序的集合数据类型，它由若干键值对组成。

## 创建

JavaScript用一个`{...}`表示一个对象，键值对以`xxx: xxx`形式申明，用`,`隔开。注意，最后一个键值对不需要在末尾加`,`，如果加了，有的浏览器（如低版本的IE）将报错。



## 访问属性

访问属性是通过`.`操作符完成的，但这要求属性名必须是一个有效的变量名。如果属性名包含特殊字符，就必须用`''`括起来：

```javascript
var xiaohong = {
    name: '小红',
    'middle-school': 'No.1 Middle School'
};
```

`xiaohong`的属性名`middle-school`不是一个有效的变量，就需要用`''`括起来。访问这个属性也无法使用`.`操作符，必须用`['xxx']`来访问：

```javascript
xiaohong['middle-school']; // 'No.1 Middle School'
xiaohong['name']; // '小红'
xiaohong.name; // '小红'
```

JavaScript规定，访问不存在的属性不报错，而是返回`undefined`



## 修改属性

由于JavaScript的对象是动态类型，你可以自由地给一个对象添加或删除属性

```javascript
var xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
xiaoming.age = 18; // 新增一个age属性
xiaoming.age; // 18
delete xiaoming.age; // 删除age属性
xiaoming.age; // undefined
delete xiaoming['name']; // 删除name属性
xiaoming.name; // undefined
delete xiaoming.school; // 删除一个不存在的school属性也不会报错
```



## 检查属性

如果我们要检测`xiaoming`是否拥有某一属性，可以用`in`操作符：

```javascript
var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};
'name' in xiaoming; // true
'grade' in xiaoming; // false
```

**注意**，如果`in`判断一个属性存在，这个属性不一定是`xiaoming`的，它可能是`xiaoming`继承得到的

```
'toString' in xiaoming; // true
```

因为`toString`定义在`object`对象中，而所有对象最终都会在原型链上指向`object`，所以`xiaoming`也拥有`toString`属性。

要判断一个属性是否是`xiaoming`自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法：

```javascript
var xiaoming = {
    name: '小明'
};
xiaoming.hasOwnProperty('name'); // true
xiaoming.hasOwnProperty('toString'); // false
```



# 循环

## for...in

`for`循环的一个变体是`for ... in`循环，它可以把一个对象的所有属性依次循环出来：

```javascript
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    console.log(key); // 'name', 'age', 'city'
}
```

要过滤掉对象继承的属性，用`hasOwnProperty()`来实现：

```javascript
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    if (o.hasOwnProperty(key)) {
        console.log(key); // 'name', 'age', 'city'
    }
}
```

由于`Array`也是对象，而它的每个元素的索引被视为对象的属性，因此，`for ... in`循环可以直接循环出`Array`的索引：

```javascript
var a = ['A', 'B', 'C'];
for (var i in a) {
    console.log(i); // '0', '1', '2'
    console.log(a[i]); // 'A', 'B', 'C'
}
```

**注意**，`for ... in`对`Array`的循环得到的是`String`而不是`Number`。



## for...of

这个循环可以直接拿到元素本身，和 Java 里 enhanced for loop 是一样的。ES6标准引入了新的`iterable`类型，`Array`、`Map`和`Set`都属于`iterable`类型。

```javascript
var a = ['A', 'B', 'C'];
var s = new Set(['A', 'B', 'C']);
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
for (var x of a) { // 遍历Array
    console.log(x);
}
for (var x of s) { // 遍历Set
    console.log(x);
}
for (var x of m) { // 遍历Map
    console.log(x[0] + '=' + x[1]);
}
```





# Map

初始化`Map`需要一个二维数组，或者直接初始化一个空`Map`。

```javascript
// 二维数组:
// var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```



(Set 省略)

`Map`和`Set`是ES6标准新增的数据类型，请根据浏览器的支持情况决定是否要使用。

Map 的 iterator 获取：

```javascript
var iterator = map.entries();
console.log(iteratir.next());
```



# iterable

`for ... in`循环由于历史遗留问题，它遍历的实际上是对象的属性名称。一个`Array`数组实际上也是一个对象，它的每个元素的索引被视为一个属性。

当我们手动给`Array`对象添加了额外的属性后，`for ... in`循环将带来意想不到的意外效果：

```javascript
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    console.log(x); // '0', '1', '2', 'name'
}
```

`for ... in`循环将把`name`包括在内，但`Array`的`length`属性却不包括在内。

`for ... of`循环则完全修复了这些问题，它只循环集合本身的元素：

```javascript
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a) {
    console.log(x); // 'A', 'B', 'C'
}
```

这就是为什么要引入新的`for ... of`循环。

更好的方式是直接使用`iterable`内置的`forEach`方法，它接收一个函数，每次迭代就自动回调该函数。

```javascript
// 廖雪峰教程中书写方式
a.forEach(function (element, index, array) {
    console.log(element + ", index = " + index);
});
// VSCode 自动生成的格式，这个array声明集合类型的
a.forEach((element, index(,array) => {
    console.log(element + index);
});
```

`Set`与`Array`类似，但`Set`没有索引，因此回调函数的前两个参数都是元素本身

```javascript
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    console.log(element);
});
```

`Map`的回调函数参数依次为`value`、`key`和`map`本身：

```javascript
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    console.log(value);
});
```



# 导入导出模块

导出模块使用 ```export``` 关键字：

```javascript
export function formatDate(date) {
	return `${date.getFullYear()}-${date.getMonth()}-${date.getDate()}`;
}
```

导入之前，需要在 HTML 的 script 标签中，添加 type 属性，值为 module：

```html
<script src="main.js" type="module"></script>
```

```javascript
import { formatDate } from "./myjs.js";
console.log(formatDate(new Date()));
```

在导出的时候，可以把 export 写在最后面，然后用大括号包起来，一起导出。



导入导出还有另外一个写法：

```javascript
// 导出的情况 my.js
module.exports = {
    mod1, 
    mod2,
    ...
};
```

```javascript
// 导入的情况 main.js
const {mod1, mod2, ...} = require(./my.js);
```



# 函数

**函数本身也是一个对象！**

## 定义函数

方法 1：

```javascript
function 函数名(参数) {
    ...
}
```

这个函数名，可以理解成对这个函数对象的引用，和 Java 里的引用一个意思。

方法 2：

```javascript
var 函数名 = function(参数) {
    ...
}
```

**上述两种定义*完全等价*，注意第二种方式按照完整语法需要在函数体末尾加一个`;`，表示赋值语句结束。**



## 调用函数

由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数：

```javascript
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
```

传入的参数比定义的少也没有问题：

```javascript
abs(); // 返回NaN
```

此时`abs(x)`函数的参数`x`将收到`undefined`，计算结果为`NaN`。

要避免收到`undefined`，可以对参数进行检查：

```javascript
function abs(x) {
    if (typeof x !== 'number') {
        throw 'Not a number';
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```



## Arguments 和 Rest

在 JS 里，默认集成了叫 `arguments` 的东西，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。`arguments `类似 `Array` 但它不是一个 `Array`

```javascript
function foo(x) {
    console.log('x = ' + x); // 10
    for (var i=0; i<arguments.length; i++) {
        console.log('arg ' + i + ' = ' + arguments[i]); // 10, 20, 30
    }
}
foo(10, 20, 30);
```

上面规定了只拿一个参数来做处理，那后面的并不是完全忽略了，其传进来的值都会存到 `arguments` 里。**所以说，就算是函数本身不定义传参的多少，也是可以拿到实际传进来的所有参数的！**实际上 `arguments` 最常用于判断传入参数的个数。

对应 `arguments` ，还有一个叫 `rest` 的

>ES6标准引入了rest参数

这个 `rest` 只会把多余的传进来的参数打包放在这里面，和 argument 非常相似。



## 作用域

JS 中，用`var`申明的变量实际上是有作用域的。基本上原则和 Java 类似，由于JavaScript的函数可以嵌套，此时，内部函数可以访问外部函数定义的变量，反过来则不行：

```javascript
use strict';

function foo() {
    var x = 1;
    function bar() {
        var y = x + 1; // bar可以访问foo的变量x!
    }
    var z = y + 1; // ReferenceError! foo不可以访问bar的变量y!
}
```

出现重名的情况，就是就近原则，或者说是自己访问自己领域中的。



## 变量提升

JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部：

```javascript
'use strict';

function foo() {
    var x = 'Hello, ' + y;
    console.log(x);
    var y = 'Bob';
}

foo();
```

虽然是strict模式，但语句`var x = 'Hello, ' + y;`并不报错，原因是变量`y`在稍后申明了。但是`console.log`显示`Hello, undefined`，说明变量`y`的值为`undefined`。

**这块说人话就是，JS 会先把一个 function 中的所有变量找出来进行声明，但是不赋值，然后按顺序编译。**

我们在函数内部定义变量时，请严格遵守 **“在函数内部首先申明所有变量”** 这一规则。最常见的做法是用一个`var`申明函数内部用到的所有变量：

```javascript
function foo() {
    var
        x = 1, // x初始化为1
        y = x + 1, // y初始化为2
        z, i; // z和i为undefined
    // 其他语句:
    for (i=0; i<100; i++) {
        ...
    }
}
```



## 全局作用域

JS 这面有点意思，所有最高层级的变量，包括函数，都会被存放在一个叫 `window` 的变量中，如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报 `ReferenceError` 错误。

```javascript
'use strict';

function foo() {
    alert('foo');
}

foo(); // 直接调用foo()
window.foo(); // 通过window.foo()调用
```



## 名字空间

全局变量会绑定到 `window` 上，**不同的JavaScript文件**如果使用了**相同的全局变量**，或者**定义了相同名字的顶层函数**，都会造成**命名冲突**，并且很难被发现。

减少冲突的一个方法是**把自己的所有变量和函数全部绑定到一个全局变量中**。例如：

```javascript
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```

把自己的代码全部放入唯一的名字空间`MYAPP`中，会大大减少全局变量冲突的可能。许多著名的JavaScript库都是这么干的：jQuery，YUI，underscore等等。



## 局部作用域

JS 里如果在 for 循环里用 var 声明了一个变量，即使循环结束，这个变量也依然存在于**当前的函数内**，所以为了解决块级作用域，ES6引入了新的关键字 `let` ，用 `let` 替代 `var` 可以申明一个块级作用域的变量。

```javascript
'use strict';

function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    // SyntaxError:
    i += 1;
}
```

个人见解：其 var 的声明方式很宽泛，只要在一个 “函数” 内，无论在哪里他都是有效的，因为这些变量都会集体存放在一个 “地方”。



## 常量

在ES6之前是不行的，我们通常用全部大写的变量来表示“这是一个常量，不要修改它的值：

```javascript
var PI = 3.14;
```

ES6标准引入了新的关键字`const`来定义常量，`const`与`let`都具有块级作用域：

```javascript
'use strict';

const PI = 3.14;
PI = 3; // 某些浏览器不报错，但是无效果！
// Uncaught TypeError: Assignment to constant variable.
PI; // 3.14
```



## 解构赋值

在ES6中，可以使用解构赋值，直接对多个变量同时赋值：

```javascript
var [x, y, z] = ['hello', 'JavaScript', 'ES6'];  // 如果 x 和 y 为空，则只给 z 赋值
```

以上就对 x，y，和 z 同时进行赋值，对数组元素进行解构赋值的时候，格式要一一对应

```javascript
let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
x; // 'hello'
y; // 'JavaScript'
z; // 'ES6'
```

利用这个特点，可以直接从一个对象中取出若干属性

```javascript
'use strict';

var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};
var {name:nickname, age, passport} = person;
// 当属性名和变量名不一样的时候，可以用上面的冒号进行赋值
```

对一个对象进行解构赋值时，同样可以直接对嵌套的对象属性进行赋值，只要保证对应的层次是一致的:

```javascript
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school',
    address: {
        city: 'Beijing',
        street: 'No.1 Road',
        zipcode: '100001'
    }
};
var {name, address: {city, zip}} = person;
name; // '小明'
city; // 'Beijing'
zip; // undefined, 因为属性名是zipcode而不是zip
// 注意: address不是变量，而是为了让city和zip获得嵌套的address对象的属性:
address; // Uncaught ReferenceError: address is not defined
```

解构的时候，可以给一个默认值，防止对象中没有该属性，null 会覆盖默认值，在 JS 中是一个 “值”



注意事项：

如果变量名在提前已经声明了，那么在进行解构赋值的时候，注意要再代码的最两端加上小括号，这是因为JavaScript引擎把`{`开头的语句当作了块处理，于是`=`不再合法。

```javascript
// 声明变量:
var x, y;
// 解构赋值:
{x, y} = { name: '小明', x: 100, y: 200};
// 语法错误: Uncaught SyntaxError: Unexpected token =
```



## rest操作符

rest 操作符为三个点 ```...``` ，其表示没有被解构到剩余的元素，会以一个数组的形式返回

```javascript
var arr = [1, 2, 3, 4, 5, 6, 7];
var [a, b, ...rest] = arr;
console.log(a, b, rest);
```

在 ```...``` 之后需要给个剩余元素数组一个名字

对于对象来说，rest 会获取到其他的剩余的元素，而不像数组只会获取到后面的元素。

```javascript
const post = {
    id: 2,
    title: "Hello!Java!",
    cotent: "123456",
    comments: [
        {
            userId: 1,
            comment: "Comment 1"
        },{
            userId: 2,
            comment: "Comment 2"
        },{
            userId: 3,
            comment: "Comment 3"
        }
    ]
}

var { comments, ...rest} = post;  // 这里的 rest 包含 id, title, 和 content
```



通过 rest 结构还可以巧妙的获取到特定的参数：

```javascript
function savePostObj({ id, title, content, ... rest}) {
    console.log("保存了文章 ", id, title, content);
    console.log(rest);
}
```





## spread操作符

这个和 rest 虽然长得很像都是三个点，但是概念刚好相反，spread 是取所有，**并逐一分配**，多用于克隆一整个对象

```javascript
var post = {
    id: 1,
    title: "Title 1",
    content: "content"
};

var postClone = { ...post };
console.log(post === postClone);  // false
```

这样克隆出来的是一个新的对象，地址和之前的不一样，同时利用 spread 可以在原有的基础上进行添加：

```javascript
var post2 = {
    ...post, 
    author: "BlackKnife"
};
```

数组也是适用的！





## 解构赋值使用场景



解构赋值在很多时候可以大大简化代码。例如，交换两个变量`x`和`y`的值，可以这么写，不再需要临时变量：

```javascript
var x=1, y=2;
[x, y] = [y, x]
```

快速获取当前页面的域名和路径：

```javascript
var {hostname:domain, pathname:path} = location;
```

如果一个函数接收一个对象作为参数，那么，可以使用解构直接把对象的属性绑定到变量中。例如，下面的函数可以快速创建一个`Date`对象：

```javascript
function buildDate({year, month, day, hour=0, minute=0, second=0}) {
    return new Date(year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second);
}
```

它的方便之处在于传入的对象只需要`year`、`month`和`day`这三个属性：

```javascript
buildDate({ year: 2017, month: 1, day: 1 });
// Sun Jan 01 2017 00:00:00 GMT+0800 (CST)
// 传入情况：
// {year, month, day, hour=0, minute=0, second=0} = { year: 2017, month: 1, day: 1 }
```

也可以传入`hour`、`minute`和`second`属性：

```javascript
buildDate({ year: 2017, month: 1, day: 1, hour: 20, minute: 15 });
// Sun Jan 01 2017 20:15:00 GMT+0800 (CST)
```

使用解构赋值可以减少代码量，但是，**需要在支持ES6解构赋值特性的现代浏览器中才能正常运行**。目前支持解构赋值的浏览器包括Chrome，Firefox，Edge等。



## 方法

绑定到对象上的函数称为方法，**这个方法可以写在对象外，也可以写在对象内！**

```javascript
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 今年调用是25,明年调用就变成26了
```

上面 ```xiaoming.age``` 返回的只是这个 function 本身，而 ```xiaoming.age()``` 才是真正的调用了该函数。**特别特别注意一点！** 这个 ```this``` 是个其调用者绑定的！这个 ```this``` 是个其调用者绑定的！这个 ```this``` 是个其调用者绑定的！首先上面这个代码是可以拆开写的：

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

console.log(xiaoming.age()) // 25, 正常结果
getAge(); // NaN 或者直接报错
```

这里，```xiaoming.age()``` 的调用者是 ```xiaoming``` ，那么这个 ```this``` 也就是指向的是 ```xiaoming``` 本身，才能获取到需要的属性值；而下面直接调用 ```getAge()``` 的话，其实际的调用者为 ```window``` ！切记！这个 ```this``` 是跟着调用者走的！

## apply和call

这个 apply 可以在某种程度上解决上面的 “方法” 带来的种种缺陷，它接收两个参数，第一个参数就是需要绑定的 `this `变量，第二个参数是 `Array`，表示函数本身的参数。

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空，getAge() 本身没参数，所以不写

var fn = xiaoming.age;
console.log(fn.apply(xiaoming, []))  // 这样也是可以的
```

另一个与`apply()`类似的方法是`call()`，唯一区别是：

- `apply()`把参数打包成`Array`再传入；
- `call()`把参数按顺序传入。

比如调用`Math.max(3, 5, 4)`，分别用`apply()`和`call()`实现如下：

```javascript
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```

对普通函数调用，我们通常把`this`绑定为`null`。



还有一个 ```bind``` 用法和 ```call``` 一样只是它会返回一个闭包，不会马上执行。



## 装饰器

利用`apply()`，我们还可以动态改变函数的行为。这里个人觉得就是对某个函数的重写再封装，先获取到原函数对象，然后改写。

现在假定我们想统计一下代码一共调用了多少次`parseInt()`，可以把所有的调用都找出来，然后手动加上`count += 1`，不过这样做太傻了。最佳方案是用我们自己的函数替换掉默认的`parseInt()`：

```javascript
'use strict'; 

var count = 0; 
var oldParseInt = parseInt; // 保存原函数 

window.parseInt = function () {    
    count += 1;    
    return oldParseInt.apply(null, arguments); // 调用原函数 
};
```





# 高阶函数

所谓高阶函数，就是把一个 function 当做参数传给另一个方法，然后在其内部进行调用：

```javascript
function add(x, y, f) {
    return f(x) + f(y);
}
```

当我们调用 `add(-5, 6, Math.abs)` 时，参数`x`，`y`和`f`分别接收`-5`，`6`和函数`Math.abs`，根据函数定义，我们可以推导计算过程为：

```javascript
x = -5;
y = 6;
f = Math.abs;
f(x) + f(y) ==> Math.abs(-5) + Math.abs(6) ==> 11;
return 11;
```



## map

JS 里的 map 和我想的 map 不太一样，它更多的是起到一个**批量统一处理**的作用，可以用一个数组来调用 map() 函数，然后在函数里传一个想进行操作的函数。

```javascript
'use strict';

function pow(x) {
    return x * x;
}

var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
console.log(results);

```



## reduce

这东西，也是 array 里有的一方法，需要往里传入一个 function，这个 function 必须有两个参数。作用呢，是需对这个数组里所有的元素进行一个操作，一个接着一个，且结果累加：

这里的 x 为上一次的结果，y 为当前元素

```javascript
'use strict';

function foo(x, y) {
    return x * y;
}
var arr = [1, 3, 5, 7, 9];

console.log(arr.reduce(foo));

```

上面这个 reduce，首先是把 1 * 3 的乘在一起，然后这个结果继续和下一个相乘，直到结尾。



## map + reduce 小案例

把一个数字字符串转换成数字，写一个 string2int 的function：

```javascript
'use strict';

function string2int(s) {
        return s.split("")
            .map(function(x) { return x - 0; })
            .reduce(function (x, y) { return x * 10 + y; });
}
```

把一个字符串格式的数字转换成整数的形式，可以用 ```parseInt```，```Number``` 和 字符串减0



请把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。输入：`['adam', 'LISA', 'barT']`，输出：`['Adam', 'Lisa', 'Bart']`

```javascript
function normalize(arr) {
    for (let i = 0; i < arr.length; i++) {
        var temp = arr[i].toLowerCase().split("");
        temp[0] = temp[0].toUpperCase();
        arr[i] = temp.reduce(function (x, y) {
            return x.concat(y);
        })
    }
    return arr;
}
```



## filter

和`map()`类似，`Array`的`filter()`也接收一个函数。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`true`还是`false`决定保留还是丢弃该元素。

例如，在一个`Array`中，删掉偶数，只保留奇数，可以这么写：

```javascript
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
r; // [1, 5, 9, 15]
```

用 filter 巧妙去重：

```javascript
'use strict';

var r,
    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];

r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});

```



## sort

JS 里的 Array 自己的 sort()  方法，是先把内容转换成字符串然后在进行比较的！！真是天坑！！所以说，这个 sort 可以往里传入一个自己写的排序函数，来重写他的排序规则

```javascript
'use strict';

var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
});
console.log(arr); // [1, 2, 10, 20]
```

如果按照字符串的先后顺序进行比较的话，可以先把他们全都变成小写的，然后在进行比较。

另外注意一点就是，**sort 会对原数组进行修改！**



## Array

对于数组，除了`map()`、`reduce`、`filter()`、`sort()`这些方法可以传入一个函数外，`Array`对象还提供了很多非常实用的高阶函数。



`every()`方法可以判断数组的所有元素是否满足测试条件，比如下面的例子：

```javascript
'use strict';
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.every(function (s) {
    return s.length > 0;
})); // true, 因为每个元素都满足s.length>0

console.log(arr.every(function (s) {
    return s.toLowerCase() === s;
})); // false, 因为不是每个元素都全部是小写

```



`find()`方法用于查找符合条件的第一个元素，如果找到了，返回这个元素，否则，返回`undefined`：



```javascript
'use strict';
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.find(function (s) {
    return s.toLowerCase() === s;
})); // 'pear', 因为pear全部是小写

console.log(arr.find(function (s) {
    return s.toUpperCase() === s;
})); // undefined, 因为没有全部是大写的元素
```



`findIndex()`和`find()`类似，也是查找符合条件的第一个元素，不同之处在于`findIndex()`会返回这个元素的索引，如果没有找到，返回`-1`



`forEach()`和`map()`类似，它也把每个元素依次作用于传入的函数，但不会返回新的数组。`forEach()`常用于遍历数组，因此，传入的函数不需要返回值



还有 ```some``` 方法...等等



# 闭包

闭包这块，个人感觉，是把一个参数丢进去之后，先不通过内部存在的函数马上获取到结果，而是把这个参数与这个内部存在的函数绑定在一起，返回成一个类似于对象的东西？等需要的时候，再执行，不过这里面有一些坑：

```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push(function () {
            return i * i;
        });
    }
    return arr;
}

var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];
```

这里，f1，f2，和 f3 存的那个闭包对象里的那个 i，其实都是同一个 i，因为退出循环最后这个 i 是4，所以他们的结果都是 16，所以返回闭包时牢记的一点就是：**返回函数不要引用任何循环变量，或者后续会发生变化的变量**。

如果非要这么做的话，可以采用匿名函数的形式来解决：

```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push((function (n) {
            return function () {
                return n * n;
            }
        })(i));
    }
    return arr;
}
```

“创建一个匿名函数并立刻执行”的语法：

```javascript
(function (x) {
    return x * x;
})(3); // 9
```

理论上讲，创建一个匿名函数并立刻执行可以这么写：

```javascript
function (x) { return x * x } (3);
```

但是由于JavaScript语法解析的问题，会报SyntaxError错误，因此需要用括号把整个函数定义括起来：

```javascript
(function (x) { return x * x }) (3);
```

在没有`class`机制，只有函数的语言里，借助闭包，同样可以封装一个私有变量。我们用JavaScript创建一个计数器

```javascript
'use strict';

function create_counter(initial) {
    var x = initial || 0;
    return {
        inc: function () {
            x += 1;
            return x;
        }
    }
}
```

它用起来像这样：

```javascript
var c1 = create_counter();
c1.inc(); // 1
c1.inc(); // 2
c1.inc(); // 3

var c2 = create_counter(10);
c2.inc(); // 11
c2.inc(); // 12
c2.inc(); // 13
```

闭包还可以把多参数的函数变成单参数的函数。例如，要计算x^y可以用`Math.pow(x, y)`函数，不过考虑到经常计算x2或x3，我们可以利用闭包创建新的函数`pow2`和`pow3`：

```javascript
'use strict';
function make_pow(n) {
    return function (x) {
        return Math.pow(x, n);
    }
}
```



## 闭包的脑洞

这块就不详细展开说了，自己写了一个简易版的

```javascript
'use strict';
// a 代表 1
var a = function (f) {
    return function () {
        return f();
    }
}
// 相加操作，相当于在做一层层的嵌套
function add(m, n) {
    return function (f) {
        // 这一层加上去的目的是保持闭包，同时它能作为一个 function 来作参数
        // 这里这一步是核心所在
        return function () {
            // 形成多层嵌套
            return m(f)(n(f)());
        }
    }
}
// 得到 2
var b = add(a, a);
// 得到 3
var c = add(a, b);
var five = add(b, c);

// 打印 5 次
five(function () { console.log("5"); })(); 


```



# 箭头函数

**ES6 标准**新增了一种新的函数：Arrow Function（箭头函数）。

```
x => x * x
```

如果参数不是一个，就需要用括号`()`括起来：

```javascript
// 两个参数:
(x, y) => x * x + y * y

// 无参数:
() => 3.14

// 可变参数:
(x, y, ...rest) => {
    var i, sum = x + y;
    for (i=0; i<rest.length; i++) {
        sum += rest[i];
    }
    return sum;
}
```



如果要返回一个对象，就要注意，如果是单表达式，这么写的话会报错：

```javascript
// SyntaxError:
x => { foo: x }
```

因为和函数体的`{ ... }`有语法冲突，所以要改为：

```javascript
// ok:
x => ({ foo: x })
```



# generator

generator 就是字面义，一个生产者，每次调用这个对象里的某个函数，就可以得到一次次的结果。其本身有点像可以多次返回的函数，通过调用 ```next()``` 来获取到下一轮结果。它的命名方式和 function 和相似，需要在后面加一个 ```*``` 

```javascript
function* fib(max) {
    var
        t,
        a = 0,
        b = 1,
        n = 0;
    while (n < max) {
        yield a;
        [a, b] = [b, a + b];
        n ++;
    }
    return;
}
```

个人觉得它的实现也是在某种程度上使用的闭包：

```javascript
var f = fib(5);
f.next(); // {value: 0, done: false}
f.next(); // {value: 1, done: false}
f.next(); // {value: 1, done: false}
f.next(); // {value: 2, done: false}
f.next(); // {value: 3, done: false}
f.next(); // {value: undefined, done: true}
```

这里获得的是 generator 对象，然后里面的变量都会存在着里面，随着 while 里一次次 yield 来实现类似暂停判断的效果，然后返回 yield 时想要的值。调用 ```next()``` 获取到的是一个数组，里面存了 value 和 done 属性。

generator对象本身，好像也是一个数组... 可以通过 `for ... of` 来获取到里面的内容：

```javascript
for (var x of fib(10)) {
    console.log(x); // 依次输出0, 1, 1, 2, 3, ...
}
```



在JavaScript的世界里，一切都是对象。

但是某些对象还是和其他对象不太一样。为了区分对象的类型，我们用 `typeof` 操作符获取对象的类型，它总是返回一个字符串：

```javascript
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```



# 对象

## 包装（装箱）

JS 里有和 Java 类似的有一个装箱，`number`、`boolean `和 `string` 都有包装对象

```javascript
var n = new Number(123); // 123,生成了新的包装类型
var b = new Boolean(true); // true,生成了新的包装类型
var s = new String('str'); // 'str',生成了新的包装类型

typeof new Number(123); // 'object'
new Number(123) === 123; // false

...
```

虽然包装对象看上去和原来的值一模一样，显示出来也是一模一样，但他们的类型已经变为`object`了！所以，包装对象和原始值用`===`比较会返回 `false`

如果不写 new 的话，其本身只是对应的 `number`、`boolean` 和 `string` 类型

```javascript
var n = Number('123'); // 123，相当于parseInt()或parseFloat()
typeof n; // 'number'
```

个人认为，上面这个 ```Number()``` 其本身只是起类型转换的作用，而不是其创建某个类型对象的作用

有这么几条规则需要遵守：

- 不要使用`new Number()`、`new Boolean()`、`new String()`创建包装对象；
- 用`parseInt()`或`parseFloat()`来转换任意类型到`number`；
- 用`String()`来转换任意类型到`string`，或者直接调用某个对象的`toString()`方法；
- `typeof`操作符可以判断出`number`、`boolean`、`string`、`function`和`undefined`；
- 判断`Array`要使用`Array.isArray(arr)`；
- 判断`null`请使用`myVar === null`；
- 判断某个全局变量是否存在用`typeof window.myVar === 'undefined'`；
- 函数内部判断某个变量是否存在用`typeof myVar === 'undefined'`。





## Date

在JavaScript中，`Date`对象用来表示日期和时间。

```javascript
var now = new Date();
now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
now.getFullYear(); // 2015, 年份
now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
now.getDay(); // 3, 表示星期三
now.getHours(); // 19, 24小时制
now.getTime(); // 1435146562875, 以number形式表示的时间戳
...
```

 **JavaScript的Date对象月份值从0开始，牢记0=1月，1=2月，2=3月，……，11=12月。所以要表示6月，我们传入的是`5`！**

创建 Date 对象两个方法：

```javascript
var d = new Date(2015, 5, 19, 20, 15, 30, 123);
d; // Fri Jun 19 2015 20:15:30 GMT+0800 (CST)

var d = Date.parse('2015-06-24T19:49:22.875+08:00');
d; // 1435146562875
```

第二种返回的不是`Date`对象，而是一个时间戳。不过有时间戳就可以很容易地把它转换为一个`Date`：

```javascript
var d = new Date(1435146562875);
d; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
d.getMonth(); // 5
```

时区问题不用考虑，只需要获取时间戳，JS 就会根据浏览器的本地的时区自动转换成当地时间：

```javascript
if (Date.now) {
    console.log(Date.now()); // 老版本IE没有now()方法
} else {
    console.log(new Date().getTime());
}
```



## RegExp

### 基础

用`\d`可以匹配一个数字，`\w`可以匹配一个字母或数字，`.`可以匹配任意字符，所以：

- `'00\d'`可以匹配`'007'`，但无法匹配`'00A'`；
- `'\d\d\d'`可以匹配`'010'`；
- `'js.'`可以匹配`'jsp'`、`'jss'`、`'js!'`等等。



用`*`表示任意个字符（包括0个）

用`+`表示至少一个字符

用`?`表示0个或1个字符

用`{n}`表示n个字符

用`{n,m}`表示n-m个字符：

- `\d{3,8}`表示3-8个数字，例如`'1234567'`。
- `\d{3}`表示匹配3个数字，例如`'010'`；



### 进阶

要做更精确地匹配，可以用`[]`表示范围，比如：

- `[0-9a-zA-Z\_]`可以匹配一个数字、字母或者下划线；
- `[0-9a-zA-Z\_]+`可以匹配至少由一个数字、字母或者下划线组成的字符串，比如`'a100'`，`'0_Z'`，`'js2015'`等等；
- `[a-zA-Z\_\$][0-9a-zA-Z\_\$]*`可以匹配由字母或下划线、$开头，后接任意个由一个数字、字母或者下划线、$组成的字符串，也就是JavaScript允许的变量名；
- `[a-zA-Z\_\$][0-9a-zA-Z\_\$]{0, 19}`更精确地限制了变量的长度是1-20个字符（前面1个字符+后面最多19个字符）。

`A|B`可以匹配A或B，所以`(J|j)ava(S|s)cript`可以匹配`'JavaScript'`、`'Javascript'`、`'javaScript'`或者`'javascript'`。

`^`表示行的开头，`^\d`表示必须以数字开头。

`$`表示行的结束，`\d$`表示必须以数字结束。



这里要多写个个人注释：

`^` 如果在正则中出现，则表示从输入的整个内容的最开始开始，`$` 则只规定了输入的整个内容的最后的部分，但是当他俩同时出现的时候，则是对一整段内容进行约束，当执行 exec() 的时候，它就会去整个内容的头和尾包括中间的部分去找，所以它不能用于全局搜索，比如：

```javascript
var s = 'JavaScript, VBScript, JScript and ECMAScript';
```

想把里面所有的 XXXScript 找出来，那么就不能用 ```^...$``` 

```javascript
var re=/^[a-zA-Z]+Script$/g;
```

这个写法里，从 Java 开始 一直到 ECMA 都可以被看做是开头部分，这里面已经有空格和逗号了，所以这已经是一整段匹配不到，最后返回一个 null。正确写法是去掉 ```^...$```，这样在搜的时候就是匹配到，在每次遇到完空格和标点之后开始去匹配。



JS 使用正则的格式

```javascript
var re1 = /ABC\-001/;
var re2 = new RegExp('ABC\\-001');

var re = /^\d{3}\-\d{3,8}$/;
re.test('010-12345'); // true
re.test('010-1234x'); // false
```



### 贪婪匹配

需要特别指出的是，正则匹配默认是贪婪匹配，也就是匹配尽可能多的字符。举例如下，匹配出数字后面的`0`：

```javascript
var re = /^(\d+)(0*)$/;
re.exec('102300'); // ['102300', '102300', '']
```

由于`\d+`采用贪婪匹配，直接把后面的`0`全部匹配了，结果`0*`只能匹配空字符串了。

必须让`\d+`采用非贪婪匹配（也就是尽可能少匹配），才能把后面的`0`匹配出来，加个`?`就可以让`\d+`采用非贪婪匹配：

```javascript
var re = /^(\d+?)(0*)$/;
re.exec('102300'); // ['102300', '1023', '00']
```



### 验证邮箱

一个简单的写法：

```javascript
var re = /^\w{2,10}(\.(\w{2,20}))?\@\w+(\.\w+)$/;
```



## JSON

JSON是JavaScript Object Notation的缩写，它是一种**数据交换格式**。SON定死了字符集必须是**UTF-8**，表示多语言就没有问题了。为了统一解析，JSON的**字符串规定必须用双引号** `""` ，**Object的键也必须用双引号** `""`。

几乎所有编程语言都有解析JSON的库，而在JavaScript中，我们可以直接使用JSON，因为**JavaScript内置了JSON的解析**。把任何JavaScript对象变成JSON，就是把这个对象序列化成一个JSON格式的字符串，这样才能够通过网络传递给其他计算机。

如果我们**收到一个JSON格式的字符串**，只需要把它**反序列化成一个JavaScript对象**，就可以在JavaScript中直接**使用这个对象**了。



### 序列化

```javascript
var s = JSON.stringify(object);
```

要输出得好看一些，可以加上参数，按缩进输出：

```javascript
var s = JSON.stringify(xiaoming, null, "  ");
```

第二个参数可以传的有很多，比如可以传一个 Array 进去，里面填写想获得的 key 的名字：

```javascript
var s =  JSON.stringify(xiaoming, ["name", "age"], "  ");
```

还可以传入一个函数，这样对象的每个键值对都会被函数先处理：（下面是把所有属性值变大写）

```javascript
function convert(key, value) {
    if (typeof value === 'string') {
        return value.toUpperCase();
    }
    return value;
}

var s = JSON.stringify(xiaoming, convert, "  ");
```

如果我们还想要精确控制如何序列化小明，可以给`xiaoming`定义一个`toJSON()`的方法，直接返回JSON应该序列化的数据：

```javascript
// 在 xiaoming 的对象里添加一个这样的方法
toJSON: function () {
    return { // 只输出name和age，并且改变了key：
        'Name': this.name,
        'Age': this.age
    };
}
```



### 反序列化

拿到一个JSON格式的字符串，我们直接用`JSON.parse()`把它变成一个JavaScript对象：

```javascript
JSON.parse('[1,2,3,true]'); // [1, 2, 3, true]
JSON.parse('{"name":"小明","age":14}'); // Object {name: '小明', age: 14}
JSON.parse('true'); // true
JSON.parse('123.45'); // 123.45
```

`JSON.parse()`还可以接收一个函数，作为第二个参数，用来转换解析出的属性：



### 调用API



## 对象

JavaScript不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程。JavaScript的原型链和Java的Class区别就在，它没有“Class”的概念，所有对象都是实例，所谓继承关系不过是**把一个对象的原型指向另一个对象**而已。

![xiaoming-prototype](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/l)





在编写JavaScript代码时，不要直接用`obj.__proto__`去改变一个对象的原型，并且，低版本的IE也无法使用`__proto__`。`Object.create()`方法可以传入一个原型对象，并创建一个基于该原型的新对象

```javascript
var Person = {
    name: "default",
    age: 0,
    run: function () {
        console.log(this.name + "is running");
    },
    getAge: function () {
        return this.age;
    }
}

function creatPerson(name) {
    // 这个 p 其实感觉就是复制了一份 Person，然后自己手写这个方法来充当构造函数
    var person = Object.create(Person);
    person.name = name;
    return person;
}

var p = creatPerson("Jade");
p.run();
console.log(p.getAge());
```



### 创建对象

创建对象的方法有很多，可以通过大括号包含，然后键值对的方式来创建：

```javascript
var person = {
    name: "张三",
    age: 20,
    postion: "前端工程师",
    signIn: function() {
        console.log("张三打卡");
    }
}
```

也可以先通过 ```new``` 关键字，然后赋予属性值和方法：

```javascript
var person = new Object();
person.name = "Jobs";
person.signIn = function() {
    console.log("打卡");
}
```



### 遍历对象属性

可以通过 ```Object.keys()``` 这个方法来获取到一个键的数组：

```javascript
var keyArr = Object.keys(person);
```

也可以用 ```for...in``` 的方式来得到每一个键值

```javascript
for (key in person) {
    console.log(key);
}
```



删除一个属性直接用 ```delete``` 就可以

```javascript
delete person.name;
```



### setter & getter

创建对象时添加 getter 和 setter：

```javascript
const person = {
    firstName: "三",
    lastName: "张",
    get fullName() {
        return this.lastName + this.firstName;
    },
    set fullName(fullname) {
        let [lastname, firstname] = fullname.split(",");
        this.lastName = lastname;
        this.firstName = firstname;
    }
}

console.log(person.fullName);
person.fullName = "李,四";
console.log(person.fullName);
```

在使用构造函数时候，添加 getter 和 setter：

```javascript
function Employee(name, position) {
    this.name = name;
    this.position = position;
}

const emp = new Employee("张三", "前端工程师");

Object.defineProperty(emp, "info",  {
    get: function() {
        return this.name + " " + this.position;
    },
    set: function(info) {
        let [name, position] = info.split(" ");
        this.name = name;
        this.position = position;
    }
})

console.log(emp.info);
emp.info = "赵四 后端工程师";
console.log(emp.info);


```





### 原型

这个“原型”，我个人猜测是一个看不见的父类，这个Student函数在调用 new 的时候，才会产生这个原型，原型的属性是通过Student中的具体内容而生成的。后面每一个生成的“实例对象”都会按照这个原型进行“一比一复制”，这个原型里有什么属性就会“复制”过来什么属性，有什么方法就会“复制”过来什么方法，同时在这个原型里，会在调用 new 之后，添加一个名为 constructor 的属性（函数），即生成某个对象的构造函数，该函数的最大作用是初始化！且要注意的是，是对创建的 “实例对象” 初始化！上一些代码来做详细说明：

```javascript
function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}

var xiaoming = new Student('小明');
xiaoming.name; // '小明'
xiaoming.hello(); // Hello, 小明!
```

这里的 Student 构造函数实际上就是对 xiaoming 对象其初始化的作用，Student 就理解成是对 “Student” 这个 “类” 的构造函数就可以了。那这个原型，到底长啥样的？以下是我个人对其大致的猜测：

```javascript
var prototype = {
    name: undefined,
    hello: undefined,
    constructor: Student(name)
}

function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}
```

其原型的产生时间点还猜不到，感觉应该是一个函数定义出来的时候，其原型就会存在了吧。new 之后就会产生这样一个唯一原型，也可以说是唯一 “父类”，在创建对象的时候，调用 constructor，也就是 Student 这个（构造）函数，然后在里面对 name 和 hello 进行初始化。所以，它的实现本质就是使用了闭包！

![image-20200809020228423](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/image-20200809020228423.png)

另外，函数`Student`恰好有个属性`prototype`指向`xiaoming`、`xiaohong`的原型对象，但是`xiaoming`、`xiaohong`这些对象可没有`prototype`这个属性，不过可以用`__proto__`这个非标准用法来查看。

这里再加一下个人见解：之所以让 Student 去获取 prototype 的原因是因为，按照正常的逻辑只有父类自己才能修改自己，而实例对象是没权利修改这个原型的，所以说，实例对象不提供 xiaohong 这个 prototype 这个属性。但是，通过所谓的 `__proto__` 来查看，我个人觉得应该是利用了类似 Java 映射的原理，强行查看到的。就有点像 Java 中使用 ```.class()``` 获取其字节码文件一个道理，应用到这面就很好理解了。

另外还要说一下共享方法的问题：

```javascript
xiaoming.name; // '小明'
xiaohong.name; // '小红'
xiaoming.hello; // function: Student.hello()
xiaohong.hello; // function: Student.hello()
xiaoming.hello === xiaohong.hello; // false
```

这里生成俩对象，但是他俩的 hello 方法确实不一样的，所以说创建对象的时候，这个方法都是被各自所拥有，可是他俩代码是一样的，为了优化，可以把这个方法在 Student 构造函数里删掉，然后手动添加到 Student 的原型里。这样，虽然实例对象（子类）中没有这个叫 hello 的方法，它就会去它的原型（父类）里去找。

```javascript
function Student(name) {
    this.name = name;
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
};
```

其原型内的内容大概就会变成这样：

```javascript
var prototype = {
    name: undefined,
    hello: function () {
    alert('Hello, ' + this.name + '!');}，
    constructor: Student(name),
}

function Student(name) {
    this.name = name;
}
```

注意一点，这样写的话，实例对象中是没有 hello 这个方法的！为了区分普通函数和构造函数，按照约定，构造函数首字母应当大写，而普通函数首字母应当小写，这样，一些语法检查工具如[jslint](http://www.jslint.com/)将可以帮你检测到漏写的`new`。



### createXXX()

继续上面的例子说，分开是因为，个人觉得这块内容又涉及到另外的知识了。

我们还可以编写一个`createStudent()`函数，在内部封装所有的`new`操作。一个常用的编程模式像这样：

```javascript
function Student(props) {
    this.name = props.name || '匿名'; // 默认值为'匿名'
    this.grade = props.grade || 1; // 默认值为1
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
};

function createStudent(props) {
    return new Student(props || {})
}
```

上面这个 props 传进来的是一个对象！注意是一个对象！

这个`createStudent()`函数有几个巨大的优点：一是不需要`new`来调用，二是参数非常灵活，可以不传，也可以这么传：

```javascript
var xiaoming = createStudent({
    name: '小明'
});

xiaoming.grade; // 1
```

如果创建的对象有很多属性，我们只需要传递需要的某些属性，剩下的属性可以用默认值。由于参数是一个Object，我们无需记忆参数的顺序。如果恰好从`JSON`拿到了一个对象，就可以直接创建出`xiaoming`。

再加一下个人的小总结：这么写的话，反序列化的时候就会非常方便，得到某一个“类”的实例对象之后，就直接把它传进来，就可以直接生成一个一模一样的对象。





### 原型继承

上面如果理解了，这块估计也差不多了，直接上其实现的大概情况：

```javascript
var prototype = {
    name: undefined,
    hello: undefined,
    grade: undefined,
    constructor: PrimaryStudent(props)
}

function PrimaryStudent(props) {
    Student.call(this, props);
    this.grade = props.grade || 1;
}

function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}
```

具体的实现过程：

```javascript
// PrimaryStudent构造函数:
function PrimaryStudent(props) {
    Student.call(this, props);
    this.grade = props.grade || 1;
}

// 空函数F:
function F() {
}

// 把F的原型指向Student.prototype:
F.prototype = Student.prototype;

// 把PrimaryStudent的原型指向一个新的F对象，F对象的原型正好指向Student.prototype:
PrimaryStudent.prototype = new F();

// 把PrimaryStudent原型的构造函数修复为PrimaryStudent:
PrimaryStudent.prototype.constructor = PrimaryStudent;

// 继续在PrimaryStudent原型（就是new F()对象）上定义方法：
PrimaryStudent.prototype.getGrade = function () {
    return this.grade;
};

// 创建xiaoming:
var xiaoming = new PrimaryStudent({
    name: '小明',
    grade: 2
});
xiaoming.name; // '小明'
xiaoming.grade; // 2
```

一开始用一个空函数的原型对象指向原 Student 的原型对象（有constructor），所以再 new F() 的时候，其实是创建了一个空的 Student 对象，因为没参数，所以 name 和 hello 依旧是undefined，继承在这时候应该就算已经发生了。再之后把 PrimaryStudent 的原型里，添加上 constructor 函数，就成了！而后面那个 getGrade 只是再起原型基础上继续添加新方法而已。原型里的 grade 这个属性，是在 PrimaryStudent 这个构造函数里，直接新创建一个属性给其原型。

如果把继承这个动作用一个`inherits()`函数封装起来，还可以隐藏`F`的定义，并简化代码：

```javascript
function inherits(Child, Parent) {
    var F = function () {};
    F.prototype = Parent.prototype;
    Child.prototype = new F();
    Child.prototype.constructor = Child;
}
```



### 小节

JavaScript的原型继承实现方式就是：

1. 定义新的构造函数，并在内部用`call()`调用希望“继承”的构造函数，并绑定`this`；
2. 借助中间函数`F`实现原型链继承，最好通过封装的`inherits`函数完成；
3. 继续在新的构造函数的原型上定义新方法。





## class

class 的使用基本和 Java 一毛一样，直接上代码就完事了：

```javascript
class Student {
    constructor(name) {
        this.name = name;
    }

    hello() {
        alert('Hello, ' + this.name + '!');
    }
}
```

继承的话就是：

```javascript
class PrimaryStudent extends Student {
    constructor(name, grade) {
        super(name); // 记得用super调用父类的构造方法!
        this.grade = grade;
    }

    myGrade() {
        alert('I am at grade ' + this.grade);
    }
}
```

也是用 super 给父类的构造函数传参，特有的用 this.XXX 创建并赋值。

**注意：**因为不是所有的主流浏览器都支持ES6的class。如果一定要现在就用上，就需要一个工具把`class`代码转换为传统的`prototype`代码，可以试试[Babel](https://babeljs.io/)这个工具。