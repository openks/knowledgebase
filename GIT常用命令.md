---
title:  git常用命令   
date: 2016-11-17 13:21:35  
description:  git常用命令记录  
keywords: [前端,git,git命令]  
---



## git log 命令

#### git log -n
显示前N条日志

#### git log --stat -n
显示简要的增改行数统计,每次提交文件的变更统计

#### git log --pretty=format:" "

选项 | 说明
-----|-----
%H  |提交对象（commit）的完整哈希字串   
%h  |提交对象的简短哈希字串  
%T  |树对象（tree）的完整哈希字串  
%t  |树对象的简短哈希字串  
%P  |父对象（parent）的完整哈希字串  
%p  |父对象的简短哈希字串  
%an |作者（author）的名字  
%ae |作者的电子邮件地址  
%ad |作者修订日期（可以用 -date= 选项定制格式）  
%ar |作者修订日期，按多久以前的方式显示  
%cn |提交者(committer)的名字  
%ce |提交者的电子邮件地址  
%cd |提交日期  
%cr |提交日期，按多久以前的方式显示  
%s  |提交说明  


```
git log --pretty=format:" %s" -3
test:add new question add question6
test:add new question add question5
test:add new question
//命令好用，起个别名
git config alias.logs "log --pretty=format:'%s'"
// 以后就可以使用如下命令，命令结果和上面一条命令结果一样
git logs -3
// 某些文件或某个文件的最近3次记录
git logs -3  static/*.html
// 指定作者为"BeginMan"的所有提交
git log --author=BeginMan
// 指定关键字为“init”的所有提交
git log --grep=init
// 指定提交者为"Jack"的所有提交
git log --committer=Jack

//查看文件的修改记录 可以看到提交版本
git log demo/js/propagation.js
commit bdbcf6226b05207fc4d4a6b56ab566ff0744295e
//回退到指定版本
git reset bdbcf6226b05207fc4d4a6b56ab566ff0744295e demo/js/propagation.js
```

## git Commit message
Commit message 都包括三个部分：Header，Body 和 Footer。
```
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

 Header部分只有一行，包括三个字段：type（必需）、scope（可选）和subject（必需）

type  | 意义
---|---
feat|新功能（feature）
fix|修补bug  
docs|文档（documentation）  
style| 格式（不影响代码运行的变动）  
refactor|重构（即不是新增功能，也不是修改bug的代码变动）  
test|增加测试  
chore|构建过程或辅助工具的变动  

scope用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

subject是 commit 目的的简短描述，不超过50个字符。

>以动词开头，使用第一人称现在时，比如change，而不是changed或changes  
第一个字母小写  
结尾不加句号（.）  

总结：`commit message应包含type+":"+subject`

## git issues
```
// 关闭一个
Closes #234
// 关闭多个issue
Closes #123, #245, #992
```

## git branch

```cmd
查看所有分支(前面带*号的代表你当前工作目录所处的分支)
git branch -a  


创建test分支
git branch test  

切换到test分支
git checkout test  

删除本地test分支
git branch -d test

删除远程test分支
git push origin --delete test

合并分支(把test分支合并到master分支上)
git checkout master
git merge test

把当前test分支提交到远程test分支
git push origin test

把本地所有分支提交到远程相对应的分支上
git push


//复制代码库到本地
git clone http://192.168.1.231/group-two/jujusports-web.git

//添加所有未添加的文件
git add .


//查看版本区别只显示文件名字
git diff --name-only

//git 查看某版本文件
//git show 版本号 文件名
git show 33e636914bf61536fe56d3872338f0087c1a1e5c static/order_undone.html

//刚创建git需要添加分支
git push -u origin master

//git撤销用branch上代码覆盖本地仓库及本地工程
git checkout routes/index.js

git reset [--hard|soft|mixed|merge|keep]
/*
[<commit>或HEAD]：将当前的分支重设（reset）到指定的<commit>或者HEAD（默认，如果不显示指定commit，默认是HEAD，即最新的一次提交），并且根据[mode]有可能更新index和working directory。mode的取值可以是hard、soft、mixed、merged、keep。下面来详细说明每种模式的意义和效果。
A). --hard：重设（reset） index和working directory，自从<commit>以来在working directory中的任何改变都被丢弃，并把HEAD指向<commit>。
B). --soft：index和working directory中的内容不作任何改变，仅仅把HEAD指向<commit>。这个模式的效果是，执行完毕后，自从<commit>以来的所有改变都会显示在git status的"Changes to be committed"中。
C).  --mixed：仅reset index，但是不reset working directory。这个模式是默认模式，即当不显示告知git reset模式时，会使用mixed模式。这个模式的效果是，working directory中文件的修改都会被保留，不会丢弃，但是也不会被标记成"Changes to be committed"，但是会打出什么还未被更新的报告。报告如下：
引用
Unstaged changes after reset:
M Test.Scala
M test.txt
*/

```

## git config
```
git config --global user.name  openks
git config --global user.email  zhuyangxing@foxmail.com
//显示所有配置
git config --list
//保存git的用户名密码
git config --global credential.helper store

```


[有道分享](http://note.youdao.com/yws/public/redirect/share?id=5f785b0976e591a76c962e0624feec5a&type=false)
