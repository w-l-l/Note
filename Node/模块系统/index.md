# 模块系统

## exports 和 module.exports 的区别

每个模块都有一个 `module` 对象，这个对象中有一个 `exports` 对象。

我们可以把需要导出的成员挂载到 `module.exports` 接口对象中。

```js
module.exports.propName = 'xxx'
```

但是每次都 `module.exports.propName = xxx` 很麻烦。

所以 node 为了方便，同时在每一个模块中都提供了一个成员叫 `exports`。

```js
module.exports === exports // true
```

所以对于 `module.exports.propName = xxx` 的方式完全可以使用 `exports.PropName = xxx`。

当一个模块需要导出单个成员的时候，这个时候必须使用 `module.exports = xxx` 的方式，`exports = xxx` 不管用。

因为每个模块最终向外暴露的是 `module.exports`。

而 `exports` 只是 `module.exports` 的一个引用。

所以即便 `exports = xxx` 重新赋值，也不会影响 `module.exports`。

**注意：有一种赋值方式比较特殊**：

- `exports = module.exports` 重新建立引用关系。

## require 详情

### require 优先从缓存加载

如果两个 js 文件相互 `require` 引用。都可以拿到其中的接口对象，但是不会重复执行里面的代码，这样做的目的是为了避免重复加载，提高模块加载效率。

### require 标识符分析

**路径形式的模块**：

- `./`：当前目录，不可省略。

- `../`：上一级目录，不可省略。

- `/xxx`：根目录，几乎不用。

- `d:/`：根目录，几乎不用。

首位的 `/` 在这里表示的是当前文件模块所属磁盘的根路径。

`.js` 后缀名的文件可以省略。

**核心模块**：

核心模块的本质也是文件。

核心模块的文件已经被编译到二进制文件中了（node.exe），我们只需要按照名字来引用就可以了（比如 `fs`、`http` 等）。

**第三方模块**：

凡是第三方模块都必须通过 `npm` 来下载。

使用的时候可以通过 `require('模块名')` 的方式来进行加载使用。

不可能有任何一个第三方包和核心模块的名字一样。

核心模块引用过程：

- 先找到当前文件所处目录中的 `node_modules` 目录。

- 在 `node_modules` 目录下找到对应的模块名文件夹。

- 在模块名文件夹中找到 `package.json` 文件。

- `package.json` 文件中的 `main` 属性，记录了该第三方模块的入口模块。  
然后加载使用这个第三方模块。  
实际最终加载的还是文件。

- 如果 `package.json` 文件不存在或者 `main` 属性指向的入口模块也没有。则 node 会自动找该目录下的 `index.js` 文件。也就是说，`index.js` 会作为一个默认备选项。

- 如果以上所有任何一个条件都不成立，则会进入上一级目录中的 `node_modules` 目录中查找。

- 如果上一级还没有，则继续往上上一级查找，直到找到当前磁盘根目录，如果还没找到，则直接报错。
