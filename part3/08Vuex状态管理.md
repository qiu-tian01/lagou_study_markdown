### 01. 组件内的状态管理流程

- state : 驱动应用的数据源
- view：以声明方式将state映射到视图
- actions : 响应在view上的用户输入导致的状态变化



### 02. 组件间的传输方式(父子组件传参)

1. 子组件中通过props接收数据
2. 父组件中给子组件通过相应属性传值

### 03.组件间通信方式(子组件给父组件传值)

1. 子组件通过this.$emit(‘事件名’,'参数')祖册事件传递参数
2. 父组件使用v-on:事件名来接收事件
3. 父组件methods内定义函数，并接收参数

### 04. 组件间通信方式(不相关组件间传值)

通过eventBus时间事件总线来完成不相关组件间传值。

1. 在eventBus.js中导出Vue实例，作用是使用实例上$on,$emit方法

2. 在使用eventBus的组件中导入bus，使用bus.$emit()方法通过事件传递参数

3. 另一个组件中引入bus，通过bus.$on()来接收参数

   ```
   bus.$on('xxx',(value)=>{
   	xxxxx
   })
   ```

   

### 05. 组件间通信方式(通过ref获取子组件)

通过$refs获取子组件的状态

ref的作用

- 在普通HTML标签上使用ref，获取到的是DOM
- 在钻进标签上使用ref，获取到的是组件实例



### 06. 简易的状态管理方案

### 07. Vuex概念回顾

- VUex是专门为Vue.js设计的状态管理库
- Vuex采用集中式的方式存储需要共享的状态
- Vuex的作用是进行状态管理，解决复杂组件通信，数据共享
- Vuex继承到了devtools中，提供了time-travel时光旅行历史回滚功能

什么情况下使用Vuex

- 大型的单页应用程序
  - 多个视图依赖统一状态
  - 来自不同视图的行为需要变更同一状态



### 08.Vuex核心概念

![image-20200907214233789](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200907214233789.png)



### 09. VueX基本代码结构

![image-20200907214658772](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200907214658772.png)

![image-20200907214720327](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200907214720327.png)



### 10. State

state是单一状态树，这里面存储所有的状态数据，并且atate是响应式的

![image-20200907221847999](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200907221847999.png)



### 11. Getter

相当于计算属性。

使用mapGetters来优化，书写结构在计算属性中

```
computed:{
	...mapGetters(['reverseMsg'])
}
```



### 12.Mutation

### 13. Action

### 14. Module





