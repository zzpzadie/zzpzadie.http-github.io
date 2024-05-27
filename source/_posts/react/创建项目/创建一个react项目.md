---
title: '1.通过 vite 创建一个 react 项目， 并通过 npm run dev 能在线下环境正确运行'
date: '2024-4-17 13:00:00'
description: '如何创建react项目'
cover: "./image/haibiantian.jpg"
categories:               
    - github配置
---

# 前提：
1.首先，确保你已经安装了Node.js和npm。你可以在命令行中输入以下命令来检查它们的版本：
```bash
node -v
npm -v
```
我一开始的node.js版本安装久了点，提示让我更新，什么好几个方案，官网下载啊什么的，麻烦~
因为我已经有了这两，直接就在终端用命令更新，没有当时记录，不记得命令了，刚搜的，要是不对可以评论。更新最后版本是v21.7.1：
```bash
npm cache clean -f
npm install -g n
n latest
```
# 接下来，运行以下命令来创建一个新的React项目：
```bash
npm create-vite my-project //运行后选择自己要的技术和框架就行
cd my-project
npm install
npm run dev
```
至此，就创建完成了一个属于我们的本地react项目啦~如果还想在项目中使用别的技术，请安装相应的依赖即可~
