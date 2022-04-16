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

## React.useLayoutEffect

`useLayoutEffect` 的使用方式与 `useEffect` 相同，但它会在所有的 DOM 变更之后同步调用。可以使用它来读取 DOM 布局并同步触发重渲染。在浏览器执行绘制之前，`useLayoutEffect` 内部的更新计划将被同步刷新。

`useEffect` 和 `useLayoutEffect` 的区别：简单来说是调用时机不同，`useLayoutEffect` 和原来 `componentDidMount` & `componentDidUpdate` 一致，在 `react` 完成 DOM 更新后马上同步调用代码，会阻塞页面阻塞。而 `useEffect` 是会在整个页面渲染完才会调用的代码。

官网建议优先使用 `useEffect`。

在实际使用时如果想避免**页面抖动**（在 `useEffect` 里修改 DOM 很有可能出现）的话，可以把需要操作 DOM 的代码放在 `useLayoutEffect` 里。在这里做点 DOM 操作，这些 DOM 修改会和 `react` 做出的更改一起被一次性渲染到屏幕上，只有一次回流、重绘的代价。

```js
import { useEffect } from 'react'

export default function App() {
  useEffect(_ => {
    const box = document.getElementById('box')
    box.style.marginLeft = '500px'
    // 页面绘制完成之后，将元素又移动了，页面又会重新绘制一次，前后两次绘制，就会出现闪屏现象
  }, [])
  return <div id='box' style={{
    width: 100,
    height: 100,
    background: 'red'
  }}>box</div>
}
```

```js
import { useLayoutEffect } from 'react'

export default function App() {
  useLayoutEffect(_ => {
    const box = document.getElementById('box')
    box.style.marginLeft = '500px'
    // 使用 useLayoutEffect 会在 DOM 加载完成，浏览器绘制之前执行，前后就一次绘制操作，所以不会出现闪屏现象
  }, [])
  return <div id='box' style={{
    width: 100,
    height: 100,
    background: 'red'
  }}>box</div>
}
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

## React.useMemo

`useMemo` 记忆组件，`useCallback` 的功能完全可以由 `useMemo` 所取代，如果你想通过 `useMemo` 返回一个记忆函数也是完全可以的。

```js
useCallback(fn, deps) === useMemo(_ => fn, deps)
```

唯一的区别是：`useCallback` 不会执行第一个参数函数，而是将它返回给你，而 `useMemo` 会执行第一个函数并且将函数执行结果返回给你。

所以 `useCallback` 常用记忆事件函数，生成记忆后的事件函数并传递给组件使用。

而 `useMemo` 更适合经过函数计算得到一个确定的值，比如记忆组件。

**记忆值**

```js
import { useMemo, useState } from 'react'

export default function App() {
  const [count, setCount] = useState(0)
  const memo = useMemo(_ => {
    console.log('useMemo')
    let n = count
    for(let i = 0; i < 1000000000; i++) {
      n++
    }
    return n
  }, []) // 因为传的空数组，组件每次更新的时候，不会触发重新计算
  return (
    <>
      <h1>{count}--{memo}</h1>
      <button onClick={_ => setCount(count + 1)}>+1</button>
    </>
  )
}
```

**记忆组件**

```js
// 父组件
import { useMemo, useState } from 'react'
import Child from './Child'

export default function App() {
  const [count, setCount] = useState(0)
  const ChildMemo = useMemo(_ => {
    console.log('useMemo')
    return <Child count={count} />
  }, []) // // 因为传的空数组，父组件每次更新的时候，Child 组件不会触发 render() 重新渲染
  return (
    <>
      {ChildMemo}
      <h1>{count}</h1>
      <button onClick={_ => setCount(count + 1)}>+1</button>
    </>
  )
}
```

```js
// Child 组件
export default function Child(props) {
  console.log('Child组件更新了')
  return <h1>Child组件--{props.count}</h1>
}
```

## React.useReducer

`useReducer` 是 `useState` 的代替方案。它接收一个形如 `(state, action) => newState` 的 `reducer`，并返回当前的 `state` 以及与其配套的 `dispatch` 方法。

```js
import { useReducer } from 'react'

const initValue = 0

function reducer(prevState, action) {
  switch(action.type) {
    case 'increment':
      return prevState + 1
    case 'decrement':
      return prevState - 1
    default:
      return prevState
  }
}

export default function App() {
  const [count, dispatch] = useReducer(reducer, initValue)
  return (
    <>
      <h1>{count}</h1>
      <button onClick={_ => dispatch({ type: 'increment' })}>+1</button>
      <button onClick={_ => dispatch({ type: 'decrement' })}>-1</button>
    </>
  )
}
```

通常我们会配合 `context` 一起使用，达到 `redux` 的效果。

```js
import { useReducer, createContext, useContext } from 'react'

const initValue = 0

function reducer(prevState, action) {
  switch(action.type) {
    case 'increment':
      return prevState + 1
    case 'decrement':
      return prevState - 1
    default:
      return prevState
  }
}

const Context = createContext()

function Increment() {
  const { dispatch } = useContext(Context)
  return <button onClick={_ => dispatch({ type: 'increment' })}>+1</button>
}

function Decrement() {
  const { dispatch } = useContext(Context)
  return <button onClick={_ => dispatch({ type: 'decrement' })}>-1</button>
}

export default function App() {
  const [count, dispatch] = useReducer(reducer, initValue)
  return (
    <Context.Provider value={{dispatch}}>
      <h1>{count}</h1>
      <Increment />
      <Decrement />
    </Context.Provider>
  )
}
```

**注意：useReducer 不支持异步。**

## React.memo

`memo` 使得组件仅在 `props` 发生改变的时候进行重新渲染。通常来说，在组件树中 `react` 组件，只要有变化就会走一遍渲染流程。但是 `memo`，我们可以仅仅让某些组件进行渲染。

`PureComponent` 只能用于 `class` 组件，`memo` 用于函数式组件。

`memo` 仅检查 `props` 变更。如果函数组件被 `memo` 包裹，且其实现中拥有 `useState`、`useReducer` 或 `useContext` 的 Hook，当 `state` 或 `context` 发生变化时，它仍会重新渲染。

```js
// App.tsx
import { useState } from 'react'
import Children from './Children'

export default function App() {
  console.log('App')
  const [count, setCount] = useState(0)
  return (
    <>
      <Children />
      <h1>{count}</h1>
      <button onClick={_ => setCount(count + 1)}>+1</button>
    </>
  )
}
```

```js
// Children.tsx
import { memo } from 'react'

function Children() {
  console.log('Children')
  return <h1>Children</h1>
}

export default memo(Children)
```

Children 组件不使用 `memo` 包裹的时候，App 组件中的状态发生改变，Children 函数就会重新执行。当 Children 组件使用 `memo` 包裹时，因为 `props` 一直没变，所以不会频繁调用。

默认情况下只会对复杂对象做浅层对比，如果你想要控制对比过程，那么请将自定义的比较函数通过第二个参数传入来实现。

```js
export default React.memo(MyComponent, (prevProps, nextProps) => {
  ...
  return booldean
})
```

**注意：与 class 组件中 shouldComponentUpdate() 方法不同的是，如果 props 相等，memo 第二个参数会返回 true；如果 props 不相等，则返回 false。这与 shouldComponentUpdate() 方法的返回值相反。**
