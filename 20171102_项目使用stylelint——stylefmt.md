eslint可以很简单的让团队js代码风格一致,而css也有一款这样的工具只是还没有那么多人知道罢了  
他就是stylelint,它其实是参考eslint风格做的，也是需要安装stylelint和配置规则
```
//安装stylelint
npm i stylelint
//安装标准配置文件
npm i stylelint-config-standard
```
新增`.stylelintrc`配置文件
上面是继承标准，下面是自定义标准
```js
{
  'extends': 'stylelint-config-standard',
  'rules': {
    'indentation': 'tab',
    'rule-empty-line-before': 'always-multi-line',
    'number-leading-zero': null
  }
}
```

只不过这个东西的插件没有eslint那么方便，eslint可以保存自动修复。这个cli工具虽然已经提供自动修复功能但是插件没有  
找了非官方插件发现stylefmt符合要求但是无法做到保存自动修复  
也有一个atom-vue-stylefmt插件可以格式化.vue单文件组件  