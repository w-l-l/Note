# webpack配置详解

## entry

```js
// string 单入口
module.exports = {
  entry: './src/index.js'
}
```

单入口，打包形成一个 `chunk`，输出一个 `bundle` 文件，此时 `chunk` 的名称默认是 `main`。

****

```js
// array
module.exports = {
  entry: ['./src/index.js', './src/index.html']
}
```

多入口，所有入口文件最终只会形成一个 `chunk`，输出出去只有一个 `bundle` 文件。

通常我们在 `HMR` 功能中让 html 热更新生效才会使用。

****

```js
// object
module.exports = {
  entry: {
    index: './src/index.js',
    test: './src/test.js'
  }
}
```

多入口，有几个入口文件就形成几个 `chunk`，输出几个 `bundle` 文件，此时 `chunk` 的名称是 `key` 值。

****

```js
// 特殊用法
module.exports = {
  index: ['./src/index.js', './src/test.js'],
  add: './src/add.js'
}
```

数组会形成一个 `bundle` 文件，所以这种方式只会形成两个 `bundle` 文件。
