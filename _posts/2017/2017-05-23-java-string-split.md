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
从形式上，我更倾向于前一种。后面一种魔法味十足。