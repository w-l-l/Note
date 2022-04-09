# styled_components相关

安装依赖。

```js
npm i styled-components -S
```

它是通过 js 改变 css 编写方式的解决方案之一，从根本上解决常规 css 编写的一些弊端。

通过 js 来为 css 赋能，我们能达到常规 css 所不好处理的逻辑复杂、函数方法、复用、避免干扰。

样式书写将直接依附在 jsx 上面，HTML、CSS、JS 三者再次内聚。`all in js` 的思想。

## 基本使用

```js
import styled from 'styled-components'

const StyledDiv = styled.div({
  backgroundColor: 'red',
  width: 100,
  height: 100,
  '&:hover': {
    backgroundColor: 'blue'
  }
})
// const StyledDiv = styled.div`
//   background-color: red;
//   width: 100px;
//   height: 100px;
//   &:hover {
//     background-color: blue;
//   }
// `
// 支持对象和字符串两种写法

export default function App() {
  return <StyledDiv />
}
```

## 透传 props

对标签设置样式，同时支持原生属性传递。

```js
import styled from 'styled-components'

const StyledInput = styled.input({
  color: 'red',
  borderRadius: 8
})

export default function App() {
  return <StyledInput type='text' placeholder='请输入' />
}
```
