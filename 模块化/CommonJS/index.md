# CommonJS

CommonJS 的实现：

- 服务器端：Node.js。

- 浏览器端：Browserify 或者 webpack。

基本语法：

- 定义暴露模块：`exports`。

```js
module.exports = {
  ...
}

exports.xxx = value
```

- 引入模块：`require`。

```js
const module = require(/* 模块名(第三方) / 模块相对路径(自定义) */)
```

引入模块发生在什么时候？

- Node：运行时，动态同步引入。

- Browserify / webpack：在运行前对模块进行编译打包处理（已经将依赖的模块包含进来了），运行的是打包生成的 js，运行时不需要再从远程引入依赖模块，只引入打包生成的 js 文件。
