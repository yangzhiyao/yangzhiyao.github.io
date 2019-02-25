---
title: osx 多java版本共存
---

MacOS 有一个非常有用的工具：

/usr/libexec/java_home

他可以告诉你当前的JAVA_HOME例如

/Library/Java/JavaVirtualMachines/jdk-9.jdk/Contents/Home

采取下面的别名可以方便的进行java版本的切换，配合jenv

alias j8='export JAVA_HOME=`/usr/libexec/java_home -v 1.8`; jenv global 1.8; java -version'
alias j11='export JAVA_HOME=`/usr/libexec/java_home -v 11`; jenv global 11.0; java -version'