组件化让我们开发出一个个独立可服用的小组件来构建应用，有更好的维护性和扩展性

![image-20210405063614345](R:/mdImages/image-20210405063614345.png)

例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件

使用组件主要有三个步骤：

- 创建组件构造器  -- `Vue.extend()`
- 注册组件 -- `Vue.component()`
- 使用组件



# 快速开始

通过 `Vue.extend` 来获取创建组件：

```javascript
const cpnC = Vue.extend({
  template: `
    <div>
      <h1> H1 标题</h1>
      <h2> H2 标题</h2>
      <h3> H3 标题</h3>
    </div>`
})
```



注册组件（这里使用全局注册的方式做示范）

```javascript
Vue.component('my-cpn', cpnC);
```



使用组件：

```html
<div id="app">
  <my-cpn></my-cpn>
  <my-cpn></my-cpn>
  <my-cpn></my-cpn>
</div>
```



# 全局注册和局部注册

通过 `Vue.component()` 方式注册为**全局注册**，在任何 Vue 实例中都可以进行使用，比如像下面这样：

```html
<div id="app1">
  <my-cpn></my-cpn>
</div>

<div id="app2">
  <my-cpn></my-cpn>
</div>
```



而**局部注册**，只允许在特定的一个实例中使用：

```javascript
const app = new Vue({
  el: '#app',
  data: {
    message: 'Hello World!',
  },
  components: {
    // 左侧为使用组件的标签名，右侧为创建的组件
    // 带有符号的组件名，比如横杠，可以左侧使用字符串来
    cpn: cpnA,
    "my-cpn": cpnB
  }
})
```



通常会使用局部注册，因为在一个项目中通常也只有一个 Vue 实例



## 语法糖写法

`Vue.extend()` 可以省略不写，直接把组件作为参数传给 `Vue.component()` 中，或者直接赋到组件里



全局：

```javascript
Vue.component('my-cpn', {
  template: `
  <div>
    <h1> H1 标题</h1>
    <h2> H2 标题</h2>
    <h3> H3 标题</h3>
    <cpn2></cpn2>
  </div>`
});
```



局部：

```javascript
const app = new Vue({
  el: '#app',
  components: {
    cpn1: {
      template: `
        <div>
          <h1> H1 标题</h1>
          <h2> H2 标题</h2>
          <h3> H3 标题</h3>
          <cpn2></cpn2>
        </div>`
    }
  }
})
```





# 组件模板抽离

避免 template 中的内容让代码看着很混乱，可以把 template 中的内容抽离出来



**通过 script 标签抽离**（注意：类型需要是 `text/x-template`）

```html
<script type="text/x-template" id="cpn">
  <div>
    <h1> H1 标题</h1>
    <h2> H2 标题</h2>
  </div>
</script>

<script>
  Vue.component('cpn', {
    template: "#cpn"
  })
</script>
```



**通过 template 标签**：

```html
<template id="cpn">
  <div>
    <h1> H1 标题</h1>
    <h2> H2 标题</h2>
  </div>
</template>
```



（不过这样的写法都是写死的内容）



# 组件中的数据

组件假如就算可以访问到 Vue root 的数据，但是不推荐这样做，因为这样会让 Vue root 变得越来越臃肿，所以 Vue 组件应该有自己保存数据的地方

所以，在组件中也有 data 这条选项，但是 **data 必须为函数，而且函数需要返回一个对象，对象内部保存数据**

```javascript
Vue.component('cpn', {
  template: '#cpn',
  data() {
    return  {
      title: 'Hello World!'
    }
  }
})
```



组件的原型实际上就是指向的 Vue 对象，它也有同样的生命周期



关于 data 为什么一定要是一个 function 是因为要**保证每个组件的数据是独立的**，比如做一个计数器的组件，不同的计数器之间的值是不可以互相干扰的，所以在创建组件实例的时候，data 函数会 return 数据给每一个组件，让数据独立





# 父子组件

父组件可以在其 template 中使用子组件中的内容，从组件树的结构可以看出，当一个大的组件下面有多个小组件分支的时候，就形成了这样的父子组件的关系



## 创建父子组件

首先创建一个组件：

```javascript
const cpnC1 = Vue.extend({
  template: `
    <div>
      <h1> 我是组件 1 </h1>
    </div>`
});
```



然后创建第二个组件，并且在第二个组件中注册第一个组件：

```javascript
const cpnC2 = Vue.extend({
  template: `
    <div>
      <h1> 我是组件 2 </h1>
      <cpn1></cpn1>
    </div>`,
  components: {
    cpn1: cpnC1
  }
});
```



之后在 Vue 实例中注册 `cpnC2` 就可以正确的使用他们；因为组件 1 已经在组件 2 中注册，他们之间已经形成这种子父关系，此时只需要把组件 2，也就是父组件，注册到 Vue 实例上（也就是 root 组件）即可

若想单独使用子组件，那么还需要把子组件注册到 Vue 实例上



## 父传子通信

在一个页面中，我们从服务器请求到了很多的数据，其中一部分数据，并非是我们整个页面的大组件来展示的，而是需要下面的子组件进行展示。这个时候，并不会让子组件再次发送一个网络请求，而是直接让大组件（父组件）将数据传递给小组件（子组件）



**父组件通过 props 给子组件传递数据：**

首先注册组件，并且绑定好 template

```html
<template id="cpn">
  <div>
    <!-- 在模板中用 props 声明的变量名来使用 -->
    {{cfruit}}
  </div>
</template>

<script>
  const app = new Vue({
    el: '#app',
    data: {
      fruits: ['苹果', '橘子', '香蕉', '西瓜']
    },
    components: {
      'cpn': {
        template: '#cpn',
        // 这个 props 里用字符串的形式声明变量名
        props: ['cfruit']
      }
    }
  })
</script>
```



然后传输（绑定）数据的操作是在创建组件实例的时候完成的

```html
<div id="app">
  <cpn :cfruit="fruits"></cpn>
</div>
```



上面这种写法在实际使用中并不是很好用，所以 props 可以通过对象的方式来接数据，不但可以**限制数据类型**，还可以在父组件没有传数据来的时候有**默认值**，还可以规定是不是**必须传入**：

```javascript
const app = new Vue({
  el: '#app',
  data: {
    fruits: ['苹果', '橘子', '香蕉', '西瓜']
  },
  components: {
    'cpn': {
      template: '#cpn',
      props: {
        // 1. 变量名
        cfruit: {
          // 2. 限制类型，这里可以用数组允许多类型
          type: Array,
          // 3. 默认值（注意：对象或数组默认值必须从一个工厂函数获取）
          default: function() {
            return ['苹果', '榴莲', '葡萄', '草莓'];
          }
          // 4. 规定在使用这个组件的时候，不必须传入值给这个组件
          required: false
        }
      }
    }
  }
})
```



接受的数据还可以做数据验证：

```javascript
// 下面的内容是在 props 中的

propF: {
  validator: function (value) {
    // 这个值必须匹配下列字符串中的一个
    return ['success', 'warning', 'danger'].indexOf(value) !== -1
  }
}
```



这里子组件从父组件接数据，感觉就像是向上开了一个入口，只需要从父级传入子组件需要的数据即可



## props 驼峰标识

Vue 在使用组件传参的时候，是不支持驼峰命名法的，但是在组件中，以及在模板中是可以正常使用的

```javascript
const app = new Vue({
  el: '#app',
  data: {
    movies: ['喜洋洋', '窃听风云', '姜子牙']
  },
  components: {
    'cpn': {
      template: '#cpn',
      props: {
        cMovies: {
          type: Array,
          default: function () {
            return [];
          }
        }
      }
    }
  }
})
```



模板中：

```html
<template id="cpn">
  <div>
    {{cMovies}}
  </div>
</template>
```



而在使用组件传参的时候，如果想正确传参，则**驼峰的写法必须要用横杠分开**

```html
<!-- 正确写法 -->
<div id="app">
  <cpn :c-movies="movies"></cpn>
</div>

<!-- 错误写法 -->
<div id="app">
  <cpn :cMovies="movies"></cpn>
</div>
```



## 子传父通信

子组件通过 `this.$emit()` 方法来向父组件传一个自定义事件，首先创建子组件的模板：

```html
<!--子组件模板-->
<template id="cpn">
  <div>
    <button v-for="item in categories"
            @click="btnClick(item)">{{item.name}}</button>
  </div>
</template>
```



上面子组件通过监听点击事件，触发 `btnClick` 方法，并将 item 作为参数传进去：

```javascript
// 子组件
const cpn = {
  template: '#cpn',
  data: () => {
    return {
      categories: [
        {id: '1', name: '热门推荐'},
        {id: '2', name: '手机数码'}
      ]
    }
  },
  methods: {
    btnClick(item) {
      // 注意：这里如果不是在脚手架里的话，不可以使用驼峰命名法！
      this.$emit('item-click', item)
    }
  }
}
```



这里 emit 的第一个参数为自定义事件的名称，第二个参数则是需要传递的对象，这个对象会被封装进事件中，然后**在父组件里监听这个自定义事件**：

```html
<div id="app">
  <!-- 这里监听到这个事件之后出发自定义方法 -->
  <cpn @item-click="cpnClick"></cpn>
</div>
```



父组件：

```javascript
// 父组件
const app = new Vue({
  el: '#app',
  methods: {
    cpnClick(item) {
      console.log(item)
    }
  },
  components: {
    cpn: cpn
  }
})
```





## 父组件拿子组件

方法 1：

所有写在父组件里所有组件（包括全局组件），都可有通过 `this.$children `的方式来获取到全部组件，但是它没办法精准获取到特定的一个



方法 2：

通过给子组件添加 `ref` 属性来精准获取

```html
<div id="app">
  <cpn ref="cpn"></cpn>
  <mycpn ref="mycpn"></mycpn>
</div>
```



在以上这种情况中，通过在父组件中 `this.$refs.cpn` 来精准获取到第一个组件。获取子组件后，可以调用组件的方法，也可以直接获取到组件的数据



## 子组件拿父组件

方法 1：

拿上一层及的父组件使用 `this.$parent` 的方法来获取，但是通常用的很少，因为一旦该组件依赖父组件的话，其复用性就会降低



方法 2：

通过 `this.$root` 来获取到 Vue 实例（根组件）



# 插槽

插槽是在不确定写什么内容的时候，预留出来的位置。插槽的好处是可以方便一个组件保留一些共性，同时开放出插槽来实现不同，比如手机端一些购物网站的导航栏



## 快速开始

使用插槽预留位置：

```html
<template id="cpn">
  <div>
    <h2>我是组件</h2>
    <slot><span>默认内容</span></slot>
  </div>
</template>
```



在使用组件的时候只需要在标签里添加想要添加的元素即可，如果不添加内容则显示默认的内容

```html
<div id="app">
  <cpn>
    <button>插槽</button>
  </cpn>
</div>
```



## 具名插槽

当有很多插槽的时候，想要把内容精确的放到特定的一个插槽中就需要给插槽起名字，否则插入的内容会同时插入到每一个插槽中：

```html
<template id="cpn">
  <div>
    <slot name="left"><span>默认 left</span></slot>
    <slot name="center"><span>默认 center</span></slot>
    <slot name="right"><span>默认 right</span></slot>
  </div>
</template>
```



在使用的时候则只需要在标签上添加上 `slot` 属性，值写插槽名字即可：

```html
<div id="app">
  <cpn>
    <!-- 这里就是替换掉了 name 为 center 的插槽里的默认内容 -->
    <template slot="center">替换 center</template>
  </cpn>
</div>
```



---



**注意：这个用法已经被官方弃用**

> 在 2.6.0 中，我们为具名插槽和作用域插槽引入了一个新的统一的语法 (即 `v-slot` 指令)。它取代了 `slot` 和 `slot-scope` 这两个目前已被废弃但未被移除且仍在[文档中](https://cn.vuejs.org/v2/guide/components-slots.html#废弃了的语法)的 attribute。新语法的由来可查阅这份 [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md)。

新的用法为：

```html
<div id="app">
  <cpn>
    <template v-slot:center>
      <span>替换 center</span>
    </template>
  </cpn>
</div>
```



**这里必须使用 template 来做替换，**其他的没有被 template 和 v-slot 指令包括在内的都会被放进默认插槽里（就是没有 name 属性的）





## 编译作用域

直接看注释

```html
<div id="app">
  <!-- 
	   这里的 isShow 绑定的是 Vue 实例中的 isShow
	   所有在 <div id="app"> 内的内容，变量都用的是
	   Vue 实例中的
  -->
  <cpn v-show="isShow" ref="child"></cpn>
</div>

<template id="cpn">
  <div>
    <h3>我是组件</h3>
    <!-- 
		 而只有在模板中的内容，才能访问到组件里的数据，或者严格
		 点说，是在 id 为 cpn 的模板中的内容才能使用组件内的
		 数据，因为它挂载在组件里
	-->
    <span v-show="isShow">内容</span>
  </div>
</template>

<script src = "../00-js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      isShow: true
    },
    components: {
      cpn: {
        template: '#cpn',
        data() {
          return {
            isShow: false
          }
        }
      }
    }
  })
</script>
```





## 插槽作用域

> 这一部分笔记总结自官方文档，视频内的内容已经被弃用



使用插槽作用域的原因，简单的来说就是一些组件的数据展示对于父组件来说不够好，需要父组件能够拿到子组件里的数据，然后按照父组件需求进行展示。

通过把子组件里的内容作为 `<slot>` 元素的一个 attribute 给带出去，这样被绑定在 `<slot>` 元素上的 attribute 叫做插槽 prop：

首先创建模板：（解释都在注释上了）

```html 
<template id="cpn">
  <div>
    <!--
        这里给个名字，在使用组件的时候可以直接通过 v-slot
        指定替换那个一个 slot，并且能拿到绑定在这个 slot
        上的数据
        
        而 data 就是一个自定义 attribute，在父组件中可以
        直接获取到绑定的数据
    -->
    <slot name="showlanguages" :data="languages">
      <ul>
        <li v-for="item in languages">{{item}}</li>
      </ul>
    </slot>
  </div>
</template>
```



使用模板，并且获取到数据，按照父组件想要的方式展示数据：

```html
<cpn>
  <!--
      先通过 v-slot 选定 slot，然后后面的值是我们
      对插槽 prop 的自定义命名
      
      注意：下面必须使用 template
  -->
  <template v-slot:showlanguages="slotProps">
    <span>{{slotProps.data.join(' - ')}}</span>
  </template>
</cpn>
```



**针对插槽的使用再强调一下，在替换的时候，一个 `template` 替换一个 `slot`** 



# ES6 模块化

模块化是指将一个复杂的系统分解为多个模块以方便编码。当项目变大时，需要用模块化的思想来组织代码。ES6在语言规格的层面上实现了模块功能，而且实现的相当简单，完全可以成为浏览器和服务器通用的模块解决方案

ES6模块化提出之后，实现了模块功能而起相当简单，逐渐取代 CommonJS 和 AMD 的两种规范，因为它同时具有CommonJS 简练的语法（for single exports and support for cyclic dependencies）和AMD的可异步加载依赖



## 导出方式

在声明时就导出（变量、函数、类都可以导出）

```javascript
export let flag = true;
export function sum(a, b) {
  return a + b;
}
```



另外一种方式是先声明，后导入

```javascript
let name = 'Hello';
function sum(a, b) {
  return a + b;
}

export { name, sum }
```



还有一种特别的导入方法是 `export default`，这种导出方式没有对导出的内容有明确的命名，所以当导入的时候可以自已定义命名：

```javascript
// 导出
let a = 20;
let b = 40;

function sum(a, b) {
  return a + b;
}

export {b, sum}
export default a

// 导入
import {b, sum} from "./fileB.js";
import v from "./fileB.js"

console.log(b)
console.log(sum(1, 1))

console.log(v)
```

使用格式就是 `import name from path`，其中 name 可以随便起

在一个模块里只能有一个 `export default`



在导入的时候，如果导入的内容太多，可以用 `*` 来全部导入并且自定义命名

```javascript
import * as lib from path;
```









