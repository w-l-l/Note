# AMD

1. 下载 require.js，并引入。

2. 创建项目结构。

```js
|--js
  |--libs
    |--require.js
    |--jquery-1.10.1.js
    |--angular.js
  |--modules
    |--alerter.js
    |--dataService.js
  |--main.js
|--index.html
```

3. 定义 require.js 的模块代码：

```js
// dataService.js
define(function() {
  let msg = 'xxx'
  function getMsg() {
    return msg.toUpperCase()
  }
  return {
    getMsg
  }
})

// alerter.js
define(['dataService', 'jquery'], function(dataService, $) {
  let name = 'alert'
  function showMsg() {
    $('body').css('background', 'gray')
    alert(dataService.getMsg() + ',' + name)
  }
  return {
    showMsg
  }
})
```

4. 应用主入口 main.js：

```js
(function() {
  // 配置
  require.config({
    // 基本路径
    baseUrl: 'js/',
    // 映射 --> 模块标识名: 路径
    paths: {
      // 自定义模块
      alerter: 'modules/alerter',
      dataService: 'modules/dataService',
      // 库模块
      jquery: 'libs/jquery-1.10.1',
      angular: 'libs/angular'
    }
    // 配置不兼容 AMD 的模块
    shim: {
      angular: {
        exports: 'angular'
      }
    }
  })

  // 引用模块使用
  require(['alerter', 'angular'], function(alerter, angular) {
    alerter.showMsg()
    console.log(angular)
  })
})()
```

5. 页面使用模块：

```html
<!-- data-main="js/main.js" -->
<script data-main="js/main.js" src="js/libs/require.js"></script>
```
