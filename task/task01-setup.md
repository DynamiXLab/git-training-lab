# 任务一：环境准备

本任务用于完成 Git 环境的安装与基础配置。

---

## 一、安装 Git

### Windows 系统

进入 Git 官网下载安装包：

👉 https://git-scm.com/

点击页面上显示的 Windows 版本，下载后双击运行，**一路默认配置**即可完成安装。

> 安装过程中如有选项，保持默认不动。

### Linux 系统

打开终端（Terminal），根据你的发行版选择对应命令：

```bash
# Debian / Ubuntu /  Deepin / 麒麟
sudo apt update
sudo apt install git

# Fedora / CentOS / RHEL
sudo dnf install git

# Arch Linux / Manjaro
sudo pacman -S git
```

安装完成后可以跳到第二步验证。

---

## 二、打开命令行工具

### Windows 系统

按下 `Win + R`，输入 `cmd`，回车。

或者搜索"命令提示符"打开。

### Linux 系统

打开系统自带的终端（Terminal）。

> 快捷键通常是 `Ctrl + Alt + T`（Ubuntu 等）。

---

## 三、验证安装

在命令行中输入：

```bash
git --version
```

若安装成功，将输出类似内容：

- **Windows：** `git version 2.38.1.windows.1`
- **Linux：** `git version 2.34.1`

如果显示 `command not found` 或类似的提示，说明安装未成功，请检查上一步或询问教官。

---

## 四、配置用户信息（必须）

Git 通过用户名和邮箱标识提交者身份，此步骤为**必做项**。

Windows 和 Linux 的命令完全相同。

### 命名规范建议

- **用户名**建议使用姓名拼音（驼峰式）
  - **例如：**
  -  `林海鑫` → `LinHaixin`  
     `刘艺峰` → `LiuYifeng`  
     `黄柳俊` → `HuangLiujun`  
     `李日威` → `LiRiwei`
- **邮箱**使用常用邮箱即可（如 QQ / 网易邮箱等）

### 执行配置

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

### 验证配置

```bash
git config --global --list
```

如果输出中有 `user.name` 和 `user.email`，说明配置成功。

---

## 五、安装 VS Code 编辑器

VS Code（Visual Studio Code）是轻量、好用的代码编辑器，也是本训练推荐的编辑工具。

### Windows 系统

1. 进入官网下载：👉 https://code.visualstudio.com/
2. 点击 **Download for Windows**
3. 下载后双击运行，**一路默认配置**即可
4. 建议在安装时勾选"添加到右键菜单"（方便以后用 VS Code 打开文件夹）

### Linux 系统

根据发行版选择一种方式：

```bash
# 方式一：下载 .deb / .rpm 包（推荐）
# 进入 https://code.visualstudio.com/ 下载对应安装包
# Debian / Ubuntu
sudo dpkg -i 下载的.deb文件

# Fedora / CentOS
sudo rpm -i 下载的.rpm文件

# 方式二：使用 Snap（Ubuntu 适用）
sudo snap install code --classic

# 方式三：使用 apt 仓库（Ubuntu / Debian）
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg
sudo apt update
sudo apt install code
```

> 如果使用 Linux 方式三感觉复杂，直接用方式一下载安装包即可。

---

## 六、安装中文语言包（选做）

打开 VS Code 后，在左侧点击 **Extensions（扩展）**，或按快捷键 `Ctrl + Shift + X`。

在搜索框中输入：

```
Chinese
```

找到 **Chinese (Simplified) Language Pack for Visual Studio Code**（简体中文语言包），点击 **Install**。

安装完成后，右下角会提示是否重启切换语言，点击 **Restart** 即可完成汉化。

> 此步骤 Windows 和 Linux 操作完全一样。

---

## 七、配置 SSH 密钥（推荐）

使用 HTTPS 方式操作 Git 时，每次 push 都可能需要输入 Token。
配置 SSH 密钥后，可以实现**免密操作**，更加方便和安全。

> 💡 此步骤为推荐项。如果你觉得配置 Token 也能接受，可以跳过，不影响后续训练。

### 第一步：检查是否已有 SSH 密钥

```bash
# Windows
dir %USERPROFILE%\.ssh

# Linux
ls -la ~/.ssh
```

如果看到 `id_ed25519.pub` 或 `id_rsa.pub` 文件，说明已有密钥，可以跳到第四步。

### 第二步：生成新的 SSH 密钥

```bash
ssh-keygen -t ed25519 -C "你的邮箱"
```

执行后：
1. 提示 `Enter file in which to save the key` — **直接回车**（使用默认路径）
2. 提示 `Enter passphrase` — **直接回车**（不设密码）
3. 再次回车确认

### 第三步：查看公钥内容

```bash
# Windows
type %USERPROFILE%\.ssh\id_ed25519.pub

# Linux
cat ~/.ssh/id_ed25519.pub
```

输出类似：

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA.... <你的邮箱>
```

**复制整段内容**（从 `ssh-ed25519` 到邮箱末尾）。

### 第四步：添加到 GitHub

1. 登录 GitHub，点击右上角头像 → **Settings**
2. 左侧菜单选择 **SSH and GPG keys**
3. 点击 **New SSH key**
4. 标题（Title）填写标识名称，方便你辨认是哪台电脑：
   - **自己的电脑** → 填写 `个人电脑`
   - **实验室/学校公共电脑** → 填写 `实验室训练电脑`
   - 也可以按自己的想法写，能认出来就行
5. 密钥（Key）粘贴你刚才复制的内容
6. 点击 **Add SSH key**

### 第五步：验证配置

```bash
ssh -T git@github.com
```

看到类似输出即表示成功：

```
Hi 你的用户名! You've successfully authenticated, but GitHub does not provide shell access.
```

> 💡 配置一次后，后续的 clone 和 push 都不需要再输入密码或 Token。

---

## 八、克隆训练仓库

接下来将本训练项目从 GitHub 克隆到本地，后面的任务都基于这个仓库完成。

**先执行命令，遇到看不懂的不要慌，后面的任务会逐一解释。**

### Windows 系统

```bash
D:
cd \git-training
git clone https://github.com/DynamiXLab/git-training-lab.git
cd git-training-lab
```

### Linux 系统

```bash
cd ~/git-training
git clone https://github.com/DynamiXLab/git-training-lab.git
cd git-training-lab
```

执行完成后，用 `dir`（Windows）或 `ls`（Linux）查看，应该能看到 `task/`、`README.md` 等文件。

> 如果之前没创建 `git-training` 目录，请先参考任务二的步骤创建。

---

## 九、在 VS Code 中操作

### 使用 VS Code 内置终端

VS Code 内置了命令行终端，不需要在 VS Code 和 cmd 之间来回切换。

1. 打开 VS Code
2. 点击菜单栏 **终端 → 新建终端**，或按快捷键 `` Ctrl + ` ``
3. 底部会打开一个终端面板，在这里输入 Git 命令即可

### 在 VS Code 中打开仓库

1. 点击 **文件 → 打开文件夹**（或 `Ctrl + K Ctrl + O`）
2. 选择本地的 `git-training` 或 `git-training-lab` 文件夹
3. 左侧显示文件列表，底部运行命令，一个窗口搞定

> 💡 后续所有任务都建议在 VS Code 中完成：左侧看文件、底部跑命令，方便又直观。

---

## ✅ 本章验收标准

- [ ] `git --version` 能正常输出版本号
- [ ] 已配置 `user.name` 和 `user.email`
- [ ] 能用 `git config --global --list` 看到自己的配置
- [ ] VS Code 已安装并能正常打开
- [ ] 已克隆训练仓库到本地（`git-training-lab/` 目录存在）
- [ ] （推荐）已配置 SSH 密钥，`ssh -T git@github.com` 验证通过
