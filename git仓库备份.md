---
title: git仓库备份  
date: 2017-3-23 13:22:03  
description: git仓库备份,  git一次提交到两个仓库解决办法  
---

### git仓库备份
需求很简单  
在`oschina`上做主要的代码，在`coding`做代码备份  
1. 首先在`oschina`和`coding`上分别创建项目`test_git_mirroe`
2. 从`oschina`上`clone`代码`git clone https://git.oschina.net/openks/test_git_mirroe.git`  
3. 跳转到项目目录`cd test_git_mirroe`  
4. 添加coding的远程仓库`git remote add coding https://git.coding.net/zhuyangxing/test_git_mirroe.git`  
5. 提交`coding`做镜像代码`git push --mirror coding`  
6. 以后在`test_git_mirroe`上提交的代码都提交到`oschina`上  
7. 想要在`coding`上同步就执行`git push --mirror coding`即可  


### git一次提交到两个仓库
修改`.git`文件夹下的`config`文件  
在`fetch`下添加想要提交的其他`git`路径即可
```
[remote "origin"]
	url = https://git.oschina.net/openks/test_git_mirroe.git
	fetch = +refs/heads/*:refs/remotes/origin/*
	url = new git url
```
