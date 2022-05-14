# webpack生产环境配置

## 提取 css 成单独文件

下载插件。

```js
npm i mini-css-extract-plugin -D
```

修改 `webpack.config.js`。

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin')

module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader, // 取代 styl-loader，提取 js 中的 css 成单独文件
            options: {
              publicPath: '../' // 解决 css 引用文件路径问题，在每个引用路径前面加上../
            }
          },
          'css-loader'
        ]
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: 'css/[name].css', // 对输出的 css 文件进行重命名
      chunkFilename: 'css/[id].css'
    })
  ]
  ...
}
```

## css 兼容性处理

下载插件。

```js
npm i postcss-loader postcss-preset-env -D
```

修改 `webpack.config.js`。

```js
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'postcss-loader']
      }
    ]
  }
  ...
}
```

创建 `postcss.config.js`。

```js
module.exports = {
  plugins: [require('postcss-preset-env')]
}
```

修改 `package.json`。

```json
{
  "browserslist": {
    "development": [ // 开发环境
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ],
    "production": [ // 生产环境
      ">0.2%",
      "not dead",
      "not op_mini all"
    ]
  }
}
```

## 压缩 css

下载插件。

```js
npm i optimize-css-assets-webpack-plugin -D
```

修改 `webpack.config.js`。

```js
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')

module.exports = {
  ...
  plugins: [
    new OptimizeCssAssetsWebpackPlugin() // 压缩 css 成一行
  ]
  ...
}
```

## eslint 语法检查

下载插件。

```js
npm i eslint eslint-loader eslint-config-airbnb-base eslint-plugin-import -D
```

修改 `webpack.config.js`。

```js
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'eslint-loader',
        options: {
          fix: true // 自动修复 eslint 的错误
        },
        enforce: 'pre' // 优先执行这条规则
      }
    ]
  }
  ...
}
```

配置 `package.json`。

```json
{
  "eslintConfig": { // 设置检查规则
    "extends": "airbnb-base",
    "env": { // eslint 不认识 window、navigator 全局变量
      "browser": true // 支持浏览器端的全局变量
    }
  }
}
```

**注意：eslint-loader 规则必须要先于 babel-loader 规则。**

## js 兼容性处理

下载插件。

```js
npm i babel-loader @babel/core @babel/preset-env @babel/polyfill core-js -D
```

js 兼容性处理的关键包：`babel-loader`、`@babel/core`。

基本 js 兼容性处理：`@babel/preset-env`。

- 只能转换基本语法，如 `Promise` 高级语法不能转换。

全部 js 兼容性处理：`@babel/polyfill`。

```js
// 只需解决部分兼容性问题，但是将所有兼容性代码全部引入，体积太大
import '@babel/polyfill'
```

**按需加载**

```js
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        options: {
          presets: [
            [
              '@babel/perset-env',
              {
                useBuiltIns: 'usage', // 按需加载
                corejs: { version: 3 }, // 指定 core-js 版本
                targets: { // 指定兼容性做到哪个版本浏览器
                  chrome: '60',
                  firefox: '60',
                  ie: '9',
                  safari: '10',
                  edge: '17'
                }
              }
            ]
          ]
        }
      }
    ]
  }
  ...
}
```
