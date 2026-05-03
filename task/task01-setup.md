# 任务一：环境准备

本任务用于完成 Git 环境的安装与基础配置

## 一、安装 Git

进入 Git 官网下载安装包：


👉 https://git-scm.com/ 

按照默认配置完成安装即可。

## 二、打开命令行

安装完成后，按下：

`Win + R`

输入：

`cmd`

打开命令行工具（Command Prompt）。

## 三、验证安装

在命令行中输入：

`git --version`

若安装成功，将输出类似内容：

`git version 2.38.1.windows.1`

## 四、配置用户信息（必须）

Git 通过用户名和邮箱标识提交者身份，此步骤为必做项。

### 命名规范建议
* 用户名建议使用姓名拼音（驼峰式）
  
  例如：

  `林海鑫 -> LinHaixin`
* 邮箱使用常用邮箱即可（如 QQ / 网易）

### 配置命令

```
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

## 六、编辑软件安装

### 一、下载安装

本节用于配置基础开发环境，推荐使用 Visual Studio Code（简称 VS Code）作为代码编辑器。
推荐使用`vscode`做编辑软件，
进入官网下载

👉 https://code.visualstudio.com/

按照默认配置完成安装即可。

### 二、安装中文语言包

打开 VS Code 后，在左侧点击：

`Extensions（扩展）`

或使用快捷键：

`Ctrl + Shift + X`

在打开的扩展商店中搜索：

`Chinese`

选择官方语言包：

* **Chinese (Simplified) Language Pack for Visual Studio Code**

* 点击 **Install（安装）**

安装完成后，右下角 VS Code 会提示是否切换语言：

* 点击 Restart（重启）
* 即可完成汉化