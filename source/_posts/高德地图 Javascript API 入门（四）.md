---
title: 高德地图 Javascript API 入门（四）
date: 2017-02-22 19:03:02
tags: [GIS,API,JS]
---


高德地图 Javascript API 入门（四）
===


地图覆盖物
---

#### 覆盖物


<table style="width:100%;border-collapse:collapse;"><thead><tr><th class="">类名</th><th class="">说明</th><th>是否插件</th></tr></thead><tbody><tr><td><a href="#Marker">AMap.Marker</a></td><td><p style="margin-top:10px">点标记</p></td><td><p style="margin-top:10px">否</p></td></tr><tr><td><a href="#Icon">AMap.Icon</a></td><td><p style="margin-top:10px">覆盖物&gt;点标记&gt;复杂点标记对象，对普通点标记Marker 的扩展</p></td><td><p style="margin-top:10px">否</p></td></tr><tr><td><a href="#Polyline">AMap.Polyline</a></td><td><p style="margin-top:10px">覆盖物&gt;折线</p></td><td><p style="margin-top:10px">否</p></td></tr><tr><td><a href="#Polygon">AMap.Polygon</a></td><td><p style="margin-top:10px">覆盖物&gt;多边形</p></td><td><p style="margin-top:10px">否</p></td></tr><tr><td><a href="#Circle">AMap.Circle</a></td><td><p style="margin-top:10px">覆盖物&gt;圆</p></td><td><p style="margin-top:10px">否</p></td></tr><tr><td><a href="#GroundImage">AMap.GroundImage</a></td><td><p style="margin-top:10px">图片覆盖物</p></td><td><p style="margin-top:10px">否</p></td></tr><tr><td><a href="#ContextMenu">AMap.ContextMenu</a></td><td><p style="margin-top:10px">地图右键菜单</p></td><td><p style="margin-top:10px">否</p></td></tr></tbody></table>



#### 点标记

JS

```js
	var marker=new AMap.Marker({
		map:map,
		position:new AMap.LngLat(112.736465,37.696495)
	});
```


预览

![iamge](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap401.JPG)


自定义点标记

JS

```js
	var marker=new AMap.Marker({
		map:map,
		position:new AMap.LngLat(112.736465,37.696495),
		icon:new AMap.Icon({
			image:"https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=2803620233,1906638381&fm=23&gp=0.jpg",
			size:[72,72],
			imageSize:[36,36]
		}),
		draggable:true,
		raiseOnDrag:true,
		shape:new AMap.MarkerShape({
			type:"circle",
			coords:[112.736465,37.696495,100]
		}),
		label:{
			content:"label",
			offset:new AMap.Pixel(0,-36)
		}
	});
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap402.JPG)


##### 多边形


JS

```js
	var lineArr=[
		[112.49157,37.897392],
		[112.602806,37.898747],
		[112.608643,37.797355],
		[112.49775,37.79627]
	];
	var polygon=new AMap.Polygon({
		map:map,
		path:lineArr
	});
```



预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap404.JPG)


#### 右键菜单

JS

```js
	var contextmenu=new AMap.ContextMenu();
	var pos=[];
	// 添加右键菜单内容项
	contextmenu.addItem("放大",function () {
		map.zoomIn();
	},0);
	contextmenu.addItem("缩小",function () {
		map.zoomOut();
	},1);
	contextmenu.addItem("添加点标记",function () {
		var marker=new AMap.Marker({
			map:map,
			position:pos
		});
	},2);
	// 监听鼠标右击事件
	map.on("rightclick",function (e) {
		contextmenu.open(map,e.lnglat);
		pos=e.lnglat;
	});
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap405.JPG)





信息窗体
---

#### 信息窗体

JS

```js
	var infowindow=new AMap.InfoWindow({
		isCustom:false,
		content:"<h3>Hello,Yuanping</h3>",
		offset:new AMap.Pixel(0,-36),
		showShadow:true,
		closeWhenClickMap:true,
		autoMove:true
	});
	infowindow.open(map,new AMap.LngLat(112.736465,38.696495));
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap406.JPG)



小练习
---

#### 鼠标划过山西大剧院时，弹出信息窗体

JS

```js
	// 坐标
	var lineArr=[
		[112.532802,37.808395],
		[112.533049,37.808395],
		[112.533124,37.808476],
		[112.533521,37.808459],
		[112.533558,37.808391],
		[112.533832,37.808391],
		[112.533848,37.80792],
		[112.534159,37.807959],
		[112.534159,37.80748],
		[112.533826,37.807514],
		[112.533832,37.807179],
		[112.533966,37.806848],
		[112.533376,37.806683],
		[112.533054,37.806687],
		[112.532684,37.806878],
		[112.53278,37.807191],
		[112.532796,37.80745],
		[112.532013,37.807285],
		[112.532019,37.808213],
		[112.532796,37.808018],
		[112.532818,37.808412]
	];
	// 实例化Polygon类
	var polygon=new AMap.Polygon({
		map:map,
		path:lineArr
	});
	// 适应窗口
	map.setFitView();
	// 实例化信息窗体类
	var infowindow=new AMap.InfoWindow({
		content:"<h3>太原市</h3><p>山西大剧院</p>",
		closeWhenClickMap:true
	});
	// 监听鼠标移入、移除事件
	polygon.on("mouseover",function (e) {
		infowindow.open(map,map.getCenter());
	}).on("mouseout",function () {
		infowindow.close();
	});
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap407.JPG)




------
参考来源：[http://lbs.amap.com/](http://lbs.amap.com/ "高德地图开放平台")  
作者：[Yangfan](http://yangfan.ga "http://yangfan.ga")

