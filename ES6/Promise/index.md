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
  if(/* 异步操作成功 */) {
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

## Promise 扩展

```js
// Promise 构造函数执行时立即调用 exector 函数
new Promise((resolve, reject) => {...} /* exector */)
```

静态方法：

- `Promise.resolve(value)`：返回一个状态由给定 value 决定的 promise 对象。如果该值是 thenable（带有 then 方法的对象），返回的 promise 对象的最终状态由 then 方法执行决定；否则的话（该 value 为空，基本类型或者不带 then 方法的对象），返回的 promise 对象状态为 fulfilled，并且将该 value 传递给对应的 then 方法。通常而言，如果您不知道一个值是否是 promise 对象，使用 Promise.resolve(value) 来返回一个 promise 对象，这样就能将该 value 以 promise 对象形式使用。

```js
Promise.resolve({
  then(resolve, reject) {
    resolve(100)
  }
}) // fulfilled 100

Promise.resolve(new Promise((resolve, reject) => reject(100))) // rejected 100

Promise.resolve(100) // fulfilled 100
```

- `Promise.reject(error)`：返回一个状态为失败的 promise 对象，并将给定的失败信息传递给对应的处理方法。

```js
Promise.reject(100) // rejected 100
```

- `Promise.all([p1, p2...])`：所有 promise 实例成功的时候才会触发成功回调，有一个实例失败就触发失败回调。

```js
const p1 = new Promise((resolve, reject) => setTimeout(_ => resolve(1)))
const p2 = new Promise((resolve, reject) => setTimeout(_ => resolve(2)))

Promise.all([p1, p2]) // fulfilled [1, 2]

const p3 = new Promise((resolve, reject) => setTimeout(_ => reject(3)))

Promise.all([p1, p3]) // rejected 3
```

- `Promise.race([p1, p2...])`：数组中某一个 promise 对象先执行成功就触发成功的回调，先执行失败就触发失败的回调。

```js
const p1 = new Promise((resolve, reject) => setTimeout(_ => resolve(1)))
const p2 = new Promise((resolve, reject) => setTimeout(_ => resolve(2)))

Promise.race([p1, p2]) // fulfilled 1

const p3 = new Promise((resolve, reject) => setTimeout(_ => reject(3)))

Promise.race([p3, p1]) // rejected 3
```

- `Promise.any([p1, p2...])`：接收一个 promsie 对象的集合，当其中的一个 promise 成功，就执行成功的回调，全部失败才会执行失败的回调。

```js
const p1 = new Promise((resolve, reject) => setTimeout(_ => resolve(1)))
const p2 = new Promise((resolve, reject) => setTimeout(_ => resolve(2)))
const p3 = new Promise((resolve, reject) => setTimeout(_ => reject(3)))

Promise.any([p3, p2, p1]) // fulfilled 2
```

- `Promise.allSettled([p1, p2...])`：等到所有的 promise 都执行通过（不管成功或失败），返回一个 promise，并带有一个对象数组，每个对象对应每个 promise 的结果。

```js
const p1 = new Promise((resolve, reject) => setTimeout(_ => resolve(1)))
const p2 = new Promise((resolve, reject) => setTimeout(_ => resolve(2)))
const p3 = new Promise((resolve, reject) => setTimeout(_ => reject(3)))

Promise.allSettled([p1, p2, p3])
/*
  fulfilled
  [
    { status: 'fulfilled', value: 1 },
    { status: 'fulfilled', value: 2 },
    { status: 'rejected', reason: 3 }
  ]
*/
```

原型方法：

- `Promise.prototype.catch(errorCallback)`：promise 实例失败的回调，返回一个新的 promise 实例。

```js
Promise.reject(1).catch(error => {
  console.log(error) // 1
})
```

- `Promise.prototype.then(successCallback, errorCallback)`：promise 实例成功或失败对应的回调，返回一个新的 promise 实例。

```js
Promise.resolve(1).then(success => {
  console.log(success) // 1
  return Promise.reject(2)
}).then(_ => {}, error => {
  console.log(error) // 2
})
```

- `Promise.prototype.finally(finallyCallback)`：不管 promise 实例成功还是失败都会执行 finally 回调，返回一个新的 promise 实例。

```js
Promise.resolve(1).finally(_ => {
  console.log('finally') // finally
})

Promise.reject(1).finally(_ => {
  console.log('finally') // finally
})
```

## Promise 案例

如果前面的 promise 执行失败，我们不想让后续的 promise 操作被终止，可以为每个 promise 指定失败的回调。

```js
Promise.reject('error').then(success => {
  console.log(success, 1) // promise 是失败状态，所以该回调不会执行
}).then(success => {
  console.log(success, 2) // promise 操作被终止
})

Promise.reject('error').then(_ => {}, error => {
  console.log(error) // error
}).then(success => {
  console.log(success, 2) // undefined 2
})
```

如果前面的 promise 执行失败，则立即终止所有 promise 的执行。

```js
new Promise(...).then(...).then(...).catch(error => {
  console.log(error) // 使用 catch 捕获
})
```

## 使用 Promise 封装 ajax 请求

```js
// 可以通过回调函数获取数据，也可以使用 then 获取数据
function promiseAjax(url, callback) {
  return new Promise((resolve, reject) => {
    const xml = new XMLHttpRequest()
    xml.onload = _ => {
      const res = JSON.parse(xml.responseText)
      callback && callback(res)
      resolve(res)
    }
    xml.onerror = err => void reject(err)
    xml.open('get', url, true)
    xml.send()
  })
}
```
