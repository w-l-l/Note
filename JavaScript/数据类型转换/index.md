# 数据类型转换

一般转换 String、Number、Boolean 这 3 种类型。

## 将其他的数据类型转换为String

### 方式一

调用被转换数据类型的 toString 方法，该方法不会影响到原变量，它会将转换的结果返回。

```js
(1).toString() // "1"
(true).toString() // "true"
```

注意：null 和 undefined 没有 toString 方法，如果调用它们的方法会报错。
```js
(null).toString() // Cannot read property 'toString' of null
(undefined).toString() // Cannot read property 'toString' of undefined
```

### 方式二

调用 String 函数，并将被转换的数据作为参数传递给函数。

```js
String(1) // "1"
String(true) // "true"
```

通过 String 函数做强制类型转换时，对于 Number 和 Boolean 类型的值，实际上就是调用的 toString() 方法，但是对于 Null 和 Undefined 类型来说，就不会调用 toString 方法，因为它们本身就没有该方法。而是将 null 直接转换为 "null"，undefined 直接转换为 "undefined"。

```js
String(null) // "null"
String(undefined) // "undefined"
```

### 方式三

通过 `+''` 的方式进行隐式转换。

```js
1 + '' // "1"
true + '' // "true"
null + '' // "null"
undefined + '' // "undefined"
```

## 将其他的数据类型转换为Number

### 方式一

使用 Number 函数，并将被转换的数据作为参数传递给函数。

字符串转换为数值时注意：

1. 如果是纯数字的字符串，则直接将其转换为数字。
2. 如果字符串中有非数字的内容，则转换为 NaN。
3. 如果字符串是一个空串或者是一个全是空格的字符串，则转换为0。

```js
Number('1') // 1
Number('a') // NaN
Number('') // 0
Number('  ') // 0
```

布尔值转化为数字时，true 转化为 1，false 转化为 0。

```js
Number(true) // 1
Number(false) // 0
```

null 转化为数字时，值为 0。

```js
Number(null) // 0
```

undefined 转化为数字时，值为 NaN。

```js
Number(undefined) // NaN。
```

### 方式二

这种方式专门作用于字符串。

- parseInt()：把一个字符串数字转换为一个整数。

- parseFloat()：把一个字符串数字转换为一个浮点数。

```js
parseInt('1') // 1
parseFloat('1.2') // 1.2
```

如果不是字符串数组将会返回 NaN。

```js
parseInt('a') // NaN
parseFloat('a.b') // NaN
```

如果是数字开头的组合字符串，将会进行截取返回。

```js
parseInt('1a') // 1
parseFloat('1.2a.b') // 1.2
```

如果对非 String 类型的值使用 parseInt 或 parseFloat，它会先将其转换为 String 然后在操作。

```js
// String(true) --> "true"
parseInt(true) // NaN

// String(null) --> "null"
parseInt(null) // NaN

// String(undefined) --> "undefined"
parseInt(undefined) // NaN

// String({}) --> "[object Object]"
parseInt({}) // NaN
```

可以在 parseInt 函数中传递一个第二个参数，来指定数字的进制。

```js
parseInt('1010', 2) // 10
parseInt('12', 8) // 10
parseInt('a', 16) // 10
```

### 方式三

使用运算符进行隐式转换。

```js
+'100' // 100
'100' * 1 // 100

+true // 1
true * 1 // 1

+false // 0
false * 1 // 0
```

## 将其他的数据类型转换为Boolean

### 方式一

使用 Boolean 函数，并将被转换的数据作为参数传递给函数。

Number 类型转换 Boolean 类型，除了 0 和 NaN，其余的都返回 true。

```js
Boolean(0) // false
Boolean(NaN) // false

Boolean(1) // true
Boolean(1.2) // true
Boolean(Infinity) // true
```

String 类型转换 Boolean 类型，除了空字符串，其余的都是返回 true。

```js
Boolean('') // false

Boolean('1') // true
Boolean('a') // true
```

null 和 undefined 都会转换为 false。

```js
Boolean(null) // false
Boolean(undefined) // false
```

Object 类型都会转换为 true。

```js
Boolean({}) // true
Boolean([]) // true
Boolean(function() {}) // true
```

### 方式二

使用非运算符。（!）

```js
!!0 // false
!!NaN // false
!!'' // false
!!null // false
!!undefined // false

!!1 // true
!!1.2 // true
!!Infinity // true
!!'1' // true
!!'a' // true
!!{} // true
!![] // true
!!function() {} // true
```
