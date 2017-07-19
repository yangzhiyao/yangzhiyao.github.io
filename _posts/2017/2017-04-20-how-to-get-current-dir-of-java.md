---
title: 如何获得java应用程序的当前目录
---
# 如何获得java应用程序的相关路径

## 当前工作路径
你所在的文件夹如果是：/home/user123，而你输入java命令执行程序则会返回/home/user123文件夹。
```java
String currentPath = System.getProperty("user.dir");
```

## 项目所在路径
```java
File file = new File(FilePathUtil.class.getProtectionDomain().getCodeSource().getLocation().getFile());
String basePath = file.getParentFile().getPath();
```

经测试其他几种获取方式都有其独特的坑爹特质，如果想在jar和class运行时都正常就按上面用法就行了。

终于可以把jar复制到哪就从哪里读取配置和设定上传文件等相对路径而不用写死到程序里面了。

