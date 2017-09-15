---
title:  express允许多个网站跨域访问  
date: 2016-11-17 13:21:35  
description:  express允许多个网站跨域访问简单办法   
keywords: [前端]  
---


`Access-Control-Allow-Origin`只允许一个值不允许多个值，我们会很自然的理解为多个域添加逗号，但是的确不行，该属性的值只允许一个值。  
**解决方案：**  
判断当前请求的host，是否在允许列表如果在允许列表，则设置`Access-Control-Allow-Origin`属性为该请求的host即可。详细代码入下：

```js
//设置允许跨域访问
app.all('/api/*', function(req, res, next) {
  //把允许的域都写在allows数组里即可
  var allows = ["http://openks.oschina.io", "http://www.zhuyangxing.cn"];
  //线上环境使用线上地址，线下环境使用线下地址。也可把所有地址写在允许的数组里不用判断线上和线下环境
  if (process.env.SERVER_SOFTWARE != undefined && process.env.SERVER_SOFTWARE.indexOf("bae") > -1) {
    if (allows.indexOf(req.headers.origin) > -1) {
      res.header("Access-Control-Allow-Origin", req.headers.origin);
    }
  } else {
    res.header("Access-Control-Allow-Origin", "http://10.10.11.222");
  }
  res.header("Access-Control-Allow-Headers", "X-Requested-With");
  res.header("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS");
  res.header("Content-Type", "application/json;charset=utf-8");
  next();
});
```

当然也可以使用`npm`上已经存在的插件`cors` 关于 `cors`更多信息在[这里](https://www.npmjs.com/package/cors)
