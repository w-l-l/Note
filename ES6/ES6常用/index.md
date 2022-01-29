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

## const关键字

`cosnt` 用来声明一个常量。

使用 const 定义标识符，必须进行赋值。

```js
const x // Missing initializer in const declaration
```

不能修改。

```js
const a = 1
a = 2 // Assignment to constant variable
```

其他特点跟 let 一样。
