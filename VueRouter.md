# Vue Router

通过 CLI 创建项目后，如果启用 router，则会多出一个 router 的文件夹，里面的 `index.js` 就是用来配置路由映射的，并且它在全局注册了两个组件：`<router-link>` 和 `<router-view>`，前者是用来跳转页面，修改 url 的，而后者是用来做组件展示的



## 快速开始

首先在 `router -> index.js` 中配置组件和路径的映射关系：

```javascript
// 默认有一个叫 Home 的组件和一个叫 About 的组件
routes: [
  {
    path: '/home',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: About
  }
]
```



之后在想设置跳转按钮的界面添加 `<router-link>` 标签：

```javascript
// App.vue
<template>
  <div id="app">
    <router-link to="/home">Home</router-link>
    <router-link to="/about">about</router-link>

    <router-view/>
  </div>
</template>
```



`<router-link>`最后会被渲染为 `<a>` 标签，而 `<router-view>` 会根据当前的路径，动态的渲染出不同的组件



## 重定向

`router -> index.js` 中在配置路由的时候，可以通过 `redirect` 属性来实现重定向，比如我们配置了 home 页面的 path，而当打开网站的时候默认直接打开 home 页面，则可以通过 `redirect` 属性实现

```javascript
routes: [
  {
    path: '/',
    redirect: '/home'
  }
]
```



## 修改路由为 history 模式

由于修改 url 的 hash 属性会带有 `#`，并不是很美观，router 默认就是修改 hash。可以通过在创建 router 的时候（在 `router -> index.js` 中），添加条叫 `mode` 的属性：

```javascript
export default new Router({
  routes: [],
  mode: 'history'
})
```



## router-link

最常用的 `router-link` 标签的属性为 `to` 属性，点击这个标签就会跳转到对应的路径

其他常用属性：

1. tag: tag 属性可以指定 `<router-link>` 之后会渲染成什么组件，比如渲染为一个 button
2. replace: 这个属性不会留下 history 记录，其实就和 `history.replaceState` 的效果一样
3. active-class: 等 `<router-link>` 对应的路由匹配成功后，会给当前元素默认添加一个 `router-link-active` 的 class 属性，这里我们可以手动设置想要的 class



## 代码跳转（不用 router-link）

可以通过监听事件的方式，来实现 URL 跳转。在安装了 `vue-router` 的项目中，所有的 Vue 组件都有一个 `$router` 属性（对象），它有一个 push 方法和 replace 方法，都可以实现手动跳转



```vue
<template>
  <div id="app">
    <button @click="homeClick">Home</button>
    <button @click="aboutClick">About</button>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  methods: {
    homeClick() {
      this.$router.push('/home')
    },
    aboutClick() {
      this.$router.push('/about')
    }
  }
}
</script>
```



重复跳转同一路由会报错（但是不影响使用），解决方法可以在 `router/index.js` 中，使用 `Vue.use(Router)` 之前添加如下两行代码：

```javascript
const origianl = Router.prototype.push

Router.prototype.push = function push(location) {
  return origianl.call(this, location).catch(err => err)
}
```



## 动态路由使用

假如我们有一个 URL 的形式为 `/user/userId`，此时想要动态的根据 `userId` 来展示页面，需要配置路由：

```javascript
/* router/index.js */

{
  // 这里可以用 & 连接，传入多个参数
  path: '/user/:userId',
  name: 'User',
  component: User
}

```



在跳转到 `/user/userId` 这个 URL 的时候，需要对整个 URL 进行拼接：

```vue
// App.vue
// 这里的 userId 为 data 里的数据
<router-link :to="'/user/' + userId">User</router-link>
```



之后在 User 组件中取到这个参数，通过 `$route.params.XXX` 的方式获得，这个 XXX 就是在配置路由的时候配置的 `:XXX`，其中 route 所表示的就是当前所访问的、所激活的路径（在路由配置中有很多个 routes）

```javascript
export default {
  name: "User",
  computed: {
    userId() {
      return this.$route.params.userId
    }
  }
}
```



## 打包之后的文件

打包之后主要有三个文件：

1. app.js：这个文件里全部都存放的是我们自己写的业务代码
2. manifest.js：这个文件里存放的是打包、运行我们代码的最底层支撑，帮助把我们的代码都整合到一起，核心的函数为 `__webpack_require__`
3. vendor.js：存放的为外部代码，如 Vue 框架、axios 框架等




## 懒加载

在打包构建项目的时候，JS 文件会非常大（大项目），导致页面第一次加载的时候时间会比较长，可以通过懒加载的机制把组件分成不同的代码块，在有需要的时候在进行请求加载

（偷懒直接发截图了）

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20210616025722.png)



还有其他两种旧的方式来实现懒加载，这里就不写了



## 路由嵌套

当访问 Home 页面，其次级还有 `/home/news` 这样的路径的时候，就需要配置嵌套路由，首先创建一个叫 News 的组件，然后在路由配置中添加 `children` 属性，是个数组数据类型：

```javascript
// router/index.js
{
  path: '/home',
  name: 'Home',
  component: Home,
  children: [
    {
      path: '/',
      redirect: 'news'
    },
    {
      path: 'news',
      name: 'HomeNews',
      component: HomeNews
    }
  ]
},

```



同时设置了重定向，这样用户打开首页的时候，也可以直接打开新闻界面，之后这个 News 的组件是要展示在 Home 页面的，所以要在 Home 组件内添加对应的 `router-link` 和 `router-view`：

```vue
// Home.vue

<template>
  <div>
    <h2>我是 Home</h2>

    <router-link to="/home/news">News</router-link>
    <router-link to="/home/message">Message</router-link>

    <router-view></router-view>
  </div>
</template>
```





## 参数传递

在动态路由小节提到可以通过在 path 后面跟上对应值的方式来传参，通过 params 的方式实现，同时还有一种通过 query 的形式实现的，大概长这个样子：

```
/router?id=123
```



query 形式，问号后面的那一串对应的为一个对象，问号前为 path，在给 `router-link` 的 `to` 设置属性值的时候，赋一个对象：

```vue
// 
<router-link :to="
  {path: '/profile', 
  query: {
    // userId 和 userName 为 data 中的数据
    id: userId,
    name: userName}
  }"
> Profile </router-link>
```



最终上面跳转时候的 URL 为：

```
http://localhost:8080/profile?id=10001&name=Jack
```



在  `Profile` 组件中，想拿到这些参数可以通过 `this.$route.query.XXX` 来获取到

如果通过代码的方式，就是监听点击事件，手动写跳转的话，方式也是一样的：

```javascript
// push 传一个链接
userClick() {
  this.$router.push('/user/' + this.userId)
}

// push 传一个对象
profileClick() {
  this.$router.push({
    path: '/profile',
    query: {
      name: 'Jack',
      age: 22
    }
  })
}
```



## 路由守卫

在 Vue 中，所有的 URL 跳转，都会被 `NavigationGuad` 监听到，所以在每次跳转的时候我们就可以对应做一些操作，比如修改网页的 Title

在路由配置文件中，创建 `VueRouter` 之后，重写 router 中的 `beforeEach` 方法：

```javascript
// 声明个变量先拿到 Router，重写之后在 export 出去
router.beforeEach((to, from, next) => {
  document.title = to.matched[0].meta.title
  next()
})
```



这里，`to` 就代表的是在 Router 配置文件中的一个个 route，有的子路径如果没有配置 title，需要跟父路径的 title 保持一致，所以这里取的是 `matched` 中的第一个 meta 中的 title



关于路由守卫，还有一个 `afterEach` ，是在跳转之后执行回调，以上的两个都是全局的守卫。同时还有路由独享守卫和对应到每一个组件的守卫，具体参考官方文档：

> https://router.vuejs.org/zh/guide/advanced/navigation-guards.html

（他们的使用基本一样）



## keep-alive

当从一个页面，比如 `/home/message` ，跳转到另外一个页面，比如 `/profile` ，之后再回到 `/home` 页面之后，如果想做到可以停留在 `/home/message` 而不是 `/home/news`，此时我们需要使用 `keep-alive`（默认的话，在跳转之后会重新构建一次这个组件）

这里我们首先要知道，`router-view` 是一个组件（在 VueRouter 中），如果直接用 `keep-alive` 抱住 `router-view` 的话，那么所有路径匹配到的视图组件都会被缓存。

`keep-alive` 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或者避免重新渲染，包括其包含组件下面的子组件。

这里我们先做一个简单的实验，重写 Home 组件的 `created` 和 `destroyed` 两个生命周期回调函数：

```vue
<script>
export default {
  name: "Home",
  created() {
    console.log("Created...")
  },
  destroyed() {
    console.log("Destroyed...")
  }
}
</script>
```



然后不断在 About 页面和 Home 页面之间跳转，可以很容易发现在点 Home 页面的时候，会回调 created 函数，而点 About 页面的时候，会回调 destroyed 函数

那么想实现最开始的效果的话，首先我们需要有一个思路，就是在保留组件状态，不让它重新创建的情况下，用一个变量来记录离开 `/home/message` 的这个路径，所以先在 App 组件中，把 `router-view` 用 `keep-alive` 包裹起来，然后在 Home 组件里先添加 data 属性：

```vue
<script>
export default {
  name: "Home",
  data() {
    return {
      // 默认为 /home/news
      path: '/home/news'
    }
  }
}
</script>
```



我们想在 Home 组件一旦激活的时候，就让 router 把我们 push 到缓存的这个 path 中去，所以这里需要使用到 `actived` 生命周期回调函数，同时在跳转到其他页面的时候，记录下最新的 URL：

```vue
<script>
export default {
  name: "Home",
  data() {
    return {
      path: '/home/news'
    }
  },
  activated() {
    // 直接 push 到离开该页面前最新的 URL
    this.$router.push(this.path)
  },
  beforeRouteLeave: (to, from, next) => {
    // 获取到最新的 URL 并进行替换
    this.path = this.$route.path
    // 必须有！！！
    next()
  }
}
</script>
```



这里没使用 `deactived` 生命周期回调是因为它执行的时间太晚了，导致 `this.$route.path` 获取到的是跳转的页面的 URL

这里注意一点：`actived` 和 `deactived` 只有在组件在 `keep-alived` 组件下的时候，这而两个回调函数才有效果！！



## keep-alive 的属性设置

一旦被 `keep-alive` 包含之后，所有的组件都会被缓存，为了能实现一部分组件被缓存，一部分不被缓存，可以通过设置 `inlude` 和 `exclude` 属性

- include：字符串或正则，匹配的组件就会被缓存
- exclude：同样也是字符串或正则，匹配的组件都不会被缓存

注意：这里写的字符串，或是正则匹配的，是每一个组件里的 name 属性！！

```vue
<template>
  <div id="app">
	<!-- ...code... -->
    <keep-alive exclude="Profile,User">
      <router-view></router-view>
    </keep-alive>
  </div>
</template>
```


