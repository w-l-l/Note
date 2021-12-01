# BOM

`BOM`：（Browser Object Model）也叫浏览器对象，它提供了很多对象，用于访问浏览器的功能。

但是 `BOM` 是没有标准的，每一个浏览器厂家会根据自己的需求来扩展 `BOM` 对象。

`BOM` 可以使我们通过 JavaScript 来操作浏览器。

在 `BOM` 中为我们提供了一组对象，用来完成对浏览器的操作。

`BOM` 对象包括如下：

- `window`：代表的是整个浏览器的窗口，同时 `window` 也是网页中的全局对象。

- `navigator`：代表的当前浏览器信息，通过该对象可以来识别不同的浏览器。

- `location`：代表当前浏览器的地址栏信息，通过 `location` 可以获取地址栏信息，或者操作浏览器跳转页面。

- `history`：代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录。

- `screen`：代表用户的屏幕信息，通过该对象可以获取到用户的显示器的相关信息。

这些 `BOM` 对象在浏览器中都是作为 `window` 对象的属性保存的，可以通过 `window` 对象来使用，也可以直接使用。

## navigator

由于历史原因，`Navigator` 对象中的大部分属性都已经不能帮助我们识别浏览器了。

一般我们只会使用 `userAgent` 来判断浏览器的信息。

`userAgent` 是一个字符串，这个字符串中包含有用来描述浏览器信息的内容。

不同的浏览器会有不同的 `userAgent`。

```js
navigator.userAgent // Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
```

在 `IE11` 中已经将微软和 `IE` 相关的标识都去除了，所以我们基本已经不能通过 `userAgent` 来识别一个浏览器是否是 `IE` 了。

如果通过 `userAgent` 不能判断，还可以通过一些浏览器中特有的对象，来判断浏览器的信息。

比如：`ActiveObject`。

## history

由于隐私原因，该对象不能获取到具体的历史记录，只能操作浏览器向前或向后翻页。

而且该操作只在当次访问时有效。

- `history.length`：可以获取当前访问的链接数量。

- `history.back()`：可以用来回退到上一个页面，作用和浏览器的回退按钮一样。

- `history.forward()`：可以跳转下一个页面，作用和浏览器的前进按钮一样。

- `history.go(n)`：可以用来跳转到指定的页面。
<br>
需要一个整数作为参数。
<br>
`n=1`：表示向前跳转一个页面，相当于 `forward()`。
<br>
`n=2`：表示向前跳转两个页面。
<br>
`n=-1`：表示向后跳转一个页面，相当于 `back()`。
<br>
`n=-2`：表示向后跳转两个页面。

## location

如果直接 `alert(location)`，则可以获取到地址栏的信息。（当前页面的完整路径）

如果直接将 `location` 属性修改为一个完整的路径，或相对路径，则我们页面会自动跳转到该路径，并且会生成相应的历史记录。

```js
location = 'https://www.baidu.com'
```

- `location.assign(url)`：用来跳转到其他的页面，作用和直接修改 `location` 一样。

```js
location.assign('https://www.baidu.com')
```

- `location.reload()`：用于重新加载当前页面，作用和刷新按钮一样。
<br>
如果在方法中传递一个 `true`，则会强制清空缓存刷新页面。

```js
location.reload() // 正常刷新
location.reload(true) // 清空缓存刷新
```

- `location.replace(url)`：可以使用一个新的页面替换当前页面，调用完毕也会跳转页面。
<br>
不会生成历史记录，使用回退按钮不能回到之前的页面。

```js
location.replace('https://www.baidu.com')
```
