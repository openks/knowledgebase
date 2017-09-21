# 从element-ui学习怎样做js单元测试

1. 寻找入口  
所有正常项目都是从`package.json`里的`script`开始的  
```js
//package.json 
"scripts": {
  "lint": "eslint src/**/* test/**/* packages/**/*.{js,vue} build/**/* --quiet",
  "test": "npm run lint && cross-env CI_ENV=/dev/ karma start test/unit/karma.conf.js --single-run",
},
```
只需要看这两行  
先执行`lint`脚本再使用[cross-env](https://github.com/kentcdodds/cross-env)设置全局环境变量`CI_ENV`  
然后使用`test/unit/karma.conf.js`的配置文件启动[karma](http://karma-runner.github.io/1.0/index.html)  
先看这个参数[single-run](http://karma-runner.github.io/1.0/config/configuration-file.html)这里没有定位只能全页面搜索  
说明`karma`启动并使用配置的所有浏览器根据测试结果返回0或者1  
2. 查看karma的配置文件`karma.conf.js`

```js
//引入webpack配置
var webpackConfig = require('../../build/cooking.test');

// no need for app entry during tests 删除webpack入口
delete webpackConfig.entry;

module.exports = function(config) {
  config.set({
    // to run in additional browsers:
    // 1. install corresponding karma launcher
    //    http://karma-runner.github.io/0.13/config/browsers.html
    // 2. add it to the `browsers` array below.
    // 设置浏览器 这里使用了PhantomJS  http://phantomjs.org/
    browsers: ['PhantomJS'],
    //使用测试框架 mocha http://mochajs.org/  和 sinon-chai https://www.npmjs.com/package/sinon-chai
    frameworks: ['mocha', 'sinon-chai'],
    //输出为 spec 
    //使用karma-coverage 输出为 coverage  使用Istanbul http://gotwarlost.github.io/istanbul/ 
    //输出为 进行代码覆盖检查 https://www.npmjs.com/package/karma-coverage
    reporters: ['spec', 'coverage'],
    // 入口文件为test/unit/index.js文件
    files: ['./index.js'],
    preprocessors: {
      //使用webpack和sourcemap做预处理 更多：https://www.npmjs.com/package/karma-webpack
      './index.js': ['webpack', 'sourcemap']
    },
    webpack: webpackConfig,
    webpackMiddleware: {
      noInfo: true
    },
    coverageReporter: {
      dir: './coverage',
      reporters: [
        { type: 'lcov', subdir: '.' },
        { type: 'text-summary' }
      ]
    }
  });
};

```

根据`karma-webpack`里的描述 `test/unit/index.js`会把`specs`文件夹下的所有文件都作为测试用例使用`PhantomJS`进行测试。

随便查看任意一个测试文件
```js
//test/unit/specs/alert.spec.js
import { createTest, createVue, destroyVM } from '../util';
import Alert from 'packages/alert';
```
导入了`test/unit/util.js`文件 查看该文件
```js
// test/unit/util.js
import Vue from 'vue';
import Element from 'main/index.js';
// main文件夹怎么木有的？一定是配置了别名 
```
main文件夹木有的？一定是配置了别名不然早报错了   
终于在`build/config.js`找到别名了  
```js
// build/config.js
exports.alias = {
  main: path.resolve(__dirname, '../src'),
};
```
继续看`util.js`先在util文件里引入了`Element`也就是说引入`src/index.js`文件，
这也正常要做测试需要先把被测的东西给引进来   
回看`alert.spec.js`  
引入了`createTest`,`createVue`,`destroyVM`三个东西  
`createTest`创建一个测试组件，根据参数进行判断是否接受props数据及是否需要绑定到dom节点上
如果需要绑定到dom节点则生成一个新节点绑定到document对象上并作为该测试组件的根节点。  
`createVue`创建一个vue实例，根据参数进行判断是否需要绑定到dom节点上  
`destroyVM` 删除一个vue实例相关的dom节点，根据参数判断要删除的是哪个实例  
```js
import { createTest, createVue, destroyVM } from '../util';
import Alert from 'packages/alert';

describe('Alert', () => {
  let vm;
  //每一个测试结束后都需要收回vm 
  afterEach(() => {
    destroyVM(vm);
  });

  it('create', () => {
    //先测试创建alert对象 title为test,并且显示icon 
    //通过判断title内容和icon的class是否存在来做测试  
    vm = createTest(Alert, {
      title: 'test',
      showIcon: true
    }, true);
    expect(vm.$el.querySelector('.el-alert__title').textContent).to.equal('test');
    expect(vm.$el.classList.contains('el-alert--info')).to.true;
  });
  
  it('description', () => {
    // 设置描述内容
    // 并通过class获取描述对象内容和设置内容是否一致判断描述是否生效
    vm = createTest(Alert, {
      title: 'Dorne',
      description: 'Unbowed, Unbent, Unbroken',
      showIcon: true
    }, true);
    expect(vm.$el.querySelector('.el-alert__description').textContent)
      .to.equal('Unbowed, Unbent, Unbroken');
  });
});
```












