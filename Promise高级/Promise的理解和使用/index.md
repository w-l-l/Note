# Promise的理解和使用

## Promise 是什么

抽象表达：`Promise` 是 `js` 中进行异步编程的新的解决方案（旧的是异步回调）。

具体表达：

- 从语法上来说：`Promise` 是一个构造函数。

- 从功能上来说：`Promise` 对象用来封装一个异步操作并可以获取其结果。

`Promise` 的状态改变（只有 2 种，只能改变一次）。

- `pending` 变为 `resolved(fulfilled)`。

- `pending` 变为 `rejected`。

说明：只有这 2 种，且一个 `Promise` 对象只能改变一次。无论变为成功还是失败，都会有一个结果数据。成功的结果数据一般称为 `value`，失败的结果数据一般称为 `reason`。
