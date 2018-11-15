---
title: 高德地图 Javascript API 入门（二）
date: 2017-02-21 13:53:12 
tags: [GIS,API,JS]
---


高德地图 Javascript API 入门（二）
===


鼠标工具插件
---

#### 测量距离

JS

```js
	map.plugin(["AMap.MouseTool"],function () {
		var mouseTool=new AMap.MouseTool(map);
		mouseTool.rule();
	});
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap201.JPG)


#### 测量面积

JS

```js
	map.plugin(["AMap.MouseTool"],function () {
		var mouseTool=new AMap.MouseTool(map);
		mouseTool.measureArea();
	});
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap202.JPG)

#### 绘制点标注

JS

```js
	map.plugin(["AMap.MouseTool"],function () {
		var mouseTool=new AMap.MouseTool(map);
		mouseTool.marker();
	});
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap203.JPG)


#### 绘制折线

JS

```js
	map.plugin(["AMap.MouseTool"],function () {
		var mouseTool=new AMap.MouseTool(map);
		mouseTool.polyline();
	});
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap204.JPG)


#### 绘制多边形

JS

```js
	map.plugin(["AMap.MouseTool"],function () {
		var mouseTool=new AMap.MouseTool(map);
		mouseTool.polygon();
	});
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap205.JPG)


#### 绘制矩形

JS

```js
	map.plugin(["AMap.MouseTool"],function () {
		var mouseTool=new AMap.MouseTool(map);
		mouseTool.rectangle();
	});
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap206.JPG)


#### 绘制圆

JS

```js
	map.plugin(["AMap.MouseTool"],function () {
		var mouseTool=new AMap.MouseTool(map);
		mouseTool.circle();
	});
```


预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap207.JPG)

#### 全部功能

<table style="width:100%;border-collapse:collapse;"><tbody><tr><th style="white-space:nowrap;"><p>方法</p></th><th style="white-space:nowrap;"><p>返回值</p></th><th><p>说明</p></th></tr><tr><td style="white-space:nowrap;"><p><code class="inline-code">marker( options：</code><a href="/api/javascript-api/reference/overlay#MarkerOptions" class="" target=""><code class="inline-code">MarkerOptions</code></a><code class="inline-code">)</code></p></td><td style="white-space:nowrap;"><p><br></p><p>&nbsp;</p></td><td><p>开启鼠标画点标注模式。鼠标在地图上单击绘制点标注，标注样式参考MarkerOptions设置</p></td></tr><tr><td style="white-space:nowrap;"><p><code class="inline-code">polyline( options：</code><a href="/api/javascript-api/reference/overlay#PolylineOptions" class="" target=""><code class="inline-code">PolylineOptions</code></a><code class="inline-code">)</code></p></td><td style="white-space:nowrap;"><p><br></p></td><td><p>开启鼠标画折线模式。鼠标在地图上点击绘制折线，鼠标左键双击或右键单击结束绘制，折线样式参考PolylineOptions设置</p></td></tr><tr><td style="white-space:nowrap;"><p><code class="inline-code">polygon( options：</code><a href="/api/javascript-api/reference/overlay#PolygonOptions" class="" target=""><code class="inline-code">PolygonOptions</code></a><code class="inline-code">)</code></p></td><td style="white-space:nowrap;"><p><br></p></td><td><p>开启鼠标画多边形模式。鼠标在地图上单击开始绘制多边形，鼠标左键双击或右键单击结束当前多边形的绘制，多边形样式参考PolygonOptions设置</p></td></tr><tr><td style="white-space:nowrap;"><p><code class="inline-code">rectangle( options：</code><a href="/api/javascript-api/reference/overlay#PolygonOptions" class="" target=""><code class="inline-code">PolygonOptions</code></a><code class="inline-code">)</code></p></td><td style="white-space:nowrap;"><p><br></p></td><td><p>开启鼠标画矩形模式。鼠标在地图上拉框即可绘制相应的矩形。矩形样式参考PolygonOptions设置</p></td></tr><tr><td style="white-space:nowrap;"><p><code class="inline-code">circle( options：</code><a href="/api/javascript-api/reference/overlay#CircleOptions" class="" target=""><code class="inline-code">CircleOptions</code></a><code class="inline-code">)</code></p></td><td style="white-space:nowrap;"><p><br></p></td><td><p>开启鼠标画圆模式。鼠标在地图上拖动绘制相应的圆形。圆形样式参考CircleOptions设置</p></td></tr><tr><td style="white-space:nowrap;"><p><code class="inline-code">rule( options：</code><a href="/api/javascript-api/reference/overlay#PolylineOptions" class="" target=""><code class="inline-code">PolylineOptions</code></a><code class="inline-code">)</code></p></td><td style="white-space:nowrap;"><p><br></p></td><td><p>开启距离量测模式。鼠标在地图上单击绘制量测节点，并计算显示两两节点之间的距离，鼠标左键双击或右键单击结束当前量测操作。量测线样式参考 <a href="/api/javascript-api/reference/overlay#PolylineOptions" class="" target="">PolylineOptions</a> 设置</p><p>注：不能同时使用rule方法和RangTool插件进行距离量测</p></td></tr><tr><td style="white-space:nowrap;"><p><code class="inline-code">measureArea( options：</code><a href="/api/javascript-api/reference/overlay#PolygonOptions" class="" target=""><code class="inline-code">PolygonOptions</code></a><code class="inline-code">)</code></p></td><td style="white-space:nowrap;"><p><br></p></td><td><p>开启面积量测模式。鼠标在地图上单击绘制量测区域，鼠标左键双击或右键单击结束当前量测操作，并显示本次量测结果。量测面样式参考PolygonOptions设置</p></td></tr><tr><td style="white-space:nowrap;"><p><code class="inline-code">rectZoomIn( options：</code><a href="/api/javascript-api/reference/overlay#PolygonOptions" class="" target=""><code class="inline-code">PolygonOptions</code></a><code class="inline-code">)</code></p></td><td style="white-space:nowrap;"><p><br></p></td><td><p>开启鼠标拉框放大模式。鼠标可在地图上拉框放大地图。矩形框样式参考PolygonOptions设置</p></td></tr><tr><td style="white-space:nowrap;"><p><code class="inline-code">rectZoomOut( options：</code><a href="/api/javascript-api/reference/overlay#PolygonOptions" class="" target=""><code class="inline-code">PolygonOptions</code></a><code class="inline-code">)</code></p></td><td style="white-space:nowrap;"><p><br></p></td><td><p>开启鼠标拉框缩小模式。鼠标可在地图上拉框缩小地图。矩形框样式参考PolygonOptions设置</p></td></tr><tr><td style="white-space:nowrap;"><p><code class="inline-code">close( Boolean)</code></p></td><td style="white-space:nowrap;"><p><br></p></td><td><p>关闭当前鼠标操作。参数arg设为true时，鼠标操作关闭的同时清除地图上绘制的所有覆盖物对象；设为false时，保留所绘制的覆盖物对象。默认为false</p></td></tr></tbody></table>


#### 自定义覆盖物样式

以折线为例 Polyline

JS

```js
	map.plugin(["AMap.MouseTool"],function () {
		var mouseTool=new AMap.MouseTool(map);
		mouseTool.polyline({
			strokeColor:"#f50", // 线条颜色，十六进制
			strokeOpacity:0.5, // 线条透明度
			strokeWeight:10, // 线条宽度
			strokeStyle:"dashed" // 线条样式 solid || dashed
		});
	});
```
	

更多详细参数参考 [http://lbs.amap.com/api/javascript-api/reference/overlay#PolylineOptions](http://lbs.amap.com/api/javascript-api/reference/overlay#PolylineOptions "Polyline类")

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap208.JPG)




------
参考来源：[http://lbs.amap.com/](http://lbs.amap.com/ "高德地图开放平台")  
作者：[Yangfan](http://yangfan.ga "http://yangfan.ga")

