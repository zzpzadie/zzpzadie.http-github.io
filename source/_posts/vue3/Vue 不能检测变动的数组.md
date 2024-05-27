---
title: '1.Vue 不能检测变动的数组'
date: '2024-5-8 15:56:00'
description: '深拷贝'
cover: "./image/haibiantian.jpg"
categories:               
    - Vue 不能检测变动的数组
---
# 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
```bash
       set(arr,index,{age:12,name:'wee'})
```
