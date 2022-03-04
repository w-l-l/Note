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
