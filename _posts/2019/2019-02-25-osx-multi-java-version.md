---
title: osx 多java版本共存
---

MacOS 有一个非常有用的工具：

/usr/libexec/java_home

他可以告诉你当前的JAVA_HOME例如

/Library/Java/JavaVirtualMachines/openjdk-11.0.1.jdk/Contents/Home

## 方法1 采取别名进行java版本的切换

alias j8='export JAVA_HOME=`/usr/libexec/java_home -v 1.8`; jenv global 1.8; java -version'
alias j11='export JAVA_HOME=`/usr/libexec/java_home -v 11`; jenv global 11.0; java -version'

## 方法2 使用export插件

jenv enable-plugin export

之后重启shell则自动会把当前jenv设置的版本设置到环境变量里，很方便