---
title:  Building APIs with Node.js一书阅读笔记
date: 2017-1-17 12:54:35  
description:  粗略记录下Building APIs with Node.js 一书的部分新知识
keywords: [前端,restful API,JWT认证]  
---

1. npm Task介绍  

    即package.json的script标签,可以通过npm start + scriptname 来运行  

2. sqlite介绍  

   使用sqlite3作为数据库，不常用。

3. 使用apidoc为项目的api添加api文档
   文档写狠起来很麻烦，但是没有文档一段时间或更长时间之后用起来会更麻烦

4. mocha测试  

5. 通过插件JWT(json web token)实现用户认证  

    例子使用了最简单的认证方式，通过用户名密码获取一个简单的token，只要用户名密码不变则该token不变，而且该token在服务器与网页间传输，安全性无法的到保证。可以启用复杂点的`jsonwebtoken`插件添加token的有效期增强安全性

6. 通过插件compression启用gzip压缩

7. 通过插件cors允许非本域下的请求

8. 添加https支持

9. 通过插件helmet增强 HTTP security

10. postman 测试

    postman是强大的工具，之前有过粗略的了解，这次了解了更多，知道了变量配置和全局变量配置，这样切换环境只需修改几个变量即可立即参与测试，还可把上次测试返回数据保存到全局变量以供下次测试使用。写好测试用例后一次可以全部运行。非常方便实用
