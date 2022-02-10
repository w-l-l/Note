# Generator

ES6 提供的解决异步编程的方案之一。

Generator 函数是一个状态机，内部封装了不同状态的数据。

用来生成遍历器对象（iterator）。

可暂停函数（惰性求值），`yield` 可暂停，`next` 方法可启动，每次返回的是 yield 后的表达式结果。

## Generator 特点

`function` 与函数名之间有一个星号。

```js
function* gen() {
  ...
}
```

Generator 函数返回的是一个 Generator 对象，而不是执行函数内部的逻辑。

```js
function* gen() {
  console.log(1)
}

gen() // 不会打印 1，而是返回一个生成器对象
```

调用 `next` 方法，函数内部逻辑开始执行，遇到 `yield` 表达式停止，返回 `{ value: yield 后表达式的值 / undefined, done: boolean }`，done 为 true，表示函数执行完成。

```js
function* gen() {
  console.log(1)
  yield 2
  console.log(3)
}

const g = gen()
g.next() // 打印 1，返回 { value: 2, done: false }
g.next() // 打印 3，返回 { value: undefined, done: true }
```

yield 语句返回结果通常为 undefined（一般都是调用异步函数），当调用 next 方法时传参内容会作为启动时 yield 语句的返回值。

```js
function* gen() {
  let p1 = yield 1
  console.log(p1)
  let p2 = yield 2
  console.log(p2)
}

const g = gen()
g.next() // { value: 1, done: false }
g.next(100) // 打印 100，返回 { value: 2, done: false }
g.next(200) // 打印 200，返回 { value: undefined, done: true }
```

## Generator 方法

- `Generator.prototype.next(value)`：返回一个由 yield 表达式生成的值。

- `Generator.prototype.return(value)`：返回给定的值并结束生成器。

```js
function* gen() {
  yield 1
  yield 2
  yield 3
}

const g = gen()
g.next() // { value: 1, done: false }
g.return(100) // { value: 100, done: true }
g.next() // { value: undefined, done: true }
```

- `Generator.prototype.throw(new Error(xxx))`：向生成器抛出一个错误。

```js
function* gen() {
  yield 1
  yield 2
  yield 3
}

const g = gen()
g.next() // { value: 1, done: false }
g.throw(new Error('报错了')) // 直接报错
g.next() // { value: undefined, done: true }
```

该异常可以通过 try...catch 进行捕获。

```js
function* gen() {
  try {
    yield 1
    yield 2
    yield 3
  } catch (e) {}
}

const g = gen()
g.next() // { value: 1, done: false }
g.throw(new Error('报错了')) // 不报错 { value: undefined, done: true }
g.next() // { value: undefined, done: true }
```
