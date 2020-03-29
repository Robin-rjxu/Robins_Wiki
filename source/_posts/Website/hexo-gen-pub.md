---
title: 使用 Hexo 生成与配置站点
toc: true
date: 2020-03-28 23:31:18
tags:
    - hexo
    - website
---

Hexo 安装完成后，进入博客存放的文件夹，初始化我们的博客，输入：

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

## References
> - [GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)