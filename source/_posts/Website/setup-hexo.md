---
title: 安装 Hexo
toc: true
tags:
  - hexo
  - website
abbrlink: 52045
date: 2020-03-28 23:25:07
---

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

至此，Hexo 环境配置完毕。

## References

> - [GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)
