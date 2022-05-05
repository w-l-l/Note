# Vue3新增的内置组件

**Fragment**

在 Vue2.x 中：组件必须有一个根标签。

在 Vue3.x 中：组件中可以没有根标签，内部会将多个标签包含在一个 `Fragment` 虚拟元素中。

好处：减少标签层级，减少内存占用。

**Teleport**

`teleport` 是一种能够将我们的组件 `html` 结构移动到指定位置的技术。

`to` 属性：要移动到指定位置的标签或者 `css` 选择器。

```js
<teleport to="body">
  <div>...</div>
</teleport>
```

**Suspense**

等待异步组件时渲染一些额外内容，让应用有更好的用户体验。

使用步骤：

```html
<!-- 异步组件 Child -->
<template>
  <h2>我是异步组件</h2>
</template>
```

```js
export default {
  async setup() {
    await new Promise(resolve => setTimeout(resolve, 2000))
  }
}
```

异步引入组件：

```js
import { defineAsyncComponent } from 'vue'

const AsyncComponent = defineAsyncComponent(_ => import('./components/Child'))
```

使用 `Suspense` 包裹组件，并配置 `default` 与 `fallback`。

```html
<Suspense>
  <template v-slot:default>
    <AsyncComponent />
  </template>
  <template v-slot:fallback>
    <h1>加载中...</h1>
  </template>
</Suspense>
```
