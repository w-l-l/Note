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

## 组件通信方式

### 父组件向子组件传值（props）

在引用子组件的时候，通过属性绑定 `v-bind` 的形式，传递给子组件内部，供子组件使用。

把父组件传递过来的属性，先在 `props` 中定义一下，才能使用这个数据。

```html
<temp :msg="msg"></temp>
```

```js
new Vue({
  data: {
    msg: 'xxx'
  },
  components: {
    temp: {
      tempalte: '<h1>{{ msg }}</h1>',
      // props: ['msg']
      props: {
        msg: {
          type: String,
          required: true,
          default: ''
          // default: _ => [] 对象或者数组默认值必须从一个工厂函数获取
        }
      }
    }
  }
})
```

**注意：组件中 props 中的数据，都是通过父组件传递给子组件的，props 中的数据都是只读的，无法重新赋值。**

### 子组件向父组件传值（$emit / props）

父组件向子组件传递自定义事件，使用的事件绑定机制 `v-on`，当我们自定义一个事件属性之后，那么子组件就能够通过 `$emit` 来调用这个方法了。

父组件将事件的引用，传递到子组件内部，子组件在内部调用父组件传递过来的方法，同时把要发送给父组件的数据，在调用方法的时候当作参数传递进去。

```html
<temp @fn="add" :propFn="addd"></temp>
```

```js
new Vue({
  data: {
    msg: ''
  },
  methods: {
    add(v) {
      this.msg = v
    }
  },
  components: {
    temp: {
      template: `<div>
        <button @click="add">$emit添加</button>
        <button @click="propFn(msg)">props添加</button>
      </div>`,
      data() {
        return {
          msg: '子组件'
        }
      },
      methods: {
        add() {
          this.$emit('fn', this.msg)
        }
      },
      props: ['propFn'] // props 也可以子组件向父组件传值
    }
  }
})
```

### 兄弟组件传值（eventHub）

单独的事件中心管理组件间的通信。

```js
new Vue({
  el: '#app',
  created() {
    Vue.prototype.eventHub = this
  }
})
```

监听事件：组件挂载页面的时候，可以给中心管理绑定一个事件，让其他组件调用并传值。

```js
this.eventHub.$on('eventName', function() {
  console.log(arguments)
})
```

销毁事件。

```js
this.eventHub.$off('eventName')
```

触发事件。

```js
this.eventHub.$emit('eventName', params)
```

## this.$refs

获取元素或者组件。

```html
<div ref="divRef"></div>
<temp ref="tempRef"></temp>
```

```js
this.$refs.divRef // dom 对象
this.$refs.tempRef // 组件实例
```
