# ES6常用

## let关键字

与 var 类似，用于声明一个变量。

会形成块级作用域。

```js
if(true) {
  let a = 1
}
console.log(a) // a is not defined
```

不能重复声明（使用 let 或 const 声明的变量，再用 var 声明会报错）。

```js
let a = 1
var a = 2 // Identifier 'a' has already been declared
```

不会预处理，不存在变量提升。

```js
console.log(a)
let a = 1 // a is not defined
```

不会挂载到 window 上。

```js
let a = 1
window.a // undefined
```
