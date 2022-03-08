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

## 模板解析

模板解析的关键对象：compile 对象。

模板解析的基本流程：

- 将 `el` 的所有子节点全部取出，添加到一个新建的文档 `fragment` 对象。

- 将 `fragment` 中的所有层次子节点递归进行编译解析处理。  
对表达式文本节点进行解析。  
对元素节点的指令属性进行解析。  
事件指令解析。  
一般指令解析。

- 将解析后的 `fragment` 添加到 `el` 中显示。

**解析表达式文本节点：**

根据正则对象得到匹配出的表达式字符串。

```js
const str = '{{name}}';
/\{\{(.*)\}\}/.test(str);
// RegExp.$1 --> name
```

从 data 中取出表达式对应的属性值。

将属性值设置为文本节点的 `textContent`。

```js
textNode.textContent = value
```

**事件指令解析：**

从指令中取出事件名 `v-on:click="callback"`。

根据事件指令的值（表达式）从 `methods` 中得到对应的事件处理函数。

给当前元素节点绑定指定事件名和回调函数的 `DOM` 事件监听。

指令解析完后，移除此指令属性。

```js
elementNode.addEventListener(eventName, callback.bind(vm), false)
```

**一般指令解析：**

得到指令名和指令值。

从 data 中根据表达式得到对应的值。

根据指令名确定需要操作元素节点的什么属性。

- `v-text`：textContent 属性。

- `v-html`：innerHTML 属性。

- `v-class`：className 属性。

- ...

将得到的表达式的值设置到对应的属性上。

移除元素的指令属性。
