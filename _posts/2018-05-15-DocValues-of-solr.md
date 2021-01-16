---
layout: post
title: solr 之 DocValues
date: 2018-05-15 9:29
---

>   来源：solr-guide-6.3：DocValues

DocValues 是一种通过记录内部值的方法，在某些场景下比传统的索引更有效，例如：sorting、faceting。

<!-- more -->

### 为什么需要 DocValues

solr 构建索引的标准方法是倒排索引。这种方式是通过构建在所有文档中出现过的词的列表，每一个词跟出现这个词的文档列表相关联（包括这个词在某个文档中出现的次数）。这种形式可以让查询飞快，因为当用户搜索词的时候，词跟文档的关联关系已经存在。

但是，对于 sorting、faceting、highting 则不是那么的高效。举个例子，faceting 引擎会在每个文档中查找每个词，然后组装结果列表，并将文档 id 构建 facet 列表。在 solr 里面，这些操作都是在内存中进行，但是在加载数据的时候是会变慢的（根据文档、词的数量等等）。

在 Lucene 4.0 中，介绍了一个种新的方法。DocValue 字段是面向列的，在索引期间就建立了文档到值的对应关系。这种方式可以减少 fieldCache 对内存的的依赖，可以让 faceting、sorting、grouping 快很多。

### 使用 DocValues

你需要在字段或者字段类型定义的时候添加 `docValues="true"`。在 solr `sample_techproducts_configs` 中的 `schemal.xml` 有如下的例子：

```xml
<field name="manu_exact" type="string" indexed="false" stored="false" docValues="true" />
```

>   如果你已经将数据索引到 solr 中了，你需要在更改了字段的定义后重新索引才可以使用 docValues

DocValues 只对特定的字段类型有效。指定的字段将决定 Lucene 底层使用 docValue 使用什么类型。有效的 solr 字段类型如下：

-   `StrField` 与 `UUIDField`
    -   如果字段是单值字段（multiValued=false），Lucene 将会使用 **SORTED** 类型
    -   如果字段是多值字段，Lucene 将会使用 **SORTED_SET** 类型
-   任何 `Trie*` 数字字段、date 字段以及 `EnumField`
    -   如果字段是单值字段（multiValued=false），Lucene 将会使用 **NUMERIC** 类型
    -   如果字段是多值字段，Lucene 将会使用 **SORTED_SET** 类型

这些 Lucene 字段跟值如何排序以及存储有关。

### 检索 DocValues 字段值

搜索的时候，字段值的检索通常是返回存储的值。然而，没有存储的 docValues 字段也可以随着其它的字段一起返回（当指定该字段返回或者全部字段返回的时候）。这是因为对于全部字段都生效的参数 `useDocValuesAsStored`。对于版本 >= 1.6 的 schema，默认 `useDocValuesAsStored=true`。

当 `useDocValuesAsStored="false"` 时，对于 `stored="false"` 的 docValues 字段，在查询的时候如果指定了字段的名字，那么该字段可以返回，但是对于通配符（"*"）则不会返回。

注意，在查询的时候，docValues 跟通常的 stored 字段返回会有性能的影响。但是 stored 字段不会，因为 docValues 字段是面向列的，因此会有额外的开销去检索每个文档。而且在返回具有多值的 docValues 字段时，返回的是排序后的值（而不是插入顺序的值）。如果想要多值字段返回原始插入顺序的值，你需要让多值字段 `stored="true"`（做的这些更改都需要重新索引）。

如果查询的时候返回的字段只有 docValues 属性，性能可能会有所提高，因为存储字段需要读取磁盘并且解压缩。然而在 fl 返回 docValues 字段只需要操作内存就可以了。

当以 docValues 的形式检索字段时，与传统的存储字段相比有两个重要的不同：

1. 顺序不是预先设定好的。仅仅检索存储字段，值插入的顺序就是返回的顺序。但是对于 docValues 字段，是排序后的顺序。
2. 多个相同的值将会被合并成一个值。如果你插入的值是 4, 5, 2, 4, 1，那么返回将是 1, 2, 4, 5。





