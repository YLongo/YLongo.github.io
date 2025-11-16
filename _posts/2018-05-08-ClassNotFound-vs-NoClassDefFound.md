---
layout: post
title: ClassNotFound vs NoClassDefFound
date: 2018-05-08 09:05:15
tags: [java]
---

ClassNotFoundException 与 NoClassDefFoundError 的区别

<!-- more -->

### ClassNotFoundException

当尝试通过以下方式通过类名去获取类的时候：

-   [`Class.forName(java.lang.String)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#forName-java.lang.String-) 
-   [`ClassLoader.findSystemClass(java.lang.String)`](https://docs.oracle.com/javase/8/docs/api/java/lang/ClassLoader.html#findSystemClass-java.lang.String-) 
-   [`ClassLoader.loadClass(java.lang.String, boolean)`](https://docs.oracle.com/javase/8/docs/api/java/lang/ClassLoader.html#loadClass-java.lang.String-boolean-) 

但是通过类名指定的类又不存在的时候。

例：[ClassNotFoundTest](https://github.com/YLongo/javademo/blob/master/src/main/java/github/io/ylongo/exception/ClassNotFoundTest.java)

```java
public class ClassNotFoundTest {

    public static void main(String[] args) {
        
        String className = "com.mysql.jdbc.driver";
        try {
            Class.forName(className);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

报错信息如下：

```java
java.lang.ClassNotFoundException: com.mysql.jdbc.driver
	at java.net.URLClassLoader.findClass(URLClassLoader.java:381)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:331)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	at java.lang.Class.forName0(Native Method)
	at java.lang.Class.forName(Class.java:264)
	at github.io.ylongo.exception.ClassNotFoundTest.main(ClassNotFoundTest.java:13)

```



### NoClassDefFoundError

当 JVM 或者 ClassLoader 实例尝试去加载类的定义时（通过方法的调用，或者 `new` 表达式新建一个实例），但是却没有找到该类的定义。

在编译时期，类的定义就已经存在了，但是在运行时期却没有找到该类的定义。

例：[NoClassDefFoundTest](https://github.com/YLongo/javademo/blob/master/src/main/java/github/io/ylongo/exception/NoClassDefFoundTest.java)

```java
public class NoClassDefFoundTest {
    public static void main(String[] args) {
        A a = new A();
    }
}

class A {
    
}
```

在运行之前删除 A.class。报错信息如下：

```java
Exception in thread "main" java.lang.NoClassDefFoundError: github/io/ylongo/exception/A
	at github.io.ylongo.exception.NoClassDefFoundTest.main(NoClassDefFoundTest.java:5)
Caused by: java.lang.ClassNotFoundException: github.io.ylongo.exception.A
	at java.net.URLClassLoader.findClass(URLClassLoader.java:381)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:331)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	... 1 more
```



参考：

1.  [ClassNotFoundException](https://docs.oracle.com/javase/8/docs/api/java/lang/ClassNotFoundException.html)
2.  [NoClassDefFoundError](https://docs.oracle.com/javase/8/docs/api/java/lang/NoClassDefFoundError.html)

