# Vue3与Vue2的其他差异

**全局 API 的转移**

Vue2.x 有许多全局 API 和配置。

Vue3.x 中对这些 API 做出了调整：将全局的 API，即：`Vue.xxx` 调整到应用实例 `app` 上。

- `Vue.config.xxx` --> `app.config.xxx`。

- `Vue.config.productionTip` --> 移除。

- `Vue.component` --> `app.component`。

- `Vue.directive` --> `app.directive`。

- `Vue.mixin` --> `app.mixin`。

- `Vue.use` --> `app.use`。

- `Vue.prototype` --> `app.config.globalProperties`。

**其他改变**

- `data` 选项应始终被声明为一个函数。

- 过渡类名的更改。

```css
/* Vue2.x */
.v-enter,
.v-leave-to {...}

.v-leave,
.v-enter-to {...}
```

```css
/* Vue3.x */
.v-enter-from,
.v-leave-to {...}

.v-leave-from,
.v-enter-to {...}
```

- 移除 `keyCode` 作为 `v-on` 的修饰符，同时不再支持 `config.keyCodes`。

- 移除 `v-on.native` 修饰符。

```html
<!-- 组件绑定事件 -->
<component @click="xxx" @close="xxx" />
```

```js
// 组件声明自定义事件
export default {
  emits: ['close', 'click'] // click 声明了就是自定义事件，否则就视为原生事件
}
```

- 移除过滤器（filter）：过滤器虽然看起来方便，但它需要一个自定义语法，打破了大括号内表达式“只是 JavaScript”的假设，这不仅有学习成本，而且有实现成本！建议用方法调用或计算属性去替换过滤器。
