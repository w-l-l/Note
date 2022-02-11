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
