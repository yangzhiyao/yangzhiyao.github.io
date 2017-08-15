---
title: Use SSH Config
---

使用SSH的配置文件可以在很大程度上方便各种操作，特别适应于有多个SSH帐号、使用非标准端口或者写脚本等情况。

man ssh_config
可以查看手册

如果之前是用密码方式来登录SSH，需要先改用证书方式。可以看最后面生成SSH证书

## 配置文件

用户配置文件在~/.ssh/config，没有的话新建一个。基本的写法是
```
Host 名称(自己决定，方便输入记忆的)
    HostName 主机名
    User 登录的用户名
```

假设有两个SSH帐号，一个是github的，一个是其他服务器的，私钥都是~/.ssh/id_rsa，可以这样写：

```
Host github.com
    HostName github.com
    User git

Host server
    HostName 服务器地址
    User 登录用户名
```

注意，github的Host必须写成“github.com”。你可以会有其他要求，比如指定端口号、绑定本地端口，这些都可以通过man来查询，比如

Port 端口号
DynamicForward 本地端口号
如果服务器同时有ipv4/ipv6地址，HostName使用域名会比较方便

## 使用

有了这些配置，很多操作就非常简化了。比如登录服务器

```
ssh server
```

## 传输文件

```
scp server:~/test .
```

如果使用Putty等工具，可能需要一些其他操作(转换私钥格式，貌似)，自行搜索吧
生成SSH证书

先在本地生成密钥

```
ssh-keygen -t rsa
```

会询问将密钥放在何处，默认即可。然后是输入密码，留空(否则你登录不仅需要私钥还要输入密码)。

完成后在~/.ssh目录下会生成另个文件id_rsa和id_rsa.pub，一个私钥一个公钥。之后将id_rsa.pub上传到服务器端用于SSH登录的用户的家目录下。执行

```
cat id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
rm id_rsa.pub
```

修改服务器端的sshd配置。编辑/etc/ssh/sshd_config如下

```
PubkeyAuthentication yes
PasswordAuthentication no
```

之后重启sshd服务