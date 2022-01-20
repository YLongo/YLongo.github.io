---
layout: post
title: 编译 solr 源码
date: 2018-02-05 12:36:21
---



导入 `solr` 源码到  `eclipse` 

<!-- more -->

### 环境

1. win7
2. jdk 1.8
3. solr 6.3.0
4. ant 1.9.0
5. zookeeper 4.9.0

### 步骤

1. `git clone https://github.com/apache/lucene-solr.git`
2. `git check releases/lucene-solr/6.3.0`
3. `ant ivy-bootstrap`
4. `ant complie`
5. `ant eclipse`
6. 导入 `eclipse` 

### 搭建伪集群

1. 将 `${solr_dir}/solr/server` 复制两份，分别命名为 `server_8984`、`server_8985`  
2. 在 `StartSolrJetty` 添加 `System.setProperty("zkHost", "127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183");`  
3. 复制两份 `StartSolrJetty`，分别命名为 `StartSolrJetty1`、`StartSolrJetty2`
 > 1. `StartSolrJetty1` 增加如下代码：
```
System.setProperty("solr.solr.home", "solr/server_8984/solr");
```
 > 2. `StartSolrJetty2` 增加如下代码：
```
System.setProperty("solr.solr.home", "solr/server_8985/solr");
```
4. 配置 `zookeeper` 集群

5. 通过 `run application` 分别启动 `StartSolrJetty` 类   


> 修改[default-nested-ivy-settings.xml](https://gitee.com/ylongo/solr/blob/master/lucene/default-nested-ivy-settings.xml)文件可以加速构建

