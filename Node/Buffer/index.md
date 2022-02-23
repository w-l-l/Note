# Buffer(缓冲区)

`Buffer` 的结构和数组很像，操作的方法也和数组类似。

数组中不能存储二进制的文件，而 Buffer 就是专门用来存储二进制数据的。

使用 Buffer 不需要引入模块，直接使用即可。

在 Buffer 中存储的都是二进制数据，但是在显示时都是以 16 进制的形式显示的。

- Buffer 中每一个元素的范围是从 `00 --> ff(16进制)`，也就是 `00 --> 255`（`00000000 --> 11111111`）。

- 在计算机中，一个 0 或一个 1 我们称为 1 位（bit）。

- 注意：一个汉字占 3 个字节。

单位转换：

- 8bit = 1byte（字节）。

- 1024byte = 1kb。

- 1024kb = 1mb。

- 1024mb = 1gb。

- 1024gb = 1tb。

Buffer 中的一个元素，占用内存的一个字节。

Buffer 的大小一旦确定，则不能修改，Buffer 实际上是对底层内存的直接操作。

不推荐使用 `new Buffer()` 创建，这种方式创建的 buffer 内容未知，可能包含敏感数据。

## Buffer 方法

- `Buffer.from(str)`：将一个字符串转换为 Buffer 数组。

```js
let buf = Buffer.from('孙悟空')
console.log(buf) // <Buffer e5 ad 99 e6 82 9f e7 a9 ba>
```

- `Buffer.alloc(size)`：创建一个指定大小的 Buffer 数组。

```js
let buf = Buffer.alloc(10)
console.log(buf) // <Buffer 00 00 00 00 00 00 00 00 00 00>
```

- `Buffer.allocUnsafe(size)`：创建一个指定大小的 Buffer 数组，但是可能包含敏感数据。

```js
let buf = Buffer.allocUnsafe(10)
console.log(buf) // <Buffer ff ff ff ff 0a 00 00 00 ff ff>
```

- `bufArr.toString()`：将缓冲区中的数据转换为字符串。

```js
let buf = Buffer.from('孙悟空')
buf.toString() // 孙悟空
```

- `bufArr.copy(target, [targetStart [,sourceStart [,sourceEnd]]])`：复制 buffer。  
target：复制数据要存入的 buffer 对象。  
targetStart：target 开始复制的偏移量。  
sourceStart：bufArr 开始复制的偏移量。  
sourceEnd：bufArr 停止复制的偏移量（不包括停止偏移量的那个元素）。

```js
const a = Buffer.from('abcd')
const b = Buffer.from('ef')

b.copy(a, 1)
console.log(a.toString()) // aefd
```

- `bufArr.slice([start [,end]])`：对 buffer 进行切片。  
start：开始偏移量。  
end：结束偏移量。

```js
const bufArr = Buffer.from('123456')
const bufArrSlice = bufArr.slice(1, 3)
console.log(bufArrSlice.toString()) // 23
```

- `Buffer.concat([bufArr1, bufArr2...], totalLength)`：拼接 buffer。  
[bufArr1, bufArr2...]：多个 buffer 对象数组。  
totalLength：设置拼接出新的 buffer 对象的长度，超出会被截取。

```js
const a = Buffer.from('123')
const b = Buffer.from('456')

const result = Buffer.concat([a, b])
console.log(result.toString()) // 123456
```
