# git结合Eslint

## 安装使用 Eslint

```bash
npm i eslint -D
# 本地安装
```

设置 `package.json` 文件。

```json
// ...
"scripts": {
  "lint": "eslint src", // 校验代码的程序，自动校验 src 目录下所有的 .js 文件
  "lint:create": "eslint --init" // 生成 .eslintrc.js 文件，提供编码规则
}
// ...
```

```bash
npm run lint:create
# 生成 .eslintrc.js 文件

npm run lint
# 检查 src 目录下的文件
```

`--fix`：是 `eslint` 提供的自动修复基础错误的功能，只能修复基础的不影响代码逻辑的错误。

```json
// ...
"scripts": {
  "lint": "eslint src --fix"
}
// ...
```
