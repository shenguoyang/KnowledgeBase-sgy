---
tags:
  - seed
  - project
  - guide
created: 2026-05-30
updated: 2026-05-30
topic: ai
---

# Git 使用手册

## 1. Git 简介

Git 是一个**分布式版本控制系统**，由 Linus Torvalds 于 2005 年创建，用于高效管理各类项目的源代码。

**核心优势：**

- **分布式** — 每个开发者本地都有完整的仓库副本，无需网络也能提交、查看历史
- **分支轻量** — 创建、切换、合并分支极快，鼓励频繁使用分支
- **本地操作** — 绝大多数操作在本地完成，速度快，不受网络影响
- **数据完整性** — 基于 SHA-1 哈希，任何改动都能被检测到

**与 SVN 的关键区别：**

| 特性 | Git | SVN |
|------|-----|-----|
| 架构 | 分布式 | 集中式 |
| 离线提交 | 支持 | 不支持 |
| 分支 | 轻量指针 | 目录拷贝 |
| 存储 | 快照 | 差异 |

---

## 2. 安装与配置

### 2.1 Windows 安装

从 [https://git-scm.com/downloads/win](https://git-scm.com/downloads/win) 下载安装包，安装时推荐选择 **Git Bash** 作为终端工具。

安装完成后，右键桌面或文件夹会出现 `Git Bash Here` 选项。

### 2.2 基本配置

```bash
# 配置用户名和邮箱（必做）
git config --global user.name "你的名字"
git config --global user.email "你的邮箱@example.com"

# 配置默认分支名为 main
git config --global init.defaultBranch main

# 配置换行符处理（Windows）
git config --global core.autocrlf true

# 查看当前配置
git config --list
```

配置文件位置：
- 全局配置：`~/.gitconfig`（即 `C:\Users\你的用户名\.gitconfig`）
- 项目级配置：`项目目录/.git/config`

### 2.3 SSH Key 配置（连接 GitHub）

```bash
# 1. 生成 SSH Key（一路回车即可）
ssh-keygen -t ed25519 -C "你的邮箱@example.com"

# 2. 查看公钥内容
cat ~/.ssh/id_ed25519.pub

# 3. 将公钥内容添加到 GitHub
#    GitHub → Settings → SSH and GPG keys → New SSH key
```

验证连接：

```bash
ssh -T git@github.com
# 出现 "Hi xxx!" 即配置成功
```

---

## 3. 核心概念

Git 有四个工作区域：

```
工作区(Workspace)  ──add──▶  暂存区(Index/Staging)  ──commit──▶  本地仓库(Local Repo)  ──push──▶  远程仓库(Remote)
       ◀──────────────────  checkout/restore  ──────────────────
       ◀──────────────────────────  pull/fetch ─────────────────────────────────────
```

| 区域 | 说明 | 对应位置 |
|------|------|----------|
| 工作区 | 你正在编辑的文件 | 项目文件夹 |
| 暂存区 | 准备提交的改动 | `.git/index` |
| 本地仓库 | 已提交的历史记录 | `.git/` 目录 |
| 远程仓库 | GitHub/GitLab 等服务器 | 远程服务器 |

**文件状态流转：**

- **未跟踪 (Untracked)** — 新文件，未被 Git 管理
- **已修改 (Modified)** — 已跟踪的文件被修改
- **已暂存 (Staged)** — 改动已加入暂存区，等待提交
- **已提交 (Committed)** — 改动已安全存入本地仓库

---

## 4. 基本工作流

### 4.1 创建仓库

```bash
# 方式一：在本地初始化新仓库
git init

# 方式二：克隆远程仓库
git clone https://github.com/用户名/仓库名.git
git clone git@github.com:用户名/仓库名.git   # SSH 方式（推荐）
```

### 4.2 日常提交流程

```bash
# 1. 查看当前状态
git status

# 2. 将文件添加到暂存区
git add 文件名           # 添加指定文件
git add .                # 添加所有改动
git add -A               # 添加所有改动（包括删除）

# 3. 提交到本地仓库
git commit -m "提交说明"

# 4. 推送到远程仓库
git push                 # 推送到默认远程分支
git push origin main     # 推送到指定分支
```

### 4.3 拉取更新

```bash
# 拉取并合并远程更新（最常用）
git pull

# 只拉取不合并（先看看有什么更新）
git fetch
git diff origin/main    # 查看远程与本地的差异
git merge origin/main    # 手动合并
```

### 4.4 一个典型的日常工作流

```bash
git pull                    # 1. 开始工作前，先拉取最新代码
# ... 写代码 ...
git status                  # 2. 查看改动了哪些文件
git add .                   # 3. 暂存所有改动
git commit -m "feat: 添加用户登录功能"   # 4. 提交
git push                    # 5. 推送
```

---

## 5. 分支管理

### 5.1 分支基本操作

```bash
# 查看分支
git branch                  # 本地分支列表
git branch -r               # 远程分支列表
git branch -a               # 所有分支

# 创建分支
git branch 分支名           # 基于当前分支创建
git branch 分支名 commit-id # 基于指定提交创建

# 切换分支
git checkout 分支名         # 传统方式
git switch 分支名           # Git 2.23+ 推荐方式

# 创建并切换（一步到位）
git checkout -b 分支名
git switch -c 分支名

# 删除分支
git branch -d 分支名        # 安全删除（已合并的）
git branch -D 分支名        # 强制删除
```

### 5.2 合并分支

```bash
# 切换到目标分支（通常是 main）
git checkout main

# 合并指定分支到当前分支
git merge feature-branch

# 合并时如果发生冲突：
# 1. 打开冲突文件，手动解决冲突标记（<<<<<<< ======= >>>>>>>）
# 2. git add 冲突文件
# 3. git commit 完成合并
```

### 5.3 Rebase — 整理提交历史

```bash
# 将当前分支变基到 main 上
git checkout feature-branch
git rebase main

# 交互式 rebase：合并、编辑、删除历史提交
git rebase -i HEAD~3       # 整理最近 3 个提交
```

> **注意：** 不要对已推送到远程的公共分支执行 rebase。

### 5.4 分支策略 (Git Flow 简介)

```
main        ───●────────────●─────────●──────  生产环境
                \          / \       /
develop         ●──●──●──●───●──●──●───────  开发主线
                      \       /
feature/login         ●──●──●               功能分支
```

- **main** — 生产环境代码，只通过 PR/MR 合并
- **develop** — 开发主线，功能分支合并到这里
- **feature/xxx** — 新功能开发分支
- **hotfix/xxx** — 紧急修复分支
- **release/xxx** — 发布准备分支

---

## 6. 查看历史

### 6.1 git log — 提交历史

```bash
git log                     # 完整提交历史
git log --oneline           # 一行显示，简洁模式
git log --oneline -10       # 最近 10 条
git log --graph --oneline   # 显示分支图
git log --author="用户名"    # 按作者筛选
git log --since="2026-01-01"  # 按时间筛选
git log 文件名               # 查看某文件的历史
```

### 6.2 git diff — 差异对比

```bash
git diff                    # 工作区 vs 暂存区（未暂存的改动）
git diff --staged           # 暂存区 vs 最新提交
git diff HEAD               # 工作区 vs 最新提交
git diff 分支1..分支2        # 两个分支的差异
git diff commit1 commit2     # 两个提交的差异
```

### 6.3 其他查询命令

```bash
git show 提交ID              # 查看某次提交的详细信息
git show 提交ID 文件名        # 查看某次提交中某文件的改动
git blame 文件名              # 查看文件每一行的最后修改者和提交
git reflog                   # 查看所有 HEAD 移动记录（救命命令）
```

---

## 7. 撤销与回退

### 7.1 撤销工作区改动

```bash
# 丢弃某个文件的未暂存修改
git restore 文件名
git checkout -- 文件名       # 旧写法

# 丢弃所有未暂存修改
git restore .
```

### 7.2 撤销暂存区

```bash
# 将文件从暂存区移出（保留工作区改动）
git restore --staged 文件名
git reset HEAD 文件名         # 旧写法
```

### 7.3 撤销提交

```bash
# 撤销最近一次提交，改动回到暂存区（保留修改）
git reset --soft HEAD~1

# 撤销最近一次提交，改动回到工作区（保留修改）
git reset --mixed HEAD~1     # 默认行为
git reset HEAD~1             # 同上

# 撤销最近一次提交，丢弃所有修改（危险！）
git reset --hard HEAD~1

# 安全撤销：创建一个反向提交，不改变历史
git revert 提交ID
```

### 7.4 git stash — 临时保存

```bash
git stash                   # 暂存当前改动（让工作区变干净）
git stash save "说明"        # 带说明的暂存
git stash list              # 查看暂存列表
git stash pop               # 恢复最近一次暂存并删除记录
git stash apply             # 恢复最近一次暂存但保留记录
git stash drop              # 删除最近一次暂存
```

---

## 8. 远程协作

### 8.1 远程仓库管理

```bash
# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin git@github.com:用户名/仓库名.git

# 修改远程仓库地址
git remote set-url origin git@github.com:用户名/新仓库名.git

# 删除远程仓库
git remote remove origin
```

### 8.2 推送与拉取

```bash
# 推送当前分支到远程
git push
git push origin 分支名

# 首次推送并设置上游分支
git push -u origin 分支名

# 强制推送（危险！会覆盖远程历史）
git push --force
git push --force-with-lease  # 更安全的强制推送

# 拉取远程分支
git pull
git pull --rebase            # 拉取并 rebase（推荐，避免多余合并提交）

# 只拉取不合并
git fetch
```

### 8.3 Pull Request / Merge Request 流程

1. **Fork** 或创建功能分支 `git checkout -b feature/xxx`
2. 开发并提交 `git add . && git commit -m "xxx" && git push -u origin feature/xxx`
3. 在 GitHub/GitLab 上创建 PR/MR
4. 代码评审，根据反馈修改并推送（会自动更新 PR）
5. 评审通过后合并到目标分支
6. 删除功能分支

---

## 9. 标签管理

标签用于标记重要的版本节点（如发布版本）。

```bash
# 创建标签
git tag v1.0.0                           # 轻量标签
git tag -a v1.0.0 -m "版本说明"           # 附注标签（推荐）

# 为历史提交打标签
git tag -a v0.9.0 提交ID -m "版本说明"

# 查看标签
git tag                                  # 列出所有标签
git tag -l "v1.*"                        # 通配符筛选
git show v1.0.0                          # 查看标签详情

# 推送标签
git push origin v1.0.0                   # 推送单个标签
git push origin --tags                   # 推送所有标签

# 删除标签
git tag -d v1.0.0                        # 删除本地标签
git push origin --delete v1.0.0          # 删除远程标签
```

---

## 10. 实用场景速查

### 10.1 提交了不该提交的文件

```bash
# 如果还没 push：修改 .gitignore，然后
git rm --cached 文件名
git commit -m "chore: 移除误提交的文件"

# 如果已经 push：同上操作后 git push
```

### 10.2 回退某次特定提交

```bash
# 安全方式：创建一个反向提交
git revert 提交ID
```

### 10.3 把另一个分支的某个提交拿过来

```bash
# cherry-pick：将指定提交应用到当前分支
git cherry-pick 提交ID
```

### 10.4 合并多个 commit 为一个

```bash
# 交互式 rebase，将后两个 pick 改为 squash
git rebase -i HEAD~3

# 编辑器中将：
# pick abc1234 提交1
# pick def5678 提交2   ← 改为 squash
# pick ghi9012 提交3   ← 改为 squash
```

### 10.5 提交到错误的分支

```bash
# 1. 先不要动，记录当前提交 ID
git log --oneline -1

# 2. 切换到正确分支
git checkout 正确的分支

# 3. cherry-pick 过来
git cherry-pick 提交ID

# 4. 回到错误分支，撤销那次提交
git checkout 错误的分支
git reset --hard HEAD~1
```

### 10.6 忽略已跟踪文件的后续改动

```bash
git update-index --assume-unchanged 文件名    # 忽略改动
git update-index --no-assume-unchanged 文件名  # 恢复跟踪
```

### 10.7 找回误删的分支或提交

```bash
git reflog                   # 查看所有 HEAD 操作记录
git checkout -b 恢复的分支名 提交ID   # 从 reflog 中找到的 ID 恢复
```

---

## 11. 最佳实践

### 11.1 Commit Message 规范

推荐使用 **约定式提交 (Conventional Commits)** 格式：

```
<类型>(<范围>): <简短描述>

类型：
  feat      — 新功能
  fix       — 修复 bug
  docs      — 文档修改
  style     — 代码格式（不影响功能）
  refactor  — 重构
  perf      — 性能优化
  test      — 测试相关
  chore     — 构建/工具/依赖变更
  revert    — 回退

示例：
  feat(auth): 添加 JWT 登录验证
  fix(api): 修复用户查询空指针异常
  refactor(db): 提取数据库连接池配置
```

### 11.2 分支命名约定

```
feature/用户登录        — 新功能
bugfix/修复登录超时      — bug 修复
hotfix/紧急修复生产崩溃   — 紧急修复
release/v2.0.0          — 发布分支
```

### 11.3 .gitignore 配置

在项目根目录创建 `.gitignore`，用于排除不需要版本控制的文件：

```gitignore
# 依赖
node_modules/
vendor/
__pycache__/
*.pyc

# 构建产物
dist/
build/
*.exe
*.dll

# IDE
.vscode/
.idea/
*.swp
*.swo

# 环境变量
.env
.env.local

# 系统文件
.DS_Store
Thumbs.db

# 日志
*.log
```

### 11.4 日常原则

- **原子提交** — 一次提交只做一件事，方便回溯和 revert
- **频繁推送** — 每天结束前 push，避免本地代码丢失
- **先 pull 再 push** — 推送前先拉取最新代码，减少冲突
- **不要提交敏感信息** — 密码、Token、密钥等用环境变量管理
- **分支开发** — 不在 main 分支上直接开发，通过功能分支 + PR 合并
- **代码评审** — 合并前至少一人 review，有助于发现问题和知识共享
