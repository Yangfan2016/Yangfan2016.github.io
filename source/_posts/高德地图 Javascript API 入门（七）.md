---
title: 高德地图 Javascript API 入门（七）
date: 2017-03-04 21:03:02
tags: [GIS,API,JS]
---


高德地图 Javascript API 入门（七）
===

热力图插件
---

#### 简单例子


```js
// 显示地图
var map=new AMap.Map("container",{
    resizeEnable:true,
    center:[116.397428, 39.90923],
    zoom:11
});
// 坐标点
var points =[
	{"lng":116.191031,"lat":39.988585,"count":100},
    {"lng":116.389275,"lat":39.925818,"count":60},
    {"lng":116.287444,"lat":39.810742,"count":200},
    {"lng":116.481707,"lat":39.940089,"count":30},
    {"lng":116.410588,"lat":39.880172,"count":200},
    {"lng":116.394816,"lat":39.91181,"count":10},
    {"lng":116.416002,"lat":39.952917,"count":150}
];
// 加载热力图插件
map.plugin(["AMap.Heatmap"],function () {
 	var heatmap=new AMap.Heatmap(map,{
    	radius:50
	});
	heatmap.setDataSet({
   		data:points,
    	max:100
   	});
)};
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap701.JPG)


#### 参数

AMap.Heatmap

<table style="width:100%;border-collapse:collapse;"><tbody><tr><th>构造函数</th><th>说明</th></tr><tr><td><code>AMap.Heatmap(<span class="punderline" title="叠加的地图对象，类型为AMap.Map">map:<a href="http://lbs.amap.com/api/javascript-api/reference/map/">Map</a></span>,<span class="punderline" title="参考HeatmapOptions列表中的说明">opts:<a href="#m_HeatmapOptions">HeatmapOptions</a></span>)</code></td><td>构造一个热力图插件对象，map为要叠加热力图的地图对象，opts属性参考HeatmapOptions列表中的说明。</td></tr></tbody></table>

options

<table style="width:100%;border-collapse:collapse;"><tbody><tr><th>HeatmapOptions</th><th>类型</th><th>说明</th></tr><tr><td style="white-space: nowrap;"><code>radius</code></td><td><code>Number</code></td><td>热力图中单个点的半径，默认：30，单位：pixel</td></tr><tr><td style="white-space: nowrap;"><code>gradient</code></td><td><code>Object</code></td><td>热力图的渐变区间，热力图按照设置的颜色及间隔显示热力图，例：<br>{<br>0.4:'rgb(0, 255, 255)',<br>0.65:'rgb(0, 110, 255)',<br>0.85:'rgb(100, 0, 255)',<br>1.0:'rgb(100, 0, 255)'<br>}<br> 其中 key 表示间隔位置，取值范围： [0,1]，value为颜色值。默认：heatmap.js标准配色方案</td></tr><tr><td style="white-space: nowrap;"><code>opacity</code></td><td><code>Array</code></td><td>热力图透明度数组，取值范围[0,1]，0表示完全透明，1表示不透明，默认：[0,1]</td></tr><tr><td style="white-space: nowrap;"><code>zooms</code></td><td><code>Array</code></td><td>支持的缩放级别范围，取值范围[3-18]，默认：[3,18]</td></tr></tbody></table>

------
参考来源：[http://lbs.amap.com/](http://lbs.amap.com/ "高德地图开放平台")  
作者：[Yangfan](http://yangfan.ga "http://yangfan.ga")

