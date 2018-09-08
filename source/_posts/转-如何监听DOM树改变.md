---
title: '[转]如何监听DOM树改变'
date: 2017-11-08 10:06:44
tags:
---

# 如何监听DOM树改变[转]

## 读前必看

MutationObserver MDN[https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)     

Mutation Observer API   [http://javascript.ruanyifeng.com/dom/mutationobserver.html#toc10](http://javascript.ruanyifeng.com/dom/mutationobserver.html#toc10)


## 1 概述

Mutation observer 是用于代替 Mutation events 作为观察DOM树结构发生变化时，做出相应处理的API。为什么要使用mutation observer 去代替 mutation events 呢，我们先了解一下mutation events

Mutation Events

Mutation events 是在 DOM3中定义，用于监听DOM树结构变化的事件

### 它简单的用法如下：

```js

document.getElementById('list').addEventListener("DOMSubtreeModified", function(){
  console.log('列表中子元素被修改');
}, false);

```

### Mutation 事件列表

DOMAttrModified 
DOMAttributeNameChanged  
DOMCharacterDataModified  
DOMElementNameChanged  
DOMNodeInserted  
DOMNodeRemoved  
DOMNodeInsertedIntoDocument 
DOMSubtreeModified  

其中DOMNodeRemoved，DOMNodeInserted 和 DOMSubtreeModified 分别用于 监听元素子项的删除，新增，修改(包括删除和新增），
DOMAttrModified 是监听元素属性的修改，并且能够提供具体的修改动作。

### Mutation Events遇到的问题

浏览器兼容性问题
IE9不支持Mutation Events
Webkit内核不支持DOMAttrModified特性，
DOMElementNameChanged和DOMAttributeNameChanged 在Firefox上不被支持。  
性能问题

1. Mutation Events是同步执行的，它的每次调用，都需要从事件队列中取出事件，执行，然后事件队列中移除，期间需要移动队列元素。如果事件触发的较为频繁的话，每一次都需要执行上面的这些步骤，那么浏览器会被拖慢。   
2. Mutation Events本身是事件，所以捕获是采用的是事件冒泡的形式，如果冒泡捕获期间又触发了其他的MutationEvents的话，很有可能就会导致阻塞Javascript线程，甚至导致浏览器崩溃。

`Mutation Observer`

Mutation Observer 是在DOM4中定义的，用于替代 mutation events 的新API，它的不同于events的是，所有监听操作以及相应处理都是在其他脚本执行完成之后异步执行的，并且是所以变动触发之后，将变得记录在数组中，统一进行回调的，也就是说，当你使用observer监听多个DOM变化时，并且这若干个DOM发生了变化，那么observer会将变化记录到变化数组中，等待一起都结束了，然后一次性的从变化数组中执行其对应的回调函数。

Mutation Observer 的浏览器兼容范围


### 兼容性


## 2 方法

### 构造函数

用来实例化一个Mutation观察者对象，其中的参数是一个回调函数，它是会在指定的DOM节点发送变化后，执行的函数，并且会被传入两个参数，一个是变化记录数组(MutationRecord)，另一个是观察者对象本身

```js

new MutationObserver(function(records, itself){});

```

`observe`

在观察者对象上，注册需要观察的DOM节点，以及相应的参数

```js

void observe(Node target, optional MutationObserverInit options)

```

其中的可选参数 MutationObserverInit的属性如下：

`childLIst` 观察目标节点的子节点的新增和删除。  
`attributes` 观察目标节点的属性节点(新增或删除了某个属性,以及某个属性的属性值发生了变化)。  
`characterData` 如果目标节点为characterData节点(一种抽象接口,具体可以为文本节点,注释节点,以及处理指令节点)时,也要观察该节点的文本内容是否发生变化  
`subtree` 观察目标节点的所有后代节点(观察目标节点所包含的整棵DOM树上的上述三种节点变化)  
`attributeOldValue` 在attributes属性已经设为true的前提下, 将发生变化的属性节点之前的属性值记录下来(记录到下面MutationRecord对象的oldValue属性中)  
`characterDataOldValue` 在characterData属性已经设为true的前提下,将发生变化characterData节点之前的文本内容记录下来(记录到下面MutationRecord对象的oldValue属性中)  
`attributeFilter` 一个属性名数组(不需要指定命名空间),只有该数组中包含的属性名发生变化时才会被观察到,其他名称的属性发生变化后会被忽略想要设置那些删选参数的话，
如果想要使用哪个参数的话，就将其值设定为true  

`disconnect`

暂定在观察者对象上设置的节点的变化监听，直到重新调用observe方法

`takeRecords`

在观察者对象上调用takeRecords 会返回 其观察节点上的变化记录(MutationRecord)数组
其中MutationRecord数组也会作为，观察者初始化时的回调函数的第一个参数
其包含的属性如下：

`type` 如果是属性发生变化,则返回attributes.如果是一个CharacterData节点发生变化,则返回characterData,如果是目标节点的某个子节点发生了变化,则返回childList.   
`target` 返回此次变化影响到的节点,具体返回那种节点类型是根据type值的不同而不同的,如果type为attributes,则返回发生变化的属性节点所在的元素节点,如果type值为characterData,则返回发生变化的这个characterData节点.如果type为childList,则返回发生变化的子节点的父节点.    
`addedNodes` 返回被添加的节点    
`removedNodes` 返回被删除的节点  
`previousSibling` 返回被添加或被删除的节点的前一个兄弟节点   
`nextSibling` 返回被添加或被删除的节点的后一个兄弟节点   
`attributeName` 返回变更属性的本地名称  
`oldValue` 根据type值的不同,返回的值也会不同.如果type为attributes,则返回该属性变化之前的属性值.如果type为characterData,则返回该节点变化之前的文本数据.如果type为childList,则返回null   

## 3 使用实例

```js

// Firefox和Chrome早期版本中带有前缀
var MutationObserver = window.MutationObserver || window.WebKitMutationObserver || window.MozMutationObserver
// 选择目标节点
var target = document.querySelector('#some-id'); 
// 创建观察者对象
var observer = new MutationObserver(function(mutations) {  
  mutations.forEach(function(mutation) { 
    console.log(mutation.type); 
  }); 
}); 
// 配置观察选项:
var config = { attributes: true, childList: true, characterData: true } 
// 传入目标节点和观察选项
observer.observe(target, config); 
// 随后,你还可以停止观察
observer.disconnect();

```

原文：[http://www.jianshu.com/p/b5c9e4c7b1e1](http://www.jianshu.com/p/b5c9e4c7b1e1)
