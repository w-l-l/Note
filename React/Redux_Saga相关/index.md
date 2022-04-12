# Redux_Saga相关

`redux-saga` 是一个用于管理应用程序 `Side Effect`（副作用，例如异步获取数据，访问浏览器缓存等）的库。

`redux-saga` 使用了 ES6 的 `Generator` 功能，让异步的流程更易于读取，写入和测试。通过这样的方式，这些异步的流程看起来就像是标准同步的 JavaScript 代码。

你可能已经用了 `redux-thunk` 来处理数据的读取。不同于 `redux-thunk`，你不会再遇到回调地狱了，你可以很容易地测试异步流程并保持你的 `action` 是干净的。
