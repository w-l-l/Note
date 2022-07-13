# 面对对象class

面对对象是程序中一个非常重要的思想，它被很多同学理解成了一个比较难，比较深奥的问题，其实不然。面对对象很简单，简而言之就是程序之中所有的操作都需要通过对象来完成。

举例来说：

- 操作浏览器要使用 `window` 对象。

- 操作网页要使用 `document` 对象。

- 操作控制台要使用 `console` 对象。

一切操作都要通过对象，也就是所谓的面向对象，那么对象到底是什么呢？

这就要先说程序是什么。计算机程序的本质就是对现实事物的抽象，抽象的反义词是具体，比如：照片是对一个具体的人的抽象，汽车模型是对具体汽车的抽象等等。程序也是对事物的抽象，在程序中我们可以表示一个人、一条狗、一把枪、一颗子弹等等所有的事物。

一个事物到了程序中就变成了一个对象。

在程序中所有的对象都被分成了两个部分：数据和功能。

以人为例：人的姓名、性别、年龄、身高、体重等属于数据；人可以说话、走路、吃饭、睡觉这些属于人的功能。

数据在对象中被称为属性，而这些功能就称为方法。所以简言之，在程序中一切皆是对象。

## 定义 class 类

```ts
/*
  class 类名 {
    属性名: 类型
    constructor(参数: 类型) {
      this.属性名 = 参数
    }
    方法名() {...}
  }
*/
class Person {
  name: string
  constructor(name: string) {
    this.name = name
  }
  sayName() {
    console.log(this.name)
  }
}
```

## 修饰符

对象实质上就是属性和方法的容器，它的主要作用就是存储属性和方法，这就是所谓的封装。

默认情况下，对象的属性是可以任意修改的，为了确保数据的安全性，在 TS 中可以对属性的权限进行设置。

- `readonly`：只读属性。如果在声明属性时添加一个 `readonly`，则属性便成了只读属性无法修改。

```ts
class Person {
  readonly name: string = '孙悟空'
}
const person = new Person()
person.name = '猪八戒' // 赋值报错，因为 name 属性只读
```

TS 中属性具有三种修饰符。

- `public`：默认值，公共属性，修饰的属性可以在任意位置访问、修改。

```ts
class Person {
  public name: string = '孙悟空'
}
```

- `private`：私有属性。只能在类内部进行访问/修改。  
通过在类中添加方法使得私有属性可以被外部访问。

```ts
class Person {
  private _name: string = '孙悟空'
  get name() {
    return this._name
  }
  set name(value) {
    this._name = value
  }
}

class Student extends Person {
  say() {
    console.log(this._name) // 报错
  }
}

const person = new Person()
person.name // 能访问
person._name // 报错
```

- `protected`：受保护的属性，只能在当前类和当前类的子类中访问/修改。

```ts
class Person {
  protected _name: string = '孙悟空'
  get name() {
    return this._name
  }
  set name(value) {
    this._name = value
  }
}

class Student extends Person {
  say() {
    console.log(this._name) // 不报错
  }
}

const person = new Person()
person.name // 能访问
person._name // 报错
```

类中的属性每次都要声明比较麻烦，我们可以直接将属性定义在构造函数中。

```ts
class Person {
  constructor(
    public name: string,
    protected age: number
  ) {
    // ...
  }
}
```
