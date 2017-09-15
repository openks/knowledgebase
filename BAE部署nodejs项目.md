---
title: 在BAE上部署nodejs项目  
date: 2016-11-10 21:20:55  
description: 如何快速在BAE上部署项目及错误问题排查  
---

## 在BAE上添加应用,并clone代码

```cmd
//从git clone代码
git clone https://git.duapp.com/appid9rupli9dyd
```

## 安装express
在`appid9rupli9dyd`文件夹安装express
```
//安装express
npm install express --save
//安装express生成器
npm install express-generator -g
//-e 启用ejs模板(默认jade) -f 在非空文件夹生成目录结构
express -e-f
//项目初始化，修改当前package.json文件
npm init
//安装依赖包
npm install
//以debug方式启动项目
SET DEBUG=appid9rupli9dyd:* & npm start
```
本地3000端口查看效果 http://127.0.0.1:3000

因为BAE使用的是18080端口需要修改端口
```
//在app.js 文件导出之前添加默认端口修改
process.env.PORT =18080;
````
重启后去本地3000及18080端口查看效果 http://127.0.0.1:18080 ，3000端口已经不能访问，18080端口可访问

修改代码需要重新部署，我们可以通过使用supervisor提高nodejs调试效率,可以做到修改代码自动部署，使用起来也很简单

```bash
#全局安装supervisor
npm install -g supervisor
supervisor bin/www
```
public 目录可以直接访问，我不习惯使用默认，会修改为js,css,img同时会修改相对应的路径

## 部署BAE

上传代码到BAE,发现报错，错误信息如下
```
552 occured because of user code error

your package.json file error

pacakge.json must json file and must contain "scripts":{"start":"node xxx.js"}

please modify your package.json and recommit your code

Error Message is

[2016-07-29 14:47:57.550] [ERROR] manager - catch signal SIGTERM, manager will exit
[2016-07-29 14:48:00.316] [ERROR] manager - scripts.start in package.json must contain ".js", but your scripts.start is "node ./bin/www"
get more message, please check manager.log
```
start 必须以.js结尾。重命名`bin/www`为`bin/www.js`即可

如果不上传依赖文件,错误信息如下：
```
失败原因：编译失败
失败信息：Fix depends failed. Please check requirements.txt or package.json.

npm ERR! Linux 3.10.1-grsec
npm ERR! argv "/home/bae/bae/node-v4.4.4-linux-x64/bin/node" "/home/bae/bae/node-v4.4.4-linux-x64/bin/npm" "install" "--production"
npm ERR! node v4.4.4
npm ERR! npm v2.15.1
npm ERR! failed to fetch from registry: http://registry.cnpmjs.org/morgan
npm ERR!
npm ERR! If you need help, you may report this error at:
npm ERR! <https://github.com/npm/npm/issues>
npm ERR! Please include the following file with any support request:
npm ERR! /home/bae/run/baeng/helios/work/appid9rupli9dyd/.bae/npm-debug.log
```
一看就知道依赖加载失败，把依赖文件夹整体上传就可以了。  
由于BAE使用的可视化界面为rockmongo,而且不能远程连接不能使用mongoshell,所以我们需要稍微了解下rockmongo

## rockmongo
插入数据
```
array(
  "userName" : "张三疯",
  "passWord" : "123456",
  "birthday" : "1991-01-01",
  "gender" : 2
)
//结果
array (
  '_id' => new MongoId("579b0eb5b6939eec77000000"),
  'userName' => '张三疯',
  'passWord' => '123456',
  'birthday' => '1991-01-01',
  'gender' => new MongoInt64(2),
)
```
