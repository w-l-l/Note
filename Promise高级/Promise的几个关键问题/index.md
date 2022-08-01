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
