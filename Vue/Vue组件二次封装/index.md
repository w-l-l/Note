# Vue组件二次封装

对于二次封装组件，需要批量继承的主要有 3 个：`属性`、`方法`、`插槽`。

可能还需要在封装的组件上实现 `v-model` 操作。

## 批量继承

**属性继承**

```html
<component v-bind="$attrs"></component>
```

**方法继承**

```html
<component v-on="$listeners"></component>
```

**插槽继承**

插槽继承官网并没有直接提供方法，但是我们可以使用一些技巧实现。

```html
<component>
  <template v-for="(item, name) in $slots" :slot="name">
    <slot :name="name" />
  </template>
</component>
```

**注意：如果插槽内容是动态渲染的，则获取到的 $slots 是个空对象，当数据设置好时它是不会渲染出来的，所以建议还是将插槽名列出来，这样才能渲染出来。**

以上差不多就可以完成批量继承属性、方法和插槽了，但是只适用于 Vue2.x，在 Vue3.x 中并不是完全适用的，比如 `$listeners` 在 Vue3.x 中被移除了，需要注意。

## 实现 v-model

用 `el-input` 做示例。

**二次封装组件**

```html
<template>
  <el-input v-model="value" @input="handleInput">
</template>
```

```js
export default {
  name: 'myInput',
  props: {
    newVlaue: {
      type: String,
      default: ''
    }
  },
  model: {
    prop: 'newVlaue',
    event: 'input-change'
  },
  data() {
    return {
      value: this.newValue
    }
  },
  watch: {
    newValue(v) {
      this.value = v
    }
  },
  methods: {
    handleInput(v) {
      this.$emit('input-change', v)
    }
  }
}
```

**组件使用**

```html
<my-input v-model="xxx"></my-input>
```

这里我们使用了 `model` 配置项，允许一个自定义组件使用 `v-model` 时定制 `prop` 和 `event`。

默认情况下，一个组件上的 `v-model` 会把 `value` 用作 `prop`，把 `input` 事件用作 `event`，但是一些输入类型比如单选框和复选框按钮可能想使用 `value` prop 来达到不同的目的。使用 `model` 选项可以回避这些情况产生的冲突。
