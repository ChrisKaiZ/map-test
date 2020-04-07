---
layout: default
---
# 地图接口说明
##  1.mapControl
> 地图操作
### 1.1 3d.zoomToPoint
- 功能：将镜头聚焦到某点位
- 参数：
	- action："3d.zoomToPoint"
	- params:
		- lon：点位经度
		- lat：点位纬度
		- z：点位高度
- 返回：无
- 示例：
``` javascript
var ptParams = {lon:114.123456,lat:42.123456}
Cross.call("gisiframe", "mapControl", {
    action: '3d.zoomToPoint',
    params: ptParams 
});
```
### 1.2 3d.trajContorl
- 功能：控制轨迹线的生成，清除，播放，暂停，加速和减速
- 参数：
	- action："3d.trajContorl"
	- params：
		- action：轨迹线操作，值列表如下：
			- start：播放
			- stop：暂停
			- fast：加速
			- slow：减速
			- destroy：清除
			- create：生成
		- data：轨迹线数据，action值为生成时必填
			- 数据格式：[[lon:114.123456, lat:42.123456, time: "2019-03-15 19:43:36"], ....]
		- height：轨迹线高度，默认为0，可不传
		- timeStamp：轨迹线动画播放步长，默认为60，可不传
- 返回：无
- 示例：
``` javascript
var trajParams = {
	action:"create",
	data:[
			[lon:114.123456, lat:42.123456, time: "2019-03-15 19:43:36"],
			[lon:114.223456, lat:42.223456, time: "2019-03-15 19:44:36"]
		 ],
	height:0,
	timeStamp:60		
}
Cross.call("gisiframe", "mapControl", {
    action: '3d.trajContorl',
    params: trajParams 
});
```
### 1.3 3d.loadClusterLayer
- 功能：聚合图层加载
- 参数：
	- action："3d.loadClusterLayer"
	- params：
		- layerid：图层id
		- data：点位数据
		- iconUrl：真实点位图标
		- clusterIconUrl：聚合点位图标
		- width：真实点位图标宽度
		- height：真实点位图标高度
- 返回：无
- 示例：
``` javascript

```
### 1.4 3d.loadResToMap
- 功能：点位加载
- 参数：
	- action："3d.loadResToMap"
	- params：
		- layerid：图层名
		- datas：点位数据，数组形式，数组内元素key值如下
			- id：点位id，不可重复
			- coordinate：[经度，纬度]，数组形式，数字型格式
			- iconUrl：点位图标路径
			- width：点位图标宽度
			- height：点位图标高度
			- attributes：点位属性值，用于点击后的弹窗
		- z：图层高度
		- clickAnimation：点击是否有跳动动画。true：有，false：无。默认为无
		- zoomToLayer：图层加载后视角是否聚焦到该图层。true：是，false：否。默认为否
- 返回：无
- 示例：
```javascript
var resParams = {
    layerid:layerName,
    z:150,
    datas:[],
    noCenter:true,
    zoomToLayer:true,
    clickAnimation:true
};
//处理数据
$.each(data,function(index,ele){
    var curData = {
        id:ele.id,
        coordinate:[ele.lon,ele.lat],
        iconUrl:"",
        width:128,
        height:435,
        attributes:ele
    };
    resParams.datas.push(curData);
});
Cross.call("gisiframe", "mapControl", {
     action: "3d.loadResToMap",
     params: resParams
});
```
### 1.1 3d.loadHeatMap
- 功能：热力图加载
- 参数：
	- action: "3d.loadHeatMap"
	- params:
		 - data：包含 x,y,value的对象集合，value为0~1之间的数值
		 - style：每个value值对应的颜色值rgb
- 返回：无
- 示例：
```javascript
//参数定义
var htmap_pramas= {
	        data: [{
	                "x": 104.0818103980392,
	                "y": 30.63734106182757,
	                value: 0.8
	            }, {
	                "x": 104.0772875185593,
	                "y": 30.63402428354232,
	                value: 0.2
	            }
	        ],
	        style: {
	            0.1: "rgb(15, 251 ,58)",
	            0.2: "rgb(191 ,241 ,15)",
	            0.3: "rgb(175, 216, 34 )",
	            0.4: "rgb(184, 216, 75)",
	            0.5: "rgb(189, 212 ,108)",
	            0.6: "rgb(242 ,239 ,148)",
	            0.7: "rgb(241, 236, 88 )",
	            0.8: "rgb(245, 238, 56)",
	            0.9: "rgb(233, 67 ,23)"
	        }
	    }
//方法调用
//gisiframe为引用gis页面的iframe的id
Cross.call("gisiframe", "mapControl", {
    action: '3d.loadHeatMap',
    params: htmap_pramas
});
```
### 1.2 3d.loadEventMarkers
- 功能：添加各种事件
- 参数：
	- action："3d.loadEventMarkers"
	- params： 
		- datas：事件信息，包括id，coordinate，style和label属性的对象集合
		-  type：类型
		-  z：高度
		
 | 序号 | style | 说明 |
 | ----- | ---- | ---- |
 | 1 | yellow | 内置黄色图标 |
 | 2 | red | 内置红色图标 |

- 返回：无
- 示例：
```js
//事件传入参数
var sj_data = {
    datas: [{
            "id": 1,
            "coordinate": [104.0818103980392, 30.63734106182757],
            "style": "red",
            "label": "事件1"
        }, {
            "id": 2,
            "coordinate": [104.0772875185593, 30.63402428354232],
            "style": "yellow",
            "label": "事件2"
        }
    ],
    "type": "0",
    "z": 100
}
//方法调用
Cross.call("gisiframe", "mapControl", {
    action: '3d.loadEventMarkers',
    params: {
        layerid: "eventlayer",
        data: sj_data
    }
});
```
### 1.3 3d.initPop
- 功能：初始化指定图层的pop框
- 参数：
	- action："3d.initPop"
	- params：传入参数
		- layerid：图层id,"renderZone":3d.renderZones;"zoneRenderPoint":3d.renderNumber;"zoneRenderLabel":3d.renderLabel
		- tpl：模板名称
		- data：填充pop模板的json对象，不同的模板传入的值不同
		- offset：xy的偏移两数组
		- direction：pop框固定的位置，上下左右
		- width：宽度
		- height：高度
- 返回：无
- 示例：
```js
Cross.call("gisiframe", "mapControl", {
    action: '3d.initPop',
    params: {
	 layerid:'eventlayer',
        tpl: 'template1',
        data: {
            'wtly': 'qweqwe',
            'wtlx': '四川'
        },
        offset: [1072, 953],
        direction: 'right',
        width: 2504,
        height: 1907
    }
})
```
### 1.4 3d.loadResToMap
- 功能：资源上图
- 参数：
	- action："3d.loadResToMap"
	- params:
		- layerid：图层标识
		- datas：资源对象集合
			- id：资源标识
			- coordinate：经纬度，[104.06920721130878, 30.655912504463668]
			- iconUrl：图标地址
			- width:  图标宽度
			- height: 图标高度
			- label: label内容
			- labelConfig
				- fillColor:字体颜色，[1.0, 1.0, 1.0, 0.8]
				- showBackground：是否显示背景
				- backgroundColor：背景色，[0.4, 0.4, 0.7, 0.3]
				- offset:label标签在x和y方向上的偏移量，[0, -230]
			- attributes: {}传入的资源属性,可用于弹窗信息
		- z：离地高度
		- noCenter: 为true时不重定向到中心
- 返回：无
- 示例：
```js
Cross.call("gisiframe", "mapControl", {
	action: "3d.loadResToMap",
	params: {
		layerid:"redLayer",
		z:100,
		datas:[{
			id:"1",
			coordinate:[104.06920721130878, 30.655912504463668],
			iconUrl:"img/red.png",
			content:""
		}]
	}
})
```
### 1.5 3d.setOverviewVisible
- 功能：控制鹰眼显示隐藏
- 参数：
	- action："3d.setOverviewVisible"
	- params:
		- flag：true实现，false隐藏
- 返回：无
- 示例：
```javascript
Cross.call("gisiframe", "mapControl", {
  	action: "3d.setOverviewVisible",
	params:{
		flag:false
	}
});
```
### 1.6 3d.destroyLayer
- 功能：移除图层
- 参数：
	- action："3d.destroyLayer"
	- params:
		- layerid：图层标识
- 返回：无
- 示例：
```javascript
   Cross.call("gisiframe", "mapControl", {
       action: "3d.destroyLayer",
		params:{
			layerid: "heatmaplayer"
		}
   })
```
### 1.7 3d.monitorLevelChange
- 功能：开启图层层级变化监听
- 参数：
	- action："3d.monitorLevelChange"
- 返回：空
- 示例：
```javascript
//调用
Cross.call("gisiframe", "mapControl", {
    action: "3d.monitorLevelChange"
});

//接收返回
window.addEventListener('message', function (e) {
    var data = e.data;
    if (data.action == "mapControl.mapLevelChange") {
		mapLevel = data.result;
		//此处添加你需要的操作
    }
}, false);
```
## 2 layerContrl
> 图层操作
### 2.1 3d.setLayerVisible
- 功能：设置指定图层显示/隐藏
- 参数：
	- action："3d.setLayerVisible"
	- params:
		- layerid：图层标识
		- flag：是否显示，true：显示，false：隐藏
- 返回：无
- 示例：
```js
Cross.call("gisiframe","layerControl",{
	action:"3d.setLayerVisible",
	params:{
	   layerid:"redLayer",
	   flag:true
	}
})
```

### 2.2 3d.getLayers
- 功能：返回所有业务图层列表
- 参数：
	- action："3d.getLayers"
- 返回：业务图层名称列表
- 示例：
```javascript
//调用
Cross.call("gisiframe", "layerControl", {
    action: "3d.getLayers"
});

//接收返回
window.addEventListener('message', function (e) {
    var data = e.data;
    if (data.action == "3d.onload") {
		//此处添加你需要的操作
    }
	else if(data.action == "layerControl.getLayers"){
		//此处添加你需要的操作
		alert(data.result);
	}
}, false);
```
## 3 zoneRender
> 区域统计渲染
### 3.1 3d.renderZones
- 功能：根据统计值渲染区域
- 参数：
	- action："3d.renderZones"
	- params:
		- zoneDatas
			- name：区域名称
			- number：统计对象的数值，区域统计根据统计值排名，1~22，根据此值来做颜色区分
			- attributes:{} 区域属性，可用于弹窗
		- zoneStyle（建议使用默认，无需设置）
			- zoneHeight：高度，默认为0
			- zoneOutlineColor：轮廓线颜色rgba，默认：[0, 255, 255, 1.0]
```js
//调用
var params= {
    zoneDatas: [{
            name: "武侯区",
            number: 232
        }, {
            name: "锦江区",
            number: 233
        }],
    zoneStyle:{
		zoneHeight:0,
		zoneOutlineColor:[0, 255, 255, 1.0]
	}     
 };
 Cross.call("gisiframe", "zoneRender", {
            action: "3d.renderZones",
            params: params
        });
```
### 3.2 3d.renderNumber
- 功能：显示区域统计的数值
- 参数：
	- action："3d.renderNumber"
	- params
		- zoneDatas
			- name：区域名称
			- number：统计对象的数值
		- style（建议使用默认，无需配置）
			- fillColor：字体颜色
			- showBackground：是否显示背景
			- backgroundColor：背景色
			- offset：xy偏移量，[x,y]
- 返回：无
- 示例：
```js
var params= {
    zoneDatas: [{
            name: "武侯区",
            value: 1,
            number: 233
        }, {
            name: "锦江区",
            value: 2,
            number: 233
        }],
    style:{
		fillColor:[220.0 / 256.0, 173.0 / 256.0, 47.0 / 256.0, 1.0],
		showBackground:true,
		backgroundColor:[0.4, 0.4, 0.7, 0.3],
		"offset": [0, -130]
	}     
 };
 Cross.call("gisiframe", "zoneRender", {
            action: "3d.renderNumber",
            params: params
        });
```
### 3.3 3d.renderLabel
- 功能：显示区域名称
- 参数：
	- action："3d.renderLabel"
	- params:
		- zoneDatas
			- name：区域名称
		- style（建议使用默认，无需配置）
			- fillColor：字体颜色
			- showBackground：是否显示背景
			- backgroundColor：背景色
			- offset：xy偏移量，[x,y]
- 返回：无
- 示例：
```js
var params= {
    zoneDatas: [{
            name: "武侯区",
            value: 1,
            number: 233
        }, {
            name: "锦江区",
            value: 2,
            number: 233
        }],
    style:{
		fillColor:[220.0 / 256.0, 173.0 / 256.0, 47.0 / 256.0, 1.0],
		showBackground:true,
		backgroundColor:[0.4, 0.4, 0.7, 0.3],
		"offset": [0, -130]
	}     
 };
 Cross.call("gisiframe", "zoneRender", {
            action: "3d.renderLabel",
            params: params
        });
```
### 3.4  3d.removeRenderZones
- 功能：移除区域统计渲染层
- 参数：
	- action："3d.removeRenderZones"
- 返回：无
- 示例：
```js
Cross.call("gisiframe", "zoneRender", {
    action: "3d.removeRenderZones"
});
```
### 3.5  3d.removeRenderNumber
- 功能：移除区域统计统计值气泡层
- 参数：
	- action："3d.removeRenderNumber"
- 返回：无
- 示例：
```js
Cross.call("gisiframe", "zoneRender", {
    action: "3d.removeRenderNumber"
});
```
### 3.6 3d.removeRenderLabel
- 功能：移除区域名称层
- 参数：
	- action："3d.removeRenderLabel"
- 返回：无
- 示例：
```js
Cross.call("gisiframe", "zoneRender", {
    action: "3d.removeRenderLabel"
});
```
### 3.6 3d.loadHistogram
- 功能：在区域上添加柱状图
- 参数：
	- action："3d.loadHistogram"
	- params: 
		- style：【optional】样式设置，如无特殊需要，可不用设置此项
			- width：宽度
			- height：高度
			- strokeStyle：边框颜色
			- radiusX：椭圆长半径
			- radiusY：椭圆短半径
			- infoBoxH：消息框高度
			- fontSize：//字体大小
			- infoBoxBg：// 消息框背景
			- infoBoxFontSize：信息框字体大小
			- infoBoxAndEllSpace：消息框与圆柱体间隙
			- infoBoxFontSpace：消息框文字间距
			- infoRhombColor：消息框小圆点颜色
		- datas：【Required 】数据集合
			- 集合中单个对象参数结构
				- id:柱图对象id
				- coordinate：柱图落点坐标
				- data:[],统计数值集合
				- color：
				- fontColor:[]
				- infoBoxC:[]
				- infoBoxFontColor:[]
- 返回：无
- 示例：
```js
var params = {};
params.datas = [{
        id: 1,
        coordinate: [104.06920721130878, 30.655912504463668],
        data: [20, 30],
        color: ['#f00', '#ff0', '#0f0'],
        fontColor: ['#ff0', '#0f9', '#f00'],
        infoBoxC: ['市场主体总数', '312312312'],
        infoBoxFontColor: ['#000', '#f20'],
    }
];
Cross.call("gisiframe", "mapControl", {
    action: "3d.loadHistogram",
    params: params
});
```
## 4 监听事件
> 父页面监听事件
### 4.1 地图点击监听
- 功能：监听地图点击事件的返回值
- 返回值: data.result: {layerid:"",id:""}
- layerid：建立图层时传入的layerid
	- 特殊图层的layerid（区域渲染）
	- 3d.renderZones："renderZone"
	- 3d.renderNumber："zoneRenderPoint"
	- 3d.renderLabel："zoneRenderLabel"
- id：图层上点位的id
```js
window.addEventListener('message', function (e) {
    var data = e.data;
    if(data.action == "mapClick"){
	        console.log(data.result.layerid);//建立图层时传入的图层id
	        console.log(data.result.id);//该点位id
    }
}, false);
```
