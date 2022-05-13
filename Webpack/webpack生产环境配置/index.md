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
