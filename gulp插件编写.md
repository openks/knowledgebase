---
title: gulp预处理插件编写
date: 2016-4-5 17:22:10
tags: [前端]
categories: [前端]
description:  gulp预处理插件编写
keywords: [gulp]
---

常写好多页面通用代码如下
index.html
```html
<div class="header">
	<div class="wrapper">
		<div class="topbar">
			<a class="logo" href="###"></a>
			<div class="login_status"><a class="p_center" href="###">个人中心</a> <a href="###">登陆</a> <a href="###">注册</a></div>
		</div>
		<div class="nav">
			<ul>
				<li tag="0"><a href="index.html">首 页<br><span>HOME</span></a></li>
				<li tag="140"><a href="index.html">新闻公告<br><span>NEWS</span></a></li>
				<li class="cur" tag="280"><a href="###">数据中心<br><span>DATA</span><p style="text-indent:160px"><a href="data-trailer.html">赛事预告</a><a href="data-schedule.html">赛程赛果</a><a href="data-points.html">赛事数据</a></p></a></li>
				<li tag="420"><a href="index.html">视频图集<br><span>VIDEO</span></a></li>
				<li tag="560"><a href="index.html">参赛球队<br><span>TEAM</span></a></li>
				<li tag="700"><a href="index.html">商务合作<br><span>COOPERATION</span></a></li>
			</ul>
			<div class="mark"></div>
			</li>
		</div>
	</div>
</div>
```

多个页面公用nav我希望index.html可以这样写，如果导航增加二级菜单删除二级菜单等不用所有页面都做处理
index.html
```html

<div class="header">
	<div class="wrapper">
		<div class="topbar">
			<a class="logo" href="###"></a>
			<div class="login_status"><a class="p_center" href="###">个人中心</a> <a href="###">登陆</a> <a href="###">注册</a></div>
		</div>
		<link rel="import" href="static/widget/nav.html">
	</div>
</div>
```
至于cur可以在每个页面通过js进行添加

widget/nav.html

```html
<div class="nav">
	<ul>
		<li tag="0"><a href="index.html">首 页<br><span>HOME</span></a></li>
		<li tag="140"><a href="index.html">新闻公告<br><span>NEWS</span></a></li>
		<li tag="280"><a href="###">数据中心<br><span>DATA</span><p style="text-indent:160px"><a href="data-trailer.html">赛事预告</a><a href="data-schedule.html">赛程赛果</a><a href="data-points.html">赛事数据</a></p></a></li>
		<li tag="420"><a href="index.html">视频图集<br><span>VIDEO</span></a></li>
		<li tag="560"><a href="index.html">参赛球队<br><span>TEAM</span></a></li>
		<li tag="700"><a href="index.html">商务合作<br><span>COOPERATION</span></a></li>
	</ul>
	<div class="mark"></div>
	</li>
</div>
```

插件功能就是把`<link rel="import" href="static/widget/nav.html">`移除并替换为`nav.html`中的内容
插件内容如下index.js
```js
'use strict';

var PluginError = require('gulp-util').PluginError,
	Transform = require('stream').Transform,
	fs = require('fs'),
	PLUGIN_NAME = 'gulp-pre-link';

module.exports = function(opts) {
	opts = opts || {};
	var stream = new Transform({
		objectMode: true
	});

	stream._transform = function(file, encoding, cb) {
		if (file.isNull()) {
			return cb(null, file);
		}
		if (file.isStream()) {
			var error = 'Streaming not supported';
			return cb(new PluginError(PLUGIN_NAME, error));
		} else if (file.isBuffer()) {
			var content = String(file.contents);
			var reg = /<link\s+[\s\S]*?["'\s\w\/\-](?:>|$)/gi; 
			var arrs = content.match(reg); //获取link标签
			var reg2 = /\srel\s*=\s*('[^']+'|"[^"]+"|[^\s\/>]+)\s*href\s*=\s*('[^']+'|"[^"]+"|[^\s\/>]+)/i;
			if (arrs != null) {
				for (var i = 0; i < arrs.length; i++) {
					var result = arrs[i].match(reg2);
					if (result && result[1]) {
						var rel = result[1].replace(/^['"]|['"]$/g, '').toLowerCase();
						if (rel === 'import') {     //如果link的rel为import获取href内容
							var addr = result[2].replace(/^['"]|['"]$/g, '').toLowerCase();
							var _this = this;
							fs.readFile(addr, function(err, data) {
								if (err) throw err;
								content = content.replace(result.input, String(data));//读取href文件内容并替换link标签
								file.contents = new Buffer(content);
								_this.push(file);
							});
						} else {
							file.contents = new Buffer(content);
							this.push(file);
						}
					} else {
						file.contents = new Buffer(content);
						this.push(file);
					}
				}
				cb();
			}
		}
	};
	return stream;
};

```

使用方法
```js
//安装插件
npm install --save-dev gulp-pre-link

gulp.task('html', function() {
	var htmlmin = require('gulp-htmlmin');
	var posthtml = require('gulp-pre-link');

	return gulp.src(htmlSrc)
		.pipe(posthtml())//先处理link引入标签再压缩html
		.pipe(htmlmin({
			collapseWhitespace: true,
			removeComments: true,
			minifyJS: true,
			minifyCSS: true
		}))
		.pipe(gulp.dest(htmlDec));
});
```
并把插件发布到npm上

