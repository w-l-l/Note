# 操作符

## keyof 操作符

对一个对象类型使用 `keyof` 操作符，会返回该对象属性名组成的一个字符串或者数字字面量的联合。

```ts
type O = { x: number, y: number }
type P = keyof O
const n: P = 'x' // 'x' | 'y'
```

我们希望获取一个对象给定属性名的值，为此，我们需要确保我们不会获取对象上不存在的属性。所以我们在两个类型之间建立一个约束。

```ts
function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key]
}

const o = {
  a: 1,
  b: 2,
  c: 3,
  d: 4
}
getProperty(o, 'a')
getProperty(o, 's') // 报错，第二个参数只能是 'a' | 'b' | 'c' | 'd'
```

## typeof 类型操作符

JS 本身就有 `typeof` 操作符。

而 TS 添加的 `typeof` 方法可以在类型上下文中使用，用于获取一个变量或者属性的类型。

```ts
let x = 'hello'
let n: typeof x // let n: string
```

如果仅仅用来判断基本数据类型，自然是没什么太大用处，和其他的类型操作符搭配使用才能发挥它的作用。

比如搭配 TS 内置的 `ReturnType<T>`，你传入一个函数类型，`ReturnType<T>` 会返回该函数的返回值类型。

```ts
type F = (x: number) => boolean
type K = ReturnType<F> // type K = boolean
```

如果我们直接对一个函数名使用 `ReturnType` 则会报错。

```ts
function foo() {
  return ''
}
type K = ReturnType<foo> // 报错，因为值和类型不是一种东西

type K = ReturnType<typeof foo> // type K = string
```

对对象使用 `typeof`：

```ts
const person = { name: 'xxx', age: 18 }
type P = typeof person // type P = { name: string, age: number }
```

对函数使用 `typeof`：

```ts
function foo<T>(x: T): T {
  return x
}
type F = typeof foo // type F = <T>(x: T) => T
```

对 `enum` 使用 `typeof`：

```ts
enum person {
  name,
  age
}
type P = typeof person // 类似于 type P = { name: number, age: number }
```

不过对一个 `enum` 类型只使用 `typeof` 一般没什么用，通常还会搭配 `keyof` 操作符用于获取属性名的联合字符串。

```ts
enum person {
  name,
  age
}
type result = keyof typeof person // type result = 'name' | 'age'
```
