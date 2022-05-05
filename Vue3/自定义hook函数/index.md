# 自定义hook函数

什么是 `hook`：本质是一个函数，把 `setup` 函数中使用的 `Composition API` 进行了封装。

类似于 Vue2.x 中的 `mixin`。

自定义 `hook` 的优势：复用代码，让 `setup` 中的逻辑更清楚易懂。

**获取鼠标点击位置 hook**

```js
import { reactive, onMounted, onBeforeUnmount } from 'vue'

export default function usePoint() {
  const point = {
    x: 0,
    y: 0
  }
  const pointFn = event => {
    point.x = event.pageX
    point.y = event.pageY
  }
  onMounted(_ => window.addEventListener('click', pointFn))
  onBeforeUnmount(_ => window.removeEventListener('click', pointFn))
  return point
}
```

使用 `hook`：

```html
<template>
  <h1>当前鼠标点击的位置：x：{{ point.x }} y：{{ point.y }}</h1>
</template>
```

```js
import usePoint from './hooks/usePoint'
export default {
  setup() {
    const point = usePoint()
    return { point }
  }
}
```
