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

## package 详情

包（package）：将多个模块组合为一个完整的功能，就是一个包。

包的主要结构：

- `bin`：二进制的可执行文件，一般都是一些工具包中才有。

- `lib`：js 文件。

- `doc`：文档。

- `test`：测试代码。

- `package.json`：包的描述文件。

**package.json：**

它是一个 json 格式的文件，在它里面保存了包各种相关的信息。

- `name`：包名。

- `version`：版本。

- `dependencies`：依赖。

- `main`：包的主要文件。

- `bin`：可执行文件。

**package-lock.json：**

`npm5` 以前是没有 `package-lock.json` 这个文件的，npm5 之后才加入这个文件的。

当你安装包的时候，npm 都会生成或者更新 package-lock.json 这个文件。

- npm5 以后的版本安装包不需要加 `--save` 参数，它会自动保存依赖信息。

- 当你安装包的时候，会自动创建或更新 package-lock.json 这个文件。

- package-lock.json 这个文件会保存 `node_modules` 中所有包的信息（版本、下载地址等）。  
这样重新 `npm install` 的时候速度就可以得到提升。

- 从文件名看，有一个 `lock` 称之为锁，这个 lock 代表是用来锁定版本的。

我们按照包的时候，版本号前面都有一个 `^` 符号。

```json
"dependencies": {
 "@types/node": "^8.0.33",
}
```

这个符号代表的**向后（新）兼容依赖**，如果 `@types/node` 的版本超过 8.0.33，重新 npm install 的时候就会安装最新的包，也就是实际安装的包的版本可能是 8.0.35。

package-lock.json 这个文件就是来解决这个问题的。锁定版本号，防止自动升级。

**node 搜索包的流程：**

1. 通过 npm 下载的包都放到 `node_modules` 文件夹中，我们通过 npm 下载的包，直接通过包名引入即可。

2. node 在使用模块名字来引入模块时，它会首先在当前目录的 node_modules 中寻找是否含有该模块。如果有则直接使用，没有则去上一级目录中的 node_modules 中寻找。如果有则直接使用，没有则再去上一级目录中的 node_modules 中寻找，直到找到为止。如果直到找到磁盘根目录还是没有，则报错。

`npm-shrinkwrap.json`：可以用来指定第三方包的子依赖版本，通过 `npm shrinkwrap` 命令生成 `npm-shrinkwrap.json` 文件。
