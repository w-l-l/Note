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
