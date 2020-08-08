### 01 虚拟DOM概念回顾

- 虚拟DOM （Virtual DOM）是使用JavaScript对象描述真实DOM
- Vue.js中虚拟DOM借鉴Snabbdom，并添加了Vue.js的铁兴
  - 比如 ： 指令和组件机制



为什么要使用虚拟DOM

1. 因为要避免直接操作DOM，提高开发效率，避免浏览器兼容
2. 作为一个中间层可以跨平台，如如week可以跨移动端
3. 虚拟DOM不一定可以提高性能
   1. 首次渲染的时候会增加开销
   2. 复杂视图情况下提升渲染性能



### 02 虚拟DOM研究内容

h函数就是vm.$createElement(tag,data.childrren.normalizeChildren)

- tag
  - 标签名称或者组件对象
- data
  - 描述tag，可以设置DOM的属性或者标签的属性
- children
  - tag中的文本内容或者子节点



h函数的返回结果是VNode对象

- ​	VNode的核心属性
  - tag
  - data
  - children
  - text
  - elm
  - key



### 03 整体过程分析



### 04 VNode的创建过程 -- createElement



### 05 在patch函数中的createElement

createElement的 作用是把vnode转换为真实dom，然后 挂载在dom树上









​	