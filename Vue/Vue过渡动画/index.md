# Vue过渡动画

操作 css 的 `transition` 或 `animation`。

vue 会给目标元素添加或移除特定的 class 类名。

## 过渡类名

- `v-enter-active`：进入过渡生效时的状态。  
`v-enter`：进入过渡的开始状态。  
`v-enter-to`：进入过渡的结束状态。

- `v-leave-active`：离开过渡生效时的状态。  
`v-leave`：离开过渡的开始状态。  
`v-leave-to`：离开过渡的结束状态。

`v-` 开头的类名 `transition` 标签直接使用生效。

```html
<!-- 使用 transition 标签包裹需要动画的元素 -->
<transition>
  <p v-if="show">显示隐藏</p>
</transition>
```

将 `v-` 换成 `xxx-`，则 `transition` 标签必须使用 name 属性才能使用相关的动画。

```html
<transition name="xxx">
  ...
</transition>
```

![过渡动画](./img/transition.png)

### 自定义过渡类名

- `enter-class`。

- `enter-active-class`。

- `enter-to-class`。

- `leave-class`。

- `leave-active-class`。

- `leave-to-class`。

它们的优先级高于普通的类名，这对于 vue 的过渡系统和其他第三方 css 动画库十分有用。

```html
<transition name="xxx" enter-active-class="className" leave-active-class="className"></transition>
```

## 显性的过渡持续时间

```html
<transition :duration="1000"></transition>
```

`duration` 设置的时间是毫秒级别的，也可以定制进入和移出的持续时间。

```html
<transition :duration="{ enter: 500, leave: 800 }"></transition>
```

**注意：duration 设置的过渡时长超过原来的过渡时长无效，只能设置小于原本过渡时长的时间**。

## 过渡钩子函数

入场动画：

- `@before-enter="callback"`

- `@enter="callback"`

- `@after-enter="callback"`

- `@enter-cancelled="callback"`

出场动画：

- `@before-leave="callback"`

- `@leave="callback"`

- `@after-leave="callback"`

- `@leave-cancelled="callback"`

**注意：每个构造函数的第一个参数都是当前元素，enter 和 leave 钩子函数的第二个参数是 done，在 enter 和 leave 中必须使用 done 进行回调，否则它们将被同步调用，过渡会立即完成。**

在 `enter` 和 `leave` 钩子函数中，需要写 `el.offsetWidth`（其他 offset 也行）。这句话没有实际的作用，但是不写，出不来动画效果。可以认为 `el.offsetWidth` 会强制动画刷新。

使用钩子函数过渡的元素添加 `:css="false"`，vue 会跳过 css 的检测，可以避免过渡过程中 css 的影响。

```html
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:leave="leave"
  v-bind:css="false"
>
  <p v-if="show">Demo</p>
</transition>
```
