---
layout: post
title: Java编程的逻辑
date: 2018-05-07 23:11:38
tags: [java]
---

《Java 编程的逻辑》笔记

<!-- more -->

### 8.1.5 类型参数的限定

Java 支持限定这个参数的一个上界。参数必须为给定的上界类型或其子类型，这个限定是通过 `extends` 关键字来表示的。

1. 上界为某个具体类

   ```java
   public class NumberPair<U extends Number>, V extends Number> extends Pair<U, V> {
       public NumberPair(U first, V second) {
           super(first, second);
       }
   }
   ```

2. 上界为某个接口

   例如：限定类型必须实现 Comparable 接口。

   ```java
   public static <T extends Comparable<T>> T max(T[] arr) {
       T max = arr[0];
       for (int i = 0; i < arr.length; i++) {
           if (arr[i].compareTo(max) > 0) {
               max = arr[i];
           }
       }
       return max;
   }
   ```

3. 上界为其它类型参数

   ```java
   public <T extends E> void addAll(DynamicArray<T> c) {
       for (int i = 0; i < c.size; i++) {
           add(c.get(i));
       }
   }
   ```

`<T extends E>` 用于定义类型参数，它声明了一个类型参数 `T`，可以放在泛型类定义中类名的后面、泛型方法返回值的前面。

### 8.2.1 更简洁的参数类型限定

上一个例子还可以改写成如下形式：

```java
public void addAll(DynamicArray<? extends E> c) {
    for (int i = 0; i < s.size; i++) {
        add(c.get(i));
    }
}
```

`?` 表示无限定通配符。

`<? extends E>` 表示有限定通配符，匹配 `E` 或者 `E` 的某个子类型，但是具体什么子类型是未知的。

`<? extends E>` 用于实例化类型参数，它用于实例化泛型变量中的类型参数，只是这个具体类型是未知的，只知道它是 `E` 或者 `E` 的某个子类型。

这两种通配符都有一个重要的限制：**只能读，不能写。**因为如果允许写入，Java 就无法保证类型安全性。

例：

```java
  DynamicArray<? extends Number> numbers1 = new DynamicArray<>();
  numbers1 = ints;
  numbers1.addAll(1.2); // error
		
  DynamicArray<?> numbers2 = new DynamicArray<>();
  numbers2.add(1); // error
```

### 8.2.3 超类型通配符

`<? super E>` 表示超类型通配符



