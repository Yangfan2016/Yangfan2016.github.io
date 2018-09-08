---
title: 高德地图 Javascript API 入门（一）
date: 2017-02-20 19:32:00
tags: [GIS,API,JS]
---


高德地图 Javascript API 入门（一）
===


准备工作
---

#### 首先注册个开发者账号

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap101.JPG)

#### 然后创建应用，获取Key

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap102.JPG)

#### 新建HTML文件，在body标签中引入如下代码（把你的Key值填入即可）

HTML

```html
	<script type="text/javascript" src="http://webapi.amap.com/maps?v=1.3&key=您申请的key值"></script>
```

#### 创建设置地图容器

HTML

```html
	<div id="container"></div>  
```

CSS

```css
	#container {width:300px; height: 180px; }  
```

地图初始化
---

#### 创建地图

JS

```js
	var map = new AMap.Map('container');
```


#### 设置地图参数

可以通过以下设置  
JS

```js
	var map = new AMap.Map('container',{
    	zoom: 12,
    	center: [112.549248,37.852135]
	});
```


也可通过map对象的方法设置

JS

```js
	var map = new AMap.Map('container');
	map.setZoom(12);
	map.setCenter([112.549248,37.852135]);
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap103.JPG "高德地图")


个性化地图
---

#### 改变地图样式

目前支持五种地图配色模版

地图类型列表


<table style="width:100%;border-collapse:collapse;"><tbody><tr><th><p>名称</p></th><th><p>说明</p></th></tr><tr><td><p>normal</p></td><td><p>默认样式</p></td></tr><tr><td><p>dark</p></td><td><p>深色样式</p></td></tr><tr><td><p>blue_night</p></td><td><p>夜空蓝样式</p></td></tr><tr><td><p>fresh</p></td><td><p>浅色样式</p></td></tr><tr><td><p>light</p></td><td><p>osm清新风格样式</p></td></tr></tbody></table>

可以这样设置  
JS

```js
	var map = new AMap.Map('container', {
        resizeEnable: true,
        mapStyle:'fresh',
        center: [116.408075, 39.950187]
    });
```


也可以这样设置  
JS

```js
	map.setMapStyle("fresh");
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap104.JPG)


#### 显示指定地图内容（地图要素）

<table style="width:100%;border-collapse:collapse;"><tbody><tr><th><p>名称</p></th><th><p>说明</p></th></tr><tr><td><p>bg</p></td><td><p>背景地图</p></td></tr><tr><td><p>point</p></td><td><p>兴趣点</p></td></tr><tr><td><p>road</p></td><td><p>道路</p></td></tr><tr><td><p>building</p></td><td><p>建筑</p></td></tr></tbody></table>


JS

```js
	map.setFeatures("road");//单一种类要素显示
	map.setFeatures(['road','point'])//多个种类要素显示
```


预览（只显示道路要素的地图）

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap105.JPG)


地图控件
---

JavaScript API提供了工具条、比例尺、定位、鹰眼、基本图层切换等常用的控件

<table style="width:100%;border-collapse:collapse;"><tbody><tr><th><p>名称</p></th><th><p>类名</p></th><th><p>简介</p></th></tr><tr><td><p>工具条</p></td><td><p>ToolBar</p></td><td><p>集成了缩放、平移、定位等功能按钮在内的组合控件</p></td></tr><tr><td><p>比例尺</p></td><td><p>Scale</p></td><td><p>展示地图在当前层级和纬度下的比例尺</p></td></tr><tr><td><p>定位</p></td><td><p>Geolocation</p></td><td><p>用来获取和展示用户主机所在的经纬度位置</p></td></tr><tr><td><p>鹰眼</p></td><td><p>OverView</p></td><td><p>在地图右下角显示地图的缩略图</p></td></tr><tr><td><p>类别切换</p></td><td><p>MapType</p></td><td><p>实现默认图层与卫星图、实施交通图层之间切换的控</p></td></tr></tbody></table>


添加控件

JS

```js
	map.plugin(["AMap.ToolBar"],function () {
		map.addControl(new AMap.ToolBar()); // 工具条控件
	});
```


（其他控件添加方式同上）

预览（带有工具条控件的地图）

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap106.JPG)




------
参考来源：[http://lbs.amap.com/](http://lbs.amap.com/ "高德地图开放平台")  
作者：[Yangfan](http://yangfan.ga "http://yangfan.ga")

