---
title: 高德地图 Javascript API 入门（六）
date: 2017-02-28 18:03:02
tags: [GIS,API,JS]
---


高德地图 Javascript API 入门（六）
===


出行规划
---

#### 公交路径

公交换乘服务，提供起、终点公交路线规划服务，整合步行方式

- 初始化

```js
// 加载公交线路插件
AMap.service("AMap.Transfer",function () {
    // 实例化Transfer 
    var transfer=new AMap.Transfer({
        city:"北京", // 必须值，搭乘公交所在城市
        map:map, // 可选值，搜索结果的标注、线路等均会自动添加到此地图上
        panel:"panel", // 可选值，显示搜索列表的容器,
        extensions:"all", // 可选值，详细信息        
        poliy:AMap.TransferPolicy.LEAST_TIME // 驾驶策略：最省时模式
    });
});
```

- 按关键字搜索


```js
// 关键字搜索
transfer.search([{keyword:"北京西站"},{keyword:"天安门"}],function (status,result) {
    window.top.data=result;
});
```

- 按坐标搜索


```js
// 按坐标搜索
transfer.search([116.379028, 39.865042], [116.427281, 39.903719],function (status,result) {
    window.top.data=result;
});
```

预览 

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap601.JPG)


#### 驾车路径

驾车路线规划服务，提供起、终点坐标的驾车导航路线查询功能

- 初始化

```js
// 加载驾车路径插件
AMap.service("AMap.Driving",function () {
	// 实例化Driving    
	var driving=new AMap.Driving({
        map:map, // 用来承载 显示路径
        panel:"panel", // 显示搜索列表的容器
        extensions:"all", // 详细信息
        policy:AMap.DrivingPolicy.REAL_TRAFFIC, // 驾驶策略：合理交通
        showTraffic:true, // 是否显示路况
        province:"晋", // 判断限行
        number:"A88888", // 判断限行
        hideMarkers:false, // 隐藏起点、终点的点标注
        isOutline:true, // 是否显示线路的边框
        outlineColor:"#f00" // 边框颜色
    });
});
```

- 按关键字搜索

```js
	// 按关键字搜索
    driving.search([{keyword:"北京西站",city:"北京"},{keyword:"天安门",city:"北京"}],function (status,result) {
        window.top.data=result;
    });
```

- 按坐标搜索

```js
	// 按坐标搜索
	driving.search([116.379028, 39.865042], [116.427281, 39.903719],function (status,result) {
        window.top.data=result;
    });
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap602.JPG)

	
限行结果  
0 代表限行已规避或未限行，即该路线没有限行路段  
1 代表限行无法规避，即该线路有限行路段

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap603.JPG)


#### 步行路径

步行导航服务，提供起、终点步行路线规划服务

- 初始化

```js
// 加载步行路径插件
AMap.service("AMap.Walking",function () {
    var walking=new AMap.Walking({
        map:map,
        panel:"panel"
    });
});
```

- 按关键字搜索 

```js
walking.search([{keyword:"方恒国际中心A座"},{keyword:"望京站"}],function (status,result) {
	window.top.data=result;
});
```

- 按坐标搜索 

```js
walking.search([116.379028, 39.865042],[116.427281, 39.903719],function (status,result) {
	window.top.data=result;
});
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap604.JPG)




------
参考来源：[http://lbs.amap.com/](http://lbs.amap.com/ "高德地图开放平台")  
作者：[Yangfan](http://yangfan.ga "http://yangfan.ga")

