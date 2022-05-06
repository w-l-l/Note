# webpack初体验

**初始化配置**

初始化 `package.json`。

```js
npm init
```

下载并安装 `webpack`。

```js
npm i webpack webpack-cli -g
npm i webpack webpack-cli -D
```

在 `webpack@3.x` 中，`webpack` 本身和它的 `cli` 是在同一个包中，但是在 `webpack@4.x` 已经将两者分开来更好的管理它们。

**编译打包应用**

开发环境打包：

```js
webpack src/js/index.js -o dist/js/bundle.js --mode=development
// 能够编译打包 js 和 json 文件，并且能将 ES6 的模块化语法（import）转换为浏览器能识别的语法
```

生产环境打包：

```js
webpack src/js/index.js -o dist/js/bundle.js --mode=production
// 在开发配置功能之上多一个压缩代码的功能
```

结论：

- 能打包 js 和 json 文件。

- 能将 ES6 的模块化语法（import）转换成浏览器识别的语法。

- 生产环境比开发环境多一个压缩代码。
