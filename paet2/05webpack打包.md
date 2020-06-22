### 01 模块化由来

- 常见问题
  - ES Modules存在环境兼容问题
  - 模块文件过多，网络请求频繁
  - 所有的前端资源都需要模块化
- 模块化希望解决的问题
  - 新特性代码编译
  - 模块化JS打包
  - 支持不同类型的资源模块



### 02 模块打包工具概要

webpack作为模块打包器，可以将零散的模块代码可以打包到一个js 文件中，新特性代码可以通过模块加载器（loader）进行转换。webpack还具有代码拆分的能力。webpack在js中支持我们以模块的形式载入任意类型的文件。



这里所说的打包工具解决的是前端整体的模块化，并不单指JS 模块化。



### 03 webpack 快速上手

安装

```
1.yarn init

2. yarn add webpack webpack-cli --dev


```

使用

```
//命令行输入webpack，打包，默认会从src中index,js打包
```



### 04 webpack配置文件

webpack4以后的版本支持0配置的方法直接打包，按照约定会将src/index.js作为打包入口，打包结果会存放在dist/main.js

使用webpack.config.js配置



### 05 webpack工作模式

工作模式是有production，development,none

```
yarn webpack --mode production
```

在webpack'.config.js

```
 module.exports = {
    mode : 'production',
    entry : './src/index.js',//入口文件路径
    output: {
        filename : 'builde.js',//出口文件路径
        path :  path.join(__dirname,'output') //指定输出文件所在目录,这里必须是绝对路径，所以通过Node中path模块生成绝对路径
    }
}
```



### 06 webpack打包结果运行原理

使用none模式打包出结果，发现webpack打包出来的是一个立即执行的函数这是webpack打包出来的工作入口，它接收一个modules的参数，传入的是一个数组，数组中每个元素都是一个参数列表相同的函数，这个函数对应的就是我们代码中的模块，从而去实现模块的私有作用域。

![image-20200622203446824](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200622203446824.png)

函数内部首先定义了一个对象，用来存放（缓存）加载过的模块。

![image-20200622203609997](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200622203609997.png)



接着一定了require函数，用来加载模块

![image-20200622203646928](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200622203646928.png)

剩下的就是在require上挂载的数据和工具函数

![image-20200622203735122](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200622203735122.png)



最后调用了我们这个require函数，传入了0

![image-20200622203831217](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200622203831217.png)

​    



### 07 webpack资源模块加载