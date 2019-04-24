---
title: 入门级的git使用指北
date: 2018-05-08 12:08:03
tags: [博客,Git]
---

> Git 一种免费的开源分布式版本控制系统


## Config

```bash
# 全局配置 加 --global 标识
$ git config --global user.name "your name" 
$ git config --global user.email "xxx@xxx.xxx"
# 本地配置
$ git config user.name "your name"
# 查看配置 --list,-l
$ git config --list
# 查看指定项
$ git config user.name
# 修改指定项
$ git config core.eol lf # git config <key> <value>
# 删除指定项
$ git config --unset core.eol # git config --unset <key>
```

## Basic

```bash

# init
$ git init # 创建一个空的Git仓库或重新初始化一个现有仓库

# add remote origin
$ git remote add origin https://github.com/username/xxxx.git # 添加一个远程仓库 git remote add <shortname> <远程版本库>
$ git remote # 查看已经存在的远程分支
$ git remote -v # 查看已经存在的远程分支的详细信息 git remote -v | --verbose

# pull
$ git pull https://github.com/username/xxxx.git master # 拉取数据
$ git pull origin master # origin 为你之前添加一个远程仓库的名字

# add
$ git add readme.txt # 添加单个文件到本地暂存区
$ git add . # 添加所有修改、新增的文件到本地暂存区
$ git add -u # 添加所有修改、删除的文件到本地暂存区
$ git add -A # 添加所有修改、删除、新增文件到本地暂存区

# commit
git commit -m "注释" # 此次提交的备注

# push
git push -u origin master # 将本地的master分支同步到origin所在主机的master分支


```

## Diff

```bash
$ git diff # 查看冲突
```

## Branch

```bash
$ git branch # 查看所有分支（当前分支有星号标记 e.g. *master）
$ git branch -a # 查看所有分支（包括远程分支）
$ git branch dev # 创建分支
$ git checkout dev  # 切换到 dev 分支
$ git checkout -b feature # 新建并切换到 feature 分支 
$ git branch -m feature test # 重命名   git branch -m <旧名字> <新名字>
$ git branch -D test # 删除分支
```

## Merge

```bash
$ git checkout master # 切换到 mastet 分支
$ git merge dev # 将 dev 合并到 当前分支（这里是 master）
```


## Clone

```bash
$ git clone http://xxx.xxx/xxx.git # git clone <版本库的网址> <本地目录名>
$ git clone http://xxx.xxx/xxx.git mydir # git clone <版本库的网址> <本地目录名>
$ git clone -b dev http://xxx.xxx/xxx.git #git clone -b <分支名称> <版本库的网址>
```

## View

```bash
$ git status # 查看当前工作区提交状态
$ git log # 查看提交历史记录
```

## Undo

git add 之前
```bash
$ git checkout -- a.js # 撤销某个/某些文件的更改
```

git add 之后，git commit 之前
```bash
$ git reset HEAD a.js # 撤销某个/某些文件的添加
```

git commit 之后
```bash
$ git reset HEAD^ --hard # 撤销本次提交（commit）
$ git reset <commit_id> --hard # 撤回到指定的 commit
```

## Editor && Git Config
```bash
$ git config core.autocrlf false # 禁止自动转换换行符
$ git config core.safecrlf true # 禁止换行符混用
$ git config core.eol lf # 设置换行符为 LF 
```

## Relationship

![git-relationship](https://user-gold-cdn.xitu.io/2018/10/28/166b9e70067850da?w=570&h=184&f=png&s=60462)

## Refs
1. [git-scm](https://git-scm.com/docs)
1. [Git教程](https://www.yiibai.com/git/)
