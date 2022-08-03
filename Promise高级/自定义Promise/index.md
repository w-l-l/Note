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
