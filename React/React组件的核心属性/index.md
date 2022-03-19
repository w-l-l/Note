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

## props 传值

每个组件对象都会有 `props` 属性（`properties` 的简写）。

组件标签的所有属性都保存在 `props` 中。

通过标签属性从组件外向组件内传递变化的数据。

**注意：props 中的数据是只读的，不要进行修改，否则会报错。**

内部读取某个属性值。

```js
this.props.xxx
```

`props` 传递：

- 单独传递。

```html
<Component name="xxx" />
```

- 批量传递。

```html
<Component {...props} />
```

- 覆盖之前的属性值。

```html
<Component {...props} name="xxx" />
```

### 对 props 中的属性进行类型限制和必要性限制

第一种方式：`react` v15.5 版本之前（v15.5版本之后已废弃）。

```js
Component.propTypes = {
  name: React.PropTypes.string.isRequired, // 必须传值，并且是字符串类型
  age: React.PropTypes.number // 必须是数值类型
}
```

第二种方式：使用 `prop-types` 库进行限制（需要引入 `prop-types` 库）。

```js
Component.propTypes = {
  name: PropTypes.string.isRequired, // 必须传值，并且是字符串类型
  age: PropTypes.number // 必须是数值类型
}
```

**注意：PropTypes 对类型进行了重写，原生首字母都是大写，也有个别特殊的，如下：**

```js
PropTypes.func // 函数类型
PropTypes.bool // 布尔类型
```

### 设置默认值

```js
Component.defaultProps = {
  name: 'xxx',
  age: 18
}
```

### 使用方式

- 函数式组件使用。

```js
function Component(props) {
  return <h1>{ props.name }</h1>
}

Component.propTypes = { ... }
Component.defaultProps = { ... }
```

- 类式组件使用。

```js
class Component extends React.Component {
  static propTypes = { ... }
  static defaultProps = { ... }
  render() {
    return <h1>{ this.props.name }</h1>
  }
}
```

### 组件类的构造函数

情况一：

```js
class Component extends React.Component {
  constructor(props) {
    super(props)
    console.log(this.props) // 传递进来的属性
  }
}
```

情况二：

```js
class Component extends React.Component {
  constructor(props) {
    super()
    console.log(this.props) // undefined
  }
}
```

**注意：构造器是否接收 props，是否传递给 super，取决于：是否希望在构造器中通过 this 访问 props。**

## refs 元素

组件内的标签可以定义 `ref` 属性来标识自己。

**注意：请勿过度使用 refs。**

### 字符串形式的 ref

```html
<input ref="input" />
```

获取元素：

```js
const input = this.refs.input
```

### 回调形式的 ref

```html
<input ref={ c => this.input = c } />
```

获取元素：

```js
const input = this.input
```

`react` 将在组件挂载时，会调用 `ref` 回调函数并传入 `DOM` 元素，当卸载时调用它并传入 `null`。

在 `componentDidMount` 或 `componentDidUpdate` 触发前，`react` 会保证 refs 一定是最新的。

**注意：如果 ref 回调函数是以内联函数的方式定义的，在组件更新过程中它会被执行两次，第一次传入参数 null，然后第二次传入参数 DOM 元素。这是因为在每次渲染时会创建一个新的函数实例，所以 react 清空旧的 ref 并且设置新的。通过将 ref 的回调函数定义成 class 的绑定函数的方式可以避免上述问题，但是大多数情况下它是无关紧要的。**

```js
class Component extends React.Component {
  input = null
  saveDOM = c => this.input = c
  render() {
    return <input ref={ this.saveDOM } />
  }
}
```

这种方式当组件重新渲染的时候，`ref` 对应的 `this.saveDOM` 函数不会执行，如果是内联函数的话，会执行两次。

### createRef 创建 ref 容器

```js
class Component extends React.Component {
  myRef1 = React.createRef()
  myRef2 = React.createRef()
  render() {
    return (
      <div>
        <input ref={ this.myRef1 } />
        <input ref={ this.myRef2 } />
      </div>
    )
  }
}
```

获取元素：

```js
const myRef1 = this.myRef1.current
const myRef2 = this.myRef2.current
```

`React.createRef` 调用后可以返回一个容器，该容器可以存储被 ref 所标识的节点，该容器专人专用的，一个 `React.createRef` 只接收一个 `ref` 元素。
