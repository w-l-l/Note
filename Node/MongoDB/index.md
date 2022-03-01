# MongoDB

`MongoDB` 是一个 `NoSQL` 的数据库，也是一款文档型数据库。

- 可以有多个数据库（database）。

- 一个数据库中可以有多个集合（表 collection）。

- 一个集合中可以有多个文档（表记录 document）。

- 文档结构很灵活，没有任何限制。

`MongoDB` 非常灵活，不需要像 `mysql` 一样先创建数据库、表、设计表结构。

在这里只需要，当你需要插入数据的时候，只需要指定往哪个数据库的哪个集合操作就可以了。

一切由 `MongoDB` 来帮你自动完成建库建表这件事。

## 安装使用 MongoDB

在 `MongoDB` 官网直接下载安装。

安装完成将 `mongoDB/bin` 这个路径添加为环境变量。

根目录创建 data 文件夹，在 data 文件夹中创建 db 文件夹作为 MongoDB 数据库文件的存储文件。

**注意：必须在根目录下创建 data 文件夹**。

cmd 命令行输入 `mongod --version` 显示一下信息就说明安装成功了。

![mongod_version](./img/mongoDB_version.png)

### 启动 MongoDB 数据库（mongod）

`MongoDB` 默认使用执行 `mongod` 命令启动 `MongoDB` 服务器，所处盘符根目录下的 `/data/db` 作为自己的数据库存储目录。

`/data/db` 目录不会主动创建，所以在第一次执行该命令之前先自己手动新建一个 `/data/db`，没有 `/data/db` 这个目录数据库是启动不成功的。

修改默认的数据存储目录（/data/db）：`mongod --dbpath 数据存储目录路径`。

关闭数据库：在命令行直接 `ctrl + c` 即可停止，或者直接关闭开启服务的控制台也可以。

### 连接数据库（mongo）

打开数据库后，命令行输入 `mongo` 就可以连接数据库了。

退出连接：`exit`、`ctrl + c` 或者直接关闭连接数据库的控制台。
