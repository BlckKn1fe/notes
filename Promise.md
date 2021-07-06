Promise 为异步编程的一种解决方案，在网络请求中很常见，其可以很明确的逻辑处理请求的数据



# 基本使用

将一个异步操作塞进 Promise 实例中，并且在异步操作成功的时候调用 resolve 函数，并且把异步操作的结果作为参数传给 resolve；反之则调用 reject 函数，把错误信息作为函数传给 reject

```javascript
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Hello World!')
    // reject('Refuse!')
  }, 1000)
}).then((data) => {
  console.log(data)
}).catch((err) => {
  console.log(err)
})
```



在判断结果之后，来选择调用 resolve 或者是 reject，resolve 之后会进入到下一个 then 中处理结果；若期间有错误发成（reject被调用），则会进入到下一个 catch 中来处理异常



# 多层处理

有时需要对异步操作（请求）的结果进行多层处理，之所以不在同一层处理，是希望可以让逻辑更清晰，操作更简便。比如对一串字符串先这样，然后在那样，最后在这样，此时需要多次通过 resolve/then 的方式来处理数据，期间发生错误通过 catch 来捕获

```javascript
new Promise((resolve, reject) => {
  setTimeout(() => {
    // 获得异步的返回结果
    resolve('Hello World!')
  }, 1000)
}).then(res => {
  // 打印输出处理结果
  console.log(res)
  // 通过新建 Promise 来获取到 resolve 函数
  return new Promise(resolve => {
    // 长度小于 20 
    resolve('-- ' + res + '--')
    // 长度大于 20
    // resolve('---- ' + res + '----')
  })
}).then(res => {
  // 若这一层获取到的结果长度大于 20，则调用 reject 函数
  if (res.length > 20) {
    return new Promise((resolve, reject) => {
      reject('Too Long!')
    })
  }
  // 输出获取到的结果
  console.log(res);
}).catch(err => {
  // 打印错误信息
  console.log(err)
})
```



# 链式简写

在多层处理中，通过新建一个 Promise 然后获取 resolve 或 reject 的方式有点麻烦，代码看着很乱，所以有两个简写的方式，本质是一样的

通过 Promise 类的自带静态方法（说的不准确，差不多就这意思）

```javascript
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Hello World!')
  }, 1000)
}).then(res => {
  return Promise.resolve(res)
}).then(res => {
  if (res.length === 0) {
    return Promist.reject('err')
  }
  console.log(res);
}).catch(err => {
  console.log(err)
})
```



直接 return/throw 的方式来进入下一层操作

```javascript
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Hello World!')
  }, 1000)
}).then(res => {
  return res
}).then(res => {
  if (res.length === 0) {
    throw 'err'
  }
  console.log(res);
}).catch(err => {
  console.log(err)
})
```



# Promise 的 all 方法

all 方法是用来同时处理多个异步请求用的，比如前端为了实现某一个功能，需要同时请求两个数据，此时可以通过 Promise 的 all 方法来同时拿到两个数据

```javascript
Promise.all([
    new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({
          name: 'BlackKnife',
          age: 18
        })
      }, 2000)
    }),
    new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('Hello World!')
      })
    })
]).then(results => {
  console.log(results);
})
```























