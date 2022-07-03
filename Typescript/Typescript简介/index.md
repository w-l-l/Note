# Typescript简介

`Typescript` 是 `JavaScript` 的超集。

它对 `JavaScript` 进行了扩展，向 `JavaScript` 引入了类型的概念，并添加了许多新的特性。

`TS` 代码需要通过编译器编译为 JS，然后再交由 JS 解析器执行。

`TS` 完全兼容 JS，换言之任何的 `TS` 代码都可以直接当成 JS 使用。

相较于 JS 而言，`TS` 拥有了静态类型，更加严格的语法，更强大的功能；`TS` 可以在代码执行前就完成代码的检查，减小了运行时异常的出现几率，`TS` 代码可以编译为任意版本的 JS 代码，可有效解决不同 JS 运行环境的兼容问题；同样的功能，`TS` 的代码量要大于 JS，但由于 `TS` 的代码结构更加清晰，变量类型更加明确，在后期代码的维护中，`TS` 却远远胜于 JS。

## 安装

```js
npm i -g typescript
```

使用 tsc 对 `TS` 文件进行编译。

```js
tsc xxx.ts
```

## 编译选项

使用 `tsc xxx.ts` 编译 `TS` 文件，就算报错了仍会产出文件。

如果想要 `TS` 更严厉一些，你可以使用 `noEmitOnError` 编译选项。

```js
tsc --noEmitOnError xxx.ts
```

如果 `TS` 文件报错，使用 `noEmitOnError` 编译的时候则不会生成 JS 文件。

****

`TS` 默认转换为 `ES3`，一个 `ECMAScript` 非常老的版本。我们也可以使用 `target` 选项转换为比较新的版本。

```js
// 转换为 ECMAScript 2015 版本
tsc --target es2015 xxx.ts
```

****

`noImplicitAny`：当类型被隐式推断为 `any` 时，会抛出一个错误。

```js
function foo(n) {
  // Parameter 'n' implicitly has an 'any' type
  console.log(n.subtr(3))
}
```

****

`strictNullChecks`：让我们更明确的处理 `null` 和 `undefined`，也会让我们免于忧虑是否忘记处理 `null` 和 `undefined`。

```js
const o = [...]
const item = o.find(item => item.xxx === xxx)
console.log(item.xxx) // Object is possibly 'undefined'
```
