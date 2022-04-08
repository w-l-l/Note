# React脚手架

`react` 提供了一个用于创建 `react` 项目的脚手架库：`create-react-app`。

全局安装：

```js
npm i create-react-app -g
```

切换到想创建项目的目录，使用命令：

```js
npx create-react-app projectName
// ts 项目
npx create-react-app projectName --template typescript
```

进入项目文件夹：`cd projectName`。

启动项目：`npm start`。

## index.html 文件详解

```html
<link rel="icon" href="%PUBLIC_URL%/favicon.ico">
```

`%PUBLIC_URL%`：代表 public 文件夹的路径。

```html
<meta name="viewport" content="width=device-width,initial-scale=1">
```

开启理想视口，用于做移动端网页的适配。

```html
<meta name="theme-color" content="red">
```

用于配置浏览器页签 + 地址栏的颜色（仅支持安卓手机浏览器）。

```html
<link rel="apple-touch-icon" href="%PUBLIC_URL%/logo.png">
```

用于指定网页添加到手机主屏幕后的图标（仅限于苹果手机）。

```html
<link ref="manifest" href="%PUBLIC_URL%/manifest.json">
```

应用加壳时的配置文件。

```html
<noscript>You need to enable JavaScript to run this app.</noscript>
```

如果浏览器不支持 `js` 将展示 `noscript` 标签中的内容。

## 样式模块化

模块引入：

```js
import style from './index.module.css'

<div className={ style.color }></div>
```

`webpack` 配置：

```js
// 在 webpack.config.js 中添加 modules 参数，启用 css 的模块化
module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ['style-loader', {
          loader: 'css-loader',
          options: {
            modules: {
              localIdentName: '[path][name]-[local]-[hash:5]'
            }
          }
        }, 'sass-loader']
      }
    ]
  }
}
```

`localIdentName` 参数：

- `[path]`：表示样式表（相对于项目根目录）所在路径。

- `[name]`：表示样式表文件名称。

- `[local]`：表示样式表的类名定义名称。

- `[hash:length]`：表示 32 位的 hash 值。

还有一种写法：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [
          'style-loader',
          'css-loader?modules&localIdentName=[path]',
          'sass-loader'
        ]
      }
    ]
  }
}
```

但是新版本 `css-loader3.x` 不再支持这种方式的写法了。

**注意：一般我们使用的第三次 UI 组件，它们的样式表都是 .css 结尾的，所以我们最好不要为 .css 结尾的文件启用模块化。我们可以手写 scss 或 less 样式文件，只需要为 scss 或 less 文件启用模块化就行了。**

如果想要设置某个类不被模块化，可以为其设置 `:global` 方法来阻止其被模块化，使其全局生效。

```css
:global(.className) {
  color: red;
}
```

## 严格模式

需要进行严格模式检查的组件使用 `<React.StrictMode></React.StrictMode>` 标签包裹。

```html
<React.StrictMode>
  <App />
</React.StrictMode>
```

`React.StrictMode` 标签的作用：

- 识别具有不安全生命周期的组件。

- 有关旧式字符串 ref 用法的警告。

- 关于已弃用的 `React.findDOMNode` 用法的警告。

- 检查意外的副作用。

- 检测遗留 `context API`。

## 配置代理

方法一：在 `package.json` 中追加：`"proxy": "http://localhost:5000"`。

- 优点：配置简单，前端请求资源时可以不加任何前缀。

- 缺点：不能配置多个代理。

- 工作方式：上述方式配置代理，当请求当前服务器不存在的资源时，就会将请求转发给 5000 服务器（优先匹配前端资源）。

方法二：在 `src` 目录下创建配置文件：`src/setupProxy.js`。

```js
// setupProxy.js
const proxy = require('http-proxy-middleware')
module.exports = function(app) {
  app.use(
    proxy('/api1', {
      target: 'http://localhost:5000',
      changeOrigin: true,
      pathRewrite: {
        '^/api1': ''
      }
    }),
    proxy('/api2', {
      target: 'http://localhost:5001',
      changeOrigin: true,
      pathRewrite: {
        '^/api2': ''
      }
    })
  )
}
```

proxy 的第一个参数，表示需要转发的请求，所有带有第一个参数前缀的请求都会转发给对应的 `target`。

- `target`：配置转发目标地址，能返回数据的服务器地址。

- `changeOrigin`：默认值为 false，通常都会设置为 true。  
true：服务器收到请求头中 `host` 为当前 `target` 的配置地址。  
false：服务器收到请求头中 `host` 为代理前的地址。

- `pathRewrite`：去除请求前缀，保证交给后台服务器的是正常请求地址（必须配置）。

- 优点：可以配置多个代理，可以灵活的控制请求是否走代理。

- 缺点：配置繁琐，前端请求资源时必须加前缀。
