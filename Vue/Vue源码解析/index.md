# Vue源码解析

## 前提准备

- `[].slice.call(lis)`：将伪数组转换为真数组。

- `node.nodeType`：得到节点类型。

- `Object.defineProperty(obj, propertyName, options)`：给对象定义属性（指定描述符）。  
`configurable`：boolean 类型，是否可以重新定义或者删除。  
`enumerable`：boolean 类型，是否可以枚举。  
`writable`：boolean 类型，是否可以被修改。  
`value`：指定初始值。  
`get`：函数，用来得到当前属性值。  
`set`：函数，用来监视当前属性值的变化。

- `Object.keys(obj)`：得到对象自身可枚举的属性名组成的数组。

- `DocumentFragment`：文档碎片（高效批量更新多个节点）。  
`document`：对应显示的页面，包含 n 个 element，一旦 document 内部的某个元素改变，界面就会更新。  
`documentFragment`：内存中保存 n 个 element 的容器对象（不与界面联系），如果更新 fragment 中的某个 element，界面是不更新的。全部更新完再一次渲染到页面，提高性能。

- `obj.hasOwnProperty(propName)`：判断 propName 是否是 obj 自身的属性。

## 数据代理

通过一个对象代理对另一个对象中属性的操作（读 / 写）。

通过 `vm` 对象来代理 data 对象中所有属性的操作。

好处：更方便的操作 data 中的数据。

基本实现流程：

- 通过 `Obejct.defineProperty()` 给 vm 添加与 data 对象的属性对应的属性描述符。

- 所有添加的属性都包含了 `get` 和 `set`。

- 在 `get` 和 `set` 内部去操作 data 中对应的属性数据。
