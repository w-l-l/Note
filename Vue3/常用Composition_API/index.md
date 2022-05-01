# 常用Composition API

[官方文档。](https://v3.cn.vuejs.org/guide/composition-api-introduction.html)

`Options API` 存在的问题：使用传统 `Options API` 中，新增或者修改一个需求，就需要分别在 `data`、`methods`、`computed` 里修改。

`Composition API` 的优势：我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

## 拉开序幕的 setup

Vue3.0 中一个新的配置项，值为一个函数。

`setup` 是所有 `Composition API`（组合 API）“表演的舞台”。

组件中所用到的数据、方法等等，均要配置在 `setup` 中。

`setup` 函数的两种返回值：

1. 若返回一个对象，则对象中的属性、方法，在模板中均可直接使用。（重点关注）

2. 若返回一个渲染函数，则可以自定义渲染内容。（了解）

`setup` 的参数：

- `props`：值为对象。  
包含组件外部传递过来，且组件内部声明接收了的属性。

- `context`：上下文对象，包含以下属性。  
`attrs`：值为对象，包含组件外部传递过来，但没有在 `props` 配置中声明的属性，相当于 `this.$attrs`。  
`slots`：收到的插槽内容，相当于 `this.$slots`。  
`emit`：分发自定义事件的函数，相当于 `this.$emit`。

**注意点**

尽量不要与 Vue2.x 配置混用。

- Vue2.x 配置(data、methods、computed...)中可以访问到 `setup` 中的属性、方法。

- 但在 `setup` 中不能访问到 `Vue2.x` 配置。(data、methods、computed...)

- 如果有重名，`setup` 优先。

`setup` 不能是一个 `async` 函数，因为返回值不再是 `return` 的对象，而是 `promise`，模板看不到 `return` 对象中的属性。（后期可以返回一个 `promise` 实例，但需要 `Suspense` 和异步组件的配合）

`setup` 在 `beforeCreate` 之前执行一次，`this` 是 `undefined`。
