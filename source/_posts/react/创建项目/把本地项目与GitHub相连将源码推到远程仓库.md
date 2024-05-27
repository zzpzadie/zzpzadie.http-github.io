---
title: '2.把本地项目源码推到GitHub远程仓库'
date: '2024-4-17 13:46:00'
description: '存放项目源码'
cover: "./image/haibiantian.jpg"
categories:               
    - github配置
---
在第4步后，我运行了git push -u origin master，但是报错让我pull，我这时想起fetch后没看冲突，
git status查看有哪里冲突，又让我pull，不成功就执行了5，然后解决冲突后重新提交成功~，个人觉得可能是没解决冲突就push了
```bash
git init //git初始化
git fetch //远程仓库获取所有分支的最新数据,但并不会合并或更改你当前的工作分支,此时应该git status查看有哪里冲突然后解决冲突
git branch -u origin/master master //将本地的 master 分支与远程仓库的 master 分支关联起来.-u 参数指定了上游分支,即远程跟踪的分支.这样设置后,可以使用简单的 git pull 或 git push 来同步数据而无需指定远程分支
git remote set-head origin -a //这个命令设置了所有远程分支的 HEAD 引用,使其指向远程仓库的最新提交.-a 参数表示操作应用于所有远程分支
git pull --allow-unrelated-histories
git add .
git commit -m 'df'
git push
```
