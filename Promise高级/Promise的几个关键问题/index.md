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
