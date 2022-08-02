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
