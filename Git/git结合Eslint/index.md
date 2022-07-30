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

## 跳过 eslint 校验

- `eslint-disable-line`：禁用单行检查。

```js
const a = 'xx' // eslint-disable-line
```

- `eslint-disable`：禁用多行检查。

```js
/*eslint-disable*/
  xxx
  xxx
/*eslint-disable*/
```

## git 与 eslint 结合

在没有通过 `eslint` 校验成功的情况下，禁止提交。

```bash
npm i husky -D
# 安装 husky，为 git 仓库设置钩子程序
# 在仓库初始化之后，再去安装 husky
```

在 `package.json` 文件写配置。

```json
{
  // ...
  "husky": {
    "hooks": {
      "pre-commit": "npm run lint"
      // 在 git commit 之前一定要通过 npm run lint 的检查
      // 只有检查通过时，git commit 才能真正的执行
    }
  }
  // ...
}
```
