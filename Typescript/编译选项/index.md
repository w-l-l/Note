# 编译选项

## 自动编译文件

编译文件时，使用 `-w` 指令后，TS 编译器会自动监视文件的变化，并在文件发生变化时对文件进行重新编译。

```ts
tsc xxx.ts -w
```

## 自动编译整个项目

如果直接使用 `tsc` 指令，则可以自动将当前项目下的所有 ts 文件编译为 js 文件。

但是能直接使用 tsc 命令的前提是，要先在项目目录下创建一个 ts 的配置文件 `tsconfig.json`。

```ts
tsc --init
```

`tsconfig.json` 是一个 `JSON` 文件，添加配置文件后，只需要 `tsc` 命令即可完成对整个项目的编译。

## 配置选项

- `include`：定义希望被编译文件所在的目录，默认值：`["**/*"]`。

```json
{
  "include": ["src/**/*", "tests/**/*"]
  // 只有 src 和 tests 目录下的文件才会编译
}
```

- `exclude`：定义需要排除在外的目录，默认值 `["node_modules", "bower_components", "jspm_packages"]`

```json
{
  "exclude": ["./src/lib/**/*"]
  // src 下 lib 目录下的 ts 文件都不会被编译
}
```

- `extends`：定义被继承的配置文件。

```json
{
  "extends": "./config/base"
  // 当前配置文件中会自动包含 config 目录下 base.json 中的所有配置信息
}
```

- `files`：指定被编译文件的列表，只有需要编译的文件少时才会用到。

```json
{
  "files": ["a.ts", "b.ts", "c.ts"]
  // 列表中的文件都会被 TS 编译器所编译
}
```

## 编译选项

编译选项是配置文件中非常重要也比较复杂的配置选项。

在 `compilerOptions` 中包含多个子选项，用来完成对编译的配置。

```json
{
  "compilerOptions": {
    // ...
  }
}
```

### 项目选项

- `target`：设置 TS 代码编译的目标版本。

```json
{
  "compilerOptions": {
    "target": "ES6"
    // 可选值：ES3（默认）、ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2019、ES2020、ESNext
  }
}
```

- `lib`：指定代码运行时所包含的库（宿主环境）。

```json
{
  "compilerOptions": {
    "lib": ["ES6", "DOM"]
    // 可选值：ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2019、ES2020、ESNext、DOM、WebWorker、ScriptHost...
  }
}
```

- `module`：设置编译后代码使用的模块化系统。

```json
{
  "compilerOptions": {
    "module": "CommonJS"
    // 可选值：CommonJS、UMD、AMD、System、ES2020、ESNext、None
  }
}
```

- `outDir`：编译后文件所在的目录。  
默认情况下，编译后的 js 文件会和 TS 文件位于相同的目录，设置 `outDir` 后可以改变编译后文件的位置。

```json
{
  "compilerOptions": {
    "outDir": "dist"
    // 设置后编译的 js 文件会生成到 dist 目录
  }
}
```

- `outFile`：将所有的文件编译为一个 js 文件。  
默认会将所有的编写在全局作用域中的代码合并为一个 js 文件，如果 `module` 指定为 `Node`、`System` 或 `AMD` 则会将模块一起合并到文件中。

```json
{
  "compilerOptions": {
    "outFile": "dist/app.js"
  }
}
```

- `rootDir`：指定代码的根目录。  
默认情况下编译后文件的目录结构会以最长的公共目录为根目录，通过 `rootDir` 可以手动指定根目录。

```json
{
  "compilerOptions": {
    "rootDir": "./src"
  }
}
```

- `allowJs`：是否对 js 文件进行检查。

```json
{
  "compilerOptions": {
    "allowJs": true // 默认值
  }
}
```

- `checkJs`：是否对 js 文件进行检查。

```json
{
  "compilerOptions": {
    "checkJs": true // 默认值
  }
}
```

- `removeComments`：是否删除注释。

```json
{
  "compilerOptions": {
    "removeComments": false // 默认值
  }
}
```

- `noEmit`：不对代码进行编译。

```json
{
  "compilerOptions": {
    "noEmit": false // 默认值
  }
}
```

- `sourceMap`：是否生成 `sourceMap`。

```json
{
  "compilerOptions": {
    "sourceMap": false // 默认值
  }
}
```

### 严格检查（值为true/false）

- `strict`：启用所有的严格检查，默认值是 `false`，设置后相当于开启了所有的严格检查。

- `alwayStrict`：总是以严格模式对代码进行编译。

- `noImplicitAny`：禁止隐式的 `any` 类型。

- `noImplicitThis`：禁止类型不明确的 `this`。

- `strictBindCallApply`：严格检查 `bind`、`call`、`apply` 的参数列表。

- `strictFunctionTypes`：严格检查函数的类型。

- `strictNullChecks`：严格的空值检查。

- `strictPropertyInitialization`：严格检查属性是否初始化。

### 额外检查（值为true/false）

- `noFallthroughCaseInSwitch`：检查 `switch` 语句包含正确的 `break`。

- `noImplicitReturns`：检查函数没有隐式的返回值。

- `noUnusedLocals`：检查为使用的局部变量。

- `noUnusedParameters`：检查为使用的参数。

### 高级（值为true/false）

- `allowUnreachableCode`：检查不可达代码（永远不会执行的代码）。`true`：忽略不可达代码，`false`：不可达代码将引发错误。

- `noEmitOnError`：有错误的情况下不进行编译。默认值：`false`。
