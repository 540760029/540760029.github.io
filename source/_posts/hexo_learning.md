---
title: hexo 教程
tags: hexo
categories: hexo 教程
description: hexo 基本指令
---

## 常用指令

### 创建一个新文章 .md 文件

``` bash
$ hexo new "My New Post"
```

更多信息: [Writing](https://hexo.io/docs/writing.html)

### 将md 生成静态文件

``` bash
$ hexo generate
```

更多信息: [Generating](https://hexo.io/docs/generating.html)



### 启动本地服务器查看

``` bash
$ hexo server
```

更多信息: [Server](https://hexo.io/docs/server.html)


### 无误后将文章发布到GitHub

``` bash
$ hexo deploy
```

更多信息: [Deployment](https://hexo.io/docs/one-command-deployment.html)


### 引用自己写的别的文章

有两种方式：
1、通过模板：

```
{% post_link md的名字  显示的名字%}

如：
{% post_link algor1 排序模板 %}
```
效果：

{% post_link algor1 排序模板 %}

2、引用永久链接： hexo 生成的文章是 /年/月/日/文件 格式，所以markdown引用链接就可以了：
```
[排序模板](/2020/09/19/algor1)
```
效果：
[排序模板](/2020/09/19/algor1)