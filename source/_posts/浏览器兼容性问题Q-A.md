---
title: 浏览器兼容性问题Q&A
date: 2018-05-14 11:07:01
tags: [浏览器,兼容性]
---

# 浏览器兼容性问题Q&A

### `Date` 对象时间格式差异性

Q:  

日期参数为 `yyyy/MM/dd hh:mm:ss` 格式时，IE浏览器不支持，chrome和firefox支持
```js
new Date("2018-05-05 12:25:23") // Invalid Date
```

日期参数为 `yyyy/MM/ddThh:mm:ss` 格式时，IE支持,chrome和firefox也支持

```js
new Date("2018-05-05T12:25:23") // [date] Sat May 05 2018 12:25:23 GMT+0800 (中国标准时间)
```

A:  
原因：
各个浏览器对采用的时间格式标准解析不一致

解决方法：
最好采用ISO的标准时间格式`YYYY-MM-DDTHH:mm:ss.sssZ`
或者使用时间戳代替

```js

new Date("2018-05-05T12:25:23.235Z");

// or

new Date(1525523123235)

```


### `window.open` 在异步代码中执行被浏览器拦截  
Q: 
下面方式打开新页面会被浏览器拦截
```js

// 异步代码

setTimeout(function () {
    window.open("http://www.sogou.com")
},1001)
// AJAX异步请求

$.ajax({
    url:"xxx",
    success:function () {
        window.open("yyy")
    }
});

```
A:  
原因：
出于安全考虑浏览器会拦截掉非用户操作的行为 

解决方法：
```js

// window.open()先执行，打开一个空的窗口（例如 加载页）

var newWin = window.open('http://xxx.com/loading.html')

// 然后等异步代码执行完再重定向 

newWin.location.href = 'http://www.baidu.com'
```

### `a` 标签href属性为`javascript:void(0)` 时，各浏览器处理方式不同

Q:  
```html
<a href="javascript:void(0)" target="_blank">我是空链接，在这里当做一个按钮</a>
```
firefox和IE浏览器会弹出一个新窗口

A:  
原因：
浏览器默认处理事件的顺序有差异。   
Chrome顺序：onclick -> href -> target   
IE和Firefox顺序：onclick -> target -> href

解决方法：  
1. 添加onclick事件，直接return false阻止之后浏览器默认事件的执行
```html
<a target="_blank" onclick="return false" href="javascript:void(0);">点我啊</a>
```
2. 去掉href属性
```html
<a target="_blank">点我啊</a>
```
```html
<a>点我啊</a>
```


### 各浏览器对 `table` 不规范写法解析不一致

最好以W3C的标准来写HTML，不然浏览器对HTML解析有差异  

```html
<table border="1">
    <thead>
        <tr>
            <th>Month</th>
            <th>Savings</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>January</td>
            <td>$100</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>Sum</td>
            <td>$180</td>
        </tr>
    </tfoot>
</table>
```

### 鼠标事件 mouseout 和 mouseleave 区别

- `mouseleave` 只有鼠标移开被监听的元素时，才触发 （`mouseenter`同理）  
- `mouseout` 不论鼠标移开被监听元素，还是子元素，均会触发（`mouseover`同理） 

### window.URL.createObjectURL(new Blob) 在Microsoft IE和Chrome 的区别  [https://blog.csdn.net/u013131203/article/details/80894440](https://blog.csdn.net/u013131203/article/details/80894440)

Chrome 带域名

```js

URL.createObjectURL(new Blob())
// "blob:https://note.youdao.com/e4132750-7b95-4595-b331-158267d8d9e3"

```

ie不带域名

```js

URL.createObjectURL(new Blob())
// "blob:e4132750-7b95-4595-b331-158267d8d9e3"

```

### IE浏览器缓存 get方式的XHR请求 [https://www.cnblogs.com/bk233/p/7280595.html](https://www.cnblogs.com/bk233/p/7280595.html)

每次发请求时携带一个随机参数 例如  GET http://123.com?t=15123153156161


### 浅谈 AJAX 跨域请求时的 OPTIONS 方法 [https://juejin.im/entry/58eaf351a22b9d0058a8e35c](https://juejin.im/entry/58eaf351a22b9d0058a8e35c)

`For cross-domain requests, setting the content type to anything other than application/x-www-form-urlencoded, multipart/form-data, or text/plain will trigger the browser to send a preflight OPTIONS request to the server.`    
非简单请求必须先发送预检请求，如果发送的请求内容类型如果不是 `application/x-www-form-urlencoded`，`multipart/form-data` 或 `text/plain` 这三者的话，便会触发 OPTIONS 请求