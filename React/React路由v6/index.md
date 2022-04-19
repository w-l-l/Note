# React路由v6

- `react-router`：核心模块，包含 `react` 路由大部分的核心功能，包括路由匹配算法和大部分核心组件和钩子。

- `react-router-dom`：`react` 应用中用于路由的软件包，包括 `react-router` 的所有内容，并添加了一些特定于 DOM 的 API，包括但不限于 `BrowserRouter`、`HashRouter` 和 `Link`。

- `react-router-native`：用于开发 `React Native` 应用，包括 `react-router` 的所有内容，并添加了一些特定于 `React Native` 的 API，包括但不限于 `NativeRouter` 和 `Link`。

**对比 v5。**

- 包大小对比。

![包大小对比](./img/comparison_v5.png)

- `Route` 特性变更。  
`path`：与当前页面对应的 url 匹配。  
`element`：新增，用于决定路由匹配时，渲染哪个组件。代替了 v5 的 `component` 和 `render`。

- `Routes` 组件代替了 `Switch` 组件。

- `Outlet` 让嵌套路由更简单。

- `useNavigate` 代替了 `useHistory`。

- 移除了 `NavLink` 中的 `activeClassName` 和 `activeStyle`。

- 钩子 `useRoutes` 代替 `react-router-config`。
