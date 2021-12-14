# RUSTSTART
## 1 开始
## 2 Guessing Game
## 3 常见的编程概念
## 4 理解 Ownership
Ownership 是 rust 在不使用垃圾回收机制的同时保持内存安全的独特特性。
相关功能点：借用、切片、rust 在内存中的数据分布

所有的程序在运行时都需要管理自己是如何使用计算机内存的，一些语言（比如JS,Java）拥有垃圾回收的特性，另一些语言需要自己明确的申请和释放内存（比如C/C++）,而 rust 用了第三种方式：通过在编辑时使用 ownership system 来检测内存使用是否符合约束。

### 栈内存和堆内存
在许多语言中你可能不用太考虑堆内存和栈内存（比如在JS中），但在一些系统级的编程语言中是需要的（比如C/C++，Rust）。

在 Rust 中一个值在堆内存还是栈内存中会对运行产生影响。

堆内存和栈内存都是你的代码在运行时可用的内存，栈内存中的数据一定是明确的，固定大小的，在编译时不确定大小的数据一定要存储在堆内存中
// TODO


### Ownership 规则
- Rust 中的每一个值都有它的 Owner
- 同一时间只能有一个 Owner
- 当离开 Owner 的作用域 Scope 时，这个值会被抛弃

### 变量作用域
``` rust
   {                      // s is not valid here, it’s not yet declared
        let s = "hello";   // s is valid from this point forward

        // do stuff with s
    }                      // this scope is now over, and s is no longer valid
```

### 手动内存管理的问题
如果没有 GC，我们有责任确定何时不再使用内存并调用代码显式回收它，就像我们分配它一样。这在历史上一直是一个困难的编程问题。 如果我们忘记了，我们就会浪费记忆。 如果我们做得太早，我们就会有一个无效的变量。 如果我们做两次，那也是一个错误。 我们需要将一个分配与一个回收配对。

Rust 的方式则不太一样，当 Owner 也离开作用域时，这块变量内存就自动回收了

// TODO


# 参考
- [The Rust Programming Language](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html)