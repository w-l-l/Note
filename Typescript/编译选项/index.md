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
