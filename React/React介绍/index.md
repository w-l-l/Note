# React介绍

用于动态构建用户的 `JavaScript` 库（值关注视图）。

由 `Facebook` 开源。

特点：

- 声明式编码。

- 组件化编码。

- `React Native` 编写原生应用。

- 高效、优秀的 `Diffing` 算法。  
使用虚拟（`Virtual`） `DOM`，不总是直接操作页面真实 `DOM`。  
`DOM` `Diffing` 算法，最小化页面重绘。

相关的 `js` 库：

- `react.js`：`React` 核心库。

- `react-dom.js`：提供操作 `DOM` 的 `react` 扩展库。

- `babel.min.js`：解析 `JSX` 语法代码转化为 `js` 代码的库。

## 创建虚拟 DOM 的两种方式

- `JSX` 实现。

```js
const vdom = (
  <h1 id="react">
    <span>hello, React</span>
  </h1>
)

ReactDOM.render(vdom, document.getElementById('容器id'))
```

- 传统 `js` 实现（不推荐）。

```js
const vnode = React.createElement('h1', { id: 'react' }, React.createElement('span', {}, 'hello, React'))
ReactDOM.render(vnode, document.getElementById('容器id'))
```

关于虚拟 `DOM`：

- 本质是 `Object` 类型的对象（一般对象）。

- 虚拟 `DOM` 比较轻，真实 `DOM` 比较重，因为虚拟 `DOM` 是 `React` 内部在用，无需真实 `DOM` 上那么多的属性。

- 虚拟 `DOM` 最终会被 `React` 转化为真实 `DOM`，呈现在页面上。

## JSX 语法规则

`JSX` 全称：`JavaScript XML`。

`React` 定义的一种类似于 `XML` 的 `js` 扩展语法（本质：`js + XML`）。

`React.createElement(component, props, ...children)` 方法的语法糖。

- 定义虚拟 `DOM` 时，不要写引号。

- 标签中混入 `js` 表达式时要用 `{}` 包裹。

- 样式的类名指定不要用 `class`，要用 `className`。

- 内联样式，要用 `style={{ key: value }}` 的形式去写。

- 只有一个根标签。

- 标签必须闭合。

- 标签首字母。  
若小写字母开头，则将该标签转化为 `html` 中同名元素，若 `html` 中无该标签对应的同名元素，则报错。  
若大写字母开头，`React` 就去渲染对应的组件，若组件没有定义，则报错。

`babel.js` 的作用：

1. 浏览器不能直接解析 `JSX` 代码，需要 `babel` 转译为纯 `js` 的代码才能运行。

2. 只要使用了 `JSX`，`script` 标签都要加上 `type="text/babel"`，声明需要 `babel` 来处理。
