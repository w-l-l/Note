# React表单操作

## 事件处理

通过 `onXxx` 属性指定事件处理函数（注意大小写）。

- `react` 使用的是自定义事件（合成事件），而不是使用的原生 `DOM` 事件（为了更好的兼容性）。

- `react` 中的事件是通过事件委托方式处理的（委托给组件最外层的元素，为了高效）。  
通过 `event.target` 得到发生事件的 DOM 元素对象（事件源）。

## 收集表单数据

- `非受控组件`：最后提交的时候才获取要提交的数据。

```js
class Component extends React.Component {
  name = null
  password = null
  submit = event => {
    event.preventDefault()
    console.log(`用户名：${this.name.value}`, `密码：${this.password.value}`)
  }
  render() {
    return (
      <form onSubmit={ this.submit }>
        <input type="text" ref={ c => this.name = c } />
        <input type="password" ref={ c => this.password = c } />
        <button>登录</button>
      </form>
    )
  }
}

ReactDOM.render(<Component />, document.getElementById('容器id'))
```

- `受控组件`：通过 `onChange` 事件实时获取表单数据。

```js
class Component extends React.Component {
  state = {
    name: '',
    password: ''
  }
  saveName = event => {
    this.setState({ name: event.target.value })
  }
  savePassword = event => {
    this.setState({ password: event.target.value })
  }
  submit = event => {
    event.preventDefault()
    console.log(`用户名：${this.state.name}`, `密码：${this.state.password}`)
  }
  render() {
    return (
      <form onSubmit={ this.submit }>
        <input type="text" onChange={ this.saveName } />
        <input type="password" onChange={ this.savePassword } />
        <button>登录</button>
      </form>
    )
  }
}

ReactDOM.render(<Component />, document.getElementById('容器id'))
```

总结：一般我们提交表单数据的时候，都采用受控组件的形式，因为非受控组件必须设置 `ref` 才能获取到值，我们应该尽量避免使用 `ref`。
