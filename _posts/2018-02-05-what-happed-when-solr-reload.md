---
layout: post
title: Solr Reload发生了什么
date: 2018-02-05 11:36:21
tags: [solr]
---

> 翻译自：https://www.quora.com/What-happens-during-a-Solr-Core-Reload

core reload 不会导致正在查询的请求失败。但是这个不叫做沙盒，因为它的目标是维持可用性，而不是进行隔离。

> Solr究竟是怎样来维持旧core与新core的沙盒的？

当一个 core 进行 reload 时，一个新的 core 会使用新的配置文件进行创建，一个新的 IndexWriter 会被打开，一个新的搜索器会被创建。新 core 会用旧 core 的名字重新注册。
因此一个正在进行中的请求有一个引用指向 core，会继续进行处理并且不会被中断，在 reload 之后，所有被接收到的请求将会使用新 core 的引用。

对于底层来说有点复杂，因为同一个索引你不能同时拥有两个 IndexWriter。
Solr/Lucene 有索引锁来防止类似场景出现，或者防止索引被损坏。
Solr 会等待当前请求中的 IndexWriter 被释放，当发现 IndexWriter 没有被使用时，将会被 close 掉（并提交所有没有被提交的文档）并使用新的配置文件打开一个新的 writer 用于处理正在进行中的请求。所有的这些都需要非常小心的同步，大部分场景都发生在 SolrCoreState 这个类中。这也意味着新 writer 的配置将用于在新 writer 打开与新core 注册这段时间内接收所有的请求。

然后还有一些其他的情况，例如，如果 Solr 是在 NRT（近实时）的模式下，而你在使用 SolrCloud，那么在 reload 的时候将会一直使用旧的 IndexWriter 创建一个新的搜索器，因为我们想要一个搜索器有所有已经写入的甚至没有被提交的数据。

> 所有的 core（旧的与新的）都是运行在同一个 JVM 吗？

是的，所有的 core 都是同一个 JVM 进程的一部分。一个 core 只是一个执行请求的对象。reload 只是将一个旧的 core 替换为 新的 core。

> 如果他们运行在同一个 JVM 中，那么有可能在 reload 的时候导致 Solr 崩溃，那么沙箱的目的就失败了

正如我之前所说，这里不存在沙箱这种说法，一个新的写请求能够使用新 writer 的配置，尽管 core 的 reload 操作在那个时候还没有完全完成。这不是一个安全特性，你不应该从这个角度去思考。

> 一个 core 在 reload 时的具体的步骤是什么？是否通过 inform() 调用所有 ResourceLoaderAware 的组件？

之前我已经说过具体的步骤。当进行 reload 时，会在所有的资源组件上调用通知方法。