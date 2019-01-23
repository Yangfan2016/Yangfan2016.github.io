---
title: 下载 Github 仓库的指定文件夹
date: 2019-01-23 14:44:47
tags:
---

### 下载整个仓库

```bash
$ git clone https://github.com/Yangfan2016/PersonalWorks.git

```

### 下载仓库内的指定文件

```bash

$ git init
$ git config core.sparseCheckout true # 配置
$ git remote add -f origin https://github.com/Yangfan2016/PersonalWorks.git
$ echo "static-page/" >> .git/info/sparse-checkout # 写入要下载的文件目录
$ echo "web-app/" >> .git/info/sparse-checkout
$ git pull origin master
```