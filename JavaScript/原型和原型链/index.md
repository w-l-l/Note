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

## 原型链（隐式原型链）

访问一个对象的属性时，先在自身属性中查找，找到返回。

如果没有，再沿着 `__proto__` 这条链上查找，找到返回。

如果最终没找到，返回 undefined，不会报错。

```js
function Foo() {}

Foo.prototype.key = 'key'

const foo = new Foo()
foo.key // 'key'
foo.key = 'foo'
foo.key // 'foo'
foo.__proto__.key // 'key'
```

函数的显式原型指向的对象默认是空 Object 实例对象（但 Object 不满足）。

```js
Object.prototype instanceof Object // false
```

所有函数都是 Function 的实例（包含 Function）。

```js
// Function 是通过 new 自己产生的实例
Function.__proto__ === Function.prototype // true
```

Object 的原型对象是原型链的尽头。

```js
Object.prototype.__proto__ // null
```

## 原型链属性问题

读取对象的属性值时，没有会自动到原型链中查找。

设置对象的属性时，不会查找原型链，如果当前对象中没有此属性，直接添加此属性并设置其值。

方法一般定义在原型中，属性一般通过构造函数定义在对象本身上。

```js
function Person(name, age) {
  this.name = name
  this.age = age
}

Person.prototype.say = function() {
  console.log(`我叫${this.name}，今年${this.age}岁`)
}

const person = new Person('孙悟空', 18)
person.say() // 我叫孙悟空，今年18岁
```

## 探索 instanceof

`instanceof` 是如何判断的。

表达式：`object instanceof constructor`。

如果 constructor 函数的显式原型对象在 object 对象的原型链上，则返回 true，否则返回 false。
