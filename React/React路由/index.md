# React路由

## 路由的理解

一个路由就是一个映射关系（`key: value`）。

`key` `为路径，value` 可能是 `function` 或 `component`。

后端路由：value 是 function，用来处理客户端提交的请求。

注册路由：

```js
router.get(path, function(request, response) { ... })
```

工作过程：当 node 接收到一个请求时，根据请求路径找到匹配的路由，调用路由中的函数处理请求，返回响应数据。

****

前端路由：value 是 component，用于展示页面内容。

注册路由：

```html
<Route path="/home" component={ Home } />
```

工作流程：当浏览器的 path 变为 /home 时，当前路由组件就会变为 Home 组件。
