# Vue组件相关

组件的出现，就是为了拆分 vue 实例的代码量，能够让我们以不同的组件，来划分不同的功能模块，将来我们需要什么样的功能，就可以去调用对应的组件即可。

组件化和模块化的区别：

- `模块化`：是从代码逻辑的角度进行划分的，方便代码分层开发，保证每个功能模块的职能单一。

- `组件化`：是从 UI 界面的角度进行划分的，前端的组件化，方便 UI 组件的复用。

## 组件定义方式

### 全局组件

1. 使用 `Vue.extend` 配合 `Vue.component` 方法。

```js
Vue.component('componentName', Vue.extend({
  tempalte: 'xxx'
}))
```

2. 直接使用 `Vue.component` 方法。

```js
Vue.component('componentName', {
  template: 'xxx'
})
```

3. 模板组件。

```html
<template id="temp"></template>
```

```js
Vue.component('componentName', {
  template: '#temp'
})
```

### 内部私有组件

```js
new Vue({
  components: {
    temp1: {
      template: 'xxx'
    },
    temp2: {
      template: '#id'
    },
    temp3: Vue.extend({
      template: 'xxx' // 或者 '#id'
    })
  }
})
```

**注意：组件名有大写字母，使用的时候，大写字母变小写，前面加上短杠（'tEmp' --> `<t-emp></t-emp>`）。**

**组件只能存在一个根元素，多个元素需要使用标签包裹起来。**

## 组件中的 data

组件中的 `data` 必须是一个函数，还必须返回一个对象才行。

因为组件是可以复用的，每个组件的 `data` 数据应该分割开来。

外部定义一个对象，组件 `data` 方法返回，那么所有组件都使用一个地方的数据，一个组件的 `data` 数据发生改变，所有组件页面的 `data` 数据都会发生改变。

## 组件的切换方式（:is）

```html
<component :is="componentName"></component>
```

`component` 是一个占位符，`is` 属性可以用来指定要展示的组件的名称。
