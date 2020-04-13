---
title: 安装 Git 与使用 Github
toc: true
tags:
  - git
  - github
  - tools
abbrlink: 64853
date: 2020-03-28 14:35:36
---

## Git 和 Github 简介

Git 是一个开源的分布式版本控制系统。而 GitHub 本质上是一个代码托管平台，它提供的是基于 Git 的代码托管服务。对于一个团队来说，即使不使用 GitHub，他们也可以通过自己搭建和管理 Git 服务器来进行代码库的管理，甚至还有一些其它的代码托管商可供选择，如 GitLab，BitBucket 等。值得一提的是 Git 作为一个开源项目，其代码本身就被托管在 GitHub 上，如果您感兴趣，可以上去一观其真容：[Git 项目地址](https://github.com/gitGit)。

Git 是一个源代码版本控制系统，一个可管理、追溯项目源码的工具。 GitHub 是一个提供 Git 仓库托管服务的平台。 简而言之，Git 是一个工具，而GitHub 是一个托管平台。

## 环境准备

### 创建 Github 账号
登录到 GitHub,如果没有 GitHub 帐号，使用你的邮箱注册GitHub帐号，点击 GitHub 中的 New repository 创建新仓库。如果使用 Github 默认的 Pages 作为自己的网站，仓库名应该为：`用户名.github.io`，用户名使用 GitHub 帐号名称代替，这是固定写法。

### 安装 Git

从Git官网下载：[Git - Downloading Package](https://git-scm.com/download/win) ，选择64位的安装包，下载后安装，在命令行里输入git测试是否安装成功，若安装失败，参看其他详细的Git安装教程。安装成功后，将你的 Git 与GitHub 帐号绑定，鼠标右击打开 Git Bash。

![Git_bash](https://pic3.zhimg.com/80/v2-8b1cbe253d6e0301bd9a68c6f98a9f52_hd.jpg)

### 设定 Github 作为远程仓库

设置user.name和user.email配置信息：

```bash
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub注册邮箱"
```

上述设置邮箱也可以使用 Github 提供的 public profile email，即 用户名@users.noreply.github.com。相关设置可以在自己的 [Github 设置](https://github.com/settings/emails) 中设定。

### 使用 ssh 访问 Github

下面设定使用 ssh 访问自己的 Github。首先
生成ssh密钥文件：

```bash
ssh-keygen -t rsa -C "你的GitHub注册邮箱"
```

然后直接三个回车即可，默认不需要设置密码
然后找到生成的 .ssh 的文件夹中的 id_rsa.pub 密钥，将内容全部复制。

![ssh-kegen](https://pic4.zhimg.com/80/v2-d1e47103ec1aa8675f68688c5d63bd27_hd.jpg)

如果以后使用其他 shell 的 ssh 进行 Git 操作，如 cgywin 或 WSL 环境，可以将包含该密钥对的 .ssh 复制到 home 目录即可。

打开 [GitHub_Settings_keys](https://github.com/settings/keys) 页面，新建 new SSH Key。

![new-github-sshkey](https://pic1.zhimg.com/80/v2-72a3f22c080e99343c3cc4aabce10e3c_hd.jpg)

Title 为标题，任意填即可，将刚刚复制的id_rsa.pub内容粘贴进去，最后点击 Add SSH key。
在Git Bash中检测 GitHub 公钥设置是否成功，输入`ssh git@github.com` ：

![ssh-github](https://pic3.zhimg.com/80/v2-da481ffa686410becd4186c656b4ebd6_hd.jpg)

如上则说明成功。这里之所以设置 GitHub 密钥原因是，通过非对称加密的公钥与私钥来完成加密，公钥放置在 GitHub 上，私钥放置在自己的电脑里。GitHub 要求每次推送代码都是合法用户，所以每次推送都需要输入账号密码验证推送用户是否是合法用户，为了省去每次输入密码的步骤，采用了 ssh，当你推送的时候，git 就会匹配你的私钥跟 GitHub 上面的公钥是否是配对的，若是匹配就认为你是合法用户，则允许推送。这样可以保证每次的推送都是正确合法的。

## References
> - [Git 和 GitHub 基础简介](https://www.ibm.com/developerworks/cn/opensource/os-cn-git-and-github-1/index.html)
> - [廖雪峰-Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)