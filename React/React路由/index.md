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

## 路由的基本使用

安装 `react-router-dom`。

```js
npm i react-router-dom -S
```

明确好界面中的导航区、展示区。

导航区啊 `a` 标签改为 `Link` 标签。

```html
<Link to="/xxx">Demo</Link>
```

展示区写 `Route` 标签进行路径匹配。

```html
<Route path="/xxx" component={ Demo } />
```

`<App />` 组件的最外侧包裹一个 `<BrowserRouter />` 或 `<HashRouter />`

## react-router-dom 相关组件

- `<BrowserRouter />`：history 模式。

- `<HashRouter />`：hash 模式。

- `<Route />`：注册路由。

```html
<Route path="/home" exact component={ Home } />
<!-- exact：开启严格匹配 -->
```

- `<Redirect />`：路由重定向。

```html
<Redirect to="/home" />
<Redirect from="/xxx" to="/xxx" />
<!-- 访问 from 的地址，重定向到 to 的地址 -->
```

- `<Link />`：路由链接。

```html
<Link to="/home" replace>Home</Link>
```

- `<NavLink />`：实现路由链接高亮。

```html
<NavLink activeClassName="active" replace to="/home">Home</NavLink>
<!-- activeClassName：路由激活时添加的类名 -->
<!-- replace：路由记录替换 -->
```

- `<Switch />`：通常情况下，`path` 和 `component` 是一一对应的关系。  
`Switch` 可以提高路由匹配效率（单一匹配），匹配成功一个就不会向下匹配了。

```html
<Switch>
  <Route path="/home" component={ Home } />
  <Route path="/about" component={ About } />
  <Redirct to="/home" />
</Switch>
```

## 路由组件与一般组件

写法不同：

```html
<!-- 一般组件 -->
<Demo />
```

```html
<!-- 路由组件 -->
<Route path="/demo" component={ Demo } />
```

存放位置不同：

- 一般组件：放在 `components` 目录下。

- 路由组件：放在 `pages` 目录下。

接收到的 `props` 不同：

- 一般组件：写组件标签传递什么，`props` 就能收到什么。

- 路由组件：接收到 3 个固定属性 `history`、`location`、`match`。
