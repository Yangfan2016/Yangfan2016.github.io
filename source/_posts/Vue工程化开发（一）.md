---
title: Vue工程化开发（一）
date: 2018-01-12 10:58:42
tags:
---

# Vue工程化开发（一）



> 利用Vue-cli脚手架在现有的MVC项目中进行前端开发

#### 开发环境
- Node
- npm
- Vue-cli


## 搭建项目

#### 1. 首先在项目中搭建一个前台文件夹，用 `vue init webpack projectname` （例如: `vue init webpack frontend`） 

frontend 为Vue-cli自动生成的项目文件  
content 目录下存放打包后的资源文件  
views目录下存放自动生成的cshtml文件  

![](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/webpack-vue-cli-004.png) 
  
  
#### 2. 进入此文件夹（这里是 frontend）   配置 package.json ，安装所需插件  

![](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/webpack-vue-cli-005.png) 


#### 3. 修改打包配置   进入 config/index.js     修改如下  

![](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/webpack-vue-cli-001.jpg)

#### 4. 在当前目录下新建一个模板页 index.cshtml  如下

![](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/webpack-vue-cli-002.jpg)  

#### 5. 修改打包html文件的配置  进入 build/webpack.prod.conf.js

![](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/webpack-vue-cli-003.png) 

#### 6. `npm run dev`  测试  

#### 7. `npm run build` 打包  


## 项目目录

![](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/Blog/webpack-vue-cli-006.png)

