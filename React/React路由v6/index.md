# React路由v6

- `react-router`：核心模块，包含 `react` 路由大部分的核心功能，包括路由匹配算法和大部分核心组件和钩子。

- `react-router-dom`：`react` 应用中用于路由的软件包，包括 `react-router` 的所有内容，并添加了一些特定于 DOM 的 API，包括但不限于 `BrowserRouter`、`HashRouter` 和 `Link`。

- `react-router-native`：用于开发 `React Native` 应用，包括 `react-router` 的所有内容，并添加了一些特定于 `React Native` 的 API，包括但不限于 `NativeRouter` 和 `Link`。

**对比 v5。**

- 包大小对比。

![包大小对比](./img/comparison_v5.png)

- `Route` 组件特性变更。  
`path`：与当前页面对应的 url 匹配。  
`element`：新增，用于决定路由匹配时，渲染哪个组件。代替了 v5 的 `component` 和 `render` 属性。

- `Routes` 组件代替了 `Switch` 组件。

- `Outlet` 组件让嵌套路由更简单。

- `useNavigate` 代替了 `useHistory`。

- 移除 `Redirect` 组件，改用 `Navigate` 组件代替。

- 移除了 `NavLink` 中的 `activeClassName` 和 `activeStyle`。

- 钩子 `useRoutes` 代替 `react-router-config`。

## 路由操作

**一级路由**

```js
import { Routes, Route } from 'react-router-dom'

<Routes>
  <Route path='/home' element={<Home />} />
  <Route path='/about' element={<About />} />
</Routes>
```

**路由重定向**

官方推荐方案①：使用 `Navigate` 组件代替。

```js
import { Routes, Route, Navigate } from 'react-router-dom'

<Routes>
  <Route path='/home' element={<Home />} />
  <Route path='/about' element={<About />} />
  <Route path='/' element={<Navigate to='/home' />}>
</Routes>
```

官网推荐②：自定义 `Redirect` 组件。

```js
import { useEffect } from 'react'
import { useNavigate } from 'react-router-dom'

export default function Redirect({ to }) {
  const navigate = useNavigate()
  useEffect(_ => {
    navigate(to, { replace: true })
  }, [to])
  return null
}
```

```js
// 使用
<Route path='/' element={<Redirect to='/home' />}></Route>
```

**404**

```js
// 写在路由规则最后
<Route path='*' element={<NotFound />}></Route>
```

**嵌套路由**

使用 `Outlet` 为路由组件留位置。

```js
<Route path='/about' element={<About />}>
  {/* <Route index element={<Navigate to='list' />}></Route> */}
  <Route index element={<List />}></Route>
  <Route path='list' element={<List />}></Route>
  <Route path='detail' element={<Detail />}></Route>
</Route>
```

```js
// About 路由组件
import { Outlet } from 'react-router-dom'

export default function About() {
  return (
    <>
      <h1>About</h1>
      <Outlet></Outlet>
    </>
  )
}
```

**嵌套路由必须在父级追加 Outlet 组件，作为子级组件的占位符，类似于 vue-router 中的 router-view。**

`index` 属性用于嵌套路由，仅匹配父路径时，设置渲染的组件。

解决当嵌套路由有多个子路由但本身无法确认默认渲染哪个子路由的时候，可以增加 `index` 属性来指定默认路由。`index` 路由和其他路由不同的地方是它没有 `path` 属性，它和父路由共享同一个路径。

如上面示例，设置了 `index` 属性之后，访问 `localhost:8080/about` 和 访问 `localhost:8080/about/list` 渲染的页面是一样的。

如果不喜欢这样，可以写成 `<Route index element={<Navigate to='list' />}></Route>`，访问 `localhost:8080/about` 路径的时候直接重定向到 `localhost:8080/about/list`。

## 声明式导航与编程式导航

声明式导航。

```js
import { NavLink } from 'react-router-dom'

<div className='tabbar'>
  <NavLink to='/home' className={({ isActive }) => isActive ? 'my-class' : ''}>home</NavLink>
  <NavLink to='/about' className={({ isActive }) => isActive ? 'my-class' : ''}>about</NavLink>
</div>
```

编程式导航。

```js
// query 参数
const navigate = useNavigate()
navigate('/about/detail?id=100')

// 获取 query 参数
const [searchParams, setSearchParams] = useSearchParams()
searchParams.get('id') // 100

// 同时页面也可以用 set 方法来改变路由，路由组件将重新执行
setSearchParams({ id: 200 })
```

```js
// params 参数
const navigate = useNavigate()
navigate('/about/detail/100')

// 配置动态路由
<Route path='/about/detail/:id' element={<Detail />} />

// 获取 params 参数
const { id } = useParams() // 100
```

```js
// state 参数
const navigate = useNavigate()
navigate('/about/detail', {
  state: { id: item.id }
})

// 获取 state 参数
const { state: { id } } = useLocation() // 100
```

## 路由拦截

```js
// 路由拦截组件封装
import { Navigate } from 'react-router-dom'
function AuthComponent({ children }) {
  const isLogin = localStorage.getItem('token')
  return isLogin ? children : <Navigate to='/login' />
}
```

```js
// 使用
<Route path='/home' element={
  <AuthComponent>
    <Home />
  </AuthComponent>
}></Route>
```

## 实现 withRouter 类组件跳转方法

v5 版本的时候将普通组件转换为路由组件需要使用 `withRouter` 对普通组件进行包裹，才能使用路由相关的方法。v6 提供了很多 hooks 函数，可以省去这一步骤，在普通组件中也能操作路由。

下面简单封装一下 `withRouter` 的实现。

```js
import { useNavigate, useLocation, useMatch } from 'react-router-dom'

export default function withRouter(Component) {
  return props => {
    const push = useNavigate()
    const location = useLocation()
    const match = useMatch(location.pathname)
    return <Component {...props} history={{push}} location={location} match={match} />
  }
}
```

## 路由懒加载

```js
// 封装函数
import { lazy, Suspense } from 'react'
const LazyLoad = path => {
  const Component = lazy(_ => import(`../views/${path}`))
  return (
    <Suspense fallback={<h1>加载中...</h1>}>
      <Component />
    </Suspense>
  )
}
```

```js
// 使用
<Routes>
  <Route path='/home' element={LazyLoad('Home')}></Route>
  <Route path='/about' element={LazyLoad('About')}></Route>
</Routes>
```

## useRoutes 钩子配置路由

```js
export default function MyRouter() {
  return useRoutes([
    { path: '/login', element: <Login/> },
    { path: '/home', element: LazyLoad('Home') },
    { path: '/about', element: LazyLoad('about'), children: [
      { index: true, element: <Navigate to='list' /> },
      { path: 'list', element: <List /> },
      { path: 'detail', element: (
        <AuthComponent>
          <Detail />
        </AuthComponent>
      ) }
    ] },
    { path: '/', element: <Navigate to='home' /> },
    { path: '*', element: <NotFound /> }
  ])
}
```
