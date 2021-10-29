在写 Vue Demo 相关的内容，是不需要使用 Vue CLI 的，但是如果想要开发相对大型一点的程序的时候，就需要使用到 Vue CLI，因为开发大项目时，我们需要考虑代码目录结构、项目部署、热加载、单元测试等等问题。

CLI 全称为 Command-Line Interface，它可以帮助我们快速的生成基本的目录结构，基本的一些配置，Vue CLI 可以快速搭建 Vue 开发环境以及对应的 webpack 配置



# 安装与初始化

安装 Vue CLI：

```
npm install -g @vue/cli
```

注意：这个安装的是 Vue CLI3，如果想按照 Vue CLI2 的方式初始化项目是不可以的，

如果我们想使用 Vue CLI2 的一些功能的话，可以拉取 CLI2 中的模板，需要安装 `init` 工具：

```
npm install -g @vue/cli-init
```



Vue CLI2 初始化项目：

```
vue init webpack my-project
```



Vue CLI3 初始化项目：

```
vue create my-project
```



初始化根据需要自行选择



# 目录结构说明（CLI2）

- build 和 config：这两个文件夹存放的都是关于 webpack 的配置文件，其中 config 中的配置很多都是配置的一些变量的值，比如是否开启 Eslint ，主机，端口，自动打开浏览器等
- src：这个文件夹中主要存放源码的，其中 assets 中存放的文件会被 webpack 标定为**模块依赖**，只支持相对路径的形式来访问，components 就是存放 Vue 的各种组件
- static：这里主要存放静态资源，这里面的文件会被原封不动的复制到 `dist/static` 下，通过项目中的 `/config/index.js`中的 `build.assetsPublicPath` 和 `build.assetsSubDirectory` 来进行配置
- .bablerc：babel 的作用就是转 ES5，让更多浏览器支持，`> 1%` 表示为市场份额大于百分之一的浏览器适配，`last 2 version` 表示该浏览器的最新的两个版本，`not ie <= 8` 表示不支持 IE8
- .editorconfig：对代码的一些格式、编码等配置
- .eslintignore：忽略选定文件的 Eslint 检测
- .eslintrc.js：有关 Eslint 的配置



# runtime-compiler 和 runtime-only

看了好半天... 总结就一句，前者包含 `vue-template-compiler` 插件（解析 .vue 文件），后者不包含。当创建后者的时候，需要通过 Webpack 添加这个插件。因为这个插件只是开发时依赖，所以打包项目的时候，这个插件就不会被打包进去，因此项目大小也会相对小一点点点点点



# CLI3

CLI3 对标 Webpack 4，而之前的 CLI2 对标 Webpack 3，CLI3 的设计原则为 **"0配置"**，移除配置文件跟目下的 build 和 config 等目录，同时 CLI3 提供了 Vue UI 命令，**提供可视化配置**，更为人性化。移除了 static 文件夹，增加了 public 文件夹，并且把 idnex.html 移动到了 public 中

注意：这不是 Vue 3！

使用 CLI3 创建项目：（实际安装的为 CLI4）

```
vue create project-name
```



![](https://images-1259064069.cos.ap-guangzhou.myqcloud.com/images/20210614044934.png)

空格选择，回车确定，随后选择 2.x 版本（这里直选 Babel，后面的暂时还用不到）

（目录结构很简单，略）



# CLI3 配置文件的查看和修改

通过使用 `vue ui` 来打开一个在线的 GUI，可以通过图形化的方式来修改项目的配置，如果需要对项目进行一些特殊的配置修改，可在项目根目录下创建一个 `vue.config.js` 文件

```javascript
module.exports = {
    // config...
    // 具体内容后续用到再说
}
```



真实的项目配置是在 `node_modules/@vue` 中的



# 路径别名

在 `webpack.base.config` 中，找到 `alias` 选项，可以在其中添加路径的别名，其中如果使用 CLI2 的话，路径需要写全，比如用 `@` 表示 `src`，那么在配置其他的别名路径的时候不可以使用 `@` 来代替 `src`

```javascript
resolve: {
  extensions: ['.js', '.vue', '.json'],
  alias: {
    '@': resolve('src'),
    'assets': resolve('src/assets'),
    'components': resolve('src/components'),
    'views': resolve('src/views')
  }
},
```



对于 CLI3 来说，配置方式稍微有点区别；首先在根目录下创建文件 `vue.config.js` ，并且写入一下内容：

```javascript
module.exports = {
  configureWebpack: {
    resolve: {
      alias: {
        'aseets': '@/assets',
        'common:': '@/common',
        'components': '@/components',
        'network': '@/network',
        'views': '@/views'
      }
    }
  }
}
```



