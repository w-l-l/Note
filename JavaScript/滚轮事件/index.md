# 滚轮事件

## 非Firefox浏览器中

在非 Firefox 浏览器中，使用 mousewheel 事件来监听鼠标的滚轮事件，通过事件对象的 wheelDelta 属性可以判断现在是向下滚动还是向上滚动。

```js
// 方式一 DOM0
document.documentElement.onmousewheel = function(event) {
  console.log(event.wheelDelta) // 滚轮信息
}

// 方式二 DOM2
document.documentElement.addEventListener('mousewheel', function(event) {
  console.log(event.wheelDelta) // 滚轮信息
})
```

之前在非 Firefox 浏览器中。

- 向上滚动 wheelDelta 的值为 120。

- 向下滚动 wheelDelta 的值为 -120。

![非 Firefox 浏览器中](./img/wheelDelta_120.png)

但是现在在 Chrome 浏览器中发现。

- 向上滚动 wheelDelta 的值为 150。

- 向下滚动 wheelDelta 的值为 -150。

![Chrome 浏览器中](./img/wheelDelta_150.png)

## Firefox浏览器中

在 Firefox 浏览器中，必须使用 DOMMouseScroll 事件来监听鼠标的滚轮事件，通过事件对象的 detail 属性可以判断现在是向下滚动还是向上滚动。

```js
// 注意：DOMMouseScroll 事件只能通过 DOM2 的事件绑定形式，不能使用 DOM0 的事件绑定形式
document.documentElement.addEventListener('DOMMouseScroll', function(event) {
  console.log(event.detail) // 滚轮信息
})
```

在 Firefox 浏览器中。

- 向上滚动 detail 的值为 -3。

- 向下滚动 detail 的值为 3。

![Firefox 浏览器中](./img/detail_3.png)

## 滚轮事件兼容性处理

上述可以发现，在不同浏览器中鼠标滚轮事件是不一样的，并且滚轮的信息也不一致。

滚轮的信息只需要关心它值的 `正 / 负` 就行了，不用关心值是多少，因为在快速滚动时，值会出现偏差。

在 Chrome 浏览器中举例。

![Chrome 浏览器中快速滚动](./img/wheelDelta_300.png)

可以很明显的看见，wheelDelta 的值出现了正负 150 的倍数值。不过不用担心这些，我们只需要通过值的正负，来判断鼠标是向上滚动，还是向下滚动就行了。

滚轮事件兼容性处理函数封装。

```js
/*
  element：绑定事件的元素
  upCallback：向上滚动的回调
  downCallback：向下滚动的回调
*/
function myMousewheel(element, upCallback, downCallback) {
  // 滚轮事件回调
  function mousewheelCallback(e) {
    // 事件对象兼容性处理
    e = e || event

    /*
      非 Firefox 浏览器中，wheelDelta > 0 向上滚动，wheelDelta < 0 向下滚动
      Firefox 浏览器中，detail < 0 向上滚动，detail > 0 向下滚动
      因为 wheelDelta 和 detail 只会有一个存在，另一个的取值就是 undefined，和 0 比较都返回 false
      使用或运算符，左右两边一个为 true 就返回 true，都为 false 才返回 false
    */
    if (e.wheelDelta > 0 || e.detail < 0) {
      // 向上滚动回调
      upCallback && upCallback(e)
    } else {
      // 向下滚动回调
      downCallback && downCallback(e)
    }
  }

  // 非 Firefox / IE 浏览器绑定滚轮事件
  element.onmousewheel = mousewheelCallback

  // IE 浏览器绑定滚轮事件
  element.attachEvent && element.attachEvent('onmousewheel', mousewheelCallback)

  // Firefox 浏览器绑定滚轮事件
  element.addEventListener && element.addEventListener('DOMMouseScroll', mousewheelCallback)
}
```

非 Firefox 浏览器中测试。

![非 Firefox 浏览器中测试](./img/myMousewheel_noFirefox.png)

Firefox 浏览器中测试。

![Firefox 浏览器中测试](./img/myMousewheel_Firefox.png)
