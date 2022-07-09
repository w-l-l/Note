# 基本类型

## 类型声明

类型声明是 `TS` 非常重要的特点，通过类型声明可以指定 `TS` 中变量（参数，形参）的类型。

指定类型后，当为变量赋值时，`TS` 编译器会自动检查值是否符合类型声明，符合则赋值，否则报错。

简言之，类型声明给变量设置了类型，使得变量只能存储某种类型的值。

```ts
// let 变量: 类型
let x: string

// let 变量: 类型 = 值
let x: string = '1'

// function 函数名(参数: 类型): 返回值类型 {...}
function foo(a: number, b: number): number {
  return a + b
}
```

## 自动类型判断

`TS` 拥有自动的类型判断机制。

当对变量的声明和赋值是同时进行的，`TS` 编译器会自动判断变量的类型。

所以如果你的变量声明和赋值是同时进行的，可以省略类型声明。

```ts
let x = 1
x = 2
x = '3' // 报错，x 只能是 number 类型
```

## 类型

- `number`：任意数字。

```ts
let x: number = 6
let x: number = 0xf00d
let x: number = 0b1010
let x: number = 0o744
let x: bigint = 100n
```

- `string`：任意字符串。

```ts
let x: string = 'red'
```

- `boolean`：布尔值 `true` 或 `false`。

```ts
let x: boolean = true
let x: boolean = false
```

- `字面量`：限制变量的值就是该字面量的值。

```ts
// 联合类型
let x: 1 | 2 | 3 | 4 = 4
let x: 'red' | 'blue' | 'green' = 'red'
```

- `any`：任意类型，相当于再写 js，而且可能影响其他变量。

```ts
let x: any = 1
x = true
x = '1'
let y: number = 1
y = x // 不会报错
```

- `unknown`：类型安全的 `any`。

```ts
let x: unknown = 1
x = true
x = '1'
let y: number = 1
y = x // 会报错
```

- `void`：没有值或 `undefined`。

```ts
let x: void = undefined
function foo(): void {...}
```

- `never`：不能是任何值。

```ts
function foo(): never {
  throw new Error(message)
}
/* ------------------------ */
function foo(): never {
  while(true) {}
}
/* ------------------------ */
type Num = 'one' | 'two' | 'three'
function foo(n: Num) {
  switch(n) {
    case 'one':
      break
    case 'two':
      break
    case 'three':
      break
    default:
      const num: never = n
  }
}
```

- `object`：任意 js 对象。

```ts
let x: object = {}
```

- `array`: 任意 js 数组。

```ts
let x: number[] = [1, 2, 3]
let x: Array<number> = [1, 2, 3]

// 只读数组：ReadonlyArray<Type>
let x: ReadonlyArray<string> = ['1', '2', '3']
let x: readonly string[] = ['1', '2', '3']
```

- `tuple`：元组，TS 新增类型，固定长度数组。

```ts
let x: [string, number] = ['a', 1]
```

- `enum`：枚举，TS 新增类型。

```ts
enum Color {
  Red,
  Green,
  Blue
}
let c: Color = Color.Green // 1
/* ------------------------ */
enum Color {
  Red = 5,
  Green,
  Blue
}
let c: Color = Color.Green // 6
/* ------------------------ */
enum Color {
  Red = 'Red',
  Green = 'Green',
  Blue = 'Blue'
}
let c: Color = Color.Green // Green
```

一个变量只有几个固定值的时候，我们通常使用枚举来定义。

## 类型断言

有些情况下，变量的类型对于我们来说是很明确的，但是 TS 编译器却并不清楚，此时，可以通过类型断言来告诉编译器变量的类型，断言有两种形式。

```ts
// 第一种
const x: number = (变量 as string).length

// 第二种
const x: number = (<string>变量).length
// 注意：.tsx 文件不支持该方式
```

使用 `as const` 可以将一个对象转为一个类型字面量。

```ts
const x = { name: '孙悟空', age: 18 } as const
x.age = 20 // 报错，因为是只读属性
```

## 可选属性

```ts
// 第一种
const x: { name: string, age?: number } = {
  name: '孙悟空',
  age: 10
}
// age 属性可写可不写

// 第二种
const x: { name: string, [propName: string]: any } = {
  name: '孙悟空',
  a: 1,
  b: true
}
// 添加其他属性不会报错
```

## 函数结构的类型声明

```ts
let foo: (a: number, b: number) => number
foo = function(a: number, b: number) {
  return a + b
}
```
