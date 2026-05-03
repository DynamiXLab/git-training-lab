# 任务三：仓库克隆（clone）与远程获取

本章介绍如何从远程仓库获取项目代码，并在本地进行使用与开发。


## 一、什么是克隆（clone）

clone 的作用是：

> 将远程仓库完整复制到本地，包括所有代码、提交记录与分支信息。

适用于：

* 参与已有项目开发
* 获取开源代码
* 团队协作拉取项目

## 二、基本克隆命令
```
git clone <仓库地址>
```
例如：
```
git clone https://github.com/user/demo.git
```
执行后会在当前目录生成一个同名文件夹：
```
demo/
```

## 三、进入项目目录

克隆完成后需要进入项目：
```
cd demo
```
查看文件：
```
ls
```
或 Windows：
```
dir
```

## 四、常见克隆方式

### 1. HTTPS方式（最常用）
```
git clone https://github.com/user/repo.git
```

特点：

* 无需配置 SSH
* 适合初学者
* 每次 push 可能需要输入账号密码或 token

### 2. SSH方式（推荐长期使用）
```
git clone git@github.com:user/repo.git
```
特点：

* 需要提前配置 SSH Key
* 免密操作
* 更适合开发环境

## 章节练习

