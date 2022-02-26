# 其他核心模块

**os**：

主要用来获取机器信息的。

```js
const os = require('os')
```

- `os.cpus()`：获取当前机器的 `cpu` 信息。

- `os.totalmem()`：获取当前机器的内存大小（字节单位）。

**path**：

主要用来操作路径。

```js
const path = require('path')
```

- `path.extname(path)`：获取一个路径中的扩展名部分。

- `path.join(path1, path2...)`：将多个路径连接起来，规范生成新的路径。

- `path.basename(path)`：获取一个路径的文件名（默认包含扩展名）。

- `path.dirname(path)`：获取一个路径中的目录部分。

- `path.parse(path)`：把路径转为一个对象，对象包含以下属性：  
root：根路径。  
dir：目录。  
base：包含后缀名的文件名。  
ext：后缀名。  
name：不包含后缀名的文件名。

- `path.isAbsolute(path)`：判断一个路径是否是绝对路径。

**url**：

主要用来操作 `url`。

```js
const url = require('url')
```

- `url.parse(urlPath, true)`：将路径解析为一个方便操作的 `urlObj` 对象返回。  
第二个参数为 true 表示直接将查询字符串转为一个对象（通过 `query` 属性来访问）。  
urlObj.query：查询字符串的相关内容。  
urlObj.pathname：单独获取不包含查询字符串的路径部分（该路径不包含 `?` 之后的内容）。
