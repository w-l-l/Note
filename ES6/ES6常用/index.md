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

## 箭头函数

箭头函数通常用来定义匿名函数。

```js
let foo = () => {
  console.log('foo执行了')
}
foo() // foo执行了
```

函数体如果只有一个表达式，可以不用大括号包裹，默认会返回当前表达式的结果。

函数体如果有多个语句，必须用大括号包裹，若有需要返回的内容，需要手动返回。

```js
let foo = (n) => n + 1
foo(2) // 3
```

不同参数的写法：

```js
// 没有参数
let foo = () => 1 + 1

// 一个参数，可以不用写小括号
let foo = n => n + 1

// 多个参数
let foo = (a, b, c) => a + b + c
```

箭头函数的特点：

- 比普通函数更加简洁。

- 箭头函数没有自己的 this，也没有 arguments，箭头函数的 this 不是调用的时候决定的，而是在定义的时候，所处在的对象就是它的 this。

- 箭头函数的 this 看外层是否有函数。

- 如果有，外层函数的 this 就是内部箭头函数的 this。

- 如果没有，则 this 是 window。

- 使用 bind、call、apply 指定 this 在箭头函数中是无效的。

```js
let foo = () => console.log(this)
let o = {
  name: '孙悟空',
  age: 18
}

foo.bind(o)() // window
foo.call(o) // window
foo.apply(o) // window
```
