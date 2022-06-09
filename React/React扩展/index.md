# React扩展

## setState 更新状态的两种方法

对象式 `setState`：`setState(stateChange, [callback])`。

- `stateChange`：状态改变对象（该对象可以体现出状态的更改）。

- `callback`：可选的回调函数，它在状态更新完毕、界面也更新后才被调用（在 `componentDidUpdate` 之后调用）。

函数式 `setState`：`setState(updater, [callback])`。

- `updater`：返回 `stateChange` 对象的函数，可以接收 `state` 和 `props`。

- `callback`：可选的回调函数，它在状态更新完毕、界面也更新后才被调用（在 `componentDidUpdate` 之后调用）。

**总结**

- 对象式的 `setState` 是函数式的 `setState` 的简写方式（语法糖）。

- 如果新状态不依赖与原状态，使用对象方式。

- 如果新状态依赖原状态，使用函数方式。

- 如果需要在 `setState()` 执行后获取最新的数据，要在第二个 `callback` 函数中读取。

**setState 的异步和同步**

`setState` 只在合成事件和钩子函数中是异步的，在原生异步事件中都是同步的（比如通过原生绑定的事件回调、`setTimeout`、`Promise` 等）。

在合成事件中多次调用 `setState`，传递对象的方式会被合并成一次，使用函数传递的方式不会被合并。

`setState` 处在同步的逻辑中，异步更新状态，异步更新真实 DOM。

`setState` 处在异步的逻辑中，同步更新状态，同步更新真实 DOM。

## 路由组件的 lazyLoad

通过 `React` 中的 `lazy` 函数，配合 `import()` 函数动态加载路由组件（路由组件代码会被分开打包）。

```js
import { lazy } from 'react'
const Login = lazy(_ => import('@/pages/Login'))
```

通过 `<Suspense />` 指定在加载得到路由打包文件前显示一个自定义 `loading` 界面。

```js
import { Suspense } from 'react'

<Suspense fallback={ <h1>loading...</h1> }>
  <Switch>
    <Route path="/xxx" component={ xxx } />
    <Redirect to="/login">
  </Switch>
</Suspense>
```

## Fragment

作用：可以不用必须有一个真实的 DOM 根标签。

```js
import { Fragment } from 'react'

export default function App() {
  return (
    <Fragment>
      <h1>App</h1>
      <h1>App</h1>
    </Fragment>
  )
}
```

**注意：Fragment 标签只能写 key 和 children 这两个标签属性。**

**<></> 不能写任何标签属性。**

```js
import { Fragment } from 'react'

export default function App(props) {
  return (
    <Fragment key={'app'} children={props.children}></Fragment>
  )
}
```

所以在遍历的时候，只能使用 `Fragment` 标签来遍历。

```js
this.state.data.map(item => (
  <Fragment key={item.id}>
    <div>{item.name}</div>
    <div>{item.phone}</div>
  </Fragment>
))
```

## createContext

一种组件间通信方式，常用于【祖组件】和【后代组件】通信。

创建 `Context` 容器对象。

```js
import { createContext } from 'react'

const myContext = createContext()
```

渲染子组件时，外面包裹 `myContext.Provider`，通过 `value` 属性给后代传递数据。

```js
<myContext.Provider value={data}>
  {/* 子组件 */}
</myContext.Provider>
```

后代组件读取 `Context` 数据。

- 第一种方式：仅适用于类组件。

```js
import React from 'react'

export default class App extends React.Component {
  static contextType = myContext // 声明接收 context
  render() {
    return <div>{this.context}</div> // 读取 context 中的 value 数据
  }
}
```

- 第二种方式：仅适用于函数组件。

```js
import { useContext } from 'react'

export default function App() {
  const context = useContext(myContext)
  return <div>{context}</div>
}
```

- 第三种方式：函数组件和类组件都适用。

```js
<myContext.Consumer>
  {
    value => (/* 要显示的内容，value 就是 context 中的 value 数据 */)
  }
</myContext.Consumer>
```

**注意：在应用开发中一般不用 context，一般都是用它来封装 react 插件。**

传递多个上下文对象。

```js
const OneContext = React.createContext()
const TwoContext = React.createContext()

<OneContext.Provider value={oneData}>
  <TwoContext.Provider value={twoData}>
    {/* 后代组件两个上下文对象都能使用 */}
  </TwoContext.Provider>
</OneContext.Provider>
```

## 组件优化

`React.Component` 存在的两个问题：

- 只要执行了 `setState()`，即使不改变状态数据，组件也会重新 `render()`，效率低。

- 只当前组件重新 `render()`，就会自动重新 `render()` 子组件，即使子组件没有用到父组件的任何数据，效率低。

产生问题的原因：`React.Component` 中的 `shouldComponentUpdate()` 默认总是返回 `true`。

提高效率的做法：只有当组件的 `state` 或 `props` 数据发生改变时才更新 `render()`。

**解决方法：**

- 重写 `shouldComponentUpdate()` 方法，比较新旧 `state` 或 `props` 数据，如果有变化才返回 `true`，没变化就返回 `false`。

- 使用 `React.PureComponent`。  
`PureComponent` 重写了 `shouldComponentUpdate()`，只有 `state` 或 `props` 数据发生改变才会返回 `true`。  
项目中一般使用 `PureComponent` 来优化。

**注意：PureComponent 只是进行了 state 和 props 数据的浅比较，如果只是数据对象内部数据变了，shouldComponentUpdate 返回 false，不执行 render()。**

```js
import React from 'react'

export default class App extends React.Component {
  state = {}
  render() {
    console.log('render')
    return <button onClick={_ => this.setState({})}>更新</button>
  }
}
```

以上每次点更新按钮都会触发 `render()` 调用。

```js
import React from 'react'

export default class App extends React.PureComponent {
  state = {}
  render() {
    console.log('render')
    return <button onClick={_ => this.setState({})}>更新</button>
  }
}
```

使用 `PureComponent` 后，点击更新按钮，状态没发生改变，不会触发 `render()` 调用。

**注意：不要直接修改 state 数据，而是要产生新数据。**

```js
state.push(xxx)
this.setState(state) // 错误的

this.setState({ ...this.state, ...newData }) // 正确的
```

## render props

如何向组件内部动态传入带标签的结构（标签）？

- `Vue` 中：使用 `slot` 插槽技术，也就是通过组件标签体传入结构（`<A><B /></A>`）。

- `React` 中：  
使用 `children props`，通过组件标签体传入结构。  
使用 `render props`，通过组件标签属性出传入结构，而且可以携带数据，一般用 `render` 函数属性。

**children props：**

```js
// 父组件
import A from './components/A'
import B from './components/B'

export default function App() {
  return (
    <A>
      <B />
    </A>
  )
}
```

```js
// A 组件
export default function A(props) {
  return (
    <>
      <h1>我是A组件</h1>
      <div>{props.children}</div>{/* 这里将渲染 B 组件的内容 */}
    </>
  )
}
```

```js
// B 组件
export default function B() {
  return <h1>我是B组件</h1>
}
```

A 组件中：`{ props.children }`。

缺点：B 组件不能使用 A 组件中的数据。

**render props：**

```js
// 父组件
import A from './components/A'
import B from './components/B'

export default function App() {
  return <A render={data => <B {...data} />} />
}
```

```js
// A 组件
import { useState } from 'react'

export default function A(props) {
  const [count, setCount] = useState(0)
  return (
    <>
      <h1>我是A组件--{count}</h1>
      <button onClick={_ => setCount(count + 1)}>+1</button>
      <div>{props.render({ count })}</div>{/* 这里将渲染 B 组件的内容，并将数据传递给 B 组件 */}
    </>
  )
}
```

```js
// B 组件
export default function B(props) {
  return <h1>我是B组件，接收到A组件传递过来的值--{props.count}</h1>
}
```

A 组件中：`{ props.render(内部数据) }`。

B 组件中：读取 A 组件传入的数据显示 `{ props.data }`

## 错误边界

错误边界（`Error boundary`）：用来捕获后代组件错误，渲染出备用页面。

特点：只能捕获后代组件生命周期产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误。

使用方式：`getDerivedStateFromError` 配合 `componentDidCatch`。

```js
import React from 'react'

export default class App extends React.Component {
  state = {
    isError: false
  }
  // 生命周期函数，一旦后代组件报错，就会触发
  static getDerivedStateFromError(error) {
    conosle.log(error)
    // 在 render 之前触发，返回新的 state
    return {
      isError: true
    }
  }
  componentDidCatch(error, info) {
    // 统计页面的错误，发送请求给后台
    console.log(error, info)
    /*
      error：抛出的错误
      info：带有 componentStack key 的对象，其中包含有关组件引发错误的栈信息
    */
  }
  render() {
    return <div>{this.state.isError ? '网络繁忙，请稍后再试' : this.props.children}</div>
  }
}
```

**注意：错误边界是用于可能报错组件的父组件的，生产环境下才有效果。**

## 组件通信方式总结

组件间的关系：

- 父子组件。

- 兄弟组件（非嵌套组件）。

- 祖孙组件（跨级组件）。

几种通信方式：

- props：  
children props。  
render props。

- 消息订阅-发布：  
pubs-sub、event 等等。

- 集中式管理：  
redux、dva 等等。

- 生产者-消费者模式：context。

比较好的搭配方式：

- 父子组件：props。

- 兄弟组件：消息订阅-发布、集中式管理。

- 祖孙组件：消息订阅-发布、集中式管理、context（开发用的少，封装插件用的多）。

## dangerouslySetInnerHTML

`dangerouslySetInnerHTML` 是 `React` 为浏览器 DOM 提供 `innerHTML` 的代替方案。

通常来讲，使用代码直接设置 `HTML` 存在风险，因为很容易无意中使用户暴露与 `XSS`（跨站脚本） 的攻击。

因此，你可以直接在 `React` 中设置 `HTML`，但当你想设置 `dangerouslySetInnerHTML` 时，需要向其传递 `key` 为 `__html` 的对象，以此来警示你。

```js
export default function App() {
  return <div dangerouslySetInnerHTML={{ __html: '<h1>App</h1>' }}></div>
}
```

## Portal

`Portal` 提供了一个最好的在父组件包含的 DOM 结构层级外的 DOM 节点渲染组件的方法。

```js
ReactDOM.createPortal(child, container)
```

第一个参数 child 是可渲染的 `react` 子项，比如元素、字符串或者片段等。第二个参数 container 是一个 DOM 元素。

```js
// App.tsx
import PortalDialog from './PortalDialog'

export default function App() {
  return (
    <div>
      <PortalDialog>
        <h1>PortalDialog</h1>
      </PortalDialog>
      <h1>App</h1>
    </div>
  )
}
```

```js
// PortalDialog.tsx
import { createPortal } from 'react-dom'

interface Props {
  children?: any
}

export default function PortalDialog(props: Props) {
  const portal = createPortal(
    <div className='portal'>
      {props.children}
    </div>,
    document.body
  )
  return portal
}
```

PortalDialog 组件并没有挂载在 App 组件内部，而是挂载到了 `body` 标签中的最后。

虽然通过 `portal` 渲染的元素在父组件的盒子之外，但是渲染的 DOM 节点仍在 `react` 的元素树上，在 DOM 元素上的点击事件仍然能在 DOM 树中监听到。

```js
// App.tsx
import PortalDialog from './PortalDialog'

export default function App() {
  return (
    <div onClick={_ => console.log('App身上的监听事件')}>
      <PortalDialog>
        <h1>PortalDialog</h1>
      </PortalDialog>
      <h1>App</h1>
      <button onClick={_ => console.log('App-button')}>App-button</button>
    </div>
  )
}
```

```js
// PortalDialog.tsx
import { createPortal } from 'react-dom'

interface Props {
  children: any
}

export default function PortalDialog(props: Props) {
  const portal = createPortal(
    <div className='portal'>
      {props.children}
      <button onClick={_ => console.log('PortalDialog-button')}>PortalDialog-button</button>
    </div>,
    document.body
  )
  return portal
}
```

当我们点击 App-button 这个按钮时，App 最外层的点击监听会触发，因为事件冒泡的原因。

PortalDialog 组件虽然没挂载到 App 组件内，但是点击 PortalDialog-button 这个按钮时，App 最外层的点击监听也会触发。

在父组件里捕获一个来自 `portal` 冒泡上来的事件，使之能够在开发时具有不完全依赖于 `portal` 的更为灵活的抽象。无论是否采用 `portal` 实现，父组件都能够捕获其事件。
