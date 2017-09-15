# 如何在vuecli项目里使用typescript
## 第一阶段
先安装`typescript`,`ts-loader`
```cmd 
yarn add typescript ts-loader
```
1.修改`webpack`配置文件
```js
// build/webpack.base.conf.js
entry: {
  app: './src/main.ts'// 修改为ts文件格式(务必留意修改配置文件格式需重启)
},
extensions: ['.js', '.vue', '.json', '.ts', '.tsx']//添加ts,tsx文件格式
//新增loader配置
{
    test: /\.tsx?$/,
    loader: 'ts-loader',
    exclude: /node_modules/,
    options: {
      appendTsSuffixTo: [/\.vue$/],
    }
}
```

2.新增配置文件
```json
// 根目录下 tsconfig.json
{
  "include": [
    "src/**/*"
  ],
  "exclude": [
    "node_modules"
  ],
  "compilerOptions": {
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true,
    "allowJs": true,
    "module": "es2015",
    "target": "es5",
    "moduleResolution": "node",
    "experimentalDecorators": true,
    "isolatedModules": true,
    "lib": [
      "dom",
      "es5",
      "es2015.promise"
    ],
    "sourceMap": true,
    "pretty": true
  }
}
```
3.新增.t.ds配置文件
``` js
// 新增 src/vue-shims.d.ts文件
declare module "*.vue" {
  import Vue from 'vue';
  export default Vue;
},
```
4.修改`main.js`为`main.ts`
```ts
//main.ts
import App from './App.vue';//添加vue后缀不然会报出无法识别的错误
```
经过以上四步设置后就可以直接在项目里使用`<script lang='ts'>`标签写`typescript`了
```html
<script lang='ts'>
export default {
  name: 'hello',
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
    };
  },
  computed: {
    computedMsg(): string {
      return `computed${this.msg}`;
    },
  },
  methods: {
    hello(): string {
      return `hello${this.msg}`;
    },
  },
};
</script>
```
# 第二阶段
使用官方推荐的(vue-class-component)[https://github.com/vuejs/vue-class-component]
安装插件
```js
yarn add  babel-plugin-transform-decorators-legacy vue-class-component
```
修改`babel`配置
``` babelrc 
//.babelrc 
  "plugins": ["transform-runtime","babel-plugin-transform-class-properties"],// 添加插件
```
之后就可以使用类的方式写`ts`了
```html
<script lang='ts'>
import Vue from 'vue';
import Component from 'vue-class-component';

@Component
class Hello extends Vue {
  // initial data
  name:string = 'hello';
  msga:string = 'sssd';
  msg:string = 'msg';
  // lifecycle hook
  mounted() {
    this.greet();
  }
  // computed
  get computedMsg() {
    console.log(`computed ${this.msg}`);
    return `computed ${this.msg}`;
  }
  // method
  greet() {
    console.log(`greeting: ${this.msg}`);
  }
  hello() {
    return this.msg;
  }
}
export default Hello;
</script>
```
如果需要`watch`则需要添加(`vue-property-decorator`)[https://github.com/kaorun343/vue-property-decorator]具体使用方法 请参照文档
``` cmd
yarn add vue-property-decorator
```
# 总结

我在尝试过程中发现两个坑爹问题  
1.项目起动起来后chrome浏览器无法访问 （纸飞机）  
2.第二阶段所有设置好后发现页面报错 （不认真）  
```
[Vue warn]: Property or method "msg" is not defined on the instance but referenced during render. Make sure to declare reactive data properties in the data option.

found in

---> <Hello> at src/components/Hello.vue
       <App> at src/App.vue
         <Root>
```
对于问题1:  
发现竟然跟***纸飞机***有关。。纸飞机 。。之前一直都是好的，在chrome自动升级后就不行了无法打开，
在我用mac访问局域网无法访问的时候才发觉跟网络有关把纸飞机一关，局域网和项目竟然都能正常了

对于问题2:  
一直找不到原因，明明都对啊，为什么显示不正常,看提示信息很明显没有声明就在template里使用.  
我的代码如下
```html
<script>
import Vue from 'vue';
import Component from 'vue-class-component';

@Component
class Hello extends Vue {
  // initial data
  name: 'hello';
  msga: 'sssd';
  msg: 'msg';
  // lifecycle hook
  mounted() {
    this.greet();
  }
  // computed
  get computedMsg() {
    console.log(`computed ${this.msg}`);
    return `computed ${this.msg}`;
  }
  // method
  greet() {
    console.log(`greeting: ${this.msg}`);
  }
  hello() {
    return this.msg;
  }
}
export default Hello;
</script>
```
甚至我都把`ts`都去了只用`vue-class-component`对比了N久搞到要回家了硬是没找到答案。。。
回家还在纳闷为毛不正常，为什么第二天一早很容易就发现问题了 
```  
msg: 'msg';//不是用冒号 应该用等号。。 这么简单昨天竟然一直没看出来
msg= 'msg';
```





