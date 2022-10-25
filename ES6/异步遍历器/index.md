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
