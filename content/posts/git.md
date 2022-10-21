---
title: "Git"
date: 2022-10-16T10:25:55Z
# bookComments: false
# bookSearchExclude: false
---
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
