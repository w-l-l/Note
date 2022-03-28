# antd按需加载和自定义主题

## 旧版 antd 按需加载和自定义主题

**安装依赖**

```js
npm i react-app-rewired customize-cra babel-plugin-import less less-loader
```

- `react-app-rewired`：脚手架配置被修改用它启动。

- `customize-cra`：执行 `config-overrides.js` 文件，修改脚手架配置。

- `babel-plugin-import`：按需加载需要。

设置主题的时候控制台可能会报错：`this.getOptions is not a function`。

这是因为 `less-loader` 版本太高了，需要降低版本，版本太低也会报错：`.bezierEasingMixin()`。

这里我们安装了 `less-loader@7.1.0` 的版本。

**修改package.json**

```json
"scripts": {
  "start": "react-app-rewired start",
  "build": "react-app-rewired build",
  "test": "react-app-rewired test",
  "eject": "react-scripts eject"
}
```

**根目录创建 config-overrides.js**

```js
const { override, fixBabelImports, addLessLoader } = require('customize-cra')

module.exports = override(
  fixBabelImports('import', {
    libraryName: 'antd',
    libraryDirectory: 'es',
    // style: 'css',
    style: true
  }),
  // 安装最新版的 less-loader 需要这样配置，官网还是老的 less-loader 配置
  addLessLoader({
    lessOptions: {
      javascriptEnabled: true,
      modifyVars: {
        '@primary-color': 'green'
      }
    }
  })
)
```

备注：不用在组件里亲自引入样式了，删除 `import 'antd/dist/antd.css'`。
