# React生命周期

组件从创建到死亡它会经历一些特定的阶段。

`react` 组件中包含一系列钩子函数（生命周期回调函数），会在特定的时刻调用。

我们在定义组件时，会在特定的生命周期回调函数中，做特定的工作。

## 旧生命周期

![旧生命周期流程图](./img/lifecycle_old.png)

**初始化阶段：** 由 `ReactDOM.render()` 触发，初次渲染。

- `constructor(props)`：构造器。

- `componentWillMount()`：组件将要挂载。

- `render()` ：挂载组件。

- `componentDidMount()`：组件挂载完成（常用）。  
一般在该钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息等。

**更新阶段：** 由组件内部 `this.setState()` 或父组件 `render()` 触发。

- `componentWillReceiveProps(props)`：组件将要接收新的 `props` 的钩子。  
父组件 `this.setState()` 重新 `render()`，或者父组件强制更新 `render()` 都会调用该钩子。  
**注意：第一次初始化传递的时候不会调用该钩子。**

- `shouldComponentUpdate(nextProps, nextState)`：控制组件更新的阀门。  
由 `this.setState()` 触发，返回一个布尔值。  
&emsp;true：继续执行。  
&emsp;false：state 数据会发生更新，但是后续钩子不再执行。

- `componentWillUpdate(nextProps, nextState)`：组件将要更新。  
`this.forceUpdate()` 可以强制更新组件。

- `render()`：重新渲染。

- `componentDidUpdate(prevProps, prevState)`：组件更新完毕。

**卸载组件：** 由 `ReactDOM.unmountComponentAtNode(容器)` 触发。

- `componentWillUnmount()`：组件将要卸载（常用）。  
一般在这个钩子中做一些收尾的事，例如：关闭定时器，取消订阅消息等。
