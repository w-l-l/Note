# ES5常用

## 严格模式

除了正常运行模式（混杂模式），ES5 添加了第二种运行模式：`严格模式`（strict mode）。

顾名思义，这种模式使得 JavaScript 在更严格的语法条件下运行。

严格模式出现的目的（作用）：

- 消除 JavaScript 语法的一些不合理、不严谨之处，减少一些怪异行为。

- 消除代码运行的一些不安全之外，为代码的安全运行保驾护航。

- 为未来新版本的 JavaScript 做好铺垫。

```js
function foo() {
  // 在全局或函数的第一条语句定义为 use strict
  'use strict' // 如果浏览器不支持，只解析为一条简单的语句，没有任何副作用
  ...
}
```

语法和行为改变：

- 必须用 var 声明变量。

```js
function foo() {
  'use strict'
  a = 1
}
foo() // a is not defined
```

- 禁止自定义函数中的 this 指向 window。

```js
function foo() {
  'use strict'
  console.log(this)
}
foo() // undefined
```

- 创建 eval 作用域。

```js
function foo() {
  var a = 10
  eval('var a = 20')
  console.log(a)
}
foo() // 20

function foo() {
  'use strict'
  var a = 10
  eval('var a = 20')
  console.log(a)
}
foo() // 10
```

- 严格模式下定时器中的 this 还是指向 window。

```js
setTimeout(function() {
  'use strict'
  console.log(this) // window
})
```

## JSON对象

- `JSON.stringify`：js 对象（数组）转化为 json 对象（数组）。

```js
JSON.stringify({ name: '孙悟空' }) // "{\"name\":\"孙悟空\"}"
```

- `JSON.parse`：json 对象（数组）转化为 js 对象（数组）。

```js
JSON.parse('{"name": "孙悟空"}') // {name: "孙悟空"}
```
