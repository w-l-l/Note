# XHR的理解和使用

使用 `XMLHttpRequest(XHR)` 对象可以与服务器交互，也就是发送 `ajax` 请求。

前端可以获取到数据，而无需让整个页面刷新。

这使得 web 页面可以只更新页面的局部，而不影响用户的操作。

## 区别一般的 HTTP 请求与 ajax 请求

`ajax` 请求是一种特别的 `HTTP` 请求。

对服务器端来说没有任何区别，区别在浏览器端。

浏览器端发送请求：只有 `XHR` 或 `fetch` 发出的才是 `ajax` 请求，其他所有的都是非 `ajax` 请求。

浏览器端接收到响应：

- 一般请求：浏览器一般会直接显示响应体数据，也就是我们常说的刷新/跳转页面。

- `ajax` 请求：浏览器不会对界面进行任何更新操作，只是调用监视的回调函数并传入响应相关的数据。

## 使用语法（API）

- `XMLHttpRequest()`：创建 `XHR` 对象的构造函数。

- `status`：响应状态码值，比如 200、404...

- `statusText`：响应状态文本。

- `readyState`：标识请求状态的只读属性。  
0：初始。  
1：`open()` 之后。  
2：`send()` 之后。  
3：请求中。  
4：请求完成。

- `onreadystatechange`：绑定 `readyState` 改变的监听。

- `responseType`：指定响应数据类型，如果是 `json`，得到响应后自动解析响应体数据。

- `response`：响应体数据，类型取决于 `responseType` 的指定。

- `timeout`：指定请求超时时间，默认为 0 代表没有限制。

- `ontimeout`：绑定超时的监听。

- `onerror`：绑定请求网络错误的监听。

- `open(method, url[,isAsync])`：初始化一个请求。

- `send(data)`：发送请求。

- `abort()`：中断请求。

- `getResponseHeader(name)`：获取指定名称的响应头值。

- `getAllResponseHeaders()`：获取所有响应头组成的字符串。

- `setRequestHeader(name, value)`：设置请求头。

## XHR 的 ajax 封装（简易版 axios）

`axios` 特点：

- 函数的返回值为 `Promise`，成功的结果为 `response`，异常的结果为 `error`。

- 能处理多种类型的请求：GET/POST/PUT/DELETE。

- 函数的参数为一个配置对象。

- 响应 json 数据自动解析为 js。

**编码实现**

```js
function axiox({url, method = 'GET', params = {}, data={}}) {
  return new Promise((resolve, reject) => {
    // 处理参数
    method = method.toUpperCase()
    let queryString = Object.keys(params).reduce((result, key) => result += `${key}=${params[key]}&`, '')
    if(queryString) {
      queryString = queryString.slice(0, -1)
      url += '?' + queryString
    }
    // 创建 XHR 对象
    const xhr = new XMLHttpRequest()
    xhr.open(method, url, true)
    if(['GET', 'DELETE'].includes(method)) {
      xhr.send()
    } else if(['POST', 'PUT'].includes(method)) {
      xhr.setRequestHeader('Content-Type', 'application/json;charset=utf-8')
      xhr.send(JSON.stringify(data))
    }
    // 绑定状态改变的监听
    xhr.onreadystatechange = function() {
      if(xhr.readyState != 4) return
      const { status, statusText } = xhr
      if(status >= 200 && status <= 299) {
        const response = {
          data: JSON.parse(xhr.response),
          status,
          statusText
        }
        resolve(response)
      } else {
        reject(new Error('request error status is' + status))
      }
    }
  })
}
```
