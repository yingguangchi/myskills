# 自动同步Skills到GitHub

你是一个自动同步工具，负责将本地的Claude Code skills自动同步到GitHub仓库。

**重要：优先使用SSH方式，无需GitHub Token**

## 触发时机

**自动触发**：每次Claude Code启动时自动运行（通过hooks）

## 核心功能

### 1. 检查环境

在每次同步前，检查以下环境：

```bash
# 检查SSH连接（优先）
ssh -T git@github.com 2>&1 | grep -q "successfully authenticated"

# 检查git配置
git config user.name
git config user.email

# 检查skills目录
- ~/.claude/skills/ (用户级skills)
- .claude/skills/ (项目级skills，如果存在)

# 获取GitHub用户名
GITHUB_USERNAME=$(ssh -T git@github.com 2>&1 | grep -o 'Hi [^!]*' | cut -d' ' -f2)
```

### 2. 仓库管理

#### 检查GitHub远程仓库（使用SSH）

```bash
# 使用git命令检查远程仓库是否存在
check_github_repo() {
  local repo="myskills"
  local username=$(git config user.name)

  git ls-remote "git@github.com:$username/$repo.git" &>/dev/null
  return $?
}
```

#### 创建GitHub仓库（使用gh CLI或手动）

```bash
# 方法1: 使用gh CLI（如果已安装）
create_github_repo_gh() {
  gh repo create myskills --public --description "My Claude Code Skills Collection"
}

# 方法2: 使用git（推荐）
create_github_repo_git() {
  mkdir -p ~/.claude/myskills
  cd ~/.claude/myskills

  # 初始化仓库
  git init

  # 创建初始文件
  cat > README.md << 'EOF'
# My Claude Code Skills

这个仓库存储我所有的Claude Code skills，通过自动同步工具维护。

## Skills列表

EOF

  git add .
  git commit -m "Initial commit"

  # 添加远程仓库（SSH）
  git remote add origin git@github.com:$(git config user.name)/myskills.git

  # 推送（会自动创建远程仓库）
  git branch -M main
  git push -u origin main
}
```

### 3. 同步逻辑

#### 初始化本地仓库（如果不存在）

```bash
init_local_repo() {
  local skills_dir="$HOME/.claude/skills"

  cd "$skills_dir"

  # 初始化git仓库
  git init

  # 创建.gitignore
  cat > .gitignore << 'EOF'
.DS_Store
*.log
*.tmp
.*.swp
EOF

  # 创建初始README
  cat > README.md << 'EOF'
# My Claude Code Skills

这个仓库存储我所有的Claude Code skills，通过自动同步工具维护。

## Skills列表

EOF
  ls -1 "$skills_dir" | grep -v '^\.' | sed 's/^/- /' >> README.md

  cat >> README.md << 'EOF'

## 同步信息

- **同步方式**: SSH (无需token，更安全)
- **自动触发**: 每次Claude Code启动时
- **手动触发**: 对话中说"同步skills到GitHub"
EOF

  git add .
  git commit -m "Initial commit: Auto-sync skills repository"

  # 添加远程仓库（SSH方式）
  git remote add origin git@github.com:$(git config user.name)/myskills.git

  # 推送到GitHub
  git branch -M main
  git push -u origin main
}
```

#### 同步skills到仓库

```bash
sync_skills() {
  local skills_dir="$HOME/.claude/skills"

  cd "$skills_dir"

  # 拉取最新变更
  echo "📥 拉取远程更新..."
  git pull origin main --no-edit

  # 检查是否有变更
  if [ -n "$(git status --porcelain)" ]; then
    echo "📝 发现变更，提交并推送..."

    # 添加所有变更
    git add -A

    # 提交
    git commit -m "Auto-sync skills at $(date '+%Y-%m-%d %H:%M:%S')

Changes:
$(git diff --cached --name-status | head -20)"

    # 推送到GitHub（SSH方式）
    git push origin main

    echo "✅ 同步完成！"
  else
    echo "✅ 没有变更，跳过同步"
  fi
}
```

### 4. 完整同步流程

```bash
main() {
  echo "🚀 开始自动同步skills到GitHub..."

  # 1. 检查SSH连接
  if ! ssh -T git@github.com 2>&1 | grep -q "successfully authenticated"; then
    echo "❌ SSH连接失败"
    echo "💡 请先配置SSH key：https://docs.github.com/en/authentication/connecting-to-github-with-ssh"
    return 1
  fi

  # 2. 检查本地仓库
  if [ ! -d ~/.claude/skills/.git ]; then
    echo "📁 初始化本地仓库..."
    init_local_repo
  fi

  # 3. 同步skills
  sync_skills

  echo "✅ 自动同步完成！"
}
```

## SSH配置（推荐方式）

SSH方式无需token，更安全更方便。

### 检查SSH配置

```bash
# 测试SSH连接
ssh -T git@github.com

# 如果成功，会显示：
# Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

### 如果未配置SSH Key

```bash
# 1. 生成SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# 2. 复制公钥
cat ~/.ssh/id_ed25519.pub

# 3. 添加到GitHub
# 访问：https://github.com/settings/keys
# 点击 "New SSH key"
# 粘贴公钥

# 4. 测试连接
ssh -T git@github.com
```

## 首次使用指南

当用户第一次使用这个skill时，通过交互式对话完成所有配置。

### 步骤1：检查环境

```bash
echo "🔍 检查环境..."
echo "✅ Git用户: $(git config --global user.name)"
echo "✅ Git邮箱: $(git config --global user.email)"
echo "✅ SSH连接: $(ssh -T git@github.com 2>&1 | grep -o 'Hi [^!]*')"
```

### 步骤2：首次同步

```bash
echo "🎉 开始首次同步..."

# 在skills目录直接初始化
cd ~/.claude/skills
git init

# 创建初始文件
cat > README.md << 'EOF'
# My Claude Code Skills

这个仓库存储我所有的Claude Code skills，通过自动同步工具维护。

EOF
ls -1 ~/.claude/skills | grep -v '^\.' | sed 's/^/- /' >> README.md

cat > .gitignore << 'EOF'
.DS_Store
*.log
*.tmp
.*.swp
EOF

# 初始提交
git add .
git commit -m "Initial commit: Sync Claude Code skills"

# 添加远程（SSH）
git remote add origin git@github.com:$(git config user.name)/myskills.git
git branch -M main

# 推送
echo "📤 推送到GitHub..."
git push -u origin main

echo "✅ 首次同步完成！"
echo "📍 仓库地址: https://github.com/$(git config user.name)/myskills"
```

## 日常使用

### 每次启动时自动运行

通过Claude Code hooks自动触发，在对话中直接运行：

```text
同步skills到GitHub
```

AI会自动执行：
1. ✅ 检查SSH连接
2. 📥 拉取最新代码
3. 📦 复制当前skills
4. 📝 提交变更（如果有）
5. 📤 推送到GitHub

## 错误处理

### 常见错误1：SSH连接失败

```bash
# 错误信息
# Permission denied (publickey)

# 解决方案
echo "❌ SSH连接失败"
echo "💡 请检查："
echo "  1. SSH key是否已添加到GitHub"
echo "  2. 访问 https://github.com/settings/keys"
echo "  3. 或运行：ssh -T git@github.com"
```

### 常见错误2：仓库不存在

```bash
# 错误信息
# ERROR: Repository not found

# 解决方案
echo "📦 GitHub上没有找到myskills仓库"
echo "💡 首次推送会自动创建，或手动在GitHub创建"
```

### 常见错误3：网络问题

```bash
# 错误信息
# Could not resolve host / 连接超时

# 解决方案
echo "⚠️ 网络连接失败，请检查："
echo "  1. 网络连接"
echo "  2. 代理设置（如需要）"
echo "  3. DNS解析"
```

## 使用场景

### 场景1：首次同步

```
你：同步skills到GitHub

我：🚀 开始首次同步...

📋 检查环境：
✅ Git用户: yingguangchi
✅ Git邮箱: yingguangchi@gmail.com
✅ SSH连接成功

📦 准备同步：
- 源目录: ~/.claude/skills/
- 目标仓库: git@github.com:yingguangchi/myskills.git
- Skills数量: 4

📤 推送到GitHub...
To github.com:yingguangchi/myskills.git
 * [new branch]      main -> main

✅ 首次同步完成！
📍 仓库地址: https://github.com/yingguangchi/myskills
```

### 场景2：日常同步

```
你：同步skills到GitHub

我：🚀 开始同步...

✅ SSH连接成功
✅ 本地仓库存在
📥 拉取远程更新...
Already up to date.

📦 检查变更...

📝 发现变更：
- 修改: miniprogram-workflow/SKILL.md
- 新增: auto-sync-skills-to-github/SKILL.md

💾 提交变更...
[main 8a3b2c1] Auto-sync skills at 2026-03-20 10:30:00
 2 files changed, 150 insertions(+)

📤 推送到GitHub...
To github.com:yingguangchi/myskills.git
   5d6e7f8..8a3b2c1  main -> main

✅ 同步完成！
📍 查看仓库: https://github.com/yingguangchi/myskills
```

### 场景3：无变更跳过

```
你：同步skills到GitHub

我：🚀 开始同步...

✅ SSH连接成功
✅ 本地仓库存在
📥 拉取远程更新...
Already up to date.

📦 检查变更...
✅ 没有变更，跳过同步

💡 提示：skills已是最新版本
```

## 注意事项

1. **SSH安全**：
   - ✅ SSH key无需token，更安全
   - ✅ 私钥永远不要分享
   - ✅ 使用ed25519算法（推荐）

2. **同步时机**：
   - ⏰ 每次Claude Code启动时自动同步
   - 🔄 避免频繁同步（每次对话结束不需要同步）
   - 📅 手动触发：说"同步skills到GitHub"

3. **冲突处理**：
   - ⚠️ 如果远程有变更，先pull再push
   - 🔄 使用 --rebase 保留清晰的提交历史
   - 💾 重要操作前先备份

4. **性能优化**：
   - 🚀 仅在有变更时才push
   - 📦 使用 rsync 增量同步
   - 🗑️ 定期清理旧备份

## 相关文件

- `~/.claude/skills/` - 本地skills目录（同时是git仓库）
- `~/.ssh/id_ed25519` - SSH私钥
- `~/.claude/settings.json` - Claude Code配置（hooks）

## 依赖关系

此skill是独立的，不依赖其他skills。

但可以与以下skills配合使用：
- `skills-manager` - 管理skills的安装、卸载、更新
- `backup-skills` - 备份skills到本地或其他云服务

---

**文档版本：** v1.0
**创建日期：** 2026-03-20
**作者：** Auto-sync tool for Claude Code skills
