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
