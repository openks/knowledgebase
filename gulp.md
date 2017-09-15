---
title: gulp自动化工具的学习  
date: 2016-3-11 13:25:51  
tags: [前端]  
categories: [前端]  
description: gulp自动化构建工具的使用  
keywords: [gulp,自动化构建]  
---


自动化构建在我看来就是以下几个方面：
1.自动删除开发时的注释（html,css,js）
2.检查文件是否出错(jshint)
3.压缩文件（html,css,js,img）
4.添加css浏览器兼容
在不修改源代码的基础上把构建后的代码及静态文件放到单独的目录里互不影响。

自动化构建工具常用的有两种 grunt和gulp。至于为什么选择gulp看gulp的[介绍ppt](http://slides.com/contra/gulp#/11)就能明白了，也有比较的文章[Gulp相对于Grunt的优势](http://blog.jobbole.com/81007/),当然对于我来说就是gulp比grunt简单，易用，插件多，学习成本低而且能达到相同的功能。

**为啥要用自动化构建工具**
就像 @fouber 讲的一样，前端工程有三个阶段，分别是库/框架选型，简单构建优化，JS/CSS模块化开发
几乎任何一个刚入门的前端都默认进入第一阶段，都会选jquery库，因为它使用范围广还有一个原因是你的老师们(或者教你的领导)他们经常用。然后写好了html，css，js等直接往服务器一扔就搞定了。

慢慢的就会发现里面很多注释性的内容，开始考虑删除注释，对文件进行压缩，这时候网上一找真找到这种东西了，这不就是自动化构建工具嘛。

最欣赏的是自动化构建完的文件和本地开发源文件没有任何关联，可以随意修改源文件并重新构建，gulp相当简单只有[5条命令](http://slides.com/contra/gulp#/17)了解即可。

相关配置代码（gulpfile.js）如下：
```js
var jsSrc = "static/js/*",
	jsDec = "./dest/js",
	imgSrc = "static/img/*",
	imgDec = "./dest/img",
	htmlSrc = "static/*.html",
	htmlDec = "./dest",
	lessSrc = "static/less/*.less",
	lessDec = "./dest/css",
	cssSrc = "static/css/*.css",
	cssDec = "./dest/css",
	gulp = require('gulp'),
	livereload = require('gulp-livereload');


gulp.task('default', ['img', 'html', 'less', 'css', 'js'], function() {
	// 将你的默认的任务代码放在这
	console.log("this is default!")
});

gulp.task('px2rem', function() {
	var postcss = require('gulp-postcss');
	var px2rem = require('postcss-px2rem');
	var processors = [px2rem({
		remUnit: 75
	})];
	return gulp.src(cssSrc)
		.pipe(postcss(processors))
		.pipe(gulp.dest(cssDec));
});

gulp.task('autoprefixer', function() {
	var postcss = require('gulp-postcss');
	var autoprefixer = require('autoprefixer');

	return gulp.src(cssSrc)
		.pipe(postcss([autoprefixer({
			browsers: ['last 2 versions']
		})]))
		.pipe(gulp.dest(cssDec));
});

gulp.task('sourcemaps', function() {
	var sourcemaps = require('gulp-sourcemaps');
	return gulp.src(cssSrc)
		.pipe(sourcemaps.init())
		.pipe(sourcemaps.write('.'))
		.pipe(gulp.dest(cssDec));
});

gulp.task('cssnano', function() {
	var cssnano = require('gulp-cssnano');
	return gulp.src(cssSrc)
		.pipe(cssnano())
		.pipe(gulp.dest(cssDec));
});

gulp.task('less', function() {
	var postcss = require('gulp-postcss');
	var less = require('gulp-less');
	var px2rem = require('postcss-px2rem');
	var autoprefixer = require('autoprefixer');
	var cssnano = require('gulp-cssnano');

	return gulp.src(lessSrc)
		.pipe(less()) //该任务调用的模块
		.pipe(postcss([px2rem({
			remUnit: 75
		})]))
		.pipe(postcss([autoprefixer({
			browsers: ['last 2 versions']
		})]))
		.pipe(cssnano())
		.pipe(gulp.dest(cssDec));
});


//png,jpg,gif,svg
gulp.task('img', function() {
	var imagemin = require('gulp-imagemin');
	var pngquant = require('imagemin-pngquant');
	return gulp.src(imgSrc)
		.pipe(imagemin({
			progressive: true,
			svgoPlugins: [{
				removeViewBox: false
			}, {
				cleanupIDs: false
			}],
			use: [pngquant()]
		}))
		.pipe(gulp.dest(imgDec));
});

gulp.task('html', function() {
	var htmlmin = require('gulp-htmlmin');
	return gulp.src(htmlSrc)
		.pipe(htmlmin({
			collapseWhitespace: true,
			removeComments: true,
			minifyJS: true,
			minifyCSS: true
		}))
		.pipe(gulp.dest(htmlDec));
});

gulp.task('css', function() {
	var postcss = require('gulp-postcss');
	var px2rem = require('postcss-px2rem');
	var autoprefixer = require('autoprefixer');
	var cssnano = require('gulp-cssnano');

	return gulp.src(cssSrc)
		.pipe(postcss([px2rem({
			remUnit: 75
		})]))
		.pipe(postcss([autoprefixer({
			browsers: ['last 2 versions']
		})]))
		.pipe(cssnano())
		.pipe(gulp.dest(cssDec));
});

gulp.task('js', function() {
	var jshint = require('gulp-jshint'),
		uglify = require('gulp-uglify');

	return gulp.src(jsSrc)
		.pipe(jshint())
		.pipe(jshint.reporter('default'))
		.pipe(uglify())
		.pipe(gulp.dest(jsDec));
});

// 监听任务 运行语句 gulp watch
gulp.task('watch', function() {
	var server = livereload();
	gulp.watch(htmlSrc, ['html'], function(file) {
		server.changed(file.path);
	});
	gulp.watch(lessSrc, ["less"], function(file) {
		server.changed(file.path);
	});
	gulp.watch(cssSrc, ["css"], function(file) {
		server.changed(file.path);
	});
	gulp.watch(imgSrc, ["img"], function(file) {
		server.changed(file.path);
	});
	gulp.watch(jsSrc, ["js"], function(file) {
		server.changed(file.path);
	});
});
```
对`less`，没想到更好的想法，我这里是单独处理，生成`css`后依旧需要添加前缀，删除注释，px转rem等
把`gulp.js`放在项目根目录，在项目根目录运行 `gulp & gulp watch` 就会自动进行构建。
用到插件如图：
![图片](http://7xl9u9.com1.z0.glb.clouddn.com/glup.png)

### gulp 4.0  
```
# 卸载全局的 gulp
$ npm uninstall gulp -g 
# 安装全局的 gulp 4.0 
$ npm install "gulpjs/gulp-cli#4.0" -g 
$ npm install "gulpjs/gulp#4.0" -g  
# 到项目目录里删掉本地的 gulp
$ npm rm gulp --save-dev 
# 安装本地的 gulp 4.0
$ npm install "gulpjs/gulp#4.0" --save-dev
```
 4.0主要增加了`gulp.series`和`gulp.parallel`两个方法。  
`gulp.series`里的任务是顺序执行的。   
`gulp.parallel`里的任务是同时执行的。

关于gulp4.0[更多](http://web.jobbole.com/82992/)信息


如有不对的地方还望各位看官给予指正。
