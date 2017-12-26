一个多月前研究过这个东西但是忘记记录了，现在开新项目需要重新搭建的时候发现没有做记录，只能从代码里找，实在不方便
该项目的[github](https://github.com/vue-styleguidist/vue-styleguidist)
1.安装`vue-styleguidist`
```bash
npm install --save-dev vue-styleguidist
```
2.添加配置文件
```js
// 以vuecli工具生成项目为例
// 根目录下添加文件styleguide.config.js
/* eslint global-require: "off" */
module.exports = {
  components: 'src/lib/*.vue',
  webpackConfig: Object.assign({},
    require('./build/webpack.dev.conf.js'),
    {
      /* Custom config options */
    }
  )
  showUsage: true,
  showCode: true
};
```
3.添加npm scripts
```js
{
  "scripts": {
+    "styleguide": "vue-styleguidist server",
+    "styleguide:build": "vue-styleguidist build"
  }
}
```
4.使用
```
# 使用方法
npm run styleguide
```


问题：
1. styleguide引入其他组件库的方法（如elementui）
2. 在md文件中引入变量的方法
3. 在md文件中显示code方法
问题1和问题2：
其实[文档](https://github.com/vue-styleguidist/vue-styleguidist/blob/master/docs/Cookbook.md#how-to-add-mixins-or-third-party-plugins-to-the-style-guide)里写的很详细了

```js
// src/minxins/styleguidist.js   
// styleguide的minxin文件
import VeeValidate from 'vee-validate';

Vue.use(VeeValidate);

export default {
  data() {
   return {
     colorDemo: 'blue',
     sizeDemo: 'large',
   }
 }
  /* ... */
}
```
```js
// styleguide.config.js 添加如下配置
module.exports = {
+  mixins: [
+      'src/mixins/styleguidist.js',
+  ],
};
```
只需简单配置就解决了问题1和问题2

问题3：在md文件中显示code方法
``` md
``` js static
这里是会显示出来的code
需要在js后面写上static 
不写static的代码是会运行的代码
```  <!--这是结尾符号-->
```
问题4：按照说明安装第三方插件elementui却报错找不到自定义组件
解决方法
我做了如下两个步骤
1.修改elementui的版本号从legacy到1.4.8
2.删除依赖包重新安装
结果就ok了，初步怀疑是安装依赖包时出现问题

[文档写法](https://github.com/vue-styleguidist/vue-styleguidist/blob/master/docs/Documenting.md)
更多是示例与用法可参考[Croud-Style-Guide](https://github.com/CroudSupport/Croud-Style-Guide)



