# 正则表达式

正则表达式用于定义一些字符串的规则。

计算机可以根据正则表达式，来检查一个字符串是否符合规则。

或者将字符串中符合规则的内容提取出来。

## 创建正则表达式的方式

构造函数法。

使用 `RegExp` 构造函数可以创建一个正则表达式对象。

使用 `typeof` 检查正则对象，会返回 `object`。

```js
const reg = new RegExp('a')

typeof reg // object
```

在构造函数中可以传递一个匹配模式作为第二个参数。

- `i`：忽略大小写。

- `g`：全局匹配模式。

可以为一个正则表达式设置多个匹配模式，且顺序无所谓。

```js
new RegExp('a').test('A') // false
new RegExp('a', 'i').test('A') // true

var reg = new RegExp('a')
reg.exec('aaa') // ["a", index: 0, input: "aaa", groups: undefined]
reg.exec('aaa') // ["a", index: 0, input: "aaa", groups: undefined]

var reg = new RegExp('a', 'g')
reg.exec('aaa') // ["a", index: 0, input: "aaa", groups: undefined]
reg.exec('aaa') // ["a", index: 1, input: "aaa", groups: undefined]
reg.exec('aaa') // ["a", index: 2, input: "aaa", groups: undefined]
reg.exec('aaa') null
```

字面量法。

```js
const reg = /正则表达式/匹配模式

/a/i.test('A') // true
```

使用字面量的方式创建更加简单，使用构造函数创建更加灵活。

## 正则表达式的方法

- `test()`：使用这个方法可以用来检查一个字符串是否符合正则表达式的规则，符合返回 `true`，否则返回 `false`。

- `exec()`：该方法在一个指定字符串中执行一个搜索匹配。返回一个结果数组或 `null`。

## 常用正则

- `|`：表示或者的意思。

```js
/a|b/.test('a') // true
/a|b/.test('b') // true
/a|b/.test('c') // false
```

- `[]`：也是或者的意思。

```js
// [ab] == a|b
/a[bc]d/.test('abd') // true
/a[bc]d/.test('acd') // true
/a[bc]d/.test('aad') // false
```

- `[a-z]`：任意小写字母。

```js
/[a-z]/.test('a') // true
/[a-z]/.test('1') // false
```

- `[A-Z]`：任意大写字母。

```js
/[A-Z]/.test('A') // true
/[A-Z]/.test('a') // false
```

- `[A-z]`：任意字母。

```js
/[A-z]/.test('A') // true
/[A-z]/.test('a') // true
```

- `[0-9]`：任意数字。

```js
/[0-9]/.test('1') // true
/[0-9]/.test('a') // false
```

- `[^]`：除了某个元素。

```js
// 不包含 abc 的字符串
/[^abc]/.test('a') // false
/[^abc]/.test('b') // false
/[^abc]/.test('c') // false
/[^abc]/.test('d') // true
```

- `量词`：通过量词可以设置一个内容出现的次数，量词只对它前边的一个内容起作用。

`{n}`：出现 n 次。

```js
/a{3}/.test('a') // false
/a{3}/.test('aa') // false
/a{3}/.test('aaa') // true
```

`{m,n}`：出现 m-n 次。

```js
/a{2,3}/.test('a') // false
/a{2,3}/.test('aa') // true
/a{2,3}/.test('aaa') // true
```

`{m,}`：m 次以上。

```js
/a{2,}/.test('a') // false
/a{2,}/.test('aa') // true
/a{2,}/.test('aaa') // true
```

- `^`：表示开头。

```js
// 以 a 开头的的字符串
/^a/.test('abc') // true
/^a/.test('bac') // false
```

- `$`：表示结尾。

```js
/a$/.test('bca') // true
/a$/.test('abc') // false
```

如果在正则表达式中同时使用 `^` 和 `$` 则要求字符串必须完全符合表达式。

```js
// 字符串必须为 abc
/^abc$/.test('abc') // true
/^abc$/.test('bca') // false
/^abc$/.test('cab') // false
```

- `.`：表示任意字符。

```js
/./.test('a') // true
/./.test('b') // true
/./.test('c') // true
```

- `\`：转义字符。

如果使用正则匹配 `.`、`^`、`$` 等这些特殊的字符，必须使用 `\` 转义字符来匹配。

```js
/./.test('a') // true
/\./.test('a') // false
/\./.test('.') // true
```

- `\w`：表示字母、数字、下划线。相当于 `[A-z0-9_]`。

- `\W`：除了字母、数字、下划线。相当于 `[^A-z0-9_]`。

```js
/\w/.test('1') // true
/\w/.test('a') // true
/\w/.test('_') // true
/\w/.test('@') // false

/\W/.test('1') // false
/\W/.test('a') // false
/\W/.test('_') // false
/\W/.test('@') // true
```

- `\d`：表示数字。相当于 `[0-9]`。

- `\D`：除了数字。相当于 `[^0-9]`。

```js
/\d/.test('1') // true
/\d/.test('a') // false

/\D/.test('1') // false
/\D/.test('a') // true
```

- `\s`：表示空格。

- `\S`：除了空格。

```js
/\s/.test(' ') // true
/\S/.test(' ') // false
```

- `\b`：单词边界。

- `\B`：除了单词边界。

```js
/\bhello\b/.test('hello') // true
/\bhello\b/.test('hello a') // true
/\bhello\b/.test('a hello') // true
/\bhello\b/.test('a hello a') // true
/\bhello\b/.test('helloa') // false
/\bhello\b/.test('ahello') // false
/\bhello\b/.test('ahelloa') // false

/\Bhello\B/.test('hello') // false
/\Bhello\B/.test('hello a') // false
/\Bhello\B/.test('a hello') // false
/\Bhello\B/.test('a hello a') // false
/\Bhello\B/.test('helloa') // false
/\Bhello\B/.test('ahello') // false
/\Bhello\B/.test('ahelloa') // true
```

## 字符串正则相关的方法

- `split()`：可以将一个字符串拆分为一个数组。

该方法可以传递一个正则表达式作为参数，根据正则表达式的匹配结果去拆分字符串。

这个方法即使不指定全局匹配，也会全部拆分。

```js
const str = '1a2b3c4'
str.split(/[abc]/g) // ["1", "2", "3", "4"]
```

- `search()`：搜索字符串中是否含有指定内容。

如果搜索到指定内容，则会返回第一次出现的索引，如果没有搜索到则会返回 -1。

它可以接受一个正则表达式作为参数，然后会根据正则表达式去检索字符串。

该方法只会查找第一次出现的元素位置，即使设置了全局匹配也没用。

```js
const str = '11abc11'

str.search(/a[bd]c/) // 2
```

- `match()`：可以根据正则表达式，从一个字符串中将符合条件的内容提取出来。

默认情况下 `match()` 只会找到第一个符合要求的内容，找到以后就停止检索了。

我们可以设置正则表达式为全局匹配模式，这样就可以匹配到所有的内容。

`match()` 会将匹配到的内容封装到一个数组中返回，即使只查询到一个结果，没有则返回 `null`。

```js
const str = 'abc111adc222aec333'

str.match(/a[bde]c/g) // ["abc", "adc", "aec"]
```

- `replace()`：可以将字符串中指定内容替换为新的内容。

该方法默认只会替换第一个匹配项。

使用正则表达式的全局匹配模式可以进行全局替换。

```js
const str = 'aaa111aaa'

str.replace('a', 'b') // 'baa111aaa'
str.replace(/a/g, 'b') // 'bbb111bbb'
```
