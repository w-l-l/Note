# React-Redux相关

一个 `React` 插件库。

专门用来简化 `React` 应用中使用 `Redux`。

## React-Redux 将所有组件分为两大类

`UI 组件`：

- 只负责 `UI` 的呈现，不带有任何业务逻辑。

- 通过 `props` 接收数据（一般数据和函数）。

- 不使用任何 `Redux` 的 API。

- 一般保存在 `components` 文件夹下。

`容器组件`：

- 负责管理数据和业务逻辑，不负责 `UI` 的呈现。

- 使用 `Redux` 的 API，负责和 `Redux` 通信，将结果交给 `UI` 组件。

- 一般保存在 `containers` 文件夹下。

![React-Redux工作流程](./img/react_redux_process.png)
