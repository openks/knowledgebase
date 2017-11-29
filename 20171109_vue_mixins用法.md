看到过mixins但是一直没用过，后来发现他是这么用的
```js
var mixin = {
  created: function () {
    console.log(1)
  },
  data(){
    return {
      name:"demoName"
    }
  }
  methods:{
    showName:function(){
      //minxs使用实例的数据
      console.log(this.showName+this.demoName);
    }
  }
}
var vm = new Vue({
  data(){
    showName:false
  },
  created: function () {
    console.log(2)
  },
  methods:{
    show:function(){
      //调用minxins的方法
      this.showName()
      console.log("使用mixins的数据"+this.demoName);
    }
  }
  mixins: [mixin]
})
// => 1
// => 2
// 当调用show方法的时候 会调用mixins里的showName方法，公用钩子函数和方法以及数据
```
也可以绑定全局组件,然后所有组件都可使用  

```js
// src/utils/mixin.js
let mixin = {
    data () {
        return {
          // 本地上传图片地址(图片上传到服务端，地址应该是服务端地址，如果线上静态包和服务端不一致则需要配置线上地址)
          // 也可以在 config/prod.env.js多配置两个参数  
          // baseURL: process.env.NODE_ENV === 'development' ? process.env.DEV_PATH : process.env.PROD_PATH 
            baseURL: process.env.NODE_ENV === 'development' ? '本地调试服务器路径/配置过proxy的路径' : '线上服务器路径'
        }
    },
    methods: {
      // 常用localStorage 方法 
        setStorage (key, objValue) {
            window.localStorage.setItem(key, JSON.stringify(objValue))
        },
        getStorage (key) {
            return JSON.parse(window.localStorage.getItem(key))
        }
    }
}
export default mixin
```
```js
// main.js
import mixin from '@/utils/mixin.js'
//启用全局mixin
Vue.mixin({
    ...mixin
})
```