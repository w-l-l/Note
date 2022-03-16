# React组件的核心属性

## state 状态

`state` 是组件对象最重要的属性，值是对象（可以包含多个 `key: value` 的组合）。

组件被称为 `状态机`，通过更新组件的 `state` 来更新对应的页面显示（重新渲染页面）。

**强烈注意：**

- 组件中 `render` 方法中的 `this` 为组件实例对象。

- 组件自定义方法被调用的时候，里面的 `this` 为 `undefined`，如何解决？  
强制绑定 `this`：通过 `bind` 方法返回一个新的函数。  
使用箭头函数，获取上一级的 `this`。

- 状态数据不能直接修改或更新，要使用 `react` 内部提供的 `setState` 方法来更新才有效（`setState(key: value)` 内部合并对象，不是替换）。

详细案例：

```js
class Component extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      isShow: true,
      text: '状态：'
    }
    this.changeShow = this.changeShow.bind(this) // 让回调函数中的 this 指向组件实例
  }
  changeShow() {
    // 如果不改变 this 指向，react 内部并没有通过实例调用，所以 this 是 undefined
    const { isShow } = this.state
    this.setState({ isShow: !isShow })
  }
  render() {
    const { isShow, text } = this.state
    return (
      <div>
        <h1>{ text }{ isShow ? '显示' : '隐藏' }</h1>
        <button onClick={ this.changeShow }></button>
      </div>
    )
  }
}

ReactDOM.render(<Component />, document.getElementById('容器id'))
```

简写案例（推荐）：

```js
class Component extends React.Component {
  state = {
    isShow: true,
    text: '状态：'
  }
  changeShow = _ => {
    const { isShow } = this.state
    this.setState({ isShow: !isShow })
  }
  render() {
    const { isShow, text } = this.state
    return (
      <div>
        <h1>{ text }{ isShow ? '显示' : '隐藏' }</h1>
        <button onClick={ this.changeShow }></button>
      </div>
    )
  }
}

ReactDOM.render(<Component />, document.getElementById('容器id'))
```
