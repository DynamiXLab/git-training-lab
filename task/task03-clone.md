# 任务三：远程仓库协作

在任务一中你执行了 `git clone`，但当时只让你先运行，没有解释含义。
本章将正式讲解**远程仓库**的概念，并带你走一遍标准的 **Fork → PR 工作流**。

**本章目标：** 理解 clone 的原理，掌握 Fork 工作流、推送代码、发起 Pull Request。

---

## 一、认识远程仓库

远程仓库就是存放在网络上的 Git 仓库，比如 GitHub、GitLab 等平台。

### 为什么需要远程仓库？

| 场景 | 只有本地仓库 | 配合远程仓库 |
|------|-------------|-------------|
| 备份 | ❌ 电脑坏了就没了 | ✅ 代码安全存放在云端 |
| 协作 | ❌ 别人看不到你的代码 | ✅ 团队成员都能访问 |
| 同步 | ❌ 只能一个人工作 | ✅ 多人同时开发 |

本训练使用的远程仓库地址是：

```text
https://github.com/DynamiXLab/git-training-lab.git
```

---

## 二、标准协作模型：Fork + Branch + PR

在实际团队开发中，你很少能直接往组织仓库里 push。标准流程是：

```text
组织仓库（DynamiXLab/git-training-lab）
        ↑
      Pull Request
        ↑
你的 Fork（你的用户名/git-training-lab）
        ↑
      git push
        ↑
    本地仓库
```

每一步的含义：

| 步骤 | 做什么 | 为什么 |
|------|--------|--------|
| **Fork** | 在 GitHub 上把组织仓库复制一份到你的账号 | 你 Fork 后的仓库归你所有，可以自由 push |
| **Clone** | 把你的 Fork 下载到本地 | 在本地写代码 |
| **Push** | 把本地提交推送到你的 Fork | 把代码同步到云端你的副本 |
| **PR** | 请求把你的改动合并到组织仓库 | 让管理员 review 你的代码，合并后正式入库 |

### 什么时候不需要 Fork？

如果**你本人有仓库的写入权限**（比如你在 GitHub 上自己建的项目，或者你是仓库的成员），流程会简单很多：

```text
git clone <仓库地址>          # 拉到本地
git checkout -b feature/xxx   # 创建分支
# 修改代码...
git add . && git commit
git push origin feature/xxx   # 直接推回去 ✅
```

不需要 Fork，也不需要加 upstream。

### 那为什么任务三还要学 Fork？

在实验室团队里，**新成员通常不会一开始就有仓库的写入权限**。仓库属于组织，你只能通过 Fork → PR 的方式提交代码，由管理员 review 后合并进去。

这也是开源社区的通用协作方式——你能给任何项目提 PR，但只有维护者才有权限直接 push。

| 场景 | 能不能直接 push？ | 用 Fork？ |
|------|:---------------:|:---------:|
| 你自己在 GitHub 上建的仓库 | ✅ 可以 | ❌ 不需要 |
| 你是仓库成员（有写入权限） | ✅ 可以 | ❌ 不需要 |
| 别人的仓库 / 组织仓库 / 开源项目 | ❌ 不行 | ✅ 必须 Fork → PR |

> 💡 **简单记：** 你能直接 push 的仓库就不用 Fork；你不能 push 的就需要 Fork → PR。本训练用的是组织仓库，所以走 Fork 流程。

---

## 三、Fork 仓库（复制仓库到你的账号）

Fork 是 GitHub 的一项功能，能把**别人的仓库复制一份到你的账号下**。

### 操作步骤

1. 打开训练仓库页面：👉 https://github.com/DynamiXLab/git-training-lab
2. 点击页面右上角的 **Fork** 按钮
3. 选择你的 GitHub 账号
4. 等待几秒，Fork 完成

> 💡 Fork 完成后，你的账号下会出现一个同名的仓库，地址是：
> `https://github.com/你的用户名/git-training-lab.git`

---

## 四、克隆你的 Fork（clone）

任务一你克隆的是**组织仓库**，现在需要改为克隆**你的 Fork**。

### 第一步：查看当前的远程仓库

```bash
cd git-training-lab
git remote -v
```

可以看到 `origin` 目前指向组织仓库。

### 第二步：修改 origin 指向你的 Fork

```bash
git remote set-url origin https://github.com/你的用户名/git-training-lab.git
```

> 请将 `你的用户名` 替换为你的 GitHub 用户名。

### 第三步：添加 upstream（上游仓库）

添加一个指向**原组织仓库**的远程地址，用来同步最新更新：

```bash
git remote add upstream https://github.com/DynamiXLab/git-training-lab.git
```

### 第四步：验证

```bash
git remote -v
```

现在应该显示：

```
origin    https://github.com/你的用户名/git-training-lab.git (fetch)
origin    https://github.com/你的用户名/git-training-lab.git (push)
upstream  https://github.com/DynamiXLab/git-training-lab.git (fetch)
upstream  https://github.com/DynamiXLab/git-training-lab.git (push)
```

两个远程地址各自的分工：

| 远程仓库 | 指向 | 用途 |
|----------|------|------|
| `origin` | 你的 Fork | 推送你的代码（你有写入权限） |
| `upstream` | 原组织仓库 | 同步组织仓库的最新更新（只读） |

---

## 五、克隆的原理（clone）

你已经在任务一执行过 `git clone`，它到底做了什么？

`git clone` 本质就是三步：

```text
1. 在本地创建同名文件夹（git-training-lab）
2. 把远程仓库的所有文件下载到该文件夹
3. 自动生成一个 .git 目录（包含所有历史记录和远程配置）
```

> 💡 之前你 clone 的是组织仓库，所以 origin 自动指向了组织。改远程地址只需要 `git remote set-url`，不需要重新 clone。

---

## 六、分支命名规范

本训练采用统一的**提交分支命名规范**，每位学员创建一个以自己的**入学年份 + 姓名**命名的分支，用来提交训练成果。

### 分支命名格式

```text
submit/<入学年份>-<姓名拼音>
```

| 学员 | 入学年份 | 姓名 | 分支名 |
|------|---------|------|--------|
| 林海鑫 | 2022 | LinHaixin | `submit/2022-linhaixin` |
| 刘艺峰 | 2023 | LiuYifeng | `submit/2023-liuyifeng` |
| 黄柳俊 | 2024 | HuangLiujun | `submit/2024-huangliujun` |
| 李日威 | 2024 | LiRiwei | `submit/2024-liriwei` |

### 规则

- 全部**小写**
- 姓名使用**拼音**（不包含声调）
- 年份和姓名之间用 `-` 连接

### 创建你的提交分支

```bash
git checkout -b submit/2022-linhaixin
```

> 请将 `2022-linhaixin` 替换为你的实际入学年份和姓名拼音。

> 💡 `submit/` 是前缀，表示这是一个"提交训练成果"的分支。每一届学员的分支通过 PR 合并后，将永久保存在仓库历史中，成为实验室传承的一部分。

---

## 七、创建练习文件并提交

在分支上创建你的训练记录文件。

```bash
mkdir -p submissions\你的名字
echo 训练完成 > submissions\你的名字\task03.txt
git add submissions\你的名字\task03.txt
git commit -m "feat: 提交 task03 练习"
```

---

## 八、推送到远程（push）

将你的提交分支推送到**你的 Fork**（origin）。

```bash
git push origin submit/2022-linhaixin
```

### 关于身份认证

如果提示输入用户名和密码：

- **用户名：** 你的 GitHub 用户名
- **密码：** 不是 GitHub 登录密码，而是 **Personal Access Token**

### 生成 Personal Access Token

```text
1. 登录 GitHub，点击右上角头像 → Settings
2. 左侧菜单拉到最下方 → Developer settings
3. Personal access tokens → Tokens (classic)
4. 点击 Generate new token → Generate new token (classic)
5. 勾选 repo 权限
6. 点击 Generate token
7. 复制生成的 Token（关闭页面后就看不到了）
```

> ⚠️ 请保存好 Token，以后每次 push 都会用到它。

---

## 九、发起 Pull Request（PR）

Push 完成后，你的代码已经在你的 Fork 中了。现在通过 PR 请求合并到组织仓库。

### 什么是 Pull Request？

Pull Request（简称 PR）是一种"请求合并"机制——你请求原仓库的管理员把你分支上的代码合并进去。

### 操作步骤

1. 打开你的 GitHub Fork 页面：👉 `https://github.com/你的用户名/git-training-lab`
2. 页面上方会出现一个黄色的提示条，点击 **Compare & pull request**
3. 确认以下信息正确：
   - **base repository：** `DynamiXLab/git-training-lab`（组织仓库）
   - **base：** `main`
   - **head repository：** `你的用户名/git-training-lab`（你的 Fork）
   - **compare：** `submit/2022-linhaixin`
4. 填写 PR 标题（例如：`feat: 提交 2022-linhaixin 训练成果`）
5. 在说明框中简单描述你完成了哪些任务
6. 点击 **Create Pull Request**

### PR 提交后

管理员会看到你的 PR 并进行 Review。如果通过，你的代码就会被合并到组织仓库的 main 分支，你的名字和提交记录将**永久保留在项目历史中**。

如果需要修改，你只需要在本地修改后重新 push 到同一个分支，PR 会自动更新。

---

## 十、同步上游更新（pull）

当组织仓库有新的提交时（比如其他学员的 PR 被合并了），需要同步到本地。

```bash
# 先切回 main 分支
git checkout main

# 从 upstream（组织仓库）拉取最新代码
git pull upstream main

# 再切回你的提交分支，把 main 的更新合并进来
git checkout submit/2022-linhaixin
git merge main
```

> 💡 在开始新任务前，先执行 `git pull upstream main` 确保你的代码基于最新版本。

---

## 十一、章节练习

### 练习内容

走一遍标准的 Fork 工作流：Fork → 配置远程仓库 → 创建提交分支 → 提交 → Push → PR。

### 操作总结

```bash
# 1. 先去 GitHub 上 Fork DynamiXLab/git-training-lab

# 2. 配置远程仓库（只需要做一次）
cd ~/git-training/git-training-lab         # 进入仓库
git remote set-url origin https://github.com/你的用户名/git-training-lab.git
git remote add upstream https://github.com/DynamiXLab/git-training-lab.git

# 3. 创建提交分支
git checkout -b submit/2022-linhaixin

# 4. 创建练习文件
mkdir -p submissions\你的名字
echo 这是我的标准 Fork 工作流练习 > submissions\你的名字\task03.txt

# 5. 提交
git add .
git commit -m "feat: 提交 2022-linhaixin 训练记录"

# 6. 推送到你的 Fork
git push origin submit/2022-linhaixin

# 7. 去 GitHub 上发起 Pull Request
```

---

## 十二、在 VS Code 中操作

### 克隆仓库

VS Code 可以直接从远程克隆仓库：

1. 打开 VS Code，点击 **查看 → 命令面板**（或 `Ctrl + Shift + P`）
2. 输入 `Git: Clone` 并回车
3. 粘贴仓库地址，选择本地存放位置

> 本训练已在任务一用命令行克隆过了，了解这个方式即可。

### 查看当前分支

VS Code 底部状态栏**左下角**会显示当前分支名（如 `main`），点击它可以：

- 切换分支
- 创建新分支
- 发布分支到远程

### 推送代码

1. 打开左侧 **源代码管理** 面板（`Ctrl + Shift + G`）
2. 点击 `...` 菜单 → **推送（Push）**

### 发起 Pull Request

VS Code 可以安装 **GitHub Pull Request** 扩展，在 VS Code 内直接创建和查看 PR，不必打开浏览器。安装方式：

1. 点击左侧扩展图标（`Ctrl + Shift + X`）
2. 搜索 `GitHub Pull Request`
3. 点击安装

> 💡 命令行和 VS Code 可以混着用——命令行深入理解原理，VS Code 提升操作效率。

---

### ✅ 验收标准

- [ ] 在 GitHub 上 Fork 了训练仓库
- [ ] 远程仓库配置正确（`origin` 指向你的 Fork，`upstream` 指向组织仓库）
- [ ] 提交分支名称符合规范 `submit/<年份>-<姓名>`
- [ ] 成功 push 到你的 Fork
- [ ] 发起了 Pull Request，base 为组织仓库
- [ ] PR 标题和说明填写规范
