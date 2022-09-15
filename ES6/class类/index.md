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
    console.log('我是Student类的方法', this.name)
  }
}

const stu = new Student('孙悟空', 100)
stu.sayPerson() // 我是Student类的方法 孙悟空
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
