# Vue介绍

一位华裔前 Google 工程师（尤雨溪）开发的前端 js 框架。

作用：动态构建用户界面。

特点：

- 遵循 `MVVM` 模式。

- 编码简洁，体积小，运行效率高，适用于移动端 / PC 端开发。

- 它本身只关注 UI，可以轻松引入 `vue` 插件和其他第三方库开发项目。

- 渐进式框架：可以将 `vue` 作为你应用的一部分嵌入其中。

与其他框架的关联：

- 借鉴 `angular` 的模板和数据绑定技术。

- 借鉴 `react` 的组件化和虚拟 DOM 技术。

`vue` 包含一系列的扩展插件（库）：

- `vue-cli`：vue 脚手架。

- `vue-resource / axios`：ajax 请求库。

- `vue-router`：路由。

- `vuex`：状态管理。

- `vue-lazyload`：图片懒加载。

- `vue-scroller`：页面滑动相关（better-scroll）。

- `mint-ui`：基于 vue 的组件库（移动端）。

- `element-ui`：基于 vue 的组件库（PC 端）。

## Vue 对象的基本选项

`el`：

- 指定 `DOM` 标签容器的选择器。

- vue 就会管理对应的标签及其子标签。

`data`：

- 对象获取函数类型。

- 指定初始化状态数据的对象。

- vue 实例也会自动拥有 data 中所有的属性。

- 页面中可以直接访问使用。

- 数据代理：由 vue 实例对象来代理对 data 中所有属性的操作（读 / 写）。

`methods`：

- 包含多个方法的对象。

- 供页面中的事件指令来绑定回调。

- 回调函数默认有 `event` 参数，但也可以指定自己的参数。

- 所有的方法由 vue 实例对象来调用，访问 `data` 中的属性直接使用 `this.dataPropName`。

`computed`：

- 包含多个方法的对象。

- 对状态属性进行计算返回一个新的数据，供页面获取显示。

- 一般情况下是相当于是一个只读的属性。

- 利用 `set` 和 `get` 方法来实现属性数据的计算获取，同时监视属性数据的变化。

```js
// 如何给对象定义 get / set 属性
// 在创建对象时指定
const obj = {
  get name() {
    return 'xxx'
  },
  set name(value) { ... }
}

// 在对象创建之后指定
Object.defineProperty(obj, 'name', {
  get() {
    return 'xxx'
  },
  set(value) { ... }
})
```

`watch`：

- 包含多个属性监视的对象。

- 分为一般监视和深度监视。  
一般监视：`propName: function(newValue, oldValue) {}`。  
深度监视：`propName: { deep: true, handler(newValue, oldValue) {} }`。

- 另一种添加监方式：`vm.$watch(propName, function(newValue, oldValue) {})`
