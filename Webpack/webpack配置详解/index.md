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

## output

```js
module.exports = {
  output: {
    filename: 'js/[name].js', // 文件名称（指定名称 + 目录）
    path: resolve(__dirname, 'dist'), // 输出文件目录，将所有资源输出的公共目录
    publicPath: 'img/', // 所有资源引入公共路径前缀 a.png --> img/a.png
    chunkFilename: 'js/[name]_chunk.js', // 非入口 chunk 的名称
    library: '[name]', // 整个库向外暴露的变量名
    libraryTarget: 'window' | 'global' | 'commonjs', // 变量名添加到哪个上
  }
}
```

## module

```js
module.exports = {
  module: {
    rules: [ // loader 的配置
      { test: /\.css$/, use: ['style-loader', 'css-loader'] }, // 多个 loader 用 use
      {
        test: /\.js$/,
        exclude: /node_modules/, // 排除 node_modules 下的 js 文件
        include: resolve(__dirname, 'src'), // 只检查 src 下的 js 文件
        enforce: 'pre', // 优先执行（'post' 为延后执行）
        loader: 'eslint-loader', // 单个 loader 使用 loader 配置
        options: {...}
      },
      {
        oneOf: [...] // oneOf 下的配置只会生效一个
      }
    ]
  }
}
```
