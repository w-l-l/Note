# 自定义Promise

## 整体结构

```js
// 手写 Promise
(function(window) {
  // 状态变量
  const PENDING = 'pending'
  const RESOLVED = 'resolved'
  const REJECTED = 'rejected'

  // Promise 构造函数
  function Promise(executor) {...}

  // .then 原型方法
  Promise.prototype.then = function(onResolved, onRejected) {...}

  // .catch 原型方法
  Promise.prototype.catch = function(onRejected) {...}

  // .resolve 静态方法
  Promise.resolve = function(value) {...}

  // .reject 静态方法
  Promise.reject = function(reason) {...}

  // .all 静态方法
  Promise.all = function(promises) {...}

  // .race 静态方法
  Promise.race = function(promises) {...}

  // 自定义 .resolveDelay 静态方法
  Promise.resolveDelay = function(value, time) {...}

  // 自定义 .rejectDelay 静态方法
  Promise.rejectDelay = function(reason, time) {...}

  // 暴露全局 Promise 方法
  window.Promise = Promise
})(window)
```

## Promise 构造函数

```js
// Promise 构造函数
function Promise(executor) {
  // 当前状态
  this.status = PENDING
  // 当前数据
  this.data = undefined
  // 当前所有回调 [{ onResolved, onRejected }]
  this.callbacks = []
  // 成功失败公共回调
  const resolveOrReject = (status, data, onName) => {
    // 因为在 executor 中，resolve 或者 reject 可以调用多次，但是只有第一次生效，所以这里要判断下状态
    if(this.status !== PENDING) return
    this.status = status
    this.data = data
    // 如果 resolve 或者 reject 没被异步函数包裹，则 callbacks 为空
    if(!this.callbacks.length) return
    setTimeout(() => this.callbacks.forEach(item => item[onName]()))
  }
  // 成功回调
  const resolve = value => resolveOrReject(RESOLVED, value, 'onResolved')
  // 失败回调
  const reject = reason => resolveOrReject(REJECTED, reason, 'onRejected')

  // 同步执行回调函数
  try {
    executor(resolve, reject)
  } catch(error) {
    reject(error)
  }
}
```
