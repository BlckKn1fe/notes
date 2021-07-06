Webpack 是一个 JavaScript 应用的静态模块打包工具，



# 快速开始

这里使用指定版本 Webpack 3.6.0 来做记录，并且 Node 版本为 12.18.3，Webpack 3.6.0 版本中的一些插件在 Node 14+ 的版本里没法用

```
npm install webpack@3.6.0 -g
```



Webpack 的基本目录结构如下

```
prroject
	|----dist
	|----src
```



用不同的模块化标准进行开发，浏览器无法直接运行这些代码，但是通过 Webpack 打包处理之后，浏览器是可以执行的，使用过程如下：

```javascript
// mathUtils.js
function sum(a, b) { return a + b; }

function mul(a, b) { return a * b; }

module.exports = {
  sum,
  mul
}

// main.js
const { sum, mul } = require('./mathUtils')

console.log(sum(1, 2));
console.log(mul(4, 5));
```



然后在 project 的目录下使用 Webpack 打包命令

```
webpack ./src/main.js ./dist/bundle.js
```



之后在 dist 文件夹中就会有生成的打包后的 bundle.js 文件，在 html 文件中引用即可



# 配置文件

每次使用 webpack 指令打包的时候都需要写路径，相对来说麻烦一些，通过配置文件可以直接使用 webpack 实现一键打包，首先需要在根目录下创建一个配置文件

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'bundle.js'
  }
};
```



其中，path 是一个Node 的一个核心模块，需要我们导入（注意：这时候我们已经可以直接输入 webpack 来实现打包了，但是它使用的是全局的 Webpack）

然后我们需要执行 `npm init` 命令来初始化项目（其实这个应该在一开始执行的），会得到一个 `package.json` ，这里可以对整个项目进行一些简单的配置。然后，相对来说，一个项目都有自己属于该项目的独立的 Webpack，所以还需要安装本地 Webpack

```
npm install webpack@3.6.0 --save-dev
```

（dev 表示开发阶段）

然后生成的配置文件内容大概如下

```json
{
  "name": "basic-use-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^3.6.0"
  }
}

```



其中，scripts 中，配置的就是有关 `npm run xxx` 的指令，比如这里配置了 build 指令，它会直接调用项目中的 webpack 来进行打包，实际它运行的是 `node_modules/.bin` 里的 webpack 打包脚本

`devDependencies` 表示开始阶段需要的依赖



# CSS文件配置

webpack 不仅可以对 JS 文件进行打包，也可以对 CSS 、图片等文件进行打包，还可以 TS 转 ES5， less 转换 CSS 等等，这需要给 webpack 扩展相对应的 loader 来实现



loader 使用：

1. 通过 npm 安装需要使用的 loader
2. 在 `webpack.confic.js` 中的 modules 关键字下配置

（大部分的 loader 都可以在 webpack 官网中找到，以及其对应使用方法）



修改项目文件目录：

```
project
  |--dist
  |--node_modules
  |--src
  |---|---css
  |---|---js
  |-- main.js
```



将需要的样式添加到 css 文件夹中，然后在入口 `main.js` 处进行引用

```
// normal.css
body {
  background-color: red;
}

// main.js
require('./css/normal.css');
```



随后需要安装两个 loader

> https://webpack.js.org/loaders/

1. css-loader：负责加载 css 文件

   ```
   npm install --save-dev css-loader
   ```

2. style-loader：负责将样式添加到 DOM 节点上

   ```
   npm install --save-dev style-loader
   ```



**注意：**css-loader 版本过高会出现报错，通过修改 `package.json` 中的版本为 3.6.0 

随后在 `webpack.config.js` 中添加 module：

```javascript
module: {
  rules: [
    {
      test: /\.css$/i,
      // 在使用 loader 的时候，是从右向左读取
      use: ["style-loader", "css-loader"],
    },
  ]
}
```



# 图片处理

同样也是需要先安装相对应的 loader，注意以下内容是针对 Webpack 3.6.0 （或 V4）的，不适用于 Webpack V5

```console
npm install url-loader --save-dev
```



添加 `webpack.config.js` 配置

```javascript
{
  test: /\.(png|jpg|gif)$/i,
  use: [
    {
      loader: 'url-loader',
      options: {
       	// 当加载的图片小于 limit 时候，会把图片转为 Base64 编码
        limit: 8196,
        // 这个 img 是在导出时统一放在一个文件夹，name 是原文件名
        // hash 取八位，ext 为扩展名
        name: 'img/[name].[hash:8].[ext]'
      },
    }
  ],
},
```



可以通过 CSS 背景的形式展示图片：

```css
body {
  background: url("../img/1.jpg");
}
```



而当图片的大小大于 limit 的时候，需要安装 `file-loader`，并且打包之后图片会被放在 `dist` 文件夹中，如果想正确获取到图片的话，需要在 `webpack.config.js` 的 `output` 中，添加一个 `publicPath: 'dist/'` 

（实际布置项目的时候，用其他的方式）



# ES6 转 ES5

ES6 转 ES5 也是需要一些特定的 loader 来处理

```
npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
```



之后在 `webpack.config.js` 添加配置

```
{
  test: /\.js$/,
  exclude: /(node_modules|bower_components)/,
  use: {
    loader: 'babel-loader',
    options: {
      presets: ['es2015']
    }
  }
},
```



重新 build 之后，bundle.js 文件中全部都是 ES5 的代码



# Vue



## 配置 Vue

首先通过 npm 安装 Vue 到本地项目内

```
// 注意这里不是 --save-dev
npm install vue --save
```



随后要在 Webpack 配置中添加 resolve 选项

> 具体原因参考官网 ：
>
> https://cn.vuejs.org/v2/guide/installation.html#%E8%BF%90%E8%A1%8C%E6%97%B6-%E7%BC%96%E8%AF%91%E5%99%A8-vs-%E5%8F%AA%E5%8C%85%E5%90%AB%E8%BF%90%E8%A1%8C%E6%97%B6

```javascript
module.exports = {
  // ...
  resolve: {
    alias: {
      // 用 webpack 1 时需用 'vue/dist/vue.common.js'
      'vue$': 'vue/dist/vue.esm.js' 
    }
  }
}
```



## 使用 Vue

通常 index.html 文件都不会更改，只需要用 el 挂载到一个节点上，然后交给 Vue 来渲染整个页面

```html
<div id="app"></div>
```



这一部分代码就不变了，同时在 Vue 实例实例中加入 template 选项（这还不是最终方案）

```javascript
const app = new Vue({
  el:'#app',
  template: `
    <div>
      <h2>{{message}}</h2>
    </div>
  `,
  data: {
    message: 'Hello World!'
  }
})
```



在 Vue 渲染页面的时候，如果 el 和 template 同时存在，会把 `<div id="app"></div>` 所有的内容，替换成 template 中的内容（包括 id=“app" 也不会在结构中显示出来）



## 使用的最终方案

如果 Vue 实例中的 template 内容越来越多，会让 Vue 实例结构越来越复杂，不易于阅读和修改，所以使用 `.vue` 文件进行抽取

为了能够正确编译 `.vue` 文件，需要安装对应的 loader 和插件：

```
npm install vue-loader vue-template-compiler --save-dev
```



然后添加对应的配置到 `webpack.config.js` 中：

```javascript
{
  test: /\.vue$/,
  use: {
    loader: 'vue-loader',
    options: {}
  }
}

// 对应 vue-loader 14 之后的版本，要在后面追加 plugins 选项
module.exports = {
  // ....
  plugins: [new VueLoaderPlugin()]
};
```



随后抽取组件出来：

```vue
// app.vue
<template>
  <div>
    <h2>{{message}}</h2>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      message: "Hello World!"
    }
  }
}
</script>

<style scoped>
  div {
    color: pink;
  }
</style>
```



在入口的地方引用组件，并且使用

```javascript
// main.js
import App from './vue/app.vue'

new Vue({
  el:'#app',
  template: '<cpn/>',
  components: {
    'cpn': App
  }
})
```



# Webpack Plugin



## 版权插件

Webpack 的插件在配置文件中的 plugins 的选项里，通过数组的方式来配置，比如添加版权：

```javascript
const webpack = require('webpack')

module.exports = {
  //....
  plugins: [
    new webpack.BannerPlugin('最终版权归XXX所有')
  ]
}

```



## Html 打包插件

发布项目时，index.html 需要在 `dist` 文件夹中一同发布出去，所以我们需要一个插件能帮助我们也把 index.html 在打包的时候一并放在 `dist` 文件夹中，这里使用 `HtmlWebpackPlugin` 

1. 插件可以自动生成 index.html，可以指定模板
2. 将打包的 JS 文件自动通过 script 标签插入到 body 中

安装：

```
npm install html-webpack-plugin --save-dev
```



添加配置到 `webpack.config.js` 中：

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  //....
  plugins: [
    new HtmlWebpackPlugin(),
  ]
}

```

> 笔记中使用的版本为 3.2.0 的插件版本



之后执行 `npm run build`，发现 `dist` 文件夹中的 index.html 存在两个问题：

1. JS 引用的路径不正确
2. 缺少内容、内容不正确

对于第一个问题需要去掉 Webpack 配置中的 public path 选项

而对于第二个问题，需要给插件配置一个模板，在 new 这个插件的时候，需要传入 index.html 模板的路径：

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  //....
  plugins: [
    new HtmlWebpackPlugin({
   	  // 配置文件同级目录
      template: 'index.html'
    }),
  ]
}
```

**注意：需要去掉模板文件中的 JS 引用**



## 丑化插件

我们开发时写的代码，占的空间会相对大一点，我们可以使用一些丑化插件来减小文件的体积，这里使用 `UglifyjsWebpackPlugin`

安装：

```
npm install uglifyjs-webpack-plugin@1.1.1 --save-dev
```

**注意：默认安装为 1.3.0，若报错则降到 1.1.1，对应 CLI2**



添加配置：

```javascript
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');

module.exports = {
  //....
  plugins: [
    new UglifyJsPlugin(),
  ]
}

```



## 本地服务器

在开发中，如果每次都需要通过打包来预览修改后的页面样子，非常麻烦，所以可以搭建一个本地的服务器，实时监听代码的修改情况，然后通过刷新页面来重新预览（修改之后，会通过缓存的形式来存放新编译出的文件，而不会把编译后的文件存放在 `dist` 文件夹中）

安装：（这个本地服务器是基于 Node.js，使用 express 框架完成的）

```
npm install --save-dev webpack-dev-server@2.9.3
```

安装的版本是针对 webpack 3.6.0 和 Node 14 以下



在 `webpack.config.js` 中，添加 `devServer` 选项：

```javascript
module.exports = {
  // ...
  devServer: {
    // 需要监听的文件夹
    contentBase: './dist',
    // 实时监听
    inline: true
  }
};
```



最后在 `package.json` 中添加 npm 脚本：

```json
"scripts": {
  // ...
  "dev": "webpack-dev-server --open"
},
```



使用时，通过 `npm run dev` 来进行实时预览



# Webpack 配置文件分离

之所以分离，是因为有时候开发时需要的一些配置，发布时用不到，或者是发布时用到的配置、插件等，开发时用不到。所以，分出两个配置文件，一个用于开发时，一个用于生产时。

（这里以教程来举例说明）

首先在项目根目录中创建 `build` 文件夹，并且在里面创建三个文件：`base.config.js`，`prod.config.js`，`dev.config.js`

在 `base.config.js` 中，写入公共的配置，即开发时和生产时都需要的相关配置，随后安装 `webpack-merge` 插件：

```
npm install webpack-merge@4.1.5 --save-dev
```



之后创建另外两个生产时和开发时的配置文件：

```javascript
// prod.config.js
const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config')

module.exports = webpackMerge(baseConfig, {
  plugins: [
    new UglifyjsWebpackPlugin()
  ],
})


// dev.config.js
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config')

module.exports = webpackMerge(baseConfig, {
  devServer: {
    contentBase: '../dist',
    inline: true
  }
})
```



最后，修改 `package.json` 配置，指定编译配置文件路径：

```json
"scripts": {
  // ...
  "build": "webpack --config ./build/prod.config.js",
  "dev": "webpack-dev-server --open --config ./build/dev.config.js"
},
```




