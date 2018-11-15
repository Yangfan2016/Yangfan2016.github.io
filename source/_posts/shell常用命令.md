---
title: shell常用命令
date: 2017-07-16 19:59:04
tags:
---

# shell常用命令

#### 1 ls命令：列出文件

 `ls -la` 列出当前目录下的所有文件和文件夹

 `ls a*` 列出当前目录下所有以a字母开头的文件

 `ls -l *.txt` 列出当前目录下所有后缀名为txt的文件

#### 2 cp命令：复制 

`cp a.txt b.txt` 把文件a的内容复制到b文件

`cp a.txt ./test`  把文件a复制到text目录下

`cp -a test test2` 递归的把目录test下所有文件（包括隐藏的文件）复制到新的目录 test2

#### 3  cat命令：查看 组合文件

`cat a.txt` 查看文件的内容

`cat a.txt >> b.txt` 把a文件的内容组合到b文件内容的末尾

`cat -n a.txt` 查看文件并给文件标上行号

#### 4  touch命令：建立文件

`touch a.txt` 建立一个名为a的txt类型文件

#### 5  rm命令：删除文件

`rm -rf a.txt` 强制删除文件a.txt

`rm -i a.txt` 删除文件前会有提示是否确定删除该文件

#### 6  mkdir命令：创建目录

`mkdir test` 创建一个名为test的目录

#### 7  rmdir命令：删除目录

`rmdir test` 删除一个目录

#### 8  echo、cat命令：添加内容

`echo “hello world!” >> a.txt` 添加内容到文件a里面

`cat <<EOF>> a.txt ` 可以添加多行语句到文件本身内容的末尾

`cat <<EOF> a.txt` 添加内容到文件并覆盖到原始的内容

#### 9  mv命令：移动 重命名文件

`mv a.txt b.txt` 文件a重新命名为b

`mv a.txt ./test` 把文件移动到一个目录下

#### 10  cd命令：更换目录

`cd ~ `  切换到用户目录

`cd .. ` 返回到上一层目录

`cd ../.. ` 返回到上二层目录

#### 11  grep命令：搜索文件

`ls -la | grep a.txt ` 搜索a.txt文件

#### 12  find命令：查找文件和目录

`find filename` 查找当前目录下是否有该文件/目录

#### 13  rz sz命令：上传和下载文件

#### 14  head命令：显示文件的前10行内容

#### 15  tail命令：显示文件最后10行内容


