---
title: hexo博客搭建
date: 2016-2-25 16:05:26
tags: [hexo]
categories: [hexo]
description: hexo博客搭建相关信息及插件配置等
keywords: [hexo搭建,pacman代码高亮,pacman,个人博客搭建]
---


## 安装hexo

### 创建新博客 
在F:\hexo\code文件夹中使用如下命令
``` bash
$ npm install hexo -g
$ cd f:
$ cd F:\hexo
$ hexo code
```

### 启动服务  
在F:\hexo\code目录下使用命令
``` bash
$ hexo server
```

### 生成静态文件  
在F:\hexo\code目录下使用命令
``` bash
$ hexo generate
```

### 部署到github.io

把public下面的所有文件都复制到username.github.io项目的根目录

### 更换主题pacman
下载[pacman](https://github.com/A-limon/pacman)主题,下载zip压缩包即可
把压缩包下载完成后解压并修改pacman-master为pacman,并把该目录放到F:\hexo\code\themes目录下
修改F:\hexo\code\_config.yml文件里的theme为pacman即`theme: pacman`(中间空格不能少)

### 英文改中文
修改F:\hexo\code\_config.yml文件里的language`language: zh-CN`
修改theme\pacman\layout\index.ejs文件 (这里pacman作者写成字符串了)
```js
      prev_text: '&laquo; Prev',
      next_text: 'Next &raquo;'
```
修改为
```js
      prev_text: '&laquo;'+__('prev'),
      next_text: __('next')+'&raquo;'
```
其他类似未修改的地方与此相似，prev，next是在themes\pacman\languages 下面的zh-CN.yml文件里有配置的。

### 删除文章下面的默认分享
themes\pacman\layout\_partial\post\footer.ejs文件里注释掉相关代码即可
```html
  <!-- 
  <div data-url="<%- item.permalink %>" data-title="<% if (item.title){ %><%= item.title %> | <% } %><%= config.title %>" data-tsina="<%= theme.author.tsina %>" class="share clearfix">
  </div>
  -->
```

### pacman代码高亮
在themes/pacman/source/css/_base/variable.styl 文件里注释掉highlight = hexo-config("highlight.enable")即可

### hexo使用css预处理styl文件
语法简单,使用缩进表示层级关系

### rss订阅
安装插件hexo-generator-feed,修改themes/pacman/_config.yml文件的rss,`rss: /atom.xml`.
```bash
$ npm install --save hexo-generator-feed
```

### 网站地图
安装插件hexo-generator-sitemap.
```bash
$ npm install --save hexo-generator-sitemap
```
安装完插件后都要重启服务，重启办法先暂停再启动： `ctrl+c`暂停服务,`hexo s`启动服务，端口默认4000

### 百度统计
编辑文件hexo\themes\pacman\_config.yml，增加配置选项(中间的空格绝对不能忘):
baidu_tongji: true 
新建文件hexo\themes\pacman\layout\_partial\baidu_tongji.ejs，内容如下：
```nodejs
<% if (theme.baidu_tongji){ %>
<script type="text/javascript">
#你的百度统计代码
</script>
<% } %>
```
注册并登录百度统计获取你的统计代码。
编辑文件hexo\themes\pacman\layout\_partial\head.ejs，在『/head』之前增加：
``` nodejs
<%- partial('baidu_tongji') %>
```
重新生成并部署你的站点。







