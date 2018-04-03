## 如何理解redux的middleware中间件
如果需要在dispatch前后分别打印出action,和action后的state，我们需要怎么做
实现方案如下：  
首先我们想到在前后分别添加console,每次调用都在调用前后加console  
1. 在每次dispatch前后加console  
  ```js
  console.log("start ",action)
  store.dispatch(action)
  console.log("end ",store.getState())
  ```
  但是这样每一次调用都要多写两行代码,总的来说会出现很多console，最容易想到的就是把这段代码封装到一个函数里，每次需要的时候调用函数即可  
2. 定义成方法，在需要dispatch时调用方法
  ```js
  function dispatchAndLog(store,action){
    console.log("start ",action)
    store.dispatch(action)
    console.log("end ",store.getState())
  }
  ```
  调用dispatch的地方改为调用如下代码
  ```
  dispatchAndLog(store,action)
  ```
  这样来说貌似可以了,但是每次都调用一个非api方法，新人不易理解，还是应该调用原有api,然后又有了修改默认dispatch的方案  
3. 修改默认的dispatch方法
  在以后的调用中直接调用`store.dispatch`即可
  ```js
  const next = store.dispatch
  store.dispatch = fuction dispatchAndLog(store,action){
    console.log("start ",action)
    let result = next(action)
    console.log("end ",store.getState())
    return result
  }
  ```
  这样看起来蛮好的，但是如果我现在不仅需要这一个打印日志的功能还需要一个能提供报错信息的功能
  我们可能想到如下方案  

4. 添加出错报告功能     

  ```js 
  function patchStoreToAddLog(store,action){
    const next = store.dispatch
    store.dispatch = function dispatchAndLog(store,action){
      console.log("start ",action)
      let result = next(action)
      console.log("end ",store.getState())
      return result
    }
  }

  function patchStoreToAddCrashReporting(store,action){
    const next = store.dispatch
    store.dispatch = function dispatchAndCrashReporting(store,action){
      try {
        return next(action)
      } catch (err) {
        console.error('Caught an exception!', err)
        // 把错误信息发送到其他监控系统
         throw err
      }
    }
  }
  ```
  这样即要日志，又要报错的时候依次调用两个函数即可

  ```js
  patchStoreToAddLog(store,action)
  patchStoreToAddCrashReporting(store,action)
  ```
  这样还是原来问题每次调用都要写两个，麻烦 
  更好方案如下：   

5. 使用一个功能返回一个函数,返回的函数用于处理插件的逻辑    

  ```js
  function logger(store) {
    const next = store.dispatch
    // Previously:
    // store.dispatch = function dispatchAndLog(action) {
    return function dispatchAndLog(action) {
      console.log('dispatching', action)
      let result = next(action)
      console.log('next state', store.getState())
      return result;
    }
  }
  function crashReporting(store){
    const next = store.dispatch
    return function dispatchAndCrashReporting(action){
      try {
        return next(action)
      } catch (err) {
        console.error('Caught an exception!', err)
        // 把错误信息发送到其他监控系统
         throw err
      }
    }
  }
  ```
  调用方法如下
  ```js
  logger(store)(action)
  crashReporting(store)(action)
  ```
  仔细观察其实是有规律的,可以写一个函数来遍历执行所有的插件
  ```js
  function applyMiddlewareByMonkeypatching(store, middlewares){
     middlewares = middlewares.slice()
     middlewares.reverse() //确保卸载后面的插件包在里面，后面执行
     middlewares.forEach(middleware=>
      //每一个middleware都可以直接调用前一个 middleware 包装过的 store.dispatch
       store.dispatch = middleware(store)
     )
  }
  ```
  调用如下
  ```js
  applyMiddlewareByMonkeypatching(store,[logger,crashReporting])
  ```
  等于如下代码
  ```js
  // according to https://redux.js.org/api-reference/store#dispatch
  // store.dispatch(action) 返回新的state 即新的store
  store.dispatch = logger(crashReporting(store))
  //也就等同于如下代码
  store.dispatch = function dispatchAndLog(action) {
    console.log('dispatching', action)
    let result = function dispatchAndCrashReporting(action){
      try {
        return next(action)
      } catch (err) {
        console.error('Caught an exception!', err)
        // 把错误信息发送到其他监控系统
         throw err
      }
    }
    console.log('next state', store.getState())
    return result;
  }
  ```
  这种方法已经很好了，但是它依旧是修改原来的store.dispatch方法,我们可以想办法不修改默认的`store.dispatch`方法 
 
6. 更进一步
 
  ```js
  function logger(store) {
    return function wrapperDispatchAndLog(next){
      return function dispatchAndLog(action) {
        console.log('dispatching', action)
        let result = next(action)
        console.log('next state', store.getState())
        return result;
      }
    }
  }
  // es6写法
  const logger = store => next => action => {
        console.log('dispatching', action)
        let result = next(action)
        console.log('next state', store.getState())
        return result;
      }
    }
  }
  // 修改遍历函数
  function applyMiddleware (store, middlewares){
    middlewares = middlewares.splice()
    middlewares.reverse()
    let dispatch = store.dispatch
    middlewares.forEach(middleware=>{
      dispatch = middleware(store)(dispatch)
    })
    return Object.assign({},store,dispatch)
  }
  // 调用方法 
  // 返回经过middleware包装过的含有新dispatch的store
  applyMiddleware(store,[logger,crashReporting])
  ```



