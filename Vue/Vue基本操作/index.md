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

## class 与 style 绑定

在应用界面中，某些元素的样式是变化的，`class`、`style` 绑定就是专门用来实现动态样式效果的技术。

### class 绑定

- 字符串类型：直接写 class 相关的类名。

```html
<div :class="msg"></div>
```

- 对象类型：属性名是 class 类名，属性值为 boolean 值，true 使用，false 不使用。

```html
<div :class="{ className: true }"></div>
```

- 数组类型：元素为 class 类名或对象。

```html
<div :class="['className1', { className2: true }]"></div>
```

### style 绑定

- 字符串类型：直接写 style 样式的字符串，分号隔开。

```html
<div :style="'color: red; font-size: 14px'"></div>
```

- 对象类型：属性名是样式属性名，属性值是样式属性值。

```html
<div :style="{ color: 'red', fontSize: '14px' }"></div>
```

还支持 `多重值`：属性值是一个数组，常用于提供多个带前缀的值。

```html
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

只会渲染数组中最后一个被浏览器支持的值。

- 数组类型：元素为样式对象。

```html
<div :style="[{ color: 'red' }, { fontSize: '14px' }]"></div>
```

**补充：当样式属性名需要添加前缀时，vue 会自动侦测并添加相应的前缀**。

## 条件渲染

if 指令：`v-if`、`v-else-if`、`v-else`。

**注意：v-else 和 v-else-if 必须紧跟在带 v-if 或 v-else-if 的元素之后**。

vue 会尽可能高效地渲染元素，通常会复用元素而不是从头开始渲染。这样做除了使 vue 更快之外，还有一些好处。

例如：如果你允许用户在不同的登录方式之间切换，两个文本框通过 v-if 切换显示的时候文本框之间的内容不会清空。

可以通过 key 属性使当前元素具有唯一标识，切换时清空文本。

```html
<div v-if="show">
  <input type="text" key="1" />
</div>
<div v-else>
  <input type="text" key="2" />
</div>
```

show 指令：`v-show`，只是简单地切换元素的 display 样式。

```html
<div v-show="show"></div>
```

**注意：v-show 不支持 template 元素，也不支持 v-else**。

`v-if` 对比 `v-show`：

- v-if 有更高的切换开销。

- v-show 有更高的初始化渲染开销（不管 v-show 为何值，元素始终会渲染）。

- 因此需要非常频繁地切换，使用 v-show。

- 如果在运行时条件很少改变，则使用 v-if。

**注意：不推荐同时使用 v-if 和 v-for，v-for 具有比 v-if 更高的优先级**。
