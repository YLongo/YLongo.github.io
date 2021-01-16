---
title: algs4 notebook
date: 2018-04-27 15:52:46
---

算法 第四版 笔记

<!-- more -->

使用 Scanner 进行标准输入时，告诉控制台结束输入的快捷键为：

>   Windows cmd: Ctrl + m
>
>   Eclipse : Ctrl + z



#### 1.1.9.5 重定向与管道

```java
java RandomSeq 1000 100.0 200.0 > data.txt
```

>   在终端窗口输入这个命令，终端窗口不会输出任何信息，所有的信息都会被输出到 data.txt 中

```java
java Average < data.txt
```

>   读取 data.txt 中的数据作为输入

```
java RandomSeq 1000 100.0 200.0 | java Average
```

>   将 RandonSeq 输出的数据作为 Average 数据的输入



