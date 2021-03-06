---
title: Git基础学习(4)
date: 2016-04-24 17:03:52
tags: git
---
&emsp;&emsp;此节讲解标签的创建和管理，使用GPG工具进行标签的加密处理。
<!-- more -->
## 5 git标签管理
&emsp;&emsp;标签可以简单的理解为属于分支定义的别名，分支本身都会进行指针的配置（分支都会指向某一个commit id），但是标签却是一个固定的内容，可以说标签永远指向一个固定的commit id。

## 5.1 创建标签
![1](/images/git_5.1.png) 

现在假如存在2个分支。

**范例：**为master定义一个标签.

- master是程序最终的发布版本，可以说，master就是完整的开发程序。

```bash
git tag v1.0
```

**范例：**查看所有标签.

```bash
git tag 
```

这个标签只是针对当前master分支打上的标签，可是在一个项目中一定会有许多的提交点，那么如果要为之前的提交点打上标签该怎么做呢？只需要找到commit id就可以。

**范例：**查看日志.

```bash
git log --pretty=oneline--abbrev-commit
```

![2](/images/git_5.2.png) 

在这些的日志上都存在commitid，针对一个commit id进行标签的定义。

**范例：**设定标签.

```bash
git tag v0.6 9052a56
```

此时的标签只是显示一些很简单的标签信息并不是很好的，所以下面希望可以查看标签的完整内容。

**范例：**查看标签的完整信息.

```bash
git show v0.6
```

![3](/images/git_5.3.png) 

在之前的标签都没注释信息，那么如何设置注释信息呢？

**范例：**添加标签时设置注释.

```bash
git tag -a v0.7 -m "relese version" id
```

随后再次查询标签的详细信息。

**范例：**添加标签时设置注释.

```bash
git show v0.7
```

![4](/images/git_5.4.png)

## 5.2 标签加密
&emsp;&emsp;标签创建完成之后如果开发者只希望可以由自己来修改标签，那么急必须进行标签的加密。可以使用GPG工具实现加密的操作。下载地址如下：

![5](/images/git_5.5.png)

**范例：**测试安装是否成功.

```bash
gpg -help
```

![6安装成功](/images/git_5.5.jpg)

**使用步骤：**

```bash
1.生成自己秘钥
gpg --gen-key

2.使用RSA算法（默认）
直接回车

3.秘钥长度，默认的长度是2048位
直接回车

4.秘钥的有效期，可以选择秘钥永远不过期（0）
0

5.本次输入Y表示确认以上信息。
y

6.设置个人信息

7.询问是否进行修改，信息确认过，直接输入OK
o

8.随后输入提示框，设置私钥的密码，防止有人恶意修改

9.此时会出现一系列的信息，图7

```

![7](/images/git_5.6.png)

以上有几点信息：

- 密钥号：
- 用户ID：（真实姓名 注释 邮箱）

实际上为了日后管理方便，最好生成一张撤销的整数，为的是以后如果密钥过期可以通过服务器撤销。

```bash
gpg --gen -revoke id
```

![8](/images/git_5.7.png)

![9](/images/git_5.8.png)

![10](/images/git_5.9.png)

![11](/images/git_5.10.png)


## 5.3 使用GPG生成标签
要保证在使用之前一定配置好相关GPG工具，现在已经有一个生成的密钥，名称为“*******”，所以下面进行加密标签，标签的加密只需要加“-s” 。

**范例：**使用gpg加密标签.

```bash
git tag -s v0.9 -m "gpg tag" id
```

此时并不能进行加密。我们需要在加密时使用一个指定的签名，所以实际上的标签加密处理应该输入如下的指令完成。

```bash
git tag -u "username" -s v0.9 -m "gpg tag" id
```

![12](/images/git_5.11.png)

此时其他的开发者是不可能修改此标签的。


## 5.4 标签的管理
**范例：**删除标签.

```bash
git tag -d v0.9
```

这样至少进行本地标签的删除，而标签也是可以提交到服务器上。

**范例：**提交标签到服务器上.

```bash
git push origin v0.6
```

推送完成后，可以进行标签的查看。


**范例：**一次性推送多个标签.

```bash
git push origin --tags
```

本地未推送的到被提交到服务器上。

**范例：**删除本地标签.

```bash
git tag -d v0.7
```

但是服务器还存在，但是可以删除：


**范例：**删除远程服务器标签.

```bash
git push origin :refs/tags/v0.7
```

这个时候就可以很好的实现管理。


## 5.5 总结
标签实际上就是起一个别名，有些不喜欢起别名的，那么就按着传统的方式开发，我们可以利用GPG工具建立加密的安全标签。














