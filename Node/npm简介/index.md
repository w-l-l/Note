# npm简介

`npm(Node Package Manager)` 是 `node` 的包管理器。

通过 npm 可以对 node 中的包进行上传、下载、搜索等操作。

npm 会在安装完 node 以后自动安装。

npm 的常用指令。

- `npm -v`：查看 npm 版本。

- `npm version`：查看所有模块的版本。

- `npm init`：初始化项目，创建 package.json 文件。

- `npm i/install 包名`：安装指定的包。

- `npm i/install 包名 --save/-S`：安装指定的包并添加依赖。

- `npm i/install 包名 --save-dev/-D`：安装指定的包并添加到开发依赖。

- `npm i/install 包名 -g`：全局安装，通常安装工具的时候使用。

- `npm i/install`：安装当前项目所依赖的包。

- `npm s/search 包名`：搜索包。

- `npm r/remove 包名`：删除一个包。

- `npm uninstall -g 包名`：删除全局安装的包。

- `npm view 包名 versions`：查看当前包的所有版本号。

使用 npm 下载包是从 npm 官网下载对应的包，该网站在国外，经常会出现下载缓慢或出现异常，这个时候我们就可以使用 `cnpm`。

cnpm 是淘宝团队把 npm 官网的插件都同步到了中国的服务器，提供给我们从这个服务器上稳定下载资源。

将 npm 命令换成 cnpm 命令就行了。

```js
// cnpm 设置
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
