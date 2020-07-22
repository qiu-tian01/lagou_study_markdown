### 01 数据驱动

数据响应式

- 数据模型仅仅是普通的JS对象，而当我们修改数据时，视图会进行更新，避免了繁琐的DOM操作，提高了开发效率



双向绑定

- 数据改变，视图改变；视图改变，数据也随之改变
- 我们可以使用v-model在表单元素上创建双向数据绑定



数据驱动是Vue最独特的特性之一

​	开发过程中仅需要关注数据本身，不需要关心数据是如何渲染到视图



### 02数据响应式核心原理 -- Vue2.0

使用Object.defineProperty,并使用getter和setter进行拦截和赋值



### 03 数据响应式黑心原理 --Vue 3.0

使用Proxy实现。

- Proxy可以直接监听对象，而非属性。所以对象多个值时不需要遍历。
- ES6中新增，IE不支持，性能由浏览器优化，性能比defineProperty好



### 04 发布订阅模式

发布/订阅模式

- 订阅者
- 发布者
- 信号中心

我们假定一个"信号中心"，某个人物执行完成，就想信号中心"发布"一个信号，其他任务可以向信号中心"订阅"这个信号，从而知道什么时候自己可以开始执行，这就叫发布订阅。

```js
 class EventEmitter {
            constructor () {
                this.subs = Object.create(null)
            }
            //注册事件
            $on (eventType,handler) {
                this.subs[eventType] = this.subs[eventType] || []
                this.subs[eventType].push(handler)
            }
            //触发事件
            $emit (event) {  
                if(this.subs[event]){
                    this.subs[event].forEach(handler=>{
                        handler()
                    })
                }
            }
        }

        //测试
        let em = new EventEmitter()
        em.$on('click',()=>{
            console.log('click1')
        })
        em.$on('click',()=>{
            console.log('click2')
        })
        em.$emit('click')
```



### 05 观察者模式

- 观察者（订阅者）-watcher
  - update() : 当事件发生时，具体要做的事情
- 目标(发布者) - Dep
  - subs 数组: 存储所有的观察者
  - addSub()：添加观察者
  - notify() : 当事件发生，调用所有观察者的update()方法
- 没有事件中心

```js
  //发布者-目标
        class Dep {
            constructor () {
                //记录所有订阅者
                this.subs = []
            }
            //添加订阅者
            addSub (sub) {
                if(sub && sub.update){
                    this.subs.push(sub)
                }
            }
            //发布通知
            notify () {
                this.subs.forEach(sub=>{
                    sub.update()
                })
            }
        }
        //订阅者
        class Watcher {
            update () {
                console.log('update')
            }
        }

        //测试
        const dep = new Dep()
        const watcher = new Watcher()
        dep.addSub(watcher)
        dep.notify()
```



观察者模式和发布订阅模式区别

- 观察者模式：是由具体目标调度，当事件触发，Dep就会去调用观察者的方法，所以观察者模式的订阅者与发布者之间有存在依赖的
- 发布订阅模式 ： 由统一调度中心调用，因此发布者和订阅者不需要知道对象的存在

![image-20200715221533694](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200715221533694.png)



### 06 Vue响应式原理模拟 分析

- Vue：把data中的成员注入到Vue实例，并且把data中的成员转成geter/seter

  ![image-20200715230241557](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200715230241557.png)

  

### 07 Vue

Vue的功能

- 负责接收初始化的参数（选项）
- 负责把data中的属性注入到Vue实例，转换成getter、setter
- 负责调用observer监听data中所有属性的变化
- 负责嗲用complier解析指令/差值表达式

结构：

![image-20200715230539113](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200715230539113.png)

```
class Vue {
    constructor (options){
        // 1. 通过属性保存选项的数据
        this.$options = options || {}
        this.$data = options.data || {}
        this.$el = typeof options.el === 'string' ? document.querySelector(options.el) : options.el
        // 2. 把data中的成员转换成getter和setter，注入到Vue实例中
        this._proxyData(this.$data)
        // 3. 调用observer对象，监听数据的变化
        // 4. 调用complier对象，解析指令和差值表达式
    }
    _proxyData (data) {
        // 遍历data中的所有属性
        Object.keys(data).forEach(key =>{
            Object.defineProperty(this,key, {
                enumerable : true,//可枚举
                configurable : true,//可配置
                get () {
                    return data[key]
                },
                set (newValue) {
                    if(newValue === data[key]){
                        return 
                    }
                    data[key] = newValue
                }

            })
        })
        // 把data的属性注入到vue实例中
    }
}
```



### 08 observer

- 功能
  - 负责把data 选项中的属性转换成响应式数据
  - data中的某个属性也是对象，把改属性转换成响应式数据
  - 数据变化发送通知
- 结构

![image-20200715233230360](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200715233230360.png)

```
//observer.js
class Observer {
    constructor(data) {
        this.walk(data)
    }
    walk(data) { //遍历data的所有属性
        //1. 判断data是否是对象
        if (!data || typeof data !== 'object') {
            return;
        }
        //2. 遍历data对象的所有属性
        Object.keys(data).forEach(key => {
            this.defineReactive(data, key, data[key])
        })
    }
    defineReactive(obj, key, val) {//调用val而不能直接调用obj的原因是因为会发生死递归
        Object.defineProperty(obj, key, {
            enumerable: true, //可枚举
            configurable: true, //可配置
            get() {
                return val
            },
            set(newValue) {
                if (newValue === val) {
                    return
                }
                val = newValue
            }
        })
    }
}
```



### 09 Compiler

- 功能
  - 负责编译模板，解析指令/差值表达式
  - 负责页面的手册渲染
  - 当数据变化后重新渲染视图
- 结构

![image-20200716201144377](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200716201144377.png)

   ```js
class Compiler {
    constructor(vm) {
        this.el = vm.$el
        this.vm = vm
        this.compiler(this.el)
    }
    //编译模板，处理文本节点和元素节点
    compiler(el) {
        let childNodes = el.childNodes
        Array.from(childNodes).forEach(node => {
            //处理文本节点
            if (this.isTextNode(node)) {
                this.compilerText(node)
            } else if (this.isElementNode(node)) { //处理元素节点
                this.compilerElement(node)
            }
            //判断node节点，是否存在子节点，如果有子节点就递归调用compiler
            if (node.childNodes && node.childNodes.length) {
                this.compiler(node)
            }
        })

    }
    // 编译元素节点，编写指令
    compilerElement(node) {
        console.log(node.attributes)
        //遍历属性节点
        Array.from(node.attributes).forEach(attr =>{
            //判断是否是指令
            let attrNmae = attr.name
            if(this.isDirctive(attrNmae)) {
                //v-text --> text
                attrNmae = attrNmae.substr(2)
                let key = attr.value
                this.update(node,key,attrNmae)
            }
        })
        
    }
    update (node,key,attrName){//原因是避免if语句，易于维护
        let updateFn = this[attrName + 'Updater']
        updateFn && updateFn(node,this.vm[key])
    }

    //处理v-text指令
    textUpdater (node,value) {
        node.textContent = value
    }
    //处理v-model
    modelUpdater (node,value){
        node.value = value
    }

    // 编译文本节点，处理差值表达式
    compilerText(node) {
        // console.dir(node)
        // 正则匹配{{ msg }}，并提取值
        let reg = /\{\{(.+?)\}\}/
        let value = node.textContent
        if(reg.test(value)){
            let key = RegExp.$1.trim()
            node.textContent = value.replace(reg,this.vm[key])
        }

    }
    //判断元素属性是否为指令
    isDirctive(attrName) {
        return attrName.startsWith('v-')
    }
    //判断节点是否为文本节点
    isTextNode(node) {
        return node.nodeType === 3
    }
    //判断节点是否为元素节点
    isElementNode(node) {
        return node.nodeType === 1
    }
}
   ```



### 10 Dep

功能 ： 

- 收集依赖，添加观察者（watcher）
- 通知所有观察者

结构 ： 

![image-20200716215405981](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200716215405981.png)



### 11 Watcher

![image-20200716225405740](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200716225405740.png)

功能 ： 

- 当数据变化触发依赖，dep通知所有的Watcher实例更新视图
- 自身实例化的时候往dep对象中添加自己

结构 ： 

![image-20200716225449664](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200716225449664.png)





### 12 问题总结

- 问题

  - 给属性重新复制成对象，是否是响应式的？ 是
  - 给Vue实例新增一个成员是是响应式的? 不是。Vue不允许动态添加根级别的响应式属性，但是，可以使用Vue.set(object,propertyName,value)

  ![image-20200718161747452](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200718161747452.png)

  


