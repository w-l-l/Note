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

## 使用 vue-router

下载 `vue-router`。

```js
npm install vue-router --save
```

创建路由器 `router/index.js`：

```js
import VueRouter from 'vue-router'
import home from 'views/home'
import A from 'views/home/A'
import B from 'views/home/B'
Vue.use(VueRouter)

export default new VueRouter({
  routes: [
    { // 一般路由
      path: '/home',
      component: home,
      children: [ // 嵌套路由
        {
          path: 'a',
          component: A
        }, {
          path: 'b',
          component: B
        }
      ]
    },
    { // 重定向路由
      path: '/',
      redirect: '/home'
    }
  ]
})
```

注册路由器 `main.js`：

```js
import Vue from 'vue'
import router from './router'
new Vue({ router })
```

使用路由组件标签：

```html
<!-- 用来生成路由链接 -->
<router-link to="/path" tag="xxx" replace active-class="active"></router-link>
```

- `tag`：可以根据需求渲染成其他标签（默认 a 标签）。

- `replace`：不会留下 history 记录，所以指定 replace 的情况下，后退键不能返回到上一个页面中。

- `active-class`：当 `router-link` 对应的路由匹配成功时，会自动给当前元素设置一个 `router-link-active` 的 class，设置 `active-class` 可以修改默认的名称。

```html
<!-- 用来显示当前路由组件界面 -->
<router-view></router-view>
```

使用 HTML5 的 history 模式：

```js
const router = new VueRouter({
  mode: 'hidtory',
  base: '/vue' // 应用的基路径
})
```
