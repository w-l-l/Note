# Promise准备知识

## 区别实例对象与函数对象

- 实例对象：`new` 函数产生的对象，称为实例对象，简称为对象。

- 函数对象：将函数作为对象使用时，简称为函数对象。

## 回调函数的分类

- 同步回调：立即执行，完成执行完了才结束，不会放入回调队列中。

```js
// 例如：数组遍历相关的回调函数，Promise 的 executor 函数
arr.forEach(() => {...})
new Promise((resolve, reject) => {...})
```

- 异步回调：不会立即执行，会放入回调队列中将来执行。

```js
// 例如：定时器回调，ajax 回调，Promise 的成功和失败回调
setTimeout(() => {...})
$.ajax(...)
new Promise((resolve, reject) => {...})
.then(() => {...})
.catch(() => {...})
```
