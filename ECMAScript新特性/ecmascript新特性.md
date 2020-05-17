#### 1. 对象的扩展

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



#### 2. Proxy

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



#### 2.Reflect

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

##### 3.class类

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

##### 4. 静态方法

​	static关键词定义静态方法

​	在静态方法内部this指向不是指向实例对象，而是指向当前的类型。

##### 5.类的 继承

​	使用extends 关键字实现继承。

​	super关键字会生成一个空对象，作为context来调用父类的constructor，返回this对象，作为子类constructor的context继续调用构造函数。

​	super方法用在构造函数中，必须在使用this之前调用。

​	context：执行上下文 constructor：构造函数

````

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

````

##### 5. Set

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

##### 6. Map

Map中各种类型的值都可以当做键，包括对象。

```
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"
```

| 实例属性与方法 | 描述                                                         |
| -------------- | :----------------------------------------------------------- |
| size           | 返回 Map 结构的成员总数。                                    |
| set(key,value) | 设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。因为返回的是当前的Map对象，所以可链式调用。 |
| get(key)       | 读取`key`对应的键值，如果找不到`key`，返回`undefined`。      |
| has(key)       | 返回一个布尔值，表示某个键是否在当前 Map 对象之中。          |
| delete(key)    | 删除某个键，返回`true`。如果删除失败，返回`false`。          |
| clear()        | `clear`方法清除所有成员，没有返回值。                        |

| 遍历方法  | 描述                   |
| --------- | ---------------------- |
| keys()    | 返回键名的遍历器。     |
| values()  | 返回键值的遍历器。     |
| entries() | 返回所有成员的遍历器。 |
| forEach() | 遍历 Map 的所有成员。  |

