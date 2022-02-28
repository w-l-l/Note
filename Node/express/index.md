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
