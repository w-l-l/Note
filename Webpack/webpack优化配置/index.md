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

## oneOf

`webpack` 原本的 `loader` 是将每个文件都过一遍。

> 比如有一个 js 文件，rules 中有 10 个 loader，第一个是处理 js 文件的 loader，当第一个 loader 处理完后，webpack 不会自动跳出，而是拿着这个 js 文件去尝试匹配剩下的 9 个 loader，相当于没有 break。  
而 oneOf 就相当于这个 break。

正常来讲，一个文件只能被一个 `loader` 处理。当一个文件要被多个 `loader` 处理，那么一定要指定 `loader` 执行的先后顺序。

修改 `webpack.config.js`。

```js
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        enforce: 'pre', // 优先执行
        loader: 'eslint-loader',
        options: {
          fix: true
        }
      },
      {
        oneOf: [ // 以下 loader 只会匹配一个，注意：不能有两个配置处理同一类型的文件
          { test: /\.css$/, use: ['style-loader', 'css-loader'] },
          { test: /\.html$/, loader: 'html-loader' },
          { test: /\.js$/, exclude: /node_modules/, loader: 'babel-loader' }
        ]
      }
    ]
  }
  ...
}
```

## 缓存

**babel 缓存**

- `cacheDirectory: true`。（第二次构建时，会读取之前的缓存）

**文件资源缓存**

- `hash`：每次 `webpack` 构建时会生成一个唯一的 `hash` 值。  
问题：因为 js 和 css 同时使用一个 hash 值，如果重新打包，会导致所有缓存失效，但我们只改动了一个文件。

- `chunkhash`：根据 `chunk` 生成的 `hash` 值，如果打包来源于同一个 `chunk`，那么 `hash` 值就一样。  
问题：js 和 css 的 `hash` 值还是一样的，因为 css 是在 js 中被引入的，所以同属于一个 `chunk。`

- `contenthash`：根据文件的内容生成 `hash` 值。  
不同文件的 `hash` 值是不一样的，让代码上线运行缓存更好使用。

**设置缓存实现**

```js
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'js/bundle.[contenthash:10].js',
    path: resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        options: {
          cacheDirectory: true // 开启 babel 缓存，第二次构建时，会读取之前的缓存
        }
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: 'css/common.[contenthash:10].css'
    })
  ]
}
```

## tree shaking 树摇

去除无用代码。

前提：必须使用 ES6 模块化，开启 `production` 环境。

作用：减少代码体积。

给 `package.json` 文件添加配置。

```json
{
  "sideEffects": false // 所有代码都没有副作用（都可以进行 tree shaking）
}
```

问题：可能会把 `css` 或 `@babel/polyfill`（副作用）文件干掉。

```json
{
  "sideEffects": ["*.css", "*.less", ".scss"] // 样式文件不进行 tree shaking
}
```

## code split 代码分离

**多入口**

```js
module.exports = {
  entry: {
    index: './src/js/index.js',
    test: './src/js/test.js'
  },
  output: {
    filename: 'js/[name].[contenthash:10].js',
    path: resolve(__dirname, 'dist')
  }
}
```

**node_modules 代码分离**

```js
module.exports = {
  ...
  optimization: {
    splitChunks: {
      chunks: 'all'
    }
  }
  ...
}
```

可以将 `node_modules` 中代码单独打包一个 `chunk` 最终输出。

自动分析入口 `chunk` 中，有没有公共的文件，如果有会打包成单独一个 `chunk`。

**通过 js 代码，让某个文件被单独打包成一个 chunk**

`import` 动态导入语法：能将某个文件单独打包。

```js
import(/* webpackChunkName: 'test' */ './test')
.then(result => {/* 成功 */})
.catch(error => {/* 失败 */})
```

- `webpackChunkName`：打包后的文件名字。

**code split 通常我们都设置一个入口文件，将 node_modules 单独打包，再结合实际，单独打包某些文件。**

## 懒加载和预加载

懒加载和预加载让文件单独打包才能实现。

正常加载可以认为是并行加载（同一时间加载多个文件）。

**懒加载**

当文件需要时才加载。

```js
btnElement.onclick = function() {
  import(/* webpackChunkName: 'test' */ './test').then(result => {...})
}
// 点击触发文件才会被加载，多次触发不会频繁加载，会直接从缓存中读取
```

**预加载**

会在使用之前，提前加载 js 文件。

```js
btnElement.onclick = function() {
  import(/* webpackChunkName: 'test', webpackPrefetch: true */, './test').then(result => {...})
}
// 等其他资源加载完毕，浏览器空闲了，在偷偷加载资源，后续也是直接从缓存中读取
```

**注意：预加载的兼容性不是很好。**

## PWA

`PWA`：渐进式网络开发应用程序（离线可访问）。

下载插件。

```js
npm i workbox-webpack-plugin -D
```

修改 `webpack.config.js` 文件。

```js
const WorkboxWebpackPlugin = require('workbox-webpack-plugin')

module.exports = {
  ...
  plugins: [
    // 生成一个 service-worker.js 配置文件
    new WorkboxWebpackPlugin.GenerateSW({
      clientsClaim: true, // 帮助 serviceworker 快速启动
      skipWaiting: true // 删除旧的 serviceworker
    })
  ]
  ...
}
```

在入口文件注册 serviceworker。

```js
if('serviceWorker' in navigator) {
  window.addEventListener('load', _ => {
    navigator.serviceWorker.register('./service-worker.js')
    .then(_ => {/* SW 注册成功 */})
    .catch(_ => {/* SW 注册失败 */})
  })
}
```

**注意：SW 代码必须运行在服务器上。**

在浏览器调试模式的 `Application` 下的 `Cache Storage` 可以查看离线缓存的文件信息。

## 多进程打包

安装插件。

```js
npm i thread-loader -D
```

跟相关文件开启多进程打包（一般是 js 文件）。

```js
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: [
          {
            loader: 'thread-loader', // 开启多进程打包，进程启动大概为 600ms，进程通信也有开销的，只有工作消耗时间比较长，才需要多进程打包
            options: {
              workers: 2 // 2 个进程
            }
          },
          { loader: 'babel-loader' }
        ]
      }
    ]
  }
  ...
}
```

## externals

当我们使用外部第三方文件的时候，可以禁止某些第三方文件被打包进去。（例如使用 `CDN`）

修改 `webpack.config.js`。

```js
module.exports = {
  ...
  externals: {
    jquery: 'jQuery' // 禁止 jquery 进行打包，该 jquery 代替了 npm 安装的 jquery 包
    /*
      属性名可以自定义，主要用于模块化的 js 中引入
      属性值为已经存在的 window 全局变量或者函数
    */
  }
  ...
}
```

```js
import $ from 'jquery'
// 引入的不是 npm 包的 jquery，而是全局对象的 jQuery，所以构建的时候 jquery 这个包不会进行打包
```

## dll

使用 `dll` 技术，对某些第三方库进行单独打包。

- 当你运行 `webpack` 时，默认查找 `webpack.config.js` 配置文件。

- 使用 `dll` 技术需要新创建一个配置文件 `webpack.dll.js`，并运行它。

```js
webpack --config webpack.dll.js
```

创建 `webpack.dll.js` 文件。

```js
const { resolve } = require('path')
const webpack = require('webpack')

module.exports = {
  entry: {
    jquery: ['jquery'], // 最终打包生成的 [name] --> jquery，['jquery'] 是要打包的库是 jquery
    moment: ['moment']
  },
  output: {
    filename: '[name].dll.js',
    path: resolve(__dirname, './dll'),
    library: '[name]_[hash]' // 打包的库里面向外暴露出去的内容叫什么名字，跟 webpack.DllPlugin 中的 name 保持一致
  },
  plugins: [
    new webpack.DllPlugin({ // 打包生成一个 manifest.json --> 提供和 jquery 映射
      context: __dirname,
      name: '[name]_[hash]', // 映射库暴露的内容名称
      path: resolve(__dirname, './dll/[name]-manifest.json') // 输出文件路径
    })
  ],
  mode: 'production'
}
```

安装插件。

```js
npm i add-asset-html-webpack-plugin -D
```

**注意：此插件需要结合 html-webpack-plugin 一起使用。**

修改 `webpack.config.js`。

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const webpack = require('webpack')
const AddAssetHtmlWebpackPlugin = require('add-asset-html-webpack-plugin')

module.exports = {
  ...
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
      filename: 'index.html'
    }),
    new webpack.DllReferencePlugin({ // 告诉 webpack 哪些库不参与打包，同时使用时的名称也得变
      context: __dirname,
      manifest: resolve(__dirname. 'dll/jquery-manifest.js')
    }),
    new webpack.DllReferencePlugin({
      context: __dirname,
      manifest: resolve(__dirname. 'dll/moment-manifest.js')
    }),
    new AddAssetHtmlWebpackPlugin([ // 将某个文件打包输出出去，并在 html 中自动引入该资源
      {
        filepath: resolve(__dirname, 'dll/jquery.dll.js')
      },
      {
        filepath: resolve(__dirname, 'dll/moment.dll.js')
      }
    ])
  ],
  mode: 'production'
  ...
}
```

总结：

- 先执行 `webpack --config webpack.dll.js` 生成 `manifest.json` 文件。

- 执行 `webpack` 进行打包。

- 如果没有给 `webpack.dll.js` 添加新的配置，只需执行 `webpack --config webpack.dll.js` 一次就行，不用每次打包前都执行。

- 只要给 `webpack.dll.js` 添加了新的配置，就必须重新执行一下 `webpack --config webpack.dll.js`，重新生成 `manifest.json`，重新打包。

## hard-source-webpack-plugin

为模块提供中间缓存步骤，第一次构建花费正常的时间，第二次构建将显著加快。

安装插件。

```js
npm i hard-source-webpack-plugin -D
```

在 `webpack.config.js` 中添加配置。

```js
const HardSourceWebpackPlugn = require('hard-source-webpack-plugin')

module.exports = {
  ...
  plugins: [ new HardSourceWebpackPlugn() ]
  ...
}
```

## babel-plugin-transform-remove-console

清除所有的 `console` 信息。

一般我们在生产环境才会清除所有的 `console` 信息，开发环境会使用 `console` 进行调试。

`babel` 配置文件。

```js
const proPlugins = []
if(process.env.NODE_ENV === 'production') proPlugins.push('transform-remove-console')

module.exports = {
  plugins: [...proPlugins]
}
```
