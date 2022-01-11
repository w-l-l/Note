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
