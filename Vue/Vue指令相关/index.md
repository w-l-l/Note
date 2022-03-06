# Vue指令相关

## 常用内置指令

- `v-text`：更新元素的 `textContent`，不是 `innerText`。

- `v-html`：更新元素的 `innerHTML`。

- `v-if`：如果为 true，当前标签内容才会挂载到页面上。

- `v-else`：如果为 false，当前标签内容才会挂载到页面上。

- `v-show`：通过控制 `display: none` 样式来控制元素的显示和隐藏。

- `v-for`：遍历数组或者对象。

- `v-on`：绑定事件监听，一般简写为 `@`。

- `v-bind`：强制绑定解析表达式，一般简写为 `:`。

- `v-model`：双向数据绑定，只作用于表单元素。

- `ref`：为某个元素注册一个唯一标识，vue 实例对象通过 `$refs` 属性访问这个元素对象。

```html
<div ref="refName"></div>
<!-- vue 访问：vm.$refs.refName（返回当前标识元素，如果是组件会返回组件实例对象） -->
```

**注意：标识名一样后面的元素会覆盖前面的元素**。

- `v-clock`：使用它防止闪现表达式，与 css 配合。

```css
[v-clock] {
  display: none;
}
```

- `v-pre`：显示原始信息跳过编译过程，一些静态的内容不需要编译时，加这个指令可以加快渲染。

- `v-once`：执行一次性的插值，当数据改变时，插值处的数据不会继续更新。

## 自定义指令

全局注册：

```js
Vue.directive('directiveName', function(el, binding) {
  ...
})
```

局部注册：

```js
export default {
  directives: {
    directiveName(el, binding) {
      ...
    }
  }
}
```

使用方法：

```html
<div v-directiveName></div>
<div v-directiveName="msg"></div>
<div v-directiveName:arg="msg"></div>
<!-- 相关参数 binding 对象中都能获取到 -->
```

### 指令钩子函数

- `bind`：只调用一次，初始化设置。

- `inserted`：被绑定元素插入父节点时调用（仅保证父节点存在，但不一定已被插入文档中）。

- `update`：所在组件的 `VNode` 更新时调用。

- `componentUpdated`：指令所在组件的 `VNode` 及其子 `VNode` 全部更新后调用。

- `unbind`：只调用一次，指令与元素解绑时调用。

```js
Vue.directive('directiveName', {
  bind(el, binding) {
    ...
  },
  inserted(el, binding) {
    ...
  },
  update(el, binding) {
    ...
  },
  componentUpdated(el, binding) {
    ...
  },
  unbind(el, binding) {
    ...
  }
})
```

简写形式：只在 `bind` 和 `update` 时触发回调。

```js
Vue.directive('directiveName', function(el, binding) {
  ...
})
```

### 钩子函数相关参数

- `el`：指令所绑定的元素。

- `binding`：一个对象，包含指令相关的信息。

- `VNode`：vue 编译生成的虚拟节点。

- `oldVNode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中使用。

**注意：除了 el 参数之外，其他参数都是只读的，切勿进行修改，如果需要在钩子函数直接共享数据，建议通过元素的 dataset 来进行**。
