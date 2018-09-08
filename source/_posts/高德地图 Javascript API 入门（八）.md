---
title: 高德地图 Javascript API 入门（八）
date: 2017-03-07 21:57:02
tags: [GIS,API,JS]
---


高德地图 Javascript API 入门（八）
===

点聚合插件
---

用于地图上加载大量点标记，提高地图浏览性能。点聚合支持用户自定义点标记

#### 加载地图

```js
// 加载地图
var map=new AMap.Map("container",{
    resizeEnable:true,
    center:[116.397428, 39.90923],
    zoom:11
});
```
#### 生成点标记

```js
// 生成点标记
var markers=[];
var marker=null;
for (var i=0;i<100;i++) {
    marker=new AMap.Marker({
        position:[116.39+0.001*Math.random()*i, 39.90-0.001*Math.random()*i],
        icon: "http://amappc.cn-hangzhou.oss-pub.aliyun-inc.com/lbs/static/img/marker.png"
    });
    marker.setMap(map);
    markers.push(marker);
}
```

#### 载入点聚合插件

- 默认样式

```js
map.plugin(["AMap.MarkerClusterer"],function () {
    var cluster=new AMap.MarkerClusterer(map,markers);
});
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap801.JPG)

- 自定义样式


```js
// 聚合样式
var sts=[{
    url:"http://a.amap.com/lbs/static/img/1102-1.png",
    size:new AMap.Size(32,32),
    offset:new AMap.Pixel(-16,-30),
    imageOffset:new AMap.Pixel(0,0)
},{
    url:"http://a.amap.com/lbs/static/img/2.png",
    size:new AMap.Size(32,32),
    offset:new AMap.Pixel(-16,-30)
},{
    url:"http://lbs.amap.com/wp-content/uploads/2014/06/3.png",
    size:new AMap.Size(32,32),
    offset:new AMap.Pixel(-16,-30),
    textColor:"#f00",
    textSize:30
}];
// 载入插件
map.plugin(["AMap.MarkerClusterer"],function () {
    var cluster=new AMap.MarkerClusterer(map,markers,{
        styles:sts
    });
});
```

预览

![image](https://raw.githubusercontent.com/Yangfan2016/PicBed/master/AMap/amap802.JPG)


#### MarkerClustererOptions

<table style="width:100%;border-collapse:collapse;"><tbody><tr><th><p>MarkerClustererOptions</p></th><th><p>类型</p></th><th><p>说明</p></th></tr><tr><td style="white-space: nowrap;"><p><code class="inline-code">gridSize</code></p></td><td><p><code class="inline-code">Number</code></p></td><td><p>聚合计算时网格的像素大小，默认60</p></td></tr><tr><td style="white-space: nowrap;"><p><code class="inline-code">minClusterSize</code></p></td><td><p><code class="inline-code">Number</code></p></td><td><p>聚合的最小数量。默认值为2，即小于2个点则不能成为一个聚合</p></td></tr><tr><td style="white-space: nowrap;"><p><code class="inline-code">maxZoom</code></p></td><td><p><code class="inline-code">Number</code></p></td><td><p>最大的聚合级别，大于该级别就不进行相应的聚合。默认值为18，即小于18级的级别均进行聚合，18及以上级别不进行聚合</p></td></tr><tr><td style="white-space: nowrap;"><p><code class="inline-code">averageCenter</code></p></td><td><p><code class="inline-code">Boolean</code></p></td><td><p>聚合点的图标位置是否是所有聚合内点的中心点。默认为否，即聚合点的图标位置位于聚合内的第一个点处</p></td></tr><tr><td style="white-space: nowrap;"><p><code class="inline-code">styles</code></p></td><td><p><code class="inline-code">Array&lt;Object&gt;</code></p></td><td><p>自定义聚合后的点标记图标的样式，根据数组元素顺序设置1-10,11-100,101-1000…聚合样式</p><p>当用户设置聚合样式少于实际叠加的点数，未设置部分按照系统默认样式显示</p><p>单个图标样式包括以下几个属性：</p><p>1. {string}url：图标显示图片的url地址（必选）</p><p>2. {AMap.Size}size：图标显示图片的大小（必选）</p><p>3. {AMap.Pixel} offset：图标定位在地图上的位置相对于图标左上角的偏移值。默认为(0,0),不偏移（可选）</p><p>4. {AMap.Pixel} imageOffset：图片相对于可视区域的偏移值，此功能的作用等同CSS中的background-position属性。默认为(0,0)，不偏移（可选）</p><p>5. {String} textColor：文字的颜色，默认为"#000000"（可选）</p><p>6. {Number} textSize：文字的大小，默认为10（可选）</p></td></tr><tr><td style="white-space: nowrap;"><p><code class="inline-code">zoomOnClick</code></p></td><td><p><code class="inline-code">Boolean</code></p></td><td><p>点击聚合点时，是否散开，默认值为：true</p></td></tr></tbody></table>

------
参考来源：[http://lbs.amap.com/](http://lbs.amap.com/ "高德地图开放平台")  
作者：[Yangfan](http://yangfan.ga "http://yangfan.ga")

