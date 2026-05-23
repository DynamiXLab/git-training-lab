# 任务二：本地仓库基础

本章介绍 Git 最核心的一套操作流程：
**初始化仓库 → 添加文件 → 暂存 → 提交**

**本章目标：** 学会在本地创建 Git 仓库，并完成第一次提交。

---

## 一、Git 的三个核心区域

在操作之前，必须先理解 Git 的数据流向。

```text
工作区（Working Directory）          →  你写代码的地方
    ↓ git add
暂存区（Staging Area / Index）       →  临时存放"准备提交"的文件
    ↓ git commit
版本库（Repository / .git）          →  已提交的历史版本
```

可以这样记：

> **工作区是你桌面上的草稿，暂存区是打包盒，版本库是仓库。**  
> 写完代码（工作区）→ 把要提交的文件装进盒子（`git add`）→ 把盒子存进仓库（`git commit`）。

---

## 二、初始化仓库（init）

选择一个文件夹，执行以下命令将其变成 Git 仓库：

```bash
git init
```

执行后，该目录下会生成一个隐藏文件夹：

```
.git/
```

这个文件夹存储 Git 的所有版本信息，**不要手动修改或删除它**。

### 实操

本训练建议**将所有练习仓库统一存放在一个目录下**，方便管理和日后清理。

打开命令行 / 终端，建立统一工作目录（**仅首次需要，后面直接进入即可**）：

```bash
# Windows 系统下建议在磁盘D下新建一个新的文件夹用作存放训练项目
D: 
mkdir git-training
cd git-training

# Linux
cd ~
mkdir git-training
cd git-training
```

然后创建本次练习子目录并初始化：

```bash
mkdir task02
cd task02
git init
```

看到输出 `Initialized empty Git repository` 就成功了。

> 💡 把练习文件统一放在 `git-training/` 下，桌面不会乱，学完后一键删除整个文件夹即可。

---

## 三、查看仓库状态（status）

```bash
git status
```

这个命令会告诉你当前仓库的状态，是使用频率最高的命令之一。

常见状态：

| 状态 | 含义 |
|------|------|
| **Untracked** | 新文件，未被 Git 管理 |
| **Modified** | 文件被修改过，但还没加入暂存区 |
| **Staged** | 已加入暂存区，等待提交 |
| **nothing to commit** | 工作区干净，没有未提交的改动 |

> 💡 **随时可以 `git status`**。它只是查看，不会对文件造成任何影响。

---

## 四、添加文件到暂存区（add）

```bash
# 添加单个文件
git add 文件名

# 添加当前目录下的所有改动
git add .
```

### 实操

在仓库目录下创建一个新文件：

```bash
# Windows
echo hello git > README.md

# Linux
echo "hello git" > README.md
```

查看状态：

```bash
git status
```

应该看到 `README.md` 显示为 **Untracked**（红色的未跟踪文件）。

然后将其加入暂存区：

```bash
git add README.md
```

再次查看状态：

```bash
git status
```

此时 `README.md` 变成 **绿色**，表示已加入暂存区。

---

## 五、提交到版本库（commit）

```bash
git commit -m "feat: 添加 README 文件"
```

- `-m` 后面跟的是**提交信息**，用来描述这次提交做了什么
- 提交信息必须简洁、有意义

### 实操

```bash
git commit -m "feat: 添加 README 文件"
```

看到类似输出表示成功：

```
[main (root-commit) a3f1c2b] feat: 添加 README 文件
 1 file changed, 1 insertion(+)
```

---

## 六、完整流程演示

```bash
# 1. 初始化仓库
git init

# 2. 创建文件
echo hello git > test.txt

# 3. 查看状态（Untracked）
git status

# 4. 加入暂存区
git add test.txt

# 5. 查看状态（Staged）
git status

# 6. 提交
git commit -m "feat: 添加 test.txt"

# 7. 再次查看状态（干净）
git status
```

> 💡 养成习惯：每次 `git commit` 前先 `git status`，确保只提交了你想要的内容。

---

## 七、章节练习

### 练习目标

亲手完成一次完整的 Git 提交流程，并练习修改后重新提交。

### 操作步骤

**第一步：创建练习目录并初始化**

```bash
# 进入统一工作目录（如未创建，先回到上一节的步骤建立 git-training/）
# Windows 进入 D 盘 git-training 目录
D:
cd \git-training

# Linux
cd ~/git-training

# 创建本次练习子目录并初始化
mkdir task02-practice
cd task02-practice
git init
```

**第二步：新建文件并提交**

```bash
# 创建 README.md
echo "# Git 学习笔记" > README.md

# 加入暂存区
git add README.md

# 提交
git commit -m "feat: 创建 README"
```

**第三步：查看提交历史**

```bash
git log
git log --oneline
```

**第四步：修改文件并再次提交**

```bash
# 追加内容
echo "今天是第一次使用 Git" >> README.md

# 查看状态（文件被修改了）
git status

# 暂存并提交
git add README.md
git commit -m "feat: 补充学习记录"
```

**第五步：查看状态确认干净**

```bash
git status
```

应输出 `nothing to commit, working tree clean`。

### 小提示

练习过程中，每执行完一个命令都可以跑一次 `git status`，观察文件状态的变化。这不只是在帮你理解，也是在培养"提交前确认"的好习惯。

---

## 八、在 VS Code 中查看文件状态

除了命令行，VS Code 也提供了可视化的 Git 管理界面，可以直观地看到文件状态。

### 打开源代码管理面板

点击 VS Code 左侧活动栏的 **源代码管理** 图标（三个圆圈相连的那个），或按快捷键 `` Ctrl + Shift + G ``。

你会看到几个区域：

```
源代码管理
━━━━━━━━━━━━━━━━━━━━━━━
○ 更改（Changes）             ← 已修改但未暂存（相当于 git status 的 Modified）
   README.md

○ 暂存的更改（Staged Changes） ← 已 add 但未 commit（相当于 Staged）
   README.md
```

### 在 VS Code 中完成提交流程

1. **修改文件后** — 文件会出现在"更改"区域，旁边有 `M` 标记（Modified）
2. **暂存文件** — 点击文件右边的 `+` 号，文件会移到"暂存的更改"区域
3. **提交** — 在上方的输入框写提交信息，点击 ✓ 按钮

### 查看差异

点击"更改"区域中的文件名，VS Code 会打开差异对比视图：
- 绿色 = 新增的行
- 红色 = 删除的行
- 跟 `git diff` 看到的内容完全一样

> 💡 命令行和 VS Code 可以混着用。先通过 VS Code 直观理解文件状态变化，熟练后再切回命令行。

---

### ✅ 验收标准

- [ ] 能创建并初始化一个 Git 仓库
- [ ] 能在仓库中新建文件并提交
- [ ] 能用 `git status` 查看文件状态（Untracked / Staged / clean）
- [ ] 能修改文件后再次提交
- [ ] 提交信息符合规范（`feat: xxx` 格式）
