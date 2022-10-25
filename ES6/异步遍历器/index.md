# 异步遍历器

异步遍历器的最大的语法特点，就是调用遍历器的 `next` 方法，返回一个 `Promise` 对象。

```js
asyncIterator
.next()
.then(({ value, done }) => /* ... */)
```
