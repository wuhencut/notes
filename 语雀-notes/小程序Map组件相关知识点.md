# 小程序Map组件相关知识点

<a name="dcd3d0b7"></a>
# 
<a name="9aa841dd"></a>
### 需求背景：类似美团商家展示当前位置和目的地位置的路线规划地图。

<br />

<a name="8055dded"></a>
### 长这样：

<br />

<a name="36a55c15"></a>
### ![image.png](https://cdn.nlark.com/yuque/0/2020/png/126454/1594799041240-c75f0ef9-a59c-4782-85dc-ceb2a3c593c5.png)

<br />

<a name="b009a717"></a>
### 展示大致路线图，点击打开本地地图app进行导航

<br />

<a name="d53505e4"></a>
### 知识点：


- map组价 小程序自带，会加载腾讯地图
- 属性：show-location ：展示当前定位位置，展示一个小蓝标
- polyline 展示点与点之间的划线，本功能主要由这个实现
- markers 展示画布上的点，值：latitude,longitude iconPath可设置图片，图例的终点和起点就是这么设置的
- map组件必须设置id，需要给map组件创建 画布context wx.createMapConetext("map_id")
- latitude longitude当前经纬度


<br />

<a name="api"></a>
### api


- mapCtx.moveToLocation() 中心移动到当前位置。
- wx.createMapContext() 创建map画布，所有的插件和polyline生效前提是这个指令
- wx.getLocation() 获取当前定位的位置返回经纬度，用于复制给起点
- mapCtx.includePoints({padding: [50, 50, 50, 50], points: markers}) 将起点和终点缩略到可视范围内


<br />

<a name="3f29e1d5"></a>
### 步骤：

<br />1、当然是创建小程序，获得小程序appid<br />2、打开腾讯地图api文档 申请key，设置成小程序，把你小程序的key写到设置里，生成key<br />3、外部环境准备就绪，准备写代码。map会写，不管，api的调用<br />

- 获取当前定位 赋值给当前经纬度,还有起点位置 wx.getLocation
- 从后端获取要去的终点位置，把和起点一起赋值给markers
- 请求腾讯地图api，获取返回的polyline
- 赋值给data里的polyline，完事儿 看代码


<br />

<a name="06e004ef"></a>
### 代码


- distance ：两点路程



```html
<!--pages/map/map.wxml-->
<view class="container">
  <map bindtap="openRoute"
    longitude="{{currLocation.longitude}}"
    latitude="{{currLocation.latitude}}"
    markers="{{markers}}"
    polyline="{{polyline}}" id="myMap"></map>
</view>
```


```javascript
// pages/map/map.js
Page({

  /**
   * 页面的初始数据
   */
  data: {
    polyline: [],
    currLocation: {
      latitude: "",
      longitude: ""
    },
    markers: []
  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    this.mapCtx = wx.createMapContext('myMap')
    this.getLocation();
  },
  getLocation: function () {
    let t = this;
    // this.mapCtx.moveToLocation();
    wx.getLocation({
      type: "gcj02",
      success: function (res) {
        console.log(res);
        t.setData({
          currLocation: {
            longitude: res.longitude,
            latitude: res.latitude
          },
          markers: [{
            id: 1,
            longitude: res.longitude,
            latitude: res.latitude,
            iconPath: "../../images/start.png",
            width: 20,
            height: 30
          },{
            id: 1,
            longitude: res.longitude + 0.1,
            latitude: res.latitude + 0.1,
            iconPath: "../../images/finish.png",
            width: 20,
            height: 30
          }]
        })
        t.mapCtx.includePoints({
          padding: [60, 60, 60, 60],
          points: t.data.markers
        })
        t.getRoute();
      }
    })
  },
  // 获取路线规划
  getRoute: function () {
    let t = this;
    wx.request({
      url: 'https://apis.map.qq.com/ws/direction/v1/driving/?from=' + this.data.currLocation.latitude +  ',' + this.data.currLocation.longitude + '&to=' + this.data.markers[1].latitude +  ',' + this.data.markers[1].longitude + '&output=json&callback=cb&key=VF7BZ-DC6CP-W4SDV-LQMGJ-AFFKJ-TXB4H',
      success: function (res) {
        console.log(res.data.result.routes[0])
        let coors = res.data.result.routes[0].polyline;
        wx.showToast({
          title: res.data.result.routes[0].distance + "米",
          icon: "none"
        })
        for (var i = 2; i < coors.length; i++) {
          coors[i] = coors[i - 2] + coors[i] / 1000000
        }
        //              console.log(coors)
        //划线 ]
        var b = [];
        for (var i = 0; i < coors.length; i = i + 2) {
          b[i / 2] = {
            latitude: coors[i],
            longitude: coors[i + 1]
          };
          // console.log(b[i / 2])
        }

        t.setData({
          polyline: [{
            points: b,
            color: "#1296db",
            width: 6
          }],
        })
        
      }
    })
  },
  openRoute: function(){
    let t = this;
    wx.openLocation({
      latitude: t.data.markers[1].latitude,
      longitude: t.data.markers[1].longitude,
    })
  }
})
```


> 由于高德api也是返回类似的polyline数据，也有打车费用和距离，所以都一样，就用腾讯的，万一微信不让用高德就GG


<br />

<a name="d57e4be7"></a>
### Polyline返回的"数据异常"问题，以及处理

<br />

<a name="02c6bc64"></a>
### ![image.png](https://cdn.nlark.com/yuque/0/2020/png/126454/1594800678857-fa7cbf09-571b-4899-9f1f-254ad8c07237.png)


> 看图，是不是发现，除了index  0 1是正常的经纬度外，其他的数据都有病？
> 对的，都有病，其他数据没有直接返回经纬度，而是返回了位置变化后对第一个经纬度的变化。



> 也就是说，index 2 相对于index 0 挪动了 -174 / 1000000 维度的距离，
> 一次类推，index 3 相对于 index 1 挪动了 85 / 1000000 经度的距离。
> 那么，可以自己算出所有的经纬度



```javascript
let coors = res.data.result.routes[0].polyline;
for (var i = 2; i < coors.length; i++) {
  coors[i] = coors[i - 2] + coors[i] / 1000000
}
```


> 都是逗号分隔的，怎么办？找规律。
> 每两个就是经纬度。
> 搞



```javascript
var b = [];
for (var i = 0; i < coors.length; i = i + 2) {
  b[i / 2] = {
    latitude: coors[i],
    longitude: coors[i + 1]
  };
  // console.log(b[i / 2])
}
```

<br />真·完事儿
