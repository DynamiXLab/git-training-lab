# 任务七：期末测试 — 提交训练答卷

恭喜你完成了 Git 入门训练的全部课程。本章是一次综合测验，检验你对 Git 的理解和实际操作能力。

**测验目标：** 独立完成一套完整的 Git 操作流程，并将成果提交到仓库，留下一份属于你的训练记录。

---

## 测验说明

- 所有操作在 `git-training-lab` 仓库中完成
- 严格按照步骤执行，**先思考再敲命令**
- 最终提交到你的 `submit/<年份>-<姓名>` 分支
- 你的分支将**永久保留**在仓库中，成为你加入实验室的一份印记

---

## 第一题：准备提交分支（5 分）

如果你之前已经创建过提交分支，先切换到它：

```bash
git checkout submit/2022-linhaixin
```

如果还没有创建：

```bash
git checkout -b submit/2022-linhaixin
```

> 请将 `2022-linhaixin` 替换为你的实际年份和姓名拼音。

**检查点：** 执行 `git branch`，确认当前分支为 `submit/2022-linhaixin`。

---

## 第二题：创建个人档案（25 分）

在 `submissions/你的名字/` 目录下创建一个 `README.md` 文件，写入你的个人档案。

### 操作要求

分 **三次提交** 完成，每次提交一部分内容。

**第一次提交：姓名和入学年份**

```bash
echo "# Git 训练档案" > submissions\你的名字\README.md
echo >> submissions\你的名字\README.md
echo - 姓名：林海鑫 >> submissions\你的名字\README.md
echo - 入学年份：2022 >> submissions\你的名字\README.md
git add submissions\你的名字\README.md
git commit -m "feat: 添加姓名和入学年份"
```

**第二次提交：专业和完成日期**

```bash
echo - 专业：计算机科学与技术 >> submissions\你的名字\README.md
echo - 完成日期：2025-03-20 >> submissions\你的名字\README.md
git add submissions\你的名字\README.md
git commit -m "feat: 添加专业和完成日期"
```

**第三次提交：想说的话**

```bash
echo >> submissions\你的名字\README.md
echo ## 训练感想 >> submissions\你的名字\README.md
echo >> submissions\你的名字\README.md
echo 通过本次训练，我掌握了 Git 的基本操作， >> submissions\你的名字\README.md
echo 能够参与团队协作开发了。 >> submissions\你的名字\README.md
git add submissions\你的名字\README.md
git commit -m "feat: 添加训练感想"
```

> 💡 你也可以用 VS Code 打开文件编辑，而不是用 `echo` 命令。

**检查点：** 执行 `git log --oneline`，应该能看到最近 3 条你的提交记录。

---

## 第三题：查看提交历史（10 分）

```bash
# 简洁模式查看所有提交
git log --oneline

# 只看你的提交（替换为你的名字）
git log --oneline --author="你的名字"
```

确认你至少有 **3 次提交**，且提交信息以 `feat:` 开头。

**检查点：** 将 `git log --oneline` 的输出截图保存（可选）。

---

## 第四题：创建 .gitignore（15 分）

在仓库根目录创建一个 `.gitignore` 文件，忽略以下内容：

- 所有 `.log` 文件
- `temp/` 目录
- 系统文件 `.DS_Store`

```bash
echo *.log > .gitignore
echo temp/ >> .gitignore
echo .DS_Store >> .gitignore
git add .gitignore
git commit -m "feat: 添加 .gitignore"
```

**检查点：** 执行 `git status`，仓库应该是干净的（没有未跟踪的文件）。

---

## 第五题：创建临时分支并合并（25 分）

本题模拟团队协作中的分支合并场景。

### 操作步骤

**第一步：切换到 main，创建一个临时分支**

```bash
git checkout main
git checkout -b practice/你的姓名拼音
```

**第二步：修改你的档案文件，追加一行**

```bash
echo >> submissions\你的名字\README.md
echo 测试：模拟分支合并 >> submissions\你的名字\README.md
git add submissions\你的名字\README.md
git commit -m "feat: 模拟合并测试"
```

**第三步：切回你的提交分支，合并临时分支**

```bash
git checkout submit/2022-linhaixin
git merge practice/你的姓名拼音
```

**第四步：删除临时分支**

```bash
git branch -d practice/你的姓名拼音
```

**检查点：** 用 VS Code 打开 `submissions/你的名字/README.md`，确认"模拟分支合并"这一行已出现在文件中。

---

## 第六题：提交答卷（20 分）

将你的提交分支推送到远程仓库。

```bash
git push origin submit/2022-linhaixin
```

推送完成后，去 GitHub 上发起 Pull Request：

1. 打开 👉 https://github.com/DynamiXLab/git-training-lab
2. 点击 **Compare & pull request**
3. 确认 **base: main** ← **compare: submit/2022-linhaixin**
4. 标题填写：`feat: 提交 2022-linhaixin 训练答卷`
5. 说明框里列一下你完成了哪些任务
6. 点击 **Create Pull Request**

---

## 第七题：额外挑战（选做，不计分）

如果学有余力，尝试以下操作：

```bash
# 给你的最后一次提交打个标签
git tag my-first-submit

# 查看提交历史图形模式
git log --oneline --graph --all
```

---

## 评分标准

| 题号 | 考核内容 | 分值 | 自评 |
|------|---------|:---:|:----:|
| 一 | 分支名称符合规范 | 5 | |
| 二 | 分 3 次提交，每次提交信息规范 | 25 | |
| 三 | 能正确查看提交历史 | 10 | |
| 四 | 正确创建 .gitignore | 15 | |
| 五 | 能创建分支、合并分支、删除分支 | 25 | |
| 六 | 成功推送并创建 PR | 20 | |
| | **总分** | **100** | |

---

## 在 VS Code 中完成测验

本次测验的所有操作都可以在 VS Code 中完成：

- **源代码管理面板**（`Ctrl + Shift + G`）：查看文件状态、暂存、提交、推送
- **底部状态栏左下角**：查看和切换分支
- **终端**（`` Ctrl + ` ``）：在 VS Code 内直接运行 Git 命令
- **Git Graph 扩展**：查看提交历史和分支图

具体操作方式在前面五个任务的"在 VS Code 中操作"章节中都有详细介绍，这里不再重复。

> 💡 你可以全程使用命令行完成，也可以用 VS Code 辅助，怎么顺手怎么来。

---

### ✅ 验收标准

- [ ] 提交分支为 `submit/<年份>-<姓名拼音>` 格式
- [ ] `submissions/你的名字/README.md` 存在且内容完整
- [ ] `git log --oneline` 显示至少 3 次你的提交
- [ ] `.gitignore` 已创建并提交
- [ ] 成功完成分支合并
- [ ] 成功 push 到远程仓库
- [ ] 在 GitHub 上发起了 Pull Request
- [ ] PR 标题格式正确
