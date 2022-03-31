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
