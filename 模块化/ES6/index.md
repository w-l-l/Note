# ES6

1. 定义 package.json 文件。

```json
{
  "name" : "es6-babel-browserify",
  "version" : "1.0.0"
}
```

2. 安装 babel-cli，babel-preset-es2015 和 browserify。

```js
npm install babel-cli browserify -g
npm install babel-preset-es2015 --save-dev
```

3. 定义 .babelrc 文件。

```json
{
  "presets": ["es2015"]
}
```

4. 创建项目结构。

```js
|--js
  |--src
    |--app.js
    |--module1.js
    |--module2.js
    |--module3.js
  |--lib
|--node_modules
|--.babelrc
|--index.html
|--package.json
```

5. 定义 ES6 模块代码。

```js
// module1.js 分别暴露
export function foo() {
  console.log('module1 foo()')
}
export function bar() {
  console.log('module1 bar()')
}
export const DATA_ARR = [1, 2, 3]

// module2.js 统一暴露
let data = 'module2 data'
function foo1() {
  console.log('module2 foo1()' + data)
}
function foo2() {
  console.log('module2 foo2()' + data)
}
export {
  foo1,
  foo2
}

// module3.js
export default {
  name: 'Tom',
  setName(name) {
    this.name = name
  }
}
```

6. 应用入口 app.js。

```js
import { foo, bar } from './module1'
import { DATA_ARR } from './module1'
import { foo1, foo2 } from './module2'
import person from './module3'
import $ from 'jquery'

$('body').css('background', 'red')
foo()
bar()
console.log(DATA_ARR)
foo1()
foo2()
person.setName('JACK')
console.log(person.name)
```

7. 编译。

使用 babel 将 ES6 编译为 ES5 代码。

```js
babel js/src -d js/lib
```

使用 browserify 编译 js。

```js
browserify js/lib/app.js -o js/lib/bundle.js
```

8. 页面引入测试。

```html
<script type="text/javascript" src="js/lib/bundle.js"></script>
```
