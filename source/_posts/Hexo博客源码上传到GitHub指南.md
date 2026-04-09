---
title: Hexo博客源码上传到GitHub指南
date: 2026-04-06 10:00:00
categories: [Hexo, 博客管理]
tags: [Hexo, GitHub, 源码管理]
---

# Hexo博客源码上传到GitHub指南

## 为什么需要上传源码到GitHub

将Hexo博客的源码上传到GitHub有以下几个好处：

- **版本控制**：通过Git进行版本管理，可以追踪代码变更历史
- **备份**：将源码存储在GitHub上，防止本地数据丢失
- **协作**：方便与他人协作开发或分享代码
- **部署方便**：可以在任何设备上克隆源码并进行部署
- **持续集成**：可以配置CI/CD流程自动部署博客

<!-- more -->

## 准备工作

在开始之前，确保你已经：

1. 安装了Git
2. 拥有GitHub账号
3. 已经创建了用于部署GitHub Pages的仓库（通常是 `username.github.io`）

## 详细步骤

### 1. 初始化Git仓库

如果你的Hexo项目还没有初始化Git仓库，先进行初始化：

```bash
git init
git add .
git commit -m "初始化Hexo项目"
```

### 2. 创建GitHub仓库

如果还没有创建GitHub仓库：

1. 登录GitHub
2. 点击右上角的「+」号，选择「New repository」
3. 仓库名称填写 `username.github.io`（username是你的GitHub用户名）
4. 选择公开或私有仓库
5. 点击「Create repository」

### 3. 配置远程仓库

将本地仓库与GitHub仓库关联：

```bash
git remote add origin https://github.com/username/username.github.io.git
```

### 4. 创建并切换到source分支

创建一个专门用于存储源码的分支，通常命名为 `source`：

```bash
git checkout -b source
```

### 5. 配置.gitignore文件

确保你的 `.gitignore` 文件包含以下内容，避免将不必要的文件提交到仓库：

```
# Dependency directories
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Build output
dist/
public/

# Environment variables
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# IDE and editor files
.vscode/
.idea/
*.swp
*.swo
*~

# OS generated files
Thumbs.db
.DS_Store

# Log files
logs
*.log
```

### 6. 提交源码

将所有源码文件提交到source分支：

```bash
git add .
git commit -m "提交Hexo博客源码"
```

### 7. 推送到GitHub

将source分支推送到GitHub：

```bash
git push -u origin source
```

### 8. 配置部署设置

在Hexo的 `_config.yml` 文件中，配置部署设置：

```yaml
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: master  # 或 main，取决于GitHub默认分支名
```

### 9. 部署博客

使用Hexo命令部署博客到GitHub Pages：

```bash
hexo clean
hexo generate
hexo deploy
```

## 工作流程

完成上述设置后，你的工作流程应该是：

1. 在本地修改博客内容（添加文章、修改主题等）
2. 提交源码到source分支：
   ```bash
   git add .
   git commit -m "更新内容"
   git push origin source
   ```
3. 部署博客到GitHub Pages：
   ```bash
   hexo clean
   hexo generate
   hexo deploy
   ```

## 常见问题与解决方案

### SSL证书错误

如果在执行Git操作时遇到SSL证书错误，可以运行以下命令解决：

```bash
git config --global http.sslverify false
```

### 分支冲突

如果推送时遇到分支冲突，先拉取远程分支并解决冲突：

```bash
git pull origin source --rebase
git push origin source
```

### 部署失败

如果部署失败，检查以下几点：

1. GitHub仓库配置是否正确
2. 分支名称是否正确
3. Git凭证是否有效
4. 网络连接是否正常

## 总结

通过以上步骤，你已经成功将Hexo博客的源码上传到GitHub的source分支，并配置了自动部署到GitHub Pages的功能。这样，你不仅可以通过GitHub管理源码，还可以在任何设备上克隆仓库并继续开发你的博客。

定期备份源码是一个好习惯，建议每次对博客进行重要修改后都提交并推送源码到GitHub，以确保数据安全。