---
title: 更多Redis内部信息：追踪GET和SET（译）
---



> 翻译自：https://www.pauladamsmith.com/blog/2011/03/redis_get_set.html
>
>译注：不遵循一字一句的翻译，不影响理解的可能直接略过，翻译时会加入自己的理解，尽量保证通顺，所以建议直接阅读原文



在[上一篇](https://ylongo.github.io/2023/redis-under-the-hood)文章中，我粗略的介绍了Redis如何启动以及做处理命令的准备。在这篇文章中，我将跟随`GET`以及`SET`命令，从客户端到服务端并返回。`GET`将用于一个不存在的key，`SET`用于设置那个key，然后查看之后的`GET`有何不同之处。

跟之前一样，我通过我的编辑器探索Redis的源码，通过`tags`文件进行索引，在另一个终端让Redis服务端运行在`gdb`之下。

注意：这篇文章是根据Redis 2.2.1版本编写的。关于我的上一篇文章，在我写完了之后Redis发生了哪些变化，查看HN上的[评论](https://news.ycombinator.com/item?id=2301804)

更新：我根据反馈做了两个小的改动 —— Redis的key不是Redis对象，它们都是`sds`字符串；你不必在没有优化的情况下破解Makefile进行编译。



# GET

让我们在Redis上执行`get users:1234`命令。



## 准备工作

如果你在`gdb`下检查某些变量，你可能得不到你想要的。相反你会看到类似"<value temporarily unavailable, due to optimizations>"的信息。这是因为编译器优化了机器码，你想查看的那部分内存从来没被使用过，至少对于被检查的变量来说是这样。为了让跟进代码以及检查更容易点，我们需要确保Redis编译时不使用优化。你可以通过如下的调用编译Redis来做到这一点。

```shell
make noopt
```

或者设置环境变量

```sh
OPTIMIZATION= make
```



## 从客户端发送命令

如果我们查看客户端的`repl`，将会看到它使用`linenoise`库去分割命令的参数。它通过第一个参数去转发。它会检查不需要Redis服务端处理的客户端命令。比如`exit/quit`，`clear`（清空屏幕），以及`connect`（基于host跟port连接指定的Redis服务端，而不是通过默认的127.0.0.1以及6379端口

> 译注：Redis REPL (Read-Eval-Print Loop) 是 Redis 提供的一个交互式命令行工具，它允许用户通过命令行与 Redis 服务器进行交互。通过 REPL，用户可以执行各种 Redis 命令，例如设置和获取键值对，操作数据结构等。要打开 Redis REPL，你需要在终端（或命令行）中输入 `redis-cli` 命令。这将启动 Redis 的命令行接口

任何其他的参数都被看作为要发送给服务端的命令名。剩下的参数都是命令的参数。





