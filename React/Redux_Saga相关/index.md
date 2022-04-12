# Redux_Saga相关

`redux-saga` 是一个用于管理应用程序 `Side Effect`（副作用，例如异步获取数据，访问浏览器缓存等）的库。

`redux-saga` 使用了 ES6 的 `Generator` 功能，让异步的流程更易于读取，写入和测试。通过这样的方式，这些异步的流程看起来就像是标准同步的 JavaScript 代码。

你可能已经用了 `redux-thunk` 来处理数据的读取。不同于 `redux-thunk`，你不会再遇到回调地狱了，你可以很容易地测试异步流程并保持你的 `action` 是干净的。

## 可执行生成器

```js
function foo() {
  let n = 0
  return params => new Promise(resolve => setTimeout(resolve, 1000, params + ++n))
}
const getData = foo() // getData 函数每次调用返回一个 promise 对象

// 生成器函数
function *gen() {
  const f1 = yield getData('data')
  console.log(f1)
  const f2 = yield getData(f1)
  console.log(f2)
  const f3 = yield getData(f2)
  console.log(f3)
}

// 可执行生成器函数
function run(gen) {
  const g = gen()
  function next(data) {
    const { value, done } = g.next(data)
    if(done) return value
    value.then(next)
  }
  next()
}

run(gen) // 打印 data1 data12 data123，每次打印间隔 1s
```
