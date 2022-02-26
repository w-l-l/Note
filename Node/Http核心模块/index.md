# Http核心模块

## http 相关介绍

`ip` 地址定位计算机。

端口号定位具体的应用程序。

一切需要联网通信的软件都会占用一个端口号。

端口号的范围在 `0-65536` 之间。

在计算机中有一些默认端口号，最好不要去使用。

- 例如：http 服务默认的是 80 端口。

**Content-Type**：

- 服务器最好把每次响应的数据是什么内容类型都告诉客户端，而且要正确的告诉。

- 不同的资源对应的 `Content-Type` 是不一样的，具体参照：[Content-Type大全](http://tool.oschina.net/commons)。

- 对于文件类型的数据，最好都加上编码，目的是为了防止中文解析乱码问题。

通过网络发送文件：

- 发送的并不是文件，本质上来讲发送的是文件的内容。

- 当浏览器收到服务器响应内容之后，就会根据你的 `Content-Type` 进行对应的解析处理。

## web 服务器

在 node 中专门提供了一个核心模块：`http`。

`http` 这个模块的职责就是帮你创建编写服务器的。

服务器的职责：

- 提供服务：数据的服务。

- 发请求。

- 接收请求。

- 处理请求。

- 发送响应。

加载 `http` 核心模块。

```js
const http = require('http')
```

使用 `http.createServer()` 方法创建一个 web 服务器，返回一个 `server` 实例。

```js
const server = http.createServer()
```

注册 request 请求事件。

```js
// 当客户端请求过来，就会自动触发服务器的 request 请求事件，然后执行回调函数
server.on('request', (request, response) => {
  console.log('收到客户端的请求了...')
})
```

绑定端口号，启动服务器。

```js
server.listen(3000 /* 端口号 */, _ => console.log('服务器启动成功...'))
```

**简写方式**：

```js
const http = require('http')
http.createServer((request, response) => {
  console.log('收到客户端的请求了...')
}).listen(3000, _ => console.log('服务器启动成功...'))
```

## request 请求事件

`request` 请求事件处理函数，需要接收 `request` 和 `response` 两个参数：

- `request`：请求对象。  
请求对象可以用来获取客户端大的一些请求信息。  
&emsp;`requset.url`：获取到的是端口号之后的那一部分路径，也就是说所有的 url 都是以 / 开头的。  
&emsp;`request.socket.remoteAddress`：请求我的客户端的 ip 地址。  
&emsp;`request.socket.remotePort`：请求我的客户端程序的端口。

- `response`：响应对象。  
响应对象可以用来给客户端发送响应信息。  
&emsp;`response.write('要返回的数据')`：可以用来给客户端发送响应信息，`response.write` 可以使用多次，但是最后一定要使用 `response.end` 来结束响应，否则客户端会一直等待。  
&emsp;`response.end()`：告诉客户端，我的话说完了，你可以呈递给用户了。也可以在 `response.end('要返回的数据')` 的同时发送响应数据，可以不使用 `response.write()`，这样比较简单。  
**注意：response 响应的内容只能是二进制数据（buffer）后者字符串。**

## Content-Type

在服务器默认发送的数据，其实是 `utf-8` 编码的内容。

但浏览器不知道你是 `utf-8` 编码的内容。

浏览器在不知道服务器响应内容的编码的情况下，会按照当前操作系统的默认编码去解析。

- 中午操作系统的默认编码是 `gbk`。

解决方法就是正确的告诉浏览器我给你发送的内容是什么编码的。

在 `http` 协议中，`Content-Type` 就是用来告知对方我给你发送的数据内容是什么类型。

```js
response.setHeader('Content-Type', 'text/plain;charset=utf-8')
// text/plain：普通文本
// text/html：html 格式的文本
```

## 服务器端渲染

说白了就是在服务器使用模板引擎。

模板引擎最早诞生于服务器，后来才发展到了前端。

### 服务端渲染和客户端渲染的区别

- 客户端渲染不利于 `SEO` 搜索引擎优化。

- 服务端渲染是可以被爬虫抓取到的，客户端异步渲染是很难被爬虫抓取到的。

- 所以你会发现真正的网站既不是纯异步，也不是纯服务端渲染出去的，而是两者结合来做的。  
例如：京东的商品列表就采用的是服务端渲染，目的是为了 `SEO` 搜索引擎优化。而它的商品评论列表为了用户体验，而且不需要 `SEO` 优化，所以采用的是客户端渲染。

### 模板引擎渲染

安装 `art-template`。

```js
npm install art-template
```

使用方法：

要改变的变量用 `{{ var }}` 包裹。

循环使用：`{{each arr}}{{ $value }}{{/each}}`。

```js
const template = require('art-template')
template.render(data.toString(), {
  var: '变量',
  arr: [1, 2, 3]
})
// template.render 第一个参数必须是字符串
```

### 服务端渲染过程

浏览器收到 `html` 响应之后，从上到下依次解析。

在解析的过程中，如果发现：`link`、`script`、`img`、`iframe`、`video`、`audio` 等带有 `src` 或者 `href(link)` 属性标签（具有外链的资源）的时候，浏览器会自动对这些资源发起新的请求。

**注意**：在服务端中，文件中的路径就不要去写相对路径了，因为这个时候所有的资源都是通过 `url` 标识来获取的，不要再想文件路径了，把所有的路径都想象成 `url` 地址。
