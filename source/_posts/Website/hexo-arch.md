---
title: Hexo 目录与文件简要说明
toc: true
date: 2020-03-28 23:35:46
tags:
---

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

## References

> - [Hexo 搭建个人博客指南](https://juejin.im/post/5c2dceb1518825079f78574d)
