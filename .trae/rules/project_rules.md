
# Hexo博客管理规则

## 文件命名规范
- 文章文件：使用中文标题，如 `文章标题.md`
- 分类目录：按主题分类，如 `source/categories/分类名/`
- 标签管理：在文章Front Matter中设置tags


## 文章Front Matter规范
```yaml
title: 文章标题
date: YYYY-MM-DD HH:MM:SS
categories: [主分类, 子分类]
tags: [标签1, 标签2]
```

## 内容格式
- 使用Markdown格式
- 文章摘要：150字符后，使用 `<!-- more -->` 标签
- 图片处理：使用相对路径或CDN链接
- 代码块：使用 ``` 包裹，指定语言



## 部署流程
1. 本地预览：`hexo server`
2. 清理缓存：`hexo clean`
3. 生成静态文件：`hexo generate`
4. 部署到GitHub：`hexo deploy`

## 源码管理
- 使用Git管理源码
- 主分支：部署生成的静态文件
- source分支：存储原始源码