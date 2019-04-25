---
title: 高德地图 Javascript API 入门（三）
date: 2017-02-22 12:52:44
tags: [GIS,API,JS]
---


高德地图 Javascript API 入门（三）
===

距离测量插件
---

#### 区别

虽然鼠标工具插件也提供距离量测功能，
但是距离量测插件，提供更为丰富的样式设置功能。

#### 加载插件

JS

	
```js
	map.plugin(["AMap.RangingTool"],function () {
		var rangingTool=new AMap.RangingTool(map);
		rangingTool.turnOn(); // 开启量测功能
	});
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap301.JPG)

#### 样式设置

<table style="width:100%;border-collapse:collapse;"><tbody><tr><th><p>RangingToolOptions</p></th><th><p>类型</p></th><th><p>说明</p></th></tr><tr><td><p><code class="inline-code">startMarkerOptions</code></p></td><td><p><code class="inline-code">Object</code></p></td><td><p>设置量测起始点标记属性对象，包括点标记样式、大小等，参考 &nbsp;&nbsp;<a href="/api/javascript-api/reference/overlay#MarkerOptions" class="" target="">MarkerOptions</a> &nbsp;列表</p></td></tr><tr><td><p><code class="inline-code">midMarkerOptions</code></p></td><td><p><code class="inline-code">Object</code></p></td><td><p>设置量测中间点标记属性对象，包括点标记样式、大小等，参考 &nbsp;<a href="/api/javascript-api/reference/overlay#MarkerOptions" class="" target="">MarkerOptions</a> &nbsp;列表</p></td></tr><tr><td><p><code class="inline-code">endMarkerOptions</code></p></td><td><p><code class="inline-code">Object</code></p></td><td><p>设置量测结束点标记属性对象，包括点标记样式、大小等，参考 &nbsp;<a href="/api/javascript-api/reference/overlay#MarkerOptions" class="" target="">MarkerOptions</a> &nbsp;列表</p></td></tr><tr><td><p><code class="inline-code">lineOptions</code></p></td><td><p><code class="inline-code">Object</code></p></td><td><p>设置距离量测线的属性对象，包括线样式、颜色等，参考 &nbsp;<a href="/api/javascript-api/reference/overlay#PolylineOptions" class="" target="">PolylineOptions </a>列表</p></td></tr><tr><td><p><code class="inline-code">tmpLineOptions</code></p></td><td><p><code class="inline-code">Object</code></p></td><td><p>设置距离量测过程中临时量测线的属性对象，包括线样式、颜色等，参考 &nbsp;<a href="/api/javascript-api/reference/overlay#PolylineOptions" class="" target="">PolylineOptions</a> &nbsp;列表</p></td></tr><tr><td><p><code class="inline-code">startLabelText</code></p></td><td><p><code class="inline-code">String</code></p></td><td><p>设置量测起始点标签的文字内容，默认为“起点”</p></td></tr><tr><td><p><code class="inline-code">midLabelText</code></p></td><td><p><code class="inline-code">String</code></p></td><td><p>设置量测中间点处标签的文字内容，默认为当前量测结果值</p></td></tr><tr><td><p><code class="inline-code">endLabelText</code></p></td><td><p><code class="inline-code">String</code></p></td><td><p>设置量测结束点处标签的文字内容，默认为当前量测结果值</p></td></tr><tr><td><p><code class="inline-code">startLabelOffset</code></p></td><td><p><a href="http://lbs.amap.com/api/javascript-api/reference/core/#Pixel" class="" target=""><code class="inline-code">Pixel</code></a>&nbsp;</p></td><td><p>设置量测起始点标签的偏移量。默认值：Pixel(-6, 6)</p></td></tr><tr><td><p><code class="inline-code">midLabelOffset</code></p></td><td><p><a href="http://lbs.amap.com/api/javascript-api/reference/core/#Pixel" class="" target=""><code class="inline-code">Pixel</code></a>&nbsp;</p></td><td><p>设置量测中间点标签的偏移量。默认值：Pixel(-6, 6)</p></td></tr><tr><td><p><code class="inline-code">endLabelOffset</code></p></td><td><p><a href="http://lbs.amap.com/api/javascript-api/reference/core/#Pixel" class="" target=""><code class="inline-code">Pixel</code></a>&nbsp;</p></td><td><p>设置量测结束点标签的偏移量。默认值：Pixel(-6, 6)</p></td></tr></tbody></table>


#### 示例


###### 改变标签文字


JS


```js
	map.plugin(["AMap.RangingTool"],function () {
		var rangingTool=new AMap.RangingTool(map,{
			startLabelText:"START",
			midLabelText:"MID",
			endLabelText:"END"
		});
		rangingTool.turnOn();
	});
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap302.JPG)

	
###### 改变线条样式

JS


```js
	map.plugin(["AMap.RangingTool"],function () {
		var rangingTool=new AMap.RangingTool(map,{
			lineOptions:{
				strokeColor:"#ff3300",
				strokeStyle:"dashed",
				strokeWeight:10,
				strokeOpacity:0.5,
				isOutline:true,
				outlineColor:"#009933",
				showDir:true
			}
		});
		rangingTool.turnOn();
	});
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap303.JPG)



小练习
---

#### 绘制太原市区的大概范围

JS


```js
	// 用坐标拾取器获得坐标
	var lineArr=[
		[112.490931,37.898793],
		[112.553588,37.898793],
		[112.603026,37.899877],
		[112.605086,37.855028],
		[112.605601,37.831169],
		[112.610236,37.824661],
		[112.610236,37.798487],
		[112.602683,37.793739],
		[112.499171,37.793739],
		[112.495051,37.794553],
		[112.500544,37.830762],
		[112.500716,37.843099],
		[112.48973,37.847301],
		[112.489901,37.896897],
		[112.492476,37.8992]
	];
	// 实例化一个Polyline类
	var polyline=new AMap.Polyline({
		path:lineArr,
		strokeColor:"#ff2200",
		strokeWeight:5
	});
	// 添加到地图中
	polyline.setMap(map);
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap304.JPG)





------
参考来源：[http://lbs.amap.com/](http://lbs.amap.com/ "高德地图开放平台")  
作者：[Yangfan](http://yangfan.ga "http://yangfan.ga")

