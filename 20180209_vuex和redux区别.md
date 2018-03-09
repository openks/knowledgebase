vuex和redux区别  
1. vuex的store是手工创建的，redux的store是由createStore协助创建   
  rudex只会创建需要的字段，而vuex可能创建出未用到的字段，或遗漏某些需要字段即会出现State里没有声明，但是会直接使用的情况   
2. vuex的mutation(用于执行同步操作)类似于redux的action    
3. vuex的异步操作使用的是action,redux的异步操作需要使用中间件  
常用的中间件是redux-thunk  
简单来讲逻辑是这样的actionCreator返回的不是action而是一个function,这个function会在合适的时间(一般指异步结束时)dispach action

redux-thunk的实现是这样的：  
在action被分发时,中间件(redux-thunk)被调用,在需要的时候协助action creator分发真正的action(或者什么都不做)   
中间件是纯粹的函数有明确的使用方法且严格遵守以下格式
```js
var anyMiddleware = function ({
  dispatch,
  getState
}) {
  return function (next) {
    return function (action) {
      // 中间件业务逻辑相关代码
    }
  }
}
```
函数柯里化后为
```js
var anyMiddleware = curry(({
  dispatch,
  getState
}, next, action) => {
  // 中间件业务逻辑相关代码
})
```

