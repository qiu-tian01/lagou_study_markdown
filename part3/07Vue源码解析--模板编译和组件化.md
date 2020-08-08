### 01 模板编译介绍

- Vue2.x 使用VNode描述视图以及各种交互，用户自己编写VNode比较复杂
- 用户只需要编写类似HTML的代码-Vue.js模板，通过编译器将模板转换为返回VNode的render函数
- .vue文件会被webpack在构建股票还曾中转换成render 函数



### 02 Vue Template Explorer

在Vue2.x中模板内尽量不要有空行，而Vue3.x中进行了优化





### 03 模板编译过程 -- baseCompile-AST

什么是抽象语法树 ？ 

- 抽象语法树简称AST
- 使用对象的形式描述树形的代码结构
- 此处的抽象语法树是用来描述树形结构的HTML字符串



### 04 组件化回顾

- 一个Vue组件就是一个拥有预定义选项的一个Vue实例
- 一个组件可以组成页面上一个功能完备的区域，组件可以包含脚本，样式，模板



### 05 组件注册方式

- 全局组价

```
const Comp = Vue.compnent ('comp',{
	template : '<div>Hello Component</div>'
})

const vm = new Vue({
	el : '#app',
	render (h) {
		returnn h(Comp)
	}
})
```



- 局部组件



Vue.extend



