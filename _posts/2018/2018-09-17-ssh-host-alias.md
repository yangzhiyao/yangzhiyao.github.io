---
title: ssh 多服务器登录管理tips
---

osx系统想要实现securecrt或xshell类似的免密码登录，可以用密钥方式：

## 生成密钥
```shell
ssh-keygen
```

## 复制id到服务器
把公钥复制到服务器
```shell
ssh-copy-id
```

## 创建本地配置alias
修改~/.ssh/config

添加以下格式内容：
```
Host aliasname
    Hostname ipaddress|domain
    User username
    Port portnumber
```

之后登陆服务器就只需要使用：
```
ssh aliasname
```

就可以使用证书快速登录服务器了。