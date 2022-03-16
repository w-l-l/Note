# React面向组件编程

## 模块化与组件化

模块：

- 向外提供特定功能的 js 程序，一般就是一个 js 文件。

- 随着业务逻辑增加，代码越来越多且复杂，就必须拆成模块。

- 作用：复用 js，简化 js 的编写，提高 js 运行效率。

- `模块化`：当应用的 js 都以模块来编写，这个应用就是一个模块化的应用。

组件：

- 用来实现局部功能效果的代码和资源的集合（html、css、js、image 等）。

- 界面的功能较为复杂，这个时候就应该拆成组件。

- 作用：复用编码，简化项目编码，提高运行效率。

- 组件化：当应用以多个组件的方式实现，这个应用就是一个组件化的应用。

组件化编码流程：

- 拆分组件：拆分页面，抽取组件。

- 实现静态组件：使用组件实现静态页面效果。

- 实现动态组件：  
动态显示初始化数据（数据类型、数据名称、保存在哪个组件）。  
交互（从绑定事件监听开始）。

## 函数式组件与类式组件

函数式组件：

```js
function MyComponent() {
  console.log(this) // 此处的 this 是 undefined，因为 babel 编译后开启了严格模式
  return <h1>函数式组件</h1>
}

ReacrDOM.render(<MyComponent />, document.getElementById('容器id'))
```

`react` 解析组件标签，找到 `MyComponent` 组件，发现组件是使用函数定义的，随后调用函数，将返回的虚拟 DOM 转化为真实 DOM，随后呈现在页面中。

类式组件：

```js
class MyComponent extends React.Component {
  render() {
    console.log(this) // 此处的 this 是 MyComponent 类的组件实例对象
    return <h1>类式组件</h1>
  }
}

ReactDOM.render(<MyComponent />, document.getElementById('容器id'))
```

`react` 解析组件标签，找到 `MyComponent` 组件，发现组件是使用类定义的，随后 `react` 内部 `new` 出来该类的实例，并通过该实例调用原型上的 `render` 方法。

将 `render` 方法返回的虚拟 DOM 转化为真实 DOM，随后呈现在页面上。

**注意：**

- 组件名首字母必须大写。

- 虚拟 DOM 元素只能有一个根标签。

- 虚拟 DOM 元素必须有结束标签（`<Label></Label>` 或 `<Label />`）。

## React 中的事件绑定

传统事件绑定：

```html
<div onclick="func()"></div>
```

`react` 中的事件绑定：

```html
<div onClick={func}></div>
```

**注意：on 后面拼接的事件名首字母要大写，函数表达式不要调用，否则绑定的事件为函数的返回值。**

## 组件标签中的内容

```html
<Componeent>...</Componeent>
```

组件标签中的内容会通过 `children` 属性传递给 `props`。

在组件中我们可以通过 `this.props.children` 的方式获取到组件标签中填写的内容。
