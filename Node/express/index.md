# express

安装 `express`。

```js
npm i express --save
```

引入 `express`。

```js
const exprss = require('express')
```

创建服务器应用程序。

```js
const exprss = require('express')
const app = exprss()
```

设置端口。

```js
const exprss = require('express')
const app = exprss()
app.listen(3000, _ => console.log('服务器启动成功...'))
```

当服务器收到 `get` 请求时，执行回调函数。

- `app.get('/[url地址]', (request, response) => { ... })`  
request.query：获取查询字符串参数。  
response.send('响应内容')：向浏览器发送响应，send 内部会自行处理数据。  
response.render('文件名', { 模板对象 })：模板渲染，需要进行配置才能使用。  
response.redirect([status], path)：重定向，可自行设置状态码。  
&emsp;**注意：服务端的重定向针对异步请求是无效的，客户端异步操作需要重定向必须在客户端操作，服务端操作无效。**  
response.status(code)：设置 http 状态码，express 默认是 200。  
&emsp;404：页面不存在。  
&emsp;500：服务器内部错误。  
&emsp;301：永久重定向。  
&emsp;302：临时重定向。  
response.json(data)：传送 json 响应。 
response.jsonp(data)：传送 jsonp 响应。

```js
const exprss = require('express')
const app = exprss()
app.listen(3000, _ => console.log('服务器启动成功...'))
app.get('/', (request, response) => {
  response.send('访问 exprss 根路径')
})
```

`response.send`、`response.redirect`、`response.render` 这些方法 express 会自动结束响应。

## nodemon 自动重启服务

全局安装 `nodemon`。

```js
npm i nodemon -g
```

执行 js 文件。

```js
nodemon xxx.js
```

将 node 命令转换成 nodemon 命令，每次修改保存文件，服务器就会自动重启。

## 静态资源管理

```js
app.use(express.static('静态文件夹'))
```

`express` 在静态目录查找文件，因此，存放静态文件的目录名不会出现在 url 中。

例如：`app.use(express.static(public))`，public 文件夹中有一张 `a.png`。在浏览器输入 `http://127.0.0.1:3000/a.png` 就能访问到。

**注意：如果存在多个静态资源目录，目录中有相同名字的文件，express.static 中间件函数会根据目录的添加顺序查找所需要的文件，只要找到就立刻返回，不会向下查找。**

所以最好为静态资源设置不同的前缀。

```js
// 为服务器创建虚拟路径前缀（文件系统中实际不存在该路径）
app.use('/前缀', express.static('静态文件夹'))
```

例如：

```js
app.use('/use', express.static('public')) // public 文件夹中有一张 a.png
```

`http://127.0.0.1:3000/use/a.png` 这样才能访问到 a.png。

## 路由

路由其实就是一张表，这个表有具体的映射关系。

`express` 设置路由可以链式调用。

```js
app.get('/', callback)
.get('/login', callback)
.post('/user', callback)
.listen(3000, _ => console.log('服务器启动成功...'))
```

## express 模板渲染（配置使用 art-template 模板引擎）

安装相关的模块包。

```js
npm i art-template express-art-template --save
```

使用：

```js
const express = require('express')
const app = express()
app.engine('html', require('express-art-template'))
```

第一个参数表示当以 .html 结尾文件的时候，可以使用 `art-template` 模板渲染。

`express-art-template` 是专门用来在 `express` 中把 `art-template` 整合到 `express` 中。

虽然外面不需要引入 `art-template`，但还是要安装，因为 `express-art-template` 内部依赖于 `art-template`。

```js
app.get('/', (request, response) => {
  // 用法和 art-template 是一样的
  response.render('index.html', { ... })
})
```

`response.render` 方法默认是不可以使用的，但是配置了模板引擎就可以使用了。

`response.render('模板名(或 views 下的路径)', { 模板数据 })`：默认会去项目中的 views 目录下查找该模板文件，也就是说 `express` 有一个约定，开发人员把所有的视图文件都放到 views 目录中。

该目录名也是可以修改的：

```js
app.set('views', '目录路径')
```

## express 中配置解析表单 post 提交请求

安装中间件。

```js
npm i body-parser --save
```

配置 `body-parser`。

```js
const express = require('express')
const bodyParser = require('body-parser')
const app = express()
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
```

只要加入这个配置，则在 `request` 请求对象上会多出一个属性 `body`。

`request.body` 可以来获取表单的 post 请求。

```js
app.post('/post', (request, response) => {
  response.send(request.body)
})
```

`express` 最新版本可以直接获取 post 请求数据，不允许安装 `body-parser`，低版本才需要安装。

```js
app.use(express.urlencoded({ extended: false }))
app.use(express.json())
app.post('/post', (request, response) => {
  console.log(request.body)
})
```

## express 路由模块的提取

职责：

- 处理路由。

- 根据不同的请求方法 + 请求路径设置具体的请求处理函数。

- 模块的职责要单一，不要乱写。

- 我们划分模块的目的就是为了增强项目代码的可维护性，提高开发效率。

创建 `router.js`：

```js
const express = require('express')
const router = express.Router() // 创建一个路由器
// 将路由都挂载到 router 路由器中
router.get('/', callback)
router.post('/post', callback)

// 将 router 暴露出去
module.exports = router
```

入口文件中：

```js
const express = require('express')
const app = express()
// 引入路由模块
const router = require('./router')
// 将路由器挂载到 app 服务
app.use(router)
```

**注意：配置模板引擎和 body-parser 一定要在 app.use(router) 挂载路由之前**。

## express 中的 session 操作

安装中间件：

```js
npm i express-session --save
```

配置：

```js
const session = require('express-session')
const express = require('express')
const app = express()
app.use(session({
  // 配置加密字符串，它会在原有加密基础之上和这个字符串拼起来去加密。目的是为了增加安全性，防止客户端恶意伪造
  secret: 'xxx',
  // 强制保存 session，即使它没有变化。默认是 true，建议设置成 false，session 变化时再保存
  resave: false,
  // 无论你是否使用 session，我都默认直接给你分配一把钥匙（cookie 存在浏览器上）。
  saveUninitialized: true
}))
```

配置好之后可以通过 `request.session` 来访问和设置 `session` 成员了。

- 添加 session 数据：request.session.key = value。

- 访问 session 数据：request.session.key。

- 删除 session 数据：request.session.key = null。  
更严谨的做法是 `delete` 语法：delete request.session.key。

**注意：默认 session 数据是内存存储的，服务器一旦重启就会丢失，真正的生产环境会把 session 进行持久化存储**。

## express 中间件概念

中间件：处理请求的，本质就是一个函数。

**注意：同一个请求经过的中间件都是同一个请求对象和响应对象**。

在 `express` 中，对中间件有几种分类。

1. `app.use(url, (req, res, next) => { ... })`：以 `/xxx` 开头。  
当请求进来，会从第一个中间件开始进行匹配，如果匹配，则进来。  
如果请求进入中间件之后，没有调用 `next`，则代码会停在当前中间件。  
如果调用了 `next`，则继续向后找到第一个匹配的中间件，如果不匹配，则继续匹配下一个中间件。

2. `app.use((req, res, next) => { ... })`：万能匹配。  
不关心请求路径和请求方法的中间件。  
也就是说任何请求都会进入这个中间件。  
中间件本身是一个方法，该方法接收三个参数：  
&emsp;request：请求对象。  
&emsp;response：响应对象。  
&emsp;next：下一个中间件。  
当一个请求进入中间件之后，如果不调用 `next`，则会停留在当前中间件。  
所以 `next` 是一个方法，用来调用下一个中间件的。  
调用 `next` 方法也是要匹配的，满足条件才调用，不是调用紧挨着的那个。

**以上两个称为应用程序中间件**。

3. 除了以上中间件之外，还有一种最常用的 `路由级别中间件`。  
严格匹配请求方法和请求路径的中间件：`app.get`、`app.post`、`app.put`、`app.delete`。

4. 错误处理中间件：

```js
app.use((err, req, res, next) => {
  console.log(err.stack)
  res.status(500).send('something broke')
})
```

错误处理中间件一般放在最后，中间件跟先后顺序有关，当上面的中间件调用 `next` 传参的时候，则直接往后找到带有 4 个参数的应用程序级别的中间件（必须是 4 个参数）。

5. 内置中间件：`express.static`、`express.json`、`express.urlencoded` 等等。

6. 第三方中间件：`body-parser`、`express-art-template` 等等。

**注意：如果没有匹配的中间件，则 express 会默认输出 Cannot GET 路径**。
