---
title: '1.Echart-tooltips遇到的问题汇总'
date: '2024-5-3 13:26:00'
description: 'tooltips遇到的问题汇总'
cover: "./image/haibiantian.jpg"
categories:               
    - [Echart,tooltips]
---
# 无法使用全局shallowRef，但是item效果达不到预期，用指示器
EChart 画图，由于初始化存储对象为组件传值对象，如果用全局shallowRef对象赋值，会每次mqtt更新视图，看起来像闪退，所以无法直接用shallowRef，
但是用item的tooltips,看起来效果没用那么好，所以可以用指示器
```bash
tooltip: {
    trigger: "axis",
    axisPointer: {
      type: 'cross',          // 指示器类型
      axis: 'auto',          // 指示器的坐标轴
      snap: true,            // 坐标轴指示器是否吸附到具体的数值点上
      z: 1,                  // 坐标轴指示器的 z 值,图形层级
    }
  },
```
