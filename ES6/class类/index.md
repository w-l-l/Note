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
