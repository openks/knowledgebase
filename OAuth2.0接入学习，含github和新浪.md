---
title: OAuth2.0接入学习，含github和新浪
date: 2016-9-18 17:28:58
tags: [前端]
categories: [前端]
description: OAuth2.0接入学习，含github和新浪
keywords: [前端，OAuth2.0,github,新浪]
---


# github接入
github的[文档](https://developer.github.com/v3/oauth/)其实说的已经很明白了按照文档一步一步操作就可以了

总的来说有一下三步：
第一步:获取code  
第二步:使用post获取access_token  
第三步：根据access_token获取想要数据  

首先需要创建一个[应用](https://github.com/settings/applications/new)页面如下  
![image](http://7xl9u9.com1.z0.glb.clouddn.com/2016-09-18_09-36-19.png)
填写以上四个信息即可，填写完成效果如下： 
![image](http://7xl9u9.com1.z0.glb.clouddn.com/2016-09-18_09-46-07.png)
点击进如可看到如下信息  
![image](http://7xl9u9.com1.z0.glb.clouddn.com/2016-09-18_09-38-52.png)
可以得到一个Client ID 和 Client Secret 后面会用到。

第一步：
在自己页面添加按钮点击跳转到git获取权限页面，页面地址为：`https://github.com/login/oauth/authorize?client_id=client_id`  [我的例子在这里](https://github.com/login/oauth/authorize?client_id=fdbdb307432243b88b1e) 效果如图： 

![image](http://7xl9u9.com1.z0.glb.clouddn.com/2016-09-18_10-53-00.png)
如果不同意直接叉掉页面即可，如果同意则跳转到设置的授权回调地址,会带一个名为code的参数

第二步：
根据`client_id`,`client_secret`,`code`三个参数使用POST方法获取`access_token`返回的不止这一个还有其他参数
`access_token=e72e16c7e42f292c6912e7710c838347ae178b4a&scope=user%2Cgist&token_type=bearer`

第三步：
通过`access_token` 就可以使用github 提供的API了。  
使用get请求`https://api.github.com/user?access_token=access_token`
即可获取用户相关信息

# 新浪微博接入

新浪的接入[文档](http://open.weibo.com/wiki/Oauth2/authorize)

创建[应用](http://open.weibo.com/apps)创建后页面如下  
![image](http://7xl9u9.com1.z0.glb.clouddn.com/2016-09-18_14-36-02.png)

在接口管理，授权机制页面设置`授权回调页`和`取消授权回调页`地址
在网站信息和基本信息里可以看到`App Key`和`App Secret`

第一步：
在自己页面添加按钮点击跳转到新浪微博获取权限页面，页面地址为：`https://api.weibo.com/oauth2/authorize?client_id=client_id&redirect_uri=redirect_uri`  [我的例子在这里](https://api.weibo.com/oauth2/authorize?client_id=2139214853&redirect_uri=http://openks.duapp.com/oauth?t=weibo) 效果如图： 

![image](http://7xl9u9.com1.z0.glb.clouddn.com/2016-09-18_14-17-41.png)
如果不同意直接叉掉页面即可，如果同意则跳转到设置的授权回调地址,会带一个名为code的参数

第二步：

根据`client_id(即AppKey)`,`client_secret(即AppSecret)`,`grant_type(需填写authorization_code)`,`redirect_uri`,`code(上一步返回的)`五个必须参数使用POST方法从`https://api.weibo.com/oauth2/access_token`获取`access_token`返回的不止这一个还有其他参数
```js
{
       "access_token": "ACCESS_TOKEN",
       "expires_in": 1234,
       "remind_in":"798114",
       "uid":"12341234"
 }
 ```

第三步：  
通过`access_token`及相关数据就可以使用微博提供的API了。  
使用get请求`https://api.weibo.com/2/users/show.json?access_token=access_token&uid=uid`其中uid为要查询用户id
即可获取用户相关信息

# 实现思路
点击使用第三方登录后跳转授权页面，授权完成后拿到`access_token`,根据`access_token`拿到用户id,把第三方名称加第三方用户id拼接字符串作为自己数据库里用户id,并设置为用户已登录，然后跳转页面到已登录页面，进入自己系统逻辑即可。