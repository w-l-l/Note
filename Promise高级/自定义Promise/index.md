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

## .then 原型方法

```js
// .then 原型方法
Promise.prototype.then = function(onResolved, onRejected) {
  // 如果 onResolved 或者 onRejected 不是函数，则设置一个默认函数，能够将 Promise 产生的值继续向后面传递
  typeof onResolved === 'function' || (onResolved = value => value)
  typeof onRejected === 'function' || (onRejected = reason => {throw reason})
  // .then 方法返回的是一个新的 Promise 对象
  return new Promise((resolve, reject) => {
    // 公共回调
    const handle = callback => {
      try {
        const result = callback(this.data)
        // onResolved 或者 onRejected 可能返回一个普通值，也有可能返回一个 Promise 对象，所以这里要判断一下
        result instanceof Promise
          // 如果返回 Promise 对象，则需要获取该对象的结果值，然后改变当前 Promise 对象的 status 和 data
          ? result.then(resolve, reject)
          : resolve(result)
      } catch(error) {
        reject(error)
      }
    }
    // 根据当前 Promise 对象的状态进行相应的操作
    switch (this.status) {
      // PENDING 状态将 .then 中的回调保存到 callbacks 中，后续执行
      case PENDING:
        this.callbacks.push({
          onResolved: () => handle(onResolved),
          onRejected: () => handle(onRejected)
        })
        break
      // RESOLVED 状态则直接执行 onResolved（因为微任务不好手动实现，所以用 setTimeout 代替）
      case RESOLVED:
        setTimeout(() => handle(onResolved))
        break
      // REJECTED 状态则直接执行 onRejected
      case REJECTED:
        setTimeout(() => handle(onRejected))
        break
    }
  })
}
```

## .catch 原型方法

```js
// .catch 原型方法
Promise.prototype.catch = function(onRejected) {
  return this.then(undefined, onRejected)
}
```

## .resolve 静态方法

```js
// .resolve 静态方法
Promise.resolve = function(value) {
  return new Promise((resolve, reject) => {
    value instanceof Promise
      ? value.then(resolve, reject)
      : resolve(value)
  })
}
```

## .reject 静态方法

```js
// .reject 静态方法
Promise.reject = function(reason) {
  return new Promise((resolve, reject) => reject(reason))
}
```

## .all 静态方法

```js
// .all 静态方法
Promise.all = function(promises) {
  const values = []
  let resolvedCount = 0
  return new Promise((resolve, reject) => {
    promises.forEach((item, index) => {
      Promise.resolve(item).then(value => {
        values[index] = value
        resolvedCount++
        if(resolvedCount === promises.length) resolve(values)
      }, reject)
    })
  })
}
```

## .race 静态方法

```js
// .race 静态方法
Promise.race = function(promises) {
  return new Promise((resolve, reject) => {
    promises.forEach(item => {
      Promise.resolve(item).then(resolve, reject)
    })
  })
}
```
