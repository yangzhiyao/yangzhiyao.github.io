---
title: Tomcat 在 Linux 发行版上启动速度超慢的解决办法及其原因
---

## 症状

启动Tomcat，卡在启动界面不动。

## 原因

因为默认使用了/dev/random, 导致了阻塞

## 方案

切换到非阻塞的 /dev/urandom

## 具体

方法一：设置启动参数
```
-Djava.security.egd=file:/dev/./urandom
```

需要注意的是中间的：/./

方法二：修改配置文件 jre/lib/security/java.security
```
securerandom.source=file:/dev/urandom
```

## 刨根问底

### Linux中的随机数发生器
在Linux操作系统中，有一个特殊的设备文件，可以用作随机数发生器或伪随机数发生器。

#### /dev/random

在读取时，/dev/random设备会返回小于熵池噪声总数的随机字节。/dev/random可生成高随机性的公钥或一次性密码本。若熵池空了，对/dev/random的读操作将会被阻塞，直到从别的设备中收集到了足够的环境噪声为止。当然你也可以设置成不堵塞，当你在open 的时候设置参数O_NONBLOCK， 但是当你read的时候，如果熵池空了，会返回-1

#### /dev/urandom

/dev/random的一个副本是/dev/urandom （"unlocked"，非阻塞的随机数发生器[4]），它会重复使用熵池中的数据以产生伪随机数据。这表示对/dev/urandom的读取操作不会产生阻塞，但其输出的熵可能小于/dev/random的。它可以作为生成较低强度密码的伪随机数生成器，不建议用于生成高强度长期密码。

## 结论

针对大部分应用使用urandom即可
