# React_Hooks

`hooks` 是 `React` v16.8.0 版本增加的新特性 / 新语法。

可以让你在函数组件中使用 `state` 以及其他的 `React` 特性。

常用 `hooks`：`React.useState`、`React.useEffect`、`React.useRef`。

## React.useState

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

## React.useEffect

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

## React.useRef()

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

## React.forwardRef

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

## React.useImperativeHandle

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

## React.useCallback

`useCallback` 记忆函数，防止因为组件重新渲染，导致方法被重新创建，起到缓存作用。

```js
import { useCallback, useState } from 'react'

export default function App() {
  const [count, setCount] = useState(0)
  const callback = _ => console.log(count)
  const handleCallback = useCallback(callback, [count])
  /*
    只有 count 改变后，这个函数才会重新声明一次，此时 callback === handleCallback 为 true
    如果传入空数组，那么就是第一次创建后就被缓存了，如果 count 后期改变了，拿到的还是老的 count，此时 callback === handleCallback 为 false
    如果不传第二个参数，每次都会重新声明一次，拿到的就是最新的 count，此时 callback === handleCallback 为 true
  */
  console.log(callback === handleCallback)
  return (
    <>
      <h1>{count}</h1>
      <button onClick={_ => setCount(count + 1)}>+1</button>
      <button onClick={handleCallback}>useCallback</button>
    </>
  )
}
```
