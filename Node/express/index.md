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
