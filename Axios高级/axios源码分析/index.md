# axios源码分析

## 源码目录结构

```bash
├── /dist/                     # 项目输出目录
├── /lib/                      # 项目源码目录
│ ├── /adapters/               # 定义请求的适配器 xhr、http
│ │ ├── http.js                # 实现http适配器(包装http包)
│ │ └── xhr.js                 # 实现xhr适配器(包装xhr对象)
│ ├── /cancel/                 # 定义取消功能
│ ├── /core/                   # 一些核心功能
│ │ ├── Axios.js               # axios的核心主类
│ │ ├── dispatchRequest.js     # 用来调用http请求适配器方法发送请求的函数
│ │ ├── InterceptorManager.js  # 拦截器的管理器
│ │ └── settle.js              # 根据http响应状态，改变Promise的状态
│ ├── /helpers/                # 一些辅助方法
│ ├── axios.js                 # 对外暴露接口
│ ├── defaults.js              # axios的默认配置
│ └── utils.js                 # 公用工具
├── package.json               # 项目信息
├── index.d.ts                 # 配置TypeScript的声明文件
└── index.js                   # 入口文件
```

## axios 与 Axios 的关系

从语法上来说：`axios` 不是 `Axios` 的实例。

从功能上来说：`axios` 是 `Axios` 的实例。

`axios` 是 `Axios.prototype.request` 函数 `bind(Axios 的实例)` 返回的函数。

`axios` 作为对象，有 `Axios` 原型对象上的所有方法（`get()`、`post()`、`put()`、`delete()`），也有 `Axios` 对象上的所有属性（`defaults`、`interceptors`），后面又添加了 `create()`、`CancelToken()`、`all()`。

## axios.create() 与 axios 的区别

相同点：

- 都是一个能发任意请求的函数：`request(config)`。

- 都有发特定请求的各种方法：`get()`、`post()`、`put()`、`delete()`。

- 都有默认配置和拦截器得到属性：`defaults`、`interceptors`。

不同点：

- 默认匹配的值很可能不一样。

- `axios.create()` 创建出来的实例没有 `axios` 后面添加的一些方法：`create()`、`CancelToken()`、`all()`。
