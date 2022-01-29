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

## 变量的解构赋值

从对象或数组中提取数据，并赋值给变量（多个）。

对象的解构赋值：

```js
// 普通解构
let { name, age } = { name: '孙悟空', age: 18 }
name // 孙悟空
age // 18

// 变量重命名
let { name: a, age: b } = { name: '孙悟空', age: 18 }
a // 孙悟空
b // 18

// 连续解构赋值 + 重命名
let { name: { age: a } } = { name: { age: 18 } }
a // 18
```

数组的解构赋值：

```js
// 普通解构
let [a, b, c] = [1, 2, 3]
a // 1
b // 2
c // 3

// 高级解构
let [a, ...arr] = [1, 2, 3, 4]
a // 1
arr // [1, 2, 3, 4]
```

## 模板字符串

简化字符串的拼接。

模板字符串必须用 `` 包含。

变量部分使用 `${var}` 定义。

```js
let name = '孙悟空'
let age = 18
let str = `我叫${name}，今年${age}岁。`

str // 我叫孙悟空，今年18岁。
```
