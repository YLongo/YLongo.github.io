---
layout: post
title: 近实时搜索
date: 2017-11-03 19:10
tags: [solr]
---

> 来源：solr-guide-6.3：Near Real Time Searching

<!-- more -->

近实时（`NRT`）意味着文档在索引完之后能够被马上搜索到：增加和更新文档能够接近实时。有提交操作的时候 `Solr` 不会阻塞更新。也不会等待后台合并完成之前重新打开一个搜索器并返回。
你可以将 `commit` 命令改为**软提交**，这可以避免标准提交部分昂贵的成本。你可能想通过标准提交确保文档被存储到硬盘上，同时通过**软提交**看到接近实时的数据。但是要特别注意**缓存**跟**自动预热**的设置会对 `NRT` 的性能产生很大的影响。

##### （提交与优化）Commits and Optimizing
提交操作能够让更新后的索引对新的请求可见。**硬提交**通过使用事务日志来获取最新改动文档的 `id`，同时调用 `fsync` 确保索引文件能够被刷新到磁盘，而不会因为断电而丢失。
**软提交**相对更快一点，因为它仅仅是让改变后的索引可见，并不会刷新索引到磁盘，或者重新写入一个新的索引块。如果 `JVM` 崩溃或者断电，上一次**硬提交**之后的数据将会丢失。具有 `NRT` （索引改变后能够快速被搜索到）属性的搜索集，具有频繁的软提交，较少的硬提交。软提交在时间上占有一定的优势，但是它会影响吞吐率。
**`优化`**类似一次**硬提交**，除了它刚开始的时候会要求所有的索引段合并成一个索引段。视使用情况而定，这个操作应该很少被执行（例如：每天晚上），因为这个操作一旦进行，它会读取并且重写整个索引文件。索引段会随着时间的推移而合并（视合并策略而定），但是优化操作会要求合并立即执行。

使用软提交的两个参数：`maxDocs`、`maxTime`
参数|描述|
-|-|
`maxDocs`|整数。在写入索引之前，队列里面可以有多少个文档。它结合参数 `update_handler_autosoftcommit_max_time` 来设置，当达到这个条件时，队列中的文档会被写入索引|
`maxTime`|在写入索引之前，等待的毫秒数。它结合参数 `update_handler_autosoftcommit_max_docs` 来设置，如果达到这个条件，队列中的文档将会被写入索引|
通过 `maxDocs` 、 `maxTime` 这两个参数去调节你的提交策略。

##### 自动提交（AutoCommits）
自动提交也使用 `maxDocs`、`maxTime` 这两个参数。然而在许多策略中通过使用硬提交方式的 `autocommit` 与软提交方式的 `autosoftcommit` 将有更灵活的提交策略。
常见的配置是 `autocommit` 配置 `1~10` 分钟提交一次，`autosoftcommit` 每秒都提交。在这种配置下，新文档可以在一秒之内显示出来，但是如果断电的话，软提交的文档将会丢失，除非进行了硬提交。

例如：
```
<autoSoftCommit>
    <maxTime>1000</maxTime>
</autoSoftCommit>
```
最好是使用 `maxTime` 而不是 `maxDocs` 来修改 `autoSoftCommit`，特别是在索引大文档时。批量提交索引时最好关闭 `autoSoftCommit`

##### 提交（commit）与优化（optimize）可选的属性
参数|属性值|描述
-|-|
`waitSearcher`|true / false|阻塞直到一个新建的搜索器注册成功，新的搜索器能够让改变后的索引可见，默认为 `true`
`softCommit`|true / false|执行软提交。将会更快的刷新索引的视图，但是不保证索引文档能够被存储到磁盘。默认是 `false`
`expungeDeletes`|true / false|仅仅对 `commit` 有效。从索引段中清理被删除的数据
`maxSegments`|整数|仅仅对 `optimize` 有效。优化后最多能有多少个段

例：
```
<commit waitSearcher="false" />
<commit waitSearcher="false" expungeDeletes="true" />
<optimize waitSearcher="false" />
```

##### 更改 `commitWithin` 的默认行为
`commitWithin` 允许在一定时间范围内去强制更新文档。这个参数在近实时搜索中经常用到，因为这个原因，所以它的默认执行软提交。然而，在主从模式中，不会将新文档复制到从节点中。如果对于你的实现，这个是必须的，你可以强制进行硬提交通过如下方式：
```
<commitWithin>
    <softCommit>false</softCommit>
</commitWithin>
```
你可以将这个配置作为你更新信息的一部分，那么每次都会自动进行硬提交





