---
title: Git安装和配置
date: 2019-11-08 04:31:36
updated: 2019-11-08 04:31:36
categories:
tags:
- git
---
[[git]]

### 便携版安装问题

配置文件位置：`C:\Users\<user_name>\.gitconfig`

## 关掉换行符自动转换

将配置项`core.autocrlf`设置为`false`

```shell
git config --global core.autocrlf false
```

见[[newline | 换行符]]和[Git - git-config Documentation](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresafecrlf)

不同平台对文件保存时换行的标准规定不一。

Git 作为一个源码版本控制系统，以一种（我看起来）有点越俎代庖、自作聪明的态度，对这个问题提供了一个“解决方案”。

安装好 GitHub 的 Windows 客户端之后，这个功能默认处于“自动模式”。当你在checkout或者add时，Git 会尝试将 UNIX 换行符（LF）替换为 Windows 的换行符（CRLF）；当你在提交文件时，它又试图将 CRLF 替换为 LF。

有些情况会发生，签出时把 UNIX 风格转成了 Windows 风格但提交时并没有转换的情况。造成整个文件不必要的修改提交，破坏这个文件的修改历史。

比如中文后跟着换行符。

### 问题记录

没有关闭默认autocrlf，在我Add 文件时出现警告：

```powershell
PS D:\Projects\template-react-with-typescript> git add .
warning: LF will be replaced by CRLF in .gitignore.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in README.md.
```

### 安装器安装

#### 安装组件

![git-setup-1](git-setup-1.png)

- 大文件支持[Git Large File Storage](https://git-lfs.com/)
- 向Windows terminal 添加一份 Git bash 的配置文件。
代替手动配置：
```json
// windows terminal setting.json
"terminal.integrated.defaultProfile.windows": "gitbash", "terminal.integrated.shell.windows":"E:\app\git\Git\bin\bash.exe",
```
![git-setup-1-1](git-setup-1-1.png)

#### 默认编辑器

![git-setup-2](git-setup-2.png)
代替：
``` gitconfig
// .gitconfig
export EDITOR="code --wait"
```
或者:
``` shell
git config --global core.editor "code --wait"
```

#### 默认分支名

![git-setup-3](git-setup-3.png)
默认分支名。由于 BLM 运动，master 政治不正确了。

#### 环境变量

![git-setup-4](git-setup-4.png)

只添加 Git 相关。Unix 相关的工具可以通过 windows terminal 中的 Git bash profile
 来访问，而不是把一堆MINGW64工具直接添加到Path，避免引入冲突。

#### 选择Git使用的SSH

![git-setup-5](git-setup-5.png)

#### 选择HTTPS使用的后端

![git-setup-6](git-setup-6.png)

#### 行尾符转换

![git-setup-7](git-setup-7.png)

#### 选择Git bash默认的Terminal

![git-setup-8](git-setup-8.png)
直接使用Windows Terminal

#### 选择Git pull默认行为
![git-setup-9](git-setup-9.png)


#### 证书管理工具
![git-setup-10](git-setup-10.png)


#### 额外选项
![git-setup-11](git-setup-11.png)


[version control - How does Git handle symbolic links? - Stack Overflow](https://stackoverflow.com/questions/954560/how-does-git-handle-symbolic-links)
[Git - git-config Documentation](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

#### 配置实验性选项



## 配置代理

## 配置用户

## 配置文件备忘

```config
[filter "lfs"]
	clean = git-lfs clean -- %f
	smudge = git-lfs smudge -- %f
	process = git-lfs filter-process
	required = true
[user]
	name = Nate
	email = xuyanbiaoccc@126.com
[http]
	proxy = socks5://127.0.0.1:10808
[https]
	proxy = socks5://127.0.0.1:10808
[core]
    autocrlf = false
```

## 参考

[Getting started with Git - GitHub Docs](https://docs.github.com/en/get-started/getting-started-with-git)
