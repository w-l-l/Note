# 异步遍历器

异步遍历器的最大的语法特点，就是调用遍历器的 `next` 方法，返回一个 `Promise` 对象。

```js
asyncIterator
.next()
.then(({ value, done }) => /* ... */)
```

## 异步遍历器的接口

我们知道，一个对象的同步遍历器的接口，部署在 `Symbol.iterator` 属性上面。同样地，对象的异步遍历器，部署在 `Symbol.asyncIterator` 属性上面。不管是什么样的对象，只要它的 `Symbol.asyncIterator` 属性有值，就表示应该对它进行遍历。

```js
const asyncIterable = createAsyncIterable(['a', 'b'])
const asyncIterator = asyncIterable[Symbol.asyncIterator]()

asyncIterator
.next()
.then(iterResult1 => {
  console.log(iterResult1) // { value: 'a', done: false }
  return asyncIterator.next()
})
.then(iterResult2 => {
  console.log(iterResult2) // { value: 'b', done: false }
  return asyncIterator.next()
})
.then(iterResult3 => {
  console.log(iterResult3) // { value: undefined, done: true }
  return asyncIterator.next()
})
```

异步遍历器与同步遍历器最终行为是一致的，只是会先返回 `Promise` 对象，作为中介。

由于异步遍历器的 `next` 方法，返回的是一个 `Promise` 对象。因此，可以把它放在 `await` 命令后面。

```js
(async function() {
  const asyncIterable = createAsyncIterable(['a', 'b'])
  const asyncIterator = asyncIterable[Symbol.asyncIterator]()

  console.log(await asyncIterator.next())
  // { value: 'a', done: false }
  console.log(await asyncIterator.next())
  // { value: 'b', done: false }
  console.log(await asyncIterator.next())
  // { value: undefined, done: true }
})()
```

注意：异步遍历器的 `next` 方法是可以连续调用的，不必等到上一步产生的 `Promise` 对象 `resolve` 以后在调用。这种情况下，`next` 方法会累计起来，自动按照每一步的顺序运行下去。下面是一个例子，把所有的 `next` 方法放在 `Promise.all` 方法里面。

```js
const asyncIterable = createAsyncIterable(['a', 'b'])
const asyncIterator = asyncIterable[Symbol.asyncIterator]()

const [{ value: v1 }, { value: v2 }] = await Promise.all([asyncIterator.next(), asyncIterator.next()])

console.log(v1, v2) // a b
```

另一种用法是一次性调用所有的 `next` 方法，然后 `await` 最后一步操作。

```js
async function runner() {
  const writer = openFile('someFile.txt')
  writer.next('hello')
  writer.next('world')
  await writer.return()
}
```

## 异步 Generator 函数

就像 `Generator` 函数返回一个同步遍历器对象一样，异步 `Generator` 函数的作用，是返回一个异步遍历器对象。

在语法上，异步 `Generator` 函数就是 `async` 函数与 `Generator` 函数的结合。

```js
async function* gen() {
  yield 'hello'
}
const g = gen()
g.next().then(x => console.log(x))
// { value: 'hello', done: false }
```

异步遍历器的设计目的之一，就是 `Generator` 函数处理同步操作和异步操作时，能够使用同一套接口。

```js
// 同步 Generator 函数
function* map(iterable, func) {
  const iter = iterable[Symbol.iterator]()
  while(true) {
    const { value, done } = iter.next()
    if(done) break
    yield func(value)
  }
}

// 异步 Generator 函数
async function* map(iterable, func) {
  const iter = iterable[Symbol.asyncIterator]()
  while(true) {
    const { value, done } = await iter.next()
    if(done) break
    yield func(value)
  }
}
```

上面有两个版本的 `map`，前一个处理同步遍历器，后一个处理异步遍历器，可以看到两个版本的写法基本上是一致的。

异步 `Generator` 函数内部，能够同时使用 `await` 和 `yield` 命令。可以这样理解，`await` 命令用于将外部操作产生的值输入函数内部，`yield` 命令用于将函数内部的值输出。

## for await...of

前面介绍过，`for...of` 循环用于遍历同步的 `Iterator` 接口。新引入的 `for await...of` 循环，则是用于遍历异步的 `Iterator` 接口。

```js
async function* gen() {
  yield 1
  yield 2
  yield 3
}

for await(const x of gen()) {
  console.log(x) // 1 2 3
}
```

上面代码中，`gen()` 返回一个拥有异步遍历器接口的对象，`for...of` 循环自动调用这个对象的异步遍历器的 `next` 方法，会得到一个 `Promise` 对象。`await` 用来处理这个 `Promise` 对象，一旦 `resolve`，就把得到的值传入 `for...of` 的循环体。

如果 `next` 方法返回的 `Promise` 对象被 `reject`，`for await...of` 就会报错，要用 `try...catch` 捕获。

```js
async function* gen() {
  yield 1
  yield 2
  throw new Error('报错了')
  yield 3
}

try {
  for await(const x of gen()) {
    console.log(x) // 1 2
  }
} catch(e) {
  console.error(e) // 报错了
}
```

注意：`for await...of` 循环也可以用于同步遍历器。

```js
for await(const x of [1, 2, 3]) {
  console.log(x) // 1 2 3
}
```

`yield*` 语句也可以跟一个异步遍历器。

```js
async function* gen1() {
  yield 1
  yield 2
}

async function* gen2() {
  yield* gen1()
  yield 3
}

for await(const x of gen2()) {
  console.log(x) // 1 2 3
}
```
