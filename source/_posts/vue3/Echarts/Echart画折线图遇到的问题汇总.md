---
title: '1.Echart画折线图遇到的问题汇总'
date: '2024-4-25 14:21:00'
description: 'Echart画折线图遇到的问题汇总'
cover: "./image/haibiantian.jpg"
categories:               
    - Echart
---
# EChart 配置都写好了，但是无法显示图表：报错--index.vue:167 Can't get DOM width or height. Please check dom.clientWidth an
解决方法：
设置高度和宽度，在onMounted内添加clientWidth和clientHeight，chartOne是图层id；
如果不在onMounted内添加， 则会出现无法重复使用lineChartOne()的问题；
echarts.init也要写在这里面，写在lineChartOne()里会报错重复初始化了
```bash
onMounted(() => {
  initData();
  Object.defineProperty(document.getElementById('chartOne'),'clientWidth',{get:function(){return 1000;}})
  Object.defineProperty(document.getElementById('chartOne'),'clientHeight',{get:function(){return 500;}})
  mychartOne.value = echarts.init(document.getElementById('chartOne'))
})
```
# EChart 当同一个页面重复调用该组件，会重复添加clientWidth和clientHeight报错，可以直接在style内添加
```bash
<div class="right_chart" id="chartOne" style="width: 100%;height:400px"></div>
```
# EChart 当更新Y轴的数据源，折线标题不变化，而数据源打印已经变化
这个问题可能是由于图表的缓存导致的。当你使用setOption方法更新图表时，如果数据源没有发生变化，
图表可能会继续使用之前的缓存数据进行渲染，而不是重新获取最新的数据。
可以尝试在更新图表之前清除图表的缓存。这可以通过调用clear方法来实现：
```bash
let mychartOne = {}
function lineChartOne() {
  mychartOne.value.clear();
  mychartOne.value.setOption(optionOne.value)
}
```
# EChart vue3中使用echarts报Uncaught (in promise) Error: Initialize failed: invalid dom. at Module.init错误
:报这个错误是因为页面还没挂载上开始就加载echart,加一个延时函数，时间视情况而定！仅对于vue3使用echarts亲测有效！
```bash
setTimeout(function () {
    let mychartOne = echarts.init(document.getElementById('chartOne'))
    mychartOne.setOption(optionOne.value)
  },1000)
```
# EChart拖拽组件单页面不能放置多个chart：生成一个随机数的ID，并动态绑定id
：当写死为 id="chartOne" 时，拖拽新的到页面中，每次都绑定第一个
```bash
<div class="right_chart" :id="echarId"></div>
let echarId = Math.random()
let mychartOne = echarts.init(document.getElementById('echarId'))
```
# EChart 多条折线图，数据的x轴不一样，且只能有一条x轴
```bash
option = {
    title: {
        text: '折线图堆叠'
    },
    tooltip: {
        trigger: 'axis'
    },
    legend: {
        data: ['邮件营销', '联盟广告']
    },
 
    xAxis: {
        // 根据x轴数据决定type类型
        type: 'time', 
        boundaryGap: false,
        // 注： x轴不指定data,自动会从series取
    },
    yAxis: {
        type: 'value'
    },
    series: [
         {
            name: '联盟广告',
            type: 'line',data: [
                [
                '2020-11-26 00:00:00',
                "6"
            ],
            [
                "2020-11-26 01:00:00",
                "6"
            ],
            [
                "2020-11-26 02:00:00",
                "6"
            ],
            [
                "2020-11-26 03:00:00",
                "5"
            ]
            ]
        },
        {
            name: '邮件营销',
            type: 'line',
            data: [
                [
                "2020-11-26 00:57:00",
                "17.3"
            ],
            [
                "2020-11-26 05:22:22",
                "16.8"
            ],
            [
                "2020-11-26 08:32:16",
                "17.3"
            ],
            [
                "2020-11-26 08:40:57",
                "17.8"
            ],
            [
                "2020-11-26 08:46:54",
                "18.3"
            ]
            ]
        },
       
    ]
};
```
# EChart tooltip 不显示的问题
：我是在初始化对象用全局对象存储时无法显示，在方法内部定义变量可以显示，然后将以下改为item就都可以显示了，但是只能在点上hover显示，不是在坐标轴
之后我把全局对象用shallowRef定义，就可以用axis啦~
和 ref() 不同，浅层 ref 的内部值将会原样存储和暴露，并且不会被深层递归地转为响应式。只有对 .value 的访问是响应式的。
shallowRef() 常常用于对大型数据结构的性能优化或是与外部的状态管理系统集成。
```bash
tooltip: {
    trigger: 'axis',
  },

const mychartlList = shallowRef([])
```
# EChart 作为可重复利用添加的单组件，可以在添加动态id后加下面代码，将该‘元素引用’保存到名为 setRefs 的方法中。e=document.getElementById('chartOne')
```bash
:ref="(e) => setRefs(e)"
```
