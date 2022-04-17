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

## Dva 路由

路由相关的组件都在 `dva/router` 文件下。

```js
// router.js
import React from 'react'
import { Router, Route, Switch, Redirect } from 'dva/router'
import App from './routes/App'
import Home from './routes/Home'
import About from './routes/About'
import Detail from './routes/Detail'
import Login from './routes/Login'

function RouterConfig({ history }) {
  return (
    <Router history={history}>
      <Switch>
        <Route path='/login' component={Login}></Route>
        <Route path='/' render={props => (
          <App {...props}>
            <Switch>
              <Route path='/home' component={Home}></Route>
              <Route path='/about' render={_ => localStorage.getItem('token') ? <About /> : <Redirect to='/login' />}></Route>
              <Route path='/detail/:id' component={Detail}></Route>
              <Redirect from='/' to='home'></Redirect>
            </Switch>
          </App>
        )} />
      </Switch>
    </Router>
  )
}

export default RouterConfig
```

```js
// routes/App.jsx
import Tabbar from '../components/Tabbar'

export default function App(props) {
  return (
    <div>
      {props.children}
      <Tabbar />
    </div>
  )
}
```

设置路由模式。

```js
// index.js
const app = dva({
  history: require('history').createBrowserHistory() // history 模式
  // history: require('history').createHashHistory() // hahs 模式
});
```

## Dva models

同步操作，实现 tabbar 的显示和隐藏。

```js
// models/tabbar.js
export default {
  namespace: 'tabbar',
  state: {
    isShow: true
  },
  reducers: {
    hide(state, action) {
      return { ...state, isShow: false }
    },
    show(state, action) {
      return { ...state, isShow: true }
    }
  }
}
```

```js
// index.js 注入
app.model(require('./models/tabbar').default)
```

```js
// routes/Home.jsx
import { connect } from 'dva'

function Home(props) {
  return (
    <>
      <h1>Home</h1>
      <button onClick={props.hide}>隐藏tabbar</button>
      <button onClick={props.show}>显示tabbar</button>
    </>
  )
}

export default connect(
  null,
  {
    hide: _ => ({ type: 'tabbar/hide' }),
    show: _ => ({ type: 'tabbar/show' })
  }
)(Home)
```

异步操作，获取信息列表。

```js
// models/person.js
import { getPersonList } from '../services/getPersonList'

export default {
  namespace: 'person',
  state: {
    list: []
  },
  reducers: {
    setPersonList(state, action) {
      return { ...state, list: action.payload }
    }
  },
  effects: {
    *getPersonList(action, { call, put }) {
      const list = yield call(getPersonList)
      yield put({
        type: 'setPersonList',
        payload: list
      })
    }
  }
}
```

```js
// services/getPersonList.js
export function getPersonList() {
  return new Promise(resolve => setTimeout(resolve, 2000, [
    { id: 1, name: '孙悟空' },
    { id: 2, name: '猪八戒' },
    { id: 3, name: '沙和尚' }
  ]))
}
```

```js
// index.js 注入
app.model(require('./models/person').default)
```

```js
// routes/Home.jsx
import { connect } from 'dva'

function Home(props) {
  return (
    <>
      <h1>Home</h1>
      <ul>
        { props.list.map(item => <li key={item.id} onClick={_ => {
          props.history.push(`/detail/${item.id}`)
        }}>{item.name}</li>) }
      </ul>
      <button onClick={props.hide}>隐藏tabbar</button>
      <button onClick={props.show}>显示tabbar</button>
      <button onClick={props.getPersonList}>获取person-list</button>
    </>
  )
}

export default connect(
  state => ({
    list: state.person.list
  }), 
  {
    hide: _ => ({ type: 'tabbar/hide' }),
    show: _ => ({ type: 'tabbar/show' }),
    getPersonList: _ => ({ type: 'person/getPersonList' })
  }
)(Home)
```
