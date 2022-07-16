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
    private _sex: string,
    protected age: number
  ) {
    this.name = name
    this._sex = _sex
    this.age = age
  }
  get sex() {
    return this._sex
  }
  set sex(value) {
    this._sex = value
  }
}
```

## 属性存取器

对于一些不希望被任意修改的属性，可以将其设置为 `private`。

直接将其设置为 `private` 将导致无法再通过对象修改其中的属性。

我们可以在类中定义一组读取、设置属性的的方法，这种对属性读取或设置的方法称为 `属性的存取器`。

读取属性的方法叫 `getter` 方法，设置属性的方法叫 `setter` 方法。

```ts
class Person {
  constructor (private _name: string) {
    this._name = _name
  }
  get name() {
    return this._name
  }
  set name(value) {
    this._name = value
  }
}
```

## 抽象类（abstract class）

抽象类是专门用来被其他类所继承的类，它只能被其他类所继承，不能用来创建实例。

```ts
abstract class Person {
  abstract name: string // 抽象属性
  abstract run(): void // 抽象方法
  say() {
    console.log(this.name)
  }
}

class Student extends Person {
  // 必须实现抽象类中的抽象属性和抽象方法
  constructor(public name: string) {
    super()
    this.name = name
  }
  run() {
    // ...
  }
}
const student = new Student('孙悟空')
student.say() // 孙悟空
```

使用 `abstract` 开头的属性（方法）叫做抽象属性（方法），抽象属性（方法）没有具体实现，只能定义在抽象类中，继承抽象类时抽象方法必须要实现。

## 接口（interface）

接口的作用类似于抽象类，不同点在于接口中的所有属性和方法都是没有实值的，换句话说接口中的所有方法都是抽象方法。接口主要负责定义一个类的结构，接口可以去限制一个对象的接口，对象只有包含接口中定义的所有属性和方法时才能匹配接口。同时，可以让一个类去实现接口，实现接口时类中要包含接口中的所有属性。

```ts
interface Person {
  name: string
  age: number
}

function foo(person: Person) {
  console.log(person)
}
foo({ name: '孙悟空', age: 18 })

class Student implements Person {
  constructor(
    public name: string,
    public age: number
  ) {
    this.name = name
    this.age = age
  }
}
```

## 泛型（Generic）

定义一个函数或类时，有些情况下无法确定其中要使用的具体类型（返回值、参数、属性的类型不能确定），此时泛型便能够发挥作用。

```ts
function test(arg: any): any {
  return arg
}
```

上例中，test 函数有一个参数类型不确定，但是能确定的是其返回值的类型和参数的类型是相同的，由于类型不确定所以参数和返回值均使用了 `any`，但是很明显这样做是不合适的，首先使用 `any` 会关闭 TS 的类型检查，其次这样设置也不能体现出参数和返回值是相同的类型。

### 使用泛型

```ts
function test<T>(arg: T): T {
  return arg
}
```

这里的 `<T>` 就是泛型，`T` 是我们给这个类型起的名字（不一定非叫 `T`，叫什么都行），设置泛型后就可以在函数中使用 `T` 来表示该类型。所以泛型其实很好理解，就表示某个类型。

- 方式一（直接使用）：

```ts
test(10)
// 使用时可以直接传递参数使用，类型会由 TS 自动推断出来，但有时候编译器无法自动推断时还需要使用下面的方式
```

- 方式二（指定类型）：

```ts
test<number>(10)
// 在函数名后面手动指定泛型
```

### 指定多个泛型

可以同时指定多个泛型，多个泛型间使用逗号隔开。

```ts
function test<T, K>(a: T, b: K): K {
  return b
}
test<number, string>(10, 'xxx')
// 使用泛型时，完全可以将泛型当成是一个普通的类去使用
```

### class类中使用泛型

```ts
class MyClass<T> {
  prop: T
  constructor(prop: T) {
    this.prop = prop
  }
}

new MyClass<string>('prop')
```

### 泛型约束

```ts
interface Inter {
  length: number
}

function test<T extends Inter>(arg: T): number {
  return arg.length
}
test<string>('1234')
```

使用 `T extends Inter` 表示泛型 `T` 必须是 `Inter` 的子类，不一定非要使用接口类和抽象类同样适用。
