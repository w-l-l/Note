# 模块化介绍

描述一种特别的编码项目 js 的方式，以模块为单元一个一个编写的。

将一个复杂的程序依据一定的规则（规范）封装成几个块（文件），并进行组合在一起。

块的内部数据和实现都是私有的，只是向外部暴露一些接口（方法），与外部其他块通信。

**一个模块的组成**

- 私有的数据 --> 内部的变量。

- 私有的行为（操作数据）--> 内部的函数。

- 向外暴露 n 个行为。

## 页面加载多个 js 的问题

```js
<script src="test1.js"></script>
<script src="test2.js"></script>
<script src="test3.js"></script>
<script src="test4.js"></script>
```

没有模块化之前我们都是这样引入 js 文件的，会导致请求过多、依赖模糊、难以维护。

首先我们要依赖多个模块，那样就会发送多个请求，导致请求过多。

然后就是依赖关系模糊，我们不知道它们的具体依赖关系是什么，也就是说很容易因为依赖关系导致出错。

以上的现象就导致了这样会很难维护，很可能出现牵一发而动全身的情况，导致项目出现严重的问题。

这些问题可以通过现代模块化编码和项目构建来解决。

## 模块化的进化过程

- 全局 function 模式：

```js
function a() { ... }
function b() { ... }

/*
  将不同的功能封装成不同的全局函数
  Global 被污染了，很容易引起命名冲突
*/
```

- 命名空间模式（namespace）：

```js
let obj = {
  data: { ... },
  a() { ... },
  b() { ... }
}

/*
  简单的对象封装
  作用：减少了全局变量
  问题：不安全（数据不是私有的，外部可以直接修改）
*/
```

- IIFE 模式：

```js
(function(window) {
  let data = { ... }
  function a() { ... }
  function b() { ... }
  window.myModule = { a, b }
})(window)

/*
  匿名函数自调用（闭包）
  将数据和行为封装到一个函数内部，通过给 window 添加属性来向外暴露接口
  IIFE：immediately invoked function expression（立即调用函数表达式）
  作用：数据是私有的，外部只能通过暴露的方法操作
  问题：如果当前模块依赖另一个模块怎么办？
*/
```

- IIFE 增强模式：

```js
(function(window, $) {
  let data = { ... }
  function a() { ... }
  function b() {
    $('body').css('background', 'red')
  }
  window.myModule = { a, b }
})(window, jQuery)

/*
  引入依赖
  这就是现代模块实现的基石
*/
```
