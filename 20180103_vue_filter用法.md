**需求：**  
vue中，除了在模板中使用过滤器，有时候，methods中也需要使用filters中的过滤器，如何调用  
**解决方案：**  
### 定义全局filter
```js
// src/utils/filters.js
import moment from 'moment'
let gander = {
    1: '男',
    2: '女'
}
let ganderFilter = value => {
    // console.log(a)
    return gander[value] || ''
}
let dateTimeFilter = (value, format) => {
    if (value) {
        return moment(value)
            .format(format)
    } else {
        return value
    }
}

export {
    ganderFilter,
    dateTimeFilter,
}
```
### 使用全局filter
在vue实例化之前添加全局filter
```js
// src/main.js
import * as filter from '@/utils/filters.js'
import mixin from '@/utils/mixin.js'
// 添加filter
Object.keys(filter).forEach(k => {
    Vue.filter(k, filter[k])
})
// 启用全局mixin
Vue.mixin({
    ...mixin
})
/* eslint-disable no-new */
new Vue({
    el: '#app',
    router,
    store,
    template: '<App/>',
    components: { App }
})
```
### 在模板里使用全局filter
```html
<template>
  <!-- 接受多个参数，第一个参数即需要处理的值 -->
  <div> {{ time | dateTimeFilter("YYYY-MM-DD HH:mm")}}</div>
  <!-- 只接受1个参数即需要处理的值 -->
  <div> {{ gander | ganderFilter}}</div>
</template>
```
### 在methods里使用全局filter

方法一：从filters文件中只引入需要的filter  
特点：把filter作为函数引入到需要的文件里  
```js
import ganderFilter from '@/utils/filters.js'
methods: {
  methodA: function (gander) {
    // 如下方式调用
    return ganderFilter(gander)
  }
}
```
方法二：从vue实例里获取需要的filter  
特点：不需要做任何其他处理，方便 推荐使用
```js
methods: {
  methodA: function () {
    // 如下方式调用
    this.$options.filters['filter-name'](...args)
  }
}
```
方法三：引入需要的filter到mixin里当做一个method  
特点：所有vue实例上都会有这个方法 推荐使用
```js
// src/utils/mixin.js
import * as filter from '@/utils/filters.js'
let mixin = {
    methods: {
        ...filter,
        methodA,
        methodB
    }
}
export default mixin
```
其实三种方法都可以