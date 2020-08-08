### 01 目标

- 了解虚拟DOM，以及虚拟DOM的作用
- Snabbdom的基本使用
- Snabbdom的源码解析



### 02 虚拟DOM

虚拟DOM是由普通的JS对象来描述DOM对象，因为不是真实的DOM对象，所以叫做虚拟DOM。

创建虚拟DOM的成本要比真实DOM低很多



### 03 为什么使用Virtual DOM

- 手动操作DOM麻烦，还需要考虑浏览器兼容性问题。
- 为了简化视图的操作我们可以重用模板引擎，但是模板引擎没有解决跟踪状态变化的问题，所以Virtual DOM出现了
- Virtual DOM 的好处是当状态改变时不需要立即更新DOM，只需要创建一个虚拟描述DOM，Virtual DOM 内部将弄清楚如何有效(diff)的更新DOM
- 虚拟DOM可以维护程序的状态，跟踪上一次的状态
- 通过比价前后两次状态的差异更新真实DOM



### 04 虚拟DOM的作用和虚拟DOM库

- 维护视图和状态的关系
- 复杂视图情况下提升渲染性能
- 除了渲染DOM以外，还可以实现SSR（Nuxt.js），原生应用(week)，小程序等



虚拟DOM开源库

- Bnabbdom
  - Vue2.0内部使用的Birtual DOM 就是改造的Bnabbdom
  - 
- virtual-dom



### 05 Snabbdom基本使用

- 打包工具使用parcel
- 安装parcel

```
yarn add parcel-bundler
```

- package.json

```
 "scripts": {
    "dev" : "parcel index.html --open",
    "build" : "parcel build index.html"
  },
```



### 06 导入Snabbdom

安装

```
yarn add snabbdom
```



### 07 代码演示

```js
import { init, h } from 'snabbdom'

//1. hello world

//返回值 ： patch函数，作用对比两个vnode的差异更新到真实dom
let patch = init([])
//第一个参数 : 标签+选择器
//第二个参数,如果是字符串的话就是标签中的内容
let vnode = h('div#container','Hello Wolrd')
let app = document.querySelector('#app')

//第一个参数可以是DOM元素，内容回吧DOM元素转换为Vnode
// Vnode
//返回值 ： Vnode
let oldValue = patch(app, vnode)

//架设的时刻
vnode = h('div','Hello Snabbdom')
patch(oldValue,vnode)

//2. div 中放置子元素h1,p
```



```

import { init, h } from 'snabbdom'

let patch = init([])

//2. div 中放置子元素h1,p
let vnode = h('div#container',[
    h('h1','Hello Snabbdom'),
    h('p','这是p标签')
])

let app = document.querySelector('#app')

let oldVnode = patch(app, vnode)

setTimeout(()=>{
    vnode = h('div#container',[
        h('h1','Hello World'),
        h('p','Hello P'),
    ])
    patch(oldVnode,vnode)

    //清空页面上的内容 --错误
    // patch(oldVnode,null)
    patch(oldVnode,h('!'))
},2000)
```



### 08 Snabbdom 中的模块

Snabbdom的核心库并不能处理元素的属性/样式/事件等，如果需要处理的话，可以用模块



常用模块

- 官方提供了6个模块
  - attribute
    - 设置DOM元素的属性，使用setAttribute()
    - 处理布尔类型的属性
  - props
    - 和attributes模块相似，设置DOM元素的属性 element[attr] = value
    - 不处理布尔类型的属性
  - class
    - 切换类样式
    - 注意：给元素设置类样式是通过sel选择器
  - dataset
    - 设置data-*的自定义属性
  - eventlisteners
    - 注册和移除事件
  - style
    - 设置行内样式,支持动画
    - delayed/remove/destroy

模块的使用

- 导入需要的模块
- init中注册模块
- 使用h()函数创建Vnode的时候，可以把第二个参数设置为对象，其他参数往后移



```
import {
    init,
    h
} from 'snabbdom'

//1导入模块
import style from 'snabbdom/modules/style';
import eventlisteners from 'snabbdom/modules/eventlisteners';
//2注册事件
let patch = init([
    style,
    eventlisteners
])

//使用h函数第二个参数传入模块需要的数据
let vnode = h('div', {
    style: {
        backgroundColor: 'red'
    },
    on: {
        click: eventHandler
    }
}, [
    h('h1', 'Hello Snabbdom'),
    h('p', '这是p标签')
])


function eventHandler() {
    console.log('点击我了')
}

let app = document.querySelector('#app')
patch(app, vnode)
```



### 09 Snabbdom 源码解析

如何学习源码

- 先宏观了解
- 带着目标看源码
- 看源码的过程要要求不求甚解
- 调试
- 参考资料



Snabbdom的核心

- 使用h()函数闯将JS对象（vnode）描述真实DOM
- init()设置模块，创建patch()
- patch()比较新旧两个Vnode
- 把变化的内容更新到真实DOM树上



### 10 h函数

Snabbdom中的h()函数是用来创建Vnode

函数重载

- 概念
  - 参数个数或类型不同的函数
  - JS中没有重载的概念
  - TS中有重载，不过重载的实现还是通过代码调整参数



h函数的核心就是调用vnode函数，返回一个虚拟节点



### 11 必备快捷键

1.  查看变量的定义，按f12
2. ALT+左箭头返回跳转过来的位置
3. 还可以使用ctrl+鼠标左键



### 12 vnode

vnode函数中返回的是虚拟dom的信息

![image-20200719172517229](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200719172517229.png)



### 13 Vnode渲染真是DOM

- 在snabbdom.ts中
  - patch(oldVnode,newVnode)
  - 打补丁，把新节点中变化的内容渲染到真实DOM，最后返回新节点作为下一次处理的纠结点
  - 对比新旧VNode是否相同节点(节点中的key和sel相同)
  - 如果不是相同节点，删除之前的内容，重新渲染
  - 如果是相同节点，再判断新的Vnode是否有text,如果有并且oldVnode的text不同，直接更新文本内容
  - 如果新的Vnode有children，判断子节点是否有变化，判断子节点的过程使用的就是diff算法
  - diff过程进行同层级比较



### 14 init函数

init函数有两个参数，第一个参数是模块名，第二个参数为domAPI

![image-20200719201551232](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200719201551232.png)





### 15 patch函数 

![image-20200719211234421](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200719211234421.png)

