# Loader 原理

帮助 `webpack` 将不同类型的文件转换为 `webpack` 可识别的模块。

## Loader 执行顺序

### 分类

- `pre`：前置 loader。

- `normal`：普通 loader。

- `inline`：内联 loader。

- `post`：后置 loader。

### 执行顺序

- 4 类 loader 的执行优先级为：`pre > normal > inline > post`。

- 相同优先级的 loader 执行顺序为：`从右到左，从下到上`。

```js
// 执行顺序是：loader3 -> loader2 -> loader1
module: {
  rules: [
    { test: /\.js$/, loader: 'loader1' },
    { test: /\.js$/, loader: 'loader2' },
    { test: /\.js$/, loader: 'loader3' },
  ]
}
```

```js
// 此时的执行顺序是：loader1 -> loader2 -> loader3
module: {
  rules: [
    { test: /\.js$/, loader: 'loader1', enforce: 'pre' },
    { test: /\.js$/, loader: 'loader2' }, // 没有 enforce 就是 normal
    { test: /\.js$/, loader: 'loader3', enforce: 'post' },
  ]
}
```

### 使用 loader 的方式

- 配置方式：在 `webpack.config.js` 文件中指定 loader。（pre、normal、post）

- 内联方式：在每个 `import` 语句中显式指定 loader。（inline）

### inline loader

用法：

```js
import Styles from 'style-loader!css-loader?modules!./styles.css'
```

含义：

- 使用 `css-loader` 和 `style-loader` 处理 `styles.css` 文件。

- 通过 `!` 将资源中的 loader 分开。

`inline loader` 可以通过添加不同前缀，跳过其他类型 loader。

```js
// ! 跳过 normal loader
import Styles from '!style-loader!css-loader?modules!./styles.css'
```

```js
// -! 跳过 pre 和 normal loader
import Styles from '-!style-loader!css-loader?modules!./styles.css'
```

```js
// !! 跳过 pre、normal 和 post loader
import Styles from '!!style-loader!css-loader?modules!./styles.css'
```

## 开发一个 Loader

```js
// 它接收要处理的源码作为参数，输出转换后的 js 代码。
module.exports = function loader(content) {
  console.log('hello loader')
  return content
}
```

loader 接收的参数：

- `content`：源文件的内容。

- `map`：SourceMap 数据。

- `meta`：数据，可以是任何内容。
