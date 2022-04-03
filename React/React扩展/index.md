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

## React Hooks

`hooks` 是 `React` v16.8.0 版本增加的新特性 / 新语法。

可以让你在函数组件中使用 `state` 以及其他的 `React` 特性。

常用 `hooks`：`React.useState`、`React.useEffect`、`React.useRef`。

### React.useState

`useState` 让函数组件也可以有 `state` 状态，并进行状态数据的读写操作。

```js
import { useState } from 'react'
const [state, setState] = useState(initValue)
```

`useState()` 说明：

- 参数：第一次初始化指定的值在内部缓存。

- 返回值：包含两个元素的数组，第一个为内部当前状态值，第二个为更新状态值的函数。

`setState()` 两种写法：

- `setState(newValue)`：参数为非函数值，直接指定新的状态值，内部用其覆盖原来的值。

- `setState((state, props) => newState)`：参数为函数，接收原本的状态值，返回新的状态值，内部用其覆盖原来的状态值。

**注意：React.useState 中的 setState 方法是异步执行的。**

### React.useEffect

`useEffect` 可以让你在函数组件中执行副作用操作（用于模拟类组件中的生命周期钩子）。

`React` 中的副作用操作：

- 发送 ajax 请求数据获取。

- 设置订阅。

- 启动定时器。

- 手动更改真实 DOM。

- ......

```js
import { useEffect } from 'react'

useEffect(_ => {
  // 在此可以执行任何带副作用的操作
  return _ => { // 在组件卸载前执行
    // 在此做一些收尾工作，比如清除定时器、取消订阅等
  }
}, [stateValue]) // 如果指定的是 []，回调函数只会在第一次 render() 后执行，相当于 componentDidMount()
```

可以把 `useEffect` 看做如下三个函数的组合。

`componentDidMount()`。

```js
import { useEffect } from 'react'

useEffect(_ => {
  /*
    如果新的 state 需要通过使用先前的 state 计算得出，那么可以将函数传递给 setState。
    该函数将接收先前的 state，并返回一个更新后的值。
    这个改变状态要使用 setState(state => newState) 函数参数的形式。
  */
}, [])
```

`componentDidUpdate()`。

```js
import { useEffect } from 'react'

useEffect(_ => {
  /*
    第二个参数不传或传递一个带 state 的依赖项数组。
    不传的话，只要有一个 state 发生改变，该回调函数都会执行。
    如果是带 state 的依赖性数组，则只有数组中的 state 发生改变，该回调才会执行，数组外的 state 发生改变，该回调不会执行。
  */
}, [state])
```

`componentWillUnmount()`。

```js
import { useEffect } from 'react'

useEffect(_ => {
  return _ => {
    /*
      卸载组件时执行的回调
      当 useEffect 第二个参数不传，或传一个依赖项数组时，状态发生改变，该卸载回调函数也会执行，先执行卸载回调，再执行 useEffect 的第一个参数回调
    */
  }
}, [])
```

### React.useRef()

`useRef` 可以在函数组件中存储 / 查找组件内的标签或任意其他数据。

- 获取 DOM 元素。

```js
import { useRef } from 'react'

export default function Home() {
  const h = useRef()
  return <h1 ref={h}>Home</h1>
}
// h.current --> h1
```

- 保存引用值

```js
import { useRef, useState } from 'react'

export default function Home() {
  const [count, setCount] = useState(0)
  const ref = useRef(0)
  return (
    <>
      <h1>{count}---{ref.current}</h1> {/* 每次组件更新时 ref.current 的值都是最新的 */}
      <button onClick={_ => {
        setCount(count + 1)
        ref.current++
      }}>+1</button>
    </>
  )
}
```

```js
import { useState } from 'react'

export default function Home() {
  const [count, setCount] = useState(0)
  let ref = 0
  return (
    <>
      <h1>{count}---{ref}</h1> {/* 每次组件更新时 ref 的值都是 0 */}
      <button onClick={_ => {
        setCount(count + 1)
        ref++
      }}>+1</button>
    </>
  )
}
```

### React.forwardRef

`forwardRef` 会创建一个 `React` 组件，这个组件能够将其接收的 `ref` 属性转发到其组件树下的另一个组件中。

该方法接受渲染函数作为参数。`React` 将使用 `props` 和 `ref` 作为参数来调用此函数。此函数返回 `React` 节点。

- 转发 `refs` 到 DOM 组件。

- 在高阶组件中转发 `refs`。

```js
// 父组件
import { useRef } from 'react'
import Home from './Home'

export default function App() {
  const homeRef = useRef()
  return (
    <>
      <Home ref={homeRef} />
      {/* ref 不能挂在到函数组件上的，因为函数组件没有实例，所以要使用 forwardRef 进行转发 */}
      <button onClick={ _ => console.log(homeRef)}>查看Home组件ref</button>
      {/* 获取 Home 组件中的 ref */}
    </>
  )
}
```

```js
// Home 组件
import { forwardRef } from 'react'

export default forwardRef((props, ref) => {
  return <h1 ref={ref}>Home</h1>
})
```

### React.useImperativeHandle

`useImperativeHandle` 可以让你在使用 `ref` 时自定义暴露给父组件的实例值。

在大多数情况下，应当避免使用 `ref` 这样的命令式代码。

**useImperativeHandle 应当与 forwardRef 一起使用。**

`useImperativeHandle` 为我们提供了一个类似实例的东西。它帮助我们通过 `useImperativeHandle` 的第 2 个参数，所返回的对象的内容挂载到父组件的 `ref.current` 上。让父组件可以直接操作函数子组件中的方法。

第三个参数是状态依赖性数组，只有当第三个参数中的状态发生改变的时候，父组件接收到的值才会发生变化。不写第三个参数父组件接收到的值则是最新的。空数组的话，父组件接收到的值则一直不变。

```js
// 父组件
import { useRef } from 'react'
import Home from './Home'

export default function App() {
  const homeRef = useRef()
  return (
    <>
      <Home ref={homeRef} />
      <button onClick={_ => {
        const { handleChild, count } = homeRef.current
        handleChild()
        console.log(count)
      }}>调用函数子组件方法</button>
    </>
  )
}
```

```js
// Home 组件
import { forwardRef, useImperativeHandle, useState } from 'react'

export default forwardRef((props, ref) => {
  const handleChild = _ => console.log('我是Home组件方法')
  const [count, setCount] = useState(0)
  useImperativeHandle(ref, _ => ({
    handleChild,
    count
  }), [])
  /*
    传空数组的话，当 count 为 10 时，父组件接收到的 count 还是 0
    不传第三个参数或者传 [count]，父组件接收到的值才是最新的
  */
  return (
    <>
      <h1>{ count }</h1>
      <button onClick={_ => setCount(count + 1)}>+1</button>
    </>
  )
})
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
