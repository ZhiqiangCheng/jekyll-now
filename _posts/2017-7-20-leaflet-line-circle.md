---
layout: post
title: 使用leaflet绘制圆和线，模拟台风的移动轨迹!
---

leaflet 

### part 1 : 引入 leaflet 和 jquery

使用 cdn:

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

### part3 : 获取数据并绘制点和线 

```js
function getTyphoonPath(map,timeout) {
            var that=this;
            var doubleLatLng = [];//用于保存两个坐标的经纬度
            //获取数据
            $.getJSON("201509.json",function (res) {
                //遍历数据
                $.each(res.data, function (j, val) {

                    //每个点的经纬度
                    var latlng = L.latLng(val.latitude,
                        val.longitude);
                    //给每个点配置样式
                    var markerOpts = {
                        radius: 4,
                        fillColor: that.getDotColor(val.wind_power),
                        fillOpacity: 1,
                        stroke: true,
                        color: 'gray',
                        weight: 2,
                        clickable: true
                    };

                    window.setTimeout(function () {
                        if (doubleLatLng.length > 1) {
                            doubleLatLng.shift();
                        }
                        doubleLatLng.push(latlng);
                        //台风线路
                        var typhoonLine = L.polyline(doubleLatLng, {color: '#0A7BCC', weight: 2, opacity: 0.8}).addTo(map).bringToBack();
                        //台风点
                        var typhoonMarker = L.circleMarker(latlng, markerOpts).addTo(map);

                        //台风风圈
                        var seven_wind = L.circle(latlng, val.seven_wind * 1000, {
                            fillColor: 'orange',
                            fillOpacity: 0.03,
                            stroke: false
                        }).addTo(map).bringToBack();
                    },j*timeout);

                });
            })
        };
        //获取等级颜色
        function getDotColor (val) {
          return val > 15 ? "#FF0000" : val > 13 ? "#FA83F6" : val >11 ? "#FAAB2A" : val >9 ? "#F8F92A" : val > 7 ? "#588AF6" : "#51FB52";
        };

```
调用

```js
    // 100 是延时 i*100 
    getTyphoonPath(map,100);
```

### part4 : 效果

![_config.yml]({{ site.baseurl }}/images/leaflet-typhoon.png)

[example](https://github.com/ZhiqiangCheng/zhiqiangcheng.github.io/tree/master/example)