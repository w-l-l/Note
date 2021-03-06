# Immutable相关

`immutable` 是一种持久化数据。一旦被创建就不会被修改。修改 `immutable` 对象的时候返回新的 `immutable`。但是原数据不会改变。

`immutable` 优化性能的方式：`immutable` 实现的原理是 `Persistent Data Structure`（持久化数据结构），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不可变。同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，`immutable` 使用了 `Structural Sharing`（结构共享），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其他节点则进行共享。

![immutable修改流程图](./img/immutable_process.gif)

`immutable` 的不可变性让纯函数更强大，每次都返回新的 `immutable` 的特性让程序员可以对其进行链式操作，用起来更方便。

## Map 对象

`immutable.Map()`：创建一个类似于 js 中的对象的 Map 对象。

```js
import { Map } from 'immutable'

const map = Map({
  name: '孙悟空',
  age: 18
})

console.log(map) // Map { name: '孙悟空', age: 18 }
```

操作 Map 对象。

- 新增。

```js
// map.set
const map1 = map.set('sex', '男')
console.log(map1) // Map { name: '孙悟空', age: 18, sex: '男' }
```

```js
// map.setIn 深层的 set
const map1 = map.setIn(['children', 'name'], 'xxx')
console.log(map1) // Map { name: '孙悟空', age: 18, children: { name: 'xxx' } }
```

**注意：setIn 可以深层操作，第一个参数是个数组，数组中第一个元素是操作对象的 key 值，第二个元素是 value 值，如果不需要可以不用。以下的 map.deleteIn、map.updateIn、map.getIn 同理。**

- 删除。

```js
map.delete('a') // 删除 a 的值
map.deleteIn(['a', 'b']) // 删除 a 中 b 的值
```

- 修改。

```js
// 修改的时候第二个参数是回调函数，参数是目标值的值，返回值为一个更新后的值
map.update('a', a => a + 1) // 让 a 的值 +1
map.updateIn(['a', 'b'], b => b + 1) // 让 a 中 b 的值 +1
```

- 查询。

```js
map.get('a') // 得到 a 的值
map.getIn(['a', 'b']) // 得到 a 中 b 的值
```

****

```js
import { Map } from 'immutable'
let a = Map({
  select: 'users',
  filter: Map({ name: 'Cam' })
})
let b = a.set('select', 'people')

a === b // false
a.get('filter') === b.get('filter') // true
```

如果上述 select 属性给一个组件使用，因为值改变了，`shouldComponentUpdate()` 应该返回 true，而 filter 属性给另一个组件使用，通过判断并无变化，`shouldComponentUpdate` 应该返回 false，此组件就避免了重复进行 `diff` 对比。

## List 对象

`immutable.List()`：创建一个类似于 js 中数组的 List 对象。

```js
import { List } from 'immutable'

const list = List([1, 2, 3])

console.log(list) // List [1, 2, 3]
```

- 增加。

```js
list.push(4) // List [1, 2, 3, 4]
list.splice(3, 0, 4) // List [1, 2, 3, 4]
```

- 删除。

```js
list.splice(1, 1) // List [1, 3]
```

- 修改。

```js
list.splice(1, 1, 5) // List [1, 5, 3]
```

- 查询。

```js
list.get(0) // 1
list.getIn([0, 0]) // 二维数组可以使用 getIn，获取第一个数组中的第一个元素
```

## 其他 API

- `map.merge()`：合并 map 对象。

- `map.toObject()`：immutable 的 map 对象转换为 js 对象（浅转换，只转换最外层）。

- `list.toArray()`：immutable 的 list 对象转 js 数组（浅转换，只转换最外层）。

- `fromJS()`：js 对象或 js 数组，转换为 immutable。

- `toJS`：immutable 的 map 对象或 list 对象转换为 js 对象或 js 数组。

## Immutable + Redux 的开发方式

```js
// reducer.js
import { fromJS } from 'immutable'

const initState = fromJS({
  name: '孙悟空',
  age: 18
})

export default function reducer(prevState = initState, action = {}) {
  const { type, data } = action
  switch(type) {
    case 'SET_NAME':
      return prevState.set('name', data)
    case 'SER_AGE':
      return prevState.set('age', data)
    default:
      return prevState
  }
}
```

```js
// store.js
import { createStore } from 'redux'
import reducer from './reducer'

export default createStore(reducer)
```

```js
// App.jsx
import { connect } from 'react-redux'
function App(props) {
  return (
    <>
      <h1>{props.name}</h1>
      <h1>{props.age}</h1>
      <button onClick={_ => props.setName('猪八戒')}>修改姓名</button>
      <button onClick={_ => props.setAge(props.age + 1)}>修改年龄</button>
    </>
  )
}

export default connect(
  state => ({
    name: state.get('name'),
    age: state.get('age')
  }),
  {
    setName: data => ({ type: 'SET_NAME', data }),
    setAge: data => ({ type: 'SER_AGE', data })
  }
)(App)
```

```js
// index.js
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'
import store from './store'
import App from './App'

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
)
````
