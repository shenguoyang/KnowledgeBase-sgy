---
tags:
  - seed
  - project
  - guide
created: 2026-05-30
updated: 2026-05-30
topic: ai
---

# 安卓手机 + Obsidian + Git 自动同步指南

## 1. 概述

本指南介绍如何在安卓手机上实现 **Obsidian 笔记编辑后自动同步到 GitHub**，并与 PC 端无缝衔接。

**最终效果：**

```
手机端：打开 Obsidian → 自动拉取最新 → 编辑笔记 → 关闭 Obsidian → 自动推送
PC 端：开始工作前 git pull → 编辑 → git push
```

---

## 2. 方案对比

| 方案                      | 自动同步      | 难度      | 稳定性   | 推荐    |
| ----------------------- | --------- | ------- | ----- | ----- |
| **Git Sync 插件**         | ✅ 打开/关闭自动 | ⭐ 极简    | ⭐⭐⭐⭐  | 🏆 首选 |
| **Termux + Tasker**     | ✅ 最强自动化   | ⭐⭐⭐⭐ 复杂 | ⭐⭐⭐⭐⭐ | 进阶    |
| **GitHub Gitless Sync** | ✅ 定时自动    | ⭐⭐ 简单   | ⭐⭐⭐⭐  | 备选    |
| Obsidian Git 内置         | ✅ 但不稳定    | ⭐⭐      | ⭐ 易崩溃 | ❌ 不推荐 |

> **建议**：90% 的用户直接选 **方案一（Git Sync）**，开箱即用。追求极致可靠再上方案二。

---

## 4. 方案二：Termux + Tasker（进阶·最强自动化）

如果你追求极致的稳定性和自动化程度，这是最强大的方案。

### 4.1 概述

```
┌──────────────────────────────────────────────────┐
│  Tasker（自动化引擎）                               │
│  ├─ Obsidian 打开 → 触发 Termux 执行 git pull      │
│  ├─ Obsidian 关闭 → 触发 Termux 执行 git push      │
│  └─ 每天凌晨 4 点 → 定时全量同步                    │
└──────────────────────────────────────────────────┘
         │
         ▼
┌──────────────────────────────────────────────────┐
│  Termux（Linux 终端模拟器）                         │
│  ├─ 原生 Git + SSH 环境                            │
│  └─ 与 Tasker 联动执行脚本                         │
└──────────────────────────────────────────────────┘
```

### 4.2 安装核心应用

> ⚠️ **关键**：Termux 必须从 **F-Droid** 安装（Google Play 版本已停止更新，功能不完整）。

**步骤一：安装 F-Droid**

用手机浏览器访问 [https://f-droid.org](https://f-droid.org)，下载并安装 F-Droid。

**步骤二：从 F-Droid 安装以下应用**

| 应用                | 用途                  |
| ----------------- | ------------------- |
| **Termux**        | Linux 终端模拟器         |
| **Termux:Tasker** | Termux 与 Tasker 的桥梁 |
| **Termux:API**    | 提供 Android API 访问   |

**步骤三：安装 Tasker**

从 Google Play 安装 **Tasker**（付费应用，自动化神器）。

### 4.3 配置 Termux 环境

打开 Termux，逐条执行：

```bash
# 1. 授权存储访问
termux-setup-storage
# 弹出权限请求 → 允许

# 2. 更新包管理器
pkg update && pkg upgrade -y

# 3. 安装 Git、SSH 和 API
pkg install -y git openssh termux-api

# 4. 创建 Obsidian 仓库目录
mkdir -p /storage/emulated/0/repos/Obsidian

git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub邮箱"
```

### 4.4 克隆自动化脚本

项目 `obsidian-android-sync` 封装了完整的同步脚本：

```bash
git clone https://github.com/DovieW/obsidian-android-sync.git \
  ~/storage/shared/repos/obsidian-android-sync
```

运行配置脚本：

```bash
cp "/storage/emulated/0/repos/obsidian-android-sync/setup" ~/
chmod +x "$HOME/setup"
source "$HOME/setup"
```

脚本会自动：

- 生成 SSH 密钥对
- 将公钥复制到剪贴板
- 创建 Termux 启动脚本

### 4.5 配置 SSH Key

1. 脚本已将公钥复制到剪贴板
2. GitHub → Settings → SSH and GPG keys → **New SSH key**
3. Title: `Android Termux`
4. Key: 粘贴公钥，点击 Add

验证连接：

```bash
ssh -T git@github.com
# 出现 "Hi xxx!" 即成功
```

### 4.6 Clone 知识库仓库

```bash
cd /storage/emulated/0/repos/Obsidian
git clone git@github.com:你的用户名/仓库名.git
```

> ⚠️ 仓库名避免使用特殊字符和空格。

### 4.7 执行修复脚本

```bash
"${HOME}/worktree-fix.sh"
```

此脚本修复 Android 文件系统与 Git 的兼容性问题。

### 4.8 配置 .gitignore 和 .gitattributes

```bash
cd /storage/emulated/0/repos/Obsidian/你的仓库名
```

编辑 `.gitignore`：

```gitignore
# Obsidian 工作区配置（设备不同，避免冲突）
/.obsidian/workspace.json
/.obsidian/workspace-mobile.json
/.obsidian/plugins/obsidian-git/data.json

# 冲突文件
/conflict-files-obsidian-git.md

# 垃圾箱
.trash/

# Claude Code 配置
.claude/

# 系统文件
.DS_Store
Thumbs.db
```

编辑 `.gitattributes`：

```
*.md merge=union
```

### 4.9 打开 Obsidian

打开 Obsidian → **Open folder as vault** → 导航到：

```
/storage/emulated/0/repos/Obsidian/你的仓库名
```

选择文件夹即可加载。

### 4.10 配置 Tasker 自动化

**步骤一：授权 Tasker**

1. 打开 Tasker → 设置 → 勾选 **Termux** 权限
2. Android 设置 → 应用 → Termux → **显示在其他应用的上层** → 允许

**步骤二：导入自动化项目**

1. 在手机文件管理器中找到：
   
   ```
   /storage/emulated/0/repos/obsidian-android-sync/
   ```
2. 找到 Tasker 项目 XML 文件
3. 打开 Tasker → 长按底部任务栏 → **Import** → 选择 XML 文件
4. 按提示授予所有权限

**步骤三：（可选）添加桌面小组件**

长按桌面 → 添加 Tasker 小组件：

| 组件                | 功能                      |
| ----------------- | ----------------------- |
| **Sync Vaults**   | 手动触发全量同步                |
| **Vaults Status** | 显示每个 Vault 的 git status |
| **Sync Log**      | 查看同步日志                  |

### 4.11 自动化效果

配置完成后，以下操作**全自动**：

| 触发条件        | 自动操作                                                 |
| ----------- | ---------------------------------------------------- |
| 打开 Obsidian | `git pull` 拉取最新                                      |
| 关闭 Obsidian | `git add . && git commit -m "auto sync" && git push` |
| 每天凌晨 4:00   | 定时全量同步                                               |
| 同步失败        | 推送通知提醒                                               |

---

## 5. PC 端配合使用

手机端自动化之后，PC 端只需保持常规 Git 操作即可：

```bash
# 开始工作前拉取手机端的更新
git pull

# ... 在 PC 上编辑笔记 ...

# 完成编辑后推送
git add .
git commit -m "docs: 更新技术笔记"
git push
```

> ⚠️ **重要原则**：无论手机还是 PC，**开始编辑前先 pull，编辑完成后立即 push**。这个习惯能避免 90% 的冲突。

---

## 6. 冲突处理

### 6.1 为什么会出现冲突？

两端的修改同时改了同一个文件的同一位置，Git 无法自动判断以谁为准。

### 6.2 预防冲突

- 编辑前先 pull
- 编辑完尽快 push
- 不要同时在手机和 PC 编辑同一个文件
- 使用 `.gitattributes` 中的 `merge=union`（对 Markdown 笔记非常有效）

### 6.3 冲突发生后的处理

**方案一用户（Git Sync）：**

在 Git Sync 中查看冲突文件 → 手动编辑解决冲突标记（`<<<<<<<`、`=======`、`>>>>>>>`）→ 在 Obsidian 中保存 → Git Sync 重新 commit。

**方案二用户（Termux）：**

```bash
cd /storage/emulated/0/repos/Obsidian/你的仓库名

# 查看冲突文件
git status

# 在 Obsidian 中手动编辑冲突文件，删除冲突标记

# 标记冲突已解决
git add .

# 完成合并
git commit -m "fix: 解决合并冲突"
git push
```

---

## 7. 常见问题（FAQ）

### 7.1 SSH 连接失败

**症状**：`Permission denied (publickey)` 或 `Could not resolve hostname`

**解决**：

- 确认公钥已正确添加到 GitHub
- 确认仓库使用 SSH 地址（`git@github.com:...`）而非 HTTPS
- 国内用户检查网络是否能访问 GitHub，必要时使用代理

### 7.2 Token 权限不足

**症状**：`403 Forbidden` 或 `Authentication failed`

**解决**：

- 确认 Token 勾选了 `repo` 完整权限
- 确认 Token 未过期
- 如果 Token 丢失，重新生成并替换

### 7.3 Termux 提示 "termux-setup-storage: command not found"

**解决**：从 **F-Droid** 重新安装 Termux。Google Play 版本已废弃，功能不完整。

### 7.4 Obsidian 打开仓库后空白

**解决**：

- 确认仓库目录确实包含 `.md` 文件
- 检查 `.obsidian/` 目录是否存在（首次打开 Obsidian 会自动创建）
- 尝试关闭 Vault 后重新打开

### 7.5 同步后配置混乱（主题/插件不一致）

**原因**：`.obsidian/` 目录包含了插件、主题等配置，不同设备的配置可能冲突。

**解决**：在 `.gitignore` 中排除以下文件：

```gitignore
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/plugins/
.obsidian/themes/
.obsidian/appearance.json
.obsidian/core-plugins.json
.obsidian/community-plugins.json
```

只同步笔记的 `.md` 文件，每台设备各自管理自己的 Obsidian 配置。

### 7.6 国内连接 GitHub 太慢/不稳定

**解决**：

- 使用 **Gitee（码云）** 替代 GitHub，国内访问快
- 在电脑上配置代理，手机端使用 Termux 可设置 `proxychains`
- 使用 **GitHub Gitless Sync** 插件（通过 HTTPS API 通信，兼容性更好）

### 7.7 大文件或图片同步慢

**建议**：

- 图片附件控制在 1MB 以内
- 大文件较多的仓库考虑使用 Obsidian 官方 Sync 服务（付费，$4/月）
- 或将图片单独存储（图床），笔记中引用 URL

---

## 8. 总结

| 你的需求              | 推荐方案                   |
| ----------------- | ---------------------- |
| 简单、够用、不想折腾        | 方案一：Git Sync           |
| 追求完美自动化、技术能力强     | 方案二：Termux + Tasker    |
| 不想装 Git、只用 GitHub | GitHub Gitless Sync 插件 |

**核心原则**：

1. **先 pull 再编辑** — 避免冲突的第一准则
2. **编辑完立即 push** — 不做本地积压
3. **不要同时在两端编辑同一文件** — 人为避免冲突
4. **配置好 .gitignore** — 避免设备配置互相覆盖
