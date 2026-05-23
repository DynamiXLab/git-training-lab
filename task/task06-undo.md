# 任务六：撤销与版本管理

人总会犯错：commit 错了、push 错了、想回到上一个版本……Git 提供了多种撤销方式。

**本章目标：** 学会撤销本地提交、安全撤销远程提交、只拉取不合并、打版本标签。

---

## 一、本地撤销（reset）

`git reset` 用来**撤销本地的 commit**。根据参数不同，对代码的影响也不同。

### 三种模式

| 模式 | 命令 | commit 撤销？ | 代码去哪了？ | 场景 |
|------|------|:-----------:|:----------:|------|
| `--soft` | `git reset --soft HEAD^` | ✅ | 暂存区（绿色） | 只想改 commit 信息 |
| `--mixed`（默认） | `git reset HEAD^` | ✅ | 工作区（未暂存） | 多 commit 了文件，重新选 |
| `--hard` | `git reset --hard HEAD^` | ✅ | ❌ **没了** | 本地改坏了，全部丢掉 |

### HEAD^ 的含义

```
HEAD      当前最新的 commit
HEAD^     上一个 commit（等价于 HEAD~1）
HEAD^^    上上个 commit
HEAD~3    往前 3 个 commit
```

### 场景一：commit 信息写错了

```bash
git commit -m "错别自"
git reset --soft HEAD^    # 撤销 commit，代码回到暂存区
git commit -m "错别字"    # 重新提交
```

### 场景二：多 commit 了不该提交的文件

```bash
git reset --mixed HEAD^   # 撤销 commit，代码回到工作区
# 重新选择要提交的文件
git add 需要的文件
git commit -m "正确的提交"
```

### 场景三：本地改坏了，想全部丢掉（⚠️ 不可恢复）

```bash
git reset --hard HEAD^
```

> ⚠️ `--hard` 会**彻底删除代码**，无法找回。用之前务必确认。

---

## 二、远程撤销（revert）

`git reset` 只能用于**还没 push 的 commit**。如果已经 push 到远程，再用 reset 会导致远程历史不一致，影响其他人。

这时要用 `git revert`。

### revert 的原理

revert 不是删除旧的提交，而是**生成一个反向的新提交**来抵消它。

```text
原来历史：  A → B → C
                   ↑ 想撤销 B

revert 后： A → B → C → D
                          ↑ D 自动生成，内容是"撤销 B 的改动"
```

整个历史往后延伸，没有重写，对团队成员透明。

### 实操

```bash
# 查看历史，找到想撤销的 commit id
git log --oneline

# 撤销该 commit（自动生成一个新 commit）
git revert <commit-id> --no-edit

# 推送到远程
git push origin main
```

### reset vs revert 怎么选？

| 场景 | 用哪个 |
|------|--------|
| 刚 commit，还没 push，想改提交信息 | `reset --soft` |
| commit 多了文件，还没 push | `reset --mixed` |
| 本地改坏了想全部丢弃（还没 push） | `reset --hard` |
| **已经 push 到远程了**，要撤销 | **`revert`** |

> 💡 **口诀：** 还没 push → reset；已经 push → revert。

---

## 三、只拉取不合并（fetch）

任务三里学了 `git pull`，它的本质是：

```text
git pull = git fetch + git merge
```

`git fetch` 只做前半段——**从远程下载最新数据到本地**，但不自动合并。

### 为什么需要 fetch？

有时候你只想先看看远程有什么改动，确认没问题再合并。

```bash
# 从远程下载最新数据
git fetch upstream

# 查看远程分支和本地分支的差异（谁领先谁）
git log --oneline main..upstream/main

# 确认没问题后再合并
git merge upstream/main
```

### 常用命令

```bash
# 从 origin 拉取所有分支的更新
git fetch

# 从 upstream 拉取
git fetch upstream

# 拉取后看看别人提交了什么
git log --oneline main..origin/main
```

> 💡 快速同步用 `git pull`；想**先看看再决定**用 `git fetch`。这是老手和菜鸟的区别之一。

---

## 四、版本标签（tag）

`git tag` 用来给某个 commit 打上有意义的标签，通常是版本号。

### 为什么需要 tag？

```
只看 commit 历史（看不出哪个是版本节点）：
a3f1c2b  修复登录 Bug
b8d3e1a  新增注册功能
9c7f2d0  项目初始化

打上 tag 后（一目了然）：
v1.0.0  →  a3f1c2b  修复登录 Bug
v0.9.0  →  b8d3e1a  新增注册功能
          9c7f2d0  项目初始化
```

### 常用命令

```bash
# 给当前 commit 打标签
git tag v1.0

# 给指定的某个 commit 打标签
git tag v0.9 <commit-id>

# 查看所有标签
git tag

# 查看某个标签的详细信息
git show v1.0

# 推送标签到远程
git push origin v1.0

# 推送所有标签
git push origin --tags
```

> 💡 标签和分支不同：标签是**只读的**，打上就不能改。它用来标记里程碑版本（如 v1.0、v2.0），发版用。

---

## 五、章节练习

### 练习内容

新建一个练习仓库，依次练习 reset / revert / tag。

> ⚠️ 注意：`reset --hard` 会删除代码，**务必在专门的新仓库中练习**，不要在训练仓库或其他重要仓库中操作。

### 操作步骤

**第一步：创建练习仓库**

```bash
# 进入统一工作目录（如未创建，参考任务二先建立 git-training/）
# Windows
cd %USERPROFILE%\git-training

# Linux
cd ~/git-training

# 创建本次练习子目录并初始化
mkdir task06-practice
cd task06-practice
git init
```

**第二步：制造多次提交**

```bash
echo 第一版 > app.txt
git add app.txt
git commit -m "feat: 第一版"

echo 第二版 >> app.txt
git add app.txt
git commit -m "feat: 第二版"

echo 第三版 >> app.txt
git add app.txt
git commit -m "feat: 第三版"
```

**第三步：练习 reset --soft**

```bash
# 查看当前历史
git log --oneline

# 撤销最新 commit，代码保留在暂存区
git reset --soft HEAD^

# 观察：app.txt 是绿色的（已暂存状态）
git status

# 重新提交
git commit -m "feat: 重新提交第三版"
```

现在又有 3 条 commit 了。

**第四步：练习 reset --hard**

```bash
git log --oneline

# 回退到第一个 commit（丢掉后面两个）
git reset --hard HEAD^^

# 只剩第一次提交了
git log --oneline

# 文件内容也只有第一版
type app.txt
```

> 到了这里，第二版和第三版的代码已经彻底消失了。这就是 `--hard` 的威力。

**第五步：重新提交，练习 tag**

创建几个新 commit 来打标签：

```bash
echo "v1.0 版本内容" > version.txt
git add version.txt
git commit -m "feat: v1.0 版本"

echo "v1.1 版本内容" >> version.txt
git add version.txt
git commit -m "feat: v1.1 版本"

echo "v1.2 版本内容" >> version.txt
git add version.txt
git commit -m "feat: v1.2 版本"
```

**第六步：打标签**

```bash
# 给当前 commit 打标签
git tag v1.2

# 给上一个 commit 打标签
git tag v1.1 HEAD^

# 查看所有标签
git tag

# 查看标签详情
git show v1.2
```

**第七步：练习 revert**

```bash
# 撤销最新提交（生成一个新提交来抵消它）
git revert HEAD --no-edit

# 查看历史：多了一条 "Revert ..." 的提交
git log --oneline
```

这就完成了 revert 的模拟练习——虽然这里没有真正的远程仓库，但原理是一样的。

---

## 六、在 VS Code 中操作

### 撤销改动

在 **源代码管理** 面板中：

- **放弃单个文件的修改**：点击已修改文件右侧的 `-` 号（放弃更改），等同于 `git restore 文件名`
- **撤销暂存**：点击已暂存文件右侧的 `-` 号（取消暂存），等同于 `git reset HEAD 文件名`
- **撤销最近一次提交**：点击 `...` 菜单 → **撤销上次提交**，等同于 `git reset --soft HEAD^`

### 查看标签

VS Code 的 **Git Graph** 扩展（在任务五中介绍过）可以直接显示标签。安装后在图形化历史中，标签会以菱形标记显示在每个 commit 旁边。

> 💡 reset --hard 等危险操作建议用命令行，因为命令行不会让你误点。VS Code 的撤销功能更适用于安全场景（放弃单个文件、撤销暂存）。

---

### ✅ 验收标准

- [ ] 理解 reset 三种模式（--soft / --mixed / --hard）的区别
- [ ] 知道 `reset --hard` 会彻底删除代码，使用前必须确认
- [ ] 知道已经 push 的 commit 要用 `revert` 而不是 `reset`
- [ ] 理解 `fetch` 和 `pull` 的区别（fetch 只下载不合并）
- [ ] 能创建标签、查看标签
- [ ] 完成 revert 练习，看到了自动生成的撤销提交
