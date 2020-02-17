---
title: 使用 Hexo 和 Github 创建并发布自己的网站
toc: true
tags:
  - Hexo
  - Github
abbrlink: 54683
date: 2020-02-02 17:23:10
---

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [涉及知识与技能](#涉及知识与技能)
- [基本步骤](#基本步骤)
  - [安装 Git 与使用 Github](#安装-git-与使用-github)
  - [安装 Hexo](#安装-hexo)
  - [使用 Hexo 生成与配置站点](#使用-hexo-生成与配置站点)
  - [站点的上传与发布](#站点的上传与发布)
  - [站点个性化配置](#站点个性化配置)
  - [注册与绑定域名](#注册与绑定域名)
  - [站点 https 加密](#站点-https-加密)
  - [实现站点的自动部署](#实现站点的自动部署)
- [注意事项](#注意事项)
  - [Hexo 使用](#hexo-使用)
  - [Github 发布站点](#github-发布站点)
- [References](#references)

<!-- /code_chunk_output -->

如何使用 Hexo 创建自己的网站，将其上传到 Github 上，并将其发布为公开站点。

## 涉及知识与技能

- Hexo 的基本概念与操作
- Git 的基本与操作
- Github 的基本操作
- 域名注册与绑定
- Github Actions 的基本操作

## 基本步骤

### 安装 Git 与使用 Github

Git 是一个开源的分布式版本控制系统。而 GitHub 本质上是一个代码托管平台，它提供的是基于 Git 的代码托管服务。对于一个团队来说，即使不使用 GitHub，他们也可以通过自己搭建和管理 Git 服务器来进行代码库的管理，甚至还有一些其它的代码托管商可供选择，如 GitLab，BitBucket 等。值得一提的是 Git 作为一个开源项目，其代码本身就被托管在 GitHub 上，如果您感兴趣，可以上去一观其真容：[Git 项目地址](https://github.com/gitGit)。

Git 是一个源代码版本控制系统，一个可管理、追溯项目源码的工具。 GitHub 是一个提供 Git 仓库托管服务的平台。 简而言之，Git 是一个工具，而GitHub 是一个托管平台。

登录到 GitHub,如果没有 GitHub 帐号，使用你的邮箱注册GitHub帐号，点击 GitHub 中的 New repository 创建新仓库。如果使用 Github 默认的 Pages 作为自己的网站，仓库名应该为：`用户名.github.io`，用户名使用 GitHub 帐号名称代替，这是固定写法。

从Git官网下载：[Git - Downloading Package](https://git-scm.com/download/win) ，选择64位的安装包，下载后安装，在命令行里输入git测试是否安装成功，若安装失败，参看其他详细的Git安装教程。安装成功后，将你的 Git 与GitHub 帐号绑定，鼠标右击打开 Git Bash。

![Git_bash](https://pic3.zhimg.com/80/v2-8b1cbe253d6e0301bd9a68c6f98a9f52_hd.jpg)

设置user.name和user.email配置信息：

```bash
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub注册邮箱"
```

上述设置邮箱也可以使用 Github 提供的 public profile email，即 用户名@users.noreply.github.com。相关设置可以在自己的 [Github 设置](https://github.com/settings/emails) 中设定。

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

### 安装 Hexo

Hexo是一款基于 Node.js的静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在GitHub和Heroku上，是搭建博客的首选框架。Hexo同时也是GitHub上的开源项目，参见：[hexojs/hexo](https://github.com/hexojs/hexo) 如果想要更加全面的了解Hexo，可以到其官网 Hexo 了解更多的细节，因为 Hexo 的创建者是台湾人，对中文的支持很友好，可以选择中文进行查看。

Hexo 基于 Node.js，Node.js下载地址：[Download | Node.js](https://nodejs.org/en/download/)。下载安装包，注意安装 Node.js 会包含环境变量及 npm 的安装，安装后，检测 Node.js 是否安装成功，在命令行中输入`node -v`:

![nodejs-v](https://pic1.zhimg.com/80/v2-76ea38e9545e606f975781e47933b010_hd.jpg)

检测npm是否安装成功，在命令行中输入`npm -v` :

![npm-v](https://pic2.zhimg.com/80/v2-bede250b8456df92475b455fda8c1dd9_hd.jpg)

到此，安装Hexo的环境已经全部搭建完成。

Hexo 就是我们的个人博客网站的框架， 这里需要自己在电脑常里创建一个文件夹，可以命名为 blog ，Hexo 框架与以后你自己发布的网页都在这个文件夹中。创建好后，进入文件夹中，按住 shift 键，右击鼠标点击命令行，可以使用 Git-bash 或其他 shell。

使用 npm 命令安装 Hexo，输入：

```bash
npm install -g hexo-cli
```

### 使用 Hexo 生成与配置站点

安装完成后，进入博客存放的文件夹，初始化我们的博客，输入：

```bash
hexo init blog
```

其中 `blog` 博客的名字。为了检测我们的网站雏形，分别按顺序输入以下三条命令：

```bash
hexo new test_my_site
hexo g
hexo s
```

其中第一句为在博客中新增一篇帖子 `test_my_site` ，第二句为编译生成博客，第三句为在本地启动预览服务。

完成后，打开浏览器输入地址：

```html
localhost:4000
```

可以看出我们写出第一篇博客，只不过我下图是我修改过的配置，和你的显示不一样。

![hexo-preview](https://pic4.zhimg.com/80/v2-123e73c0630d299b1c856d99b04b55bb_hd.jpg)

常用 hexo 命令：

```hexo
npm install hexo -g #安装Hexo
npm update hexo -g #升级
hexo init #初始化博客

# 命令简写
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo g == hexo generate #生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy #部署

hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP
hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令
```

### 站点的上传与发布

上面只是在本地预览，接下来要做的就是就是推送网站，也就是发布网站，让我们的网站可以被更多的人访问。在设置之前，需要解释一个概念，在 blog 根目录里的_config.yml文件称为站点配置文件，如下图：

![site_folder](https://pic2.zhimg.com/80/v2-cb1fd5e5a2e73f513234e434724c7c55_hd.jpg)

进入根目录里的themes文件夹，里面也有个_config.yml文件，这个称为主题配置文件，如下图:

![site-theme-folder](https://pic4.zhimg.com/80/v2-4252029e5634bf91c7d58916ae2b8ac3_hd.jpg)

下一步将我们的 Hexo 部署与 GitHub 关联起来，打开站点的配置文件 `_config.yml`，翻到最后修改为：

```js
deploy:
type: git
repo: 这里填入你之前在 GitHub 上创建仓库的完整路径，记得加上.git
branch: master
```

参考如下：

![deploy-setting](https://pic1.zhimg.com/80/v2-279ac5149b577f04dc099defbb12eaa8_hd.jpg)

保存站点配置文件。

上述文件其实就是给 hexo d 这个命令做相应的配置，让 hexo 知道你要把 blog 部署在哪个位置，很显然，我们部署在我们 GitHub 的仓库里。最后安装 Git 部署插件，输入命令：

```bash
npm install hexo-deployer-git --save
```

这时，我们分别输入三条命令：

```bash
hexo clean 
hexo g 
hexo d
```

其实第三条的 `hexo d` 就是部署网站命令，d 是 deploy 的缩写。完成后，打开浏览器，在地址栏输入你的放置个人网站的仓库路径，即 `https://xxxx.github.io`，比如我的 xxxx 就是我的 GitHub 用户名。你就会发现你的博客已经上线了，可以在网络上被访问了。

注意 `hexo d` 命令其实是调用了 Git，即包含了 `git commit` 和 `git push`等操作，因此需要前期在 Git 中配置好与 Github 密钥对，否则会提示输入 Github 的用户名密码等用于身份验证。

### 站点个性化配置

### 注册与绑定域名

### 站点 https 加密

### 实现站点的自动部署

## 注意事项

### Hexo 使用
在站点目录中，有几个文件值得注意，它们的具体功能与使用可以参考 hexo 的官方网站：

`.gitignore` 文件定义了 `git commit` 不包含的内容，如 node.js 的插件安装文件夹，`node_modules` 和预览文件夹 `public` 等等，可以充分利用该文件定义部署后无需上传的内容，如密钥文件，涉及隐私的文件等等。

`scaffolds` 文件夹内包含了样式模板，可以用于定义不同新建不同页面包含的初始内容，如标题，标签，分类，日期，链接等等。

`themes` 文件夹内含主题配置文件，默认不会上传，但如果要将整个站点上传至仓库，如实现自动部署等等，则需要通过配置 `.gitignore` 文件，将该文件夹一并上传。

`package.json` 文件记录了站点生成过程中使用的所有插件，以及其版本号。其中，站点生成最基本的依赖为 `hexo-generator` 系列插件，如果该插件调用错误，则无法生成站点发布所需要的 `index.html` 文件，表现为站点发布后，访问 `user.github.io` 页面提示找不到页面，或 404 错误。

Hexo 生成与发布常见问题都可以通过 `hexo s` 预览提示中看到。如常见的 `找不到 / ` 通常是因为没有正确安装插件，导致未生成静态页面 `index.html` 导致。

正确的站点创建操作应当为：

- `npm` 安装 hexo
- `npm` 安装 依赖插件
- `hexo i` 初始化站点
- `hexo g` 生成站点

记录在 `package.json` 中的插件，会在生成站点时自动联网安装，但如果是在本地第一次新建站点，需要先行安装依赖插件，需要注意使用参数 `--save` 或最后输入一次 `npm install` 保证正确安装。安装后的插件存放在 `node_modules` 文件夹，通常无需发布上传。

安装 hexo：

```hexo
npm install -g hexo-cli
npm install
```

安装 插件

```hexo
npm install hexo-deployer-git --save
```

或
```hexo
npm install hexo-deployer-git
npm install
```

升级插件：

```hexo
npm update
```

### Github 发布站点

使用 Github 默认的 `user.github.io` 页面，部署到的仓库需要按规定命名，部署发布至指定使用 `master` 分支；其他自定义仓库和域名等，部署发布指定使用 `ghpages` 分支，如果需要实现自动部署，需要把网站配置文件夹和自动部署设置存放在其他分支，相关设定参考 [Github Pages 帮助](https://help.github.com/en/github/working-with-github-pages)。

## References

> - [GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)
> - [Git 和 GitHub 基础简介](https://www.ibm.com/developerworks/cn/opensource/os-cn-git-and-github-1/index.html)
> - [廖雪峰-Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
