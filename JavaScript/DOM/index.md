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
