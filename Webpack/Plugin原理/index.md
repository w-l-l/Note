# Plugin 原理

作用：通过插件我们可以扩展 `webpack`，加入自定义的构建行为，使 `webpack` 可以执行更广泛的任务，拥有更强的构建能力。

工作原理：`webpack` 就像一条生产线，要经过一系列处理流程后才能将源文件转换成输出结果。这条生产线上的每个处理流程的职责都是单一的，多个流程之间有存在依赖关系，只有完成当前处理后才能交给下一个流程去处理。插件就像是一个插入到生产线中的一个功能，在特定的时机对生产线上的资源做处理。`webpack` 通过 `Tapable` 来组织这条复杂的生产线。`webpack` 在运行过程中会广播事件，插件只需要监听它所关心的事件，就能加入到这条生产线中，去改变生产线的运作。`webpack` 的事件流机制保证了插件的有序性，使得整个系统扩展性很好。

站在代码逻辑的角度就是：`webpack` 在编译代码过程中，会触发一系列 `Tapable` 钩子事件，插件所做的，就是找到相应的钩子，往上面挂自己的任务，也就是注册事件，这样，当 `webpack` 构建的时候，插件注册的事件就会随着钩子的触发而执行了。

## Webpack 内部的钩子

钩子的本质就是：事件。为了方便我们直接介入和控制编译过程，`webpack` 把编译过程中触发的各类关键事件封装成事件接口暴露了出来。这些接口被很形象地称做：`hooks`（钩子）。

### Tapable

`Tapable` 为 `webpack` 提供了统一的插件接口（钩子）类型定义，它是 `webpack` 的核心功能库。`webpack` 中目前有 10 种 `hooks`，在 `Tapable` 源码中可以看到，它们是：

```js
// https://github.com/webpack/tapable/blob/master/lib/index.js
exports.SyncHook = require("./SyncHook");
exports.SyncBailHook = require("./SyncBailHook");
exports.SyncWaterfallHook = require("./SyncWaterfallHook");
exports.SyncLoopHook = require("./SyncLoopHook");
exports.AsyncParallelHook = require("./AsyncParallelHook");
exports.AsyncParallelBailHook = require("./AsyncParallelBailHook");
exports.AsyncSeriesHook = require("./AsyncSeriesHook");
exports.AsyncSeriesBailHook = require("./AsyncSeriesBailHook");
exports.AsyncSeriesLoopHook = require("./AsyncSeriesLoopHook");
exports.AsyncSeriesWaterfallHook = require("./AsyncSeriesWaterfallHook");
exports.HookMap = require("./HookMap");
exports.MultiHook = require("./MultiHook");
```

`Tapable` 还统一暴露了三个方法给插件，用于注入不同类型的自定义构建行为：

- `tap`：可以注册同步钩子和异步钩子。

- `tapAsync`：回调方式注册异步钩子。

- `tapPromise`：Promise 方式注册异步钩子。
