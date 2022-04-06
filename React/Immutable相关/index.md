# Immutable相关

`immutable` 是一种持久化数据。一旦被创建就不会被修改。修改 `immutable` 对象的时候返回新的 `immutable`。但是原数据不会改变。

`immutable` 优化性能的方式：`immutable` 实现的原理是 `Persistent Data Structure`（持久化数据结构），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不可变。同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，`immutable` 使用了 `Structural Sharing`（结构共享），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其他节点则进行共享。

![immutable修改流程图](./img/immutable_process.gif)

`immutable` 的不可变性让纯函数更强大，每次都返回新的 `immutable` 的特性让程序员可以对其进行链式操作，用起来更方便。
