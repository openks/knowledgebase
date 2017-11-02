vue-loader使用css-module方法
简易版
```
<style module>
.red {
  color: red;
}
.bold {
  font-weight: bold;
}
</style>
<template>
  <p :class="$style.red">
    This should be red {{$style.red}}
  </p>
</template>
```
升级版-对模块进行命名
```
<style module="a">
  /* identifiers injected as a */
  .yellow{
      color:yellow;
  }
</style>
<template>
  <p :class="a.yellow">
    This should be red {{a.yellow}}
  </p>
</template>
```
升级版二--自定义css-module类名
```
module: {
  rules: [
    {
      test: '\.vue$',
      loader: 'vue-loader',
      options: {
        cssModules: {
          localIdentName: '[path][name]---[local]---[hash:base64:5]',
          camelCase: true
        }
      }
    }
  ]
}
//在vuecli生成项目中使用方法 添加cssModules部分即可
// build/vue-loader.conf.js
module.exports = {
  loaders: utils.cssLoaders({
    sourceMap: isProduction
      ? config.build.productionSourceMap
      : config.dev.cssSourceMap,
    extract: isProduction
  }),
  cssModules: {
    localIdentName: '[path][name]---[local]---[hash:base64:5]',
    camelCase: true
  },
  transformToRequire: {
    video: 'src',
    source: 'src',
    img: 'src',
    image: 'xlink:href'
  }
}
```

详见[这里](https://vue-loader.vuejs.org/en/features/css-modules.html)