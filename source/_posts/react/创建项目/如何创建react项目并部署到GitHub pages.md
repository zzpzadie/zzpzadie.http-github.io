---
title: '如何创建react项目并部署到GitHub pages'
date: '2024-4-15 17:15:00'
description: '如何创建react项目并部署到GitHub pages'
cover: "./image/haibiantian.jpg"
categories:
    - github配置
---
# 前提：

[//]: # ({% post_link hello-world 你好世界 %})

<a href="https://zzpzadie.github.io/2024/04/17/react/%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE/%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AAreact%E9%A1%B9%E7%9B%AE/?highlight=react%E9%A1%B9%E7%9B%AE" target="_blank">
1.你已经通过 create-react-app 创建了一个 react 项目， 并通过 npm run dev 能在线下环境正确运行。
</a><br/>
<a href="https://zzpzadie.github.io/2024/04/17/react/%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE/%E6%8A%8A%E6%9C%AC%E5%9C%B0%E9%A1%B9%E7%9B%AE%E4%B8%8EGitHub%E7%9B%B8%E8%BF%9E%E5%B0%86%E6%BA%90%E7%A0%81%E6%8E%A8%E5%88%B0%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93/?highlight=%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93" target="_blank">
2.在 github 上已经创建了一个与你本地代码同步的分支--存放源码（master)～
</a><br/>


3.在GitHub仓库中--另外创建一个分支部署构建项目（gh-pages）
注：不同情况下可能有不同的效果，可以多尝试
# 准备工作：
1.安装 gh-pages
```bash
  npm install gh-pages --save-dev
```
2.修改 package.json，添加主页路径homepage，网上有的是用GitHub pages，有的是像下面简单写，但是两种在我这写了好像没区别，就不加赘述。
部署的出入口deploy:gh-pages(出口，部署分支)，dist(入口，打包生成的文件夹)，仓库repository
```bash
{
  // ...
  "homepage": "./",
  "dependencies": {
    // ...
  },
  "scripts": {
    // ...
    "deploy": "gh-pages -d dist"
  },
   "repository": {
    "type": "git",
    "url": "https://github.com/zzpzadie/test.git"
  },
}
```
3.修改 vite.config.js,可以先执行后面的部署，如果部署后缺少主路径可以到这里添加,我的就是部署后网页访问接口少了仓库名
本来应该是https://用户名.github.io/仓库名/路径，但是访问的都是https://用户名.github.io/路径
```bash
  base:'/test/'
```
# 开始部署
1.Github Pages 无法识别 React 代码，只能识别 html,css,js，故你需要先打包编译你的项目：
```bash
npm run build
```
2.你会发现你的项目目录多了一个 build 文件夹，这就是要部署的文件夹，终端执行以下代码：
```bash
npm run deploy
```
3.这时 github 上项目就多出了一个gh-pages的分支，在设置中Github Pages处选择gh-pages分支保存，部署完成。
过几分钟你再点击生成的链接即可访问你的页面，与线下环境下的页面相同即成功。详情如下：
![](./image/deployed.png)
