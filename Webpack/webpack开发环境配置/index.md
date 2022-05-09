# webpack开发环境配置

## 创建配置文件

在项目根目录创建 `webpack.config.js`，配置如下：

```js
const { resolve } = require('path')

module.exports = {
  entry: './src/js/index.js', // 入口文件
  output: { // 输出配置
    filename: 'bundle.js', // 输出文件名
    path: resolve(__dirname, 'dist') // 输出文件路径
  },
  mode: 'development' // 开发环境
}
```

直接运行 `webpack` 命令就能进行打包了。

当我们在控制台，直接输入 `webpack` 命令执行的时候，`webpack` 做了以下几步：

- 首先，`webpack` 发现，我们并没有通过命令的形式，给它指定入口和出口。

- `webpack` 就会去项目的根目录，查找 `webpack.config.js` 的配置文件。

- 找到配置文件后，`webpack` 会去解析执行这个配置文件，导出配置对象。

- 拿到配置对象指定的入口和出口，然后进行打包构建。

所有的构建工具都是基于 `node.js` 平台运行的，模块化采用 `Commonjs`。

## 打包样式资源

下载安装 `loader` 包。

```js
npm i css-loader style-loader less-loader less sass-loader node-sass -D
```

在 `webpack.config.js` 文件中添加配置规则。

```js
module.exports = {
  ...
  module: {
    rules: [ // 详细的 loader 配置
      { test: /\.css$/, use: ['style-loader', 'css-loader'] },
      { test: /\.less/, use: ['style-loader', 'css-loader', 'less-loader'] },
      { test: /\.scss/, use: ['style-loader', 'css-loader', 'sass-loader'] }
    ]
  }
  ...
}
```

- `test`：匹配哪些文件。

- `use`：使用哪些 `loader` 进行处理。**use 数组 loader 的执行顺序是从后往前依次执行。**

例如：

- `less-loader`：将 `less` 文件编译成 `css` 文件。

- `css-loader`：将 `css` 文件变成 `Commonjs` 模块加载到 js 中，里面的内容是样式字符串。

- `style-loader`：创建 `style` 标签，将 js 中的样式资源插入，并添加到 `head` 中生效。

## 打包 html 资源

下载安装 `plugin` 包。

```js
npm i html-webpack-plugin -D
```

`html-webpack-plugin` 的两个作用：

- 自动在内存中根据指定页面生成一个内存的页面。

- 自动把打包好的 `bundle.js` 追加到页面中去。

修改 `webpack.config.js` 文件。

```js
const { resolve } = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  ...
  plugins: [
    new HtmlWebpackPlugin({ // 创建一个在内存中生成 html 页面的插件
      template: resolve(__dirname, './src/index.html'), // 模板路径，去生成内存中的页面
      filename: 'index.html' // 自动生成的 html 文件的名称（生成的 html 文件在项目根目录下）
    })
  ]
  ...
}
```

在 `index.html` 中的 `script` 标签可以注释掉，因为 `html-webpack-plugin` 插件会自动把 `bundle.js` 注入到 `index.html` 页面中，不需要我们手动处理 js 的引用路径，插件会帮我们自动创建一个合适的 `script`，并且引入正确的路径。

**注意：此时页面中显示的 index.html 文件不是 src 目录下的 index.html，而是内存中的 index.html。**

## 打包图片资源

下载 `loader` 包。

```js
npm i html-loader url-loader file-loader -D
```

在 `webpack.config.js` 中配置规则。

```js
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.(jpg|png|gif)$/,
        loader: 'url-loader',
        options: {
          limit: 8 * 1024,
          // esModule: false,
          name: '[hash:8]-[name].[ext]'
        }
      },
      { test: /\.html$/, loader: 'html-loader' } // 处理 html 文件中的 img 图片，负责引入 img，从而能被 url-loader 进行处理
    ]
  }
  ...
}
```

`limit`：单位字节（byte），图片小于等于设置的值就会被 `base64` 处理。

- 优点：减少请求数量（减轻服务器压力）。

- 缺点：图片体积会更大（文件请求速度更慢）。

`esModule`：关闭 `url-loader` 的 ES6 模块化，使用 `Commonjs` 解析。

- 因为 `url-loader` 默认使用的是 ES6 模块化解析，而 `html-loader` 引入图片使用的是 `Commonjs` 解析。

- 解析出现的问题：`src=[object Module]`。

- 所以需要关闭 `url-loader` 的 ES6 模块化。

`name`：给图片进行重命名。

****

父组件给子组件传图片路径参数，在父组件中 `src` 的值会被当作是普通字符串传给子组件，所以子组件只会当字符串处理，不会当路径处理。

解决方法：结合 `require` 使用。

```html
<com :src="require('../xxx.png')"></com>
```

**注意：require() 只能接收常量。**
