# 原型和原型链

## 原型

每个函数都有一个 `prototype` 属性，它默认指向一个 Object 空对象。（简称原型对象）

原型对象中有一个属性 `constructor`，它指向函数对象。

```js
function foo() {}

foo.prototype.constructor === foo // true
```

给原型对象添加属性（通常都是方法），函数的所有实例对象自动拥有原型中的属性（方法）。

```js
function Foo() {}

Foo.prototype.test = function() {
  console.log('test')
}

const f1 = new Foo()
const f2 = new Foo()

f1.test() // test
f2.test() // test

f1.__proto__.test === f2.__proto__.test // true
```

## 显式原型与隐式原型

每个函数 Function 都有一个 `prototype`，即显式原型（属性）。

每个实例对象都有一个 `__proto__`，可称为隐式原型（属性）。

对象的隐式原型的值为其对应构造函数的显式原型的值（地址值）。

```js
function Foo() {}

const foo = new Foo()

foo.__proto__ === Foo.prototype // true
```

程序员能直接操作显式原型，但不能直接操作隐式原型（ES6 之前）。

注意：通过 Function.prototype.bind 方法构造出来的函数没有 prototype 属性。

因为 Function.prototype 是函数对象。

```js
Function.prototype // ƒ () { [native code] }
```

也可以理解为 Function.prototype 函数没有 prototype 属性。

```js
Function.prototype.prototype // undefined
```
