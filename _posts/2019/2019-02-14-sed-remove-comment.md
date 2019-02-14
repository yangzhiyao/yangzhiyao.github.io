---
title: sed快速移除注释
---

``` shell
sed去除注释行：sed -i -e '/^#/d' config_file
sed去除空行： sed -i -e '/^$/d' config_file
sed去空行和注释行：sed -i -e '/^$/d;/^#/d' config_file
```