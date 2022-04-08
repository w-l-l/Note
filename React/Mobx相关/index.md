# Mobx相关

`Mobx` 是一个功能强大，上手非常容易的状态管理工具。

`Mobx` 背后的哲学很简单：任何源自应用状态的东西都应该自动地获得。

`Mobx` 利用 getter 和 setter 来收集组件的数据依赖关系，从而在数据发生变化的时候精确知道哪些组件需要重绘，在界面的规模变大的时候，往往会有很多细粒度更新（Vue 类似）。

![mobx流程图](./img/mobx_process.png)

`Mobx` 与 `Redux` 的区别：

- `Mobx` 写法上更偏向于 OOP。

- 对一份数据直接进行修改操作，不需要始终返回一个新的数据。

- 并非单一 `store`，可以多 `store`。

- `Redux` 默认以 JavaScript 原生对象形式存储数据，而 `Mobx` 使用可观察对象。

优点：学习成本小，面向对象编程，而且对 `TS` 友好。

缺点：过于自由（`Mobx` 提供的约定及模板代码很少，代码编写很自由，如果不做一些约定，比较容易导致团队代码风格不统一），相关的中间件很少，逻辑层业务整合是问题。

## Mobx 的使用

`observable` 和 `autorun`。

```js
import { observable, autorun } from 'mobx'

const value = observable.box(0)
const number = observable.box(100)

autorun(_ => {
  console.log(value.get())
})

value.set(1)
value.set(2)
number.set(101)
// 打印 0 1 2 autorun 使用到才能被执行
// 只能是同步，异步需要处理

// 观察对象，通过 map
const map = observable.map({ key: 'value' })
map.get('key') // 获取属性值
map.set('key', 'new value') // 修改属性

// 观察对象，不通过 map
const map = observable({ key: 'value' })
map.key // 获取属性值
map.key = 'new value' // 修改属性

// 观察数组
const list = observable([1, 2, 3])
list[2] = 3
```

`action`、`runInAction` 和严格模式。

```js
import { observable, action, configure, runInAction } from 'mobx'
configure({
  enforceActions: 'always'
})
/*
  严格模式，必须写 action
  如果是 never，可以不写 action
  最好设置 always，防止任意地方修改值，降低不确定性
*/
class Store {
  @observable number = 0
  @observable name = '孙悟空'
  @action add = _ => this.number++
  // action 只能影响正在运行的函数，而无法影响当前函数调用的异步操作
  @action load = async _ => {
    const data = await getData()
    runInAction(_ => {
      this.name = data.name
    })
    // runInAction 解决异步问题
  }
}
const newStore = new Store()
newStore.add()

// 如果在组件监听
componentDidMount() {
  autorun(_ => {
    console.log(newStore.number)
  })
}
```

## Mobx-React 的使用

`react` 组件里使用 `@observer`。

`observer` 函数 / 装饰器可以用来将 `react` 组件转变成响应式组件。

可以观察的局部组件状态：`@observable` 装饰器在 `react` 组件上引入可观察属性。而不需要通过 `react` 的冗长和强制性的 `setState` 机制来管理。

```js
import { observer } from 'mobx-react'
import { observable } from 'mobx'

@observer class Timer extends React.Component {
  @observable secondsPassed = 0

  componnetWillMount() {
    setInterval(_ => this.secondsPassed++, 1000)
  } // 如果是严格模式需要加上 @action 和 runInAction

  componentWillReact() {
    /*
      一个新的生命周期钩子函数
      当组件因为它观察的数据发生了改变，它会安排重新渲染
      这个时候 componentWillReact 会被触发
    */
    console.log('I will re-render, since the todo has changed!')
  }

  render() {
    return <span>Seconds passed: { this.secondsPassed }</span>
  }
}
```

`Provider` 组件：它使用了 `react` 的上下文 `context` 机制，可以用来向下传递 `stores`。要连接到这些 `stores`，需要传递一个 `stores` 名称的列表给 `inject`，这使得 `stores` 可以作为组件的 `props` 使用。

```js
class Store {
  @observable number = 0
  @action add = _ => this.number++
}

export default new Store() // 导出 Store 实例

@inject('myStore')
@observer // 需要转换为响应式组件
class Child extends Component {
  render() {
    return <div>Child--{this.props.myStore.number}</div>
  }
}

@inject('myStore')
class Middle extends Component {
  render() {
    return (
      <>
        Middle-<button onClick={
          this.props.myStore.add()
        }>test</button>
        <Child />
      </>
    )
  }
}

// 通过 Provider 传 store 进去
<Provider myStore={store}>
  <Middle />
</Provider>
```

### 支持装饰器

安装插件。

```js
npm i @babel/core @babel/plugin-proposal-decorators @babel/preset-env
```

创建 `.babelrc`。

```json
{
  "presets": [
    "@babel/preset-env"
  ],
  "plugins": [
    [
      "@babel/plugin-proposal-decorators",
      {
        "legacy": true
      }
    ]
  ]
}
```

创建 `config-overrides.js`。

```js
const path = rquire('path')
const { override, addDecoratorsLegacy } = require('customize-cra')

function resolve(dir) {
  return path.join(__dirname, dir)
}

const customize = _ => (config, env) => {
  config.resolve.alias['@'] = resolve('src')
  if(env === 'production') {
    config.externals = {
      'react': 'React',
      'react-dom': 'ReactDOM'
    }
  }
  return config
}

module.exports = override(addDecoratorsLegacy(), customize())
```

安装依赖。

```js
npm i customize-cra react-app-rewired
```

修改 `package.json`。

```json
{
  ...
  "scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-app-rewired eject"
  }
  ...
}
```
