# canvas画布

## canvas画布的宽度与高度

`canvas` 标签只有两个属性：width 和 height。

当没有设置宽度和高度的时候，`canvas` 会初始化成宽300px，高150px的画布。

使用 html 属性设置 width，height 时，只影响画布本身，不影响画布内容。

通过 css 样式指定 `canvas` 的 width，height 时，不但影响画布本身的宽高，还会使画布中的内容等比例缩放（缩放参照画布默认的尺寸）。

```html
<canvas width="300" height="150"></canvas>
```

## canvas画笔

`canvas` 标签身上有一个 getContext 方法。

这个方法是用来获取渲染上下文和它绘画的功能。

```js
if (canvasElement.getContext) {
  // 检查浏览器是否支持canvas
  var ctx = canvasElement.getContext('2d')
  // canvas相关的操作基本都在ctx上面进行
}
```

## canvas绘制矩形

`canvas` 标签只支持一种原生的图形绘制，那就是矩形。

其他图形的绘制至少都需要生成一条路径（后续会介绍路径）。

矩形包含 fillRect（填充矩形）和 strokeRect （边框矩形）两种。

两种矩形的参数都是一样的：

1. x：x轴偏移量。
2. y：y轴偏移量。
3. width：矩形宽度。
4. height：矩形高度。

注意：画布左上方x轴，y轴的偏移量为(0, 0)，所以这里可以看作为原点。

```js
ctx.fillRect(x, y, width, height)
ctx.strokeRect(x, y, width, height)
```

## strokeRect边框矩形渲染问题

按理来说，strokeRect 渲染时的默认边框应该是1px。

但是 `canvas` 在渲染矩形边框时，边框宽度是平均分在偏移位置两侧的。

举例说明：

```js
ctx.strokeRect(10, 10, 50, 50)
// 边框会渲染在9.5-10.5之间，浏览器是不会让一个像素只显示一半的，只会全部显示。相当于边框会渲染在9-11之间，也就是2px。

ctx.strokeRect(10.5, 10.5, 50, 50)
// 将偏移量多移动0.5，边框会渲染在10-11之间，也就是1px。
```

## canvas清除区域

clearRect 可以清除 `canvas` 画布上指定的区域，让清除部分完全透明。

```js
ctx.clearRect(x, y, width, height)
```

## canvas样式和颜色

在绘制图形之前，我们可以先设置图形的样式或者颜色。

fillStyle：设置图形的填充颜色（默认黑色）。

strokeStyle：设置图形轮廓的颜色（默认黑色）。

lineWidth：设置当前绘线的粗细，属性值必须为正数（默认值1.0，0、负数、Infinity和NaN会被忽略）。

lineJoin：设定线条与线条间结合的样式（默认miter）。

1. round：圆角。
2. bevel：斜角。
3. miter：直角。

```js
ctx.fillStyle = 'red'
ctx.strokeStyle = 'blue'
ctx.lineWidth = 5
ctx.lineJoin = 'round'
ctx.fillRect(10, 10, 50, 50)
ctx.strokeRect(100, 100, 50, 50)
```

## canvas路径

beginPath()：新建一条路径，生成之后，图形绘制命令被指向到路径上准备生成路径。

- 本质上，路径是由多个子路径构成，这些子路径都是在一个路径列表中，每次调用 beginPath，路径列表都会清空重置。

- 通常我们在绘制图形之前，都会调用该方法。

moveTo(x, y)：将画笔移动到指定的坐标轴上（设置起点）。

lineTo(x, y)：绘制一条从当前位置到指定坐标轴位置的直线。

closePath()：闭合路径，图形绘制命令又重新指向到上下文中。

- 使用 fill() 绘制图形或图形路径已经闭合不需要使用此方法。

- 通常使用 stroke() 绘制图形的时候才使用此方法。

```js
ctx.beginPath()
ctx.moveTo(10, 10)
ctx.lineTo(50, 10)
ctx.lineTo(50, 50)
ctx.fill()
ctx.closePath()
```

stroke()：通过线条来绘制图形轮廓，不会自动调用 closePath()。

fill()：通过填充路径的内容区域生成实心的图形，自动调用closePath()。

rect(x, y, width, height)：绘制一个偏移量(x, y)，宽 width，高 height 的矩形。

- 当该方法执行的时候，moveTo() 方法自动设置起点坐标为(x, y)。

- 该方法执行完毕的时候，画布上不会呈现图形，相当于只是形成了路径列表，要调用 fill() 或 stroke() 方法才会呈现在画布中。

```js
ctx.beginPath()
ctx.rect(0, 0, 100, 100)
ctx.fill()
ctx.closePath()

ctx.beginPath()
ctx.rect(100, 100, 100, 100)
ctx.stroke()
ctx.closePath()
```

lineCap：绘制每一条线段末端的样式属性。

1. butt：线段末端以方形结束（默认值）。
2. round：线段末端以圆形结束。
3. square：线段末端以方形结束，但是增加了一个宽度和线段相同，高度是线段宽度一半的矩形区域。

```js
ctx.beginPath()
ctx.lineCap = 'round'
ctx.lineWidth = 10
ctx.moveTo(10, 10)
ctx.lineTo(50, 10)
ctx.lineTo(50, 50)
ctx.stroke()
ctx.closePath()
```
