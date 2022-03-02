# Mongoose

`MongoDB` 官方有一个 `mongodb` 的包可以用来操作 `MongoDB` 数据库，这个库确实比较强大，但是比较原始，麻烦，所以咱们不使用它。

真正在公司进行开发，使用的是 `mongoose` 这个第三方包。

- 它是基于 `MongoDB` 官方的 `mongodb` 包进一步做了封装。

- 可以提高开发效率，让你操作 `MongoDB` 数据库更方便。

## 使用 Mongoose

安装 `mongoose` 包。

```js
npm i mongoose
```

连接 `MongoDB` 数据库。

```js
const mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/数据库名字')
```

指定连接的数据库不需要存在，当你插入第一条数据之后，就会自动被创建出来。

**注意：前提必须打开 MongoDB数据库**。  

创建一个模型。

```js
// 简易方式
const Cat = mongoose.model('Cat'/* 表名 */, {
  name: String // 字段类型
})
```

一般操作如下：

- 设计文档结构（表结构）。

- 字段名称就是表结构中的属性名称。

- 可以就行约束，约束的目的是为了保证数据的完整性，不要有脏数据。

```js
const Schema = mongoose.Schema
const model = new Schema({
  name: {
    type: String,
    required: true // 必须设置 name 属性
  },
  age: {
    type: Number
  }
}, {
  versionKey: false // 可以避免文档中出现 _v 字段
})
```

将文档结构发布为模型：

```js
const User = mongoose.model('User', model)
/*
  第一个参数：传入一个大写名词单数字符用来表示你的集合名称（表名称）。
    mongoose 会自动将大写名词的字符生成小写复数的集合名称。
    例：User 最终会变为 users。

  第二个参数：架构 Schema。

  返回值：模型构造函数。
*/
```
