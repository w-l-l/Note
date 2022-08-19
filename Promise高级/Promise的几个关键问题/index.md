# Promise的几个关键问题

## 如何改变 Promise 的状态

- `resolve(value)`：如果当前是 `pending` 就会变为 `resolved`。

- `reject(reason)`：如果当前是 `pending` 就会变为 `rejected`。

- 抛出异常：如果当前是 `pending` 就会变为 `rejected`。

## 一个 Promise 指定多个成功/失败，都会调用吗

```js
const p = new Promise((resolve, reject) => resolve(100))

p.then(value => console.log(value), reason => {...})
p.then(value => console.log(value), reason => {...})
```

指定多个成功/失败的回调函数，当 `Promise` 改变为对应状态时都会调用。

## 改变 Promise 状态和指定回调函数谁先谁后

都有可能，正常情况下是先指定回调再改变状态，但也可以先改变状态再指定回调。

如何先改变状态再指定回调？

- 在执行器中直接调用 `resolve()` 或者 `reject()` 同步执行。

- 延迟更长时间才调用 `then()`。

什么时候才能得到数据？

- 如果先指定的回调，那当状态发生改变时，回调函数就会调用他，得到数据。

- 如果先改变的状态，那当指定回调时，回调函数就会调用，得到数据。

**注意：.then 里面的回调函数是异步回调函数。**

## Promise.prototype.then() 返回的新 promise 的结果由什么决定

简单表达：由 `then()` 指定的回调函数执行的结果决定。

详细表达：

- 如果抛出异常，新 `promise` 变为 `rejected`，`reason` 为抛出的异常。

- 如果返回的是非 `promise` 的任意值，新 `promise` 变为 `resolved`，`value` 为返回的值。

- 如果返回的是另一个新的 `promise`，此 `promise` 的结果就会成为新 `promise` 的结果。

```js
new Promise((resolve, reject) => {
  throw 100
})
.then(
  value => console.log(`value1:${value}`),
  reason => console.log(`reason1:${reason}`)
)
.then(
  value => console.log(`value2:${value}`),
  reason => console.log(`reason2:${reason}`)
)
// 打印结果：reason1:100 value2:undefined
```

## Promise 如何串联多个操作任务

`Promise` 的 `.then()` 返回一个新的 `promise`，可以看成 `.then()` 的链式调用。

通过 `.then()` 的链式调用串联多个同步/异步任务。

## Promise 异常穿透

当使用 `promise` 的 `then` 链式调用时，可以在最后指定失败的回调。

前面任何操作出了异常，都会传到最后失败的回调中处理。

```js
new Promise((resolve, reject) => {...})
.then(...)
.then(...)
.then(...)
.then(...)
.catch(...)
// 只要前面 .then 中语法报错或返回 rejected，都会执行 .catch 里面的回调
```

## 中断 Promise 链

在使用 `Promise` 的 `.then()` 链式调用时，在中间中断，不再调用后面的回调函数。

可以在回调函数中返回一个 `pending` 状态的 `Promise` 对象。

```js
new Promise((resolve, reject) => {...})
.then(...)
.then(value => console.log(value), reason => new Promise(() => {})) // 如果前面报错或者返回 rejected，后续的链式调用将不再执行
.then(...)
.then(...)
```

## resolve 一个 Promise 实例会发生什么

如果 `resolve` 传递的是一个 `Promise` 实例，则后续 `.then()` 接收到的值不是 `Promise` 实例，而是这个实例传递过来的数据。

```js
const p1 = new Promise((resolve, reject) => {
  // ...
})

const p2 = new Promise((resolve, reject) => {
  // ...
  resolve(p1)
})
```

上面代码这，p1 和 p2 都是 `Promise` 的实例，但是 p2 的 `resolve` 方法将 p1 作为参数，即一个异步操作的结果返回另一个异步操作。

注意：这时 p1 的状态就会传递给 p2，也就是说，p1 的状态决定了 p2 的状态。如果 p1 的状态是 `pending`，那么 p2 的回调函数就会等待 p1 的状态改变；如果 p1 的状态已经是 `resolved` 或者 `rejected`，那么 p2 的回调函数将会立刻执行。

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(reject, 3000, new Error('error'))
})

const p2 = new Promise((resolve, reject) => {
  resolve(p1)
})

p2.then(value => console.log(value, 'success'))
.catch(reason => console.log(reason, 'error'))
// Error: error 'error'
```

上面代码中，p1 是一个 `Promise`，3 秒后变为 `rejected`。p2 的状态直接改变，`resolve` 方法返回 p1。由于 p2 返回的是另一个 `Promise`，导致 p2 自己的状态无效了，由 p1 的状态决定 p2 的状态。所以，后面的 `then` 语句都变成针对后者 p1。3 秒之后 p1 变为 `rejected`，导致触发 `catch` 方法指定的回调函数。

**注意：reject 一个 Promise 实例，后续接收到的就是该实例，这跟 resolve 有所不同。**
