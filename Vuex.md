Vuex 可以看做一个全局静态对象管理工具，可以在里面存放不同的信息、方法等等，所有的组件都可以直接使用 Vuex 中的 store

并且，Vuex 提供完整的额状态管理模式

![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20210629062451.png)



当 Vue 的组件 dispatch 一个 action 之后，会把结果 commit 到 mutation 这个环节，mutation 中发生的操作是可以同 Vue Devtools 来监测到的，然后会对 state 进行 mutate，最后把新的 state 重新 render 到 Vue Components 上



# 安装

通过以下指令安装 Vuex：

```
npm install vuex --save
```



随后，创建名为 `store` 的文件夹，并且创建 `index.js` 文件，随后安装 Vuex 并导出，最后挂载在项目入口 `main.js` 中即可

```javascript
// store/index.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
      
  },
  mutations: {

  },
  actions: {

  },
  getters: {

  },
  modules: {

  }

})

// main.js
import store from './store'
new Vue({
  // ...
  store,
  render: h => h(App)
})
```



安装成功之后，在所有的组件中都可以通过 `$store` 来获取到 store 对象，然后获取到 state、mutations 等等中的数据、方法等



# 快速使用

比如，我们想多个组件共享一个 counter，此时可以在 state 中声明一个 counter，并且在 mutations 里写好对 counter 操作相关的方法

```javascript
//store/index.js
export default new Vuex.Store({
  state: {
   counter: 0
  },
  mutations: {
    counterIncrement(state) {
      state.counter++
    },
    counterDecrement(state) {
      state.counter--;
    }

})
    
```



之后，在其他所有组件中通过 `$store.state.counter` 来获取到 counter 的值，通过 `$store.commit()` 来调用 mutations 中的方法，其中 commit 传入的参数为声明的方法名（字符串形式）

```html
<button @click="$store.commit('counterIncrement')">+</button>
```



不过这里最好是在组件自己的 scope 里单独声明一个方法，然后再通过 `$store.commit()` 的方法修改



# 五个核心



## State

单一状态树（Single Source of Truth），确保一个项目中，所有的全局状态都来自有且仅有一个 store 对象，它就是用来保存数据的

使用 state 来存数据的时候，需要在一开始就把每一个变量都初始化好，如果想要给 state 里添加新的数据（属性）的时候，需要用以下两种方式：

```javascript
// 这里假设 state 里有一个 info 对象
mutations: {
  updateInfo(state, payload) {
    // 方法 1： Vue.set()
    Vue.set(state.info, 'address', payload.address)
    // 方法 2 ：给
    state.info = {...state.info, 'address', payload.address}
  }
}
```



如果想要删除某一条属性的话，则需要使用 `Vue.delete(object, key)`

```javascript
// 假设 state 里有一个 info 对象，并且里面有一条 name 属性
Vue.delete(state.info, 'name')
```



只有通过以上的方式来添加/删除属性，页面才会是响应式的，如果使用以下方式的话，页面不会随着改变而重新渲染：

```javascript
state.info['address'] = 'Beijing'
```





## Getters

这个就和计算属性（computed）一毛一样，也是对某一个 state 中的数据做一些处理之后返回的特定的、想要的数据形式。其中 getters 里写的函数（方法）第一个默认参数为 state，第二个默认参数为 getters。而当使用 getters 的时候想要传递一些参数（比如自定义筛选出年龄大于 X 的人），需要在方法中 return 一个函数出去

```javascript
// 假设 state 里有一个叫 students 的数组存放很多个 student 对象
getters: {
  greaterThanAgeStu(state) {
    return (age) => {
      return state.students.filter(s => s.age > age)
    }
  }
}
```



## Mutations

Vuex 中 store 的更新要通过提交 mutation，mutation 主要包含两个部分，字符串的事件类型（type）和一个回调函数，该回调函数的第一个参数就是state，第二个参数为额外参数，可以为一个值，也可以为一个对象

```javascript
// 以自定义增加 counter 值为例
addCounter(state, value) {
  state.counter += value;
}
```



在其他组件中想调用该方法，需要在在调用 `$store` 的 `commit` 方法时，跟一个 payload：

```javascript
// 组件内的 methods
addCounter(value) {
  this.$store.commit('addCounter', value)
}
```



当遇到多个参数的时候，讲其封装为一个对象再传入。以上这种 commit 的方式为提交 commit 的其中一种风格，还有另一种风格就是给 commit 方法传入一个带有 type 的对象：

```javascript
addCounter(value) {
  this.$store.commit({
    type: 'addCounter',
    value,
    // more arguments...
  })
}
```



在 mutations 里通过对象取出对应参数：

```javascript
addCounter(state, payload) {
  state.counter += payload.value;
}
```



用这种风格的话，传入更多参数的时候会更加清楚一些



## 常量定义 mutations

在其他组件内通过 commit mutations，来实现对 state 进行更新，但是在 mutation type 的时候，可能因为字符串手打敲错了，而出现问题，所以最好通过一些手段让 mutations 里的方法名和 commit 中填写的 mutation type 保持一致，这里就可以在外部首先创建一个新的文件

```javascript
// mutation-types.js
export const INCREMENT = 'increment'
export const UPDATE_INFO = 'updateInfo'
```



随后在 `store/index.js` 和对应的组件里引入该文件：

```javascript
// store/index.js
import * as type from './mutation-types'

const store = new Vuex.Store({
  // ...
  mutations: {
    [type.INCREMENT](state) {
      state.counter++
    }
  }
})

```



## Actions

当 mutations 里使用异步操作的时候，devtool 是追踪不到的，mutations 的所有操作都是同步的，如果想使用异步操作的话，都写在 actions 里，比如网络请求，actions 中如果有需要对 state 进行修改的操作，必须通过 commit mutations 的方式

```javascript
actions: {
  // 假定还是 state 中一个 info 对象
  // 并且 mutations 中有 updateInfo 方法
  asynUpdateInfo(context, payload) {
    setTimeout(() => {
      context.commit('updateInfo', payload)
    }, 1000)
  }
}
```



actions 中的异步方法，第一个参数为 context，第二个参数为 payload，其中 context 此处为当前的 store 对象（或者说是当前的模块）

随后在组件内调用，通过 `$store` 中的 dispatch 来调用 actions 中的异步方法

```javascript
updateInfo() {
  this.$store.dispatch('asynUpdateInfo', {
    age: 18
  })
}
```



为了得知异步操作的成功还是失败，并且根据结果做不同的处理，我们可以通过 Promise 包裹异步操作，并且将 Promise 返回来实现

```javascript
actions: {
  asynUpdateInfo(context, payload) {
    // 返回一个 Promise
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        context.commit('updateInfo', payload)
        // 异步操作成功则调用 resolve 方法
        resolve('success')
      }, 1000)
    })
  }
},
```



而在组件内使用的时候，只需要通过 `then` 和 `catch` 来根据返回的结果来做相应的处理即可

```javascript
// App.vue
updateInfo() {
  this.$store.dispatch('asynUpdateInfo', payload)
  .then(res => {
    // success...
  
  .catch(err => {
    // fail...
  })
}

```



# Modules

这部分参考官方文档：

> https://vuex.vuejs.org/zh/guide/modules.html



# Store 的目录结构

当项目越来越大的时候，使用以下项目目录：

```
├── index.html
├── main.js
├── api
│   └── ... # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```



官方建议，state 还可以放在 index.js 中，然后 actions 和 mutations 抽出来，如果 getters 也很多的话，也可以抽出去

然后对于不同的模块，可以单独建立一个 modules 文件夹，把不同的模块放在这个文件夹里，避免把 modules 的代码都写在 index.js 中



#  线程总线

在 Vuex 里补充一下这个吧... 毕竟都是全局的东西

线程总线简单来说就是一个可以全局实现发送事件，全局监听事件的一个东西，在项目入口 `main.js` 给 Vue 的原型添加一个 `$bus`

```javascript
Vue.prototype.$bus = new Vue()
```



这里通过创建新的 Vue 实例来作为线程总线使用

之后在任意一个组件中，都可以通过以下方式来发送事件出去：

```javascript
this.$bus.$emit('eventName', {})
```



之后在任意一个组件中都可以监听到这个事件：

```javascript
this.$bus.$on('eventName', () => { /* callback function */})
```



# 防抖函数

假如，当用户在使用网页搜索内容的时候，如果每次检测到输入框的修改就发送一次请求，然后修改下方内容提示的话，会对服务器造成很大的压力，比较好的处理方法就是用户输入间隔大于一定时间之后，再发送请求，可以通过防抖函数来实现

防抖 debounce  / （节流throttle）

```javascript
debounce(fn, delay) {
  // 此处的 timer 会作为局部变量一直被引用
  let timer = null
  return function (...args) {
    if (timer) clearTimeout(timer)

    timer = setTimeout(() => {
      fn.apply(this, args)
    }, delay)
  }
}
```









