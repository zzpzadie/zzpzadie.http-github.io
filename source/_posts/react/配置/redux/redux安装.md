---
title: '1.安装redux'
date: '2024-5-30 14:02:00'
description: '安装redux'
cover: "./image/haibiantian.jpg"
categories:               
    - [react,配置,redux]
---
# 安装扩展程序
链接：https://pan.baidu.com/s/17ggewOjfpR_fRWhvHHUxug
提取码：zrch

下载完成之后得到Redux Devtools.crx文件，打开chrome浏览器的设置-->扩展程序界面。直接将这个文件拖进这个界面就行了。

# 安装reactRedux和redux
redux：管理状态；
reactRedux：作者写出让开发者更方便书写代码的工具
```bash
npm i redux react-redux --save
```
# /src下新建store，新建index.js
```bash
import { legacy_createStore} from "redux";
import reducer from "./reducer.js"
//创建数据仓库，让浏览器能正常使用ReduxDevtools
const store  = legacy_createStore(reducer)
export default store

```
