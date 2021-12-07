# DOM

`DOM`：（Document Object Model）也叫文档对象模型，是 `HTML` 和 `XML` 文档的编程接口。

## 文档的加载

浏览器在加载一个页面时，是按照自上而下的顺序加载的。

读取一行就允许一行，如果将 `script` 标签写到页面的上边，在执行代码时，页面还没有加载，`DOM` 对象也没有加载，会导致无法获取到 `DOM` 对象。

`onload` 事件用于检测一个完全加载的页面，页面的 `html`、`css`、`js`、图片等资源都已经加载完之后才会触发 `load` 事件。

为 `window` 绑定一个 `onload` 事件。

该事件对应的响应函数将会在页面加载完成之后执行。

这样可以确保我们的代码执行时所有的 `DOM` 对象已经加载完毕。

推荐将 `script` 标签写到页面的底部，或者设置 `async` 和 `defer` 属性。

还有一个事件 `DOMContentLoaded`：当初始化的 HTML 文档被完全加载和解析完成之后，该事件就会被触发，而无需等待样式表、图像和子框架完成加载。

## DOM查询

- `ddocument.getElementById()`：通过 id 获取元素。

- `document.getElementsByTagName()`：通过标签名来获取一组元素节点对象。  
这个方法会给我们返回一个伪数组对象，所有查询到的元素都会封装到对象中。  
即使查询到的元素只有一个，也会封装到数组中返回。

- `document.getElementsByName()`：通过元素的 `name` 属性进行查找。

- `document.getElementsByClassName()`：通过 `class` 属性获取一组元素节点对象。（IE8 及以下浏览器不支持）

- `document.querySelector()`：需要一个 `css` 选择器的字符串作为参数，返回第一个匹配到的元素节点对象。

- `document.querySelectorAll()`：该方法和 `querySelector()` 用法类似，不同的是它会将符合条件的元素封装到一个伪数组中返回。

****

- `node.childNodes`：该属性会获取包括文本节点在内的所有子节点。  
`DOM` 标签之间的空白也会当成文本节点。  
注意：在 `IE8` 及以下的浏览器中，不会将空白文本当成子节点。

- `node.children`：该属性可以获取当前元素的所有子元素。

****

- `node.firstChild`：可以获取到当前元素的第一个子节点。（包括空白文本节点）

- `node.firstElementChild`：可以获取到当前元素的第一个子元素。（IE8 及以下浏览器不支持）

- `node.lastChild`：可以获取到当前元素的最后一个子节点。（包括空白文本节点）

- `node.lastElementChild`：可以获取到当前元素的最后一个子元素。（IE8 及以下浏览器不支持）

****

- `node.parentNode`：返回当前元素的父节点。

- `node.previousSibling`：返回当前元素的前一个兄弟节点。（也可能获取到空白的文本）

- `node.previousElementSibling`：获取当前元素前一个兄弟元素。（IE8 及以下浏览器不支持）

- `node.nextSibling`：返回当前元素的后一个兄弟节点。（也可能获取到空白的文本）

- `node.nextElementSibling`：获取当前元素后一个兄弟元素。（IE8 及以下浏览器不支持）

****

- `document.body`：获取 `body` 标签。

- `document.documentElement`：获取 `html` 标签。

- `document.all`：获取页面中所有元素。（该标准已废弃）  
使用 `typeof` 检查 `document.all`，`IE11` 以下返回的类型是 `onject`，其他浏览器返回的是 `undefined`。

- `node.scrollIntoView(boolean)`：让当前元素滚动到浏览器窗口的可视区域内。  
`true`：元素的顶端和其所在滚动区的可视区域的顶端对齐。  
`false`：元素的底端和其所在滚动区的可视区域的底端对齐。

## DOM操作

- `document.createElement(tagName)`：创建一个元素节点对象。  
它需要一个标签名作为参数，将会根据该标签名创建元素节点对象。

- `document.createTextNode(text)`：创建一个文本节点对象。  
需要一个文本内容作为参数，将会根据该内容创建文本节点。

- `parentElement.appendChild(node)`：向父节点中添加一个新的子节点到末尾。

- `parentElement.insertBefore(newNode, oldNode)`：在指定的子节点前插入新的子节点。

- `parentElement.replaceChild(newNode, oldNode)`：使用指定的子节点替换已有的子节点。

- `parentElement.removeChild(node)`：删除一个节点。  
`node.parentNode.removeChild(node)`

使用 `innerHTML` 也可以完成 `DOM` 的相关操作，一般我们会两种方式结合使用。

## DOM样式相关

通过 js 修改元素的样式。

```js
element.style.key = value
```

注意：如果 css 的样式中含有 `-`，使用 js 修改样式就必须转成驼峰命名法。

```js
element.style.backgroundColor = value
```

通过 `style` 属性设置的样式都是内联样式，而内联样式有较高的优先级，所以通过 js 修改的样式往往会立即显示。

但是如果在样式中写了 `!important`，则此时样式会有最高的优先级，即使通过 js 也不能覆盖样式，此时将会导致 js 修改样式失效，所以尽量不要为样式添加 `!important`。

通过 `style` 属性设置和读取的都是内联样式，无法读取样式表中的样式。

**window.getComputedStyle(element, [pseudoElt])**：获取元素当前的样式。（IE8及以下浏览器不支持）

参数：

- `element`：要获取样式的元素。

- `pseudoElt`：可选，可以传递一个伪元素，一般都传 `null`。

```js
window.getComputedStyle(document.body, '::after')
```

该方法会返回一个对象，对象中封装了当前元素对应的样式。

如果获取的样式没有设置，则会获取到真实的值，而不是默认值。（比如：没有设置 `width`，它不会获取到 `auto`，而是一个真实的长度）

**element.currentStyle**：获取元素当前显示的样式。（只有 IE 浏览器支持，其他的浏览器都不支持）

它可以用来读取当前元素正在显示的样式。

如果当前元素没有设置该样式，则获取它的默认值。

```js
// 获取 body 标签现在的背景样式
document.body.currentStyle.background
```

**通过 currentStyle 和 getComputedStyle() 获取到的样式都是只读的，不能修改，如果修改必须通过 style 属性**

定义一个函数，用来获取指定元素的指定样式。

```js
function getStyle(element, styleName) {
  return window.getComputedStyle ? getComputedStyle(element)[styleName] : element.currentStyle[styleName]
}
```

## DOM其他样式操作的属性

- `clientWidth,clientHeight`：这两个属性可以获取元素的可见宽度和高度，包括内容区和内边距。（不包括 `border`、`margin` 和滚动条）  
这些属性值返回的是一个数字，不带 `px` 的，可以直接用来进行计算。  
这些属性都是只读的，不能修改。

- `clientLeft,clientTop`：获取元素的左边框和上边框宽度，出现滚动条也包含滚动条的宽度。

- `offsetWidth,offsetHeight`：获取元素整个的宽度和高度，包括内容区、`padding`、`border` 和滚动条。（不包括 `margin`）

- `offsetLeft,offsetTop`：获取当前元素相对于定位父元素的水平垂直偏移量。

- `scrollWidth,scrollHeight`：可以获取元素整个内容滚动区域的宽度和高度。

- `scrollLeft,scrollTop`：可以获取元素水平垂直滚动的距离。

当满足 `scrollHeight - scrollTop == clientHeight` 时，说明垂直滚动条滚动到底了。

## DOM事件相关

当事件的响应函数触发时，浏览器每次都会将一个事件对象作为实参传递进响应函数。

在事件对象中封装了当前事件相关的一切信息。

比如：鼠标的坐标，键盘那个键被按下，鼠标滚轮滚动的方向...

```js
element.onclick = function(event) {
  console.log(event)
}
```

注意：在 IE8 及以下浏览器中，响应函数被触发时，浏览器不会传递事件对象，是将事件对象作为 `window` 对象的属性保存的。

```js
// 解决方法
element.onclick = function(event) {
  event = event || window.event
}
```

### 事件绑定

使用 `element.onXxx = foo` 的形式绑定响应函数，它只能同时为一个元素的一个事件绑定一个响应函数。

不能绑定多个，否则，后面绑定的会覆盖掉前面的。

`element.addEventListener(type, callback, useCapture)`：通过这个方法也可以为元素绑定响应事件。

- `type`：事件名字符串，不要 `on`。

- `callback`：事件回调函数。

- `useCapture`：布尔值，默认 `false`（事件冒泡），设置为 `true` 则事件捕获。

使用 `addEventListener()` 可以同时为一个元素的相同事件同时绑定多个响应函数。

当事件触发时，响应函数会按照函数的绑定顺序执行。

如果为同一事件多次添加同一个监听函数，该函数只会执行一次，多余的添加将自动被去除。

```js
function callback(event) {
  console.log(event)
}

document.addEventListener('click', callback)
document.addEventListener('click', callback)
```

虽然注册了 2 个点击事件，但是都是调用的同一个回调函数，所以当点击事件触发时，只会执行一次回调。

```js
document.addEventListener('click', function(event) {
  console.log(event)
})
document.addEventListener('click', function(event) {
  console.log(event)
})
```

这种单独指定回调函数的方式才会执行两次。

注意：IE8 及以下浏览器不支持 `addEventListener`。

`attachEvent(type, callback)`：在 IE8 中可以使用 `attachEvent()` 来绑定事件。

- `type`：事件名字符串，要 `on`。

- `callback`：事件回调函数。

**注意：IE8 及以下浏览器没有事件捕获阶段。**

这个方法也可以同时为一个事件绑定多个处理函数。

不同的是它是后绑定先执行，执行顺序和 `addEventListener()` 相反。

```js
document.attachEvent('onclick', function() {
  console.log(111)
})
document.attachEvent('onclick', function() {
  console.log(222)
})
// 打印顺序 222 --> 111
```

注意：`addEventListener()` 中的 `this` 是绑定的事件对象，`attachEvent()` 中的 `this` 则是 `window`。

两者的兼容性处理如下：

```js
function bind(element, type, callback) {
  if(element.addEventListener) {
    element.addEventListener(type, callback)
  } else {
    element.attachEvent('on' + type, callback.bind(element))
  }
}
```

### 事件冒泡（Bubble）

所谓的事件冒泡指的是事件的向上传导，当后代元素上的事件被触发时，其祖先元素的相同事件也会被触发。

在开发中大部分情况事件冒泡都是有用的，如果不希望发生事件冒泡可以通过事件对象来取消冒泡。

```js
event.cancelBubble = true // 已废弃
event.stopPropagation() // 推荐使用

event.stopImmediatePropagation() // 元素注册多个相同事件，执行该函数后，后续注册的事件将不再触发
```

`event.stopImmediatePropagation()`方法阻止同一事件的其他监听函数被调用，不管监听函数定义在当前节点还是其他节点。

也就是说，该方法阻止事件的传播，比 `event.stopPropagation()` 更彻底。

如果同一节点对于**同一事件**指定了多个监听函数，这些函数会根据添加的顺序依次调用。

只要其中有一个监听函数调用了 `event.stopImmediatePropagation()` 方法，其他的监听函数就不会再执行了。

### 事件默认行为

有些元素自带默认行为，比如点击 `a` 标签，会跳转相应的页面，就算绑定了点击事件，也会跳转。

```html
<a href="https://www.baidu.com"></a>
```

```js
const a = document.querySelector('a')
a.onclick = function(event) {
  console.log(event)
}
```

点击事件触发，但是还是跳转了页面，有什么方法可以阻止页面跳转呢？

在 `DOM0` 事件中，可以用 `return false` 和 `event.preventDefault()` 来阻止事件的默认行为。

```js
a.onclick = function(event) {
  console.log(event)
  event.preventDefault()
  // return false
  // 这两种方式都可以
}
```

但是在 `DOM2` 事件中，只能使用 `event.preventDefault()` 来阻止事件的默认行为。

```js
a.addEventListener('click', function(event) {
  console.log(event)
  event.preventDefault()
  // return false 这种方式不能阻止默认行为
})
```

IE8 及以下浏览器需要使用 `event.returnValue = false` 来阻止事件的默认行为。

### 事件委派（事件委托）

我们希望，只绑定一次事件，即可应用到多个元素上，即使元素是后添加的，我们可以尝试将其绑定给元素的共同祖先元素。

`事件委派`：指将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件触发时，会一直冒泡到祖先元素。

从而通过祖先元素的响应函数来处理事件。

事件委派是利用了冒泡，通过委派可以减少事件绑定的次数，提高程序的性能。

核心：`冒泡`、`target 属性`。

```js
document.body.onclick = function(event) {
  if(event.target.className.split(' ').indexOf('class 类名') === -1) return
  // 代码逻辑...
}
```

### 事件的传播

关于事件的传播，网景公司和微软公司有不同的理解。

- 微软公司认为：事件应该是由内向外传播，也就是当事件触发时，应当先触发当前元素上的事件。然后再向当前元素的祖先元素上传播，也就是说事件应该在冒泡阶段执行。

- 网景公司认为：事件应该是由外向内传播，也就是当前事件触发时，应该先触发当前元素最外层的祖先元素事件，然后再向内传播给后代元素。

`W3C` 综合两个公司的方案，将事件的传播分为三个阶段。

1. `捕获阶段`：在捕获阶段时，从最外层的祖先元素，向目标元素进行事件的捕获，但是默认此时不会触发事件。

2. `目标阶段`：事件捕获到目标元素，捕获结束开始在目标元素上触发事件。

3. `冒泡阶段`：事件从目标元素向它的祖先元素传递，依次触发祖先元素上的事件。

如果希望在捕获阶段就触发事件，可以将 `addEventListener()` 的第三个参数设置为 `true`。

一般情况下我们不会希望在捕获阶段触发事件，所以这个参数默认是 `false`。

### 鼠标拖拽注意事项

当我们拖拽一个网页中的内容时，浏览器会默认去搜索引擎中搜索内容。

此时会导致拖拽功能的异常，这个是浏览器提供的默认行为。

如果不希望发生这个行为，这可以通过 `return false` 来取消默认行为。

但是这招对 IE8 及以下浏览器不起作用。

在 IE8 及以下浏览器我们可以使用 `element.setCapture()` 来解决。

`element.setCapture()`：在处理一个 `mousedown` 事件过程中调用这个方法来把全部的鼠标事件重新定向到这个元素，直到鼠标按钮释放或者 `document.releaseCapture()` 被调用。

`element.setCapture()` 只有 IE 支持，但是火狐中调用不会报错，其他浏览器调用会报错。

当调用一个元素的 `setCapture()` 方法以后，这个元素将会把下一次所有的鼠标按下相关事件捕获到自身上。

`document.releaseCapture()`：释放鼠标捕获。

`element.setCapture()` 和 `document.releaseCapture()` 必须成对出现。

## 滚轮事件

`onmousewheel`：鼠标滚轮滚动的事件，会在滚轮滚动时触发。

注意：火狐浏览器不支持该事件，需要通过 `DOMMouseScroll` 来绑定滚动事件，并且必须通过 `DOM2` 的形式绑定，也就是 `addEventListener()` 来绑定。

这两种事件都是通过事件对象中的一个属性来判断是向上还是向下滚动。

- `onmousewheel`：通过 `event.wheelDelta` 来判断鼠标滚动的方向。  
向上滚：120。  
向下滚：-120。

- `DOMMouseScroll`：通过 `event.detail` 来判断鼠标滚动的方向。  
向上滚：-3。  
向下滚：3。

兼容写法：

```js
function wheel(event) {
  if(event.wheelDelta > 0 || event.detail < 0) {
    // 向上滚
  } else {
    // 向下滚
  }
}
```

## 键盘事件

- `onkeydown`：按键被按下时触发的事件。  
对于 `onkeydown` 来说，如果一直按着某个按键不松手，则事件会一直触发。  
当 `onkeydown` 连续触发时，第一次和第二次之间会间隔稍微长一点，其他的会非常的快。  
这种设计是为了防止误操作的行为发生。

- `onkeyup`：按键松开触发的事件。

键盘事件一般都会绑定给一些可以获得焦点的对象或者是 `document`。

按键的事件对象中有几个常用的属性：

- `keyCode`：获取按键的编码。

- `altKey`：判断 alt 键是否被按下。

- `ctrlKey`：判断 ctrl 键是否被按下。

- `shiftKey`：判断 shift 键是否被按下。

按下：`true`，未按下：`false`。

注意：如果在 `onkeydown` 中取消了默认行为，则输入的内容不会在文本框中显示。

```html
<input type="text" />
```

```js
const input = document.querySelector('input')
input.addEventListener('keydown', function(event) {
  console.log(event)
  event.preventDefault()
})
```
