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
    */
  }
}, [])
```

### React.useRef()

`useRef` 可以在函数组件中存储 / 查找组件内的标签或任意其他数据。

```js
import { useRef } from 'react'

export default function Home() {
  const h = useRef()
  return <h1 ref={h}>Home</h1>
}
// h.current --> h1
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
