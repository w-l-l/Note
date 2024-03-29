# 数据类型

目前 JavaScript 中一共有 8 种数据类型。

| type | 描述 |
| :-----: | :---- |
| `String` | 字符串。 |
| `Number` | 数值。 |
| `Boolean` | 布尔值。 |
| `Null` | 空值。 |
| `Undefined` | 未定义。 |
| `Symbol` | 独一无二的值。 |
| `BigInt` | 表示任意大的整数。 |
| `Object` | 对象。 |

这 8 种数据类型又分为 `简单(基本)数据类型` 和 `复杂(引用)数据类型`。

- 简单数据类型：`String`、`Number`、`Boolean`、`Null`、`Undefined`、`Symbol`、`BigInt`。

- 复杂数据类型（引用数据类型）：`Object`。

## String类型

在 JavaScript 中字符串需要使用引号引起来。

```js
typeof '' // string
```

使用双引号或单引号都可以，但是不要混着使用。

引号不能嵌套，双引号不能放双引号，单引号不能放单引号.

在字符串中我们可以使用 `\` 符作为转义字符，当表示一些特殊字符时可以使用 `\` 符进行转义。

工作中常用的转义字符：

| 转义字符 | 描述 |
| -----: | :---- |
| `\"` | 表示双引号 `"`。 |
| `\'` | 表示单引号 `'`。 |
| `\n` | 表示换行。 |
| `\t` | 表示制表符。（tab 空格） |
| `\\` | 表示 `\`。 |

## Number类型

在 JavaScript 中所有的数值都是 Number 类型。

```js
typeof 1 // number
```

Number 身上有可以表示数值的最大值 / 最小值。

- Number.MAX_VALUE：1.7976931348623157e+308。

- Number.MIN_VALUE（大于零的最小值）：5e-324。

如果使用 Number 表示的数字超过了最大值，则会返回一个 `Infinity `。

- Infinity：表示正无穷。

- -Infinity：表示负无穷。

`NaN` 是一个特殊的数字，表示 `Not A Number`。

使用 typeof 检查 NaN Infinity 都会返回 number。

```js
typeof NaN // number
typeof Infinity // number
```

在 JavaScript 中整数的运算基本可以保证精确，如果进行小数运算，可能得到一个不精确的结果。

```js
0.1 + 0.2 // 0.30000000000000004
```

## Boolean类型

布尔值只有 `true` 和 `false` 两个值，主要用来做逻辑判断。

- true：表示真。

- false：表示假。

```js
typeof true // boolean
typeof false // boolean
```

## Null类型

Null 类型的值只有一个，就是 `null`。

通常我们初始化变量，这个变量之后会被赋值为对象时，使用 null 进行变量的初始化赋值。

```js
const obj = null
```

使用 typeof 检查一个 null 值时，会返回 object。

```js
typeof null // object
```

## Undefined类型

Undefined 类型的值只有一个，就是 `undefined`。

当声明一个变量，但是并不给变量赋值时，它的值就是 undefined。

```js
var str
console.log(str) // undefined
```

使用 typeof 检查一个 undefined值时，会返回 undefined。

```js
typeof undefined // undefined
```

## Symbol类型

ES6 引入了一种新的数据类型 Symbol，表示独一无二的值。

### 使用方式

直接调用 Symbol 函数返回一个 Symbol 类型的值。

```js
const symbol = Symbol() // Symbol()

typeof symbol // symbol
```

或者传递一个参数，用于对 Symbol 的描述。

通过 Symbol 类型值身上的 `description` 属性，可以访问当前对 Symbol 的描述。

```js
const symbol = Symbol('描述') // Symbol(描述)

symbol.description // 描述
```

传递的参数一般是字符串类型，如果不是，内部会将参数转换字符串类型，再返回 Symbol。

```js
Symbol(100) // Symbol(100)
Symbol(true) // Symbol(true)
Symbol(null) // Symbol(null)
Symbol(undefined) // Symbol(undefined)
Symbol({}) // Symbol([object Object])
Symbol([]) // Symbol()
Symbol(function(){}) // Symbol(function() {})
```

虽然 Symbol 看起来像一个构造函数，但是不支持 `new Symbol()` 的方式进行创建。

```js
new Symbol() // 报错
```

### 特点

Symbol 属性对应的值是唯一的，解决命名冲突的问题。

```js
const symbol1 = Symbol() // Symbol()
const symbol2 = Symbol() // Symbol()

symbol1 === symbol2 // false
```

使用 Symbol 可以用来定义对象的唯一属性名。

```js
const obj = {
  [Symbol()]: 1,
  [Symbol()]: 2,
  [Symbol()]: 3
}
// { Symbol(): 1, Symbol(): 2, Symbol(): 3 }
```

Symbol 值不能与其他数据进行计算，包括同字符串拼串。

```js
const str = Symbol()

str + '' // 报错
```

`for in`，`for of` 遍历时不会遍历 Symbol 属性。

也不会被 `Object.keys()` 和 `Object.getOwnPropertyNames()` 检测到。

```js
const obj = {
  name: 'name',
  [Symbol()]: 'Symbol'
}
// { name: 'name', Symbol(): 'Symbol' }

for (const k in obj) {
  console.log(k) // 只会输出 name 属性
}

Object.keys(obj) // ['name']
Object.getOwnPropertyNames(obj) // ['name']
```

如果想获取对象的 Symbol 属性，可以使用 `Object.getOwnPropertySymbols()` 或 `Reflect.ownKeys()`

注意：Object.getOwnPropertySymbols() 只获取 Symbol 类型的属性。

```js
const obj = {
  name: 'name',
  [Symbol()]: 1,
  [Symbol()]: 2,
  [Symbol()]: 3,
}

Object.getOwnPropertySymbols(obj) // [Symbol(), Symbol(), Symbol()]
Reflect.ownKeys(obj) // ['name', Symbol(), Symbol(), Symbol()]

const symbolArr = Object.getOwnPropertySymbols(obj)
obj[symbolArr[0]] // 1
obj[symbolArr[1]] // 2
obj[symbolArr[2]] // 3
```

### 全局共享Symbol

每次调用 Symbol() 方法都会返回一个独一无二的值，哪怕添加的描述一样。

```js
const symbol1 = Symbol('one') // Symbol(one)
const symbol2 = Symbol('one') // Symbol(one)

symbol1 === symbol2 // false
```

可以通过 `Symbol.for()` 的方式，创建一个全局可供搜索的 Symbol 类型值。

传递一个参数，作为 Symbol 的描述。

- 如果这个描述在全局不存在，就创建返回一个新的 Symbol 类型值。

- 如果这个描述全局已经存在，就直接返回已经存在的那个 Symbol 类型值。（类似单例模式）

```js
const symbol1 = Symbol.for('one') // Symbol(one)
const symbol2 = Symbol.for('one') // Symbol(one)

symbol1 === symbol2 // true
```

Symbol() 和 Symbol.for() 区别在于，前者是独一无二的，后者是全局可供搜索的。

```js
const symbol1 = Symbol('one') // 此时 one 描述的 Symbol 不登记到全局
symbol1.description // one

const symbol2 = Symbol.for('one') // 查找全局是否有 one 描述的 Symbol 类型值，有就直接返回，没有就创建登记全局并返回
symbol2.description // one

symbol1 === symbol2 // false

const symbol3 = Symbol.for('one') // 此时全局已经有 one 描述的 Symbol 类型值了，会直接返回之前创建的值
symbol3.description // one

symbol2 === symbol3 // true
```

使用 `Symbol.keyFor()` 可以返回一个全局已登记的 Symbol 类型值的描述。

可以用来检测该描述的 Symbol 类型值是否已被全局登记可供搜索。

```js
const symbol1 = Symbol('one')
Symbol.keyFor(symbol1) // undefined

const symbol2 = Symbol.for('one')
Symbol.keyFor(symbol2) // one
```

### 内置Symbol类型值

除了定义自己使用的 Symbol 类型值外，ES6 还暴露了 12 个内置的 Symbol 类型值，它们代表了内部语言行为。

| Symbol 类型值 | 描述 |
| -----: | :---- |
| Symbol.iterator | 返回一个对象默认迭代器的方法。<br>被 `for of` 使用。 |
| Symbol.asyncIterator | 返回对象默认的异步迭代器的方法。<br>被 `for await of` 使用。 |
| Symbol.match | 用于对字符串进行匹配的方法，也用于确定一个对象是否可以作为正则表达式使用。<br>被 `String.prototype.match()` 使用。 |
| Symbol.replace | 替换匹配字符串的子串的方法。<br>被 `String.prototype.replace()` 使用。 |
| Symbol.search | 返回一个字符串中与正则表达式相匹配的索引的方法。<br>被 `String.prototype.search()` 使用。 |
| Symbol.split | 在匹配正则表达式的索引处拆分一个字符串的方法。<br>被 `String.prototype.split()` 使用。 |
| Symbol.hasInstance | 确定一个构造器对象识别的对象是否为它的实例的方法。<br>被 `instanceof` 使用。 |
| Symbol.isConcatSpreadable | 一个布尔值，表明一个对象是否应该 flattened 为它的数组元素。<br>被 `Array.prototype.concat()` 使用。 |
| Symbol.unscopables | 拥有和继承属性名的一个对象的值被排除在与环境绑定的相关对象外。 |
| Symbol.species | 用于创建派生对象的构造器函数。 |
| Symbol.toPrimitive | 将对象转化为基本数据类型的方法。 |
| Symbol.toStringTag | 用于对象的默认描述的字符串值。<br>被 `Object.prototype.toString()` 使用。 |

在实际工作中，基本用不上这些，需要了解更多的同学可查看：[阮老师写的内置 Symbol 值](https://es6.ruanyifeng.com/#docs/symbol#%E5%86%85%E7%BD%AE%E7%9A%84-Symbol-%E5%80%BC)

笔者只用过 `Symbol.iterator`，让 Object 对象可以被 `for of` 遍历。

#### Symbol.iterator

`iterator` 是一种接口机制，为各种不同的数据结构提供统一的访问机制。

工作原理：

- 创建一个指针对象（遍历器对象），指向数据结构的起始位置。

- 第一次调用 `next` 方法，指针自动指向数据结构的第一个成员。

- 接下来不断调用 `next` 方法，指针会一直往后移动，直到指向最后一个成员。

- 每次调用 `next` 方法，返回的是一个包含 value 和 done 的对象。  
value：当前成员的值。  
done：布尔值。（表示当前数据是否遍历结束）  
遍历结束时，value 的值为 undefined，done 值为true。

默认情况下，`for of` 只能遍历具备 iterator 接口的数据。

比如：Array、arguments、set、map、String...

Object 对象是不支持的。

```js
const arr = [1, 2, 3]
for (const v of arr) {
  console.log(v) // 依次输出1 2 3
}

const obj = {
  a: 1,
  b: 2,
  c: 3
}
// 报错 obj is not iterable
for (const [key, value] of obj) {
  console.log(key, value)
}
```

扩展 Object 原型对象方法，让 Object 对象支持 `for of` 遍历。

```js
Object.prototype[Symbol.iterator] = function() {
  // 获取对象的键名放到一个数组中
  const keys = Object.keys(this)

  // 获取数组长度
  const len = keys.length

  // 初始化索引
  let i = 0

  // 返回一个包含 next 方法的对象，遍历时内部会自行调用
  return {
    // next 方法将返回一个对象，包含当前遍历的值 value，是否遍历完成 done
    next: () => {
      const key = keys[i++]
      return { value: [key, this[key]], done: i > len }
    }
  }
}

// 使用 Generator 简写
Object.prototype[Symbol.iterator] = function* () {
  const keys = Object.keys(this)
  for(let key of keys){
    yield [key, this[key]]
  }
}
```

现在我们再来遍历下 Object 对象。

```js
const obj = {
  a: 1,
  b: 2,
  c: 3
}
for (const v of obj) {
  console.log(v)
}
/*
  a 1
  b 2
  c 3
*/
```

## BigInt类型

`BigInt` 是一个内置对象，可以用来表示任意大的整数。

### 使用方式

直接在整数字面量加 `n`，或者调用 `BigInt()函数`。

通过 `typeof` 测试，`BigInt` 的值将返回 'bigint'。

```js
100n // 100n
typeof 100n // bigint

BigInt(100) // 100n
typeof BigInt(100) // bigint
```

在目前的 `Number` 类型下，只能安全的计算 `-(2^53-1)` 到 `(2^53-1)` 之间的整数，超出此范围整数可能会丢失精度。

`BigInt` 就能完美的解决这个问题。

```js
// 超出范围
console.log(9999999999999999) // 10000000000000000
console.log(9999999999999999n) // 9999999999999999n

// 计算
11111111111111111 + 11111111111111111 // 22222222222222224
11111111111111111n + 11111111111111111n // 22222222222222222n

// 比较 (注意个位数)
9007199254740992 === 9007199254740993 // true
9007199254740992n === 9007199254740993n // false
```

`BigInt` 在转换成 `Boolean` 时，跟 `Number` 转换成 `Boolean` 的效果一样。

```js
Boolean(1) // true
Boolean(1n) // true

Boolean(0) // false
Boolean(0n) // false
```

`BigInt` 和 `Number` 进行比较时不是严格比较，因为两者的类型不同。

```js
1 === 1n // false
1 == 1n // true

2 > 1n // true
2 >= 1n // true

2 > 2n // false
2 >= 2n // true
```

`BigInt` 和 `Number` 两种类型可以相互进行转换。

```js
BigInt(100) // 100n

Number(100n) // 100
```


### 注意事项

`BigInt` 类型的值不能和 `Number` 类型的值直接进行运算，否则会报错。

```js
1 + 1n // TypeError
1 - 1n // TypeError
1 * 1n // TypeError
1 / 1n // TypeError
1 % 1n // TypeError
```

`BigInt` 类型的值除了一元加号运算符外，所有算术运算符都可以使用。

```js
+1n // TypeError
-1n // -1n
1n + 1n // 2n
1n - 1n // 0n
1n * 1n // 1n
1n / 1n // 1n
1n % 1n // 0n
10n ** 2n // 100n

let n = 10n
++n // 11n
--n // 10n
```

`BigInt` 类型的值进行相除的时候，不会像 `Number` 类型的值精确计算，而是向下舍入到最接近的整数。

```js
1 / 2 // 0.5
1n / 2n // 0n
```

## Object类型

对象属于一种复合的数据类型，在对象中可以保存多个不同数据类型的属性。

### 对象分类

#### 内建对象

由 ES 标准中定义的对象，在任何的 ES 的实现中都可以使用。

比如：`Math`、`String`、`Number`、`Boolean`、`Function`、`Object`...

```js
typeof Math // object
typeof String // object
typeof Number // object
typeof Boolean // object
typeof Function // object
typeof Object // object
```

#### 宿主对象

由 JavaScript 的运行环境提供的对象，目前来讲主要指由浏览器提供的对象。

比如：`BOM`、`DOM`

```js
typeof window // object
typeof document // object
```

#### 自定义对象

由开发人员自己创建的对象。

```js
const obj = new Object()
typeof obj // object
```

### 创建对象

通过 `new` 关键字，或者字面量的方式来创建对象。

```js
const obj1 = new Object
const obj2 = {}

typeof obj1 // object
typeof obj2 // object
```

`new` 关键字作用于构造函数，构造函数是专门用来创建对象的函数。

```js
function Person(name) {
  this.name = name
}

const person = new Person('构造函数')
console.log(person) // Person { name: '构造函数' }
```

对象是由一组一组 `{ key: value }` 形式的键值对组成，多个键值对用英文逗号隔开，最后一组键值对后面不用写逗号。（浏览器会忽略最后一个逗号）

- key：属性名，一个对象中的 key 不能重复。

- value：属性值，可以是任意的数据类型，也可以是一个对象。

使用对象字面量的方式，可以在创建对象时，直接指定对象中的属性。

```js
const obj = {
  name: '字面量'
}
```

对象字面量的属性名可以加引号也可以不加，通常我们都不加，但是要使用一些特殊的属性，则必须加引号。

```js
const obj = {
  @@: '特殊属性' // 报错
}

const obj = {
  '@@': '特殊属性' // 正常
}
```

#### 向对象添加属性

通常我们使用 `object.key = value` 的方式向一个对象添加属性。

```js
const obj = {}
obj.name = '添加属性'
console.log(obj) // { name: '添加属性' }
```

对象的属性名不强制要求遵守标识符的规范，但是我们使用时，还是尽量按照标识符的规范去做。

如果要使用特殊的属性名，不能使用 `object.key = value` 的方式来向一个对象添加属性。

必须使用 `object[key] = value` 这种方式。

```js
const obj = {}
obj.@@ = '添加属性' // 报错
obj['@@'] = '添加属性'
console.log(obj) // { @@: '添加属性' }
```

使用这种方式去操作属性，更加的灵活，在 `[key]` 中可以直接传递一个变量，这样变量值是多少就会操作哪个属性。

```js
const obj = {}
const key = 'name'
obj[key] = '添加属性'
console.log(obj) // { name: '添加属性' }
```

#### 读取对象属性

读取对象属性，跟向对象添加属性的两种方式一样。

```js
const obj = {
  name: '读取属性'
}
obj.name // 读取属性
obj['name'] // 读取属性
```

如果读取对象中的某个属性不存在，不会报错而是会返回 undefined。

```js
const obj = {}
obj.name // undefined
```

#### 修改对象属性

修改对象属性，跟向对象添加属性的两种方式一样。

```js
const obj = {
  name: '修改属性'
}

obj.name = '修改属性-1'
console.log(obj) // { name: '修改属性-1' }

obj['name'] = '修改属性-2'
console.log(obj) // { name: '修改属性-2' }
```

#### 删除对象属性

使用关键字 `delete` 来删除对象属性。

语法：`delete object.key` 或 `delete object[key]`。

```js
const obj = {
  name: '删除属性',
  age: 100
}

delete obj.name
console.log(obj) // { age: 100 }

delete obj['age']
console.log(obj) // {}
```

#### 对象是否包含某个属性

使用 `in` 关键字来检查对象是否包含某个属性。

语法：`key in object`

```js
const obj = {
  name: '包含属性'
}

'name' in obj // true
'age' in obj // false
```

### 对象特点

之所以说对象是一种复制数据类型，跟它的存储方式有很大的关联。

JavaScript 中的变量都是保存到栈内存中的。

基本数据类型的值直接在栈内存中存储，值与值之间是独立存在，修改一个变量不会影响其他的变量。

```js
var str1 = '100'
var str2 = str1
str2 = '200'

console.log(str1) // 100
console.log(str2) // 200
```

对象是保存到堆内存中的，每创建一个新的对象，就会在堆内存中开辟出一个新的空间。

而变量保存的是对象的内存地址（对象的引用），如果两个变量保存的是同一个对象引用，当一个通过一个变量修改属性时，另一个也会受到影响。

```js
const obj1 = {
  name: '100'
}
const obj2 = obj1
obj2.name = '200'

console.log(obj1) // { name: '200' }  
console.log(obj2) // { name: '200' }  
```

当比较两个基本数据类型的值时，就是比较值。

```js
var str1 = '100'
var str2 = '100'

str1 === str2 // true
```

而比较两个引用数据类型时，它是比较的对象的内存地址，如果两个对象是一模一样的，但是地址不同，也会返回 false。

```js
const obj1 = {}
const obj2 = {}

obj1 === obj2 // false
```

### 枚举对象属性

使用 `for in` 语句可以枚举对象中的属性。

```js
const obj = {
  a: 1,
  b: 2,
  c: 3
}

for (const key in obj) {
  console.log(key, obj[key]) // 依次输出(a 1),(b 2),(c 3)
}
```
