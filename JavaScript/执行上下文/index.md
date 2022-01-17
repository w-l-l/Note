# 执行上下文

## 声明提升

通过 var 声明的变量，在定义语句之前就可以访问到，值为 undefined。

```js
console.log(a) // undefined
var a = 100
```

通过 function 声明的函数，在之前就可以直接调用。

```js
foo() // 100
function foo() {
  console.log(100)
}
```

**注意：函数在 JavaScript 中是一等公民，先执行函数提升，再执行变量提升，函数在变量前面。**
