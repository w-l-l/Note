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

## Object扩展

属性描述符 `descriptor`：

- `value`：指定值。

- `writable`：标识当前属性值是否是可修改的，默认 false。

- `configurable`：标识当前属性是否可配置（删除、对该属性重新定义），默认 false。

- `enumerable`：标识当前属性是否可枚举，默认 false。

- `get`：用来获取当前属性值的回调函数，默认 undefined。

- `set`：监视当前属性值变化触发的回调函数，并且实参为修改后的值，默认值 undefined。

访问器属性：`configurable`、`enumerable`、`get`、`set`。

数据属性：`configurable`、`enumerable`、`writable`、`value`。

`get` 和 `value`、`set` 和 `writable` 不能同时存在，否则会报错。

```js
var o = {}

Object.defineProperty(o, 'name', {
  get() {
    return '孙悟空'
  },
  value: '猪八戒'
}) // 报错

Object.defineProperty(o, 'age', {
  set(v) {
    console.log(v)
  },
  writable: true
}) // 报错
```

- `Object.create(prototype, [descriptors])`：以指定对象为原型创建新的对象。

```js
var o1 = {
  name: '孙悟空'
}
var o2 = Object.create(o1)

o2.__proto__ === o1 // true
```

- `Object.defineProperty(obj, prop, descriptor)`：在对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

```js
var o = {}

Object.defineProperty(o, 'name', {
  value: '孙悟空'
})

o.name // 孙悟空
```

- `Object.defineProperties(obj, props)`：可以在对象上定义多个新的属性或者修改现有属性，并返回该对象。

```js
var o = {}

Object.defineProperties(o, {
  name: {
    value: '孙悟空'
  },
  age: {
    value: 18
  }
})

o.name // 孙悟空
o.age // 18
```

对象本身的两个方法：

- `get propertyName(){}`：用来得到当前属性值的回调函数。

- `set propertyName(){}`：用来监视当前属性值变化的函数。

```js
var o = {
  v: '孙悟空',
  get name() {
    return this.v
  },
  set name(v) {
    this.v = v
  }
}
o.v // 孙悟空
o.name // 孙悟空
o.name = '猪八戒'
o.v // 猪八戒
o.name // 猪八戒
```
