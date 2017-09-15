---
title: 淘宝Rap服务使用简述
date: 2016-9-18 17:26:34
tags: [前端]
categories: [前端]
description: mockjs，生成随机数据，拦截 Ajax 请求，淘宝Rap服务使用简述
keywords: [前端，mockjs,前端模拟数据，拦截ajax请求,rap]
---

前端时间曾经发布文章说[独立于后端的前端开发](http://www.zhuyangxing.cn/2016/06/17/%E7%8B%AC%E7%AB%8B%E4%BA%8E%E5%90%8E%E7%AB%AF%E7%9A%84%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/),但是仍然需要自己写模拟数据，可以通过使用淘宝提供的rap服务来自动生成数据。需要在[淘宝rap](http://rap.taobao.org/org/index.do)上进行配置

注册后选择或创建一个团队，新建一个产品线，新建产品，创建项目，创建模块，创建页面
对页面的接口进行编辑，然后只需引入一个js文件即可使用模拟数据。
![image](http://7xl9u9.com1.z0.glb.clouddn.com/2016-09-18_17-07-45.png)

可以通过项目的编辑添加接口，设置请求类型，请求连接，返回数据格式以及请求和返回数据的数据结构来新建接口，接口建完之后就可以通过引入js文件(在配置按钮里查看)就可以看到效果。

请求和响应参数均支持mock写法,经测试只支持部分 如@cfirst,@cname就不支持了。



