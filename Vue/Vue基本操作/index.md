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

## 循环渲染

使用 `v-for` 可以根据数组（对象）循环生成元素。

```html
<ul>
  <li v-for="(item, key, index) in/of arr/obj" :key="item.id">{{ item.name }}</li>
</ul>
```

`key`：为了给 vue 一个提示，以便它能够跟踪每个节点的身份，需要为每一项提供一个唯一的 `:key`。

不推荐使用 index 为 key 的值，最好使用静态的 key。

也可以理解为：

- 无 key：状态默认绑定的是位置。

- 有 key：状态根据 key 的属性值绑定到了相应的数组元素，这样设计也是为了性能考虑（高效的更新虚拟 DOM）。

数组更新检测：vue 将被侦听的数组的变更方法进行了包裹，所以它们会触发视图更新，被包裹的方法有：`push`、`pop`、`shift`、`unshift`、`splice`、`sort`、`reverse`。

其他操作改变数组不会引发视图更新（如：`arr[i] = 'xxx'`）。

循环数值：

```html
<!-- item 是从 1 循环到 10，不是从 0 循环到 9 -->
<p v-for="item in 10">{{ item }}</p>
```

## 事件处理

事件修饰符：

- `.stop`：阻止事件冒泡。  
event.stopPropagation()

- `.prevent`：阻止浏览器默认行为。  
event.preventDefault()

- `.capture`：设置事件捕获模式。

- `.self`：元素自身才触发。  
`event.target === 当前元素` 该元素的事件不能通过内部元素冒泡触发。

- `.once`：事件只触发一次。

- `.passive`：不阻止事件的默认行为。  
**注意：不要把 .passive 和 .prevent 一起使用，因为 .prevent 将会被忽略**。

`.stop` 和 `.self` 的区别：.self 只会阻止自己身上冒泡行为的触发，并不会真正阻止冒泡的行为。

****

按键修饰符：`.enter`、`.tab`、`.delete(捕获删除和退格)`、`.esc`、`.space`、`.up`、`.down`、`.left`、`.right`、`.page-down`。

自定义按键修饰符别名：`Vue.config.keyCodes.xxx = 112`。

`@keyup.xxx` 这样使用。

`@keyup.keyCodes` 直接点按键编码也行。

****

系统修饰符：可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

`.ctrl`、`.alt`、`.shift`、`.meta`、`.exact(修饰符允许你控制由精确的系统修饰符组合触发的事件)`

`@click.ctrl.exact` 只有 ctrl 被按下的时候才触发。

`@click.exact` 没有任何系统修饰符被按下的时候才触发。

****

鼠标按钮修饰符：`.left`、`.right`、`.middle`。

这些修饰符会限制处理函数仅响应特定的鼠标按钮。

****

`.native` 修饰符：监听组件根元素的原生事件。

主要给自定义的组件添加原生事件。

```html
<temp @click.native="foo"></temp>
```

将自定义事件和原生的 click 事件区分开来，不加 .native 就是自定义事件，加了就作用于组件根元素上。

## 表单输入绑定

通过 v-model 指令在表单元素上创建双向数据绑定，它会根据控件类型自动选取正确的方法来更新元素。

`v-model` 相关的修饰符。

- `.lazy`：将 input 事件同步转为 chang 事件同步。

- `.number`：将值转换为数值类型。

- `.trim`：自动过滤用户输入的首尾空白字符。

复选框：

```html
<!-- 勾选时 msg 值为 yes，取消勾选时值为 no -->
<input type="checkbox" v-model="msg" true-value="yes" false-value="no" />
```
