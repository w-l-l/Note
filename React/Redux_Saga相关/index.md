# Redux_Saga相关

`redux-saga` 是一个用于管理应用程序 `Side Effect`（副作用，例如异步获取数据，访问浏览器缓存等）的库。

`redux-saga` 使用了 ES6 的 `Generator` 功能，让异步的流程更易于读取，写入和测试。通过这样的方式，这些异步的流程看起来就像是标准同步的 JavaScript 代码。

你可能已经用了 `redux-thunk` 来处理数据的读取。不同于 `redux-thunk`，你不会再遇到回调地狱了，你可以很容易地测试异步流程并保持你的 `action` 是干净的。

## 可执行生成器

```js
function foo() {
  let n = 0
  return params => new Promise(resolve => setTimeout(resolve, 1000, params + ++n))
}
const getData = foo() // getData 函数每次调用返回一个 promise 对象

// 生成器函数
function *gen() {
  const f1 = yield getData('data')
  console.log(f1)
  const f2 = yield getData(f1)
  console.log(f2)
  const f3 = yield getData(f2)
  console.log(f3)
}

// 可执行生成器函数
function run(gen) {
  const g = gen()
  function next(data) {
    const { value, done } = g.next(data)
    if(done) return value
    value.then(next)
  }
  next()
}

run(gen) // 打印 data1 data12 data123，每次打印间隔 1s
```

## Redux-Saga 基础使用

安装依赖。

```js
npm i redux-saga -S
```

目录结构。

```js
|src
|--redux
|--|--reducer.js
|--|--saga.js
|--|--store.js
|--App.tsx
|--index.tsx
```

```js
// reducer.js
const initState = {
  list: []
}

export default function reducer(prevState = initState, action) {
  const { type, payload } = action
  const state = { ...prevState }
  switch(type) {
    case 'change-list':
      state.list = payload
      return state
    default:
      return prevState
  }
}
```

```js
// saga.js
import { take, fork, call, put } from 'redux-saga/effects'

function *watchSaga() {
  while(true) {
    // 监听组件传来的 action
    yield take('get-list')
    // fork 非阻塞调用的形式执行 getList
    yield fork(getList)
  }
}

function *getList() {
  const res = yield call(getListAction) // call 函数发异步请求，阻塞调用的 getListAction
  yield put({ // put 函数发出新的 action，非阻塞式执行
    type: 'change-list',
    payload: res
  })
}

function getListAction() {
  return new Promise(resolve => setTimeout(resolve, 2000, [111, 222, 333]))
}

export default watchSaga
```

```js
// store.js
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'
import reducer from './reducer'
import saga from './saga'

const sagaMiddleware = createSagaMiddleware()
const store = createStore(reducer, applyMiddleware(sagaMiddleware))
sagaMiddleware.run(saga) // 运行时机必须在 store 创建之后

export default store
```

```js
// App.tsx
import store from './redux/store'

export default function App() {
  return (
    <>
      <ul>
        {store.getState().list.map(item => <li key={item}>{item}</li>)}
      </ul>
      <button onClick={_ => {
        store.dispatch({ type: 'get-list' })
      }}>获取数据</button>
    </>
  )
}
```

```js
// index.tsx
import React from 'react'
import { createRoot } from 'react-dom/client'
import store from './redux/store'
import App from './App'

const container = document.getElementById('root') as HTMLElement
const root = createRoot(container)

function render() {
  root.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>
  )
}
render()

store.subscribe(render)
```

**注意：在组件中触发 saga 函数执行的时候，action.type 的关键字不要和 reducer 中的关键字一致，因为每次 dispatch 的时候会先经过 reducer 函数，如果匹配上可能会产生一些错误。随后再去 saga 中匹配，执行异步函数，让 put 传递 action 通知 reducer 更新 state。**
