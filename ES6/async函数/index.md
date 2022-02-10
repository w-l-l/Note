# async函数

真正意义上去解决异步回调的问题，同步流程表达异步操作。

本质上是 `Generator` 的语法糖。

```js
async function fnName() {
  await 表达式 // 一般返回 promise 对象
}
```
