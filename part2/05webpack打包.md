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

webpack是前端工程的打包工具

通过webpack打包css文件，可以为其他文件安装css-loader,再使用syle-loader将css-loader编译过后的结果通过style标签中渲染到页面上

```
module : {
    rules : [
		{
			test : /.css$/
			use:[
				‘style-loader'
				'css-loader'
			] 
		}，
    ]
}
```

这里use中如果是数组，那么会重后往前去执行。

借助于Loader就可以加载任何类型的资源



### 08 webpack 导入资源模块

webpack的打包入口文件一般还是js文件，因为打包入口一般是运行入口。JavaScript驱动整个前端应用的业务

webpack建议我们根据代码的需要动态导入资源，因为需要资源的不是应用，而是代码

逻辑合理，JS确实需要这些资源文件

确保上线资源不确实，都是必要的



### 09 webpack文件资源加载器

通过import的方式导入文件

使用file-loader处理项目中图片



### 10 webpack URL加载器

- Data URLs 可以直接去表示文件，不会发送任何http请求。

- 像图片和字体这种二进制文件可以通过base64转码

- 使用data-url可以使用代码表示任何文件

- 使用url-loader
- 转换data-url适用小文件，减少请求次数

- 大文件单独提取存放，增加加载速度



### 11 Webpack 常用加载器分类

- 编译转换类型的加载器 ： 会将加载到的模块转换为JS代码 （css-loader）
- 文件操作类型加载器 ： 将文件拷贝到输出目录，同时将文件的访问路径向外导出(file-loader)
- 代码检查类加载器 ： 统一代码风格 (eslint-loader)



### 12Webpack 处理ES2015

因为模块打包需要，所以处理import和export，但本身并不是帮我们去自动处理ES6代码

需要配置babel-loader,@babel/core,@babel/preset-env 



### 13 webpack模块加载方式

- import加载模块 ： ES规范
- require函数加载模块 ： CommonJS规范
- define函数和require函数 ： AMD规范

不要在项目中混合使用标准！！



另外，loader加载的非JavaScript也会触发资源加载，比如样式代码中的@import和url函数，还有HTML代码中图片标签的src属性也会触发资源加载

下载html-loader处理html文件，html-loader只会处理img下的src属性，如果说其他标签的属性也需要打包，要去配置



### 14 webpack核心工作原理

webpack会根据配置找到一个文件作为入口文件，一般都是一个JS文件，会根据这个文件中出现的import或require去判断文件所依赖的模块，再去解析模块，这样就形成了一个依赖树，webpack会去递归这个依赖树，去找到每个节点所对应的依赖，最后把结果放在bundle.js。

![image-20200626224244825](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200626224244825.png)



Loader机制时webpack的核心



### 15 webpack中Loader的工作原理

实现一个markdown-loader.

每一个loader都要导出一个函数，作为处理资源的过程,输入就是要加工的文件，输出就是加工后的结果。

每个loader输出的结果应该是一个JavaScript代码，原因是因为它会将处理结果直接拼接到bundle.js中、

导出文件可以写为

```js
return `module.exports = ${JSON.stringify(html)}`
```

也可以谢伟

```
return `export default ${JSON.stringify(html)}`
```

```
//markdown-loader.js
const marked = require('marked')

module.exports = source => {
    console.log(source)
    const html = marked(source)
    // return `export default ${JSON.stringify(html)}`
    //返回 html 字符串交给下一个loader处理
    return html
}
```

loader的核心就是负责资源文件从输入到输出的转换，同时Loader实际上是一个管道，一个loader处理的结果可以转给下一个loader，完成一个功能。



### 16webpack插件机制介绍

webpack中的插件的目的是增强webpack自动化方面的能力。laoder专注实现资源模块的加载，Plugin解决除了资源加载以外自动化的工作，比如清除dist目录，或者拷贝文件到输出目录，或者压缩输出代码。



### 17 webpack自动清除清除输出目录文件

使用clean-webpack-plugin插件。在webpack.config.js中添加plugins属性。

```
module.exports = {
    plugins ; [
		new CleanWebpackPlugin()
    ]
}


```





### 18 webpack自动生成HTML插件

使用html-webpack-plugin的插件

```js
module.exports = {
    mode: 'none', //有development,production,none三种模式
    entry: './src/main.js', //入口文件路径
    output: {
        filename: 'bundle.js', //出口文件路径
        path: path.join(__dirname, 'dist'), //指定输出文件所在目录,这里必须是绝对路径，所以通过Node中path模块生成绝对路径
        // publicPath: 'dist/' //默认值是空字符串，表示网站根目录
    },
    module: {
        rules: [{
                test: /.md$/,
                use: [
                    'html-loader',
                    './markdown-loader.js'
                ]
            }
        ]

    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPligun()
    ]
}
```



有了html-webpack-plugin我们就可以动态生成项目中的html文件。改进html

定义一个html模板，对于需要动态生成的标签可以使用htmlWebpackPlugin.options

使用插件同时输出多个页面文件

```js
new HtmlWebpackPligun({
    title : 'webpack plugin sample',//标题
    meta :{//设置原数据的一些标签
         viewport : 'width-device-width'
    },
    template : './src/index.html'
}),
//用于生成about.html
new HtmlWebpackPligun({
   filename : 'about.html'
})
```



### 19webpack常用插件总结

将文件复制到输出目录，借助到copy-webpack-plugin插件

```
new CopyWebpackPlugin([
   //这里写要复制文件的路径
])
```

可以在github上搜索自己需要的pliugin



### 20 webpack开发一个插件

相比于Loader，Plugin拥有更宽的能力范围

- plugin的核心就是通过钩子机制实现的。

- webpack要求plugin是一个函数或者是一个包含apply方法的对象

```js
class MyPlugin {
    apply(compiler) {//这个方法会自动被调用，complier对象包含了所有的配置信息，也通过这个对象来注册钩子函数
        //这个插件是用来清除掉webpack打包中没有作用的注释
        console.log('MyPlugin 启动')
        //注册钩子函数，接收两个参数，第一个参数为插件名称，第二个参数就是要挂在到钩子上的函数
        compiler.hooks.emit.tap('Myplugin',compilation=>{//complilation : 可以理解为此次打包的上下文
            for(const name in compilation.assets) {
                // console.log( compilation.assets[name].source())
                if(name.endsWith('.js')){//只处理js文件
                    const contents = compilation.assets[name].source()
                    const widthoutComments = contents.replace(/\/\*\*+\*\//g,'')
                    compilation.assets[name] = {
                        source: ()=> widthoutComments,
                        size : ()=>widthoutComments.length
                    }
                }
            }

        })
    }   
}
```

插件通过在声明周期的钩子中挂在函数实现扩展



### 21webpack开发体验问题

- 需要HTTP server 运行
- 自动编译和自动刷新
- 提供Source Map 支持，一旦出现错误就可以快速对应源代码信息



### 22 webpack自动编译

使用webpack中watch参数监听文件变化，自动重新打包

```
yarn webpack --watch
```



### 23 webpack 实现自动刷新浏览器

使用BrowserSync这个工具就可以实现自动刷新浏览器的功能，这里操作麻烦，效率降低（会多出两部磁盘读写操作）



### 24webpack 中 Dev Server

dev Server提供用于开发的HTTP Server，集成“自动编译”和“自动刷新浏览器”等功能

安装

```
 yarn add webpack-dev-server --dev
```

dev server 为了提高效率所以并没有将打包结果写入磁盘当中，而是将打包结果存放在内存当中.

还可以添加一个open的参数，自动打开浏览器

```
yarn webpack-dev-server --open
```



### 25 Dev Server 静态资源访问

dev server 默认只会serve打包输出文件，只要是webpack输出的文件都可以直接被访问，其他静态资源文件也需要serve，就需要额外的高速dev server

可以在webpack配置文件中新加一个devServer属性

```
devServer : {
  contentBase : './public'
}
```



### 26 webpack Dev Server 代理API

dev Server 支持代理服务

```js
devServer : {
        contentBase : './public',
        proxy : {
            //请求路径前缀
            '/api' : {
                //请求http ：//localhost:8080/api/users => https://api.github.com/users
                target : 'https://api.github.com',
                pathRewrite : {
                    '^/api':''
                },
                //不能使用localhost:8080作为请求github的主机名
                changeOrigin : true
            }
        }
    }
```



### 27 Source Map 介绍

运行啊与源代码之间不同，如果需要调试应用，或者错误信息无法定位，调式和报错都是基于运行代码，Source Map就可以解决这个问题。

Source Map 解决了源代码与运行代码不一致所产生的问题



### 28webpack配置source map

在webpack.config.js配置

```
    devtool : 'source-map'
```

webpack支持12中不同的方式的source map,每种方式的效率和效果各不相同



### 29 webpack eval模式的Source Map

![image-20200627143900545](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200627143900545.png)



- eval模式

````js
devtool : 'eval'
````

在eval模式下会将打包后的代码放在eval函数汇总去执行，最后通过sourceURL的形式去确认代码路径。

这种模式不会生成source map文件，所以构建速度最快，但是效果只会定位源代码的名称，而不会定位行列信息



### 30webpack  devtool模式对比

- eval-source-map 模式 可以生成source map文件，可以定位到错误的行列
- cheap-eval-source-map模式，生成了source map文件，但只能定位到行，但定位不到列，并且是经过loader加工后的源代码
- cheap-module-eval-source-map 是没有经过loader加工的源代码
- hidden-source-map模式 生成了source-map文件，但是在开发工具上看不到，使用场景是开发第三方模块时使用
- 是osources-source-map模式是没有源代码但是提供了行列信息



### 31 webpack 选择Source Map模式

- 开发模式 ： cheap-module-eval-source-map\

  - 每行代码少
  - 经过loader编译后的代码变化差异较大
  - 首次打包速度慢，使用webpack-dev-server重写打包速度快

- 生产模式 ： none
  - Source map会暴露源代码
  - 还可以使用nosource-source-map



### 32 webpack自动刷新的问题

页面不刷新的前提下，模块也可以及时更新



### 33 Webpack HMR 体验

模块热替换（模块热更新），应用运行过程中实时替换某个模块，应用运行状态不受影响。

HMR是webpack中最强大的功能之一，它极大程度的提高了开发者的工作效率



### 34 webpack 开启 HMR

集成在webpack-dev-server中

```js
webpack-dev-server --hot
```

或者在配置文件配置

```
//1.在devServer中配置
devServer :{
	hot : true
}
//引入webpack的插件
const webpack = require('webpack')

new webpack.HotModuleReplacementPlugin()
```

webpack中的HMR并不可以开箱即用，webpack中的HMR需要手动处理模块热替换逻辑。



Q1 ： 为什么央视文件的热更新开箱即用 ？

因为css文件是经过loader处理过的

Q2 : 为什么样式文件自动处理

因为只需要替换样式就可以

Q3 ： 我的项目没有手动处理，JS照样可以热替换

框架下的开发，每种文件都是有规律的，所以就有替换办法，并且通过脚手架创建的项目内部都继承了HMR方案

总结： 我们需要手动处理JS模块更新后的热替换



### 35 webpack 使用HMR API

```
module.hot.accept(‘./eidte.js’,()=>{
	console.log('更新模块内容')
})//接收两个参数，一个参数为模块路径，一个是模块处理函数
```



### 36 Webpack处理JS模块热替换

这不是一个通用的方式

![image-20200627202327936](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200627202327936.png)

webpack'不能对js文件热替换提供一个通用的解决方案，所以只能我们按照需求来配置js模块



### 37 webpack处理图片模块热替换

```
module.hot.accept('./better.png,()=>{
	img.src = background
	console.log(background)
})
```



### 38 HMR注意事项

- 处理HWR的代码报错会导致自动刷新 （使用hotOnly)
- 没请用HMR的情况下，HMR API会报错（先去判断module.hot）
- 在使用时写了很多与项目业务代码无关的代码会不会有影响？（webpack打包后会删除这些代码）



### 39 Webpack生产环境优化

生产环境种种运行效率，webpack4提供了模式(mode)webpack建议我们为不同的工作环境创建不同的配置



### 40 webpack不同环境下的配置

不同环境下不同的配置主要有两种

1. 配置文件根据环境不同导出不同配置
2. 一个环境对应一个配置文件



webpack.config.js还支持导出一个函数，函数中接收两个参数，第一个是环境名参数，第二个参数是运行cli过程中所有的参数

```
//1.
module.exports = (env, argv) => {
    const config = {
        mode: 'development', //有development,production,none三种模式
        entry: './src/main.js', //入口文件路径
        output: {
            filename: 'bundle.js', //出口文件路径
            path: path.join(__dirname, 'dist'), //指定输出文件所在目录,这里必须是绝对路径，所以通过Node中path模块生成绝对路径
            // publicPath: 'dist/' //默认值是空字符串，表示网站根目录
        },
        module: {
            rules: [{
                    test: /.md$/,
                    use: [
                        'html-loader',
                        './markdown-loader.js'
                    ]
                } {
                    test: /.css$/,
                    use: [
                        'style-loader',
                        'css-loader'
                    ]
                },
                {
                    test: /.png$/,
                    use: {
                        loader: 'url-loader',
                        options: {
                            limit: 10 * 1024 //单位为字节， 这里是设置只将10kb一下的文件转换为data-url,它对超出的文件依然会调用file-loader，所以要安装file-loader
                        }
                    }
                },
                {
                    test: /.js$/,
                    use: {
                        loader: 'babel-loader',
                        options: {
                            presets: ['@babel/preset-env']
                        }
                    }
                },
                {
                    test: /.html$/,
                    use: {
                        loader: 'html-loader',
                        options: {
                            attrs: ['img:src', 'a:href']
                        }
                    }

                }
            ]

        },
        plugins: [
            new CleanWebpackPlugin(),
            new HtmlWebpackPligun({
                title: 'webpack plugin sample', //标题
                meta: { //设置原数据的一些标签
                    viewport: 'width-device-width'
                },
                template: './src/index.html'
            }),
            //用于生成about.html
            new HtmlWebpackPligun({
                filename: 'about.html'
            }),
            //开发阶段最好不要使用这个插件
            // new CopyWebpackPlugin([
            //     //这里写要复制文件的路径
            // ]),
            new MyPlugin(),
            new webpack.HotModuleReplacementPlugin()
        ],
        devServer: {
            hot: true
            // contentBase : './public',
            // proxy : {
            //     //请求路径前缀
            //     '/api' : {
            //         //请求http ：//localhost:8080/api/users => https://api.github.com/users
            //         target : 'https://api.github.com',
            //         pathRewrite : {
            //             '^/api':''
            //         },
            //         //不能使用localhost:8080作为请求github的主机名
            //         changeOrigin : true
            //     }
            // }
        },
        //devtool用于配置开发中一些工具配置，source Map配置
        devtool: 'source-map'
    }

    if (env === 'production') {
        config.mode = 'production',
            config.plugin = [
                ...config.plugin,
                new CleanWebpackPlugin(),
                new CopyWebpackPlugin(['public'])
            ]
    }
    return config
}
```



### 41webpack不同环境的配置文件

 上面这种方式值适用于中小型项目，大型项目还是选择不同环境对应不同配置文件的方式。

先创建一个webpack.common.js将公共代码部分粘贴进去

```js
//webpack.config.js
const HtmlWebpackPligun = require('html-webpack-plugin')
module.exports = {
    mode: 'none', //有development,production,none三种模式
    entry: './src/main.js', //入口文件路径
    output: {
        filename: 'bundle.js', //出口文件路径
        path: path.join(__dirname, 'dist'), //指定输出文件所在目录,这里必须是绝对路径，所以通过Node中path模块生成绝对路径
        // publicPath: 'dist/' //默认值是空字符串，表示网站根目录
    },
    module: {
        rules: [{
                test: /.md$/,
                use: [
                    'html-loader',
                    './markdown-loader.js'
                ]
            }
            // {
            //     test: /.css$/,
            //     use: [
            //         'style-loader',
            //         'css-loader'
            //     ]
            // },
            // {
            //     test: /.png$/,
            //     use: {
            //         loader: 'url-loader',
            //         options: {
            //             limit: 10 * 1024 //单位为字节， 这里是设置只将10kb一下的文件转换为data-url,它对超出的文件依然会调用file-loader，所以要安装file-loader
            //         }
            //     }
            // },
            // {
            //     test: /.js$/,
            //     use: {
            //         loader: 'babel-loader',
            //         options: {
            //             presets: ['@babel/preset-env']
            //         }
            //     }
            // },
            // {
            //     test: /.html$/,
            //     use: {
            //         loader: 'html-loader',
            //         options: {
            //             attrs: ['img:src', 'a:href']
            //         }
            //     }

            // }
        ]

    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPligun({
            title: 'webpack plugin sample', //标题
            meta: { //设置原数据的一些标签
                viewport: 'width-device-width'
            },
            template: './src/index.html'
        }),
        //用于生成about.html
        new HtmlWebpackPligun({
            filename: 'about.html'
        }),
        //开发阶段最好不要使用这个插件
        // new CopyWebpackPlugin([
        //     //这里写要复制文件的路径
        // ]),
        new MyPlugin(),
        new webpack.HotModuleReplacementPlugin()
    ],
    devServer : {
        hot : true
        // contentBase : './public',
        // proxy : {
        //     //请求路径前缀
        //     '/api' : {
        //         //请求http ：//localhost:8080/api/users => https://api.github.com/users
        //         target : 'https://api.github.com',
        //         pathRewrite : {
        //             '^/api':''
        //         },
        //         //不能使用localhost:8080作为请求github的主机名
        //         changeOrigin : true
        //     }
        // }
    },
    //devtool用于配置开发中一些工具配置，source Map配置
    devtool : 'source-map'
}

```

在创建webpack.dev.js与webpack.prod.js

适用webpack-merge来合并项目

```js
//1webpack.prod.js
const common = require('./webpack.common')
const {
    CleanWebpackPlugin
} = require('clean-webpack-plugin')
const CopyWebpackPlugin = require('copy-webpack-plugin')

const merge = require('webpack-merge')

module.exports = merge(common, {
    mode: 'production',
    plugins: [
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin('public')
    ]
})
```



### 42 Webpack DefinePlugin

DefinePlugin是为代码注入全局成员

```js
process.env.NODE_ENV
```

```
const webpack = require('webpack')

module.exports = {
	mode : 'none',
	entry : './src/main.js',
	output : {
		filename : 'bundle.js'
	}
	plugins : [
		new wenpack.DefinePlugin({
			API_BASE_URL：‘"webpack.prod.js"'
		})
	]

}
```



### 43 webpack 体验Tree Shaking

Tree Shaking解决未引用代码

Tree Shaking会在生产模式下自动取开启



### 44 适用 Tree Shaking

Tree Shaking不是指某个配置选项，是一组功能搭配适用后的优化效果，这个效果会在production模式下自动开启。

tree Shaking的实现

```
//webpack.config.js
module.exports = {
	optimization : {//这个属性几种配置webpack优化功能
		usedExports : true,//只导出外部使用过的资源
		minimize : true
	}
}
```

usedExports负责标记“枯树叶”，minimize负责“摇掉”他们



### 45 webpack 合并模块

使用concatenateModules尽可能的将所有模块合并输出到一个函数中，既提升了运行效率，又减少了代码的体积，这一特性被称为（Scope Hoisting）

```
module.exports = {
	optimization : {//这个属性几种配置webpack优化功能
		usedExports : true,//只导出外部使用过的资源
		concatenateModules : true,
		minimize : true
	}
}
```



### 46webpack tree Shacking 与Babel

Tree Shaking 前提是ES Modules，也就是说由webpack打包的代码必须使用ESM定义的模块化

最新的babel-loader帮我们关闭了自动转换ESM的插件



### 47 webpack sideEffects副作用

允许我们通过配置的方式标记我们的代码是否有副作用，从而让tree Sharking有更大的压缩空间。

所谓副作用：模块执行时除了导出成员之外所做的事情

sideEffects一般用于npm包标记是否有副作用。

sideEffects与tree Shacking并没有什么太大的关系

```
//webpack.config.js
optimization : {
	sideEffects : true,//在production中自动被开启
}
//package.json
{
	“sideEffects" : false,//表示项目中所有代码都没有副作用
}
```



### 48 webpack sideEffects注意

使用sideEffects的前提就是确保你的代码真的没有副作用

css模块也属于副作用模块

```
//package.json
{
	...
	”sideEffects“ ： [
		"*/css"
	]
}

```



### 49 webpack 代码分解

webpack也会有一些弊端，那就是所有代码最终都被打包到一起，如果文件特别多的话，bundle体积就会特别大，但是我们启动应用时并不是每个模块在启动时都是必要的，在浏览器中占用特别大的带宽，所以我们需要分包，按需加载。

目前webpack实现分包的方式主要有两种

- 多入口打包
- 动态导入



### 50 webpack多入口打包

多入口打包适用于传统的多页应用程序，一个页面对应一个打包入口，公共部分单独提取。

entry需要定义为一个对象

```js
//webpack.config.js
module.exports + {
    entry : {
        index : './src/index.js',
        album : './src/album.js'
    },
    output : {
        filename : '[name].bundle.js'
    },
    plugins ;[
    	new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title : 'title',
            template : './src/index.html',
            dilename : 'index.html',
            chunks : ['index']
        }),
        new HtmlWebpackPlugin({
            title : 'title',
            template : './src/album.html',
            dilename : 'album.html',
            chunks : ['album']
        }),
    ]
}
```



### 51 提取公共模块

不同入口中肯定会有公共模块

开启公共模块，在配置文件中优化配置optimization中配置aplitChunks

```
optimization : {
	aplltChunks : {
		chunks : 'all'//把所有的公共模块提取到bundle中
	}
}
```



### 52webpack动态导入

这里的按需加载指的是需要用到某个模块时，再加载这个模块

webpack动态导入的模块会被自动分包

这里使用的动态导入就是ESM中的动态导入

![image-20200628223342018](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200628223342018.png)



### 53webpack 魔法注释

动态导入的文件bundle的名称知识一个序号，

在动态导入的模块内加入一段注释

```
/*webpackChunkName:'post'*/
```

![image-20200628223945530](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200628223945530.png)



### 54 webpack MiniCssExtractPlugin 提取css到单个文件

安装mini-css-extract-plugin

```
yarn add mini-css-extract-plugin --dev
```

![image-20200628224350385](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200628224350385.png)

​     

### 55 OPtimizeCssAssetsWebpackPlugin 压缩输出的css文件

```
optimize-css-assets-webpack-plugin
```

![image-20200628225302901](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20200628225302901.png)



同时也可以放在plugin中



### 56 输出文件名Hash

生产模式下，文件名使用Hash

webpack中的filename支持通过占位符的方式为名称添加hash值，支持三种hash值

```
//1.hash
output : {
	filename : '[name]-[hash].hundle.js'
}
//2. chunkhash 每一路的hash不同，统一路的hash相同
output : {
	filename : '[name]-[chunkhash].hundle.js'
}
//3. contenthash 文件级的hash，只要是不同的文件就有不同的hash
output : {
	filename : '[name]-[conetenthash].hundle.js'
}
```

还可以通过’：number‘的形式去修改hash值的长度

```
output : {
	filename : '[name]-[conetenthash:8].hundle.js'
}
```



















