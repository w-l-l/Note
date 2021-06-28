# 流程控制语句

`javaScript` 中的程序是从上到下一行一行执行的，通过流程控制语句可以控制程序执行的流程，使程序可以根据一定的条件来选择执行。

语句的分类：

1. 条件判断语句。
2. 条件分支语句。
3. 循环语句。

## 条件判断语句

使用 `if` 关键字进行条件判断。

### 语法一

```js
if (/*条件表达式*/) {
  // 逻辑语句...
}
```

`if` 语句在执行时，会先对条件表达式进行求值判断。

- true：执行 `if` 后的语句。

- false：不会执行 `if` 后的语句。

`if` 语句只能控制紧随其后的那条语句。

如果希望 `if` 语句可以控制多条语句，可以将这些语句统一放到代码块中。

`if` 语句后的代码块不是必须的，但是在开发中尽量写上代码块，即使 `if` 后只有一条语句。

```js
if (true) console.log(100)
```

### 语法二

```js
if (/*条件表达式*/) {
  // 逻辑语句...
} else {
  // 逻辑语句...
}
```

当该语句执行时，会先对 `if` 后的条件表达式进行求值判断。

- true：执行 `if` 后的语句。

- false：执行 `else` 后的语句。

### 语法三

```js
if (/*条件表达式*/) {
  // 逻辑语句...
} else if (/*条件表达式*/) {
  // 逻辑语句...
} else if (/*条件表达式*/) {
  // 逻辑语句...
} else {
  // 逻辑语句...
}
```

当该语句执行时，会从上到下依次对条件表达式进行求值判断。

- true：执行当前条件表达式紧跟的语句。

- false：继续向下执行判断。

如果所有的条件都不满足，则执行最后一个 `else` 后的语句。

该语句中，只会有一个代码块被执行，一旦代码块执行了，则直接结束语句。

## 条件分支语句

使用 `switch` 关键字进行条件分支判断。

```js
switch (/*条件表达式*/) {
  case value1:
    // 逻辑语句...
    break
  case value2:
    // 逻辑语句...
    break
  default:
    // 逻辑语句...
    break
}
```

### 执行流程

`switch` 语句在执行时，会依次将 `case` 后的表达式的值和 `switch` 后的条件表达式的值进行全等比较。

如果比较的结果为 `true`，则从当前 `case` 处开始执行代码。

当前 `case` 后的所有代码都会执行，我们可以在 `case` 的后边跟着一个 `break` 关键字，这样可以确保只会执行当前 `case` 后的语句，而不会执行其他 `case` 后的语句。

如果比较结果为 `false`，则继续向下比较。

如果所有的比较结果都为 `false`，则只执行 `default` 后的语句。

### case后不跟breack

如果判断结果为 `true` 的 `case` 后的语句不跟 `break` 关键字跳出，则会依次向下执行，不会管后续的 `case` 表达式的值是否跟条件表达式的值全等。

直到遇到 `break` 关键字才会跳出判断，否则一直向下执行。

```js
switch (0) {
  case 0:
    console.log(0)
  case 1:
    console.log(1)
  case 2:
    console.log(2)
    break
  default:
    console.log('default')
}
// 打印 0 1 2
```

根据这个特性我们可以处理条件表达式的值不一致的判断流程。

```js
var expression = Math.floor(Math.random() * 4)
switch (expression) {
  case 0:
  case 1:
  case 2:
    // 逻辑语句...
    break
  default:
    // 逻辑语句...
    break
}
// expression 的结果为 0 1 2 时，都会执行同一条逻辑语句
```

### default可以不用放到最后

通常我们在 `switch` 中，`default` 关键字都写在最后的，其实这个顺序并没有严格要求。

```js
switch (0) {
  default:
    console.log('default')
    break
  case 1:
    console.log(1)
    break
  case 2:
    console.log(2)
    break
}
// 打印 default
```

即使你把 `default` 放到其他 `case` 之上，结果不匹配时，仍然会跳回到那个 `default`。

### switch和if的区别

`switch` 语句和 `if` 语句的功能实际有重复的，使用 `switch` 可以实现 `if` 的功能，同样使用 `if` 也可以实现 `switch` 的功能，那么两者有什么区别吗？

```js
// if 语句
var n = 1
if (n > 3) {
  console.log('n大于3')
} else if (n > 2) {
  console.log('n大于2')
} else if (n > 1) {
  console.log('n大于1')
} else {
  console.log('前面条件都不满足') // 执行这句
}

// switch 语句
switch (n) {
  case 3:
    console.log('n等于3')
    break
  case 2:
    console.log('n等于2')
    break
  case 1:
    console.log('n等于1') // 执行这句
    break
  default:
    console.log('前面条件都不满足')
}
```

从上述案例，我们可以得出如下结论：

- `switch` 的执行效率比 `if` 高。<br>因为 `switch` 是随机访问的，`if` 则要自上而下依次判断执行。

- `switch` 中的 `case` 只能是常量，不像 `if` 可以对非常量进行判断。<br>如：`if (n > 1 && n < 3)`，所以有些场合还是使用 `if` 判断比较灵活。

## 循环语句

通过循环语句可以反复执行一段代码多次。

### while循环

```js
while (/*条件表达式*/) {
  // 循环体语句...
}
```

`while` 语句在执行时，先对条件表达式进行求值判断。

如果值为 `true`，则执行循环体语句。

循环体语句执行完毕以后，继续对条件表达式进行判断。

如果值还是为 `true`，则继续执行循环体，以此类推。

如果值为 `false`，则终止循环。

综上所述，循环体语句中必须有改变条件表达式状态的逻辑，否则就会陷入死循环。

```js
// 死循环
let x = 1
while (x > 0) {
  x++
  console.log(x)
}

// 正常循环
let y = 0
while (y < 10) {
  y++
  console.log(y)
}
```

### do...while循环

```js
do {
  // 循环体语句...
} while (/*条件表达式*/)
```

`do...while` 语句在执行时，会先执行循环体语句。

循环体语句执行完毕以后，再对 `while` 后的条件表达式进行判断。

如果结果为 `true`，则继续执行循环体语句，以此类推。

如果结果为 `false`，则终止循环。

实际上 `while` 和 `do...while` 两种语句的功能类似，不同的是 `while` 是先判断再执行，而 `do...while` 是先执行再判断。

`do...while` 可以保证循环体语句至少执行一次，而 `while` 不能。

```js
// 循环体语句不会执行
while (false) {
  console.log(1)
}

// 循环体语句会执行一次
do {
  console.log(1)
} while (false)
```

### for循环

```js
for (/*初始化表达式*/; /*条件表达式*/; /*更新表达式*/) {
  // 循环体语句...
}
```

`for` 循环过程中，初始化表达式只会执行一次，也就是初始化变量。

然后执行条件表达式，判断是否执行循环。

- true：执行循环。

- false：终止循环。

然后执行更新表达式，再进行条件判断，是否继续执行循环体语句。

`for` 循环中的三个部分都可以省略，也可以写在外部。

```js
// 打印 0 1 2 3 4
let i = 0
for (; i < 5; i++) {
  console.log(i)
}

// 死循环 --> 缺少更新表达式，判断条件一直成立
let x = 0
for (; x < 5;) {
  console.log(x)
}

// 死循环 --> 缺少条件表达式
let y = 0
for (; ; y++) {
  console.log(y)
}

// 死循环 --> 不存在任何表达式
for (;;) {
  console.log(1)
}
```

在编码过程中，如果 `for` 循环中缺少了条件表达式或者更新表达式，就很容易造成死循环。

### break和continue

`break` 关键字可以跳出当前循环。

`continue` 关键字可以跳出当次循环。

```js
// break 打印 0 1
for (let i = 0; i < 5; i++) {
  if (i === 2) break // break 之后就跳出循环了，该循环体不会再执行
  console.log(i)
}

// continue 打印 0 1 3 4
for (let i = 0; i < 5; i++) {
  if (i === 2) continue // continue 之后，本次循环体语句 continue 之后的代码将不会执行，但不会影响下次循环的执行
  console.log(i)
}
```

### label标记语句

可以为循环语句创建一个 `label`，用来标记当前循环语句。

语法：

```js
label:
  循环语句
```

可以和 `break` 和 `continue` 一起使用。

`break` 和 `continue` 默认只会对离它最近的循环语句起作用，通过 `label` 标记，可以跳出指定的循环语句。

```js
// 不使用 label 标记
for (let i = 0; i < 5; i++) {
  console.log('外层循环' + i)
  for (let j = 0; j < 5; j++) {
    break
    console.log('内层循环' + j)
  }
}
/*
打印结果：
  外层循环0
  外层循环1
  外层循环2
  外层循环3
  外层循环4
*/

// 使用 label 标记
label:
for (let i = 0; i < 5; i++) {
  console.log('外层循环' + i)
  for (let j = 0; j < 5; j++) {
    break label // 跳出最外层的循环语句
    console.log('内层循环' + j)
  }
}
/*
打印结果：
  外层循环0
*/
```
