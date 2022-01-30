# Promise

代表了未来某个将要发生的事件（通常是一个异步操作）。

有了 promise 对象，可以将异步操作以同步的流程表达出来，避免了层层嵌套的回调函数（俗称‘回调地狱’）。

ES6 的 Promise 是一个构造函数，用来生成 promise 实例。

Promise 对象的 3 个状态。

- `pending`：初始化状态。

- `fulfilled`：成功状态。

- `rejected`：失败状态。

## 使用 Promise 基本步骤

创建 Promise 对象：

```js
let promise = new Promise((resolve, reject) => {
  // 初始化 promise 状态为 pending
  // 执行异步操作
  if(异步操作成功) {
    resolve(response) // 修改 promise 的状态为 fulfilled
  } else {
    reject(errMsg) // 修改 promise 的状态为 rejected
  }
})
```

调用 Promise 的 then()：

```js
promise.then(
  success => console.log(success),
  err => console.log(err)
)
```

promise 可以链式调用。

then 里面函数 return 的结果可以在后面 then 中的函数接收到。

```js
let promise = new Promise((resolve, reject) => resolve(100))

promise.then(e => e).then(e => console.log(`接收到数据 ${e}`)) // 接收到数据 100
```

这种没有什么意义，通常我们都会 return 一个 promise 对象。

当 return 一个 promise 对象的时候，后续 then 中函数的第一个参数就会作为 return 的 promise 对象的 resolve / reject 传递的参数。

```js
let promise = new Promise((resolve, reject) => resolve(100))

promise.then(e => new Promise((resolve, reject) => reject(e))).then(_ => {}, e => console.log(`接收到数据 ${e}`)) // 接收到数据 100
```
