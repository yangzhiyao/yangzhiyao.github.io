---
title: Java String split 操作
---

举个例子：
```java
String string = "004-034556";
String[] parts = string.split("-");
String part1 = parts[0]; // 004
String part2 = parts[1]; // 034556
```

网页中经常会有textarea换行需要拆分成一行一行，这时候需要这样做：
```java
String.split(System.getProperty("line.separator"));
```
或者这样：
```java
String.split("\\r?\\n");
```
PS：第一种方式存在问题，建议一般使用第二种方式。

在部分系统中，取得的换行符是\n，然而，用户提交内容却是\r\n，并没有想象之中那么完美的实现转换。
导致结果每行末尾会多一个\r

尽管看起来魔法味十足，但是至少能正确匹配结果。