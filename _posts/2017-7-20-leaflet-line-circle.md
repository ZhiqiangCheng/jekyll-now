---
layout: post
title: 使用leaflet绘制圆和线，模拟台风的移动轨迹!
---

leaflet 

### part 1 : 引入 leaflet 和 jquery

为了方便演示 使用 cdn:

```html
    <!-- leaflet css-->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.1.0/dist/leaflet.css" />
    <!-- leaflet js-->
    <script src="https://unpkg.com/leaflet@1.1.0/dist/leaflet.js"></script>
    <!-- jquery js-->
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.0/jquery.min.js"></script>
```

### part 2 : 地图初始化

在HTML中添加用于显示地图的容器
```html
    <div id="mapid" style="width: 100%;" ></div>
```
初始化地图
```js
    //经纬度 为宁波市
    var map = L.map('mapid').setView([29.860634, 121.601529], 4);

    L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token=pk.eyJ1IjoibWFwYm94IiwiYSI6ImNpejY4NXVycTA2emYycXBndHRqcmZ3N3gifQ.rJcFIG214AriISLbB6B5aw', {
        maxZoom: 18,
        id: 'mapbox.satellite'
    }).addTo(map);
```

[example](https://github.com/ZhiqiangCheng/zhiqiangcheng.github.io/tree/master/example)