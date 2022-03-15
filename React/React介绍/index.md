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
