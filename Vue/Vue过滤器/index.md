# Vue过滤器

对要显示的数据进行特定格式化后再显示。

**注意：并没有改变原本的数据，可是产生新的对应的数据**。

## 定义过滤器

定义全局过滤器：

```js
Vue.filter('filterName', function(value) {
  // 函数的第一个参数为需要过滤的值
  // 进行一定的数据处理
  return xxx
})
```

**注意：全局定义过滤器必须在创建 vue 实例之前**。

局部过滤器：

```js
export default {
  filters: {
    filterName(value) {
      // 函数的第一个参数为需要过滤的值
      // 进行一定的数据处理
      return xxx
    }
  }
}
```

**注意：当全局过滤器和局部过滤器重名时，还优先采用局部过滤器（就近原则）**。

## 使用过滤器（| 管道符）

在双花括号中：

```html
<div>{{ msg | filterName }}</div>
```

在 `v-bind` 中：

```html
<div :attr="msg | filterName"></div>
```

过滤器也可以串联：

```html
<div>{{ msg | filterName1 | filterName2 }}</div>
```

过滤器也可以传递参数：

```html
<div>{{ msg | filterName(a, b) }}</div>
```

传递的参数从过滤器函数参数的第二位开始依次排列。
