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

## 解决多路径刷新页面样式丢失的问题

`public/index.html` 中引入第三方资源的时候，开头路径写 `/` 不写 `./` （常用）。

- `./`：相对路径查找。

- `/`：相当于网站根目录查找。

```html
<!-- 资源引入 -->
<link rel="stylesheet" href="/css/xxx.css">
```

网址：`localhost:3000/app/home`。

资源请求：

- `./`：`localhost:3000/app/css/xxx.css`（错误）。

- `/`：`localhost:3000/css/xxx.css`（正确）。

`public/index.html` 中引入第三方资源的时候，开头路径写 `%PUBLIC_URL%` 不写 `./`（常用）。

使用 `<HashRouter />` 模式发送资源请求时，# 号后面的内容会全部忽略。

**注意：在 react 脚手架中，访问路径不存在，会默认返回 public/index.html 页面。**

## 路由的严格匹配和模糊匹配

默认使用的是模糊匹配，输入路径必须包含匹配的路径，且顺序要一致。

```html
<NavLink to="/home/a/b">home</NavLink>
<Route path="/home" component={ Home } />
<!-- 这样也能匹配上的 -->
```

开启严格匹配（`exact`）。

```html
<Route exact path="/home" component={ Home } />
```

**注意：严格模式不要随便开启，需要的时候再开启，有些时候开启会导致无法继续匹配二级路由。**

App 组件：

```html
<Route exact path="/home" component={ Home } />
<Route path="/about" component={ About } />
<Redirect to="/about" />
```

Home 路由组件：

```html
<Route path="/home/a" component={ A } />
<Route path="/home/b" component={ B } />
<Redirect to="/home/b" />
```

如上这种情况，如果 `/home` 开启严格匹配，当我们跳转 `/home/a` 时，`react` 会从注册路由的顺序开始匹配，`/home` 和 `/about` 都不匹配，就会重定向到 `/about`，导致跳转二级路由失败。

如果没有开启严格匹配，`/home` 就会匹配 `/home/a`，渲染 `Home` 组件，不再向下匹配。在 `Home` 组件中再依次匹配路由，匹配到 `/home/a`，成功展示 `A` 路由组件。

## 重定向

一般写在所有路由注册的最下方，当所有路由都无法匹配时，跳转到 `Redirect` 指定的路由。

```html
<Switch>
  <Route path="/home" component={ Home } />
  <Route path="/about" component={ About } />
  <Redirect to="/about" />
</Switch>
```

## 嵌套路由

注册子路由时要写上父路由的 `path` 值。

路由的匹配是按照注册路由的顺序进行的。

```html
<NavLink to="/home/a">home-a</NavLink>
<NavLink to="/home/b">home-b</NavLink>

<Switch>
  <Route path="/home/a" component={ A } />
  <Route path="/home/b" component={ B } />
</Switch>
```

## 向路由组件传递参数

**params 参数：**

路由链接（携带参数）：

```html
<Link to="/demo/name/18">详情</Link>
```

注册路由（声明参数）：

```html
<Route path="/demo/:name/:age" component={ Demo } />
<!-- 参数可选，添加问号 -->
<Route path="/demo/:name?/:age?" component={ Demo } />
```

接收参数：`this.props.match.params`。

****

**search 参数：**

路由链接（携带参数）：

```html
<Link to="/demo?name=name&age=18">详情</Link>
```

注册路由（无需声明，正常注册即可）：

```html
<Route path="/demo" component={ Demo } />
```

接收参数：`this.props.location.search`。

**注意：这种方式获取到的 search 是 urlencoded 编码字符串，需要借助 querystring 解析。**

****

**state 参数**

该方式传递参数不适应于 `HashRouter`，`HashRouter` 刷新页面，传递过来的 `state` 数据会丢失。

路由链接（携带参数）：

```html
<Link to={{ pathname: '/dmeo', state: { name: 'name', age: 18 } }}>详情</Link>
```

注册路由（无需声明，正常注册即可）：

```html
<Route path="/demo" component={ Demo } />
```

接收参数：`this.props.location.state`。

**提醒：地址栏虽然没有参数，但是刷新也可以保留参数。**

## 编程式路由导航

借助 `this.props.history` 对象上的 API，可以操作路由的跳转、前进、后退。

- 切换路由：`this.props.history.push(pathname, state)`。

- 替换路由：`this.props.history.replace(pathname, state)`。

- 后退：`this.props.history.goBack()`。

- 前进：`this.props.history.goForward()`。

- 前进或者后退几步：`this.props.history.go(n)`。

在 `BrowserRouter` 路由中 `push` 和 `replace` 方法可以简写：

```js
this.props.history.push(pathname, state)
this.props.history.replace(pathname, state)
```

在 `HashRouter` 路由中 `push` 和 `replace` 不能简写，而且刷新页面接收的 `state` 数据会丢失。

```js
this.props.history.push({ pathname, state })
this.props.history.replace({ pathname, state })
```

一般组件中进行编程式路由导航，因为一般组件的 `props` 上没有 `history` 对象，我们可以借助 `react-router-dom` 提供的 `withRouter()` 方法，让一般组件具备路由组件特有的 API。

该方法返回一个新的路由组件。

```js
class MyRouter extends React.component { ... }

export default withRouter(MyRouter)
```
