# HTTP相关

## HTTP 请求交互的基本过程

前台应用从浏览器端向服务器发送 `HTTP` 请求（请求报文）。

后台服务器接收到请求后，调度服务器应用处理请求，向浏览器端返回 `HTTP` 响应（响应报文）。

浏览器端接收到响应，解析显示响应体/调用监视回调。

![请求大致流程](./img/HTTP_process.png)

## HTTP 请求报文

- 请求行：`method`、`url`。

```bash
GET /user?id=1
POST /login
```

- 请求头（多个）：`headers`。

```bash
Host: www.baidu.com
Cookie: BAIDUID=AD3B0FA706E; BIDUPSID=AD3B0FA706
Content-Type: application/x-www-form-urlencoded
# 或者
Content-Type: application/json
```

- 请求体：`body`。

```bash
username=wll&pwd=123

{
  "username": "wll",
  "pwd": 123
}
```
