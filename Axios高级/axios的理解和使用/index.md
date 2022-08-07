# axios的理解和使用

## axios 的特点

- 基于 `Promise` 的异步 `ajax` 请求库。

- 浏览器端和 `node` 端都可以使用。

- 支持请求拦截器和响应拦截器。

- 支持取消请求。

- 请求和响应的数据转换。

- 批量发出多个请求。

## axios 常用语法

- `axios(config)`：通用/最本质的发任意类型请求的方式。

- `axios(url[, config])`：可以只指定 url 发 get 请求。

- `axios.request(config)`：等同于 `axios(config)`。

- `axios.get(url[, config])`：发 `get` 请求。

- `axios.delete(url[, config])`：发 `delete` 请求。

- `axios.post(url[, data, config])`：发 `post` 请求。

- `axios.put(url[, data, config])`：发 `put` 请求。

****

- `axios.defaults.xxx`：请求的默认全局配置。

- `axios.interceptors.request.use()`：添加请求拦截器。

- `axios.interceptors.response.use()`：添加响应拦截器。

****

- `axios.create([config])`：创建一个新的 `axios`，它没有下面的功能。

****

- `axios.Cancel()`：用于创建取消请求的错误对象。

- `axios.CancelToken()`：用于创建取消请求的 `token` 对象。

- `axios.isCancel()`：是否是一个取消请求的错误。

- `axios.all(promises)`：用于批量执行多个异步请求。

- `axios.spread()`：用来指定接收所有成功数据的回调函数的方法。

## 难点语法理解与使用

**axios.create(config)**

- 根据指定配置创建一个新的 `axios`，也就是每个新的 `axios` 都有自己独立的配置。

- 新 `axios` 只是没有取消请求和批量发送请求的方法，其他所有语法都是一致的。

- 为什么要设计这个语法？  
需求：项目中有部分接口需要的配置与另一部分接口需要的配置不太一样，如何处理？  
解决：创建 2 个新 `axios`，每个都有自己特有的配置，分别应用到不同要求的接口请求中。

```js
const instance = axios.create({
  baseURL: 'http://localhost:3000'
})
instance.get('/xxx')
```

****

**拦截器函数、ajax 函数、请求的回调函数的调用顺序**

- 说明：调用 `axios()` 并不是立即发送 `ajax` 请求，而是需要经历一个较长的流程。

- 流程：请求拦截器2 --> 请求拦截器1 --> 发送 `ajax` 请求 --> 响应拦截器1 --> 响应拦截器2 --> 请求的回调。

- 注意：此流程是通过 `Promise` 串联起来的，请求拦截器传递是的 `config`，响应拦截器传递的是 `response`。

```js
// 添加请求拦截器（回调函数）
axios.interceptors.request.use(
  config => {
    console.log('request1 success')
    return config
  },
  error => {
    console.log('request1 error')
    Promise.reject(error)
  }
)
axios.interceptors.request.use(
  config => {
    console.log('request2 success')
    return config
  },
  error => {
    console.log('request2 error')
    Promise.reject(error)
  }
)
// 添加响应拦截器
axios.interceptors.response.use(
  response => {
    console.log('response1 success')
    return response
  },
  error => {
    console.log('response1 error')
    Promise.reject(error)
  }
)
axios.interceptors.response.use(
  response => {
    console.log('response2 success')
    return response
  },
  error => {
    console.log('response2 error')
    Promise.reject(error)
  }
)
// 发送请求
axios.get('http://localhost:3000/xxx')
.then(response => console.log('data', response.data))
.catch(error => console.log('error', error.message))
```

****

**取消请求**

基本流程：

- 配置 `cancelToken` 对象。

- 缓存用于取消请求的 `cancel` 函数。

- 在后面特定时机调用 `cancel` 函数取消请求。

- 在错误回调中判断如果 `error` 是 `cancel`，做相应处理。

实现功能：在请求一个接口前，取消前面一个未完成的请求。

```js
let cancel // 用于保存取消请求的函数
axios.interceptors.request.use(config => {
  // 在准备发请求前，取消未完成的请求
  if(typeof cancel === 'function') cancel('取消请求')
  config.cancelToken = new axios.CancelToken(e => (cancel = c)) // c 是用于取消当前请求的同步函数
  return config
})
axios.interceptors.response.use(
  response => {
    cancel = null
    return response
  },
  error => {
    // 取消请求的错误
    if(axios.isCancel(error)) {
      console.log('取消请求的错误', error.message)
      return new Promise(() => {})
    } else {
      cancel = null
      return Promise.reject(error)
    }
  }
)
// 执行取消请求的函数
function cancelRequest() {
  typeof cancel === 'function'
    ? cancel('强制取消请求')
    : console.log('没有可取消的请求')
}
```
