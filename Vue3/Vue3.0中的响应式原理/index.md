# Vue3.0中的响应式原理

## Vue2.x 的响应式

实现原理：

- 对象类型：通过 `Object.defineProperty()` 对属性的读取、修改进行拦截。（数据劫持）

- 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）

```js
Object.defineProperty(data, 'key', {
  get() {},
  set() {}
})
```

存在问题：

- 新增属性、删除属性，界面不会更新。

- 直接通过下标修改数组，界面不会自动更新。

## Vue3.x 的响应式

实现原理：

- 通过 [Proxy](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)（代理）：拦截对象中任意属性的变化，包括属性值的读写、属性的添加、属性的删除等。

- 通过 [Reflect](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect)（反射）：对源对象的属性进行操作。（成功返回 true，失败返回 false，不会报错）

使用 `Proxy`，Vue2.x 数据响应式存在的问题将全部解决。

```js
new Proxy(data, {
  // 拦截读取属性值
  get(target, prop) {
    return Reflect.get(target, prop)
  },
  // 拦截设置属性或添加属性
  set(target, prop, value) {
    return Reflect.set(target, prop, value)
  },
  // 拦截删除属性
  deleteProperty(target, prop) {
    return Reflect.deleteProperty(target, prop)
  }
})
```
