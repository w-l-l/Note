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
