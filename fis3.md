---
title: 前端工程构建工具fis3的学习
date: 2016-3-11 13:32:28
tags: [前端]
categories: [前端]
description: 前端工程构建工具fis3的学习和使用
keywords: [fis,fis3,自动化构建]
---


>fis很强大对我来说有一下几个方面可供利用，这几点足以让我学习并利用它了
1.对文件增加MD5戳（便于增量发布，缓存）
2.对某些图片进行合并（把小图标们进行合并很有必要）
3.文件压缩（css,js以及图片文件的压缩）
4.多个js,css合并为一个


使用`npm install -g fis3`命令安装fis3  
安装完成后执行`fis3 -v` 判断是否安装成功  
升级fis使用 `npm update -g fis3`命令  
发布`fis3 release -d <path>` 

配置文件`fis-conf.js`  
```js
fis.match('::packager', {
//打包的时候启用csssprites插件
  spriter: fis.plugin('csssprites'),
//一个页面只包含这个页面用过的资源
  postpackager: fis.plugin('loader', {
    //allInOne: true
  })
});
//js,css,png文件添加MD5戳
fis.match('*.{js,css,png}', {
  useHash: true
});
//less文件解析
fis.match('*.less', {
  // fis-parser-less 插件进行解析 
  //需要提前安装该插件
  parser: fis.plugin('less'),
  // .less 文件后缀构建后被改成 .css 文件
  rExt: '.css'
})
fis.match('*.{less,css}', {
//需要fis3-postpackager-loader插件
  packTo: '/static/aio.css'
});


//生产环境压缩js文件
fis.media('prod').match('*.js', {
// fis-optimizer-uglify-js 插件进行压缩，已内置
  optimizer: fis.plugin('uglify-js')
});
//生产环境压缩css文件,FIS3 构建会对 CSS 中，路径带 ?__sprite的图片进行合并。
//为了节省编译的时间，分配到 useSprite: true 的 CSS 文件才会被处理。
fis.media('prod').match('*.css', {
  useSprite: true,
  //fis-optimizer-clean-css 插件进行压缩，已内置
  optimizer: fis.plugin('clean-css')
});
//生产环境压缩png文件
fis.media('prod').match('*.png', {
  // fis-optimizer-png-compressor 插件进行压缩，已内置
  optimizer: fis.plugin('png-compressor')
});


//debug环境下不使用MD5,
//fis3 release debug 启用 media debug 的配置
fis.media('debug').match('*.{js,css,png}', {
  useHash: false,
  useSprite: false,
  optimizer: null
})

//执行 fis3 release 直接发布到某个目录
// fis.match('*', {
//   deploy: fis.plugin('local-deliver', {
//     to: '/Users/my-name/work/htdocs'
//   })
// })
```

使用 `fis3 server open `命令开启内部自带的server所在文件夹 默认8080  
发布到fis自带的server上 `fis3 release`  
启动文件监听 `fis3 release -w`  
启动fis3的server `fis3 server start`  
修改端口并启动`fis3 server start -p 10203`  
