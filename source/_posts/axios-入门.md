---
title: axios 入门
date: 2017-10-12 9:00:50
tags:
---

# axios 入门

#### 请忽略大图，不只是Vue，其他js均可用

![001](https://devdojo.com/media/images/June2017/vue-js-and-ajax-requests.jpg?fm=jpg&q=70&s=587cf6e4e2ed983ae163ebc0ebef71a8)



#### Editor: Yangfan 2017-08-17

#### Intro: 本文档参考axios的官方文档和网上的axios使用教程所写，后续还会不断完善，鄙人水平有限，还望批评指正


---

**强烈建议查看完整文档，链接如下**

英文原文档： [https://github.com/mzabriskie/axios](https://github.com/mzabriskie/axios)   

中文文档：     
[https://www.awesomes.cn/repo/mzabriskie/axios](https://www.awesomes.cn/repo/mzabriskie/axios)   
[https://segmentfault.com/a/1190000008470355](https://segmentfault.com/a/1190000008470355)    


---



## 什么是axios

引用axios的介绍：

> Promise based HTTP client for the browser and node.js




axios是一个基于ES6的Promise的网络请求库，是一个ajax库。


可以实现：

- 在浏览器里建立XHR

- 通过nodejs进行http请求

甚至可以实现：

- 转换或者拦截请求数据或响应数据

- 支持Promise的API

- 可以取消请求

- 自动转换JSON

- 可以防御XSRF攻击！

浏览器支持也没什么问题，IE这种本时代异端都能支持到8+，这问题是不大了。（VUE支持到9+！）

![Chrome](https://raw.github.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![Firefox](https://raw.github.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![Safari](https://raw.github.com/alrra/browser-logos/master/src/safari/safari_48x48.png) | ![Opera](https://raw.github.com/alrra/browser-logos/master/src/opera/opera_48x48.png) | ![Edge](https://raw.github.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![IE](https://raw.github.com/alrra/browser-logos/master/src/archive/internet-explorer_9-11/internet-explorer_9-11_48x48.png) |
--- | --- | --- | --- | --- | --- |
Latest ✔ | Latest ✔ | Latest ✔ | Latest ✔ | Latest ✔ | 8+ ✔ |

[![Browser Matrix](https://saucelabs.com/open_sauce/build_matrix/axios.svg)](https://saucelabs.com/u/axios)


## 怎么用

### 方法一  npm安装

1. npm下载

```
npm install axios

```
2. webpack之类的打包工具导入

```js

import axios from 'axios'

// or

var axios=require('axios');



//===============Vue===================
// Vue全局引用及使用

import axios from 'axios'
Vue.prototype.$http = axios

this.$http.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

```


### 方法二  script标签引入

```html

<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

```

## 用法及DEMO

### 简单的demo

**一个简单的HTTP GET请求**

```js

// 法一 请求参数写到url中

axios.get('http://getRes.php?id=123456&name=Yangfan').then(function (response) {
  // TODO
  // 返回HTTP请求成功的数据
}).catch(function (error) {
  // TODO
  // 返回HTTP请求失败的失败信息
});

// 法二 请求参数写到axios配置参数中

axios.get('http://getRes.php'，{
  params:{
    id:123456,
    name:"Yangfan"
  }
}).then(function (response) {
  // TODO
  // 返回HTTP请求成功的数据
}).catch(function (error) {
  // TODO
  // 返回HTTP请求失败的失败信息
});


```

**一个简单的HTTP POST 请求**

```js

axios.post('http://getRes.php',{
  id:123456,
  name:"Yangfan"
}).then(function (response) {
  // TODO
  // 返回HTTP请求成功的数据
}).catch(function (error) {
  // TODO
  // 返回HTTP请求失败的失败信息
});

```

**自定义请求配置**

```js

// 配置
var config={
  method: 'GET',  // 请求方法                                           
  url: 'http://getRes.php',  // 请求url                          
  headers: { 
    token: 'ftv1443qby6bdfa41t90sfvq89hg3h54u989m9imog79g4'   // 请求头                                              
  },
  data: {         // 需要传递的数据                                 
    id: 123456,
    name: 'Yangfan'
  }
};

axios(config).then(function (response) {
  // TODO
  // 返回HTTP请求成功的数据
}).catch(function (error) {
  // TODO
  // 返回HTTP请求失败的失败信息
});

```


## 完整配置

<table>
	<thead>
		<tr>
			<th>参数</th>
			<th>类型</th>
			<th>注解</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>url</td>
			<td>String</td>
			<td>`url`是服务器链接，用来请求</td>
		</tr>
		<tr>
			<td>method</td>
			<td>String</td>
			<td>`method`是发起请求时的请求方法</td>
		</tr>
		<tr>
			<td>baseURL</td>
			<td>String</td>
			<td>`baseURL`如果`url`不是绝对地址，那么将会加在其前面。</td>
		</tr>
		<tr>
			<td>transformRequest</td>
			<td>Function</td>
			<td>`transformRequest`允许请求的数据在传到服务器之前进行转化。</td>
		</tr>
		<tr>
			<td>transformResponse</td>
			<td>Function</td>
			<td>`transformResponse`允许返回的数据传入then/catch之前进行处理</td>
		</tr>
		<tr>
			<td>headers</td>
			<td>Object</td>
			<td>`headers`是自定义的要被发送的头信息</td>
		</tr>
		<tr>
			<td>params</td>
			<td>Object</td>
			<td>`params`是请求连接中的请求参数，必须是一个纯对象，或者URLSearchParams对象</td>
		</tr>
		<tr>
			<td>paramsSerializer</td>
			<td>Function</td>
			<td>`paramsSerializer`是一个可选的函数，是用来序列化参数</td>
		</tr>
		<tr>
			<td>data</td>
			<td>Object</td>
			<td>`data`是请求提需要设置的数据
    <br />只适用于应用的'PUT','POST','PATCH'，请求方法
    <br />当没有设置`transformRequest`时，必须是以下其中之一的类型（不可重复？）：
    <br />-string,plain object,ArrayBuffer,ArrayBufferView,URLSearchParams
    <br />-仅浏览器：FormData,File,Blob
    <br />-仅Node：Stream</td>
		</tr>
		<tr>
			<td>timeout</td>
			<td>Number</td>
			<td>`timeout`定义请求的时间，单位是毫秒。</td>
		</tr>
		<tr>
			<td>withCredentials</td>
			<td>Boolean</td>
			<td>`withCredentials`表明是否跨网站访问协议，</td>
		</tr>
		<tr>
			<td>adapter</td>
			<td>Function</td>
			<td>`adapter`适配器，允许自定义处理请求，这会使测试更简单。</td>
		</tr>
		<tr>
			<td>auth</td>
			<td>Object</td>
			<td>`auth`表明HTTP基础的认证应该被使用，并且提供证书。
    <br />这个会设置一个`authorization` 头（header），并且覆盖你在header设置的Authorization头信息。</td>
		</tr>
		<tr>
			<td>responseType</td>
			<td>String</td>
			<td>`responsetype`表明服务器返回的数据类型，这些类型的设置应该是
    <br />'arraybuffer','blob','document','json','text',stream'</td>
		</tr>
		<tr>
			<td>xsrfCookieName</td>
			<td>String</td>
			<td>`xsrfCookieName`是cookie名，用作xsrf token值</td>
		</tr>
		<tr>
			<td>xsrfHeaderName</td>
			<td>String</td>
			<td>`xsrfHeaderName` 是http头（header）的名字，并且该头携带xsrf的值</td>
		</tr>
		<tr>
			<td>onUploadProgress</td>
			<td>Function</td>
			<td>`onUploadProgress`允许处理上传过程的事件</td>
		</tr>
		<tr>
			<td>onDownloadProgress</td>
			<td>Function</td>
			<td>`onDownloadProgress`允许处理下载过程的事件</td>
		</tr>
		<tr>
			<td>maxContentLength</td>
			<td>Number</td>
			<td>`maxContentLength` 定义http返回内容的最大容量</td>
		</tr>
		<tr>
			<td>validateStatus</td>
			<td>Function</td>
			<td>`validateStatus` 定义promise的resolve和reject。
    <br />http返回状态码，如果`validateStatus`返回true（或者设置成null/undefined），promise将会接受；其他的promise将会拒绝。</td>
		</tr>
		<tr>
			<td>maxRedirects</td>
			<td>Number</td>
			<td>`maxRedirects`定义了node.js中要重定向的最大数量。
  <br />如果设置为0，则不会重定向。</td>
		</tr>
		<tr>
			<td>httpAgent</td>
			<td>Object</td>
			<td>`httpAgent` 和 `httpsAgent`当产生一个http或者https请求时分别定义一个自定义的代理，在nodejs中。
    <br />这个允许设置一些选选个，像是`keepAlive`--这个在默认中是没有开启的。</td>
		</tr>
		<tr>
			<td>httpsAgent</td>
			<td>Object</td>
			<td>`httpAgent` 和 `httpsAgent`当产生一个http或者https请求时分别定义一个自定义的代理，在nodejs中。
    <br />这个允许设置一些选选个，像是`keepAlive`--这个在默认中是没有开启的。</td>
		</tr>
		<tr>
			<td>proxy</td>
			<td>Object</td>
			<td>`proxy`定义服务器的主机名字和端口号。
    <br />`auth`表明HTTP基本认证应该跟`proxy`相连接，并且提供证书。
    <br />这个将设置一个'Proxy-Authorization'头(header)，覆盖原先自定义的。</td>
		</tr>
		<tr>
			<td>cancelToken</td>
			<td>Function</td>
			<td>`cancelTaken` 定义一个取消，能够用来取消请求</td>
		</tr>
	</tbody>
</table>


## 返回结果

```js

{
  // 服务器返回的数据                                        
  data: {},

  // 服务器返回的状态码                              
  status: 200,

  // 服务器返回的状态信息                                                                     
  statusText: 'OK',

  // 服务器返回的响应头信息                      
  headers: {},

  // `config`是提供给`axios`的请求的配置
  config: {}
}

```
