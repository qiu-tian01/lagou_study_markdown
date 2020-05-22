### 1. TypeScript介绍

- TypeScript是JavaScript的类型的一个超集，可以变编译纯JavaScript



### 2.强类型和弱类型

​	从类型安全角度分为强类型和弱类型。强类型哟更强的类型约束，而弱类型 没有什么约束。

​	强类型语言中不允许任意的隐式类型转换。而弱类型语言则允许任意类型的隐式类型转换。 



### 3. 静态类型和动态类型

类型检查的角度分为静态类型与动态类型。

静态类型就是一个变量声明时它的类型就是明确的，声明过后，类型就不允许被修改。

动态类型则是在运行阶段才能够明确变量类型，而且变量的类型随时可以改变。

在动态类型语言中的变量没有类型，变量中存放的值时有类型的。



### 4. JavaScript类型系统特征

JavaScript是一门弱类型且动态类型的语言。这样的语言缺失了类型系统的可靠性。

JavaScript为什么设计成这样？

- 早期JavaScript应用简单
- JavaScript没有编译环节



### 5.弱类型的问题

- 必须要等到运行阶段才能发现类型上的异常。
- 传参是可能造成函数功能错误
- 造成对象属性名都变为字符串类型



### 6.强类型的优势

- 错误更早暴露
- 代码更智能，编码更准确
- 重构更牢靠
- 减少不必要的类型判断 



### 7. Flow概述

JavaScript的类型检查器，2014年由facbook'推出。

工作原理是通过添加类型注解的方式去标记变量或者参数是什么类型的

```
function sum (a:number.b:number){
	return a+b;
}
```

Flow并不要求给每个变量或参数进行注解。



### 8. Flow的使用

使用时需要在文件前面添加一个//@flow,flow才会去检查。

在使用vscode时在javascript validate关闭类型检查。

FLow的安装与使用

1. yarn add flow-bin -g
2. yarn flow init 创建.flowconfig文件
3. yarn flow 检查
4. yarn flow stop结束

```
//@flow

function sum(a : number, b : number) {
    return a + b;
}

sum('100','100')//报错
```



### 9.Flow编译移除注释

上面的代码在JavaScript运行时会报错，解决方法是自动移除掉类型注解。

- 使用官方提供的 flow-remove-types 模块

  安装 ： yarn add flow-remove-types --dev 

  使用 ： yarn flow-remove-types （去除类型注释的文件路径） -d （存放路径）

- 使用Babel的预置babel-preset-flow

  - 安装 ： 

  ```
  yarn add @babel/core @babel/cli @babel/preset-flow
  ```

  - 使用

  ```
  yarn babel src -d dist
  ```



### 10.Flow开发工具插件

vscode安装一个Flow Language Support 插件



### 11.Flow类型推断

会根据代码的使用情况来推荐参数的类型叫做类型推断

```

//@flow

function square (n) {
    return n*n //会提示错误
}

square('100')
```



### 12.Flow类型注释

可用在变量和参数和函数

```
let num : number = 100

function square (n:number) {
    return n*n 
}

```

若函数没有返回值，js默认返回undefind，所以要定义为void

```
function foo() : void{
	
}
```



### 13.原始类型

| 数据类型 | 说明                        |
| -------- | --------------------------- |
| string   | 存放字符串类型              |
| number   | 存放数字类型，NaN，Infinity |
| boolean  | 存放true和false             |
| null     | null                        |
| void     | undefind                    |
| symbol   | symbol                      |

```

// @flow

//原始类型

//JavaScript的6中原始类型

let str : string = 'string'

let num : number =  Infinity//123 // NaN

let bol : boolean = true // false 

let nul : null = null

let und : void = undefined

let sym : Symbol = Symbol()
```



### 14数组类型

```
const arr:Array <number> = [1,2,3,4]
//或者
const arr2 : number[] = [1,2,3]
```

数组规定长度

```
//元祖
const arr3:[string,number] = ['foo',123]
```



### 15 对象类型

```
const obj1 : {foo : string ,bar : number } = {foo:'qqq',bar:100}
```

在：前面添加?,表示该属性值可能会存在,也可能不存在

```
const obj2 : {foo ?: string ,bar : number } = {bar:100}
```

还有一种方式是

```
const obj3 : {[String] : string} = {}

obj3.foo = 'value'
```



### 16 函数类型

```
function foo (callback: (string,number) => void) {
    callback('qqq',100)
}
```



### 17 特殊类型

- 字面量类型 ： 限制变量必须是某一个值

```
const a : 'foo' = 'foo'
```

一般会配合联合类型使用

```
const yupe : 'success' | 'failed' = 'success'
```

- 联合类型

```
const b : number | string = 'string'
```

还可以使用type来定义联合类型

````
type StringOrNumber = string | number

const c : StringOrNumber = 'string'
````

- maybe类型，表示属性可能是某一种类型

```js
const d : ?number = undefind
```



### 18 Mixed 与Any

- mixed可接受任意类型值，是所有类型的联合类型

```js
const d : mixed = 'string'
```

- any 也可以接受任意类型值

```
const d : any = 'string'
```

两者的区别为any为弱类型，mixed为强类型的

如果想知道mixed具体的类型，就需要调用typeof方法

```
function passMixed (value:mixed){
	if(typeod value === 'string'){
		value.substr(1)
	}
	if(typeod value === 'number'){
		value * value
	}
}
```



### 19 类型小结

[Flow类型](https://www.saltycrane.com/cheat-sheets/flow-type/latest/)



### 20 FLow 运行环境API

```js
const element : HTMLElement | null = document.getElementByID('id')
```



### 21 TypeScript 概述

TypeScript是JavaScript的类型的一个超集，可以变异成纯JavaScript。

任何一种JavaScript运行环境都支持typeScript

typeScript作为一门语言功能更加强大，生态更加健全，更完善。

typeScript语言本身多了很多概念。



### 22typeScript使用

- 安装typescript

​	yarn add typescript -g

- 在node_modules中.bin下tsc负责编译ts代码
- 编译 yarn tsc （文件名 ）
- 因为ts是渐进式的，所以可以完全按照js标准编写

```
const hello = (name:string) => {
    console.log(`Hello,${name}`)
}

hello('qt')
```



### 23 typeScript配置文件

  使用ts的命令自动生成配置文件,生成一个tsconfig.json文件

```
yarn tsc --init
```

tsconfig.json文件中

```
"target": "es5",       //表示编译成es5标准语法
"module": "commonjs",    //表示使用commonjs模块规范
"outDir": "./",			//表示输出结果到的文件夹
"rootDir": "./", 		//源代码所在文件夹
"sourceMap": true,       //开启源代码映射
"strict": true,     	//开启严格模式
```

配置好后使用

```
yarn tsc
```



### 24typeScript原始类型

与flow不同的是，ts允许在string，number，boolean类型可以为空，前提是关闭严格模式，也可以使用配置文件中

````

const str : string = 'str'

const num : number = 100 //NaN Infinity

const bol : boolean = true //false

const v :  void = undefined

const n : null = null

const u : undefined = undefined

const s : symbol = Symbol()
````



### 25 typeScript标准声明库

标准库就是内置对象所对应的声明

因为symbol是es6新增的内置对象，本身也是有类型的。要把target改为es2015标准库。

解决方法为

- target改为es2015
- 使用lib选项增加标准库

```
"lib": ["es2015","DOM"],  
```

### 26typeScript中文错误提示

```
yarn tsc --locale zh-CN
```

在vscode中配置TypeScript: Locale设置为zh-CN

### 27typeScript作用域问题

创建一个立即执行作用域

```
(function () {
	const a = 123
})
```

使用export到处

```
export{}
```

### 28 Object类型

ts中Object类型指的是非原始数据类型，也就是对象，数组，函数。它并不单单指普通的对象



### 29数组类型

```
const arr : Array <number> = [1,2,3]
```

```
const arr2 : number[] = [1,2,3]
```



### 30元组类型

元组类型就是明确数量和类型的数组类型

```
const type : [number,string] = [10,'f00']
```



### 31枚举类型

- 给每个数组取一个更好理解的名字
- 一个枚举中只会存在固定的数值

使用enum 定义枚举

```
enum post {
    a = 0,
    b = 1,
    c = 2
}

```

如果不赋值的话，枚举中的属性会从0累加

```
enum post {
    a ,
    b ,
    c 
}
```

枚举的属性还可以定义成字符串，因为字符串不能累加，定义成字符串后需要给每个值手动赋值

```
enum post {
    a = 'foo',
    b = 'bar',
    c = 'pos',
}
```

- 枚举类型会入侵运行时的代码，也就是说它会影响编译后的结果。

```
//枚举类型编译成js的代码
var post;
(function (post) {
    post[post["a"] = 0] = "a";
    post[post["b"] = 1] = "b";
    post[post["c"] = 2] = "c";
})(post || (post = {}));
```

目的是让我们动态的通过枚举值获取枚举名称



### 32函数类型

可选参数需要放在最后

函数声明：

```
//可选参数
function foc1(a : number,b?: number) : string {
    return 'foo'
}

foc1(10,10)
```

```
//任意参数
function foc1(a : number,...res:number) : string {
    return 'foo'
}
```

函数表达式

```
const fun : (a:number) => string =  function func(a:number):string {
    return 'foo'   
}
```



### 33任意类型

any类型任然属于动态类型，ts不会对any做类型检查

```
function str(value:any) {
    return value
}
```

因为这个类型不安全所以尽量不使用



### 34隐式类型推断

ts会将赋值后的类型进行隐式类型推断

```
let a = 111;
a = 'foo'
```

a被推断为number类型，改为字符串后会报错

如果ts无法推断类型时会将类型设置为any

```
let b;//被推断为any
```



### 35类型断言

断言的方式有两种

- 使用as关键字

```
const nums = [100,110,120]

const res = nums.find(i => i > 100)

let num1 = res as number
```

- 使用尖括号方式

```
let num1 = <number>res 
```

类型断言并不是类型转换

### 36 接口

interfaces可以理解成一个规范。

实际应用上接口可以约定一个对象有哪些成员，成员的类型

```
interface Post {
  title: string;
  content : string
}

function printPost(post:Post) {
    console.log(post.title)
    console.log(post.content)
}

printPost({
    title : 'q',
    content : 'q'
})

```

接口就是用来约束对象的结构，编译后接口代码不会存在



### 37 接口补充

可选成员

```
interface Post {
  title: string;
  content : string
  subTitle ?: string
}
```

只读成员

```
interface Post {
  title: string;
  content : string
  subTitle summary: string
}
```

动态成员

```
interface Post {
  [prop:string] : string
}
```



### 38类

类就是描述一类具体事务的抽象特征。

```
class person {
    name : string
    age : number
    constructor (name : string,age:number) {
        this.name = name
        this.age = age
    }
}

```



### 39类访问修饰符

```
class person {
    public name : string	//公有
    private age : number //私有属性
    protected gender : boolean //受保护的，子类可访问
    constructor (name : string,age:number) {
        this.name = name
        this.age = age
        this.gender = true
    }
    
    static create(){
    
    }
}
```



### 40只读属性

使用readonly

只读属性定义后不允许再被修改

~~~
class person {
    public name : string	//公有
    private age : number //私有属性
    protected readonly gender : boolean //受保护的，子类可访问
    constructor (name : string,age:number) {
        this.name = name
        this.age = age
        this.gender = true
    }
    
    static create(){
    
    }
}
~~~



### 41类与接口

一个接口定义一个规范

```
interface Eat {
	eat (food : string) : void
}

interface Run {
	run (distance : number) : void
}

function implementEat,Run () {

}
```



### 42抽象类

抽象类也可以约束子类中有哪些成员，不同于接口的是抽象类可以有某些具体的实现。

定义成抽象类后就只能去继承，而不能通过new来定义新的构造函数

当父类中有抽象方法时，子类必须去实现这个方法

```
abstract class Animal {
    eat (food : string):void{
        console.log(`吃的${food}`)
    }

    abstract run (distance : number) : void
}

class Dog extends Animal {
    run(distance: number): void {
        console.log(distance)
    }
}

```



### 43 泛型

在定义函数接口或类的时候没有定义具体的数据类型，在使用的时候再去定义类型。

```
function func<T> (length : number ,value : T) :T[] {
    const arr = Array<T>(length).fill(value)
    return arr
}

```



### 44类型声明

在ts中引用第三方模块，如果模块中不包含响应的类型声明文件，可以尝试安装对应的类型声明模块。如果依然没有就使用declare自行声明。

```
declare function func(input:string):string
```



