# 任务四：分支与合并

分支（Branch）是 Git 最核心的功能之一。团队协作中几乎每天都在跟分支打交道。

**本章目标：** 掌握分支的创建、切换、合并操作，学会解决合并冲突。

---

## 一、什么是分支

### 生活中的类比

想象你在写一篇论文：

- `main` 分支 = 已经提交给老师的**正式版本**
- 新分支 = 你的**草稿本**，在里面随便写、随便改，不会影响正式版本
- 等草稿写好了，再把内容**合并**到正式版本中

### 为什么需要分支？

| 场景 | 不用分支 | 用分支 |
|------|----------|--------|
| 开发新功能 | 只能在 main 上改，改到一半不能提交 | 开个分支慢慢改，互不干扰 |
| 修复 Bug | 必须等新功能做完才能修 | 开个 fix 分支立刻修，修完就合并 |
| 多人协作 | 你改我改，乱成一团 | 各开各的分支，互不影响 |

> 💡 **记住一句话：** 分支是 Git 团队协作的基石，不会用分支等于不会用 Git。

---

## 二、查看分支（branch）

```bash
git branch
```

显示当前仓库的所有分支。`*` 标记的是**当前所在分支**。

示例输出：

```
  feature/task03
* main
```

---

## 三、创建分支

```bash
git branch feature/hello
```

创建一个名为 `feature/hello` 的分支。

> ⚠️ 注意：创建分支后，你**还停留在原来的分支上**。需要手动切换过去。

---

## 四、切换分支（checkout）

```bash
git checkout feature/hello
```

切换到 `feature/hello` 分支。

### 验证是否切换成功

```bash
git branch
```

输出中 `*` 应该已经从 `main` 移到了 `feature/hello`：

```
* feature/hello
  main
```

---

## 五、创建并切换（一步到位）

实际开发中，创建分支后几乎都会立刻切换过去。Git 提供了快捷方式：

```bash
git checkout -b feature/hello
```

这条命令 = `git branch feature/hello` + `git checkout feature/hello`

> 这是最常用的分支命令，请一定记住它。

---

## 六、在不同分支上工作

切换分支后，**工作区的文件会跟着变化**。我们来做个实验。

### 实验步骤

**第一步：在 main 分支上创建文件并提交**

```bash
git checkout main
echo main 分支的内容 > branch-test.txt
git add branch-test.txt
git commit -m "feat: 在 main 上创建 branch-test.txt"
```

**第二步：切换到 feature 分支**

```bash
git checkout feature/hello
```

此时打开文件夹，你会发现 `branch-test.txt` **消失了**——因为 `feature/hello` 分支创建时还没有这个文件。

**第三步：切回 main**

```bash
git checkout main
```

`branch-test.txt` 又回来了。

> 💡 **核心理解：** 切换分支时，Git 会把工作区的文件替换成该分支对应的版本。这就是"分支隔离"的原理——不同分支的文件互不影响。

---

## 七、合并分支（merge）

当你在分支上的工作完成了，需要把改动合并回主分支。

### 合并流程

```bash
# 第一步：切到目标分支（接收改动的一方）
git checkout main

# 第二步：执行合并
git merge feature/hello
```

### 示例

假设 `feature/hello` 分支上新增了一个 `hello.txt` 文件：

```bash
git checkout main
git merge feature/hello
```

合并后，`main` 分支也会包含 `hello.txt` 的内容。

---

## 八、删除分支

合并完成后，功能分支就可以删除了：

```bash
git branch -d feature/hello
```

- `-d`：安全删除（Git 会检查该分支是否已被合并）
- `-D`：强制删除（不管是否已合并，**慎用**）

---

## 九、合并冲突（Conflict）

### 冲突是怎么产生的？

当两个分支**修改了同一个文件的同一部分**时，Git 不知道该保留哪个版本，就会报告冲突。

### 冲突长什么样？

用 VS Code 打开冲突文件，会看到：

```
<<<<<<< HEAD
这是当前分支的内容
=======
这是被合并分支的内容
>>>>>>> feature/xxx
```

各部分含义：

| 标记 | 含义 |
|------|------|
| `<<<<<<< HEAD` | 当前分支的内容开始 |
| `=======` | 分隔线 |
| `>>>>>>> feature/xxx` | 被合并分支的内容结束 |

### 解决冲突三步走

```text
第1步：手动编辑文件 → 决定保留哪些内容，删掉哪些内容
第2步：删除冲突标记（<<<<<<<、=======、>>>>>>>）
第3步：git add → git commit 完成合并
```

---

## 十、章节练习

本练习将模拟一个场景：你分别在两个分支上修改了同一个文件，产生冲突并解决。

> 本练习可以在你之前克隆的 `training-repo` 中完成，也可以新建一个目录用 `git init` 创建新仓库来做。

### 操作步骤

**第一步：在 main 分支上创建文件**

```bash
git checkout main
echo 第一行：项目初始化 > project.txt
echo 第二行：开发中 >> project.txt
echo 第三行：准备发布 >> project.txt
git add project.txt
git commit -m "feat: 创建 project.txt"
```

**第二步：创建功能分支并修改**

```bash
git checkout -b feature/update
```

修改 `project.txt` 的第二行：

```bash
echo 第一行：项目初始化 > project.txt
echo 第二行：开发中 - 新增登录功能 >> project.txt
echo 第三行：准备发布 >> project.txt
git add project.txt
git commit -m "feat: feature 分支修改第二行"
```

**第三步：切回 main，做不同的修改**

```bash
git checkout main
```

修改 `project.txt` 的同一行，改成不同的内容：

```bash
echo 第一行：项目初始化 > project.txt
echo 第二行：开发中 - 优化首页加载速度 >> project.txt
echo 第三行：准备发布 >> project.txt
git add project.txt
git commit -m "feat: main 分支修改第二行"
```

**第四步：合并，触发冲突**

```bash
git merge feature/update
```

此时会看到：

```
Auto-merging project.txt
CONFLICT (content): Merge conflict in project.txt
Automatic merge failed; fix conflicts and then commit the result.
```

**第五步：查看冲突**

用 VS Code 打开 `project.txt`，会看到类似这样的内容：

```
<<<<<<< HEAD
第二行：开发中 - 优化首页加载速度
=======
第二行：开发中 - 新增登录功能
>>>>>>> feature/update
```

**第六步：解决冲突**

手动编辑 `project.txt`，例如两个功能都保留：

```
第一行：项目初始化
第二行：开发中 - 优化首页加载速度、新增登录功能
第三行：准备发布
```

> 记得删除 `<<<<<<<`、`=======`、`>>>>>>>` 这些冲突标记。

**第七步：提交合并结果**

```bash
git add project.txt
git commit -m "feat: 解决 project.txt 冲突，合并两个分支的修改"
```

---

## 十一、在 VS Code 中操作

### 查看和切换分支

VS Code 底部状态栏**左下角**会显示当前分支名：

- 点击分支名 → 弹出分支列表 → 选择即可切换分支
- 选择 **创建新分支** → 输入分支名 → 自动创建并切换

### 解决合并冲突

当发生冲突时，VS Code 会在文件中高亮标记，并在编辑器上方提供操作按钮：

```
○ 采用当前更改（Accept Current）     ← 保留 HEAD 版本
○ 采用传入更改（Accept Incoming）    ← 保留合并过来的版本
○ 采用两个更改（Accept Both）        ← 两个都保留
○ 比较变更（Compare Changes）        ← 对比差异
```

点击对应按钮即可快速解决，不需要手动删除冲突标记。

> 💡 对于简单冲突，VS Code 的按钮比手动编辑更快。复杂冲突还是建议手动编辑。

---

### ✅ 验收标准

- [ ] 理解分支的概念和作用
- [ ] 能使用 `git branch` 查看分支列表
- [ ] 能使用 `git checkout -b` 创建并切换分支
- [ ] 能在不同分支间切换，理解文件会随之变化
- [ ] 能使用 `git merge` 合并分支
- [ ] 遇到冲突能手动解决
- [ ] 冲突解决后文件中**没有残留的冲突标记**
