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
