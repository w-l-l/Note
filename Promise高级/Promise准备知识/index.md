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

## js 中的 Error

错误类型：

- `Error`：所有错误的父类型。

- `ReferenceError`：引用的变量不存在。

- `TypeError`：数据类型不正确的错误。

- `RangeError`：数据值不在其所允许的范围内。

- `SyntaxError`：语法错误。

错误处理：

- 捕获错误：`try {...} catch (err) {...}`。

- 抛出错误：`throw Error(...)`。

错误对象：`try...catch` 捕获的 `err` 是一个对象，包含两个属性。

- `message`：错误相关信息。

- `stack`：函数调用栈记录信息。
