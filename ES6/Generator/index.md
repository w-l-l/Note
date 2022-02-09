# Generator

ES6 提供的解决异步编程的方案之一。

Generator 函数是一个状态机，内部封装了不同状态的数据。

用来生成遍历器对象（iterator）。

可暂停函数（惰性求值），`yield` 可暂停，`next` 方法可启动，每次返回的是 yield 后的表达式结果。
