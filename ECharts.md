### 基本用法

```html
<!-- 
    步骤1：引入echarts.js文件
    步骤2：准备一个呈现图表的盒子
    步骤3：初始化echarts实例对象
    步骤4：准备配置项
    步骤5：将配置项设置给echarts实例对象 
  -->
<div id="charts-wrap"></div>
<!-- CDN 引入 -->
<script src="https://cdn.jsdelivr.net/npm/echarts@4.9.0/dist/echarts.min.js"></script>
<script>
  const charts = echarts.init(document.getElementById('charts-wrap'))
  // 柱状图
  const option = {
        xAxis: { 
          type: 'category',
          data: [类目数组]
        },
        yAxis: {
          type: 'value'
          // 数值轴不需要设置data，而是在series中设置data
        },
    		// series:系列，以折线图为例，一个折线图可以出现多条折线，每条折线对应一个系列（图例）
        series: [
          {
            type: 'bar',
            data: [数值数组]
          }
        ]
      }
  charts.setOption(option)
</script> 
```

### 图表类型

#### 通用配置

##### title 标题

- 文字样式
  `textStyle`
- 标题边框
  `borderWidth`、`borderColor`、`borderRadius`
- 文字样式
  `top`、`left`、`bottom`、`right`

##### tooltip 提示框

- 触发类型：`trigger`

  `item`、`axis`

- 触发时机：`triggerOn`

  `mouseover`、`click`

- 格式化：`formatter`

  字符串模板、回调函数
##### toolbox 工具栏

- 内置有导出图片、数据视图、动态类型切换、数据区域缩放、重置五个工具
- 显示工具栏按钮feature

`saveAsImage`、`dataView`、`magicType`、`dataZoom`、`restore`

##### legend 图例

- 用于筛选系列，需要和`series`配合使用
- `legend`的`data`是一个数组
- `legend`的`data`的元素需要和`series`数组中某组数据的`name`值一致



#### 柱状图

##### 基本

- x轴和y轴
- `series`中的`type`设置为`'bar'`

##### 常见效果

- 最大值、最小值（`markpoint`） 、平均值（`markline`）

- 图形上文本的显示 

  ```js
  series: [
            {
              type: 'bar',
              label: {
                show: true
              }
            }
          ]
  ```

  

##### 特点

- 描述分类数据：每个类目的数值及排名



#### 折线图

##### 基本

- x轴和y轴
- `series`中的`type`设置为`'line'`

##### 常见效果

- 标记：最大值、最小值、平均值、标注区间

  `markPoint`、`markLine`、`markArea`

- 线条控制：平滑、风格

  `smooth`、`lineStyle`

- 填充风格（折线下方区域）

  `areaStyle`

- 边界分开

  `boundaryGap`  // 设置于类名轴，`true`：与数值轴分开，`false`：紧挨数值轴

- 脱离0值比例 `scale`

  设置成 true 后坐标刻度不会强制包含零刻度（不会强制轴的起点为0），在设置 min 和 max 之后该配置项无效

- 堆叠图 `stack` 

  数据堆叠，同个类目轴上系列配置相同的stack值后，后一个系列的值会在前面所有系列的值上相加

##### 特点

- 折线图常用来分析数据的变化趋势

#### 散点图

##### 基本

- x轴和y轴的共同数据：二维数组

- 数组：[[身高1，体重1]，[身高2，体重2]，... ]

- `xAxis`和`yAxis`的type都要设置为`value`，不需设置`data`属性

- `series`中的`type`设置为`'scatter'`

##### 常见效果

- 气泡图效果

  散点大小不同 `symbolSzie`

  散点颜色不同 `itemStyle.color`

- 涟漪动画效果

  `type`：'effectScatter'

  触发时机 `showEffectionOn`: 'render' | 'emphasis'

  涟漪效果 `rippleEffect`: {}
  
- 如果想在地图中显示散点的数据，所以我们需要给散点的图表增加一个配置

  coordinateSystem : 'geo'  // 该系列使用的坐标系 

##### 特点

- 散点图可以帮助我们推断出不同维度数据之间的相关性
- 可以展示数据的分布和聚合情况
- 散点图经常用在地图标注

####   直角坐标系常用配置

直角坐标系图表：柱状图、折线图、散点图

- 配置1：网格`grid`

  `grid`是用来控制直角坐标系的布局和大小

  x轴和y轴是在 `grid`的基础上进行绘制的

  - 显示`grid`

    `show`

  - `grid`的边框

    `borderWidth`、`borderColor`

  - `grid`的位置和大小

    距离容器的距离：`left`、`top`、`right`、`bottom`
    
    `containLabel:true` 在定位时是以坐标轴的文字为边界
    
    大小：`width`、`height`

- 配置2：坐标轴`axis`

  - 坐标轴类型 `type`

    `category`：类目轴，该类型必须通过data设置类目数据

    `value`：数值轴，自动会从目标数据中读取数据

    > x轴和y轴都可以设置为类目轴或数值轴

  - 位置 `position`

    `xAxis`：`可取值为 top`或 `bottom`

    `yAxis`：可取值为 `left`或 `right`
    
  - 坐标轴刻度标签的相关设置 `axisLabel`，可以设置坐标轴文字的大小、位置

  -  坐标轴名称 `name`

- 配置3：柱间的距离（柱状图）

  - series-bar.barCategoryGap： 同一系列的柱间距离 
  - series-bar.barGap： 不同系列的柱间距离 

- 配置4：区域缩放`dataZoom`

  `dataZoom`用于区域缩放，对数据范围过滤，x轴和y轴都可以配置

  `dataZoom`是一个数组，意味着可以配置多个区域缩放器

  > `toolbox.feature.dataZoom`可配置区域缩放器按钮工具

  - 类型`type`

    `slider`：滑块

    `inside`：内置，依靠鼠标滚轮或双指缩放

  - 指明产生作用的轴

    `dataZoom-slider.xAxisindex`：设置缩放组件控制的是哪个x轴，一般写0即可， 在单个图表实例中存在多个 x 轴的时候有用。 

    `dataZoom-slider.yAxisindex`：设置缩放组件控制的是哪个y轴，一般写0即可， 在单个图表实例中存在多个 y 轴的时候有用。 

  - 指明初始状态的缩放情况

    `start`：数据窗口范围的起始百分比

    `end`：数据窗口范围的结束百分比

#### 饼图

##### 基本

- 数据是由 `name`和 `value`组成的对象为元素的数组，[{name:'xxx'，value:123}，...... ]

- `series`中的 `type`设置为 `pie`

- 无需配置`xAxis`和`yAxis`

##### 常见效果

- 显示文字的格式化

  label.formatter

- 指定半径

  `radius`：20 

  `radius`：20%

- 圆环

  设置两个半径

  ```js
  series:[{
  	type:'pie',
  	radius：['50%'，'70%'] // 第一项是内半径，第二项是外半径
  }]
  ```

- 南丁格尔图：饼图每个区域的半径与占比呈正比

  `roseType`：'radius'

- 选中效果

  `selectedMode`：`single`/`multiple`

  `selectedOffset`：偏移距离

##### 特点

- 饼图可以清晰表达不同分类数据的**占比**情况

#### 地图

##### 地图图标的使用方式

- 百度地图API

  需要申请百度地图ak

- 矢量地图

  实现步骤：

  - ECharts最基本的代码结构
    引入is文件,DOM容器,初始化对象,设置 option

  - 准备中国的矢量地图json文件
    china.json

  - 使用Ajax获取 china. json

    原生JS使用Ajax读取本地文件或发送请求

    JQuery：$.get( '地图文件本地路径/请求地址', function(res) { } )

    Vue：import  xxx  from  'xxx.json'
  
  > GeoJson （地图数据json）格式的数据，具体格式见https://www.jianshu.com/p/852d7ad081b3
    >
    > ```json
  > {
    >   "type": "FeatureCollection",
  >   "features": []
    > }
  > ```
  
- 在回调函数中往 echarts全局对象注册地图的json数据
    echarts. registerMap('chinamap', res);

  - 在`geo`属性内设置
  `type`: 'map'
  
    `map`:'chinaMap'
  
    - 其余可选配置：
  
      可缩放：`roam`
  
      名称显示：`label.show:true`
  
      初始缩放比例：`zoom`
  
      地图中心点：`center:[x坐标，y坐标]`

##### 常见效果

- 显示区域
  1. 加载该区域的矢量地图数据
  2. 通过`registerMap`注册到`echarts`全局对象中
  3. 指明`geo`配置下的`type`和 `map`属性
  4. 通过`geo`配置下`zoom`放大该区域
  5. 通过`geo`配置下`center`定位中心点
  
- 不同区域根据数据显示不同颜色

  `visualMap`、`calculable`

- 散点图和地图的结合

  `series.type:'map'`、`series.type:'effectScatter'`

##### 地图特点

直观观察不同地理位置上数据的差异

#### 雷达图

##### 实现步骤

- ECharts最基本的代码结构:
  引入ECharts.js文件,DOM容器,初始化对象,设置 option

- 定义各个维度的最大值
  radar.indicator:[{name:'易用性' , max:100 } , ......]

  radar.shape: 'polygon'  // 配置雷达图最外层的图形 circle polygon

- 准备具体产品的数据
  series.data:[{name:'华为手机1' , value:[80,90,80,82,90]} , ...... ]  // 五个维度，value数组5个元素

- 图表类型
  在 `series.type : 'radar'`

##### 特点

雷达图可以用来分析多个维度的数据与标准数据的对比情况

#### 仪表盘

##### 实现步骤

- Echarts最基本的代码结构:
  引入ECharts.js文件,DOM容器,初始化对象,设置 option
- 准备数据,设置给 series下的data
  data: [{value: 97} , ...]  // 一个元素对象代表一个指针
- 图表类型：
  在 serles下设置type: 'gauge'

##### 常见效果

- 数值范围

  `min`、`max`

- 多个指针

  增加`series.data`的数组元素

- 多个指针颜色差异

  `itemStyle`

##### 特点

直观表示某些指标的变化或进度

#### 七个图表小结

##### 各个图表

- bar
- line
- scatter/effectScatter
- pie
- map
- radar
- gauge

##### 使用场景

- 柱状图:柱状图描述的是分类数据,呈现的是每一个分类中有多少
- 折线图:折线图常用来分析数据随时间的变化趋势
- 散点图:散点图可以帮助我们推断出不同维度数据之间的相关性
- 饼图:饼图可以很好地帮助用户快速了解不同分类的数据的占比情况
- 地图:地图主要可以帮助我们从宏观的角度快速看出不同地理位置上数据的差异
- 雷达图:雷达图可以用来分析多个维度的数据与标准数据的对比情况
- 仪表盘:仪表盘可以更直观的表现出某个指标的进度或实际情况

##### 其他使用效果

- axisPointer：坐标轴指示器配置项  
- series[{type:'xxx',emphasis:{}}]：鼠标经过图形高亮时的样式
- series-type.hoverAnimation：鼠标悬浮的动画效果开关
- series-type.labelLine：指示线显示与隐藏

### Echarts高级

#### 主题的使用

##### 内置主题

- Echarts中默认内置了两套主题: `light` 、`dark`
- 在初始化对象方法init中可以指明
  var chart=echarts.init(dom, 'light)
  var chart=echarts.init(dom, 'dark');

##### 自定义主题

https://echarts.apache.org/zh/download-theme.html

https://echarts.apache.org/zh/theme-builder.html

- 1.在官网（上述网站）中编辑主题
- 2.下载主题，是一个js文件
- 3.引入主题js文件
- 4.在init方法中使用主题

**在vue项目中引入主题**

- 在`public/index.html`中通过script标签引入主题js文件，注意该文件要放在public目录下（不论层级）



#### 调色盘

- 主题调色盘

- 全局调色盘

  ```javascript
  option:{
  	color:['red','green','blue']
  }
  ```

- 局部调色盘
  
  ```javascript
  series:[{
  	type:'bar',
  	color:['red','green','blue']
  }]
  ```
  
- 优先级：就近原则

  直接样式 > 局部 > 全局 > 主题

- 颜色渐变

  - 线性渐变

    ```javascript
    series:[
    	{
    		itemStyle:{
          /* 线性渐变，前四个参数分别是 x0, y0, x2, y2, 范围从 0 - 1，相当于在图形包围盒中的百分比，在y轴，向下为正方向，如果 globalCoord 为 `true`，则该四个值是绝对的像素位置 */
    			color:{
    				type:'linear',
    				x:0,
    				y:0,
    				x2:0,
    				y2:1,
    				colorStops:[
    					{
    						offset:0, color:'red' // 0%处的颜色为红色
    					},
    					{
    						offset:1, color:'blue' // 100%处的颜色为蓝色
    					}
    				]
    			}
    		}
    	}
    ]
    // echarts构造函数内置渐变生成器，常用于设置不同系列对应的样式
    color: new echarts.graphic.LinearGradient(x, y, x2, y2, colorStops)
    // color还可以是函数
    color:arg=>{}
    ```

  - 径向渐变

  ```javascript
  series:[
  	{
  		itemStyle:{
        // 径向渐变，前三个参数分别是圆心 x, y 和半径，取值同线性渐变
        color: {
            type: 'radial',
            x: 0.5,
            y: 0.5,
            r: 0.5,
            colorStops: [{
                offset: 0, color: 'red' // 0% 处的颜色
            }, {
                offset: 1, color: 'blue' // 100% 处的颜色
            }],
            global: false // 缺省为 false
        }
  		}
    }
  ]
  // echarts构造函数内置渐变生成器，常用于设置不同系列对应的样式
  color: new echarts.graphic.RadialGradient(x, y, r, colorStops)
  // color还可以是函数
  color:arg=>{}
  ```

  - 纹理填充

  ```javascript
  series:[
  	{
  		itemStyle:{
        // 纹理填充
        color: {
          image: imageDom, // 支持为 HTMLImageElement, HTMLCanvasElement，不支持路径字符串
            repeat: 'repeat' // 是否平铺，可以是 'repeat-x', 'repeat-y', 'no-repeat'
        }
  		}
    }
  ]
  ```

- 直接样式

  itemStyle、 textStyle、 lineStyle、 areaStyle、label

- 高亮样式（鼠标经过）
  
    用 emphasis包裏 itemStyle、 textStyle、 lineStyle、 areaStyle、label
    
- 优先级高，会覆盖主题中、调色盘的效果

##### 自适应

当浏览器的大小发生变化的时候，图表也能随之适配变化

- 1.取消容器的CSS样式属性width

- 2.监听窗口大小変化事件

- 3.在事件处理函数中调用 Echarts实例对象的 resize即可

  ```javascript
  window.onresize = function(
  	myChart.resize()
  )
  ```

#### 加载动画

ECharts已经内置好了加载数据的动画，我们只需要在合适的时机显示或者隐藏即可

- 显示加载动画
  mcharts.showLoading()
- 隐藏加载动画
  mcharts. hideLoading()

#### 增量动画的实现方式

- mcharts.setOption(option)
  所有数据的更新都通过 setOption实现，可以多次setOption，新option中只需要包含要修改或增加的属性即可，旧option的其余属性会保留整合到新option。

#### 动画配置

- 开启动画`animation`，默认为`true`

- 动画时长 `animationDuration`， `number`| `function`

  支持回调函数，可以通过每个数据返回不同的时长实现更戏剧的初始动画效果：

  ```js
  animationDuration: function (idx) {
      // 越往后的数据时长越大
      return idx * 100;
  }
  ```

-  数据更新动画的时长  ` animationDurationUpdate `， `number`| `function`，支持回调函数
- 动画的缓动效果 `animationEasing`

- 数据更新动画的缓动效果， ` animationEasingUpdate `
-  初始动画的延迟 `animationDelay`，`number`| `function`，支持回调函数
-  数据更新动画的延迟 `animationDelayUpdate`，`number`| `function`，支持回调函数 

-  是否开启动画的阈值，`animationThreshold` 当单个系列显示的图形数量大于这个阈值时会关闭动画 

#### `echarts`对象和`echartsInstance`对象

- 全局 `echarts`对象
  全局 `echarts`对象是引入 echarts. js文件之后就可以直接使用的
  
  - 常用方法
    - `init`  创建一个 ECharts 实例，返回 echartsInstance ，不能在单个容器上初始化多个 ECharts 实例
    
    - `registerTheme` 注册主题，只有注册过的主题才能在`init`方法中使用该主题
    
    - `registerMap`  注册可用的地图，必须在包括 [geo](https://echarts.apache.org/zh/option.html#geo) 组件或者 [map](https://echarts.apache.org/zh/option.html#series-map) 图表类型的时候才能使用
    
    - `connect `  多个图表实例实现联动 
    
      ```js
      // 分别设置每个实例的 group id
      chart1.group = 'group1';
      chart2.group = 'group1';
      echarts.connect('group1');
      // 或者可以直接传入需要联动的实例数组
      echarts.connect([chart1, chart2]);
      ```
  
- `echartsInstance`对象
  `echartsInstance`对象是通过 echarts.init方法调用之后得到的
  
  - 常用方法
  
    - setOption
  
      设置或修改图表实例的配置项以及数据
      多次调用 setoption方法，合并新的配置和旧的配置，用处：
    
    - 增量动画
      
    - 将初始化配置与数据配置和（动态）样式配置分离，从而使初始化配置能够复用
      
    - resize
    
    重新计算和绘制图表
      一般和 window对象的 resize事件结合使用
    
      ```js
    window.onresize = function{
      myChart.resize()
    }
      ```
    
  - on / off 
      绑定或者解绑事件处理函数
    
    - 鼠标事件
        常见事件：'click'、'dblclick'、' mouseup'、' mousedown'、' mousemove'、等
  
        事件参数arg：和事件相关的数据信息，详情查看[官网](https://echarts.apache.org/zh/api.html#events)
    
        ```js
        myChart.on('click', function(arg){
        	console.log(arg)
        })
      // 解绑点击事件，函数名可选，可以传入需要解绑的处理函数，不传的话解绑所有该类型的事件函数。
      myChart.off('click', 函数名) 
    
      ```
      
      - Echarts事件
    常见事件：'legendselectchanged'、' datazoom'、' pieselectchanged'、' mapselectchanged'等
        事件参数arg：和事件相关的数据信息，详情查看[官网](https://echarts.apache.org/zh/api.html#events)
      
        ```js
        myChart.on('legendselectchanged',function(arg){
        	console.log(arg)
        })
      myChart.off('legendselectchanged') // 解绑 legendselectchanged 事件
        ```
    
    
    - dispatchaction
      触发某些行为，使用代码模拟用户的行为
    
      ```js
      mCharts.dispatchAction({
        type:'highlight', //事件类型
        serieslndex:0, // 系列索引,series的第几个元素
        datalndex:1 // 数据中哪一项高亮
      })
      ```
    
    - clear
      清空当前实例，会移除实例中所有的组件和图表
      清空之后可以再次 setOption
    
    - dispose方法
      销毁实例
      销毁后实例无法再被使用，例如无法再次setOption

#### Ecahrts 卡顿优化

每一个图例在没有数据的时候它会创建一个定时器去渲染气泡，页面切换后，echarts 图例是销毁了，但是这个 echarts 的实例还在内存当中，同时它的气泡渲染定时器还在运行，这就导致 Echarts 占用CPU高，导致浏览器卡顿，当数据量较大时，浏览器甚至会崩溃。

解决方法：在页面卸载时释放该页面的 chart 资源。使用 dispose() 方法，虽然 dispose 销毁了这个实例，但会调用实例的 resize() 方法，则会报没有 resize 这个方法。而 clear() 方法则是清空当前实例数据（实例还是存在），不影响实例的 resize，而且能够减少内存，切换的时候会更顺畅。

### Vue组件格式

```vue
<template>
  <div class="com-container">
    <div class="com-chart" ref="demo"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // echarts对象
      myChart: null,
      // 数据
      demoData: []
    }
  },
  mounted() {
    this.initChart()
    this.getData()
    window.addEventListener('resize', this.screenAdapter)
    this.screenAdapter()
  },
  beforeDestroy(){
    this.myChart.clear()
  },
  destroyed() {
    window.removeEventListener('resize', this.screenAdapter)
  },
  methods: {
    // 初始化echarts对象
    initChart() {
      this.myChart = this.$echarts.init(this.$refs.demo)
      const initOption = {}
      this.myChart.setOption(initOption)
    },
    // 获取数据
    async getData() {
      // 处理数据后赋值给demoData
      this.updateChart()
    },
    // 更新图表
    updateChart() {
      const dataOption = {}
      this.myChart.setOption(dataOption)
    },
    // 图表适配浏览器窗口大小变化
    screenAdapter() {
      const adaptOption = {}
      this.myChart.setOption(adaptOption)
      this.myChart.resize()
    }
  }
}
</script>

<style>
</style>

```

> 在VScode中Ctrl+h，把demo全部替换