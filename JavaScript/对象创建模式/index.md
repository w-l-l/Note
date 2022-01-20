# 对象创建模式

## object构造函数模式

先创建空 Object 对象，再动态添加属性或方法。

使用场景：初始化时不确定对象内部数据。

缺点：语句太多。

```js
var obj = new Object()
obj.name = 'xxx'
obj.age = 18
```

## 对象字面量模式

使用 `{}` 创建对象，同时指定属性或方法。

使用场景：初始化时对象内部数据是确定的。

缺点：如果创建多个对象，有重复代码。

```js
var obj = {
  name: 'xxx',
  age: 18
}
```

## 工厂模式

通过工厂函数动态创建对象并返回。

使用场景：需要创建多个对象。

缺点：对象没有一个具体的类型，都是 Object 类型。

```js
function createPerson(name, age) {
  return {
    name: name,
    age: age
  }
}
createPerson('xxx', 18)
createPerson('xxx', 20)
```
