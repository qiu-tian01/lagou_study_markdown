### 01 NuxtJS介绍

- Nuxt.js是一个基于Vue.js生态的第三方开源服务端渲染应用框架
- 它可以帮我们使用Vue.js技术栈构建同构应用



### 02 Nuxt.js的使用方式

- 初始项目
- 已有的Node.js服务端项目
  - 直接把Nuxt当做一个中间件继承到Node Web Server中
- 现有Vue.js项目
  - 需要非常熟悉Nuxt.js
  - 知道百分之10 的代码改动



- Nuxt.js

  - 1.安装

  ```
  npm i nuxt
  ```

  - 2. package.json配置

  ```
  "scripts":{
  	"dev" : "nuxt"
  }
  ```

  - 根目录下创建pages文件夹，再创建index.vue，不需要配置路由，会自动扫描pages文件



### 03 路由-基本路由

Nuxt.js依据pages目录结构自动生成vue-router模块的路由配置

​	

### 04 Nuxt.js路由--路由导航

- a标签（它会刷新整个页面，不要使用）

  ```
  <a href="/">a链接</a>
  ```

  

- nuxt.js组件

  ```
  <nuxt-link to="/">首页</nuxt-link>
  ```

  

- 编程式导航

  ```
  <button @click="onClick"></button>
  
  //js
  methods : {
  	onClick () {
  		this.$router.push('/')
  	}
  }
  
  ```





### 05Nuxt中的动态路由

与Vue-router的动态路由相同

创建文件需要在文件名前加入'_'

```
_id.vue
```

使用$route.parmas,id获取id值

```
{{$route.parmas,id}}
```



### 06 嵌套路由

可以通过Vue-router的子路由创建Nuxt.js应用的嵌套路由



创建内嵌子路由，需要添加一个Vue文件，同时添加一个与改文件同名的目录用来存放子视图组件

**注意：在父组件内增加< nuxt-child >用来显示子视图内容**

比如在文件中创建user.vue，user文件夹下的文件会变成user.vue的子路由，就会形成嵌套路由



### 07 自定义路由配置

参考文档API中router

### 08 Nuxt 视图

 ### 09 异步数据 asyncData

如果想要动态页面内容有利于SEo或者是提升首屏渲染速度的时候，就在asyncData中发送请求



如果是非异步数据或者普通数据则正常初始化data中即可

![image-20200926214024592](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200926214024592.png)



