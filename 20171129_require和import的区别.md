今天同事问我require和import的区别   
其实这个我也不确定  

 
我的观点  
1.import是 ES6 Module 的语法,是ES2015标准,require是CommonJS标准  
2.CommonJS 还是 ES6 Module 输出都可以看成是一个具备多个属性或者方法的对象  
3.default 是 ES6 Module 所独有的关键字，    
  export default fs 输出默认的接口对象，  
  import fs from 'fs' 可直接导入这个对象  
4.ES6 Module 中导入模块的属性或者方法导出后可继续修改再次导出（只认最后一次导出）  
  而 CommonJS 则是一旦导出修改后再导出则不起作用（只认第一次导出）  
5.import和require里面的代码都会先执行一次
（简单讲就是：两种方式引入的console都会执行，立即执行函数也都会执行）

***部分内容来自知乎***
作者：寸志
链接：https://www.zhihu.com/question/56820346/answer/150724784
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
