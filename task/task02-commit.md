# 任务二：仓库初始化与基本提交流程

本章将介绍 Git 最核心的一套操作流程：
**初始化仓库 → 添加文件 → 暂存 → 提交**

---

## 一、初始化仓库（init）

在一个普通文件夹中，使用以下命令即可将其变为 Git 仓库：

```bash
git init
```

执行后会生成一个隐藏目录：

```
.git
```

这个目录用于存储 Git 的所有版本信息（不要手动修改）。

### 示例

```bash
git init
```

初始化成功后，该目录就已经被 Git 接管了。

---

## 二、工作区与版本区概念

在正式操作前，必须理解 Git 的三个核心区域：

```
工作区（Working Directory） -> 实际编辑的文件
        ↓
暂存区（Staging Area） -> 临时存放“准备提交”的文件
        ↓
版本库（Repository） -> 已经提交的历史版本
```

---

## 三、文件状态查看

使用以下命令查看当前状态：

```bash
git status
```

常见状态：

* Untracked：未被 Git 管理
* Modified：文件被修改但未暂存
* Staged：已加入暂存区

---

## 四、添加文件到暂存区（add）

### 1. 添加单个文件

```bash
git add main.c
```

### 2. 添加所有文件

```bash
git add .
```

作用：把工作区的修改加入暂存区

---

## 五、提交到版本库（commit）

```bash
git commit -m "第一次提交"
```

作用：将暂存区内容正式保存为一个版本

---

## 六、完整流程示例

```bash
# 初始化仓库
git init

# 创建文件
echo "hello git" > test.txt

# 查看状态
git status

# 添加到暂存区
git add test.txt

# 再次查看状态
git status

# 提交
git commit -m "添加 test.txt 文件"
```

## 七、章节练习

要求：
1. 新建文件夹 task02
2. 在该目录初始化 Git 仓库
3. 新建 README.md 文件
4. 编辑文件内容，并提交到 Git
5. 修改 README.md 内容
6. 在 VSCode 的源代码管理中观察文件变化
