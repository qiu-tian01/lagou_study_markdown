#### 脚手架

1. 脚手架的实际作用

   Yeoman之类的工具到底有啥用

   - 像Vue-cli,react-create-app创建出来的都只适用于特定的框架，并且适用于大部分项目的基础结构，对于实际业务项目项目太过于简单
   - 常用实践
     - 基于Yeoman写Gennerator
   - 自己造轮子
     - Metaismith

#### 

#### Gulp VS. Webpack

Gulp不具备任何具体功能，完全自主，自定义性强

- 需要开发者自己实现各种功能
- 对Node.js储备要求高
- 强调任务的概念，Gulp本身实际上是一个任务调度工具
- 通俗上说Gulp是你想干什么就干什么



webpack从模块打包出发，通过插件实现一部分Web项目的自动化任务

- ​	开箱即用，门槛低
- 主要应对SPA类应用的模块打包



##### Gulp常见场景

- 如果只是传统的静态页面开发，注重的是页面结构与样式，建议采用Gulp
- 小程序项目中使用sass/Less
- 再者就是日常的综合事务：文件重命名/前后缀



##### 最佳实践

- 工具层面没有唯一标准答案
- 充分掌握Gulp与webpack，因地制宜
- SPA类使用webpack（单页面应用）
- MPA类使用Gulp（多页面应用）
- 如果只是个别的需求直接使用npm scripts配合个别工具就好
  - 例如： 只需要校验代码，单独使用ESList



##### npm 与yarn

都是包管理工具 

yarn 可以自动找到node_module/bin下的可执行文件，npx也可以

npx可以直接执行远程(线上)模块，一次性使用



##### 本地安装呢全局安装

大部分模块作为跟着项目本地安装而不是全局安装

全局安装模块：只有本地经常用到，而且与某一个特定项目无关的工具或模块

脚手架类型的工具，建议使用npx/yarn init ,一次性使用

其他所有的模块都应该安装到项目本地，也就是在package.json声明这个依赖，便于后期管理

```
npm config get prefix # 获取npm全局目录
yarn config get prefix
```



yarn xxx /npm xxx

有时间zce-cli阅读

 







