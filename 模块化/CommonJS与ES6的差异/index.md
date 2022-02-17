# CommonJS与ES6的差异

它们有两个重大差异：

1. CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。

2. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

第二个差异是因为 CommonJS 加载的是一个对象（即 module.exports 属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

ES6 模块的运行机制与 CommonJS 不一样。ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。

- CommonJS：

```js
// lib.js
let counter = 3
function incCounter() {
  counter++
}
module.exports = {
  counter,
  incCounter
}

// main.js
let counter = require('./lib').counter
let incCounter = require('./lib').incCounter
console.log(counter) // 3
incCounter()
console.log(counter) // 3
```

- ES6：

```js
// lib.js
export let counter = 3
export function incCounter() {
  counter++
}

// main.js
import { counter, incCounter } from './lib'
console.log(counter) // 3
incCounter()
console.log(counter) // 4
```
