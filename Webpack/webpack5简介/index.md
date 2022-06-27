# webpack5简介

此版本重点关注以下内容。

- 通过持久缓存提高构建性能。

- 使用更好的算法和默认值来改善长期缓存。

- 通过更好的树摇和代码生成来改善捆绑包大小。

- 清除处于怪异状态的内部结构，同时在 v4 中实现功能而不引入任何重大更改。

- 通过引入重大更改来为将来的功能做准备，以使我们能够尽可能长时间地使用 v5。

## 默认值

```js
module.exports = {
  entry: './src/index.js',
  output: {
    path: resolve(__dirname, 'dist'),
    filename: '[name].js'
  }
}
```

## 特点

**自动删除 node.js polyfills**

> &emsp;&emsp;早期，`webpack` 的目标是允许在浏览器中运行大多数 `node.js` 模块，但是模块格局发生了变化，许多模块用途现在主要是为前端目的而编写的。  
&emsp;&emsp;`webpack` <= 4 附带了许多 `node.js` 核心模块的 `polyfill`，一旦模块使用任何核心模块（即 `crypto` 模块），这些模块就会自动应用。  
&emsp;&emsp;尽管这使使用 `node.js` 编写的模块变得容易，但它会将这些巨大的 `polyfill` 添加到包中，再许多情况下，这些 `polyfill` 是不必要的。  
&emsp;&emsp;`webpack5` 会自动停止填充这些核心模块，并专注于前端兼容的模块。

迁移：

- 尽可能尝试使用与前端兼容的模块。

- 可以为 `node.js` 核心模块手动添加一个 `polyfill`，错误信息将提示如何实现该目标。

****

**chunk 和模块 ID**

添加了用于长期缓存的新算法，再生产模式情况下启用这些功能。

```js
module.exports = {
  ...
  optimizeation: {
    chunkIds: 'deterministic',
    moduleIds: 'deterministic'
  }
  ...
}
```

****

**Chunk ID**

你可以不使用 `import(/* webpackChunkName: 'name' */ 'module')` 在开发环境为 `chunk` 命名，生产环境还是有必要的。

`webpack` 内部有 `chunk` 命名规则，不再是以 id（0,1,2）命名了。

****

**Tree Shaking**

1. 能够处理对嵌套模块的 `tree shaking`。

```js
// inner.js
export const a = 1
export const b = 2
```

```js
// module.js
import * as inner from './inner'
export {
  inner
}
```

```js
// user.js
import * as module from './module'
console.log(module.inner.a)
```

以上这种情况，在生产环境中，inner 模块暴露的 b 会被删除。

2. 更好的处理多个模块之间的关系。

```js
import { something } from './something'
function aa() {
  return something
}
export function bb() {
  return aa()
}
```

当设置了 `"sideEffects": false` 时，一旦发现 bb 方法没有使用，不但删除 bb，还会删除 './something'。

3. 能处理对 `Commonjs` 的 `tree shaking`。

****

**output**

`webpack4` 默认只能输出 ES5 代码。

`webpack5` 可以生成 ES5 和 ES6 代码。

```js
module.exports = {
  ...
  output: {
    ecmaVersion: 2015
  }
  ...
}
```

****

**splitChunk**

能够对不同文件分开设置。

```js
// webpack4
module.exports = {
  ...
  optimizeation: {
    splitChunk: {
      minSize: 30000
    }
  }
  ...
}
```

```js
// webpack5
module.export = {
  ...
  optimizeation: {
    splitChunk: {
      javascript: 30000,
      style: 50000
    }
  }
  ...
}
```

****

**caching**

```js
module.exports = {
  ...
  // 配置缓存
  cache: {
    // filesystem 磁盘存储，memory 内存存储
    type: 'filesystem',
    // 缓存文件存放的路径
    cacheDirectory: 'node_modules/.cache/webpack',
    // 额外的依赖文件，当这些文件内容发生变化时，缓存会完全失效执行完整的编译构建，通常可设置为项目配置文件
    buildDependencies: {
      config: [path.join(__dirname, 'webpack.dll_config.js')]
    },
    // 受控目录，webpack 构建时会跳过新旧代码哈希值与时间戳的对比，直接使用缓存副本
    managePaths: ['./node_modules'],
    // 是否输出缓存处理过程的详细日志
    profile: false,
    // 缓存失效时间
    maxAge: 5184000000
  }
  ...
}
```

****

**监视输出文件**

`webpack` <= 4：在第一次构建时输出全部文件，但是监视重新构建时会只更新修改的文件。

`webpack5`：在第一次构建时会找到输出文件看是否有变化，从而决定要不要输出全部文件。
