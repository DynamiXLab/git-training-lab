# 任务五：查看历史与对比

日常开发中，你经常需要查看提交历史、对比代码差异、暂时搁置手头工作。
这些操作不会改变代码，但它们是你每天都会用到的技能。

**本章目标：** 学会查看提交历史、对比文件差异、暂存工作进度、忽略不需要的文件。

---

## 一、查看提交历史（log）

`git log` 用来查看仓库的提交记录。

### 基本用法

```bash
git log
```

输出示例：

```
commit a3f1c2b8d3e1a9c7f2d0b5e6f7a8b9c0d1e2f3a4
Author: LinHaixin <lin@example.com>
Date:   Mon Mar 10 14:30:00 2025 +0800

    feat: 添加登录功能
```

包含四部分信息：
- **commit id** — 一串哈希值，唯一标识这次提交
- **作者** — 提交者的名字和邮箱
- **日期** — 提交时间
- **提交信息** — 你写的 commit message

### 常用参数

```bash
# 简洁模式（一行一条，精简显示）
git log --oneline

# 图形模式（看分支分叉关系）
git log --oneline --graph --all

# 限制条数（只看最近 3 条）
git log -3
```

> 💡 `--graph` 用 ASCII 线条展示分支关系，`--all` 显示所有分支的提交。

### 实操

```bash
# 在你之前的 training-repo 中试试
cd training-repo
git log --oneline
```

---

## 二、对比修改内容（diff）

`git diff` 用来查看文件的具体改动——**到底改了哪些行**。

### 三种常用对比

```bash
# 工作区 vs 暂存区（改了但还没 add）
git diff

# 暂存区 vs 最近一次 commit（add 了但还没 commit）
git diff --staged

# 两个 commit 之间对比
git diff <commit1> <commit2>
```

### diff 输出怎么看

```diff
- 这是被删除的行（红色）
+ 这是新增的行（绿色）
```

### 实操

```bash
# 1. 修改一个文件
echo "新增一行内容" >> README.md

# 2. 查看改动
git diff
```

> 💡 **养成习惯：** 每次 `git commit` 前先执行 `git diff --staged`，确认自己改对了再提交。这是老手的肌肉记忆。

---

## 三、暂存工作进度（stash）

`git stash` 用于把**写到一半的代码暂时存起来**，让工作区恢复干净。

### 典型场景

你正在开发功能 A，突然来了紧急 Bug：

```text
工作区：功能 A 写到一半（还不能提交）
         ↓
想切分支修 Bug，但 Git 不让（有未提交的修改）
         ↓
git stash → 工作区变干净 → 切分支 → 修 Bug
         ↓
切回来 → git stash pop → 继续写功能 A
```

### 常用命令

```bash
# 暂存当前工作区
git stash

# 查看暂存列表
git stash list

# 恢复最近一次暂存（恢复后自动删除该记录）
git stash pop

# 恢复但不删除记录（可以多次 apply）
git stash apply

# 包含未跟踪的新文件
git stash -u
```

### 实操

```bash
# 1. 修改一个已跟踪的文件
echo "写到一半的功能" >> README.md

# 2. 查看状态（工作区有修改）
git status

# 3. 暂存
git stash

# 4. 工作区变干净了
git status

# 5. 恢复工作
git stash pop
```

> 💡 `git stash` 默认只暂存已跟踪的文件。加了 `-u` 才会包含新建的未跟踪文件。

---

## 四、忽略文件（.gitignore）

项目中有一些文件**不应该**被 Git 跟踪，比如编译产物、依赖包、IDE 配置。

### 哪些文件该忽略？

| 类型 | 例子 | 原因 |
|------|------|------|
| 编译产物 | `*.exe`、`*.jar`、`dist/` | 每次编译都会重新生成 |
| 依赖目录 | `node_modules/`、`vendor/` | 数量巨大，别人用包管理器自行安装 |
| IDE 配置 | `.vscode/`、`.idea/` | 每个人用的编辑器可能不同 |
| 系统文件 | `.DS_Store`、`Thumbs.db` | 操作系统自动生成 |
| 环境配置 | `.env` | 可能包含密码 / API Key |

### 使用方式

在仓库根目录创建 `.gitignore` 文件，一行一个规则：

```bash
node_modules/
*.log
.env
.DS_Store
dist/
```

### 实操

```bash
# 创建 .gitignore
echo node_modules/ > .gitignore
echo .env >> .gitignore

# 提交
git add .gitignore
git commit -m "feat: 添加 .gitignore"
```

### 如果文件已经被跟踪了怎么办？

```bash
# 把文件从 Git 跟踪中移除（但保留在磁盘上，不删文件）
git rm --cached 文件名

# 然后添加到 .gitignore
echo 文件名 >> .gitignore
```

> 💡 **原则：** 自动生成的、从网上下的、包含隐私的文件，都不该被 Git 跟踪。项目一开始就创建 `.gitignore` 是最省事的做法。

---

## 五、章节练习

### 练习内容

新建一个仓库，依次练习 log / diff / stash / .gitignore。

### 操作步骤

**第一步：创建练习仓库**

```bash
# 进入统一工作目录（如未创建，参考任务二先建立 git-training/）
# Windows
cd %USERPROFILE%\git-training

# Linux
cd ~/git-training

# 创建本次练习子目录并初始化
mkdir task05-practice
cd task05-practice
git init
```

**第二步：创建 .gitignore**

```bash
echo node_modules/ > .gitignore
echo *.log >> .gitignore
git add .gitignore
git commit -m "feat: 添加 .gitignore"
```

**第三步：多次提交，制造历史**

```bash
echo "# 项目说明" > README.md
git add README.md
git commit -m "feat: 添加 README"

echo "v1.0" > version.txt
git add version.txt
git commit -m "feat: 添加 version.txt"

echo "v2.0" > version.txt
git add version.txt
git commit -m "feat: 更新到 v2.0"
```

**第四步：查看提交历史**

```bash
git log
git log --oneline
```

观察输出格式的不同。

**第五步：对比差异**

```bash
# 修改 README
echo "新的一行内容" >> README.md

# 查看差异
git diff
```

**第六步：练习 stash**

```bash
# 修改文件
echo "写到一半的功能" >> README.md

# 暂存
git stash

# 验证工作区干净了
git status

# 恢复
git stash pop
```

---

## 六、在 VS Code 中操作

### 查看提交历史

VS Code 本身不直接显示 git 历史，可以安装 **Git Graph** 扩展获得图形化的历史视图：

1. 点击左侧扩展图标（`Ctrl + Shift + X`）
2. 搜索 `Git Graph` 并安装
3. 安装后底部状态栏会出现 **Git Graph** 按钮，点击即可看到分支图和提交历史

也可以不使用扩展，点击左侧文件列表底部的 **时间线（Timeline）**，查看当前文件的修改历史。

### 查看差异

在 **源代码管理** 面板（`Ctrl + Shift + G`）中，点击已修改的文件名，右侧会打开差异对比视图。绿色为新增行，红色为删除行，跟 `git diff` 看到的内容一致。

### 生成 .gitignore

VS Code 扩展市场中有 `.gitignore` 生成器：

1. 搜索 `gitignore` 并安装
2. `Ctrl + Shift + P` → 输入 `Add gitignore`
3. 选择项目类型（如 Node、Python），自动生成对应的 `.gitignore`

> 💡 小而直观的操作用命令行，大批量查看用 VS Code 扩展，两者互补。

---

### ✅ 验收标准

- [ ] 能用 `git log` 和 `git log --oneline` 查看提交历史
- [ ] 能在 commit 前用 `git diff` 确认过改动
- [ ] 能用 `git stash` 暂存工作并用 `git stash pop` 恢复
- [ ] 理解 `.gitignore` 的作用，能写出基本的忽略规则
