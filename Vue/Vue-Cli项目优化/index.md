# Vue-Cli项目优化

默认情况下，`vue-cli` 生成的项目，隐藏了 `webpack` 的配置项，如果我们需要配置 `webpack`，需要在项目根目录中创建 `vue.config.js` 文件来进行配置。

## 为开发环境和生产环境指定不同的打包入口

默认情况下，`vue-cli` 项目的开发环境和生产环境，共用同一个打包的入口文件（一般是 `src/main.js`）。

为了将项目的开发过程与生产过程分离，我们可以为两种模式指定不同的入口文件。

- 开发环境：`src/main-dev.js`。

- 生产环境：`src/main-pro.js`。

在 `vue.config.js` 导出的配置对象中，新增了 `configureWebpack` 或 `chainWebpack`，来定义 `webpack` 的打包配置。

两者的作用相同，唯一的区别就是它们修改 `webpack` 配置的方式不同。

- `chainWbpack`：链式编程方式。

- `configureWebpack`：操作对象方式。

通过 `chainWebpack` 自定义打包入口。

```js
// vue.config.js 配置
module.exports = {
  chainWebpack: config => {
    // 生产环境
    config.when(process.env.NODE_ENV === 'production', confog => {
      config.entry('app').clear().add('./src/main-pro.js')
    })
    // 开发环境
    config.when(process.env.NODE_ENV === 'development', config => {
      config.entry('app').clear().add('./src/main-dev.js')
    })
  }
}
```

- `entry`：找到默认的打包入口。

- `clear`：删除默认的打包入口。

- `add`：添加新的打包入口。

## 通过 externals 加载外部 CDN 资源

默认情况下，通过 `import` 语法导入的第三方依赖包，最终会被打包合并到同一个文件中，从而导致打包成功后，单文件体积过大。

可以通过 `webpack` 的 `externals` 节点，来配置并加载外部的 `CDN` 资源，凡是声明在 `externals` 中的第三方依赖，都不会进行打包。

```js
// vue.config.js 配置
module.exports = {
  chainWebpack: config => {
    config.when(process.env.NODE_ENV === 'production', config => {
      config.entry('app').clear().add('./scr/main-pro.js')
      config.set('externals', {
        vue: 'Vue',
        'vue-router': 'VueRouter',
        vuex: 'Vuex',
        axios: 'axios'
        ...
      })
    })
  }
}
```

最后在 `index.html` 文件的头部，添加相关的 `CDN` 资源引用。

## 首页内容定制

不同的打包环境下，首页内容可能会有所不同，我们可以通过插件的方式进行定制，配置如下。

```js
// vue.config.js 配置
module.exports = {
  chainWebpack: config => {
    config.when(peocess.env.NODE_ENV === 'production', config => {
      config.plugin('html').tap(args => {
        args[0].isPro = true
        return args
      })
    })
    config.when(peocess.env.NODE_ENV === 'development', config => {
      config.plugin('html').tap(args => {
        args[0].isPro = false
        return args
      })
    })
  }
}
```

在 `index.html` 首页中，可以根据 `isPro` 的值，来决定如何渲染页面结构。

使用 `<%%>` 脚本片段包裹。

- `<%...%>`：写服务器代码。

- `<%=...%>`：写后台的变量值。

按需渲染页面标题。

```html
<title>
  <%= htmlWebpackPlugin.options.isPro ? '生产环境' : '开发环境' %>
  系统名
</title>
```

按需加载外部的 `CDN` 资源。

```html
<% if(htmlWebpackPlugin.options.isPro) { %>
  <!-- CDN 资源 -->
<% } %>
```

## 路由懒加载

安装 `@babel/plugin-syntax-dynamic-import` 插件。

```js
npm i @babel/plugin-syntax-dynamic-import -D
```

配置 `babel.config.js`。

```js
module.exports = {
  plugins: ['@babel/plugin-syntax-dynamic-import']
}
```

将路由更改为按需加载的形式，修改路由配置文件。

```js
const Home = _ => import(/* webpackChunkName: 'chunk1' */, './Home.vue')
const User = _ => import(/* webpackChunkName: 'chunk1' */, './User.vue')
const About = _ => import(/* webpackChunkName: 'chunk2' */, './About.vue')
const Login = _ => import(/* webpackChunkName: 'chunk2' */, './Login.vue')

new VueRouter({
  routes: [
    { path: '/home', component: Home },
    { path: '/user', component: User },
    { path: '/about', component: About },
    { path: '/login', component: Login }
  ]
})
```

`webpack` 会将任何一个异步模块与相同的块名称组合到相同的异步块中。

## 通过 node 创建 web 服务器

通过 `express` 快速创建 `web` 服务器，将 `vue-cli` 打包生成的 `dist` 文件夹托管为静态资源即可。

```js
// app.js
const express = require('express')
const app = express()
app.use(express.static('./dist'))
app.listen(80, _ => console.log('服务器启动成功...'))
```

### 开启 gzip 压缩配置

使用 `gzip` 可以减少文件体积，使传输速度更快。

安装 `compression`。

```js
npm i compression -S
```

`app.js` 文件配置中间件。

```js
const express = require('express')
const compression = require('compression')
const app = express()
app.use(compression())
app.use(express.static('./dist'))
app.listen(80, _ => console.log('服务器启动成功...'))
```

**注意：开启 gzip 压缩，一定要写到 dist 静态资源托管之前。**

### 配置 https 服务

首先[申请 SSL 证书](https://freessl.org)。

- 进入网站，输入要申请的域名并选择品牌。

- 输入自己的邮箱并选择相关选项。

- 验证 `DNS`（在域名管理后台添加 `TXT` 记录）。

- 验证通过之后，下载 `SSL` 证书。  
`full_chain.pem`：公钥。  
`private.key`：私钥。

在 `app.js` 中导入证书。

```js
const express = require('express')
const compression = require('compression')
const https = require('https')
const fs = require('fs')

const app = express()
// 创建配置对象设置公钥和私钥
const options = {
  cert: fs.readFileSync('./full_chain.pem'),
  key: fs.readFileSync('./private.key')
}
app.use(compression())
app.use(express.static('./dist'))
// 启动 https 服务
https.createServer(options, app).listen(443)
```

### 使用 pm2 管理应用

在服务器中安装 `pm2`。

```js
npm i pm2 -g
```

启动项目。

```js
pm2 start app.js/* 脚本 */ --name/* 自定义名称 */
```

其他命令。

- 查看所有项目：`pm2 ls`。

- 重启项目：`pm2 restart name`。

- 停止项目：`pm2 stop name`。

- 删除项目：`pm2 delete name`。
