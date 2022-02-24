# File_System文件系统

文件系统简单来说就是通过 node 来操作系统中的文件。

使用文件系统，需要先引入 `fs` 模块，fs 是核心模块，直接引入不需要下载。

```js
const fs = require('fs')
```

## 同步文件的写入

打开文件（在内存中打开的）：  

- `fs.openSync(path, flags[,mode])`  
path：要打开文件的路径。  
flags：打开文件要做的操作类型（默认 w）（r：只读的，w：可写的）。  
mode：设置文件的操作权限，一般不传（默认0o666）。  
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
