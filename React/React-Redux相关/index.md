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

## React-Redux 相关 API

`Provider`：让所有组件都可以得到 `state` 数据。

```js
import { Provider } from 'react-redux'
import store from './redux/store'

<Provider store={ store }>
  <App />
</Provider>
```

`connect`：用于包装 `UI` 组件生成新的容器组件。

容器组件中的 `store` 是靠 `<Provider store={ store }></Provider>` 传进来的，而不是在容器组件中引用的。

```js
import { connect } from 'react-redux'

export default connect(mapStateToProps, mapDispatchToProps)('UI组件')
```

- `mapStateToProps`：将外部的数据（即 `state` 对象）转换为 `UI` 组件的标签属性（必须函数返回一个对象）。

```js
const mapStateToProps = function(state) {
  return {
    value: state
  }
}
```

- `mapDispatchToProps`：将分发 `action` 的函数转换为 `UI` 组件的标签属性（设置为函数返回一个对象或者设置为一个对象）。

```js
const mapDispatchToProps = function(dispatch) {
  return {
    funcName: data => dispatch({
      type: 'xxx',
      data
    })
  }
}

/*
  简写
  react-redux 内部会自动进行 dispatch 分发，不需要指明
*/
const mapDispatchToProps = {
  funcName: data => ({ type: 'xxx', data })
}
```

## React-Redux 总结

容器组件和 UI 组件整合到一个文件中。

无需自己给容器组件传递 `store`，给 `<App />` 包裹一个 `<Provider store={ store }></Provider>` 即可。

使用 `react-redux` 后不用自己再检测 `Redux` 中状态的改变了，容器组件可以自动完成这个工作。

```js
store.subscribe(_ => { /* 更新页面操作 */ }) // 这句监听可以去掉了
```

`mapDispatchToProps` 可以简写成一个简单对象。

一个组件要和 `Redux` 打交道要经过哪几步：

- 定义好 `UI` 组件不暴露。

- 引入 `connect` 生成一个容器组件，并暴露。

```js
import { connect } from 'react-redux'

export default connect(
  state => ({ key: value }), // 映射状态
  { funcKey: xxxAction } // 映射操作状态的 action 方法
)('UI组件')
```

- 在 `UI` 组件中通过 `this.props.xxx` 的方式读取 `store` 中的状态或者操作状态。
