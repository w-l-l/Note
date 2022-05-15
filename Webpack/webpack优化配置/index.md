# webpack优化配置

开发环境性能优化：

- 优化打包构建速度：`HMR`（热模块替换）。

- 优化代码调试：`source-map`。

生产环境性能优化：

- 优化打包构建速度：`onOf`、`babel缓存`、`多进程打包`、`externals`、`dll`。

- 优化代码运行的性能：`缓存（hash、chunkhash、contenthash）`、`tree shanking`、`code split`、`懒加载`、`预加载`、`pwa`。

## HMR 热模块替换

`HMR`：hot module replacement 热模块替换 / 模块热替换。

作用：一个模块发生变化，只会重新打包这一个模块（而不是打包所有模块），极大提升构建速度。

- 样式文件：可以使用 `HMR` 功能，`style-loader` 内部已经实现了。

- js 文件：默认不能使用 `HMR` 功能，需要修改 js 代码，添加支持 `HMR` 功能的代码。  
**注意：HMR 功能对 js 的处理，只能处理非入口 js 文件的其他文件。**

- html 文件：默认不能使用 `HMR` 功能，同时会导致问题，html 文件不能热更新了。（不用做 HMR 功能）  
**解决：修改 entry 入口，将 html 文件引入。**

给 `webpack.config.js` 文件中的 `devServer` 添加新属性，修改 `entry`。

```js
module.exports = {
  ...
  devServer: {
    contentBase: resolve(__dirname, './dist'),
    compress: true,
    port: 3000,
    open: true,
    hot: true // 开启 HMR 功能
  },
  entry: ['./src/js/index.js', './src/index.html']
  ...
}
```

修改入口文件，实现文件单独热更新。

```js
import xxx from './xx.js'

if(module.hot) {
  // 一旦 module.hot 为 true，说明开启了 HMR 功能
  module.hot.accept('./xxx.js', function() {
    // accept 方法会监听 xxx.js 文件的变化，一旦发生变化，其他模块不会重新打包构建
    // 也可以设置回调函数
  })
}
```

## source-map

一种提供源代码到构建后代码映射技术。（如果构建后代码出错了，通过映射可以追踪源代码错误）

在 `webpack.config.js` 中添加 `devtool` 属性。

```js
module.exports = {
  ...
  devtool: 'source-map'
  ...
}
```

`devtool` 属性的值如下所示：

- `source-map`：外部。  
错误代码准确信息和源代码的错误位置。

- `inline-source-map`：内联。  
只生成一个内联 `source-map`。  
错误代码准确信息和源代码的错误位置。

- `hidden-source-map`：外部。  
错误代码错误原因，但是没有错误位置。  
不能追踪源代码错误，只能提示构建后代码的错误位置。

- `eval-source-map`：内联。  
每一个文件都生成对应的 `source-map`，都在 `eval`。  
错误代码准确信息和源代码的错误位置。

- `nosource-source-map`：外部。  
错误代码准确信息，但是没有任何源代码信息。

- `cheap-source-map`：外部。  
错误代码准确信息和源代码的错误位置。

- `cheap-module-source-map`：外部。  
错误代码准确信息和源代码的错误位置。  
module 会将 loader 的 source map 加入。

内部和外部的区别：外部生成 `bundle.js.map` 文件，内联没有，内联构建速度更快。

如何选择？

开发环境：

- 速度快：`eval > inline > cheap > ...`  
`eval-cheap-source-map`  
`eval-source-map`

- 调试更友好：`source-map`  
`cheap-module-source-map`  
`cheap-source-map`  
`eval-source-map / eval-cheap-module-source-map`

生产环境：源代码要不要隐藏？调试要不要更友好？内联会让代码体积变大，所以在生产环境不用内联。

- `nosource-source-map`：全部隐藏。

- `hidden-source-map`：只隐藏源代码，会提示构建后代码错误信息。

- `source-map / cheap-module-source-map`。
