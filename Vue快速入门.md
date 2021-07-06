



# 快速开始

创建 Vue 实例可以设置的选项：

```
https://cn.vuejs.org/v2/api/#%e9%80%89%e9%a1%b9-%e6%95%b0%e6%8d%ae
```

其中有对 data，methods 等选项的进一步说明



## Hello World

首先导入**开发版本**的 Vue.js

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

创建 Vue 实例对象，设置 **el** 属性和 **data** 属性：

```html
<div id="app">
    {{ message }}
</div>

<script>
    // 声明式编程
    var app = new Vue({
        el: "#app",
        data: {
            message: "Hello Vue!",
        }
    })
</script>
```

**使用简洁的模板语法把数据渲染到页面上**



## 列表展示

```html
<body>
    <div id="app">
        <ul>
            <li v-for="item in movies">{{item}}</li>
        </ul>
    </div>

    <script>
        const app = new Vue({
            el: '#app',
            data: {
                message: 'Hello',
                movies: ['A', 'B', 'C']
            }
        })
    </script>
</body>
```

在控制台可以通过 ```app.movies.push("Name")``` 的方式实现响应式添加



## 计数器

```html
<body>

    <div id="app">
        当前计数：{{counter}}
        <br>
        <!-- <button v-on:click="increment">+</button> -->
        <!-- <button v-on:click="decrement">-</button> -->

        <button @click="counter++">+</button>
        <button @click="counter--">-</button>
    </div>

    <script>
        const app = new Vue({
            el: '#app',
            data: {
               counter: 0
            },
            methods: {
                increment: function () {
                    console.log('counter++ 执行...');
                    this.counter++;
                },
                decrement: function () {
                    console.log('counter-- 执行...');
                    this.counter--;
                }
            }
        })

    </script>

</body>
```





# MVVM

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20210417122045.png)

**View 层：**

- 视图层
- 在前端开发中，也就是 DOM 层
- 主要展示信息

**Model 层：**

- 数据层
- 可以是 fixed data，也可以是服务器请求来的

**ViewModel 层：**

- 视图模型层
- View 和 Model 沟通的桥梁
- 实现了 Data Binding，可以把 Model 的改变实时反映到 View 中
- 实现了 DOM Listener，监听各类事件，回调函数





# 基础选项

选项就是创建 Vue 实力对线的时候，设定的一些属性，比如 el 挂载点，data，methods 等



- **el：**
  - 类型：string | HTMLElement
  - 作用：绑定 DOM 节点
- **data：**
  - 类型：Object | Function（组件中的 data 必须是一个函数）
  - 作用：Vue 实例对应的数据
- **methods：**
  - 类型：{ [key : string] : Function}
  - 作用：定义属于 Vue 实例对象的方法



# 生命周期

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/lifecycle.png)

在不同的生命周期中，可以通过调用一些已经内置好的回调函数，来实现不同生命周期时，执行特定的代码，图中红色框的部分，都可以让我们来实现自己的回调

```javascript
cont app = new Vue({
    el: '#app',
    data: {
        // data...
    },
    methods: {
        // methods...
    },
    beforeCreate: function() {
        console.log('before create...');
    },
    created: function() {
        console.log('created...');
    },
    mounted: function() {
        console.log('mounted...');
    }
    
})
```



# 基础语法



## 插值操作

把 Vue 对象中的数据，插入到 DOM 节点中，在 Vue 中的 ```{{}}``` 属于 Mustache 语法，Mustache 中文释义为胡须

Mustache 语法中，不仅可以写变量，还可以写简单的表达式

```html
<div id="app">
  {{message}}
  {{firstname + ' ' + lastname}}
  {{counter * 2}}
</div>
```



插值操作中有一些指令可以对值进行操作

1. **v-once**：固定得到的值，取消其响应式

   ```html
   <!-- 更改 message 的值，显示不会有改变 -->
   <h2 v-once>{{message}}</h2>
   ```

   

2. **v-html**: 解析 HTML 字符串

   有时后端会之间传来一串这样的字符串：

   ```javascript
   url: '<a href="https://www.baidu.com">百度一下</a>'
   ```

   这时候需要正确的解析，需要在标签上添加该指令

   ```html
   <h2 v-html="url"></h2>
   ```

   

3. **v-text**: 和 {{}} 的使用基本一样

   ```html
   <h2 v-text="message"> content </h2>
   ```

   比较少用这个，，同时标签内 content 会被覆盖

   

4. **v-pre**: 标签内有什么就展示什么，清除所有其他的特殊语法

   ```html
   <!-- 这个正常解析 message 的值 -->
   <h2>{{message}}</h2>
   <!-- 这个展示出来的就是 {{message}} 在页面上 -->
   <h2 v-pre>{{message}}</h2>
   ```

   

5. **v-cloak**: 这是一个需要手动设置的标签属性，其主要作用是，防止 JS 代码加载缓慢而导致不友好内容展示给用户。当 Vue 实例对象找到对应节点后，会去除掉这一条属性

   ```html
   <head>
     <style>
       [v-cloak] {
         display: none;
       }
     </style>
   </head>
   <body>
   
   <div id="app" v-cloak>
     <h2>{{message}}</h2>
   </div>
   ```



## 属性绑定

绑定属性主要用来动态的修改标签的某些属性，比如动态的修改 a 标签的 href 属性，动态绑定 img 标签的 src 属性等等



- v-bind：绑定 Vue 实例中的某一条数据到对应的属性上

  ```html
  <!-- imgSrc 为 Vue 实例的 data 中的一条数据 -->
  <img v-bind:src="imgSrc" alt="">
  ```

- 语法糖：由于其使用频率很高，有很简介的写法

  ```html
  <!-- imgSrc 为 Vue 实例的 data 中的一条数据 -->
  <img :src="imgSrc" alt="">
  ```



对于绑定 class 上，有一个进阶使用，叫做对象语法，在 bind 后面传入一个对象的形式来决定是否要添加多个属性，**我们手动写上去的 class 不会被 v-bind 覆盖**

```html
<head>
  <style>
    .active {
      color: lightpink;
    }
    .move {
      margin-left: 100px;
    }
  </style>
</head>

<div id="app">
  <h2 class="red" :class="classes">{{message}}</h2>
</div>

<script src = "../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: 'Hello World!',
      classes: {
        active: true,
        move: false
      }
    }
  })
</script>
```



## 简单排他

初始默认第一个具有特定的样式，后面点到哪个元素，哪个元素具有该样式，且其他的元素没有

```html
<head>
  <style>
    .actived {
      color: red;
    }
  </style>
</head>
<body>

<div id="app">
  <ul>
    <li v-for="(item, index) in movies"
        :class="{actived:active===index}"
        @click="switchActivation(index)"
        >{{item}}</li>
  </ul>
</div>

<script src = "../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: 'Hello World!',
      movies: ['海尔兄弟', '葫芦娃', '火影忍者', '进击的巨人'],
      active: 0
    },
    methods: {
      switchActivation: function (index) {
        this.active = index;
      }
    }
  })
</script>

</body>
```



## 样式绑定

**样式的绑定和属性绑定也非常非常相似**，只不过当使用对象语法的时候，需要注意的是，如果这个对象里面的键值对是 ```{key:value}``` 的形式的话，这个 value 会去 Vue 实例对象中找对应的一个叫 value 的变量；而如果我们把这个键值对的 value 使用单引号括起来的时候，就会直接传值进去

```html
<body>

<div id="app">
  <h2 :style="{color: red}">{{message}}</h2>
</div>

<script src = "../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: 'Hello World!',
      red: 'red'
    },
    methods: {

    }
  })

</script>

</body>
```



## 计算属性

计算属性为创建 Vue 实例中的一个选项，这里面储存的是 function，其本身是对 data 进行一些简单的处理，而不是把复杂的表达式写在 Mustache 表达式中，但是其本身所想代表的概念是一个属性，而不是 function，所以每个 function 起的名字都为名词，而非动词

```html
<body>

<div id="app">
  <h2>{{firstName + ' ' + lastName}}</h2>
  <h2>{{getFullName()}}</h2>
  <!-- 这个 fullName 就是一条已经被经过处理的属性 -->
  <h2>{{fullName}}</h2>
</div>

<script src = "../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: 'Hello World!',
      firstName: 'Bob',
      lastName: 'Smith'
    },
    computed: {
      fullName: function () {
        return this.firstName +  ' ' + this.lastName;
      }
    },
    methods: {
      // 简写方式
      getFullName: function () {
        return this.firstName + ' ' + this.lastName;
      }
    }
  })

</script>

</body>
```

**计算属性调用一次后，数据会得到缓存！效率高！**

就像面向对象一样，每条属性也会有对应的 accesser 和 mutater 来获取和修改属性值，所以每条计算属性的完整写法不仅仅有其对应的处理数据的函数，也要有 getter 和 setter（通常会只写 getter ）：

```javascript
computed: {
  fullName: {
    set: function (arg) {

    },
    get: function () {
      return this.firstName + ' ' + this.lastName;
    }
  }
}
```

一旦 get 中的任意属性值发生改变的话，get 方法会被重新调用



## 事件监听

监听用户的各种操作，实现程序和用户之间的交互，在 Vue 中通过 ```v-on``` 指令来实现，语法糖为 ```@```

（基本使用的 demo 已经在快速开始中）

监听时有时会希望传参，传参主要就分为两个情况：

1. 不传入 ```$event``` 对象：这种时候就正常该怎么用怎么用，监听事件后面填写的函数后面需要跟有括号，不传参的话就都会显示 undefined

   ```html
   <button @click="btn_1_click()">按钮 1</button>
   <button @click="btn_2_click">按钮 2</button>
   <button @click="btn_3_click(123)">按钮 3</button>
   ```

   

2. 传入 ```$event``` 对象：如果监听事件后面绑定的函数没有加括号，且该函数需要传参的话，Vue 会默认把 event 对象作为第一个参数传入，后面的都为 undefined；若想手动获取到 event 对象，只需要手动传入 ```$event``` 即可

   ```html
   <!-- 这种若函数 btn_4_click 有参数，则默认传入 event 对象 -->
   <button @click="btn_4_click">按钮 4</button>
   <!-- 手动传入 event 对象 -->
   <button @click="btn_5_click($event, 123)">按钮 5</button>
   ```

   

```v-on``` 还有一些修饰符来实现一些特殊的功能：

1.  stop，阻止事件冒泡：

   想在哪一个节点终止冒泡，就在哪个节点添加 stop 修饰符

   ```html
   <div id="app">
     <div @click="div_2_Click()">
       <div @click.stop="div_1_Click()">
         <button @click="btnClick()">冒泡</button>
       </div>
     </div>
   </div>
   ```

   

2. prevent，阻止默认事件发生：

   比如要阻止表单提交这个默认事件，使用自己定义好的函数来实现提交

   ```html
   <form action="/api">
     <!-- 若不添加 prevent 修饰，则点击提交后（本地运行）直接 404 -->
     <input type="submit" value="提交" @click.prevent="submitClick()">
   </form>
   ```

   

3. {keyCode | keyAlias}，事件从特定键位触发，比如用户按回车的时候

   ```html
   <input type="text" @keyup.enter="keyUp()">
   ```

   

4. native，监听组件根元素的原生事件（后头再写，现在不会）

5. once，只触发一次，

   ```html
   <button @click.once="btnClick()"></button>
   ```

   



## 条件判断

通过条件判断来决定某些内容是否显示，最基本的可以通过 ```v-if``` 指令实现：

```html
<!-- 当 message 的长度大于 10 才会显示 -->
<h2 v-if="message.length > 10">{{message}}</h2>
```



还可以通过 ```v-else``` 来决定，如果 `v-if` 的条件不满足的时候要显示什么：

```html
<h2 v-if="message.length > 10">{{message}}</h2>
<h2 v-else>Hello Vue!</h2>
```



当遇到更复杂的情况时候，可以再通过 `v-else-if` 来进行多条件判断：

```html
<h2 v-if="score >= 90">优秀</h2>
<h2 v-else-if="score >= 80">良好</h2>
<h2 v-else-if="score >= 60">及格</h2>
<h2 v-else>不及格</h2>
```

不过当遇到这种复杂的判断的时候，可以在计算属性（computed attribute）中先判断出结果，然后直接显示



`v-show` 也可以实现决定内容的显示还是不显示，但是它和 `v-if` 不同的是，`v-show` 通过修改 display 样式的方式来控制显示还是不显示，而 `v-if` 是在 DOM 节点层次上进行修改。

> 如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。



## 循环

循环没啥说的...

遍历对象：

```html
<li v-for="(value, key, index) in object">{{index}}.{{key}}.{{value}}</li>
```





## 响应式方法

- push
- pop
- shift（删除第一个）
- unshift
- splice
- sort
- reverse

- ...

之所以是响应式，是因为 Vue 监听了这些函数，一旦函数触发就会去检查数据是否变化。Vue 实例自己提供了一个 set 方法，可以使用来修改单独的某一个元素，同时还能实现响应式：

```javascript
Vue.set(obj, index, value);	
```



# 三个 JS 高阶函数

这里的三个高阶函数实际就是函数式，和 Haskell 非常像

1. filter 函数：

   把一个数组内的每一个元素按照特定的条件进行筛选过滤，最后返回一个新的数组

   ```javascript
   let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
   let newArr = arr.filter(n => n < 5);
   ```

   return 后面的条件为 true 则保留该元素，反之不会加到新的数组里

   

2. map 函数：

   map 此处取映射的含义比较好理解，即传入一个回调函数，并将该函数 map 到每一个元素上

   ```javascript
   let arr = [1, 2, 3, 4];
   // 对每一个元素进行乘 2 的操作
   let newArr = arr.map(n => n * 2);
   ```

   

3. reduce 函数：

   reduce 函数取第一个元素和下一个函数，并且将 return 的结果最为下一次回调的第一个元素

   ```javascript
   let arr = [1, 2, 3, 4, 5];
   let sum = arr.reduce((pre, next) => pre + next， 0)
   ```

   上面是求一个数组的和，reduce 函数的第二个参数为第一轮的初始值，这时候第一轮的 next 就会是 arr[0]



# 表单绑定 v-model

Vue 中使用 `v-model` 指令来实现表单元素和数据之间的双向绑定，主要用在注册页面



## 快速使用

```html
<div id="app">
  <input type="text" v-model="message">
  <h2>{{message}}</h2>
</div>

<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: 'Hello World!',
    }
  })
</script>
```

这里 message 和 input 中的值绑定在一起，无论在控制台修改还是在 input 中修改，h2 标签中的内容都都会跟着一起变

v-model 是可以通过一个数据绑定和一个监听来实现的：

```html
<div id="app">
  /* 
   * v-bind 用于把 Vue 实例中的 message 值渲染到页面上
   * v-on 来监听 input 值的变化，并调用 valueChange 方法来修改实例中 message 中的值
   */
  <input type="text" :value="message" @input="valueChange">
  <h2>{{message}}</h2>
</div>

<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: 'Hello World!',
    },
    methods: {
      valueChange(event) {
        this.message = event.target.value;
      }
    }
  })

</script>
```



 ## v-model 和 radio 类型

在使用 v-model 做同一个数据的双向绑定的时候，不需要写 name 属性即可实现单选互斥

```html
<div id="app">
  <label for="">
    <input type="radio" value="男" v-model="gender">男
    <input type="radio" value="女" v-model="gender">女
  </label>
  <h3>您选择的性别是：{{gender}}</h3>
</div>

<script>
  const app = new Vue({
    el: '#app',
    data: {
      gender:'男'
    }
  })
</script>
```

以上 value 值必须有，且 v-model 绑定的数据必须一致，若 Vue 实例中的 gender 有值，且值正确的话，则可以实现页面打开时候默认选择其中一个选项的效果；如果值没有对一个上 value 属性的话，则什么都不选



## v-model 和 checkbox 类型

v-model 结合 checkbox 类型的表单做**单选**，比如在注册页面必须要求同意注册协议：

```html
<div id="app">
  <label for="licence">
    <input type="checkbox" id="licence" v-model="isAgree">同意协议
  </label>
  <button :disabled="!isAgree">注册</button>
</div>

<script>
  const app = new Vue({
    el: '#app',
    data: {
      isAgree: false
    }
  })
</script>
```



v-model 结合 checkbox 类型的表单做多选，比如选择城市、爱好等等：

```html
<div id="app">
  <label for="beijing">
    <input type="checkbox" id="beijing" value="北京" v-model="cities">北京
  </label>
  <label for="shenzhen">
    <input type="checkbox" id="shenzhen" value="深圳" v-model="cities">深圳
  </label>
  <label for="guangzhou">
    <input type="checkbox" id="guangzhou" value="广州" v-model="cities">广州
  </label>
</div>

<script>
  const app = new Vue({
    el: '#app',
    data: {
      cities: []
    }
  })
</script>
```

这里元素添加的方式是采用 append 的方式追加到 cities 中，先选的在前面，后选的在后面



简单来说，对应的是一个值的时候，不需要 value 属性，且在 Vue 实例中需要一个 bool 类型数据来做记录；做多选的时候需要在选项上添加 value 属性，在 Vue 实例中需要一个数组类型数据来储存



做复选框的时候，可以把一些原始数据交给 Vue 实例来管理，然后使用循环的方式把数据展示出来：

```html
<div id="app">
  <label v-for="item in cities" :for="item">
    <input type="checkbox" :id="item" :value="item" v-model="selectedCities">{{item}}
  </label>
</div>

<script>
  const app = new Vue({
    el: '#app',
    data: {
      cities: ['北京', '上海', '广州', '深圳'],
      selectedCities: []
    },
  })
</script>
```





## v-model 结合 select 类型

select 也分为单选和多选两种情况

单选情况需要把 v-model 写在 select 标签中：

```html
<div id="app">
  <select name="fruit" id="" v-model="fruit" multiple >
    <option value="苹果">苹果</option>
    <option value="香蕉">香蕉</option>
    <option value="西瓜">西瓜</option>
    <option value="葡萄">葡萄</option>
  </select>
  <h3>你选中的水果是：{{fruit}}</h3>
</div>

<script>
  const app = new Vue({
    el: '#app',
    data: {
      fruit:''
    }
  })
</script>
```



多选的情况下，只需要在 select 标签中添加 **multiple 属性**，并且使用**数组来储存**即可



## 修饰符

1. **lazy 修饰符：**

   在数据双向绑定之后，数据会实时根据变化而变化，通过 lazy 修饰符可以让数据在失去焦点或按下回车后进行数据更新

   ```html
   <input type="text" v-model.lazy="message">
   <h3>{{message}}</h3>
   ```

   

2. **number 修饰符：**

   有时 input 只允许用户输入数字，这时 input 的类型可以写成 number，但是在这时进行双向绑定的时候，用户输入完数字后，数据会变成字符串类型，实际就是变量重定向了。如果，我们希望这个值是数字类型，而非字符串类型的时候，就可以加上 number 修饰符：

   ```html
   <div id="app">
     <!--1. lazy 修饰符-->
     <input type="number" v-model.number="age">
     <h3>{{message}} -- {{typeof message}}</h3>
   </div>
   
   <script>
     const app = new Vue({
       el: '#app',
       data: {
         // 这里即使一开始是个字符串，一旦数据发生改变，其会替换成数字类型
         age: '0'
       }
     })
   </script>
   ```

   

3. **trim 修饰符：**

   去除用户在两边输入的多余的空格

   

**修饰符可以串联使用**














