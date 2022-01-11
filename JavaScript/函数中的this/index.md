# 函数中的this

一个关键字，一个内置的引用变量。

任何函数本质上都是通过某个对象来调用的，如果没有直接指定的就是 `window`。

所有的函数都会有一个变量 `this`，称为函数执行的上下文对象。

它的值是调用函数的当前对象。

在定义函数的时候，`this` 还没有确定，只有在执行时才动态绑定的。

本质上任何函数在执行时都是通过某个对象调用的。

```js
function foo() {
  console.log(this)
}

foo() // window
window.foo() // window
```

## this的情况

以函数形式调用时，严格模式下 this 为 undefined，非严格模式下为 window。

```js
function foo() {
  console.log(this)
}
foo() // window

function foo() {
  'use strict'
  console.log(this)
}
foo() // undefined
```

以方法的形式调用时，this 是调用方法的对象。

```js
const o = {
  name: '方法调用',
  test() {
    console.log(this)
  }
}

o.test() // { name: '方法调用', test: ƒ }
```

以构造函数的形式调用时，this 是新创建的对象。

```js
function Person(name, age) {
  this.name = name
  this.age = age
  console.log(this) // Person { name: '孙悟空', age: 18 }
}

const person = new Person('孙悟空', 18)
```

使用 bind、call 和 apply 调用时，this 是指定的那个对象。

```js
function foo() {
  console.log(this)
}

foo.bind({ name: 'bind' })() // { name: 'bind' }
foo.call({ name: 'call' }) // { name: 'call' }
foo.apply({ name: 'apply' }) // { name: 'apply' }
```

如果传入 null 或者 undefined，严格模式下 this 就为 null 或者 undefined，非严格模式下 this 就指向 window。

```js
function foo1() {
  'use strict'
  console.log(this)
}

function foo2() {
  console.log(this)
}

foo1.bind(null)() // null
foo1.bind(undefined)() // undefined
foo2.bind(null)() // window
foo2.bind(undefined)() // window

foo1.call(null) // null
foo1.call(undefined) // undefined
foo2.call(null) // window
foo2.call(undefined) // window

foo1.apply(null) // null
foo1.apply(undefined) // undefined
foo2.call(null) // window
foo2.call(undefined) // window
```
