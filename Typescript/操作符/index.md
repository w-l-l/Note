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
