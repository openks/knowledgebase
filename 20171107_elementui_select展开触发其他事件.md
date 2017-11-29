最近同事做到一个需求，页面有多个el-select每个el-select点开后需要向下拉框里添加不同的内容<br>
想写一个统一的方法用于处理这种情况，不用每个下拉框的`@visible-change`事件单独写一个函数<br>
结果发现`@visible-change`默认接受一个参数无法修改<br>
相当于添加自定义参数问题<br>
```vue
//这样可以获取下拉框状态 arguments为[true]
@visible-change="handerClick"
//这样无法获取下拉框状态 arguments为[a,b,c]
@visible-change="handerClick('a','b','c')"
//添加一个参数即可 arguments为[true,a,b,c]
@visible-change="handerClick($event,'a','b','c')"
```
```vue
<template lang="html">
  <div>
      <el-select v-model="value" placeholder="请选择"
      @visible-change="handerClick($event,'zx','ls')"
      id="s1"
      ref="ref0">
       <el-option
         v-for="item in options"
         :key="item.value"
         :label="item.label"
         :value="item.value">
       </el-option>
        </el-select>
      <el-select v-model="value1" placeholder="请选择"
      id="s2"
      @visible-change="handerClick"
      ref="ref1">
       <el-option
         v-for="item in options"
         :key="item.value"
         :label="item.label"
         :value="item.value">
       </el-option>
       </el-select>
  </div>
</template>

<script>
export default {
    data () {
        return {
            options: [{
                value: '选项1',
                label: '黄金糕'
            }, {
                value: '选项2',
                label: '双皮奶'
            }, {
                value: '选项3',
                label: '蚵仔煎'
            }, {
                value: '选项4',
                label: '龙须面'
            }, {
                value: '选项5',
                label: '北京烤鸭'
            }],
            value1: '',
            value: ''
        }
    },
    methods: {
        handerClick () {
            console.log('arguments', arguments)
        }
    }
}
</script>

```