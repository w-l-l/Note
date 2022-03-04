# Vue基本操作

## 模板渲染

模板的理解：动态的 html 界面，包含一些 js 语法代码（双大括号表达 `{{}}`，指令）。

- 双大括号表达式（mustache 语法）：向页面输出数据，可以调用对象的方法。  
语法：`{{ msg }}`。

- 指令一：强制数据绑定，指定变化的属性值。  
完整写法：`v-bind:attr = "value"`。  
简洁写法：`:attr = "value"`。  
动态参数写法：`:[key] = "value"`（vue 2.6.0+ 才支持）。

- 指令二：绑定事件监听，绑定指定事件名的回调函数。  
完整写法：`v-on:click = "callback"`。  
简洁写法：`@click = "callback"`。  
动态参数写法：`@[event] = "callback"`（vue 2.6.0+ 才支持）。  
传参：`@click = "callback(params)"`。  
保留 event：`@click = "callback(params, $event)"`。

- 双向数据绑定（`v-model`）：监听 v-model 绑定的数据，实时进行更新显示。

```html
<input type="text" v-model="msg" />
```

**注意：v-model 只适用于表单元素**。

## 计算属性和监视属性

### 计算属性

在 `computed` 属性对象中定义计算属性的方法。

```js
export default {
  computed: {
    msg() {
      ... // 该区域包含了哪些 vue 实例的属性，当这些属性发生改变的时候，就会调用 msg 这个计算属性
      return xxx
    }
  }
}
```

通过 `get`、`set` 实现对属性数据的显示和监视。

```js
export default {
  computed: {
    msg: {
      get() {
        ...
        return xxx
      },
      set(v) {
        ...
      }
    }
  }
}
```

**计算属性存在缓存，多次读取只执行一次 get 计算**。

### 监视属性

通过 vue 实例对象的 `$watch` 或 `watch` 配置来监视指定的属性，当属性变化时，回调函数自动调用，在函数内部进行计算。

```js
export default {
  data() {
    return {
      msg: 'xxx'
    }
  },
  watch: {
    msg(newValue, oldValue) { ... }
  }
}
```

```js
vm.$watch('msg', function(newValue, oldValue) {
  ...
})
```

`watch` 也可以设置参数。

- `handler`：属性发生变化执行的函数。

- `deep`：是否深度监听某个对象的值，默认 false。

- `immediate`：是否第一次加载页面的时候执行 handler 函数，默认 false。

```js
export default {
  data() {
    return {
      msg: 'xxx'
    }
  },
  watch: {
    msg: {
      handler() { ... },
      deep: true,
      immediate: true
    }
  }
}
```

```js
vm.$watch('msg', function(newVaule, oldValue) {
  ...
}, {
  deep: true,
  immediate: true
})
```

**特殊字符属性的监视**：

```js
vm.$watch(_ => vm['xxx-xxx'], function(newValue, oldValue) {
  ...
})
```
