---
title: Rust - 什么时候该使用macro(宏)/function(函数)
date: 2022-10-30T15:07:33+08:00
tags: [Rust, Development, Learning]
categories: [Learning]
---

## rust中 `macro` 与 C/C++ 中 `macro` 的区别

大致可以把rust中的宏看作是一种完善了C语言中宏种种弊端又添加了一些新特性的实现. 

C语言中的宏是一种比复制粘贴稍微高明但有没那么高明的东西, 例如常见的 `#include` 宏将对应的文件递归复制并展开到当前文件, `#define` 宏将对应的占位符替换成对应的文本等等等. 这种设计颇有用 `sleep` 模拟异步的既视感, 其定义并没有一门编程语言那么精确. 

这种简陋的设计也会带来很多与人第一感觉不同的结果, 例如最经典的[非预期宏](https://doc.rust-lang.org/1.30.0/book/first-edition/macros.html#hygiene)例子: 

```c
#define FIVE_TIMES(x) 5 * x

int main() {
    printf("%d\n", FIVE_TIMES(2 + 3));
    return 0;
}
```

由于宏只是简单替换, 上述语句会变成 `5 * 2 + 3`, 最后结果是 `13` 而并不是期望的 `25`. 

Rust的宏做了很多改进, 一个最经典的形容是 *hygenic* (干净的). 这一节在[Rust的文档](https://doc.rust-lang.org/1.30.0/book/first-edition/macros.html#hygiene)中就有单独介绍. 在设计上, 文档里有这么一句话: 

> Each macro expansion happens in a distinct ‘syntax context’, and each variable is tagged with the syntax context where it was introduced.

如果将上面的C语言代码用rust宏重新实现, 那么在宏调用中的语句 `2 + 3` 会被当成一个单独的表达式来对待, 而不仅仅只是替换. 

除此之外, rust的宏可以和编译之后的object一起分发 (C语言的宏预处理完就没了) , 还支持 [Attribute Like](https://doc.rust-lang.org/book/ch19-06-macros.html#attribute-like-macros)、 [Function Like](https://doc.rust-lang.org/book/ch19-06-macros.html#function-like-macros) 等多种使用方式和特性, 也支持可变参数列表 (这个在Rust的function里还不支持) . 

## `macro` 与 `function` 的区别

与 C/C++ 一样, `macro` 是在编译期进行检查和展开的. 

但是Rust利用编译期展开这个特性, 针对一些语言设计上的不安全隐患进行了改善. 例如pwn👴闭着眼睛都能利用的格式化字符串漏洞, 是由于 `printf` 函数是在运行时进行字符串构建并且并没有对参数进行严格检查导致的. Rust设计的 `print!` 宏就可以避免这一点. 宏通过编译时展开, 在编译期就可以得知参数数量是否匹配、变量是否有效、第一个参数是否是合法字符串字面值等等, 然后就可以直接解决格式化字符串不安全的问题. 

而 `function` 的概念就和其他语言差不多了, 但是 Rust 的函数限制稍多, 不支持可变参数、不支持函数重载 (运算符是可以重载的) 、不支持给参数设置默认值等等等…… 而官方给出的理由是通过 Rust 编写程序的时候即使不使用这些奇怪的、可能导致不安全行为的特性, 也能够以另一种优雅的方式实现需求. 

`macro` 与 `function` 的一个主要区别就是, `macro` 不进行参数~~评估~~*求值* (Evaluate) ——或者说直到代码中使用时才会对参数进行*求值* (Evaluate) . 

这里的 “求值” 稍稍抽象, 可以这么解释: 

比如说你调用函数的时候这么写: `hello(world(123))`, 程序会先对 `world(123)` 进行求值, 然后再将结果代入 `hello` 函数进行下一步处理. 这里先求值 `world(123)` 即为 *求值 (Evaluate)*. 而在 `macro` 中, 参数是作为一个表达式实体传进去的, 在宏中的代码调用之前, 这个表达式并不会被执行. 

## 什么时候使用 `macro`, 什么时候使用 `function`？

通过上面的比较, 我们可以发现 `macro` 最突出的特点就是不会对传入的参数做过多的限制, 传入的表达式也不会被预先执行求值. 

先说情景所限, 必须要使用 `macro` 的情况, 这里直接拿官方 `assert!` 宏作为一个例子: 

```rust
#[macro_export]
#[stable(feature = "rust1", since = "1.0.0")]
macro_rules! assert {
    ($cond:expr) => (
        if !$cond {
            panic!(concat!("assertion failed: ", stringify!($cond)))
        }
    );
    ($cond:expr, $($arg:tt)+) => (
        if !$cond {
            panic!($($arg)+)
        }
    );
}
```

如果这个宏写成函数的话, 大概长下面这个样子: 

```rust
fn assert(cond: bool, expr: ?) {
    ...
}
```

显然 `expr` 是没有一个合适类型声明的, 同时 `expr` 在传入之前就会被执行求值, 传进去的仅仅只是一个值而不是表达式, 我们就没办法实现宏中 `stringify!($cond)` 这样的功能. 

除此之外, 一些类似 `printf` 之类需要进行不安全构造操作或者需要可变函数参数长度的功能也可以抽象成宏来使用. 

使用宏还有一个好处, 就是宏中的部分计算都可以在编译期完成, 这样就可以实现一些类似于编译器打表的操作, 可以显著缩减运行时间. 

一般的经验法则是, 在函数不能提供所需解决方案的情况下, 在你有相当重复的代码的情况下, 或者在你需要检查类型的结构并在编译时生成代码的情况下, 可以使用宏. 在能够使用函数的场景下还是要尽量使用函数. 

宏由于对参数不做限制, 滥用可能会导致项目代码以后会很难以维护, 项目的可读性也会成倍增加. 另外, 宏是在编译期展开的, 也就意味着会有非常多的重复代码片段在展开时被插入到代码中, 这可能会影响CPU指令缓存, 从而导致性能下降. 

## 参考

[What is the difference between macros and functions in Rust? - StackOverflow](https://stackoverflow.com/questions/29871967/what-is-the-difference-between-macros-and-functions-in-rust)

[Why macros vs. functions? - Rust User Forum](https://users.rust-lang.org/t/newbie-why-macros-vs-functions/1012)

[ch19.06 macros - The Rust Programming Language](https://doc.rust-lang.org/book/ch19-06-macros.html)
