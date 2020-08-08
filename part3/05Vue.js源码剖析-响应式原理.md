- ### 01目标

  - Vue.js的静态成员和实例成员初始化过程
  - 首次渲染的过程
  - 数据响应式原理、

  

- ### 02 目录结构
  - flow ： javaScript的静态类型检查器

    - Flow的静态类型检查错误时通过静态类型推断实现的
      - 文件开头通过//@flow或者/*@flow*/声明

    ```
    /*@flow*/
    
    function add (n:number): number {
    	return n*n
    }
    ```



### 03 调试

- Vue的打包工具：Rollup
  - Rollup比webpack更轻量
  - webpack把所有文件当做模块，Rollup只处理js文件，所以更适合Vue.js这样的库中使用
  - Rollup不会生成冗余的代码
- 在package.json中命令

```
//dev命令
"dev": "rollup -w -c scripts/config.js --sourcemap --environment TARGET:web-full-dev",
// -w是监视，-c是设置配置文件配置 --environmen设置环境变量 --sourcemap生成代码地图
```



### 04 Vue的不同构建版本

在源码中使用npm run build去打包所有版本的vue

|                            | UMD                | CommonJS              | ES Module          |
| -------------------------- | ------------------ | --------------------- | ------------------ |
| Full（包含编译器和运行时） | vue.js             | vue.common.js         | vue.esm.js         |
| Runtime-only（运行时）     | vue.runtime.js     | vue.runtime.common.js | vue.runtime.esm.js |
| Full（production）         | vue.min.js         |                       |                    |
| Runtime-only（production） | vue.runtime.min.js |                       |                    |



- 完整版 ： 同时包含编译器和运行时版本
- 编译器 ： 用来将模板字符串编译为JavaScript渲染函数的代码，体积大，效率低
- 运行时 ：用来创建Vue实例，渲染并处理虚拟DOM等代码，体积小，效率高。基本上就是出去编译器的代码  



- UMD：UMD版本通用的模块版本，支持多种模块方式。vue.js默认文件就是这个版本
- CommonJS:CommonJS版本用来配合老的打包工具，如webpack1，Browserify
- ES Module ： 从2.6开始Vue会提供两个ES  Module(ESM)的构建文件，为现代打包工具提供的版本
  - ESM 格式被设计为可以做静态分析，所以打包工具可以利用这一点来进行tree-shaking，并将用不到的代码排除出最终的包



利用Vue-cli创建的Vue引用的是ES Module的运行时版本vue.runtime.esm.js



因为运行时版本体积小，效率高，所以建议使用运行时版本



### 05 寻找入口文件

- Vue.$mount在什么时候调用 ？ 

是在构造函数Vue调用init方法时调用的

![image-20200726214040089](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200726214040089.png)



- 当创建Vue实例时同时设置了template和render会执行谁？

只会执行render，代码中是判断有无render，如果没有才会设置template变为render函数



- 总结
  - el不能使body或者html标签
  - 如果没有render，把template转换成render函数
  - 如果有render方法，直接调用mount挂载DOML



### 06 Vue.js 初始化的过程

四个导出Vue的模块

- src/platforms/web/entry-runtime-with-compiler.js
  - web平台相关的入口
  - 重写了平台相关的$mount方法
  - 注册了Vue.compile()方法，传递一个HTML字符串返回render函数
- src/platforms/web/runtime/index.js
  - web平台相关
  - 注册平台相关的全局指令:v-model，v-show
  - 注册和平台相关的全局组件: v-transition,v-transition-group
  - 全局方法
    - _ patch _ : 把虚拟DOM转换为真实DOM
    - $mount : 挂载方法
- src/core/index.js
  - 与平台无关
  - 设置了Vue的静态方法，initGlobalAPI(Vue)
- src/sore/instance/index.js
  - 与平台无关
  - 定义了构造函数，调用了this._init(options)方法
  - 给Vue中混入了常用的实例成员



### 07 看源码时遇到的两个问题

- 遇到红字报错，因为vscode检查flow语法为ts语法，所以报错

解决 ： 

```
// vscode > 文件 > 首选项 > 设置 > settings.json

//设置不检查JS的语法问题，放置flow报错
"javascript.validate.enable": false,
```



- 遇到泛型，不高亮显示

  使用插件Babel JavaScript插件



### 08 Vue初始化静态成员

在Vue/src/core/global-api/index.js中



### 09 Vue初始化- 实例成员

在Vue/src/core/instance/index.js中



### 10 Vue初始化 - init

在Vue/src/core/instance/init.js 中



### 11 调试Vue初始化过程



### 12 首次渲染过程

Vue中三种watcher，一种是渲染watcher，还有计算属性和监听器watcher。



### 13 首次渲染过程总结

首次渲染过程 

1. Vue初始化，初始化实例成员，静态成员
2. 初始化结束后调用new Vue()
3. 在new  Vue()中调用this._init()方法，这个方法相当于入口
4. 在this._init()中定义了vm.$mount(),这里有两个$mount,一个是在入口文件(src/platform/entry-runtime-with-complier.js)的$mount,这里的作用是帮我们把模板编译成render函数,这里它会判断有无render函数，如果没有就把模板变异成render函数，如果没有模板就把el中的内容编译为模板，再把模板编译成render函数。这里是通过complieToFunction()生成render()函数，再将render存储到option.render中
5. 再去调用runtime中的vm.$mount()方法(src/platform/web/runtime/index.js),会重新获取el，因为运行时版本不会获取el，在mountComponent()方法中重新获取el
6. 再调用mountComponent(this,el) 方法(src/core/insatance/lifecycle.js),这里面会判断是否有render函数，如果没有但是传入了模板，并且当前是开发环境的话就会发送一个警告，告诉我们运行时版本不支持编译器
7. 接着触发了beforeMount
8. 定义了updateComponent，这里只是定义了这个方法，并调用了vm.update和vm.render方法，vm._render渲染，渲染虚拟DOM,vm._update更新，将虚拟DOM转换成真实DOM
9. 创建了Watcher实例
10. 触发mounted
11. 最后返回实例，return vm

![image-20200728161825419](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200728161825419.png)



### 14 数据响应式原理 - 响应式处理入口

通过查看源码解决以下问题

- vm.msg = { count : 0 }，重新给属性赋值，是否是响应式的？
- vm.arr[0] = 4,给数组元素赋值，视图是否会更新
- vm.arr.length = 0, 修改数组的length，视图是否会更新
- vm.arr.push(4),视图是否会更新

响应式处理的入口:

​	，observe（）方法，其中最关键的就是创建了一个Observer对象

![image-20200728163050870](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200728163050870.png)



### 15 数据响应式原理 -- Observer

在src/core/observer/index.js中

主要做将实例挂载到对象的_ob_属性，判断是数组还是队形，进行分开处理

![image-20200728174132782](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200728174132782.png)



### 16  数据响应式原理 -- defineReactive

defineReactive方法视为一个对象定义一个响应式的属性



### 17 数据响应式原理 -- 依赖搜集



### 18 数据响应式原理 -- 数组

这里hasProto是看浏览器是否有_ proto _ 属性，就是解决浏览器兼容问题

![image-20200728211618836](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200728211618836.png)



Vue源码中修改了会改变数组的方法，如push，pop等

我们通过索引的方式改变数组元素时不触发更新

![image-20200729151135552](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200729151135552.png)

改变数组长度也不会触发响应式

![image-20200729151303935](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200729151303935.png)

在处理响应式的时候并没有去遍历数组的每一个属性，而是遍历了所有元素，把元素是对象的转换为了响应式对象，所以这就是通过索引方式改变不会触发响应式更新的原因，我们并没有处理数组对象的属性。



想去操作数组的元素可以使用vm.arr.splice方法



### 19 数据响应式原理 Watcher 

Watcher 分为三类，computed Watcher 用户Watcher 和 渲染Watcher ，前两个Watcher都是在init State的时候初始化的



vm._render()的作用就是根据用户传入的render或者编译的render生成虚拟dom

vm._update()就是对比新老虚拟dom，并更新视图，生成真实dom



###  20 动态添加一个响应式属性

```
// 使用set或$set
vm.$set(vm.arr,0,100)
vm.$set(vm.obj,'name','qt')
```



### 21 set 源码实现原理



### 22 三种类型的Wacther对象

在Vue中没有$watch的静态方法，因为$watch方法中要使用Vue实例

Watcher分三种 :计算属性Watcher，用户Watcher(侦听器),渲染Watcher

​	创建顺序 : 计算属性Wather，用户uWatcher，渲染Watcher

vm.$watch()路径在src/core/instance/state.js



### 23 nextTick

异步更新队列 -- nextTick()

- Vue更新DOM是异步执行的，批量的
  - 在下次DOM更新循环结束 之后执行延迟回调，在修改数据之后立即使用这个方法，获取更新后的DOM
- vm.$nextTick(function () {}) / vm.nextTick(function () {})

