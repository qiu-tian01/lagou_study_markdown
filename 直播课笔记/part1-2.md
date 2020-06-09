- 函数式编程
  - lodash和lodash/fp模块

    lodash中不是所有方法都是纯函数，lodash/fp中函数都是纯函数

    ```js
    //lodash
    reverse()
    ```

  - 函子在开发中的实际使用场景

    - 作用是控制副作用，异常处理，异步任务

    - folktale提供了基础的函子

    - 柯里化的概念和用法

      柯里化 ： 把多个参数的函数转换成只有一个参数的函数，可以给函数组合提供细粒度的函数

      应用：

      - ​	vue源码

      - ​	固定不常使用的参数

        ![image-20200603203321270](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200603203321270.png)

        ![image-20200603203504742](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200603203504742.png)

      - 延迟执行

      ```js
      //模拟bind
      Function.prototype.mybind = function(context,...args){
          return (...rest) =>{
              return this.call(context,...args,...rest)
          }
      }
      
      function fn(a,b,c){
          return a+b+c;
      }
      
      const func = fn.mybind(this,1,2)
      console.log(func(3))
      ```

      柯里化属于闭包，需要手动去释放，不释放会导致内存泄漏

    - ![image-20200603210654686](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200603210654686.png)

      

- js性能优化

![image-20200603212406829](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200603212406829.png)

![image-20200603213737028](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200603213737028.png)

<img src="C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200603213813945.png" alt="image-20200603213813945" style="zoom:80%;" />

- 其他问题

  - 原型，原型链

  ![image-20200603215506959](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200603215506959.png)

  ![image-20200603215519733](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200603215519733.png)

  ![image-20200603220031606](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200603220031606.png)

- ![image-20200603220207450](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200603220207450.png)

  

- 防抖和节流

lodash防抖