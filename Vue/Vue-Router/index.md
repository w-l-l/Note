# Vue-Router

## 什么是路由

`后端路由`：对于普通的网站，所有的超链接都是 url 地址，所有的 url 地址都对应服务器上对应的资源。

`前端路由`：对于单页面应用程序来说，主要通过 url 中的 `hash`（#号）来实现不同页面之间的切换。

`hash` 有一个特点：HTTP 请求中不会包含 `hash` 相关的内容，所以，单页面程序中的页面跳转主要用 `hash` 实现。

在 `SPA`(Single Page Application) 单页面应用程序中，这种通过 `hash` 改变来切换页面的方式，称为前端路由（区别于后端路由）。

HTML5 的 `history` 模式：

- `history.pushState(state, title, url)`。

- `history.replaceState(state, title, url)`。

通过 `hashchange` 事件可以监听 `hash` 值的改变。

```js
window.addEventListener('hashchange', function() {
  console.log('hash值改变了')
})
```
