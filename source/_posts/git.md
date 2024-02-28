---
title: 版本管理Git
date: 2019-11-08 04:31:36
updated: 2019-11-08 04:31:36
categories:
tags:
- git
---

`--`跟全名长参数，`-`跟等价的缩写短参数。比如 -d 和 --delete

## git clone

### 语法

```shell
git clone [--template=<template_directory>]
	  [-l] [-s] [--no-hardlinks] [-q] [-n] [--bare] [--mirror]
	  [-o <name>] [-b <name>] [-u <upload-pack>] [--reference <repository>]
	  [--dissociate] [--separate-git-dir <git dir>]
	  [--depth <depth>] [--[no-]single-branch] [--no-tags]
	  [--recurse-submodules[=<pathspec>]] [--[no-]shallow-submodules]
	  [--[no-]remote-submodules] [--jobs <n>] [--sparse]
	  [--filter=<filter>] [--] <repository>
	  [<directory>]
```

### 常用示例

```shell
git clone https://github.com/next-theme/hexo-theme-next themes/next
```



## git add

**git add** 命令可将该文件添加到暂存区。

### 常用示例

#### 添加当前目录下的所有文件到暂存区
```shell
git add .
```

## git push

[Git - git-push Documentation](https://git-scm.com/docs/git-push)

使用本地 refs 更新远程 refs

### 常用示例

将本地的 master 分支推送到 origin 主机的 master 分支。

```shell
git push origin master
```

等价于

```shell
git push origin master:master
```









## git branch

[Git - git-branch Documentation](https://git-scm.com/docs/git-branch)

### 语法

```shell
git branch [--color[=<when>] | --no-color] [--show-current]
	[-v [--abbrev=<n> | --no-abbrev]]
	[--column[=<options>] | --no-column] [--sort=<key>]
	[--merged [<commit>]] [--no-merged [<commit>]]
	[--contains [<commit>]] [--no-contains [<commit>]]
	[--points-at <object>] [--format=<format>]
	[(-r | --remotes) | (-a | --all)]
	[--list] [<pattern>…​]
git branch [--track[=(direct|inherit)] | --no-track] [-f]
	[--recurse-submodules] <branchname> [<start-point>]
git branch (--set-upstream-to=<upstream> | -u <upstream>) [<branchname>]
git branch --unset-upstream [<branchname>]
git branch (-m | -M) [<oldbranch>] <newbranch>
git branch (-c | -C) [<oldbranch>] <newbranch>
git branch (-d | -D) [-r] <branchname>…​
git branch --edit-description [<branchname>]
```


``--move` 加上`--force`表示即使新分支名称已经存在，也允许重命名分支。
`-M`是`--move --force`的缩写。


### 常用示例

#### 修改分支名称

修改当前分支master的名称为`main`

```shell
git branch -m main
```

提交到远程（因为远程分支不存在，则会被新建，并追踪）

```shell
git push origin main
```

删除远程之前的分支

```
git push origin -d master
```
等同于
```
$ git push origin --delete master
```

## 常用

### 创建一个新项目并推到远程仓库
```
echo "# template-react-with-typescript" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:natexub/template-react-with-typescript.git
git push -u origin main
```

### push an existing repository from the command lin

```
git remote add origin git@github.com:natexub/template-react-with-typescript.git
git branch -M main
git push -u origin main
```

## git merge 和 rebase

merge会保存完整的历史记录。针对容易反复出现的问题，完整的历史记录很重要，他保存了这个错误被反复纠正的过程，后来者可以通过查看历史来避免再次犯错。

rebase会通过修改历史记录，形成干净整洁的提交历史。在本地仓库拉取代码时进行


## 参考
[Git远程操作详解 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)