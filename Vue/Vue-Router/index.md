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

## 向路由组件传递数据

**params 传参：**

配置路由：

```js
import VueRouter from 'vue-router'
import home from 'views/home'

const router = new VueRouter({
  routes: [
    {
      name: 'home',
      path: '/home/:id',
      component: home
    }
  ]
})
```

路由路径：

```html
<router-link :to="'/home/' + id"></router-link>
<!-- 或者 -->
<router-link :to="{ name: 'home', params: { id } }"></router-link>
```

等同于：

```js
this.$router.push(`/home/${id}`)
this.$router.push({
  name: 'home',
  params: { id }
})
```

**注意：params 只能通过 name 来引入路由，使用 path 会 undefined。**

路由组件中读取 `params` 参数：`this.$route.params.id`。

**query 传参：**

配置路由：

```js
import VueRouter from 'vue-router'
import home from 'views/home'

const router = new VueRouter({
  routes: [
    {
      name: 'home',
      path: '/home',
      component: home
    }
  ]
})
```

路由路径：

```html
<router-link to="/home?id=1"></router-link>
<router-link :to="{ path: '/home', query: { id } }"></router-link>
<router-link :to="{ name: 'home', query: { id } }"></router-link>
```

等同于：

```js
this.$router.push('/home?id=1')
this.$router.push({
  path: '/home',
  query: { id }
})
this.$router.push({
  name: 'home',
  query: { id }
})
```

`query` 传参可以通过 name 或 path 来引入路由。

路由组件中读取 `query` 参数：`this.$route.query.id`

**props 参数：**

`router-view` 携带数据。

```html
<router-view :msg="msg"></router-view>
```

- `props` 的值为布尔类型，设置为 `true` 时，`route.params` 将被设置为组件的 `props`。

```js
{
  path: '/home/:id',
  component: home,
  props: true
}

const home = {
  props: ['id'], // 使用 props 接收路由参数
  template: '<div>用户ID: {{ id }}</div>' // 使用路由参数
}
```

- `props` 的值为对象类型，它将原样设置为组件的 `props`，当 `props` 是静态的时候很有用。

```js
{
  path: '/home/:id',
  component: home,
  props: {
    name: 'xxx',
    age: 18
  }
}

const home = {
  props: ['name', 'age', 'id'], // 这种方式 id 不存在，因为 props 对象中没有 id 属性
  template: '<div>用户信息: {{ name }} --- {{ age }}</div>'
}
```

- `props` 的值为函数类型，这允许你将参数转换为其他类型，将静态值与基于路由的值相结合等等。

```js
{
  path: '/home/:id',
  component: home,
  props: route => {
    return {
      name: 'xxx',
      age: 18,
      id: route.params.id
    }
  }
}

const home = {
  props: ['name', 'age', 'id'],
  template: '<div>用户信息: {{ name }} --- {{ age }} --- {{ id }}</div>'
}
```

## 缓存路由组件

默认情况下，被切换的路由组件对象会死亡释放，再次回来时是重新创建的。

如果可以缓存路由组件对象，可以提高用户体验。

实现方式：`keep-alive`。

```html
<keep-alive>
  <router-view></router-view>
</keep-alive>
```

属性：

- `include`：支持字符串、数组、正则表达式，只有名称匹配的组件会被缓存。

```html
<keep-alive include="home,about"></keep-alive>
<keep-alive :include="['home', 'about']"></keep-alive>
<keep-alive :include="/home|about/"></keep-alive>
```

- `exclude`：支持字符串、数组、正则表达式，任何名称匹配的组件都不会缓存。

```html
<keep-alive exclude="home,about"></keep-alive>
<keep-alive :exclude="['home', 'about']"></keep-alive>
<keep-alive :exclude="/home|about/"></keep-alive>
```

- `max`：数值类型，最多可以缓存多少个路由组件实例。

```html
<keep-alive :max="10"></keep-alive>
```

**注意：匹配首先检查组件自身的 name 选项，如果 name 选项不可用，则匹配它的局部注册名称（父组件 component 选项的键值），匿名组件不能被匹配。**

当组件在 `keep-alive` 内被切换时，它的 `activated` 和 `deactivated` 这两个生命周期钩子函数将会被对应执行。

- `activated`：被 `keep-alive` 缓存的组件激活时调用。

- `deactivated`：被 `keep-alive` 缓存的组件停用时调用。

**注意：这两个钩子函数在服务器端渲染期间是不被调用的。**
