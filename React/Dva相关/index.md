# Dva相关

`dva` 首先是一个基于 `redux` 和 `redux-saga` 的数据流方案，然后为了简化开发体验，`dva` 还额外内置了 `react-router` 和 `fetch`，所以也可以理解为一个轻量级的应用框架。

全局安装 `dva`。

```js
npm install dva-cli -g
```

创建 `dva` 应用。

```js
dva new projectName
```

src 目录结构。

```js
|--src
|--|--assets // 资源目录
|--|--components // 组件目录
|--|--models // 模型（redux、redux-saga）目录
|--|--routes // 路由组件目录
|--|--services // 请求方法目录
|--|--utils // 工具库目录
|--|--index.js // 入口文件
|--|--router.js // 路由配置文件
```
