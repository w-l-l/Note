# class类

通过 `class` 可以定义类。

```js
class Person {}
```

在类中通过 `constructor` 定义构造方法。

```js
class Person {
  constructor(name) {
    this.name = name
  }
}
```

通过 `new` 来创建类的实例。

```js
new Person('孙悟空') // { name: '孙悟空' }
```

class 中定义的方法，会挂载到原型上。

```js
class Person {
  constructor(name) {
    this.name = name
  }
  sayName() {
    console.log(this.name)
  }
}

const per = new Person('孙悟空')
per.sayName() // 孙悟空
Person.prototype.sayName === per.__proto__.sayName // true
```

通过 `extends` 来实现类的继承。

通过 `super` 调用父类的构造方法。

```js
class Person {
  constructor(name) {
    this.name = name
  }
  sayPerson() {
    console.log('我是Person类的方法', this.name)
  }
}

class Student extends Person {
  constructor(name, age) {
    super(name)
    this.age = age
  }
}

const stu = new Student('孙悟空', 100) // { name: '孙悟空', age: 100 }
stu.sayPerson() // 我是Person类的方法 孙悟空
```

如果子类没有定义 `constructor()` 方法，这个方法会默认添加，并且里面会调用 `super()`，也就是说，不管有没有显示定义，任何一个子类都有 `constructor()` 方法。

```js
class Student extends Person {}
// 等同于
class Student extends Person {
  constructor(...args) {
    super(...args)
  }
}
```

重写从父类中继承的一般方法。

```js
class Person {
  constructor(name) {
    this.name = name
  }
  sayPerson() {
    console.log('我是Person类的方法', this.name)
  }
}

class Student extends Person {
  constructor(name, age) {
    super(name)
    this.age = age
  }
  sayPerson() {
    super.sayPerson()
    console.log('我是Student类的方法', this.name)
  }
}

const stu = new Student('孙悟空', 100)
stu.sayPerson()
/*
  我是Person类的方法 孙悟空
  我是Student类的方法 孙悟空
*/
```

## 特点

- function 构造函数会提升，class 类声明不会提升，在 class 类声明之前 new 实例会报错。

```js
new Person('孙悟空') // Person is not defined

class Person {
  constructor(name) {
    this.name = name
  }
}
```

- 通过 `static` 关键字可以定义一个类的一个静态属性，该静态方法不会实例化，只能类自己访问（通常用于一个应用程序创建工具函数）。  
**注意：如果静态方法包含 this 关键字，这个 this 指的是类，而不是实例。**

```js
class Person {
  static name = '孙悟空'
  static sayName() {
    console.log(this.name)
  }
}

const per = new Person()
per.name // undefined
per.sayName() // per.sayName is not a function

Person.name // 孙悟空
Person.sayName() // 孙悟空
```

- class 内部的代码总是在严格模式下执行，当调用静态或原型方法没有指定 this 时，该方法内的 this 将被置为 undefined，不会指向 window。

```js
class Person {
  static sayOne() {
    console.log(this)
  }
  sayTwo() {
    console.log(this)
  }
}

const sayOne = Person.sayOne
const sayTwo = new Person().sayTwo

sayOne() // undefined
sayTwo() // undefined
```

- class 类原型上的属性必须定义在类的外面定义。

```js
class Person {
  say() {
    console.log('say')
  }
  name = '孙悟空'
}

Person.prototype.say() // say
Person.prototype.name // undefiend

Person.prototype.name = '孙悟空'
Person.prototype.name // 孙悟空
```

## 私有字段

在属性前面加上 `#` 号，可以表示私有字段，只能内部访问，外部是访问不了的。

```js
class Person {
  // 注意：私有字段必须在封闭类中声明
  #name
  #age
  constructor(name, age) {
    this.#name = name
    this.#age = age
  }
  say() {
    console.log(this.#name, this.#age)
  }
}

const per = new Person('孙悟空', 100)
per.say() // 孙悟空 100
per.#name // 报错
per['#name'] // 报错
```

私有字段无法在外部访问，读写只能通过内部函数调用进行读写。

```js
class Person {
  // 注意：私有字段必须在封闭类中声明
  #name
  #age
  constructor(name, age) {
    this.#name = name
    this.#age = age
  }
  say() {
    console.log(this.#name, this.#age)
  }
  get() {
    return {
      name: this.#name,
      age: this.#age
    }
  }
  set(name, age) {
    this.#name = name
    this.#age = age
  }
}

const per = new Person('孙悟空', 100)
per.get() // { name: '孙悟空', age: 100 }
per.set('猪八戒', 200)
per.get() // { name: '猪八戒', age: 200 }
```

私有属性不限于从 `this` 引用，只要是在类的内部，实例也可以引用私有属性。

```js
class Foo {
  #n = 1
  getN(foo){
    return foo.#n
  }
}

const foo = new Foo()
foo.getN(foo) // 1
```

私有属性和私有方法前面，也可以加上 `static` 关键字，表示这是一个静态的私有属性或私有方法。

```js
class Person {
  static #name = '孙悟空'
  static age = 18

  static #getName() {
    return Person.#name
  }

  static nameHandle() {
    console.log('nameHandle')
    return Person.#getName()
  }
}

Person.age // 18
Person.nameHandle()
/*
  nameHandle
  孙悟空
*/
Person.#name // 报错
Person.#getName() // 报错
```

## 私有属性和私有方法的继承

父类所有的属性和方法，都会被子类继承，除了私有的属性和方法。

子类无法继承父类的私有属性，或者说，私有属性只能在定义它的 `class` 里面使用。

```js
class Parent {
  #a = 1
  #b() {
    console.log('b')
  }
}

class Child extends Parent {
  constructor() {
    super()
    console.log(this.#a) // 报错
    this.#b() // 报错
  }
}
```

上面示例中，子类 Child 调用父类 Parent 的私有属性和私有方法，都会报错。

如果父类定义了私有属性的读写方法，子类就可以通过这些方法，读写私有属性。

```js
class Parent {
  #a = 1
  getA() {
    return this.#a
  }
  #b() {
    console.log('b')
  }
  callB() {
    this.#b()
  }
}

class Child extends Parent {
  constructor() {
    super()
    console.log(this.getA()) // 1
    this.callB() // b
  }
}
```

## 静态属性和静态方法的继承

父类的静态属性和静态方法，也会被子类继承。

```js
class Parent {
  static a = 1
  static b() {
    console.log('b')
  }
}

class Child extends Parent {}

Child.a // 1
Child.b() // b
```

**注意：静态属性是通过浅拷贝实现继承的。**

```js
class Parent {
  static a = 100
  static b = { n: 100 }
}

class Child extends Parent {}

Child.a--
Child.a // 99
Parent.a // 100

Child.b.n--
Child.b.n // 99
Parent.b.n // 99
```

上面示例中，`Parent.b` 的值是一个对象，浅拷贝导致 `Child.b` 和 `Parent.b` 指向同一个对象。所以，子类 `Child` 修改这个对象的属性值，会影响到父类 `Parent`。

## class 表达式

```js
const MyClass = class Me {
  getClassName() {
    return Me.name
  }
}

const m = new MyClass()
m.getClassName() // Me
Me.name // ReferenceError: Me is not defined
```

上面示例中，类的名字是 Me，但是 Me 只能在 class 内部访问，指代当前类。在 class 外部，这个类只能用 MyClass 引用。

如果类的内部没用到的话，可以省略 Me，也就是可以写成下面的形式。

```js
const MyClass = class {/* ... */}
```

采用 class 表达式，可以写出立即执行的 class。

```js
let person = new class {
  constructor(name) {
    this.name = name
  }
  sayName() {
    console.log(this.name)
  }
}('小明')

person.sayName() // 小明
```

## class 静态块

静态属性的一个问题，它的初始化要么写在类的外部，要么写在 `constructor()` 方法里面。

```js
class Person {
  static name = '孙悟空'
}
Person.name = 'xxx'

/*-------------------*/

class Person {
  static name = '孙悟空'
  constructor() {
    Person.name = 'xxx'
  }
}
```

这两种方法都不是很理想，前者是将类的内部逻辑写到了外部，后者则是每次新建实例都会运行一次。

为了解决这个问题，`ES2022` 引入了静态块，允许在类的内部设置一个代码块，在类生成时运行一次，主要作用是对静态属性进行初始化。

```js
class Person {
  static name = '孙悟空'
  static age
  static {
    this.name = 'xxx'
    this.age = 18
  }
}
```

上面代码的好处是将静态属性的初始化逻辑，写入了类的内部，而且只运行一次。

静态块内部不能有 `return` 语句。

```js
class Person {
  static name
  static {
    this.name = 'xxx'
    return true
  }
}
// Uncaught SyntaxError: Illegal return statement
```

在静态块中只能访问之前声明的静态属性，后面声明的静态属性不能访问。

```js
class Person {
  static name = '孙悟空'
  static {
    console.log(this.name) // 孙悟空
    console.log(this.age) // undefined
  }
  static age = 18
}
```

静态块内部可以使用类名或者 `this`，指代当前类。

```js
class Person {
  static {
    console.log(this === Person) // true
  }
}
```

## new.target

`new` 是从构造函数生成实例对象的命令。`ES6` 为 `new` 命令引入了一个 `new.target` 属性，该属性一般用在构造函数之中，返回 `new` 命令作用于的那个构造函数。如果构造函数不是通过 `new` 命令或 `Reflect.constructor()` 调用的，`new.target` 会返回 `undefined`，因此这个属性可以用来确定构造函数是怎么调用的。

```js
function Person() {
  console.log(new.target, new.target === Person)
}

Person() // undefined false
new Person() // Person true
```

可以通过 `new.target` 来确保构造函数只能通过 `new` 命令调用。

```js
function Person() {
  if(new.target === Person) {
    // ...
  } else {
    throw new Error('必须使用 new 命令生成实例')
  }
}

Person() // 报错
new Person() // 正确
```

`class` 内部调用 `new.target`，返回当前 `calss`。

```js
class Person {
  constructor() {
    console.log(new.target, new.target === Person)
  }
}

new Person() // Person true
```

需要注意的是，子类继承父类，`new.target` 会返回子类。

```js
class Parent {
  constructor() {
    console.log(new.target, new.target === Parent)
  }
}

class Child extends Parent {
  constructor() {
    super()
  }
}

new Child() // Child false
```

利用这个特点，可以写出不能独立使用、必须继承后才能使用的类。

```js
class Parent {
  constructor() {
    if(new.target === Parent) {
      throw new Error('本类不能实例化')
    }
  }
}

class Child extends Parent {
  constructor() {
    super()
  }
}

new Parent() // 报错
new Child() // 正确
```
