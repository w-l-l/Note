# Vue-Cli项目优化

默认情况下，`vue-cli` 生成的项目，隐藏了 `webpack` 的配置项，如果我们需要配置 `webpack`，需要在项目根目录中创建 `vue.config.js` 文件来进行配置。

## 为开发环境和生产环境指定不同的打包入口

默认情况下，`vue-cli` 项目的开发环境和生产环境，共用同一个打包的入口文件（一般是 `src/main.js`）。

为了将项目的开发过程与生产过程分离，我们可以为两种模式指定不同的入口文件。

- 开发环境：`src/main-dev.js`。

- 生产环境：`src/main-pro.js`。

在 `vue.config.js` 导出的配置对象中，新增了 `configureWebpack` 或 `chainWebpack`，来定义 `webpack` 的打包配置。

两者的作用相同，唯一的区别就是它们修改 `webpack` 配置的方式不同。

- `chainWbpack`：链式编程方式。

- `configureWebpack`：操作对象方式。

通过 `chainWebpack` 自定义打包入口。

```js
// vue.config.js 配置
module.exports = {
  chainWebpack: config => {
    // 生产环境
    config.when(process.env.NODE_ENV === 'production', confog => {
      config.entry('app').clear().add('./src/main-pro.js')
    })
    // 开发环境
    config.when(process.env.NODE_ENV === 'development', config => {
      config.entry('app').clear().add('./src/main-dev.js')
    })
  }
}
```

- `entry`：找到默认的打包入口。

- `clear`：删除默认的打包入口。

- `add`：添加新的打包入口。

## 通过 externals 加载外部 CDN 资源

默认情况下，通过 `import` 语法导入的第三方依赖包，最终会被打包合并到同一个文件中，从而导致打包成功后，单文件体积过大。

可以通过 `webpack` 的 `externals` 节点，来配置并加载外部的 `CDN` 资源，凡是声明在 `externals` 中的第三方依赖，都不会进行打包。

```js
// vue.config.js 配置
module.exports = {
  chainWebpack: config => {
    config.when(process.env.NODE_ENV === 'production', config => {
      config.entry('app').clear().add('./scr/main-pro.js')
      config.set('externals', {
        vue: 'Vue',
        'vue-router': 'VueRouter',
        vuex: 'Vuex',
        axios: 'axios'
        ...
      })
    })
  }
}
```

最后在 `index.html` 文件的头部，添加相关的 `CDN` 资源引用。
