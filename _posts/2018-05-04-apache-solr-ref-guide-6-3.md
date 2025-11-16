---
layout: post
title: apache-solr-ref-guide-6.3
date: 2018-05-04 11:50:10
tags: [solr]
---

[solr 用户手册](http://archive.apache.org/dist/lucene/solr/ref-guide/apache-solr-ref-guide-6.3.pdf)笔记

<!-- more -->

### solr.xml 的格式

#### 定义 solr.xml

默认的 solr.xml 长这样：

```xml
<solr>
  <solrcloud>
    <str name="host">${host:}</str>
    <int name="hostPort">${jetty.port:8983}</int>
    <str name="hostContext">${hostContext:solr}</str>
    <int name="zkClientTimeout">${zkClientTimeout:15000}</int>
    <bool name="genericCoreNodeNames">${genericCoreNodeNames:true}</bool>
  </solrcloud>
  <shardHandlerFactory name="shardHandlerFactory" class="HttpShardHandlerFactory">
    <int name="socketTimeout">${socketTimeout:0}</int>
    <int name="connTimeout">${connTimeout:0}</int>
  </shardHandlerFactory>
</solr
```

存在 `<solrcloud>` 元素不意味着 solr 会在 solrcloud 的模式下运行。除非在启动的时候指定了 `-DzkHost` 或者 `-DzkRun`，否则这部分的配置将会被忽略。

## 配置良好的 Solr 实例

### 配置 solrconfig.xml

`solrconfig.xml` 在 `conf` 文件夹下。带有注释的示例配置文件在 `server/solr/configsets` 文件夹下。可以通过创建一个 `configoverlay.json` 文件来重写 `solrconfig.xml` 中的配置。

#### 替换 solr 配置文件中的属性

solr 支持在配置文件中使用**变量替代**方式来设置变量的值。

语法为：`${属性名[:默认属性值]}`

如果没有指定默认值，那么必须在启动的时候指定，否则在解析配置文件时将会报错。



