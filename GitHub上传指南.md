# GitHub 上传指南

本文档记录了将本地笔记上传到 GitHub 的完整过程，包含每一步的 Git 命令和解释，方便学习和参考。

## 最终结果

您的笔记已成功上传到 GitHub 仓库：  
📦 **https://github.com/Sats-Jz/Obsidian_JZ**

## 环境准备

在开始之前，确保已安装以下工具并完成配置：

1. **Git** - 版本控制系统
   ```bash
   git --version
   ```

2. **GitHub CLI (gh)** - GitHub 命令行工具（可选但推荐）
   ```bash
   gh --version
   ```

3. **Git 用户配置** - 设置用户名和邮箱
   ```bash
   git config --global user.name "您的用户名"
   git config --global user.email "您的邮箱"
   ```

4. **GitHub 登录** - 通过 GitHub CLI 登录
   ```bash
   gh auth login
   ```

## 完整上传流程

### 步骤 1：进入项目目录
```bash
cd "D:\Obsidian_JZ"
```

### 步骤 2：创建 .gitignore 文件（可选但推荐）

`.gitignore` 文件用于指定不需要上传到 Git 的文件和目录。我们创建了一个基础版本：

```gitignore
# OS generated files
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Editor files
*.swp
*~
*.sublime-*

# Obsidian
.obsidian/
.trash/

# Temporary files
*.tmp
*.temp

# Log files
*.log

# System files
desktop.ini
```

### 步骤 3：初始化 Git 仓库
```bash
git init
```
这将创建一个新的 Git 仓库，在当前目录生成 `.git` 文件夹。

### 步骤 4：处理嵌套 Git 仓库（如有）

如果目录中包含其他 Git 仓库（如子模块），需要特殊处理。我们遇到了一个嵌套的 `.git` 目录：

```bash
# 查找所有 .git 目录
find . -name ".git" -type d

# 将嵌套的 .git 目录重命名（避免冲突）
mv "Notes-Typroa/java/简历/.git" "Notes-Typroa/java/简历/.git.bak"
```

**注意**：如果嵌套仓库需要保留历史，应考虑使用 `git submodule` 而不是简单重命名。

### 步骤 5：添加所有文件到暂存区
```bash
git add .
```
- `.` 表示当前目录所有文件
- 此命令会将所有未忽略的文件添加到 Git 的暂存区
- 在 Windows 上可能会看到 "LF will be replaced by CRLF" 警告，这是正常的

### 步骤 6：提交更改
```bash
git commit -m "Initial commit"
```
- `-m` 参数指定提交信息
- 好的提交信息应该简洁明了，说明本次提交的目的
- 本次提交了 441 个文件，28,662 行新增

### 步骤 7：创建 GitHub 仓库并推送

使用 GitHub CLI 一键创建仓库并推送代码：

```bash
gh repo create Obsidian_JZ --public --source=. --remote=origin --push
```

参数说明：
- `Obsidian_JZ` - 仓库名称（与文件夹名相同）
- `--public` - 创建公开仓库（使用 `--private` 创建私有仓库）
- `--source=.` - 使用当前目录作为源代码
- `--remote=origin` - 设置远程仓库名称为 `origin`
- `--push` - 自动推送代码到远程仓库

### 步骤 8：验证推送结果

```bash
# 检查远程仓库配置
git remote -v

# 查看仓库状态
git status

# 查看提交历史
git log --oneline -5

# 获取远程分支信息
git fetch origin
git branch -r
```

## 遇到的问题和解决方案

### 1. 大文件警告

推送时出现以下警告：
```
remote: warning: File Notes-Typroa/java/简历/jianli.pdf is 51.87 MB; 
this is larger than GitHub's recommended maximum file size of 50.00 MB
```

**解决方案**：
- GitHub 建议单个文件不超过 50 MB
- 对于大文件，可以考虑：
  1. 使用 Git Large File Storage (LFS)
  2. 压缩文件
  3. 分割文件
  4. 使用外部存储链接

### 2. 嵌套 Git 仓库

错误信息：
```
error: 'Notes-Typroa/java/简历/' does not have a commit checked out
fatal: adding files failed
```

**原因**：目录中包含另一个 Git 仓库的 `.git` 文件夹。

**解决方案**：
- 重命名嵌套的 `.git` 目录
- 或使用 `git submodule` 管理嵌套仓库

## 常用 Git 命令速查

### 基础命令
```bash
# 初始化仓库
git init

# 添加文件
git add <文件名>
git add .              # 添加所有文件

# 提交更改
git commit -m "提交信息"

# 查看状态
git status

# 查看提交历史
git log
git log --oneline     # 简洁模式
```

### 远程仓库操作
```bash
# 添加远程仓库
git remote add origin <仓库URL>

# 推送到远程
git push origin master
git push -u origin master  # 设置上游分支

# 拉取更新
git pull origin master

# 克隆仓库
git clone <仓库URL>
```

### 分支管理
```bash
# 创建分支
git branch <分支名>

# 切换分支
git checkout <分支名>

# 创建并切换分支
git checkout -b <分支名>

# 合并分支
git merge <分支名>
```

## 下一步建议

1. **定期备份**：建议定期提交和推送更改
   ```bash
   git add .
   git commit -m "更新笔记"
   git push origin master
   ```

2. **忽略敏感信息**：确保 `.gitignore` 中包含所有敏感文件
   - 密码、密钥文件
   - 配置文件
   - 临时文件

3. **使用分支**：为不同的功能或主题创建分支
   ```bash
   git checkout -b feature/new-notes
   # 添加新笔记
   git add .
   git commit -m "添加新笔记"
   git push origin feature/new-notes
   ```

4. **协作功能**：如果与他人协作，可以使用 Issues 和 Pull Requests

## 参考资料

- [Git 官方文档](https://git-scm.com/doc)
- [GitHub CLI 文档](https://cli.github.com/)
- [GitHub 大文件存储](https://git-lfs.github.com/)
- [.gitignore 模板](https://github.com/github/gitignore)

---

✅ **上传完成！** 您的笔记现在可以在 https://github.com/Sats-Jz/Obsidian_JZ 查看和访问。

如有问题，请参考上述命令或查阅相关文档。