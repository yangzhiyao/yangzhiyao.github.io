---
title: 如何在debian（以及Ubuntu等其它变种发行版）上安装oracle jdk
---

# 如何在debian（以及Ubuntu等其它变种发行版）上安装oracle jdk

如何手工从oracle官方网站上下载jdk并优雅的安装。

## 环境：

Debian 8 jessie 干净系统

通过APT是没有办法安装最新版oracle-jdk的

## 先从oracle官网java se下载页面中找到最新版本的jdk下载地址,并使用wget下载：

```shell
$ wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u144-b01/090f390dda5b47b9b721c7dfaa008135/jdk-8u144-linux-x64.tar.gz
```

## 解压到 /opt/jdk/
```shell
$ sudo mkdir /opt/jdk/
$ sudo mv jdk-8u144-linux-x64.tar.gz /opt/jdk/
$ sudo cd /opt/jdk/
$ sudo tar -zxf jdk-8u144-linux-x64.tar.gz
```

或者直接使用下面的命令：
```shell
$ sudo tar x -C /opt/jdk -f jdk-8u144-linux-x64.tar.gz
```

## 安装jdk
```shell
$ sudo update-alternatives --install /usr/bin/java java /opt/jdk/jdk1.8.0_144/bin/java 100
$ sudo update-alternatives --install /usr/bin/javac javac /opt/jdk/jdk1.8.0_144/bin/javac 100
$ update-alternatives --config java
```

你可以看到目前系统中安装的所有jdk路径

同样javac也可以使用这种方式配置
```shell
$ update-alternatives --config javac
```

## 验证

```shell
$ java -version
java version "1.8.0_144"
Java(TM) SE Runtime Environment (build 1.8.0_66-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.66-b17, mixed mode)
root@hydra:/opt/jdk#

$ javac -version
javac 1.8.0_66
Done. Enjoy your Lambda!
```

## 升级

删除系统中现存的jdk并配置新jdk，最后删除之前的版本目录
```shell
$ sudo update-alternatives --install /usr/bin/java java /opt/jdk/jdk1.8.0_xxx/bin/java 100
$ sudo update-alternatives --install /usr/bin/javac javac /opt/jdk/jdk1.8.0_xxx/bin/javac 100

$ update-alternatives --config java
$ update-alternatives --config javac

$ sudo rm -rf /opt/jdk/jdk1.8.0_144/
```
