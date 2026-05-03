# Git Training Lab

本仓库旨在为初学者提供一套系统化的 Git 使用训练指南，涵盖常用命令讲解与实践任务。Git 是进入实验室参与项目开发的基础技能之一，本仓库通过“理论说明 + 实践训练”的方式，帮助使用者建立规范的版本控制与协作意识，快速完成从入门到实际应用的过渡。


---

# 训练目标

通过本仓库的训练，成员应具备以下能力：

* 掌握 Git 常用命令的正确使用方法
* 理解并实践标准的分支开发流程（Branch Workflow）
* 熟悉 Pull Request 协作模式
* 能够独立处理代码冲突（Conflict Resolution）
* 具备规范化提交（Commit Convention）意识

# 训练方式
本仓库采用“任务驱动”的训练模式：

* 每个任务对应一个核心 Git 能力点
* 所有训练必须通过 Fork + 分支 + PR 的方式完成
* 每位成员需在指定目录提交训练结果
* 所有提交均需符合规范，否则不予通过

# 基本要求（强制）
* 禁止直接在 main 分支进行开发
* 必须使用功能分支（feature/*）完成任务
* 所有提交必须符合 Commit 规范
* 每个任务需单独发起 Pull Request
* 提交内容必须位于指定目录（submissions//）

---

**请按照 tasks 目录中的任务顺序完成训练。建议在每一步操作前理解其作用，而非机械执行命令。**

---

# 二、标准协作模型（核心）

外部协作者统一使用：

👉 **Fork + Branch + PR 模型**

流程如下：

```
组织仓库（upstream）
        ↑
      PR
        ↑
你的仓库（fork）
        ↑
    本地开发
```

---

# 三、环境准备

## 安装
```bash
git --version
```

## 配置身份（必须）
```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

---

# 四、核心命令 + 训练体系

---

## 4.1 仓库获取（clone & fork）

```bash
git clone <fork-url>
```

### 训练任务
- Fork 指定组织仓库
- Clone 到本地
- 成功运行项目（如果有）

✅ 验收标准：
- 本地存在仓库
- 可正常 `git status`

---

## 4.2 基本提交（commit）

```bash
git add .
git commit -m "feat: 添加测试文件"
```

### 提交规范（强制）

```
feat: 新功能
fix: 修复问题
docs: 文档修改
refactor: 重构
```

### 训练任务
- 修改 README
- 提交一次规范 commit

❌ 错误示例：
```
update
改了一下
123
```

---

## 4.3 分支管理（核心能力）

```bash
git checkout -b feature/xxx
```

### 规则
- 禁止直接在 main 开发
- 一功能一分支

### 训练任务
- 创建 feature/hello 分支
- 修改文件并提交

---

## 4.4 远程同步（remote）

```bash
git remote add upstream <org-repo>
git pull upstream main
```

### 训练任务
- 添加 upstream
- 同步主仓库

---

## 4.5 推送代码（push）

```bash
git push origin feature/xxx
```

### 训练任务
- 推送你的分支到 GitHub

---

## 4.6 Pull Request（协作核心）

### 标准流程
1. push 分支
2. 创建 PR
3. 填写说明
4. 等待 review
5. 修改后再次提交

---

# 五、冲突处理训练（重点）

## 场景模拟
两人同时修改 README

```bash
git pull
```

手动修改冲突：
```
<<<<<<< HEAD
你的代码
=======
别人代码
>>>>>>>
```

### 训练任务
- 人为制造冲突
- 正确解决并提交

✅ 验收：无冲突标记

---

# 六、Rebase vs Merge（进阶）

```bash
git rebase main
```

### 教学重点
- rebase = 历史整理
- merge = 历史保留

### 训练任务
- 使用 rebase 同步主分支

---

# 七、版本管理（Tag）

```bash
git tag v1.0
git push origin v1.0
```

### 训练任务
- 发布一个版本

---

# 八、标准提交与PR模板（必须执行）

## Commit 规范

```
<type>: <简短描述>
```

## PR 模板

```md
## 变更内容
- 做了什么

## 原因
- 为什么要改

## 测试
- 如何验证

## 影响范围
- 是否影响其他模块
```

---

# 九、CONTRIBUTING.md（建议放仓库）

```md
# 贡献指南

## 流程
1. Fork 仓库
2. 创建分支
3. 提交代码
4. 发起 PR

## 规范
- 禁止直接修改 main
- 提交必须符合规范
```

---

# 十、CI（自动检查建议）

建议接入 GitHub Actions：

检查内容：
- 提交信息格式
- 代码风格
- 构建是否成功

示例：
```yaml
name: CI
on: [pull_request]
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: echo "检查通过"
```

---

# 十一、完整训练项目设计（重点）

## 仓库结构建议

```
training-repo/
├── README.md
├── tasks/
│   ├── task1.md
│   ├── task2.md
├── src/
└── docs/
```

## 训练任务设计

### Task1：提交练习
- 修改文件
- 提交规范 commit

### Task2：分支练习
- 创建分支
- 提交 PR

### Task3：冲突解决
- 模拟冲突
- 正确处理

### Task4：完整流程
- fork → 开发 → PR → 合并

---

# 十二、评分标准（用于教学）

| 项目 | 分值 |
|------|------|
| 提交规范 | 20 |
| 分支使用 | 20 |
| PR质量 | 30 |
| 冲突处理 | 30 |

---

# 十三、最佳实践总结

✔ 小步提交
✔ 一功能一分支
✔ PR 必写说明
✔ 不污染主分支
✔ 先同步再开发

---

# 结语

完成本训练后，你应具备：

- 标准 Git 操作能力
- GitHub 协作能力
- 工程级规范意识

👉 这才是“会用 Git”的真正标准。

