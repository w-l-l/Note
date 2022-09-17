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
