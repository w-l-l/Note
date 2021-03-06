# 元素位置参数

写在前面：本文章中的代码演示，默认清除的了 body 和 html 的 margin、padding。

## 定位父级

Element.offsetParent。

定位父级 offsetParent 是一个只读属性。

- 该属性返回一个指向最近的（指包含层级上的最近）包含该元素的定位元素或者最近的 table, td, th, body 元素。

```html
<div id="box1">
  <div id="box2">
    <div id="box3"></div>
  </div>
</div>
```

```js
const box1 = document.getElementById('box1')
const box2 = document.getElementById('box2')
const box3 = document.getElementById('box3')

console.log(box3.offsetParent) // 定位父级为 body

// 让 box1 开启定位
box1.style.position = 'relative'
console.log(box3.offsetParent) // 定位父级为 box1，因为它是离 box3 最近的定位父级

// 让 box2 开启定位
box2.style.position = 'relative'
console.log(box3.offsetParent) // 定位父级为 box2，因为它是离 box3 最近的定位父级
```

- 当元素的 style.display 设置为 "none" 时，offsetParent 返回 null。

```html
<div id="box"></div>
```

```js
const box = document.getElementById('box')

console.log(box.offsetParent) // 定位父级为 body

box.style.display = 'none'
console.log(box.offsetParent) // 定位父级为 null
```

- offsetParent 很有用，因为元素的 offsetTop 和 offsetLeft 都是相对于其内边距边界计算的。

> DOM 元素都包含 offsetLeft 和 offsetTop 属性。

```html
<div id="box1" style="padding: 100px">
  <div id="box2" style="padding: 200px">
    <div id="box3"></div>
  </div>
</div>
```

```js
const box1 = document.getElementById('box1')
const box2 = document.getElementById('box2')
const box3 = document.getElementById('box3')

// 定位父级为 body，box3 距离 body 内边距边界的距离，中间隔了 box2 和 box1 的距离，所以距离为 200 + 100 = 300
console.log(box3.offsetLeft, box3.offsetTop) // 300 300

box1.style.position = 'relative'
// 定位父级为 box1，box3 距离 box1 内边距边界的距离，中间隔了 box2 的距离，再加上 box1 的内边距边界，所以距离为 200 + 100 = 300
console.log(box3.offsetLeft, box3.offsetTop) // 300 300

box2.style.position = 'relative'
// 定位父级为 box2，box3 距离 box2 内边距边界的距离就为200，所以距离为 200
console.log(box3.offsetLeft, box3.offsetTop) // 200 200
```

另外提一点：在 IE7 以下浏览器中，如果当前元素的某个父级元素触发了 `haslayout`，那么 offsetParent 就会被指向到这个触发了 `hasLayout` 特性的父节点上。

### offsetParent浏览器兼容性问题

| 元素本身定位是否为 fixed | 条件 | offsetParent 值 |
| :-----: | :---- | :----: |
| true | 不是火狐浏览器<br>是火狐浏览器 | null<br>body |
| false | 父级有定位<br>父级没有定位 | 定位父级<br>body |

注意：在不是火狐浏览器情况下，元素本身定位为 fixed，虽然此时的 offsetParent 值为 null，但是该元素的 offsetLeft 和 offsetTop 却依照**视口**计算距离，这点要注意一下。

```html
<div id="box1" style="padding: 100px">
  <div id="box2" style="padding: 200px">
    <div id="box3" style="position: fixed"></div>
  </div>
</div>
```

```js
const box1 = document.getElementById('box1')
const box2 = document.getElementById('box2')
const box3 = document.getElementById('box3')

console.log(box3.offsetLeft, box3.offsetTop) // 300 300

box1.style.position = 'relative'
console.log(box3.offsetLeft, box3.offsetTop) // 300 300

box2.style.position = 'relative'
console.log(box3.offsetLeft, box3.offsetTop) // 300 300

console.dir(box3) // 如下图
```

![box3位置属性](./img/box3_location.png)

### 分清parentNode和offsetParent

parentNode 是元素的直接父级。

offsetParent 是元素的定位父级，类似于 css 的包含块。

## getBoundingClientRect

Element.getBoundingClientRect()。

元素身上的 getBoundingClientRect 方法返回一个 DOMRect 对象，里面包含元素的大小及其相对于视口的位置。

DOMRect 对象包含 8 个属性：

1. width / height：代表元素的 border-box 的尺寸。
2. x / y, left / top：元素左上方相对于视图窗口左上角的距离来计算。
3. right / bottom：元素右下方相对于视图窗口左上角的距离来计算。

> 如果是标准盒子模型，元素的尺寸等于 width / height + padding + borderWidth 的总和。如果 box-sizing: border-box，元素的的尺寸等于 width / height。

![getBoundingClientRect原理图](./img/getBoundingClientRect.png)

```html
<div style="padding: 200px">
  <div id="box" style="width: 100px;height: 100px"></div>
</div>
```

```js
const box = document.getElementById('box')

// 获取元素大小及其相对于视口的位置
const domRect = box.getBoundingClientRect()
/*
  box元素的 width 和 height 都是 100px 很清楚。
  因为父元素有 200px 的内边距，所以 x / left 为 200px，y / top 也为 200px。
  right = box的宽度 + 父元素的左内边距 = 300px。
  bottom = box的高度 + 父元素的上内边距 = 300px。
*/
console.log(domRect)
```

![box元素大小及其相对于视口的位置](./img/getBoundingClientRect_box.png)

## 其他位置距离相关

### client相关

Element.clientWidth。

Element.clientHeight。

- 表示元素内部宽度 (width | height) + padding 的距离。（只读，不包括 border，margin 和滚动条）

Element.clientTop。

Element.clientLeft。

- 元素上方和左方边框的宽度。（只读）

### offset相关

Element.offsetWidth。

Element.offsetHeight。

- 表示元素的布局宽度和高度 (width | height) + padding + border + 滚动条的距离。（只读，不包括 margin）

Element.offsetTop。

Element.offsetLeft。

- 返回当前元素相对于其 offsetParent 元素上方和左方内边距边界的距离。（只读）

### scroll相关

Element.scrollWidth。

Element.scrollHeight。

- 元素内容层的真实宽度和高度。（只读，可视区域宽高度 + 被隐藏区域的宽高度，如果元素的内容可以适合而不需要滚动条，则 scrollWidth = clientWidth，scrollHeight = clientHeight）

Element.scrollTop。

Element.scrollLeft。

读取或设置元素滚动条到元素上方或左边的距离。（元素被卷去的距离）

### 鼠标距离相关

Event.clientX。

Event.clientY。

- 返回当事件被触发时鼠标指针相对于 `浏览器页面（或客户区）` 左上方的 x, y 坐标。（客户区指的是当前窗口）

Event.screenX。

Event.screenY。

- 返回事件发生时鼠标指针相对于 `屏幕` 左上方的 x, y 坐标。

Event.offsetX。

Event.offsetY。

- 返回事件发生时鼠标指针相对于 `事件元素` 左上方的 x, y 坐标。

Event.pageX。

Event.pageY。

- 返回事件发生时鼠标指针相对于 `document 对象（即文本窗口）` 左上方为原点的 x, y 坐标。

## 扩展

document.documentElement.clientWidth：并不是根标签的可视区域，而是视口的大小。

document.documentElement.offsetWidth：根标签的 border-box。

注意：在 IE10 及 IE10 以下浏览器中，根标签的 clientWidth 和 offsetWidth 统一被指定为视口的宽度。
