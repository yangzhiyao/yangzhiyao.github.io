---
title: 搭建最简单的SVN服务器
---

## 简介

Subversion是传统的集中版本控制系统。用于跟踪项目生产和维护过程中的变化，SVN是许多开发工具链中熟悉和标准的组件。Subversion被开发为“CVS”版本控制系统的替代品。SVN试图修复CVS系统的许多主要问题，而不需要软件开发方式的任何范式转变。

作为一个现代系统，Subversion“修复”了一些CVS历史问题的问题：

“提交”操作是原子的。当用户保存修订版本并将其发送到svn服务器时，服务器进程将不会将提交数据放在服务器的数据库中。文件和目录可以重命名，同时仍然保持文件的一致记录。

因为subversion使用集中式架构，为了与其他人协作，必须拥有一个托管该项目的服务器。本文档描述了安装和设置以及svn工具的基本使用。

## Linux 不同发行版之间的小差异

在Debian和Ubuntu系统上，通过以下命令来安装：

``` shell
apt-get install subversion
```

在CentOS和Fedora系统上，subversion使用以下命令安装：

``` shell
yum install subversion
```

在Arch Linux上，svn使用以下命令进行安装：

``` shell
pacman -S
```

在Gentoo Linux上，可以通过发出以下命令来安装：

``` shell
emerge dev-util/subversion
```

## 创建 Subversion 存储仓库
安装完成后，就可以开始创建仓库。你可以把仓库放在任何svn服务账户能够有权限访问路径，在这里我们选择 /srv/svn/ 作为仓库目录，在其中可以创建多个仓库，形如：/srv/svn/repos/

为了创建目录需要使用以下命令：

``` shell
mkdir -p /srv/svn/
``` 

-p 参数的含义是如果父目录不存在，则一并建立。

好了，现在就可以使用 svnadmin 命令创建具体的仓库了：

```
svnadmin create /srv/svn/repoName
```

在这个例子中，在/srv/svn/目录下创建了一个存储库 repoName。接下来需要对仓库进行一些必要的设置。

## Subversion 权限控制

版本库建立好后，进入仓库目录，可以看到以下的文件结构：

```
conf  db  format  hooks  locks  README.txt
```

我们需要关注的一个文件夹是conf目录，以及其中的3个文件：

svnserve.conf
passwd
authz

### svnserve.conf 文件配置要点：

``` conf
[general]
#匿名用户权限
anon-access = none
#认证用户权限
auth-access = write
#密码数据库
password-db = passwd
#认证权限数据库
authz-db = authz
```

### passwd 文件配置格式

``` conf
[users]
user1=123456
user2=123456
user3=123456
#...
```

### authz 文件格式

auth 配置文件分为3段：
``` conf
#别名
[aliases]
#分组
[groups]
#项目路径权限
[路径]
用户 = 权限
```

具体应用范例如下：

### Examples

Subversion folder structs:

```
/project
├── trunk
├── tags
├── branches
```

Roles:
super 最大级别的比如boss，不需要开发，所有的给读的权限
manager 项目的管理员，有读写权限，有打tag的权限
developer 开发人员，有读写权限，没打tag的权限
packer 打包人员，只有读tag的权限

``` conf
[groups]
g_super = aaa
g_manager = bbb
g_developer = ccc,ddd
g_packer = eee


[project:/]
@g_super = r
@g_manager = rw
* =

[project:/trunk]
@g_super = r
@g_manager = rw
@g_developer = rw
* =

[project:/branches]
@g_super = r
@g_manager = rw
@g_developer = rw
* =

[project:/tags]
@g_super = r
@g_manager = rw
@g_developer = r
@g_packer = r
* =
```

## 管理 Subversion 存储仓库

svnadmin工具提供了许多其他有助于管理和维护Subversion存储库的命令。以上，我们使用svnadmin create命令，它在指定的路径上创建一个新的存储库。默认情况下，以这种方式创建的存储库使用“FSFS”数据存储系统。如果要使用“BerkleyDB”存储系统，使用以下命令：

```
svnadmin create /srv/svn/repoName --fs-type bdb
```

虽然这些系统在功能上大部分是等价的，但是BerkelyDB后端还有一些额外的限制。一般建议使用默认的FSFS，除非特别需要BekerlyDB系统。以下命令验证存储库的完整性：

```
svnadmin verify /srv/svn/repoName
```

管理工具还提供了将存储库数据存储升级到最新版本的模式的功能：

```
svnadmin upgrade /srv/svn/repoName
```

在您操作Subversion的数据存储（如升级或移动到新机器）的情况下，创建“转储”您的Subversion存储库以作为备份存储是非常有用的。默认情况下，它包含每个提交的完整内容。通过以下命令创建一个svn的“dump”：

```
svnadmin dump /srv/svn/repoName > /var/svn/repoName-1259853077.svn
```

如果要以增量格式保存备份，从而输出修订版本之间的差异，而不是修订版本的完整副本，使用命令：

```
svnadmin dump /srv/svn/repoName --incremental > /var/svn/repoName-1259853077.svn
```

如果您随时需要从备份创建存储库，请执行命令：

```
svnadmin load /srv/svn/repoName-backup < /var/svn/repoName-1259853077.svn
```

这将从svn备份文件的历史记录创建一个新的存储库
