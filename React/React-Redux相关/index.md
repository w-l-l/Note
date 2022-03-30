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

## React-Redux 开发工具的使用

安装插件：

```js
npm i redux-devtools-extension
```

`store` 中进行配置：

```js
import { createStore, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'
import { composeWithDevtools } from 'redux-devtools-extension'
import reducer from './reducer'

export default createStore(reducer, composeWithDevtools(applyMiddleware(thunk)))
```

## 纯函数和高阶函数

`纯函数`：一类特别的函数，只要是同样的输入（实参），必定得到同样的输出（返回）。

必须遵守以下一些约束：

- 不得修改参数数据。

- 不会产生任何副作用，例如：网络请求、输入和输出设备。

- 不能调用 `Date.now()` 或者 `Math.random()` 等不纯的方法。

`Redux` 中的 `reducer` 函数必须是一个纯函数。

`高阶函数`：特别的函数，参数是函数，返回值是函数。

- 常见的高阶函数：`setTimeout`、`setInterval`、`数组的方法`、`Promise`、`react-redux` 中的 `connect` 函数等。

- 作用：能实现更加动态，更加可扩展的功能。

## 代码实现

**目录结构**

```js
|--src
|--|--containers
|--|--|--Count
|--|--|--|--index.jsx
|--|--|--Person
|--|--|--|--index.jsx
|--|--redux
|--|--|--actions
|--|--|--|--count_action.js
|--|--|--|--person_action.js
|--|--|--reducers
|--|--|--|--index.js
|--|--|--|--count_reducer.js
|--|--|--|--person_reducer.js
|--|--|--const.js
|--|--|--store.js
|--|--App.jsx
|--|--index.js
```

**创建 store（redux 目录下）**

```js
// const.js
export const INCREMENT = 'increment'
export const ADD_PERSON = 'add_person'
```

```js
// count_action.js
import { INCREMENT } from '../const'
export const increment = data => ({ type: INCREMENT, data })
```

```js
// person_action.js
import { ADD_PERSON } from '../const'
export const addPerson = data => ({ type: ADD_PERSON, data })
```

```js
// count_reducer.js
import { INCREMENT } from '../const'
const initValue = 0
export default function(prevState = initValue, action) {
  const { type, data } = action
  switch(type) {
    case INCREMENT:
      return prevState + data
    default:
      return prevState
  }
}
```

```js
// person_reducer.js
import { ADD_PERSON } from '../const'
const initPersons = []
export default function(prevState = initPersons, action) {
  const { type, data } = action
  switch(type) {
    case ADD_PERSON:
      return [data, ...prevState]
    default:
      return prevState
  }
}
```

```js
// reducers 下的 index.js
import { combineReducers } from 'redux'
import count from './count_reducer'
import person from './person_reducer'
export default combineReducers({
  count,
  person
})
```

```js
// store.js
import { createStore, applyMiddleware } from 'redux'
import { composeWithDevtools } from 'redux-devtools-extension'
import reducer from './reducers'
import thunk from 'redux-thunk'
export default createStore(reducer, composeWithDevtools(applyMiddleware(thunk)))
```

**index.js 文件中传入 store**

```js
import { Provider } from 'react-redux'
import store from './redux/stores'

ReactDOM.render(
  <Provider store={ store }>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

**容器组件**

```js
// Count
import React from 'react'
import { connect } from 'react-redux'
import { increment } from '../../redux/actions/count_action'

class CountUI extends React.Component { ... }

export default connect(
  state => ({
    count: state.count,
    personNum: state.person.length
  }),
  { increment }
)(CountUI)
```

```js
// Person
import React from 'react'
import { connect } from 'react-redux'
import { addPerson } from '../../redux/actions/person_action'

class PersonUI extends React.Component { ... }

export default connect(
  state => ({
    persons: state.person,
    count: state.count
  }),
  { addPerson }
)(PersonUI)
```

`UI` 组件中直接通过 `props` 的方式获取状态，操作状态就行了。

**App.jsx**

直接引入容器组件挂载页面就行了。
