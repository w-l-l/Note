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

## CommonJS_Node

node 环境默认使用的就是 CommonJS 模块化。

1. 下载安装 node.js。

2. 创建项目结构。

```js
|-modules
  |-module1.js
  |-module2.js
  |-module3.js
|-app.js
|-package.json
  // {
  //   "name": "CommonJS_node",
  //   "version": "1.0.0"
  // }
```

3. 下载第三方模块。

```js
npm install uniq --save
```

4. 模块化编码

```js
// module1.js
module.exports = {
  foo() { ... }
}

// module2.js
module.exports = function() { ... }

// module3.js
exports.foo = function() { ... }

// app.js
// 引入模块
const module1 = require('./modules/module1')
const module2 = require('./modules/module2')
const module3 = require('./modules/module3')
const uniq = require('uniq')
// 使用模块
module1.foo()
module2()
module3.foo()
uniq()
```

5. 通过 node 运行 app.js。

```js
node app.js
```
