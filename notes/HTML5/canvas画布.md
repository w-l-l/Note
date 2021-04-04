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

## canvas状态

save()：将当前状态放入栈中，保持 `canvas` 全部状态的方法。

保存到栈中的绘制状态由下面部分组成。

1. 当前的变换矩阵。
2. 当前的剪切区域。
3. 当前的虚线列表。
4. 绘制图形的样式（strokeStyle / fillStyle / lineWidth / lineJoin / lineCap ...）。

restore()：通过在绘图状态栈中弹出顶端的状态，将 `canvas` 恢复到最近的保存状态的方法，如果没有保存状态，此方法不做任何改变。

通常我们在绘制图形进行的操作，都会放在 save() 和 restore() 方法之间，避免当前绘制图形设置的状态，影响到后续图形的绘制效果。

```js
ctx.save() // 画布默认状态放入栈中
ctx.fillStyle = 'red' // 当前填充样式设置为红色
ctx.fillRect(0, 0, 50, 50) // 绘制矩形，显示效果为红色
ctx.restore() // 弹出栈中顶端状态，这个时候红色的填充样式会被弹出的状态覆盖，变为黑色。
ctx.fillRect(50, 0, 50, 50) // 绘制矩形，显示效果为黑色
```

## canvas圆形 / 圆弧

arc(x, y, radius, startAngle, endAngle, anticlockwise)：画一个以(x, y)坐标为圆心，radius 为半径的圆弧或圆，从 startAngle 开始，到 endAngle 结束。

参数如下：

1. x：圆心在画布 x 轴上的偏移量。
2. y：圆心在画布 y 轴上的偏移量。
3. radius：绘制圆的半径。
4. startAngle：圆弧的起始点，x 轴方向开始计算，单位以弧度表示。
5. endAngle：圆弧的终点，单位以弧度表示。
6. anticlockwise：布尔值。true 表示逆时针，false 表示顺时针（默认值）。

```js
ctx.beginPath()
ctx.arc(100, 100, 50, 0, 2 * Math.PI, false) // 绘制一个在(100, 100)坐标为圆心，半径50的圆
ctx.stroke() // 绘制边框圆形
```

arcTo(x1, y1, x2, y2, radius)：根据设置的两个控制点和半径画一段圆弧。

注意：

- 必须存在一个开始坐标点 ctx.moveTo(x, y)，三点才能构成圆弧，半径为 radius 的圆向夹角里面填充。

- 绘制的圆弧一定经过起点，但不一定经过 (x1, y1) 和 (x2, y2)，这两个坐标只是用来控制方向的。

```js
ctx.beginPath()
ctx.moveTo(20, 20)
ctx.lineTo(200, 0)
ctx.lineTo(100, 100)
ctx.stroke()
// 将3个点连接起来

ctx.beginPath()
ctx.moveTo(20, 20)
ctx.arcTo(200, 0, 100, 100, 20)
ctx.stroke()
// 观察圆弧在3点之间的位置
```
![圆弧](/images/canvas/arcTo.png)

## canvas贝塞尔曲线

### 二次贝塞尔

quadraticCurveTo(cpx, cpy, x, y)：绘制二次贝塞尔曲线。

参数：

1. cpx：控制点的 x 轴坐标。
2. cpy：控制点的 y 轴坐标。
3. x：终点的 x 轴坐标。
4. y：终点的 y 轴坐标。

注意：

- 必须要设置起点 moveTo(x, y)。

- 二次贝塞尔曲线一定经过起点和终点。

```js
ctx.beginPath()
ctx.moveTo(20, 20)
ctx.lineTo(200, 0)
ctx.lineTo(100, 100)
ctx.stroke()
// 将3个点连接起来

ctx.beginPath()
ctx.moveTo(20, 20)
ctx.quadraticCurveTo(200, 0, 100, 100, 20)
ctx.stroke()
// 观察二次贝塞尔曲线在3点之间的位置
```
![二次贝塞尔曲线](/images/canvas/quadraticCurveTo.png)

### 三次贝塞尔

bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)：绘制三次贝塞尔曲线。

参数只是在二次贝塞尔的基础上多增加了一个控制点 (cp2x, cp2y)。

同样要设置起点 moveTo(x, y)，也一定经过起点和终点。

```js
ctx.beginPath()
ctx.moveTo(20, 20)
ctx.lineTo(200, 0)
ctx.lineTo(100, 100)
ctx.lineTo(200, 100)
ctx.stroke()
// 将4个点连接起来

ctx.beginPath()
ctx.moveTo(20, 20)
ctx.bezierCurveTo(200, 0, 100, 100, 200, 100, 20)
ctx.stroke()
// 观察三次贝塞尔曲线在4点之间的位置
```

![三次贝塞尔曲线](/images/canvas/bezierCurveTo.png)

## canvas变换

### 平移变换

translate(x, y)：对当前 `canvas` 画布进行平移变换。

![平移变换原理](/images/canvas/translate_origin.png)

参数：

1. x：水平方向的移动距离。
2. y：垂直方向的移动距离。

注意：在 `canvas` 中 translate 是累加的。

```js
ctx.translate(50, 50) // x 轴和 y 轴都平移 50px
ctx.fillRect(0, 0, 100, 100)
```
![平移变换](/images/canvas/translate.png)

### 旋转变换

rotate(angle)：将当前 `canvas` 画布对照原点顺时针旋转。

![旋转变换原理](/images/canvas/rotate_origin.png)

参数：

1. angle：顺时针旋转的弧度(degree * Math.PI / 180)

旋转的中心始终是 `canvas` 的原点(0, 0)，x 轴顺时针旋转，如果要改变中心点，我们可以通过 translate() 方法移动 `canvas`。

注意：在 `canvas` 中 rotate 是累加的。

```js
ctx.translate(50, 50)
ctx.rotate(45 * Math.PI / 180) // 顺时针旋转 45 度
ctx.fillRect(0, 0, 100, 100)
```
![旋转变换](/images/canvas/rotate.png)

### 伸缩变换

scale(x, y)：将当前 `canvas` 画布的 x 轴和 y 轴进行伸缩变换。

参数：

1. x：水平方向的缩放因子。
2. y：垂直方向的缩放因子。

注意：

- 在 `canvas` 中 scale 是累加的。

- 缩放因子为 1 时大小不变，值为负数按照 x 轴或 y 轴进行翻转（翻转上下文）。

> 默认的，在 `canvas` 中一个单位实际上就是一个像素。例如，如果我们将 0.5 作为缩放因子，最终的单位会变成 0.5 像素，并且形状的尺寸会变成原来的一半。相似的方式，我们将 2.0 作为缩放因子，将会增大单位尺寸变成两个像素。形状的尺寸将会变成原来的两倍。

图形伸缩：

```js
ctx.translate(50, 50)
ctx.scale(2, 1) // x 轴放大 2 倍，y 轴不变
ctx.fillRect(0, 0, 100, 100)
```

![图形伸缩](/images/canvas/scale_box.png)

文字翻转：

```js
ctx.scale(-2, 1) // x 轴放大 2 倍，并翻转，y 轴不变
ctx.font = '48px serif'
ctx.fillText('canvas', -200, 100)
```

![文字翻转](/images/canvas/scale_text.png)
