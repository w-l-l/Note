# CMD

1. 下载 sea.js，并引入。

2. 创建项目结构。

```js
|--js
  |--libs
    |--sea.js
  |--modules
    |--module1.js
    |--module2.js
    |--module3.js
    |--module4.js
    |--main.js
|--index.html
```

3. 定义 sea.js 的模块代码：

```js
// module1.js
define(function(require, exports, module) {
  // 内部变量数据
  let data = 'xxx'
  // 内部函数
  function show() {
    console.log('module1 show()' + data)
  }
  // 向外暴露
  exports.show = show
})

// module2.js
define(function(require, exports, module) {
  module.exports = {
    msg: 'xxx'
  }
})

// module3.js
define(function(require, exports, module) {
  const API_KEY = 'xxx'
  exports.API_KEY = API_KEY
})

// module4.js
define(function(require, exports, module) {
  // 引入依赖模块（同步）
  let module2 = require('./module2')
  function show() {
    console.log('module4 show()' + module2.msg)
  }
  exports.show = show
  // 引入依赖模块（异步）
  require.async('./module3', function(module3) {
    console.log('异步引入依赖模块 module3' + module3.API_KEY)
  })
})
```

4. 应用主入口 main.js：

```js
define(function(require) {
  let module1 = require('./module1')
  let module4 = require('./module4')
  m1.show()
  m4.show()
})
```

5.页面使用模块：

使用 sea.js：

- 引入 sea.js 库。

- 如何定义导出模块：`define()`、`exports`、`module.exports`。

- 如何依赖模块：`require()`。

- 如何使用模块：`seajs.use()`。

```html
<script type="text/javascript" src="js/libs/sea.js"></script>
<script type="text/javascript">
  seajs.use('./js/modules/main.js')
</script>
```
