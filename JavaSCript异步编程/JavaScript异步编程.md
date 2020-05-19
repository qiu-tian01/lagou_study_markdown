### 1. 概述

JavaScript采用单线程模式。因为javaScript是运行在浏览器端的脚本语言，目的是实现页面上的动态交互，实现页面动态交互的核心就是dom操作，所以采用单线程。避免多线程同步问题。

JS执行环境中负责执行代码的线程只有一个。

JavaScript将任务的执行模式分为同步模式和异步模式。

### 2. 同步模式

同步模式指的就是代码中任务一次执行，后一个任务要等前一个任务结束后执行。

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







