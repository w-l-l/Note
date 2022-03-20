# React脚手架

`react` 提供了一个用于创建 `react` 项目的脚手架库：`create-react-app`。

全局安装：

```js
npm i create-react-app -g
```

切换到想创建项目的目录，使用命令：`create-react-app projectName`。

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
