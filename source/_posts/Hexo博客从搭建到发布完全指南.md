---
title: Hexo博客从搭建到发布完全指南
date: 2026-04-05 22:12:25
tags:
  - Hexo
  - 博客
  - 教程
  - GitHub
  - 主题
categories:
  - 技术
---

# Hexo博客从搭建到发布完全指南

本文档整理了 Hexo 博客从搭建、主题更换、内容管理到发布的完整流程，帮助你快速掌握 Hexo 博客的使用方法。

## 一、Hexo 博客简介

Hexo 是一个快速、简洁且高效的静态博客生成框架，使用 Node.js 开发。它具有以下特点：
<!-- more -->
- **快速生成**：毫秒级的生成速度
- **支持 Markdown**：使用 Markdown 编写文章
- **主题丰富**：拥有大量精美的主题
- **插件生态**：丰富的插件扩展功能
- **易于部署**：支持一键部署到 GitHub Pages 等平台
## 二、更换 Hexo 主题

### 1. 选择主题

你可以从以下渠道选择适合的主题：
- **Hexo 官方主题库**：[https://hexo.io/themes/](https://hexo.io/themes/)
- **GitHub**：搜索 "hexo theme"
- **热门主题**：NexT、Fluid、Butterfly、Matery、Icarus

### 2. 安装主题

#### 方法一：通过 npm 安装

```bash
# 安装主题
npm install hexo-theme-next

# 复制主题文件到 themes 目录
xcopy node_modules\hexo-theme-next themes\next /E /I
```

#### 方法二：通过 Git 克隆

```bash
# 进入 themes 目录
cd themes

# 克隆主题仓库
git clone https://github.com/next-theme/hexo-theme-next.git next
```

### 3. 配置主题

修改 `_config.yml` 文件，将主题改为你安装的主题名称：

```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next  # 将默认的 landscape 改为你安装的主题名称
```

### 4. 预览主题

```bash
# 清理缓存
npm run clean

# 启动本地服务器
npm run server

# 在浏览器中访问 http://localhost:4000 查看效果
```

## 三、发布到 GitHub Pages

### 1. 准备工作

1. **创建 GitHub 仓库**：创建一个名为 `username.github.io` 的仓库（其中 `username` 是你的 GitHub 用户名）
2. **安装部署插件**：
   ```bash
   npm install hexo-deployer-git --save
   ```

### 2. 配置部署信息

修改 `_config.yml` 文件，添加部署配置：

```yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: gh-pages
```

### 3. 解决 SSL 证书问题

如果遇到 SSL 证书验证错误，执行以下命令：

```bash
git config --global http.sslverify false
```

### 4. 执行部署

```bash
# 生成静态文件
npm run build

# 部署到 GitHub
npm run deploy

# 或者使用强制部署
hexo deploy --force
```

### 5. 验证部署

访问 `https://username.github.io` 查看部署效果。

## 四、文章分类管理

### 1. 在文章中添加分类

在文章的 Front Matter 中添加 `categories` 字段：

```yaml
---
title: 文章标题
date: 2026-04-09 12:00:00
tags:
  - 标签1
  - 标签2
categories:
  - 分类1
  - 子分类1
---
```

### 2. 创建分类页面

```bash
# 创建分类页面
npx hexo new page categories

# 编辑 source/categories/index.md 文件
---
title: 分类
date: 2026-04-09 22:00:00
type: "categories"
---
```

### 3. 创建标签页面

```bash
# 创建标签页面
npx hexo new page tags

# 编辑 source/tags/index.md 文件
---
title: 标签
date: 2026-04-09 22:00:00
type: "tags"
---
```

### 4. 配置主题导航

修改 `themes/next/_config.yml` 文件，确保分类和标签菜单项已启用：

```yaml
menu:
  home: / || home
  categories: /categories/ || th
  tags: /tags/ || tags
  archives: /archives/ || archive
```

## 五、控制首页文章显示长度

### 方法一：使用摘要分隔符

在文章中添加 `<!-- more -->` 标记，标记之前的内容会显示在首页，标记之后的内容会被隐藏：

```markdown
# 文章标题

这是文章的开头部分，会显示在首页。

<!-- more -->

这是文章的后续部分，不会显示在首页，需要点击"阅读更多"才能查看。
```

### 方法二：配置自动摘要

在 `_config.yml` 文件中配置自动摘要：

```yaml
auto_excerpt:
  enable: true
  length: 150  # 摘要长度，单位为字符
```

## 六、解决端口占用问题

如果启动服务器时遇到端口被占用的问题：

1. **查找占用端口的进程**：
   ```bash
   netstat -ano | findstr :4000
   ```

2. **终止占用端口的进程**：
   ```bash
   taskkill /PID <进程ID> /F
   ```

3. **重新启动服务器**：
   ```bash
   npm run server
   ```

## 七、其他实用技巧

### 1. 创建新文章

```bash
npx hexo new "文章标题"
```

### 2. 清理缓存

```bash
npm run clean
```

### 3. 查看本地服务器

```bash
npm run server
# 访问 http://localhost:4000
```

### 4. 生成静态文件

```bash
npm run build
```

## 八、实战案例

### 1. 更换为 NexT 主题

1. 安装主题：`npm install hexo-theme-next`
2. 复制主题文件：`xcopy node_modules\hexo-theme-next themes\next /E /I`
3. 修改配置：`theme: next`
4. 清理缓存并启动服务器：`npm run clean && npm run server`

### 2. 发布博客到 GitHub

1. 创建 GitHub 仓库：`username.github.io`
2. 安装部署插件：`npm install hexo-deployer-git --save`
3. 配置部署信息：修改 `_config.yml` 文件
4. 解决 SSL 问题：`git config --global http.sslverify false`
5. 执行部署：`hexo deploy --force`
6. 验证部署：访问 `https://username.github.io`

## 九、总结

通过本指南，你已经掌握了 Hexo 博客的基本操作，包括：

- 更换主题
- 发布到 GitHub Pages
- 管理文章分类
- 控制首页文章显示长度
- 解决常见问题

Hexo 是一个功能强大且易于使用的静态博客框架，通过合理的配置和使用，可以创建出美观、高效的个人博客。希望本指南对你有所帮助，祝你在博客创作之路上取得成功！
