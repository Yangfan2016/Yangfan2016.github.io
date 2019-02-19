---
title: 你所不知道的 Chrome 控制台
date: 2019-02-19 15:21:25
tags:
---


## 你所不知道的 Chrome 控制台调试技巧

### 前言

收集了一些工作中常用到的 Chrome 调试技巧，其他大部分 Chrome 调试功能工具介绍已经在其他的很多文章中介绍到了，这里就不 DRY 了

### Request blocking 阻塞请求

使用这个功能可以拦截请求  

大部分情况是页面执行完某个操作后，页面就重定向了（比如登录），这时就不方便调试了，下面这个工具很好的解决了这个问题

比如你想了解在页面重定向之前，发去的登录请求都做了什么，使用操作如下图所示：

![console-reqblock-001](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/console-reqblock-001.png)

### Coverage 代码覆盖率

可以观察到代码覆盖率，哪些是没用的，去除无用代码，较少代码体积  


你需要点击下图的记录按钮进行记录，然后你需要在页面上进行一些交互操作（如点击、鼠标移入等）

![console-coverage-001](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/console-coverage-001.png)

然后你会得到下图，红色的部分是没有用到的代码占比，绿色部分是用到的代码占比，你可以点击占比进度条，到达指定源代码区进行细致查看

![console-coverage-002](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/console-coverage-002.png)

### Changes 变化

显示更改代码的比较，你可以通过这个工具观察，你用控制台修改过的代码，就和 git 的 diff 功能类似，红色代表删除、绿色代码新增

![console-changes-001](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/console-changes-001.png)

你如果点击代码里的某一行，它会打开源代码面板，你可以在这里进行修改、调试操作

![console-changes-002](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/console-changes-002.png)

### Snippets 代码片段

一般在 console 里可以临时运行代码，但是书写格式不太友好，而且一换行就执行了（当然你可以 shift+enter 换行），这时，你又懒癌发作，不想打开代码编辑器，你可以使用 Snippets 这个工具，如下图所示：

![console-snippets-001](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/console-snippets-001.png)

可以像代码编辑器一样书写，可以格式化代码、可以断点调试、也可以单独导出文件