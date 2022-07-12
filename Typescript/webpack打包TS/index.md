# webpack打包TS

通常情况下，实际开发中我们都需要使用构建工具对代码进行打包，TS 同样也可以结合构建工具一起使用，下面以 `webpack` 为例介绍一下如何结合构建工具使用 TS。


## 步骤

初始化项目：进入项目根目录，执行命令 `npm init -y`，创建 `package.json` 文件。

下载构建工具：

```js
npm i -D webpack webpack-cli webpack-dev-server typescript ts-loader html-webpack-plugin clean-webpack-plugin
```

共安装 7 个包：

- `webpack`：构建工具。

- `webpack-cli`：`webpack` 的命令工具。

- `webpack-dev-server`：`webpack` 的开发服务器。

- `typescript`：TS 编译器。

- `ts-loader`：TS 加载器，用于在 `webpack` 中编译 TS 文件。

- `html-webpack-plugin`：`webpack` 中的 `html` 插件，用来自动创建 `html` 文件。

- `clean-webpack-plugin`：`webpack` 中的清除插件，每次构建都会先清除上次构建的目录。

根目录创建 `webpack.config.js`。

```js
// webpack.config.js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

module.exports = {
  optimization: {
    minimize: false // 关闭代码压缩，可选
  },
  entry: './src/index.ts',
  devtool: 'inline-source-map',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
    environment: {
      arrowFunction: false // 关闭 webpack 的箭头函数，可选
    }
  },
  resolve: {
    extensions: ['.ts', '.js']
  },
  module: {
    rules: [
      {
        test: /\.ts$/,
        use: {
          loader: 'ts-loader'
        },
        exclude: /node_modules/
      }
    ]
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      title: 'html文件的title',
      template: './src/index.html'
    })
  ]
}
```

根目录创建 `tsconfig.json`，配置可以根据自己需要。

```json
{
  "compilerOptions": {
    "target": "ES2015",
    "module": "ES2015",
    "strict": true
  }
}
```

修改 `package.json` 添加如下配置。

```json
{
  // ...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --mode=production",
    "start": "webpack serve --open --mode=development"
  }
  // ...
}
```

在 src 目录下创建 TS 文件，并在命令行执行 `npm run build` 对代码进行编译，或者执行 `npm start` 来启动开发服务器。

## Babel 结合 TS

经过一系列的配置，使得 TS 和 `webpack` 已经结合到了一起，除了 `webpack`，开发中还经常需要结合 `Babel` 来对代码进行转换，以使其可以兼容到更多的浏览器，在上述步骤的基础上，通过以下步骤再将 `babel` 引入到项目中。

安装依赖包：

```js
npm i -D @babel/core @babel/preset-env babel-loader core-js
```

共安装了 4 个包：

- `@babel/core`：`babel` 的核心工具。

- `@babel/preset-env`：`babel` 的预定义环境。

- `babel-loader`：`babel` 在 `webpack` 中的加载器。

- `core-js`：使老版本的浏览器支持新版 ES 语法。

修改 `webpack.config.js`。

```js
// webpack.config.js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.ts$/,
        use: [
          {
            loader: 'babel-loader',
            options: {
              presets: [
                [
                  '@babel/preset-env',
                  {
                    targets: {
                      chrome: '58',
                      ie: '11'
                    },
                    corejs: '3',
                    useBuiltIns: 'usage'
                  }
                ]
              ]
            }
          },
          {
            loader: 'ts-loader'
          }
        ],
        exclude: /node_modules/
      }
    ]
  }
  // ...
}
```

使用 TS 编译后的文件将会再次被 `babel` 处理，使得代码可以在大部分浏览器中直接使用，可以在配置选项的 `targets` 中指定要兼容的浏览器版本。
