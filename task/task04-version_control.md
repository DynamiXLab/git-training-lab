# 第三章：版本管理
本章介绍 Git 中最重要的能力之一：**版本管理**。  
通过 Git，你可以记录每一次修改、回退历史版本、对比差异，从而安全地进行开发。

---

## 一、查看版本状态（status）

在任何 Git 仓库中，都可以使用：

```bash
git status
```

### 作用：
- 查看当前文件状态
- 判断哪些文件被修改
- 判断哪些文件已进入暂存区

### 常见状态：
- `modified`：文件被修改但未暂存
- `staged`：已暂存，等待提交
- `untracked`：未被 Git 管理的新文件

---

## 二、查看修改内容（diff）

```bash
git diff
```

### 作用：
查看**修改前后差异**

### 常用方式：

```bash
git diff          # 工作区 vs 暂存区
git diff --staged # 暂存区 vs 最近一次提交
```

👉 用于确认“改了什么”

---

## 三、提交版本（commit）

```bash
git commit -m "提交说明"
```

### 作用：
- 将暂存区内容生成一个“版本快照”
- 每一次 commit 都代表一个历史节点

### 示例：

```bash
git commit -m "完成初始化功能"
```

---

## 四、查看提交历史（log）

```bash
git log
```

### 作用：
查看所有提交记录

### 输出包含：
- 提交ID（hash）
- 作者
- 时间
- 提交说明

### 简洁模式：

```bash
git log --oneline
```

---

## 五、版本回退（reset）

当代码出现问题，可以回到历史版本。

### 1. 回到指定版本（常用）

```bash
git reset --hard <commit_id>
```

例如：

```bash
git reset --hard a3f1c2b
```

---

### 2. 回到上一个版本

```bash
git reset --hard HEAD^
```

- `HEAD`：当前版本
- `HEAD^`：上一个版本
- `HEAD^^`：上上个版本

---

## 六、撤销修改（restore）

如果只是修改了文件，还没提交：

```bash
git restore 文件名
```

作用：
- 放弃当前修改
- 恢复到最近一次 commit 状态

---

## 七、版本管理核心流程总结

```text
修改代码
   ↓
git status（查看状态）
   ↓
git diff（确认修改）
   ↓
git add（加入暂存区）
   ↓
git commit（生成版本）
   ↓
git log（查看历史）
```

---

## 八、本章核心理解

版本管理的本质是：

> 用 commit 构建一个可回溯的时间线

你可以：
- 随时查看历史
- 随时回退
- 随时对比差异
- 不怕改坏代码
