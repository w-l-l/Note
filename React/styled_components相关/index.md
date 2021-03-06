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

## 基于 props 做样式判断

根据自定义属性，设置不同的样式。

```js
import styled from 'styled-components'

interface Props {
  bg?: string
}

// const StyledButton = styled.button<Props>`
//   background-color: ${ props => props.bg || 'blue' }
// `

const StyledButton = styled.button<Props>(props => ({
  backgroundColor: props.bg || 'blue'
}))

export default function App() {
  return (
    <>
      <StyledButton>blue-button</StyledButton>
      <StyledButton bg='red'>red-button</StyledButton>
      <StyledButton bg='yellow'>yellow-button</StyledButton>
    </>
  )
}
```

## 样式化组件

对同一组件设置不同的样式。

```js
import styled from 'styled-components'

interface Props {
  className?: string
}

function Div(props: Props) {
  return <div className={props.className}></div> // 这里必须要写 className 才能生效
}

const StyledDiv1 = styled(Div)({
  backgroundColor: 'red',
  width: 100,
  height: 100
})

const StyledDiv2 = styled(Div)({
  backgroundColor: 'blue',
  width: 200,
  height: 200
})

export default function Child() {
  return (
    <>
      <StyledDiv1 />
      <StyledDiv2 />
    </>
  )
}
```

## 样式继承

类似于 js 中的继承，`styled-components` 也支持样式继承。

```js
import styled from 'styled-components'

const DefaultButton = styled.button({
  backgroundColor: 'blue',
  color: 'white',
  fontSize: 20
})

const SmallButton = styled(DefaultButton)({
  fontSize: 12
})

const BigButton = styled(DefaultButton)({
  fontSize: 30
})

export default function Child() {
  return (
    <>
      <DefaultButton>default-button</DefaultButton>
      <SmallButton>small-button</SmallButton>
      <BigButton>big-button</BigButton>
    </>
  )
}
```

## 实现动画效果

`styled-components` 提供的 `keyframes` 可以让我们定义自己的关键帧动画名称，实现 css 的动画效果。

```js
import styled, { keyframes } from 'styled-components'

const rotate = keyframes({
  '0%': {
    transform: 'rotate(0deg)'
  },
  '50%': {
    transform: 'rotate(270deg)'
  },
  '100%': {
    transform: 'rotate(360deg)'
  }
})

const StyledDiv = styled.div`
  width: 100px;
  height: 100px;
  background: red;
  animation: ${rotate} 1s linear infinite;
`

export default function Child() {
  return <StyledDiv />
}
```
