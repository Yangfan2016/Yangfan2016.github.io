---
title: Element-UI  组件 el-scrollbar 的文档
date: 2019-03-05 17:09:53
tags:
---


## Element-UI  组件 el-scrollbar 的文档

### 前言

我们在 Element-UI 的其他组件里发现，有使用这个组件，并且还不错，由于 Element-UI 官方文档并没有写到关于 `el-scrollbar` 的文档，所以稳定性上可能是有些问题，这里只是爬了源码，翻译了一些属性的使用，仅供参考
 
### 快速使用

```html

<el-scrollbar>
    <li v-for="user in userList" :key="user.id">{{user.name}}</li>
</el-scrollbar>

```

### 具体实例

```html
<el-scrollbar
    wrapClass="yf-container"
    viewClass="yf-content"
    wrapStyle="color:'#fff';fontSize:'16px';"
    viewStyle="color:'#fff';fontSize:'16px';"
    :native="false"
    :noresize="true"
    tag="ul"
>
    <li v-for="user in userList" :key="user.id">{{user.name}}</li>
</el-scrollbar>

```

### 结构构成

![constructor](https://user-gold-cdn.xitu.io/2019/1/21/1686f8b54408c5e2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 文档

Attribute

| 参数 | 说明 | 类型 | 可选值	| 默认值 | 
| :----: | :----: | :----: | :----: | :----: |
| wrapClass | 可选参数，容器的样式名 | string | - | - |
| viewClass | 可选参数，展示视图的样式名 | string | - | - |
| wrapStyle | 可选参数，容器的样式 | string | - | - |
| viewStyle | 可选参数，展示视图的样式 | string | - | - |
| native    | 可选参数，是否使用原生滚动 | boolean | - | false |
| noresize | 可选参数，容器大小是否是不可变的 | boolean | - | false |
| tag      | 可选参数，渲染容器的标签 | string | - | div |


### 备注

源码 [https://github.com/ElemeFE/element/blob/dev/packages/scrollbar/src/main.js](https://github.com/ElemeFE/element/blob/dev/packages/scrollbar/src/main.js)  

文章 [element ScrollBar滚动组件源码深入分析](https://juejin.im/post/5c4583eee51d4526e57d8a5a)