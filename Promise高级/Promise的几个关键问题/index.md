# Promise的几个关键问题

## 如何改变 Promise 的状态

- `resolve(value)`：如果当前是 `pending` 就会变为 `resolved`。

- `reject(reason)`：如果当前是 `pending` 就会变为 `rejected`。

- 抛出异常：如果当前是 `pending` 就会变为 `rejected`。
