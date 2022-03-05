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
