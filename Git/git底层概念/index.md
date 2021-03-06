# git底层概念

## 基础的 linux 命令

`git` 窗口实际输入的就是 `linux` 命令。

- `clear`：清除命令窗口的信息。

- `echo 'xxx'`：往控制台输出信息。

- `echo 'xxx' > fileName`：输出内容到文件并创建。

- `ll`：将当前文件下的子文件和子目录平铺在控制台。

- `find directoryName`：将对应目录下的子孙文件和子孙目录平铺在控制台。

- `find directoryName -type f`：将对应目录下的文件平铺在控制台。

- `rm fileName`：删除文件。

- `mv 源文件 重命名文件`：重命名。

- `cat 文件的url`：查看对应文件的内容。

- `vim 文件的url（在英文环境下）`：  
按 `i` 进入插入模式，进行文件的编辑。  
按 `esc` 键&按 `:` 键，进行命令的执行：  
&emsp;&emsp;`q!` --> 强制退出（不保存）。  
&emsp;&emsp;`wq` --> 保存退出。  
&emsp;&emsp;`set nu` --> 设置行号。

## .git 目录

通过 `git init` 初始化会生成 `.git` 隐藏目录，该目录包含以下信息：

- `hooks`：目录包含客户端或服务端的钩子脚本。

- `info`：包含一个全局性排除文件。

- `logs`：保存日志信息。

- `objects`：目录存储所有的数据内容。

- `refs`：目录存储指向数据的提交对象的指针（分支）。

- `config`：文件包含项目特有的配置选项。

- `description`：用来显示对仓库的描述信息。

- `HEAD`：文件指示目前被检出的分支。

- `index`：文件保存暂存区信息。

## git 对象（blob）

`git` 的核心部分是一个简单的键值对数据库。

你可以向该数据库插入任意类型的内容，它会返回一个键值，通过该键值可以在任意时刻再次检索该内容。

****

向数据库写入内容，并返回对应键值：

```bash
echo 'xxx' | git hash-object -w --stdin
# -w：选项提示 hash-object 命令存储数据对象，若不指定此选项，则该命令仅返回对应的键值
# --stdin(standard input)：选项则指示该命令从标准输入读取内容，若不指定此选项，则需在命令尾部给出待存储文件的路径
```

```bash
git hash-object -w filePath
# 存文件
```

```bash
git hash-object filePath
# 返回对应文件的键值
# 长度为 40 的校验和（SHA-1 哈希值）
```

****

查看 `git` 是如何存储数据的：

```bash
find .git/objects -type f
# 返回 .git/objects/xx/xxx...(38位)
```

这就是开始时 `git` 存储内容的方式：一个文件对应一条内容，校验和的前两个字符用于命名子目录，余下的 38 个字符则用作文件名。

****

根据键值拉取数据：

```bash
git cat-file -p 校验和
# -p：此选项可指示该命令自动判断内容的类型，并为我们显示格式友好的内容
# 返回对应文件的内容
```

```bash
git cat-file -t 校验和
# 返回其内部存储的任何对象类型
```

****

问题：

- 记住文件每一个版本所对应的 `SHA-1` 值并不现实。

- 在 `git` 中，文件名并没有被保存，我们仅保存了文件的内容。

解决方案：树对象。

**注意：当前的操作都是在对本地数据库进行操作，不涉及暂存区。**

## 树对象（tree）

一个树对象包含了一条或多条记录。

每条记录含有一个指向 `git` 对象或子树对象的 `SHA-1` 指针，以及对应的模式、类型、文件名信息。

一个树对象也可以包含另一个树对象

我们可以通过 `update-index`、`write-tree`、`read-tree` 等命令来构建树对象并塞入到暂存区。

### 创建暂存区

利用 `update-index` 命令为文件的首个版本创建一个暂存区。

```bash
gti update-index --add --cacheinfo 100644 [hash] [fileName]
# 文件模式
#   100644：表明这是一个普通对象
#   100755：表明这是一个可执行文件
#   120000：表明这是一个符合链接

# --add 选项：不在暂存区的文件，首次需要 --add
# --cacheinfo 选项：要添加的文件位于 git 数据库中，而不是位于当前目录下，所以需要 --cacheinfo
```

### 生成树对象

通过 `write-tree` 命令生成树对象。

```bash
git update-index ...
git write-tree
```

### 将一个树对象加入第二个树对象，使其成为新的树对象

`read-tree` 命令，可以把树对象读入暂存区。

```bash
git read-tree --prefix=bak [treeHash]
git write-tree
```

### 查看暂存区当前的样子

```bash
git ls-files -s
```

### 总结

在 `git` 中每一个文件（数据）都对应一个 `hash(blob 类型)`。

每一个树对象都对应一个 `hash(tree 类型)`。

我们可以认为树对象就是我们项目的快照。

## 提交对象（commit）

我们可以通过调用 `commit-tree` 命令创建一个提交对象，为此需要指定一个树对象的 `SHA-1` 值，以及该提交的父提交对象（如果有的话，第一次将暂存区做快照就没有父对象）。

创建提交对象：

```bash
echo 'xxx' | git commot-tree [treeHash]
# 返回 commitHash
```

查看提交对象：

```bash
git cat-file -p [commitHash]
# 返回：
# tree ...
# parent ...
# author ...
# committer ...
# 提交日志
```

**注意：git commit-tree 不但生成提交对象，而且会将对应的快照（树对象）提交到本地库中。**
