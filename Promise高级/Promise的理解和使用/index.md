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

## Promise 的基本流程

![Promise的基本流程](./img/Promise_process.png)

## 为什么要用 Promise

指定回调函数的方式更加灵活：可以在请求发出甚至结束后指定回调函数。

- 旧的：必须在启动异步任务前指定。

- `Promise`：启动异步任务 --> 返回 `Promise` 对象 --> 给 `Promise` 对象绑定回调函数（甚至可以在异步结束后指定/多个）。

支持链式调用，可以解决回调地狱的问题。

- 回调地狱：回调函数嵌套调用，外部回调函数异步执行的结果是嵌套回调函数执行的条件。

- 回调地狱的缺点：不便于阅读，不便于异常处理。

- 解决方案：`Promise` 链式调用。

- 终极解决方案：`async` / `await`。

## Promise 的 API

`Promise` 构造函数：`new Promise(executor){}`。

- `executor` 函数：同步执行 `(resolve, reject) => {}`。

- `resolve` 函数：内部定义成功时我们调用的函数 `value => {}`。

- `reject` 函数：内部定义失败时我们调用的函数 `reason => {}`。

**注意：executor 会在 Promise 内部立即同步回调，异步操作在执行器中执行。**

****

`Promise.prototype.then` 方法：`(onResolved, onRejected) => {}`。

- `onResolved` 函数：成功的回调函数 `value => {}`。

- `onRejected` 函数：失败的回调函数 `reason => {}`。

指定用于得到成功 `value` 的成功回调和用于得到失败 `reason` 的失败回调，返回一个 `Promise` 对象。

**注意：.then 中成功或者失败的回调函数都是异步回调函数。**

****

`Promise.prototype.catch` 方法：`(onRejected) => {}`。

- `onRejected` 函数：失败的回调函数 `reason => {}`。

`.then` 方法的语法糖，相当于：`.then(undefined, onRejected)`。

****

`Promise.resolve` 方法：`value => {}`。

- `value`：成功的数据或 `Promise` 对象。

返回一个成功或者失败的 `Promise` 对象。

****

`Promise.reject` 方法：`reason => {}`。

- `reason`：失败的原因。

返回一个失败的 `Promise` 对象。

****

`Promise.all` 方法：`promises => {}`。

- `promises`：包含 n 个 `Promise` 实例的数组。

返回一个新的 `Promise`，只有所有的 `Promise` 都执行成功了才成功，有一个失败就直接失败。

****

`Promise.race` 方法：`promises => {}`。

- `promises`：包含 n 个 `Promise` 实例的数组。

返回一个新的 `Promise`，最先执行完成的 `Promise` 的结果状态就是 `race` 最终的结果状态。

****

`Promise.allSettled` 方法：`promises => {}`。

- `promises`：包含 n 个 `Promise` 实例的数组。

返回一个新的 `Promise`，等所有 `Promise` 都执行完成（不管成功或失败），带有一个对象数组，每个对象对应每个 `Promise` 的结果。
