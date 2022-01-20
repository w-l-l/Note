# 对象创建模式

## object构造函数模式

先创建空 Object 对象，再动态添加属性或方法。

使用场景：初始化时不确定对象内部数据。

缺点：语句太多。

```js
var obj = new Object()
obj.name = 'xxx'
obj.age = 18
```

## 对象字面量模式

使用 `{}` 创建对象，同时指定属性或方法。

使用场景：初始化时对象内部数据是确定的。

缺点：如果创建多个对象，有重复代码。

```js
var obj = {
  name: 'xxx',
  age: 18
}
```

## 工厂模式

通过工厂函数动态创建对象并返回。

使用场景：需要创建多个对象。

缺点：对象没有一个具体的类型，都是 Object 类型。

```js
function createPerson(name, age) {
  return {
    name: name,
    age: age
  }
}
createPerson('xxx', 18)
createPerson('xxx', 20)
```

## 自定义构造函数模式

通过 new 关键字创建对象。

使用场景：需要创建多个类型确定的对象。

缺点：每个对象都有相同的数据（方法），浪费内存。

```js
function Person(name, age) {
  this.name = name
  this.age = age
  this.say = function() {
    console.log(this.name)
  }
}

var p1 = new Person('p1', 18)
var p2 = new Person('p2', 20)

p1.say() // p1
p2.say() // p2
p1.say === p2.say // false
```

## 构造函数+原型的组合模式

自定义构造函数，属性在函数中初始化，方法添加到原型上。

使用场景：需要创建多个类型确定的对象。

```js
function Person(name, age) {
  this.name = name
  this.age = age
}
Person.prototype.say = function() {
  console.log(this.name)
}

var p1 = new Person('p1', 18)
var p2 = new Person('p2', 20)

p1.say() // p1
p2.say() // p2
p1.say === p2.say // true
```

## 继承模式

### 原型链继承

- 定义父类型构造函数。

- 给父类型的原型添加方法。

- 定义子类型构造函数。

- 创建父类型的对象（实例对象）赋值给子类型的原型。

- 将子类型原型的构造函数属性（constructor）设置为子类型。

- 给子类型原型添加方法。

- 创建子类型的对象：可以调用父类型的方法。

**关键：子类型的显式原型为父类型的实例对象（或显式原型对象）。**

```js
// 父构造函数
function Parent() {}
Parent.prototype.sayName = function() {
  console.log('我是父构造函数身上的方法', this.name)
}

// 子构造函数
function Son(name) {
  this.name = name
}
// Son.prototype = new Parent()
// 避免出现 Parent 中的静态属性，只需要原型上的方法
Son.prototype = Object.create(Parent.prototype)
Son.prototype.constructor = Son

var son = new Son('小明')
son.sayName() // 我是父构造函数身上的方法 小明
```

### 借用构造函数继承

- 定义父类型构造函数。

- 定义子类型构造函数。

- 在子类型构造函数中调用父类型构造函数。

**关键：在子类型构造函数中通过call()调用父类型构造函数。**

```js
// 父构造函数
function Parent(name) {
  this.name = name
}
Parent.prototype.sayName = function() {
  console.log('我是父构造函数身上的方法', this.name)
}

// 子构造函数
function Son(name, age) {
  Parent.call(this, name)
  this.age = age
}

var son = new Son('xxx', 18) // 实例能正常创建
son.sayName() // 报错，父构造函数原型上的方法不能调用
```

### 组合继承

- 原型链 + 借用构造函数的组合继承。

- 利用原型链实现对父构造函数的方法继承。

- 利用 `call()` 借用父构造函数初始化相同的属性。

```js
// 父构造函数
function Parent(name) {
  this.name = name
}
Parent.prototype.sayName = function() {
  console.log('我是父构造函数身上的方法', this.name)
}

// 子构造函数
function Son(name, age) {
  Parent.call(this, name)
  this.age = age
}
Son.prototype = Object.create(Parent.prototype)
Son.prototype.constructor = Son

var son = new Son('xxx', 18)
son.sayName()
```
