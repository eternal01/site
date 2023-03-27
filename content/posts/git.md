---
title: "Git"
date: 2022-10-16T10:25:55Z
# bookComments: false
# bookSearchExclude: false
---

## **Git基本概念**
- 工作区(Workspace)
- 暂存区(Index)
- 版本库(Repository)

- Commit
- Tree
- Blob

- HEAD
- Branch
- Tag
## **Git配置**
```zsh
git config --global user.name ""
git config --global user.email ""   #全局配置用户名和邮箱，--global 全局
```
## **Git常用命令**
初始化仓库
```zsh
git init
```
添加本地修改至暂存区
```zsh
git add .   #添加所有修改
git add -A  #添加所有修改
git add filename    #添加具体文件
```
提交本地修改
```zsh
git commit -m "描述"
```
拉取远程文件
```zsh
git pull originname originbranchname
```
合并分支
```zsh
git merge branchname
```
分支变基
```zsh
git rebase branchname
```
修改分支名
```zsh
git branch -m branchname newname
```
删除分支
```zsh
git branch -d branchname
```

```zsh
git branch -D branchname
```
切换分支
```zsh
git checkout branchname
```

```zsh
git switch branchname
```
