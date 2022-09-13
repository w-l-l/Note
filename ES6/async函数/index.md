# async函数

真正意义上去解决异步回调的问题，同步流程表达异步操作。

本质上是 `Generator` 的语法糖。

```js
async function fnName() {
  await 表达式 // 一般返回 promise 对象
}
```

## 特点

- async 函数始终返回一个 promise 对象。

```js
async function foo() {
  console.log(1)
}

foo() // 打印 1，返回 promise 对象
```

- async 取代 Generator 函数的星号，await 取代 Generator 的 yield。

- 不需要像 Generator 去调用 next 方法，遇到 await 等待，当前的异步操作完成才往下执行。

```js
async function foo() {
  await new Promise(resolve => setTimeout(resolve, 3000))
  console.log(1)
}

foo() // 等待 3s 后才会打印 1
```

- 语义上更为明确，使用简单，到目前为止没有任何副作用。

- await 的返回值是后面 promise 实例对象 resolve 传递的参数。

```js
async function foo() {
  const value = await new Promise(resolve => resolve(100))
  console.log(value)
}

foo() // 打印 100
```

## 注意

- `await` 必须写在 `async` 函数中（`ES2022` 之前），当然 `async` 函数中可以没有 `await`。

- 如果 `await` 的 `Promise` 失败了，就会抛出异常，需要通过 `try...catch` 来捕获处理。

## 顶层 await

从 `ES2022` 开始，允许在模块的顶层独立使用 `await` 命令，它的主要目的是使用 `await` 解决模块异步加载的问题。

**注意：如果加载多个包含顶层 await 命令的模块，加载命令是同步执行的。**

```js
// x.js
console.log('x1')
await new Promise(resolve => setTimeout(resolve, 1000))
console.log('x2')

// y.js
console.log('y')

// z.js
import './x.js'
import './y.js'
console.log('z')
```

上面代码有三个模块，最后的 z.js 加载了 x.js 和 y.js，打印结果为 x1、y、x2、z。这说明 z.js 并没有等待 x.js 加载完成，再去加载 y.js。

顶层的 `await` 命令有点像，交出代码的执行权给其他的模块加载，等异步操作完成后，再拿回执行权，继续向下执行。
