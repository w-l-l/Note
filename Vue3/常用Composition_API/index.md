# 常用Composition API

[官方文档。](https://v3.cn.vuejs.org/guide/composition-api-introduction.html)

`Options API` 存在的问题：使用传统 `Options API` 中，新增或者修改一个需求，就需要分别在 `data`、`methods`、`computed` 里修改。

`Composition API` 的优势：我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

## 拉开序幕的 setup

Vue3.0 中一个新的配置项，值为一个函数。

`setup` 是所有 `Composition API`（组合 API）“表演的舞台”。

组件中所用到的数据、方法等等，均要配置在 `setup` 中。

`setup` 函数的两种返回值：

1. 若返回一个对象，则对象中的属性、方法，在模板中均可直接使用。（重点关注）

2. 若返回一个渲染函数，则可以自定义渲染内容。（了解）

`setup` 的参数：

- `props`：值为对象。  
包含组件外部传递过来，且组件内部声明接收了的属性。

- `context`：上下文对象，包含以下属性。  
`attrs`：值为对象，包含组件外部传递过来，但没有在 `props` 配置中声明的属性，相当于 `this.$attrs`。  
`slots`：收到的插槽内容，相当于 `this.$slots`。  
`emit`：分发自定义事件的函数，相当于 `this.$emit`。

**注意点**

尽量不要与 Vue2.x 配置混用。

- Vue2.x 配置(data、methods、computed...)中可以访问到 `setup` 中的属性、方法。

- 但在 `setup` 中不能访问到 `Vue2.x` 配置。(data、methods、computed...)

- 如果有重名，`setup` 优先。

`setup` 不能是一个 `async` 函数，因为返回值不再是 `return` 的对象，而是 `promise`，模板看不到 `return` 对象中的属性。（后期可以返回一个 `promise` 实例，但需要 `Suspense` 和异步组件的配合）

`setup` 在 `beforeCreate` 之前执行一次，`this` 是 `undefined`。

## ref 函数

作用：定义一个响应式函数。

```js
// 语法
const xxx = ref(initValue)
```

创建一个包含响应式数据的引用对象。（reference 对象，简称 ref 对象）

```js
// js中操作数据
const n = ref(0)
n.value // 0
```

js 中操作数据需要 `.value`，模板中读取数据不需要 `.value`。

```html
<div>{{ n }}</div>
```

备注：

- 接收的数据可以是基本类型，也可以是对象类型。

- 基本类型的数据：响应式依然依靠 `Object.defineProperty()` 的 `get` 与 `set` 完成的。

- 对象类型的数据：内部使用了 Vue3.0 中的一个新函数，`reactive` 函数。

## reactive 函数

作用：定义一个对象类型的响应式数据。（基本类型不要用它，要用 `ref` 函数）

```js
// 语法
const proxyData = reactive(obj)
/*
  接收一个对象或数组，返回一个代理对象
  Proxy 的实例对象，简称 proxy 对象
*/
```

`reactive` 函数定义的响应式数据是“深层次的”。

内部基于 ES6 的 `Proxy` 实现，通过代理对象操作源对象，对内部数据进行操作。

## reactive 对比 ref

**从定义数据角度对比：**

- `ref` 用来定义：基本数据类型。

- `reactive` 用来定义：对象数据类型。

- 备注：`ref` 也可以用来定义对象数据类型，它内部会自动通过 `reactive` 转为代理对象。

**从原理角度对比：**

- `ref` 通过 `Object.defineProperty()` 的 `get` 和 `set` 来实现响应式。（数据劫持）

- `reactive` 通过 `Proxy` 来实现响应式（数据劫持），并通过 `Reflect` 操作源对象内部的数据。

**从使用角度对比：**

- `ref` 定义的数据：操作数据需要 `.value`，模板中直接读取数据不需要 `.value`。

- `reactive` 定义的数据：操作与读取数据，均不需要 `.value`

## 计算属性

`computed` 函数：与 Vue2.x 中 `computed` 配置功能一致。

```js
import { computed } from 'vue'

export default {
  setup() {
    // 计算属性--简写
    let com = computed(_ => {
      return '...'
    })

    // 计算属性--完整
    let com = computed({
      get() {
        return '...'
      },
      set(value) {
        xxx.xxx = value
      }
    })
    return {
      com
    }
  }
}
```

## watch 函数

与 Vue2.x 中 `watch` 配置功能一致。

两个小坑：

- 监视 `reactive` 定义的响应式数据时：oldValue 无法正常获取、强制开启了深度监视。（`deep` 配置失效，因为 newValue 和 oldValue 指向同一对象）

- 监视 `reactive` 定义的响应式数据中某个属性（引用类型）时：`deep` 配置有效。

`watch` 监视有以下几种情况：

**情况一：监视 ref 定义的响应式数据**

```js
watch(ref, (newValue, oldValue) => {
  console.log('ref变化了', newValue, oldValue)
}, { immediate: true }) // 如果 ref 定义的数据是引用类型，则 deep 配置有效
```

**情况二：监视多个 ref 定义的响应式数据**

```js
watch([ref1, ref2], (newValue, oldValue) => {
  console.log('ref1或ref2变化了', newValue, oldValue)
  // 此时 newValue 和 oldValue 的值对应 [ref1, ref2]，数组类型
})
```

**情况三：监视 reactive 定义的响应式数据**

若 `watch` 监视的是 `reactive` 定义的响应式数据，则无法正确获取 oldValue，因为 newValue 和 oldValue 指向同一对象。

若 `watch` 监视的是 `reactive` 定义的响应式数据，则强制开启了深度监视。

```js
watch(reactive, (newValue, oldValue) => {
  console.log('reactive变化了', newValue, oldValue)
}, { immediate: true, deep: false }) // 此处的 deep 配置不再奏效
```

**情况四：监视 reactive 定义的响应式数据中的某个属性**

```js
watch(_ => reactive.obj, (newValue, oldValue) => {
  console.log('reactive.obj变化了', newValue, oldValue)
}, { immediate: true, deep: true }) // 若 reactive.obj 是引用类型，此处的 deep 配置是有效的
```

**情况五：监视 reactive 定义的响应式数据中的某些属性**

```js
watch([_ => reactive.obj1, _ => reactive.obj2], (newValue, oldValue) => {
  console.log('reactive中的obj1或obj2变化了', newValue, oldValue)
}, { immediate: true, deep: true })
```

## watchEffect 函数

`watch` 的套路是：既要指明监视的属性，也要指明监视的回调。

`watchEffect` 的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。

**watchEffect 有点像 computed**

- 但 `computed` 注重计算出来的值（回调函数的返回值），所以必须要写返回值。

- 而 `watchEffect` 更注重的是过程（回调函数的函数体），所以不用写返回值。

**注意：watchEffect 传递的回调会立即执行。**

```js
watchEffect(_ => {
  // watchEffect 所指定的回调中用到的数据只要发生变化，则直接重新执行回调
  const x = ref.value
  const y = reactive
  console.log('watchEffect配置的回调执行了')
})
```

```js
watchEffect(_ => {
  setTimeout(_ => {
    // 如果将数据源写在异步回调中，该数据源发生改变时，watchEffect 不会重新执行回调
    const x = ref.value
    const y = reactive
  })
  console.log('watchEffect配置的回调执行了')
})
```

## toRef 和 toRefs

作用：创建一个 `ref` 对象，其 `value` 值指向另一个响应式对象中的某个属性。

```js
import { reactive, toRef } from 'vue'
export default {
  setup() {
    const person = reactive({
      name: '孙悟空',
      age: 18
    })
    return {
      name: toRef(person, 'name'),
      age: toRef(person, 'age')
    }
  }
}
```

应用：要将响应式对象中的某个属性单独提供给外部使用时。（类似于对象的解构，页面上减少 obj.xxx 操作）

`toRefs` 与 `toRef` 功能一致，但可以批量创建多个 `ref` 对象。

```js
import { reactive, toRefs } from 'vue'
export default {
  setup() {
    const person = reactive({
      name: '孙悟空',
      age: 18
    })
    return {
      ...toRefs(person)
    }
  }
}
```

## shallowReactive 和 shallowRef

`shallowReactive`：只处理最外层属性的响应式。（浅响应式）

```js
const reactive = shallowReactive({
  name: '孙悟空',
  a: {
    b: {
      c: 100
    }
  }
})

reactive.name = '猪八戒' // 响应
reactive.a.b.c++ // 不响应，但是源数据已经发生改变，只是页面不会同步响应，模板重新渲染会显示新的源数据
```

`shallowRef`：只处理基本数据类型的响应式，不进行引用类型的响应式处理。

```js
const ref = shallowRef({
  name: '孙悟空'
})

ref.value.name = '猪八戒' // 不响应
ref.value = { name: '猪八戒' } // 响应
```

使用的场景：

- 如果一个对象数据，结构比较深，但变化时只是外层属性变化。--> `shallowReactive`

- 如果有一个对象数据，后续功能不会修改该对象中的属性，而是生成新的对象来替换。--> `shallowRef`
