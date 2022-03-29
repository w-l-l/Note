# Redux相关

`Redux` 是一个专门用于做状态管理的 js 库（不是 `React` 插件库）。

它可以用在 `React`、`Angular`、`Vue` 等项目中，但是基本与 `React` 配合使用。

集中式管理 `React` 应用中多组件共享的状态。

## Redux 场景

某个组件的状态，需要其他组件可以随时拿到（共享）。

一个组件需要改变另一个组件的状态（通信）。

总体原则：能不用就不用，如果不用比较吃力才考虑使用。

![Redux流程图](./img/redux-process.png)

## Redux 的三个核心概念

`action`：动作的对象，包含 2 个属性。

- `type`：标识属性，值为字符串，唯一，必要属性。

- `data`：数据属性，值类型任意，可选属性。

```js
{ type: 'HANDLE_TYPE', data: { ... } }
```

`reducer`：用于初始化状态、加工状态，本质是一个函数，接收 `prevState`（旧的 `state`）和 `action`，返回加工后的状态。

- `reducer` 被第一次调用时，是 `store` 自动触发的。

- 传递的 `prevState` 是 `undefined`。

- 传递的 `action` 是 `{ type: '@@redux/INIT+随机字符' }`，避免和自定义的 `type` 重复。

```js
export default function reducer(prevState, action) {
  ...
  // 返回一个新的状态
}
```

**注意：reducer 必须是一个纯函数。**

`store`：将 `state`、`action`、`reducer` 联系在一起的对象。

如何得到此对象：

```js
import { createStore } from 'redux'
import reducer from './reducers'
export const store = createStore(reducer)
```

`store` 对象的功能：

- `getState()`：得到 `state`。

- `dispatch(action)`：分发 `action`，触发 `reducer` 调用，产生新的 `state`。

- `subscribe(listener)`：注册监听，当产生新的 `state` 时，自动调用。

`Redux` 只负责管理状态，至于状态的改变驱动着页面的展示，要靠我们自己写。

通常我们都在 `index.js` 中检测 `store` 中 `state` 的改变，一旦发生改变就重新渲染 `<APP />`（不用担心渲染问题，`react` 的 `Diff` 算法会帮助我们，不会出现大量的重排和重绘）。

## Redux 的核心 API

`createStore()`：创建包含指定 `reducer` 的 `store` 对象。

```js
import { createStore } from 'redux'
import reducer from './reducers/xxx'
export default createStore(reducer)
```

`store` 对象：`Redux` 库最核心的管理对象，它内部维护着：`state`、`reducer`。

核心方法：

- `store.getState()`。

- `store.dispatch(action)`。

- `store.subscribe(listener)`。

暴露 `store` 对象：`createStore(reducer)`。

`applyMiddleware`：应用上基于 `Redux` 的中间件（插件库）。

暴露支持异步的 `store` 对象：

```js
import { applyMiddleware } from 'redux'
import { thunk } from 'redux-thunk'
import reducer from './reducers/xxx'
export default createStore(reducer, applyMiddleware(thunk))
```

`combineReducers()`：合并多个 `reducer` 函数。

```js
import { createStore, combineReducers, applyMiddleware } from 'redux'
import { thunk } from 'redux-thunk'
import reducer1 from './reducers/xxx'
import reducer2 from './reducers/xxx'
const reducer = combineReducers({
  reducer1,
  reducer2
})

export default createStore(reducer, applyMiddleware(thunk))
```

## Redux 异步编程

明确：延迟的动作不想交给组件自身，想交给 `action`。

异步 `action` 使用场景：想要对状态进行操作，但是具体的数据靠异步任务返回。

`Redux` 默认是不能进行异步处理的，但是某些场景中需要在 `Redux` 中执行异步任务（ajax，定时任务）。

使用异步中间件：

```js
npm i redux-thunk -S
```

```js
import thunk from 'redux-thunk'
import { createStore, applyMiddleware } from 'redux'
import reducer from './reducers/xxx'
export default createStore(reducer, applyMiddleware(thunk))
```

**注意：异步 action 不再返回一个对象，而是一个函数，该函数中写异步任务。**

```js
import { action } from './actions/xxx'
export const asyncIncrement = data => {
  return dispatch => {
    setTimeout(_ => dispatch(action))
  }
}
```

异步任务有结果后，分发一个同步的 `action` 去真正操作数据。

异步 `action` 不是必须要写的，完全可以自己等待异步任务的结果，再去分发同步 `action`。
