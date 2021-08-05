# axios 请求方式

```
axios(config)
axios.request(config)
axios.get(url[,config])
axios.delete()url[,config]
axios.head(url[,config])
axios.post(url[,data[,config])
axios.put(url[,data[,config])
axios.patch(url[,data[,config])
```



# 快速开始

安装 axios：

```
npm install axios --save
```



为做 demo 演示，在 Vue 项目中的 main 入口处使用 axios

```javascript
import axios from "axios"

// 默认请求
axios({
  url: 'XXXXXXXXX'
}).then(res => {
  console.log(res.config);
})
```



如果请求后面有参数的话，可以添加一个 params 属性：

```javascript
axios({
  url: 'XXXXXXXXX',
  // 针对 get 请求的参数拼接
  params: {
    type: 'pop',
    page: 1
  }
}).then(res => {
  console.log(res.config);
})
```



# 并发请求

当有多个网络请求，并且要求结果全部到达之后在做进一步处理的话，可以使用 all 方法

```javascript
axios.all([
  axios(),
  axios()
]).then(results => {
  console.log(results)
})
```



axios 还提供了 spread 方法可以把结果直接解构出来

```javascript
axios.all([
  axios(),
  axios()
]).then(axios.spread((res1, res2) => {
  console.log(res1)
  console.log(res2)
}))
```



# axios 配置

有很多公共的配置可以通过设置 axios 的全局配置，然后让所有的其他请求都使用到全局配置，可以通过设置 axios 的 defaults 属性：

```javascript
// 注意这是全局配置！
axios.defaults.baseURL = 'XXXXX'
axios.defaults.timeout = 60
```



更多配置参考官方文档：

> https://axios-http.com/zh/docs/req_config



# axios 实例

如果我们用上面的方式做全局配置，所有的 axios 都会使用同一套配置。有时候，由于后端分布式设计，需要我们去不同的服务器内请求数据，就导致了 URL 不一样，这种时候就可以创建多几个 axios 实例，然后给他们彼此独立的配置

```javascript
const instanceA = axios.create({
  baseURL: 'XXXXX',
  timeout: 5000
})

const instanceB = axios.create({
  baseURL: 'YYYYY',
  timeout: 2000
})
```



# axios 模块化封装

如果我们在每一个 Vue 的组件内都导入了 axios 并使用它，那么项目对 axios 的依赖就非常强，所以比较好的一个办法是在**外部封装好一系列请求方法，在组件的角度来看，每个组件只需要调用对应的方法就可以获取到想要的数据**，不需要考虑该方法是如何实现的

```javascript
// network/request.js
import axios from "axios";

export function request(config) {
  const instance = axios.create({
    baseURL: "http://152.136.185.210:7878/api/m5",
    timeout: 2000
  })

  return instance(config)
}

```



在其他组件想要进行网络请求的时候，只需要引用该文件，调用 request 方法就可以。如果以后出现其他的第三方框架，只需要改动 request.js 文件即可，将异步操作包裹进 Promise 中并 return 出去，不影响其他地方使用





