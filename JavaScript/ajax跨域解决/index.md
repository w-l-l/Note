# ajax跨域解决

- `jsonp` 请求，只支持 `get` 请求。

- `CORS` 跨域资源共享（设置响应头）。

```js
response.setHeader('Access-Control-Allow-Origin', '*')
// 设置 * 就不能携带 cookie 了，具体地址只写一个的话，可以携带 cookie
response.setHeader('Access-Control-Allow-Header', '*')
response.setHeader('Access-Control-Allow-Method', '*')
respones.setHeader('Access-Control-Max-Age', '1800')
// 表示隔 30 分钟发起预检请求 OPTIONS
```

- `webpack` 里的 `http proxy`。

- `nginx` 反向代理。

- 基于 `postMessage` 实现跨域请求。

- `webscoket` --> `scoket.io`。

- 基于 `iframe` 的跨域解决方案（`window.name`、`document.domin`、`location.hash`）。

**跨域请求允许携带 Cookie**

```js
response.setHeader('Access-Control-Allow-Credentials', 'true')
```
