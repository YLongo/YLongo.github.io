---
layout: post
title: Solr 4企业搜索服务
date: 2018-03-16 10:34:34
tags: [solr]
---



Solr4 企业搜索服务

<!-- more -->

**准备工作**

-   Java 8 或 Java 7
-   Solr 5.0



# 第一章：快速上手

**Solr 4 与 Solr 5 之间的不同**

Solr 5 最大的变化在于可以通过自己的服务进程来部署。不再需要将 WAR 包部署到 Tomcat 或者 Jetty 容器里面。换句话说：你不会在 Servlet 容器中部署 MySQL 数据库，也就不需要部署搜索引擎。

### 启动 Solr

Solr 自带了许多的例子。我们将运行其中一个名叫 *techproducts* 的例子。这个例子将会创建一个 collection 并插入一些样本数据。

> 注：在这本书里面 collection 与 core 的概念是一样的，只是名字不同。

> Solr 5 增加了启动脚本来启动 Solr。在之前的版本中，你需要通过 `java -jar start.jar` 来启动。

Windows 通过 `solr.cmd` 来启动，*nix 系统通过 `solr` 来启动。
> 该笔记只展示 Windows 下的命令

启动命令如下：

```bash
cd bin
./solr.cmd start -e techproducts
```

停止命令如下：

```bash
./solr.cmd stop
```

### 加载样本数据

Solr 的样本数据存放在 `example/exampledocs` 文件夹下。在我们启动的时候，这些数据有一部分作为 `techproducts` 这个 core 的一部分。

我们可以通过 `post.jar`（官方的名字为 SimplePostTool） 来重新索引这部分样本数据。执行这个 jar 包时需要通过 Java 的系统变量来指定一个 collection：`-Dc=techproducts`

命令如下：

```bash
cd example/exampledocs
java -Dc=techproducts -jar post.jar *.xml
```

### 一个简单查询

在 **q** 查询框输入 `lcq`，然后点击查询。

返回值由两部分组成：`responseHeader`、`redponse`

```json
"responseHeader": {
    "status": 0,
    "QTime": 1,
    "params": {
        "q": "lcd",
        "indent": "true",
        "wt": "json"
    }
}
```

`status`：0 正常，其它不正常

`QTime`：solr 处理查询的时间，单位为毫秒。但不包括请求返回的时间。由于多层缓存框架的存在，大多数查询都在毫秒级，如果经常执行同一个查询，那么查询时间会更少。

### 样本数据浏览界面

Solr 提供了一个快速浏览界面：` http://localhost:8983/solr/techproducts/browse`。可以用来探索一下特性：

-   **Standard keyword search**：探索 solr 的语法规则
-   **Query debugging**：查看查询语句的解析，以及返回结果分数的解释
-   **Query-suggest**：搜索建议
-   **Highlighting**：高亮显示查询的关键字
-   **More-like-this**：返回相似的关键字
-   **Faceting**：切面查询
-   **Clustering**：分类查询
-   **Query boosting**：通过 product price 来影响查询的分数
-   **Geospatial search**：通过距离来过滤。点击左上方 **spatial** 可以看到效果

## 配置文件

通过 `-e techproducts` 参数启动 solr，它会加载`server/solr/configsets/sample_techproducts_configs` 下的配置文件。

一个 Solr core 的实例包含以下文件：

-   `conf`：这个文件夹包含配置文件。`solrconfig.xml` 与 `schema.xml` 非常的重要，还有一些 `.txt`、`.xml` 文件被它们引用。
-   `conf/schemal.xml`：这个配置文件跟 index 相关，包括 analyzer 链相关的 field 类型定义。
-   `conf/solrconfig.xml`：Solr 主要的配置文件。
-   `conf/lang`：包含了语言的翻译，其中的 `.txt` 文件被其它组件使用。
-   `conf/xslt`：包含了一些 XSLT 文件，这些文件能够将 Solr 返回的 XML 格式的结果，转变成其它的格式。例如：Atom 和 RSS。
-   `conf/velocity`：包含 HTML 模版以及相关的 web 资源。能够通过 Solritas 加快 UI 原型的设计。
-   `lib`：可以用来放置额外的 JAR 包，Solr 会在启动的时间去加载。但是这个文件夹默认是没有的，需要手动去创建。

>   跟传统的数据库软件不一样的地方在于，传统的数据库软件大部分的配置文件不需要去更改，保持默认值就可以了，但是 Solr 可以随意的更改，尤其是 schema 文件。

# 第二章：设计 schema

## shemal.xml 文件

```xml
<schema name="example" version="1.5">
```

`name` 属性用来设置配置文件的名字，用来区分配置文件。

### field 定义

-   `omitNorms`：如果不想让字段长度影响打分，或者不想使用索引时加权，可以设置为 `true`。除了会影响打分之外，还可以节省一部分内存。对于原始类型 `int`、`float`、`boolean`、`string` 的字段默认为 `true`。

-   `omitPositions`：索引的时候忽略词的位置信息。phrase query 将不可用

-   `omitTermFreqAndPositions`：忽略索引中词的频率以及位置信息。phrase query 将不可用，打分也会受到影响。

-   `termVectors`：对于 MoreLikeThis 以及大文本的 highlighting，可以提高搜索的性能。但是会增加索引的大小以及索引的时间。

-   `positionIncrementGap`：对于 `multiValued` 字段，用于防止跨字段 phrase query 查询。例如：如果 A 与 B 是多值字段的两个值，当 `positionIncrementGap` 的值大于 1 时，会阻止 "A B" 这样的 phrase query 查询。

-   `docValues`：引用 [apache-solr-ref-guide-6.3](https://lucene.apache.org/solr/guide/6_6/docvalues.html)

    >   docValues 通过记录内部字段值的方式来提升在某些情况的搜索效率。例如：排序以及 faceting。
    >
    >   一般情况下，Solr 通过倒排索引的方式来构建索引，这种词与文档对应的方式让搜索效率非常高
    >
    >   但是对于某些场景，例如：sorting、faceting、highlighting，这种方式则不是特别高效。对于 faceting 引擎来说，必须找到 document 中出现过的所有 term，然后组成结果集，以及将这些 document id 构建一个 facet list。虽然这些都是在内存中进行，但是在数据量比较大的情况下，速度还是很慢的。
    >
    >   在 Lucene 4.0，可以使用 DocValues，它是面向列的字段的，在索引期间就建立起 document-value 的映射。可以减轻 fieldCache 对内存的依赖，并且能够让 faceting、sorting、grouping 更快。
    >
    >   例：
    >
    >   ```xml
    >       <field name="manu_exact" type="string" indexed="false" stored="false" docValues="true" />
    >   ```
    


​       

