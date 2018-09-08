---
title: 高德地图 Javascript API 入门（五）
date: 2017-02-26 22:03:02
tags: [GIS,API,JS]
---


高德地图 Javascript API 入门（五）
===


搜索服务
---

#### 搜索服务


<table style="width:100%;border-collapse:collapse;"><tbody><tr><th>名称</th><th>说明</th><th>是否插件</th></tr><tr><td><a href="#m_AMap.Autocomplete" class="" target="">AMap.Autocomplete</a></td><td>输入提示，根据输入关键字提示匹配信息</td><td>是</td></tr><tr><td><a href="#m_AMap.PlaceSearch" class="" target="">AMap.PlaceSearch</a></td><td>地点搜索服务插件，提供某一特定地区的位置查询服务</td><td>是</td></tr><tr><td><a href="#m_AMap.PlaceSearchLayer" class="" target="">AMap.PlaceSearchLayer</a></td><td>麻点图插件，提供海量搜索结果的辅助显示功能</td><td>是</td></tr><tr><td><a href="#m_AMap.DistrictSearch" class="" target="">AMap.DistrictSearch</a></td><td>行政区查询服务，提供行政区相关信息</td><td>是</td></tr><tr><td><a href="#m_AMap.LineSearch" class="" target="">AMap.LineSearch</a></td><td>公交路线服务，提供公交路线相关信息查询服务</td><td>是</td></tr><tr><td><a href="#m_AMap.StationSearch" class="" target="">AMap.StationSearch</a></td><td>公交站点查询服务，提供途经公交线路、站点经纬度等信息</td><td>是</td></tr></tbody></table>


#### 地点搜索插件 AMap.PlaceSearch

- 创建地点查询类的实例

```js
	// 创建地点查询类的实例
	AMap.service("AMap.PlaceSearch",function () {
		var s1=new AMap.PlaceSearch({
			city:"北京市", // 搜索范围的城市
			type:"风景名胜", // 搜索类型
			map:map, // 可选，AMap示例
			panel:"result", // 可选，结果列表的HTML容器id或容器元素
			pageSize:5, // 结果，单页展示结果数
			pageIndex:2, // 结果，页码
			extensions:"all" // 信息，默认值 "base", 详细信息 "all"
		});
	});
```


- 根据关键字搜索

```js
	// 关键字搜索
	s1.search("八达岭",function (status,result) {
		console.log(result);
	});
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap501.JPG)

<div style="margin:30px 0;border-bottom:3px dotted #9c3"></div>

- 根据中心点经纬度、半径以及关键字进行周边查询（周边搜索）

```js
	// 周边搜索
	s1.searchNearBy("餐饮",[116.403322, 39.920255],1000,function (status,result) {
    	console.log(result);
    });
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap502.JPG)

<div style="margin:30px 0;border-bottom:3px dotted #9c3"></div>


- 根据范围和关键词进行范围查询


```js
	var lnglat1=new AMap.LngLat( 116.403322, 39.920255);
	var lnglat2=new AMap.LngLat( 116.389846, 39.891365);
	s1.searchInBounds('酒店',new AMap.Bounds(lnglat1,lnglat2));
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap503.JPG)

<div style="margin:30px 0;border-bottom:3px dotted #9c3"></div>

- 根据POIID 查询POI详细信息

POIID是返回数据（JSON）的一个 id 值

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap504.JPG)

<div style="margin:30px 0;border-bottom:3px dotted #9c3"></div>

获取POI的详细信息

```js
	s1.getDetails("B000A82RP2",function (status,result) {
    	console.log(result.poiList.pois[0].name);
   		window.top.data=result.poiList.pois[0];
    });
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap505.JPG)

<div style="margin:30px 0;border-bottom:3px dotted #9c3"></div>

#### 公交线路搜索插件 AMap.LineSearch

- 创建公交线路查询类的实例

```js
	AMap.service("AMap.LineSearch",function () {
  		var lineSearch=new AMap.LineSearch({
      		city:"太原",
    		extensions:"all"         
    	});
  	});
```

- 按关键字查询公交线路

```js
	lineSearch.search("901",function (status,result) {	
    	if (status==="complete" && result.info==="OK") {
        	console.log(result);
        	window.top.data=result;
        }
    });
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap506.JPG)

<div style="margin:30px 0;border-bottom:3px dotted #9c3"></div>


- 按id查询公交线路


```js
	lineSearch.searchById("140100010458",function (status,result) {	
    	if (status==="complete" && result.info==="OK") {
        	console.log(result);
        	window.top.data=result;
        }
    });
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap507.JPG)

<div style="margin:30px 0;border-bottom:3px dotted #9c3"></div>


#### LineInfo 对象

- 基本信息

<table style="width:100%;border-collapse:collapse;"><tbody><tr><th>属性</th><th>类型</th><th>说明</th></tr><tr><td style="white-space: nowrap;"><code class="inline-code">id</code></td><td><code class="inline-code">String</code></td><td>公交线路id，该id是唯一标识</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">name</code></td><td><code class="inline-code">String</code></td><td>公交线路名称</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">path</code></td><td><code class="inline-code">Array.&lt;</code><a href="/api/javascript-api/reference/core#LngLat" class="" target=""><code class="inline-code">LngLat</code></a><code class="inline-code">&gt;</code></td><td>公交线路经纬度</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">citycode</code></td><td><code class="inline-code">String</code></td><td>公交线路所在城市的城市编码</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">type</code></td><td><code class="inline-code">String</code></td><td>公交类型列表</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">type</code></td><td><code class="inline-code">String</code></td><td>公交类型列表</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">start_stop</code></td><td><code class="inline-code">String</code></td><td>首发站</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">end_stop</code></td><td><code class="inline-code">String</code></td><td>终点站</td></tr></tbody></table>

- 详细信息

<table style="width:100%;border-collapse:collapse;"><tbody><tr><th>属性</th><th>类型</th><th>说明</th></tr><tr><td style="white-space: nowrap;"><code class="inline-code">stime</code></td><td><code class="inline-code">String</code></td><td>首班车时间</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">etime</code></td><td><code class="inline-code">String</code></td><td>末班车时间</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">basic_price</code></td><td><code class="inline-code">String</code></td><td>起步票价，单位：元</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">total_price</code></td><td><code class="inline-code">String</code></td><td>全程票价，单位：元</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">via_stops</code></td><td><code class="inline-code">String</code></td><td>途径站，包括首发站和终点站</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">distance</code></td><td><code class="inline-code">Number</code></td><td>全程距离，单位：千米</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">bounds</code></td><td><a href="http://lbs.amap.com/api/javascript-api/reference/core/#Bounds" class="" target=""><code class="inline-code">Bounds</code></a></td><td>此公交路线的地理范围</td></tr><tr><td style="white-space: nowrap;"><code class="inline-code">company</code></td><td><code class="inline-code">String</code></td><td>所属公交公司</td></tr></tbody></table>




------
参考来源：[http://lbs.amap.com/](http://lbs.amap.com/ "高德地图开放平台")  
作者：[Yangfan](http://yangfan.ga "http://yangfan.ga")

