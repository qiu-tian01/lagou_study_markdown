#### 1. ECMAScript描述和准备工作

​	[ecma地址](http://www.ecma-international.org/ecma-262/6.0/) 

- ES2015新特性

- - 解决原有语法上一些问题和不足（let和const块级作用域）
  - 对原有语法进行增强（解构）
  - 全新的对象，全新的方法，全新的功能(promise)
  - 全新的数据类型和数据结构(set,map,symbol)

- 准备工作
  - 环境为Node 12.13.0
  - 工具使用Nodemon

#### 2. let与const

- ##### 块级作用域

- 作用域 ： 某一成员能够起作用的范围

- 在es2015之前只有两种作用域

- - 全局作用域
  - 函数作用域

在之前在块中定义的成员在外部也可以访问到

```js
if(true){   var foo = 'foo' } console.log(foo) //foo
```

- es2015中新增块级作用域

用let声明的变量只能在所声明的代码块中被访问到。用let在块级作用域内定义的成员在外部无法访问

```js
if(true){   let foo = 'foo' } console.log(foo) //ReferenceError: foo is not defined
```

- 块级作用域与函数声明

  - ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。（浏览器没有遵守这个规定）

    ```
    //非法情况
    / 情况一
    if (true) {
      function f() {}
    }
    
    // 情况二
    try {
      function f() {}
    } catch(e) {
      // ...
    }
    ```

    

  - ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于`let`，在块级作用域之外不可引用。

    ```
    // ES5 环境
    // ES5 环境
    function f() { console.log('I am outside!'); }
    
    (function () {
      function f() { console.log('I am inside!'); }
      if (false) {
      }
      f();
    }());
    //会得到I am inside!。原因是因为函数被提升到头部
    ```

    ```js
    // ES5 环境
    // 浏览器的 ES6 环境
    function f() { console.log('I am outside!'); }
    
    (function () {
      if (false) {
        // 重复声明一次函数f
        function f() { console.log('I am inside!'); }
      }
      f();
    }());
    // Uncaught TypeError: f is not a function
    ```

    考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。

    ```js
    // 块级作用域内部的函数声明语句，建议不要使用
    {
      let a = 'secret';
      function f() {
        return a;
      }
    }
    
    // 块级作用域内部，优先使用函数表达式
    {
      let a = 'secret';
      let f = function () {
        return a;
      };
    }
    ```

- ##### let

- 循环计数器

  ```js
  for(var i = 0;i<3;i++){   for(var i = 0;i<3;i++){     console.log(i)   } } //0 //1 //2
  
  for(let i = 0;i<3;i++){   for(var i = 0;i<3;i++){     console.log(i)   } } //SyntaxError: Identifier 'i' has already been declared
  
  for(var i = 0;i<3;i++){   for(let i = 0;i<3;i++){     console.log(i)   } } 
  // 0 // 1 // 2 // 0 // 1 // 2 // 0 // 1 // 2
  ```

- 事件的处理函数

  ```js
  var element = [{},{},{}] 
  for(var i = 0;i<element.length;i++){
      element[i].onclick = function(){        
          console.log(i)    
  	} 
  } 
  lement[i].onclick()
  ```

此时代码i因为是全局变量，所以已经变为3，所以永远是第三个

解决思路

1. 闭包（原理就是通过函数作用域去摆脱全局作用域的影响）

   ```
   var element = [{},{},{}] 
   for(var i = 0;i<element.length;i++){
   	element[i].onclick = function(i){        
   		return function(){           
           	console.log(i)        
           }   
   	}
   } 
   element[i].onclick()
   ```

2. 使用let(原理也是闭包，因为当执行click事件时i已经被销毁掉)

   ```js
   for(let i = 0;i<element.length;i++){    
       element[i].onclick = function(){        
           console.log(i)    
       }
   } 
   element[i].onclick()
   ```

for循环中有两层作用域，一个是括号内的作用域，一个是花括号内的作用域

- let相比较于var

  - let声明不会被提升
  - let不允许重复声明

- #### const

const被用来声明只读的常量

const不能被修改的意思是不能让它指向新的内存地址，并不是说不能修改常量中的属性成员

- 暂时性死区

  定义 ： 使用let命令声明变量之前，该变量都是不可用的。在语法上成为暂时性死区

  ```
  console.log(tmp)// ReferenceError
  let tmp = 'tmp'
  ```

  

#### 3. 数组解构

ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值。这称为解构。

- ##### 数组解构（根据数组位置去解构）

  ```js
  const arr = [100,120,130];
  const [a,b,c] = arr
  console.log(a,b,c)
  //100 120 130
  ```

  ```js
  let [ , , third] = ["foo", "bar", "baz"];
  third // "baz"
  ```

  ```js
  let [head, ...tail] = [1, 2, 3, 4];
  head // 1
  tail // [2, 3, 4]
  
  ```

  ```js
  let [x, y, ...z] = ['a'];
  x // "a"
  y // undefined
  z // 
  ```

- ##### 对象解构（根据对象的属性名）

  ```
  const obj = {name : 'qqq',age:18}
  const { age } = obj;
  console.log(age)
  //18
  ```

  两者没有匹配到的值都会返回undefind

  在对象的解构赋值时有可能会发生与之前定义的变量名冲突，所以使用重命名的方式

  ```js
  //对象的重命名
  const obj = {name : 'qqq',age:18}
  const age = 20 
  const { age: myAge = 'a' } = obj;
  
  console.log(age,myAge)
  ```

  具体用法：

  1. 将console解构

  ```js
  const { log } = console;
  log('11')
  log('12')
  ```

   2. 交换位置

      ```js
      let x = 1;
      let y = 2;
      
      [x, y] = [y, x];
      ```

   3. 提取 JSON 数据

      ```js
      let jsonData = {
        id: 42,
        status: "OK",
        data: [867, 5309]
      };
      
      let { id, status, data: number } = jsonData;
      
      console.log(id, status, number);
      // 42, "OK", [867, 5309]
      ```

#### 4. 模板字符串

- 支持多行子字符串

- 允许通过插值表达式的方式在字符串中嵌入数值，也可以加入js表达式

- 带标签的模板字符串

  ```js
  const name = 'qqq'
  const age = 18
  
  function tag(arr){
      console.log(arr)
  }
  //[ '', ' age is ', '' ]
  const str = tag`${name} age is ${age}`
  ```

  这里主要的用处是对字符串加工，也可以做中英文转换，也可以做小型模板引擎

#### 5. 字符串的扩展方法

- includes()：返回布尔值，表示是否找到了参数字符串。

- startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
- endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
- `padStart()`用于头部补全
- `padEnd()`用于尾部补全
- `trimStart()`消除字符串头部的空格
- `trimEnd()`消除尾部的空格

#### 6. 默认传参与剩余参数

ES6允许为函数的参数设置默认值

```js
function log(x, y = 'World') {
  console.log(x, y);
}
log('Hello') // Hello World

```

展开参数

```
function log(...value){
	...
}
```

注意：展开参数要放在参数最后

#### 7. 数组扩展

- 展开数组

  - 之前方案

    ```js
    let arr = [100,110,120]
    console.log.apply(console,arr)
    ```

  - 现在

    ```
    console.log(...arr)
    ```

#### 8.箭头函数

写法：

```
const fun = v => v;
```

注意:

	1. 函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。

   	2. 不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。
   	3. 不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数（...）代替。
   	4. 不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。

#### 9. proxy

proxy是在目标对象之前架设了一层“拦截”，外界对该对象访问，都要先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

```js
let proxy = new Proxy(target,handlr)
```

target参数是要拦截的目标对象。handlr用来定制拦截的行为。

```
let proxy = new Proxy({},{
	get : function(target, propKey,proxy){//参数分别是目标对象，属性名和proxy实例本身
		return 100;
	}
})
```

1. get（）使用与拦截某个属性的读取操作。接受三个参数，一次为目标对象，属性名和Proxy实例本身。

2. set() 用来来接某个属性的赋值操作。可以接受四个参数，一次为目标对象，属性名，属性值和Proxy实例本身。这个方法可以用来做数据校验。 

   ```
   const person = {
       name : 'qt',
       age : 18
   }
   
   const personProxy = new Proxy(person,{
       get (target,property) {
           console.log(target,property)
       },
       set (target,propty,value) {
           console.log(propty,value)
           if(propty === 'age'){
               if(value > 20){
                   console.log('不能超过20')
               }
           }
       }
   })
   
   console.log(personProxy.hhh)
   
   personProxy.age = 22
   ```

3. apply()拦截函数的调用，call，apply操作。接受三个参数，目标对象，目标对象的上下文对象（this）和目标对象的参数数组。

4. Proxy的this问题。

   Proxy代理的情况下，目标对象内部的this关键字会指向Proxy代理。

   ```js
   const target = {
       m: function () {
         console.log(this === proxy);
       }
     };
     const handler = {};
     
     const proxy = new Proxy(target, handler);
     
     target.m() // false
     proxy.m()  // true
   ```

5. deleteProperty 用来拦截delete操作



Proxy与Object.definProperty()对比。

definProperty只能监视属性的读写。

	1. Propxy可以监视到其他操作

   	2. 对于数组对象的监视
   	3. Proxy是以非侵入的方式监管了对象的读写

#### 10. Reflect

概述：Reflect为ES6是为了操作对象而提供的全新api。Reflect相当于一个静态类。

意义: 提供了一套用于操作对象的统一的api

静态方法 ：

| 静态方法                                                | 描述                                                         |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| Reflect.get(target,name,receiver)                       | 返回target对象的name属性，如果没有该属性则返回undefind。     |
| Reflect.set(target,name.value.receiver)                 | 设置target对象的name属性等于value                            |
| Reflect.has(obj,name)                                   | 方法对应name in obj 中的in运算符                             |
| Reflect.deleteProperty()                                | 对应delete                                                   |
| Reflect.constructor(target,args）                       | 对应new target(...args)                                      |
| Reflect.getPropterOf(obj)                               | 用于兑取对象的_propto_属性，对应Object.getProptypeOf(obj)    |
| Reflect.setPropterOf(obj,newProto)                      | 用于设置目标对象的原型（prototype），对应`Object.setPrototypeOf(obj, newProto)`方法。它返回一个布尔值，表示是否设置成功。 |
| Reflect.apply(func, thisArg, args)                      | `Reflect.apply`方法等同于`Function.prototype.apply.call(func, thisArg, args)`，用于绑定`this`对象后执行给定函数。 |
| Reflect.defineProperty(target, propertyKey, attributes) | `Reflect.defineProperty`方法基本等同于`Object.defineProperty`，用来为对象定义属性。 |
| Reflect.getOwnPropertyDescriptor(target, propertyKey)   | `Reflect.getOwnPropertyDescriptor`基本等同于`Object.getOwnPropertyDescriptor`，用于得到指定属性的描述对象 |
| Reflect.isExtensible (target)                           | `Reflect.isExtensible`方法对应`Object.isExtensible`，返回一个布尔值，表示当前对象是否可扩展。 |
| Reflect.preventExtensions(target)                       | `Reflect.preventExtensions`对应`Object.preventExtensions`方法，用于让一个对象变为不可扩展。它返回一个布尔值，表示是否操作成功。 |
| Reflect.ownKeys (target)                                | `Reflect.ownKeys`方法用于返回对象的所有属性，基本等同于`Object.getOwnPropertyNames`与`Object.getOwnPropertySymbols`之和。 |

 

```
const obj = {
    name : 'qt',
    age : 18
}

console.log(Reflect.get(obj,'name')) //qt
console.log(Reflect.set(obj,'like','eat'))//true
console.log(obj)//{ name: 'qt', age: 18, like: 'eat' }
console.log(Reflect.has(obj,'name'))//true
console.log(Reflect.has(obj,'height'))//false
```

#### 11. class类

```
class Person {
    constructor (x,y){
        this.name = x
        this.age = y
    }

    say (){
        console.log(`${this.name} age is ${this.age}`)
    }
}

const person = new Person('qt',18)
person.say();
```



#### 12.静态方法

​	static关键词定义静态方法

​	在静态方法内部this指向不是指向实例对象，而是指向当前的类型。

#### 13.对象的扩展

1. Object.is() 判断两个值是否相等

```js
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

2. Object.assign()方法用于对象的合并，将源对象的所有可枚举属性，复制到目标对象。

   如果目标对象与源对象有同名属性，后面的属性会覆盖前面的属性。

   Object.assign实现的是浅拷贝。

   ```
   const target = { a: 1, b: 1 };
   
   const source1 = { b: 2, c: 2 };
   const source2 = { c: 3 };
   
   Object.assign(target, source1, source2);
   target // {a:1, b:2, c:3}
   ```

   

3. Object.getOwnPropertyDescriptors()

   该方法会返回某个对象属性的描述对象。

4. Object.keys()返回一个数组，成员是参数对象自身的（不含继承）所有可遍历属性的键名。

5. Object.values()返回一个数组，成员是参数对象自身的（不含继承）所有可遍历属性的键值。

6. Object.entries()返回一个数组，成员是参数对象自身的（不含继承）所有可遍历属性的键值对数组。

#### 14 类的继承

​	使用extends 关键字实现继承。

​	super关键字会生成一个空对象，作为context来调用父类的constructor，返回this对象，作为子类constructor的context继续调用构造函数。

​	super方法用在构造函数中，必须在使用this之前调用。

​	context：执行上下文 constructor：构造函数

```
class Student extends Person {
    constructor(name,number){
        super(name);
        this.number = number;
    }
     hello () {
         super.say();
         console.log(`${this.name} number is ${this.number}`)
     }
}

const jack = new Student('jack',1)
jack.hello()jack number is 1

```

#### 15 Set

Set是ES6提供的新的数据结构。它的成员都是唯一的。

Set本身是一个构造函数。用来生成Set数据结构。

```
const set = new Set([1,2,2,3,5,6,6])
console.log(set)//Set { 1, 2, 3, 5, 6 }
```

Set实例的属性和方法

| 实例属性    | 描述                          |
| ----------- | ----------------------------- |
| constructor | 构造函数，默认就是`Set`函数。 |
| size        | 返回`Set`实例的成员总数。     |

| 实例方法      | 描述                                           |
| ------------- | ---------------------------------------------- |
| add(value)    | 添加某个值，返回 Set 结构本身。                |
| delete(value) | 删除某个值，返回一个布尔值，表示删除是否成功。 |
| has(value)    | 返回一个布尔值，表示该值是否为`Set`的成员。    |
| clear()       | 清除所有成员，没有返回值。                     |

| 遍历方法  | 描述                     |
| --------- | ------------------------ |
| keys()    | 返回键名的遍历器         |
| values()  | 返回键值的遍历器         |
| entries() | 返回键值对的遍历器       |
| forEach() | 使用回调函数遍历每个成员 |

#### 16 Map

| 遍历方法  | 描述                   |
| --------- | ---------------------- |
| keys()    | 返回键名的遍历器。     |
| values()  | 返回键值的遍历器。     |
| entries() | 返回所有成员的遍历器。 |
| forEach() | 遍历 Map 的所有成员。  |

















