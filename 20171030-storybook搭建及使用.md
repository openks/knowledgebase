前段时间在群里看到有小伙伴发了storyBook的github[连接](https://github.com/storybooks/storybook)
自己看了下得知这是一个ui组件开发环境，考虑了下是很好用的，如果项目中UI组件越来越多管理起来势必会越来越麻烦  
storybook以可视化的方式让你可以安然处理自己的组件  
只是文档不够详细，demo是很容易跑起来的但是写个能用的就没那么容易了。 

问题来源：  
我自己在element-ui的基础上写了个新的组件，内容如下：  
1.使用props进行数据传递  
2.子组件要修改父组件的值(子组件为一弹框,父组件要打开弹框，子组件要能关闭弹框)  
3.在element-ui的基础上封装的组件  

在写story时就碰到了以下***问题***  
1.在story里如何引用其他组件(这里是element-ui)  
2.在story里如何进行数据传递   
3.在story里如何进行事件交互  

经过自己摸索发现也不是很难   
1.引用组件需要在配置文件进行配置
``` js
// .storybook/config.js
import { configure } from '@storybook/vue'
//引入vue
import Vue from 'vue';
//引入element-ui
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'
//引入自定义样式文件
import 'src/css/index.css'

Vue.use(ElementUI)

function loadStories() {
  require('../src/stories')
}

configure(loadStories, module)
```
配置过后在所有story里都可以使用element-ui了   
问题1得以解决  

2.数据传递跟父组件一样  
把这个story作为一个父组件即可  

3.事件交互也跟父组件一样  
同理问题2  
```js
//stories/index.js
//
//引入要管理的ui组件
import CompontA from './CompontA.vue';
storiesOf('compont-A', module)
  .add('decs', () => ({
    components: { CompontA },
    //组件需要数据 通过data传递
    data() {
      return {
        propa: [],
        propb: {},
        propShow: false
      };
    },
    //注意：模板只能有一个根元素
    template: `<div>
                <button @click="open">打开弹框</button>
                <compont-A 
                  :propA="propa" 
                  :propB="propb"
                  :show.sync="propShow">
                </compont-A>
              </div>`,
    methods: {
      //story的事件  
      open() {
        this.show = !this.show;
      }
    }
  }));
```
这样问题2，问题3得以***解决***



