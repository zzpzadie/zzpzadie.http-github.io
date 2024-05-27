---
title: '3.安装React Developer Tools遇到的问题'
date: '2024-5-24 15:54:00'
description: '安装React Developer Tools遇到的问题'
cover: "./image/haibiantian.jpg"
categories:               
    - [react,配置,React Developer Tools]
---
# npm克隆
我先直接npm克隆了，然后安装它的依赖啥的，结果我根本没办法build出，然后看网上说换新的版本，换了，还是不成功
```bash
git clone https://github.com/facebook/react-devtools
cd react-devtools

npm --registry https://registry.npm.taobao.org install
npm run build:extension:chrome
```
# 下载了扩展版本
直接下载下来就能拖到浏览器加载为扩展程序，好嘛，浏览器又报错：React 项目启动在 chrome 上报错 之 Uncaught TypeError: Cannot read property ‘forEach‘ of undefined
![](./image/reactdev.png)
说让我找到node_modules/@pmmmwh/react-refresh-webpack-plugin/client/ReactRefreshEntry.js这个文件夹把下面这段代码注释重启项目就能解决，
好嘛我连这个文件夹都没有，安装了依赖，清除缓存都没有
```bash
RefreshRuntime.injectIntoGlobalHook(safeThis);
```
# 换一个扩展程序版本
网址：https://www.crx4chrome.com/crx/3068/
滑到下面下这个，我一开始看到差点下成广告的了，幸好进不去，嘿嘿
![](./image/download.png)
把之前的先关了，直接把新的拖进扩展程序即可，嘿，真不报错了，祝看到的你成功吧
![](./image/devexnew.png)
