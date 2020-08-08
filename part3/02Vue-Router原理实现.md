### 01 Vue-router 基础问题与使用步骤



$route与$router的区别

$route是路由规则

$router是路由对象



### 02 动态路由

定义动态路由

```
//index.js
{
	path : 'detail/:id',
	name : 'Detail'
}
```

获取值

```
//1.通过路由规则获取数据
{{$route.params.id}}
//2.路由规则中开启props传参
<template>
	{{id}}	
</template>
<script>
	export default {
		name : 'Detail',
		props : ['id']
	}
	
</scripte>

```



### 03嵌套路由

加入有相同的部分，可以使用<router-view>占位‘

```js
{
	path : '/',
	component : Layout,
	children : [
		{
			name : 'index',
			path : '',//这里可以使用相对路径也可以使用绝对路径
			component : index
		},
		{
			name : 'Detail',
			path : 'detail/:id',
			props : true,
			component : ()=> import('@/views/detail.vue')//路由懒加载
		}
	]
}
```



### 04 编程式导航

- $router.push()//跳转指定路径

```js
$router.push('/') || $router.push({name : 'Home'}) 
$router.push({name : 'Home'}，params:{id:1}) //传参
```

- $router.replace()//跳转指定路径

```
$router.replace('/login')
```

- $router.go()//条咋混到指定页面

```
$router.go(-2)
```



### 05 Hash和History模式的区别

表现形式的区别

```
//Hash模式
https://xxxx.com/#/xxxxx?id=231231321
//history模式，需要后端去配置
https://xxxx.com/xxxx/23131312
```

原理区别

- Hash模式是基于锚点，以及onhashchange事件
- History模式是基于HTML5中的History API
  - history.pushSate() IE10以后才支持
  - history.relaceState()

### 06History模式

- History需要服务器的支持
- 原因是单页应用中，服务器不存在http://www.testurl.com/login 这样的地址会返回找不到该页面
- 在服务端应该出了静态资源外都返回单页应用的index.html



### 07 History 模式 在Node.js服务器配置

````js
const path = require('path')
//导入处理history模式的模块
const history = require('connect-history-api-fallback')
const express = require('express')

const app = express()
//处理history模式的中间件
app.use(histiry())
app.use(express.static(paty.join(__dirname,'../web')))

//开启服务器
app.listen(3000,()=>{
    console.log(’开启服务器‘)
})
````



### 08 history模式在nginx服务器配置

niginx服务器配置

- 官网下载ngix的压缩包
- 吧压缩包解压到C盘根目录，（注意不能有中文）
- 打来命令行，切换到目录C:\nginx-1.18.0

```js
//启动
start nginx
//重启
nginx -s reload 
//停止
nginx -s stop
```

 在nginx.conf配置中修改history配置

```
location / {
	root html;
	index index.html index.html;
	try_files $url $url/ /index.html;
}
```



### 09 Vue Router 实现原理

Hash模式

- URL中#后面的内容作为路径地址
- 监听hashchange事件
- 根据当前路由地址找到对象组件重新渲染

```

```



### 10 Vue Router 模拟实现分析

- 注册插件 Vue.use(VueRouter)
- 创建路由对象

```
const router = new VueRouter({
	routers : [
		{name : 'home',path:'/',component : homeComponent}
	]
})
```

- 创建Vue实例，注册router对象

```
new Vue({
	router,
	render: h=>h(App)
}).$mount('#app')
```

![image-20200713231439587](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200713231439587.png)



### 11 VueRouter实现过程

Vue的构建版本

- 运行时版：不支持template模板，需要打包的时候提前编译
- 完整版：包含运行时和编译器，体积比运行时版大10K左右，程序运行的时候把模板转换成render函数

Vue-router错误

```
[Vue warn]: Unknown custom element: <router-view> - did you register the component correctly? For recursive components, make sure to provide the "name" option.

found in

---> <App> at src/App.vue
       <Root>
```

解决方法一：使用完整版vue,带编译器，可以使用template

```

//vue.config.js
module.exports = {
    runtimeCompiler : true,//是否使用包含运行时编译器的Vue的构建版本，设置为true就可以在Vue中使用template选项
}
```



解决方法二 ： 运行时版本，运行时组件可以直接写render函数



```
//模拟vue-router 中history模式
let _Vue = null

export default class VueRouter {
  static install (Vue) {
    // 1. 判断当前插件是否已经被安装
    if (VueRouter.install.installed) {
      return
    }
    VueRouter.install.installed = true
    // 2. 把Vue构造函数记录到全局变量
    _Vue= Vue 
    // 3. 把创建Vue实例时候传入的router对象注入到Vue实例上
    // 混入
    _Vue.mixin({
      beforeCreate() {
        if (this.$options.router) {
          _Vue.prototype.$router = this.$options.router
          this.$options.router.init()
        }
      },
    })
  }

  constructor (options){
      this.options = options
      this.routerMap  = {}
      this.data = _Vue.observable({
          current :'/'
      })
  }

  init(){
    this.createRouterMap()
    this.initComponents(_Vue)
    this.initEvent()
  }

  createRouterMap () {
    //遍历所有的路由规则，把路由规则解析成键值对的形式，存储到routeMap中
    this.options.routes.forEach(route => {
        this.routerMap[route.path] = route.component
    });
  }

  initComponents(Vue) {
     Vue.component('router-link',{
       props : {
         to : String
       },
       render (h) {
         return h('a',{
           attrs : {
             href : this.to
           },
           on : {
             click : this.clickHandler
           }
         },[this.$slots.default])
       },
       methods: {
         clickHandler (e) {
           history.pushState({},'',this.to)//改变地址栏
           this.$router.data.current = this.to
           e.preventDefault();
         }
       },
      //  template: '<a :href="to"><slot></slot></a>'
     })
     const self = this
     Vue.component('router-view', {
       render (h) {//h帮我们创建虚拟dom
        const component = self.routerMap[self.data.current]
        
        return h(component)
       }
     })
  }

  initEvent () {
    window.addEventListener('popstate',()=>{
      this.data.current = window.location.pathname
    })
  }
}

```


