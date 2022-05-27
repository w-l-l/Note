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

## resolve

```js
module.exports = {
  // 解析模块规则
  resolve: {
    alias: { // 配置解析模块路径名：优点-简写路径，缺点-没有提示
      css: resolve(__dirname, 'src/css'),
      // import 'css/index.css' 等价于 import './src/css/index.css'
      vue$: 'vue/dist/vue.js'
      // $ 符号只命中关键字结尾的导入语句 import Vue from 'vue' 等价于 import Vue from 'vue/dist/vue.js'
    },
    extensions: ['.js', '.json', '.jsx', '.css'],
    // 配置省略文件路径的后缀名，从左到右进行匹配，尽量不要重名 import './index' 等价于 import './index.js'
    modules: [resolve(__dirname, '../../node_modules'), 'node_modules']
    // 告诉 webpack 解析模块是去哪个目录，而不是一层一层向外找
  }
}
```

## devServer

```js
module.exports = {
  devServer: {
    contentBase: resolve(__dirname, 'dist'), // 运行代码的目录
    watchContentBase: true, // 监视 contentBase 目录下的所有文件，一旦文件变化就会 reload
    watchOpyions: { // 监视配置
      ignored: /node_modules/ // 忽略文件，不监视 node_modules 目录
    },
    compress: true, // 启动 gzip 压缩
    port: 5000, // 端口号
    host: 'localhost', // 域名
    open: true, // 自动打开浏览器
    hot: true, // 开启 HMR 功能
    clientLogLevel: 'none', // 不要显示启动服务器日志信息
    quiet: true, // 除了一些基本启动信息以外，其他内容都不要显示
    overlay: false, // 如果报错，不要全屏提示
    proxy: { // 服务器代理 --> 解决开发环境跨域问题
      '/api': { // 一旦 devServer(5000) 服务器接收到 /api/xxx 的请求，就会把请求转发到另一个服务器(3000)
        target: 'http://localhost:3000',
        changeOrigin: true,
        pathRewrite: { // 向 3000 服务器发送请求时，请求路径重写：将 /api/xxx --> /xxx（去掉 /api）
          '^/api': ''
        }
      }
    }
  }
}
```

## optimization

安装插件。

```js
npm i terser-webpack-plugin -D
```

```js
const TerserWebpackPlugin = require('terser-webpack-plugin')

module.exports = {
  ...
  splitChunks: {
    chunks: 'all',
    /*--------------此区域为默认值，可以不写--------------------*/
    minSize: 30 * 1024, // 分割的 chunk 最小为 30kb
    maxSize: 0, // 最大没有限制
    minChunks: 1, // 要提取的 chunk 最小被引用一次
    maxAsyncRequests: 5, // 按需加载时并行加载文件的最大数量
    maxInitialRequests: 3, // 入口 js 文件最大并行请求数量
    automaticNameDelimiter: '~', // 名称连接符
    name: true, // 可以使用命名规则
    cacheGroups: {
      /*
        分割 chunk 的组
        node_modules 文件会被打包到 vendors 组的 chunk 中 --> vendors~xxx.js
        满足上面的公共规则，如：大小超过 30kb，至少被引用一次
      */
      vendors: {
        test: /[\\/]node_modules[\\/]/,
        priority: -10 // 优先级
      },
      default: {
        minChunks: 2, // 要提取的 chunk 最少被引用 2 次
        priority: -20, // 优先级
        reuseExistingChunk: true // 如果当前要打包的模块，和之前已经被提取的模块是同一个，就会复用，而不是重新打包模块
      }
    }
    /*------------------------------------------------------*/
  },
  runtimeChunk: {
    /*
      将当前模块的记录其他模块的 hash 单独打包为一个文件 runtime
      解决：修改 a 文件导致 b 文件的 contenthash 变化
    */
   name: entrypoint => `runtime-${entrypoint.name}`
  },
  minimizer: [
    // 配置生产环境的压缩方案：js 和 css
    new TerserWebpackPlugin({
      cache: true, // 开启缓存
      parallel: true, // 开启多进程打包（parallel: 4，开启 4 个进程进行打包）
      sourceMap: true // 启动 source-map
    })
  ]
  ...
}
```
