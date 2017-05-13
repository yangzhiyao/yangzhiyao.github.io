---
title: 如何获得java应用程序的当前目录
---
# 如何获得java应用程序的当前目录

## 1、使用System.getProperty("user.dir");

当前目录就是你运行程序时所在的目录，使用该方法可以得到用户当前目录，但是有一个问题，如果你是从其他目录执行的应用，则会返回命令行中用户当前所在的位置。

```java
String workingDir = System.getProperty("user.dir");
```

## 2、使用ClassLoader的getResource("").getPath();

```java
String workingDir = this.getClass().getClassLoader().getResource("").getPath();
```

这个得到的是当前类的路径,但是如果打包成jar文件执行的时候，得到的路径会是这个样子：

file:/.../projects/xxx/target/xxx.jar!/...

所以，针对这种情况需要做一下处理：

```java
String workingDir;
String resultDir;
File f = new File(this.getClass().getClassLoader().getResource("").getPath());
workingDir = f.getParent();
int idxofv = workingDir.indexOf("!");
int liof = -1;
if (idxofv != -1) {
	liof = workingDir.lastIndexOf("/", idxofv);
	resultDir = workingDir.substring(0, liof);
}else{
	resultDir = workingDir;
}
```

如果是jar包，则路径中第一个叹号是虚拟路径的开始，而上级路径则是取叹号之前的第一个“/”之前的内容。

终于可以把jar复制到哪就从哪里读取配置和设定上传文件等相对路径而不用写死到程序里面了。

