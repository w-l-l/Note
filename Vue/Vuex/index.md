# Vuex

对应用中组件的状态进行集中式的管理（读和写）。

**状态自管理应用：**

- `state`：驱动应用的数据源。

- `view`：以声明方式将 state 映射到视图。

- `actions`：响应在 view 上的用户操作导致的状态变化（包含 n 个更新状态的方法）。

## 多个组件共享状态的问题

- 多个视图依赖于同一个状态。

- 来自不同视图的行为需要变更同一状态。

- 以前的解决办法：  
将数据以及操作数据的行为都定义在父组件。  
将数据以及操作数据的行为传递给需要的各个子组件（有可能需要多级传递）。这就很难以维护。  
Vuex 就是用来解决这个问题的。

## Vuex 的核心概念

`state`：Vuex 管理的状态对象，它是唯一的。

```js
const state = {
  propName: 'initValue'
}
```

`mutations`：包含多个直接更新 `state` 的方法（回调函数）的对象。

谁来触发：

- `action` 中的 `commit(mutationName, params)`

- `this.$store.commit(mutationName, params)`

- `this.$store.commit({ type: mutationName, params: xxx })`

只能包含同步代码，不能写异步代码。因为异步代码 `devtools` 不能很好的追踪这个操作什么时候会被完成。

```js
const mutations = {
  mutationName(state, params) {
    // 更新 state 中的某个属性
  }
}
```

`actions`：包含多个事件回调函数的对象。

通过执行 `commit()` 来触发 `mutation` 的调用，间接更新 `state`。

谁来触发：`this.$store.dispatch(actionName)`。

可以包含异步代码。

```js
const actions = {
  actionName({ commit, state }, params) {
    commit('mutationName', params)
  }
}
```

`getters`：包含多个计算属性（get）的对象。

谁来读取：`this.$store.getters.getterName`。

```js
const getters = {
  getterName(state, getters) {
    return ...
  }
}
```

getters 传参：

```js
const getters = {
  total(state, getters) {
    return function(n) {
      return state.stateName + n
    }
  }
}
// this.$store.getters.total(10)
```

`modules`：包含多个 `module`，一个 `module` 是一个 `store` 的配置对象，与一个组件（包含有共享数据）对应。

```js
export default new Vuex.Store({
  state,
  mutations,
  actions,
  getters,
  modules: {
    a: { state, mutations, actions, getters },
    b: { state, mutations, actions, getters }
  }
})
/*
  访问：
    this.$store.state.a.xxx
    this.$store.state.b.xxx
*/
```

**向外暴露 store 对象：**

```js
export default new Vuex.Store({
  state,
  mutations,
  actions,
  getters
})
```

`store 对象`：

- 所有用 vuex 管理的组件中都多了一个属性 `$store`，它就是一个 `store` 对象。

- 主要属性：  
`state`：注册的 state 对象。  
`getters`：注册的 getters 对象。

- 主要方法：  
`dispatch(actionName, params)`：分发 action。  
`commit(mutationName, params)`：分发 mutation。

**组件中使用：**

```js
import { mapState, mapGetters, mapActions } from 'vuex'

export default {
  computed: {
    ...mapState(['xxx']),
    ...mapGetters(['xxx'])
  },
  methods: mapActions(['xxx'])
}
```

还可以起别名：

```js
import { mapState, mapGetters, mapActions } from 'vuex'

export default {
  computed: {
    ...mapState({
      attr: 'xxx'
    }),
    ...mapGetters({
      attr: 'xxx'
    })
  },
  methods: mapActions({
    attr: 'xxx'
  })
}
```

**映射 store：**

```js
import store from './store'
new Vue({ store })
```
