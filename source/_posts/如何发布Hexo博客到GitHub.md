---
title: 如何发布Hexo博客到GitHub
date: 2026-04-04 20:37:10
tags:
  - Hexo
  - GitHub
  - 部署
  - 教程
---

# 如何发布Hexo博客到GitHub

## 为什么要发布到GitHub？

GitHub Pages 是 GitHub 提供的静态网站托管服务，它具有以下优势：

- **免费**：无需支付任何费用
- **稳定**：由 GitHub 提供可靠的服务器
- **易于使用**：与 Git 版本控制集成
- **支持自定义域名**：可以使用自己的域名
- **无需管理服务器**：不需要维护服务器，专注于内容创作

<!-- more -->

## 准备工作

### 1. 创建 GitHub 仓库

1. 登录你的 GitHub 账号
2. 点击右上角的 "+", 选择 "New repository"
3. 仓库名填写 `username.github.io`（其中 `username` 是你的 GitHub 用户名）
4. 选择 "Public" 作为仓库可见性
5. 点击 "Create repository"

### 2. 安装部署插件

在博客根目录执行以下命令安装 `hexo-deployer-git` 插件：

```bash
npm install hexo-deployer-git --save
```


## 配置部署信息

### 1. 修改配置文件

打开 `_config.yml` 文件，找到 `deploy` 部分，修改为：

```yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: gh-pages
```

将 `username` 替换为你的 GitHub 用户名。

### 2. 配置 Git

确保你已经配置了 Git 的用户信息：

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## 解决 SSL 证书问题

如果在部署过程中遇到 SSL 证书验证错误，可以执行以下命令：

```bash
git config --global http.sslverify false
```

**注意**：这会禁用 Git 的 SSL 证书验证，可能会带来安全风险，仅在必要时使用。

## 执行部署

### 1. 生成静态文件

```bash
npm run build
```

### 2. 部署到 GitHub

```bash
npm run deploy
```

或者使用 Hexo 命令：

```bash
hexo deploy
```

如果需要强制覆盖仓库，可以使用：

```bash
hexo deploy --force
```

## 验证部署

部署完成后，你可以通过以下步骤验证：

1. 访问 `https://username.github.io`（其中 `username` 是你的 GitHub 用户名）
2. 检查博客是否正常显示
3. 确认所有文章和页面都已正确部署

## 后续更新

当你添加新文章或修改博客内容后，只需再次执行以下命令即可更新 GitHub Pages 上的内容：

```bash
npm run deploy
```

## 常见问题及解决方案

### 1. 部署失败，出现 SSL 证书错误

**解决方案**：执行 `git config --global http.sslverify false` 命令禁用 SSL 证书验证。

### 2. 部署成功但网站不显示

**解决方案**：
- 等待几分钟，GitHub Pages 需要时间生成页面
- 检查仓库设置，确保 GitHub Pages 源设置为 `gh-pages` 分支
- 检查 `_config.yml` 中的 `url` 配置是否正确

### 3. 图片或资源文件不显示

**解决方案**：
- 确保使用相对路径或正确的绝对路径
- 检查文件权限
- 执行 `hexo clean` 清理缓存后重新部署

## 实战案例

以下是我发布博客到 GitHub 的步骤：

1. 创建 GitHub 仓库：`540760029/540760029.github.io`
2. 安装部署插件：`npm install hexo-deployer-git --save`
3. 配置部署信息：修改 `_config.yml` 文件
4. 解决 SSL 问题：`git config --global http.sslverify false`
5. 执行部署：`hexo deploy --force`
6. 验证部署：访问 `https://540760029.github.io`

## 总结

发布 Hexo 博客到 GitHub Pages 是一个简单但强大的过程，它让你能够免费托管你的博客并通过互联网访问。通过遵循本教程的步骤，你可以轻松地将你的 Hexo 博客部署到 GitHub，并在未来方便地更新内容。

希望本教程对你有所帮助！如果你有任何问题，欢迎在评论区留言。
