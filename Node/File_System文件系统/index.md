# File_System文件系统

文件系统简单来说就是通过 node 来操作系统中的文件。

使用文件系统，需要先引入 `fs` 模块，fs 是核心模块，直接引入不需要下载。

```js
const fs = require('fs')
```

## 同步文件的写入

打开文件（在内存中打开的）：  

- `fs.openSync(path, flags[,mode])`  
path：要打开文件的路径（也可以是绝对路径）。  
flags：打开文件要做的操作类型（默认 w）（r：只读的，w：可写的）。  
mode：设置文件的操作权限，一般不传（默认 0o666）。  
返回值（fd）：该方法会返回一个文件的描述符作为结果，我们可以通过该描述符来对文件进行各种操作。

向文件写入内容：

- `fs.writeSync(fd, string[,position[,encoding]])`  
fd：文件的描述符，需要传递要写入的文件的描述符。  
string：要写入的内容。  
position：写入的起始位置。  
encoding：写入的编码（默认 utf-8）。

保存并关闭文件：

- `fs.closeSync(fd)`  
fd：要关闭的文件描述符。

示例：

```js
const fs = require('fs')
const fd = fs.openSync('./hello.txt', 'w')
fs.writeSync(fd, '要写入的内容')
fs.closeSync(fd)
```

## 异步文件的写入

打开文件：

- `fs.open(path, flag[,mode], callback)`  
callback：异步调用的方法，结果都是通过回调函数的参数返回的。  
回调函数接收两个参数：  
err：错误对象，没有错误则为 `null`。  
fd：文件的描述符。

向文件写入内容：

- `fs.write(fd, string[,position[,encoding]], callback)`  
callback 接收三个参数：  
err：错误对象，没有错误则为 `null`。  
written：实际写入的字节数。  
str：写入的数据。

保存并关闭文件：

- `fs.close(fd, callback)`  
callback 接收一个参数：  
err：错误对象，没有错误则为 `null`。

示例：

```js
const fs = require('fs')
fs.open('./hello.txt', 'w', (err, fd) => {
  if(err) return console.log(err)
  fs.write(fd, '异步写入内容', (err, written, str) => {
    if(!err) console.log('文件写入成功...', written, str)
    fs.close(fd, err => {
      if(!err) console.log('文件关闭...')
    })
  })
})
```

## 简单文件的写入

**同步文件的写入**

- `fs.writeFileSync(file, data[,options])`  
file：要操作的文件路径。  
data：要写入的数据。  
options：3 个选项，可以进行设置。  
encoding：编码格式（默认 utf-8）。  
mode：权限（默认 0o666）。  
flag：打开文件要做的操作类型（默认 w）

```js
const fs = require('fs')
fs.writeFileSync('./hello.txt', '同步简单文件写入')
```

**异步文件写入**

- `fs.writeFile(file, data[,options], callback)`  
callback 接收一个参数：  
err：错误对象，没有错误则为 `null`。

```js
const fs = require('fs')
fs.writeFile('./hello.txt', '异步简单文件写入', err => {
  console.log(err || '文件写入成功...')
})
```

## 流式文件的写入

同步、异步、简单文件的写入都不适合大文件的写入，性能较差，容易导致内存溢出。

创建一个可写流：

- `fs.createWriteStream(path[,options])`：返回一个新的 writeStream 对象。  
path：文件路径。  
options：配置参数。  
flags：'w'，指定用什么模式打开文件。  
encoding：'utf-8'，编码格式。  
fd：null，指定该属性时，createWriteStream 会根据传入的 fd 创建一个流，忽略 path。  
mode：0o666。  
autoClose：true，该属性为 true（默认）时，当发生错误或文件读取结束时会自动关闭文件描述符。

可以通过监听流的 open 和 close 事件来监听流的打开和关闭。

- `on(event, callback)`：为流对象绑定一个事件。

- `once(event, callback)`：为流对象绑定一个一次性事件，该事件将会在触发一次以后自动失效。

常用事件：

- `error`：读取文件发生错误事件。

- `open`：已打开要写入的文件事件。

- `finish`：文件已经写入完成事件。

- `close`：文件关闭事件。

流文件写入：

- `writeStreamObj.write('写入的内容')`

关闭流：

- `writeStreamObj.end()`

示例：

```js
const fs = require('fs')
const ws = fs.createWriteStream('./hello.txt')
ws.once('open', _ => console.log('流打开...'))
ws.once('close', _ => console.log('流关闭...'))
ws.once('finish', _ => console.log('写入完成...'))
ws.once('error', _ => console.log('读取出错...'))

ws.write('第一次写入，')
ws.write('第二次写入，')
ws.write('第三次写入，')
ws.write('第四次写入，')
ws.end()
```

## 简单文件读取

**同步文件读取**

- `fs.readFileSync(path[,options])`：返回一个 buffer 数组。  
path：要读取文件的路径。  
options：读取的选项配置。  
encoding：格式（默认 null）。  
flag：模式（默认 r）。

```js
const fs = require('fs')
const bufArr = fs.readFileSync('./hello.txt')
console.log(bufArr.toString())
```

**异步文件读取**

- `fs.readFile(path[,options], callback)`  
callback：接收两个参数。  
err：错误对象。  
data：读取到的数据，会返回一个 buffer 数组。

```js
const fs = require('fs')
fs.readFile('./hello.txt', (err, data) => {
  if(err) return console.log('读取失败...')
  console.log(data.toString())
})
```

## 流式文件读取

流式文件读取也适用于一些比较大的文件，可以分多次将文件读取到内存中。

创建可读流：

- `fs.createReadStream(path[,option])`：返回一个 ReadStream 对象。  
path：要读取文件的路径。  
options：配置选项。

如果要读取一个可读流中的数据，必须要为可读流绑定一个 `data` 事件，`data` 事件绑定完毕，它会自动开始读取数据。

```js
const fs = require('fs')
const rs = fs.createReadStream('./hello.txt')
rs.on('data', data => {
  console.log(data.toString())
})
```

复制图片：

```js
const fs = require('fs')
const rs = fs.createReadStream('./demo.jpg')
const ws = fs.createWriteStream('./copy.jpg')

rs.on('data', data => ws.write(data))
rs.on('close', _ => ws.end())
```

**流式文件快捷读取**：

- `readStreamObj.pipe(writeStreamObj)`：可以将可读流中的内容，直接输出到可写流中。

```js
const fs = require('fs')
const rs = fs.createReadStream('./demo.jpg')
const ws = fs.createWriteStream('./copy.jpg')
rs.pipe(ws)
```
