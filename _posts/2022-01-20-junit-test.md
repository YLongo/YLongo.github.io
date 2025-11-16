---
layout: post
title: "Junit Test"
date: 2022-01-20 00:00
tags: [testing, java]
---

测试类的命名: `ClassNameTest`

测试方法的命名: `testMethodName`

使用 `Assert` 来判断输出结果是否正确

常用断言:

- `Assert.assertEquals(expected, actual)`
- `Assert.assertTrue(condition)`
- `Assert.assertNotNull(object)`
- `Assert.ArrayEquals(expected, actual)`

异常测试: `@Test(ExceptionClassName.class)`

每个测试用例都会执行一次的方法: `@Before`、`@After`

每个测试类都会执行至此的方法: `@BeforeClass`、`@AfterClass`

