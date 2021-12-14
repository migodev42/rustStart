# RUSTSTART
## 1 开始
## 2 Guessing Game
## 3 常见的编程概念
## 4 理解 Ownership
<!-- 相关功能点：借用、切片、rust 在内存中的数据分布 -->
### 内存管理

所有的程序在运行时都需要管理内存，一些语言（比如JS,Java）拥有垃圾回收的特性，另一些语言需要自己明确的申请和释放内存（比如C/C++）,而 rust 用了第三种方式：通过在编译时使用 ownership system 来检测内存使用是否符合约束。

Ownership 是 rust 在不使用垃圾回收机制并保持内存安全的特性（零成本抽象）。

> 零成本抽象对于 C++ 和 Rust 是非常重要的特性，有两个关键点：1. 没有运行时开销（即GC）2. 性能：零成本抽象生成的机器代码，手写应无法超越。


> 关于栈内存和堆内存的前置知识：在许多语言中你可能不用太考虑堆内存和栈内存（比如在 JS 中），但在一些系统级的编程语言中是需要的（比如 C/C++，Rust ）。栈内存中的变量会在离开作用域时自动清理，而在堆内存中的变量，需要手动回收或者由垃圾回收机制 GC 回收。

如果没有 GC，我们需要确定内存的使用方式，并显式分配、回收它。
这种手工处理的方式在历史上是个老问题。
- 如果忘记回收内存，就会浪费内存。
- 如果我们回收得太早，就会有一个无效的指针。 
- 如果我们回收了两次，那也是一个错误，因为需要将一个分配与一个回收对应。

而在 Rust 的 Ownership 特性中 ，当 Owner 也离开作用域时，这块变量内存就自动回收了。

### Ownership 规则
- Rust 中的每一个值都有它的 Owner
- 同一时间只能有一个 Owner
- 当离开 Owner 的作用域 Scope 时，这个值会被抛弃

``` rust
{                      // s is not valid here, it’s not yet declared
    let s = "hello";   // s is valid from this point forward

    // do stuff with s
}                      // this scope is now over, and s is no longer valid
```

``` rust
{
    let s2 = String::from("hello"); // s is valid from this point forward

    // do stuff with s
}                                  // this scope is now over, and s is no
                                       // longer valid
```
当调用 `String::from` 时，`Rust` 会帮助我们申请内存。
而当 s2 离开作用域 Scope 时，`Rust` 会帮助我们清理内存。

`C++` 中也有类似的模式，叫做 `Resource Acquisition Is Initialization` `RAII`。更好理解的一个名字叫做 `Scope-Bound Resource Management`，对应了 Rust 的 ”离开了作用域就回收内存” 的设计。

手动清理：
``` C++
RawResourceHandle* handle=createNewResource();
handle->performInvalidOperation();  
// 假设 performInvalidOperation 调用抛出 exception
...
// 走不到这个调用，无法释放 handle，内存泄露了
deleteResource(handle); 
```
`RAII`
``` C++
class ManagedResource {
public:
   ManagedResource(RawResourceHandle* propsHandle) : handle(propsHandle) {};
   ~ManagedResource() {delete handle; }
   ... // omitted operator*, etc
private:
   RawResourceHandle* handle;
};

...

{
  ManagedResourceHandle handle(createNewResource());
  handle->performInvalidOperation();
  
} // 离开scope，在销毁 handle 实例的同时调用析构函数销毁 handle 内存
```


# 参考
- [The Rust Programming Language](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html)
- [Zero Cost Abstractions](https://boats.gitlab.io/blog/post/zero-cost-abstractions/)

