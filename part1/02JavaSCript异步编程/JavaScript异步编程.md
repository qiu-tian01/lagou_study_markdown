### 1. 概述

JavaScript采用单线程模式。因为javaScript是运行在浏览器端的脚本语言，目的是实现页面上的动态交互，实现页面动态交互的核心就是dom操作，所以采用单线程。避免多线程同步问题。

JS执行环境中负责执行代码的线程只有一个。

JavaScript将任务的执行模式分为同步模式和异步模式。

### 2. 同步模式

同步模式指的就是代码中任务依次执行，后一个任务要等前一个任务结束后执行。

### 3.异步模式

异步模式的api不会等待这个任务的结束才开始下一个任务，开启过后立即往后执行下一个任务。后续逻辑一般会通过回调函数的方式定义。

异步调用整个过程就通过内部的消息队列和内部循环完成的。

注意：尽管JavaScript是单线程的但是浏览器并不是单线程的，所以通过js调用的api并不是单线程的，例如倒计时器中就有一个线程负责计时，到时间后会将回调放入消息队列。

这里说的同步和异步指的是运行环境提供的API是以同步或异步模式的方式工作。

### 4. 回调函数 

  由调用者定义，交给执行者执行的函数称为回调函数。

```js
function fn(callback){
	setTimeout(function(){
		callbacl()
	},1000)
}
fn(function(){
	console.log('xxx')
})
```

### 5. Promise

 Promise是一个对象 ，表示一个异步操作为成功还是失败。一开始是一个pending状态，最终有可能为Fulfilled状态和Rejected状态，对应的状态为onFulfilled和onRejected



### 6.Promise使用方法

#### 封装ajax

```
 function ajax(method, url){
     return new Promise(function (resolve,reject){
         let xhr = new XMLHttpRequest();
         xhr.open(method,url)
         xhr.responseType = "Json"
         xhr.onload = function(){
            if(this.status === 200){
                resolve(this.response)
            }else{
                reject (new Error (this.statusText))
            }
         }
         xhr.send()
     })
 }

```



### 7..Promise常见误区

Promise本质是使用回调函数方式去定义异步操作结束后所需要执行的任务。

嵌套使用Promise是使用Promise最常见的误区，正确的方法是链式调用Promise，尽可能保证异步操作的扁平化。

### 8.Promise 链式调用

then方法返回的也是一个Promsie对象。并且then方法返回的是一个全新的Promise对象，跟之前的Promise对象并不是同一个，目的就是形成一个Promise链条。

每一个then方法，都是在为上一个返回的Promise对象添加状态明确过后的回调。

如果回调中返回不是Promise对象而是一个普通的值，这个值就是下一个then方法中的参数，如果回调中没有返回任何值，则默认返回undefind。

- Promise对象的then方法会返回一个全新的Promise对象
- 后面的then方法就是在为上一个then方法返回的Promise注册回调
- 前面的then方法中回调函数的返回值会作为后面then方法回调的参数
- 如果回调中返回的是Promise，那后面then方法的回调会等待它的结束。



### 9.Promise异常处理

 Promise在onReacted回调中抛出异常。

一种常见的方法是使用Promise的catch方法注册onRejected回调。

使用catch方法还有一个好处就是它捕获的是链条上Promise的异常。

```
function async () {
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log('prtomsie')
            resolve('ok')
        },1000)
    })
}

async().then(res=>{
    console.log(res)
}).catch(err=>{
    console.log('err')
})
```



### 10.Promsie静态方法

| 静态方法          | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| Promise.resolve() | 可将现有对象转为 Promise 对象，如果接收的是一个Promise对象则该对象被原样返回 |
| Promise.reject()  | 创建一个失败的Promise对象                                    |



### 11.Promise并行执行

1. Promise.all()方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

```
const p = Promise.all([p1, p2, p3]);
```

Promise.all()方法接受一个数组作为参数，p1、p2、p3都是 Promise 实例

（1）只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。

（2）只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

2. Promise.race()方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```
const p = Promise.race([p1, p2, p3]);
```

只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数。



### 12. Promise执行时序

1. 宏任务和微任务

   回调队列中的任务称之为“宏任务”，宏任务执行过程中可以临时加上一些额外需求，可以选择作为一个新的宏任务进到队列中排队，也可以作用当前任务的“微任务”，直接在当前任务结束后立即执行。

2. Promise的回调会作为微任务执行

3. 目前绝大多数异步调用都是作为宏任务执行

4. Promsie&MutationObserver还有node中process.nextTick是微任务在本轮任务结束时执行。



### 13. Generator异步方案

- Generator 函数是一个状态机，封装了多个内部状态。

- 执行Generator函数会返回一个遍历器对象，所以它不仅是一个状态机还是一个遍历器对象生成函数。

- Generator函数有两个特征：

	1. function关键字与函数名之间有一个*号。
 	2. 函数体内部用yield表达式，定义不同的内部状态。

- 在调用Generator函数时，函数并不会马上执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，也就是遍历器对象。

  在调用next()方法后，指针移向下一个状态，一次调用next()方法直到遇到yield或者return语句为止。

- yield表达式

  yield表达式后面的表达式只有当调用next方法，内部指针指向该语句时才会执行。相当于“惰性求值”的语法功能

- next方法

  next方法可以带一个参数，作为上一个yield表达式的返回值。

### 14 Generator异步方案

```

function async () {
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log('promise')
            resolve(10)
        },1000)
    })
}
function *foo(){
    const async_value = yield async()
    console.log(async_value)
}

const generator = foo()
const result = generator.next()
result.value.then(res=>{
    const result2 = generator.next(res)
    console.log(res)
})
```

封装，生成函数执行器

```js
function co(generator) {
    const g = generator
    function handleResult(result) {
        if (result.done) return; //如果为true，则说明生成器函数结束
        result.value.then(res => {
            handleResult(g.next(data))
        }, error => {
            g.throw(error)
        })
    }
}
```

[生成函数执行器库](https://github.com/tj/co)



### 15. ASync/Await语法糖

```

function async () {
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log('prtomsie')
            resolve('ok')
        },1000)
    })
}

async function main(){
    const result = await async()
    console.log(result)
}

main()
```

