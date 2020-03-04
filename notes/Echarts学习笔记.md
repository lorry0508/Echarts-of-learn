# Echarts学习笔记

## 1.ECharts 配置语法

### 1.第一步：创建 HTML 页面

创建一个 HTML 页面，引入 echarts.min.js：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <!-- 引入 ECharts 文件 -->
    <script src="echarts.min.js"></script>
</head>
</html>
```

### 2.第二步: 为 ECharts 准备一个具备高宽的 DOM 容器

实例中 id 为 main 的 div 用于包含 ECharts 绘制的图表:

```html
<body>
    <!-- 为 ECharts 准备一个具备大小（宽高）的 DOM -->
    <div id="main" style="width: 600px;height:400px;"></div>
</body>
```

### 3.第三步: 设置配置信息

ECharts 库使用 json 格式来配置。

```js
echarts.init(document.getElementById('main')).setOption(option);
```

这里 option 表示使用 json 数据格式的配置来绘制图表。步骤如下：

#### **标题**

为图表配置标题：

```js
title: {
    text: '第一个 ECharts 实例'
}
```

#### **提示信息**

配置提示信息：

```js
tooltip: {},
```

#### **图例组件**

图例组件展现了不同系列的标记(symbol)，颜色和名字。可以通过点击图例控制哪些系列不显示。

```js
legend: {
    data: [{
        name: '系列1',
        // 强制设置图形为圆。
        icon: 'circle',
        // 设置文本为红色
        textStyle: {
            color: 'red'
        }
    }]
}
```

#### **X 轴**

配置要在 X 轴显示的项:

```js
xAxis: {
    data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
}
```

#### **Y 轴**

配置要在 Y 轴显示的项。

```js
yAxis: {}
```

#### **系列列表**

每个系列通过 type 决定自己的图表类型:

```js
series: [{
    name: '销量',  // 系列名称
    type: 'bar',  // 系列图表类型
    data: [5, 20, 36, 10, 10, 20]  // 系列中的数据内容
}]
```

##### 每个系列通过 type 决定自己的图表类型：

```js
type: 'bar'：柱状/条形图
type: 'line'：折线/面积图
type: 'pie'：饼图
type: 'scatter'：散点（气泡）图
type: 'effectScatter'：带有涟漪特效动画的散点（气泡）
type: 'radar'：雷达图
type: 'tree'：树型图
type: 'treemap'：树型图
type: 'sunburst'：旭日图
type: 'boxplot'：箱形图
type: 'candlestick'：K线图
type: 'heatmap'：热力图
type: 'map'：地图
type: 'parallel'：平行坐标系的系列
type: 'lines'：线图
type: 'graph'：关系图
type: 'sankey'：桑基图
type: 'funnel'：漏斗图
type: 'gauge'：仪表盘
type: 'pictorialBar'：象形柱图
type: 'themeRiver'：主题河流
type: 'custom'：自定义系列
```





## 2.ECharts 样式设置

ECharts 可以通过样式设置来改变图形元素或者文字的颜色、明暗、大小等。

### 颜色主题

ECharts4 开始，除了默认主题外，内置了两套主题，分别为 **light** 和 **dark**。

```js
var chart = echarts.init(dom, 'light');
 
或者
 
var chart = echarts.init(dom, 'dark');
```

目前主题下载提供了 JS 版本和 JSON 版本。

如果你使用 JS 版本，可以将 JS 主题代码保存一个 **主题名.js** 文件，然后在 HTML 中引用该文件，最后在代码中使用该主题。

```
<!-- 引入主题 -->
<script src="https://www.runoob.com/static/js/wonderland.js"></script>
...

// HTML 引入 wonderland.js 文件后，在初始化的时候调用
var myChart = echarts.init(dom, 'wonderland');
// ...
```

如果主题保存为 JSON 文件，那么可以自行加载和注册。

```
//主题名称是 wonderland
$.getJSON('wonderland.json', function (themeJSON) {
    echarts.registerTheme('wonderland', themeJSON)
    var myChart = echarts.init(dom, 'wonderland');
});
```

### 调色盘

调色盘可以在 option 中设置。

调色盘给定了一组颜色，图形、系列会自动从其中选择颜色。

可以设置全局的调色盘，也可以设置系列自己专属的调色盘。

```js
option = {
    // 全局调色盘。
    color: ['#c23531','#2f4554', '#61a0a8', '#d48265', '#91c7ae','#749f83',  '#ca8622', '#bda29a','#6e7074', '#546570', '#c4ccd3'],

    series: [{
        type: 'bar',
        // 此系列自己的调色盘。
        color: ['#dd6b66','#759aa0','#e69d87','#8dc1a9','#ea7e53','#eedd78','#73a373','#73b9bc','#7289ab', '#91ca8c','#f49f42'],
        ...
    }, {
        type: 'pie',
        // 此系列自己的调色盘。
        color: ['#37A2DA', '#32C5E9', '#67E0E3', '#9FE6B8', '#FFDB5C','#ff9f7f', '#fb7293', '#E062AE', '#E690D1', '#e7bcf3', '#9d96f5', '#8378EA', '#96BFFF'],
        ...
    }]
}
```

### 直接的样式设置 itemStyle, lineStyle, areaStyle, label, ...

直接的样式设置是比较常用设置方式。纵观 ECharts 的 [option](https://www.echartsjs.com/zh/option.html#title) 中，很多地方可以设置 [itemStyle](https://www.echartsjs.com/zh/option.html#series.itemStyle)、[lineStyle](https://www.echartsjs.com/zh/option.html#series-line.lineStyle)、[areaStyle](https://www.echartsjs.com/zh/option.html#series-line.areaStyle)、[label](https://www.echartsjs.com/zh/option.html#series.label) 等等。这些的地方可以直接设置图形元素的颜色、线宽、点的大小、标签的文字、标签的样式等等。

一般来说，ECharts 的各个系列和组件，都遵从这些命名习惯，虽然不同图表和组件中，`itemStyle`、`label` 等可能出现在不同的地方。

### 高亮的样式：emphasis

在鼠标悬浮到图形元素上时，一般会出现高亮的样式。默认情况下，高亮的样式是根据普通样式自动生成的。

如果要自定义高亮样式可以通过 emphasis 属性来定制：

## 3.ECharts 异步加载数据

ECharts 通常数据设置在 setOption 中，如果我们需要异步加载数据，可以配合 jQuery等工具，在异步获取数据后通过 setOption 填入数据和配置项就行。

```json
{
    "data_pie" : [
    {"value":235, "name":"视频广告"},
    {"value":274, "name":"联盟广告"},
    {"value":310, "name":"邮件营销"},
    {"value":335, "name":"直接访问"},
    {"value":400, "name":"搜索引擎"}
    ]
}
```

```js
var myChart = echarts.init(document.getElementById('main'));
$.get('https://www.runoob.com/static/js/echarts_test_data.json', function (data) {
    myChart.setOption({
        series : [
            {
                name: '访问来源',
                type: 'pie',    // 设置图表类型为饼图
                radius: '55%',  // 饼图的半径，外半径为可视区尺寸（容器高宽中较小一项）的 55% 长度。
                data:data.data_pie
            }
        ]
    })
}, 'json')
```

如果异步加载需要一段时间，我们可以添加 loading 效果，ECharts 默认有提供了一个简单的加载动画。只需要调用 showLoading 方法显示。数据加载完成后再调用 hideLoading 方法隐藏加载动画：

```js
var myChart = echarts.init(document.getElementById('main'));
myChart.showLoading();  // 开启 loading 效果
$.get('https://www.runoob.com/static/js/echarts_test_data.json', function (data) {
    myChart.hideLoading();  // 隐藏 loading 效果
    myChart.setOption({
        series : [
            {
                name: '访问来源',
                type: 'pie',    // 设置图表类型为饼图
                radius: '55%',  // 饼图的半径，外半径为可视区尺寸（容器高宽中较小一项）的 55% 长度。
                data:data.data_pie
            }
        ]
    })
}, 'json')
```

### 数据的动态更新

ECharts 由数据驱动，数据的改变驱动图表展现的改变，因此动态数据的实现也变得异常简单。

所有数据的更新都通过 setOption 实现，你只需要定时获取数据，setOption 填入数据，而不用考虑数据到底产生了那些变化，ECharts 会找到两组数据之间的差异然后通过合适的动画去表现数据的变化。





## 4.ECharts 数据集（dataset）

ECharts 使用 dataset 管理数据。

dataset 组件用于单独的数据集声明，从而数据可以单独管理，被多个组件复用，并且可以基于数据指定数据到视觉的映射。

### 数据到图形的映射

我们可以在配置项中将数据映射到图形中。

我么可以使用 series.seriesLayoutBy 属性来配置 dataset 是列（column）还是行（row）映射为图形系列（series），默认是按照列（column）来映射。

常用图表所描述的数据大部分是"二维表"结构，我们可以使用 series.encode 属性将对应的数据映射到坐标轴（如 X、Y 轴）

encode 声明的基本结构如下，其中冒号左边是坐标系、标签等特定名称，如 'x', 'y', 'tooltip' 等，冒号右边是数据中的维度名（string 格式）或者维度的序号（number 格式，从 0 开始计数），可以指定一个或多个维度（使用数组）。通常情况下，下面各种信息不需要所有的都写，按需写即可。

下面是 encode 支持的属性：

```js
// 在任何坐标系和系列中，都支持：
encode: {
    // 使用 “名为 product 的维度” 和 “名为 score 的维度” 的值在 tooltip 中显示
    tooltip: ['product', 'score']
    // 使用 “维度 1” 和 “维度 3” 的维度名连起来作为系列名。（有时候名字比较长，这可以避免在 series.name 重复输入这些名字）
    seriesName: [1, 3],
    // 表示使用 “维度2” 中的值作为 id。这在使用 setOption 动态更新数据时有用处，可以使新老数据用 id 对应起来，从而能够产生合适的数据更新动画。
    itemId: 2,
    // 指定数据项的名称使用 “维度3” 在饼图等图表中有用，可以使这个名字显示在图例（legend）中。
    itemName: 3
}

// 直角坐标系（grid/cartesian）特有的属性：
encode: {
    // 把 “维度1”、“维度5”、“名为 score 的维度” 映射到 X 轴：
    x: [1, 5, 'score'],
    // 把“维度0”映射到 Y 轴。
    y: 0
}

// 单轴（singleAxis）特有的属性：
encode: {
    single: 3
}

// 极坐标系（polar）特有的属性：
encode: {
    radius: 3,
    angle: 2
}

// 地理坐标系（geo）特有的属性：
encode: {
    lng: 3,
    lat: 2
}

// 对于一些没有坐标系的图表，例如饼图、漏斗图等，可以是：
encode: {
    value: 3
}
```

### 视觉通道（颜色、尺寸等）的映射

我们可以使用 visualMap 组件进行视觉通道的映射。

视觉元素可以是：

- symbol: 图元的图形类别。
- symbolSize: 图元的大小。
- color: 图元的颜色。
- colorAlpha: 图元的颜色的透明度。
- opacity: 图元以及其附属物（如文字标签）的透明度。
- colorLightness: 颜色的明暗度。
- colorSaturation: 颜色的饱和度。
- colorHue: 颜色的色调。

### 交互联动





## 5.ECharts 交互组件

ECharts 提供了很多交互组件：例组件 legend、标题组件 title、视觉映射组件 visualMap、数据区域缩放组件 dataZoom、时间线组件 timeline。

### dataZoom

dataZoom 组件可以实现通过鼠标滚轮滚动，放大缩小图表的功能。

默认情况下 dataZoom 控制 x 轴，即对 x 轴进行**数据窗口缩放**和**数据窗口平移**操作。

如果想在坐标系内进行拖动，以及用鼠标滚轮（或移动触屏上的两指滑动）进行缩放，那么需要 再再加上一个 inside 型的 dataZoom 组件。

在以上实例基础上我们再增加 **type: 'inside'** 的配置信息。

当然我们可以通过 dataZoom.xAxisIndex 或 dataZoom.yAxisIndex 来指定 dataZoom 控制哪个或哪些数轴。





## 6.ECharts 响应式

ECharts 图表显示在用户指定高宽的 DOM 节点（容器）中。

有时候我们希望在 PC 和 移动设备上都能够很好的展示图表的内容，实现响应式的设计，为了解决这个问题，ECharts 完善了组件的定位设置，并且实现了类似 [CSS Media Query](https://www.runoob.com/css3/css3-mediaqueries.html) 的自适应能力。

### ECharts 组件的定位和布局

大部分『组件』和『系列』会遵循两种定位方式：

#### 1.left/right/top/bottom/width/height 定位方式

这六个量中，每个量都可以是『绝对值』或者『百分比』或者『位置描述』。

```json
1.绝对值

单位是浏览器像素（px），用 `number` 形式书写（不写单位）。例如 `{left: 23, height: 400}`。

2.百分比

表示占 DOM 容器高宽的百分之多少，用 string 形式书写。例如 {right: '30%', bottom: '40%'}。

3.位置描述

可以设置 left: 'center'，表示水平居中。
可以设置 top: 'middle'，表示垂直居中。
```

这六个量的概念，和 CSS 中六个量的概念类似：

```json
1.left：距离 DOM 容器左边界的距离。
2.right：距离 DOM 容器右边界的距离。
3.top：距离 DOM 容器上边界的距离。
4.bottom：距离 DOM 容器下边界的距离。
5.width：宽度。
6.height：高度。
```

在横向，left、right、width 三个量中，只需两个量有值即可，因为任两个量可以决定组件的位置和大小，例如 left 和 right 或者 right 和 width 都可以决定组件的位置和大小。 纵向，top、bottom、height 三个量，和横向类同不赘述。

#### 2.center / radius 定位方式

```json
1.center

是一个数组，表示 [x, y]，其中，x、y可以是『绝对值』或者『百分比』，含义和前述相同。

2.radius

是一个数组，表示 [内半径, 外半径]，其中，内外半径可以是『绝对值』或者『百分比』，含义和前述相同。

在自适应容器大小时，百分比设置是很有用的。
```

#### 3.横向（horizontal）和纵向（vertical）

```json
ECharts的『外观狭长』型的组件（如 legend、visualMap、dataZoom、timeline等），大多提供了『横向布局』『纵向布局』的选择。例如，在细长的移动端屏幕上，可能适合使用『纵向布局』；在PC宽屏上，可能适合使用『横向布局』。

横纵向布局的设置，一般在『组件』或者『系列』的 orient 或者 layout 配置项上，设置为 'horizontal' 或者 'vertical'。
```

要在 option 中设置 Media Query 须遵循如下格式：

```js
option = {
    baseOption: { // 这里是基本的『原子option』。
        title: {...},
        legend: {...},
        series: [{...}, {...}, ...],
        ...
    },
    media: [ // 这里定义了 media query 的逐条规则。
        {
            query: {...},   // 这里写规则。
            option: {       // 这里写此规则满足下的option。
                legend: {...},
                ...
            }
        },
        {
            query: {...},   // 第二个规则。
            option: {       // 第二个规则对应的option。
                legend: {...},
                ...
            }
        },
        {                   // 这条里没有写规则，表示『默认』，
            option: {       // 即所有规则都不满足时，采纳这个option。
                legend: {...},
                ...
            }
        }
    ]
};
```

上面的例子中，baseOption、以及 media 每个 option 都是『原子 option』，即普通的含有各组件、系列定义的 option。而由『原子option』组合成的整个 option，我们称为『复合 option』。baseOption 是必然被使用的，此外，满足了某个 query 条件时，对应的 option 会被使用 chart.mergeOption() 来 merge 进去。

##### 1.query

```js
{
    minWidth: 200,
    maxHeight: 300,
    minAspectRatio: 1.3
}
```

现在支持三个属性：width、height、aspectRatio（长宽比）。每个属性都可以加上 min 或 max 前缀。比如，minWidth: 200 表示『大于等于200px宽度』。两个属性一起写表示『并且』，比如：{minWidth: 200, maxHeight: 300} 表示『大于等于200px宽度，并且小于等于300px高度』。

##### 2.option