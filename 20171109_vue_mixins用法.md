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

有几个页面都需要任务状态,该状态是一个对象数组。
根据需求每个用户获取的状态都是相同的，多处需要只去一处即可。
注意：全局mixin的data在每个组件上都会有的，但是要改变这个值能改的只能是自己组件的值。其父组件和子组件均为默认值
```js
// src/utils/mixin.js
let mixin = {
    data () {
        return {
            mixinTaskStatus: [],
            mixinUploadUrl: process.env.NODE_ENV === 'development' ? 'aaa/fileUpload' : 'bbb/fileUpload'
        }
    },
    methods: {
        mixinFetchState () {
          let data={}
          ......
          // 判断本地存储(localStorage或sessionStorage)是否存在，
          // 如果存在则赋值
          // 如果不存在从服务端获取数据并赋值及存储在本地
            this.mixinTaskStatus = data
        }
}
export default mixin
```
```js 
// src/main.js
// 启用全局mixn
import Vue from 'vue'
import mixin from '@/utils/mixin.js'
// 启用全局mixin
Vue.mixin({
    ...mixin
})
```
具体使用：
比如我在batchSendTask页面需要使用该参数，就在该页面的mounted函数里调用该方法
```js
mounted () {
    this.mixinFetchState()
}
```
在页面中就可以直接只用这个值了
```vue
<el-option :label="item.code"
           v-for="item in mixinTaskStatus"
           :key="item.id"
           :value="item.name"></el-option>
```
-----------------
add in 2018-01-12 17:12:00
如果二级代码类型比较多，你就需要在全局mixin里添加多个data和多个method,
这么一个一个的写就不明智了，把相关字段做参数，只用一个method岂不更好
```
// src/utils/mixin.js
let mixin = {
    data () {
        return {
            mixinStaticCode: {},//把所有二级代码都作为对象的属性
            mixinUploadUrl: process.env.NODE_ENV === 'development' ? 'aaa/fileUpload' : 'bbb/fileUpload'
        }
    },
    methods: {
        mixinFetchStaticCode (storageName,featchUrl/featchParam,attribute) {
          let data={}
          ......
          // 判断本地存储(localStorage或sessionStorage)是否存在，
          // 如果存在data则直接赋值
          this.mixinStaticCode[attribute]=data;
          // 如果不存在从服务端获取数据并赋值及存储在本地
          this.mixinTaskStatus = data
        }
}
export default mixin
```
在页面中就可以直接只用这个值了
```vue
<el-option :label="item.code"
           v-for="item in mixinStaticCode.aaaa"
           :key="item.id"
           :value="item.name"></el-option>
```