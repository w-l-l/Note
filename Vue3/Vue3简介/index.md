# Vue3简介

2020 年 9 月 18 日，Vue.js 发布了 3.0 版本，代号 One Piece（海贼王）。

耗时 2 年多，2600+ 次提交，30+ 个 RFC，600+ 次 PR，99 位贡献值。

[github的tags地址。](https://github.com/vuejs/vue-next/releases/tag/v3.0.0)

## Vue3 带来了什么

**性能的提升**

- 打包大小减少 41%。

- 初次渲染快 55%，更新渲染快 133%。

- 内存减少 54%。

**源码的升级**

- 使用 `Proxy` 代替 `defineProperty` 实现响应式。

- 重写虚拟 DOM 的实现和 `Tree-Shaking`。

**拥抱TypeScript**

- Vue3 可以更好的支持 `TypeScript`。

**新的特性**

- `Composition API`（组合 API）。  
`setup` 配置。  
`ref` 与 `reactive`。  
`watch` 与 `watchEffect`。  
`provide` 与 `inject`。  
...

- 新的内置组件。  
`Fragment`。  
`Teleport`。  
`Suspense`。

- 其他改变：  
新的生命周期钩子。  
`data` 选项应始终被声明为一个函数。  
移除 `keyCode` 支持作为 `v-on` 的修饰符。
