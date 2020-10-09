### 01文件初始化

```
//1
npm init -y

npm nuxt

```

```
//2. package.json
  "scripts": {
    
    "dev":"nuxt"
  },
```

```
//3.创建pages文件，创建index.vue
```



### 02 导入样式资源

```
//根目录下新建app.html
<!DOCTYPE html>
<html {{ HTML_ATTRS }}>

<head {{ HEAD_ATTRS }}>
    {{ HEAD }}
    <link href="https://cdn.jsdelivr.net/npm/ionicons@2.0.1/css/ionicons.min.css" rel="stylesheet" type="text/css">
    <link
        href="//fonts.googleapis.com/css?family=Titillium+Web:700|Source+Serif+Pro:400,700|Merriweather+Sans:400,700|Source+Sans+Pro:400,300,600,700,300italic,400italic,600italic,700italic"
        rel="stylesheet" type="text/css">
    <!-- Import the custom Bootstrap 4 theme from our hosted CDN -->
    <link rel="stylesheet" href="/index.css">
</head>

<body {{ BODY_ATTRS }}>
    {{ APP }}
</body>

</html>
```



### 03 布局组件

自定义子路由

根目录下创建nuxt.config.js



### 04 Nuxt实现登录状态存储

Nuxt因为既要客户端可以拿到状态，服务端也要拿到状态，所以需要在Nuxt官网example中**跨域身份验证**中去模仿解决



### 05  Nuxt中状态存储

需要在根目录下创建store文件夹，nuxt会自动匹配store文件夹，新建index.js在里面定义状态

```
// 在服务端渲染期间运行都是同一个实例
// 为了防止数据冲突，务必要把state定义成一个函数，返回数据对象
export const state = () => {
    return  {
        user : null // 当前登录用户的登录状态
    }
}

export const mutations = {
    setUser(state,data) {
        state.user = data
    }
}

export const actions = {}
```



### 06 Nuxt数据持久化

将数据放入cookie中



### 07 Nuxt路由中间件(处理路由拦截)

使用middleware，定义middleware文件夹，在里面的文件就是中间件

```
export default function ({
    store,
    redirect
}) {
    // If the user is authenticated redirect to home page
    if (store.state.user) {
        return redirect('/')
    }
}
```



在需要的组件中直接写middleware ：'文件名'

````
export default {
  middleware: "authenticated",
  name: "SettingsIndex",
};
````



### 08 Nuxt插件



### 09 NUxt处理日期

使用Day.js处理日期格式，是Moment,js的轻量化版本



**全局过滤器**

在nuxt中利用插件来扩展



### 10 把markdorn转化为html

markdown-it



### 11 Nuxt部署方式

- 配置Host + Port
- 压缩发布包
- 把发布包传到服务器
- 解压
- 安装依赖
- 启动服务



需要上传

![image-20201008200331520](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20201008200331520.png)



### 12 使用pm2启动服务

![image-20201008201409011](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20201008201409011.png)



### 13 自动化部署

![image-20201008201738254](C:\Users\邱添\AppData\Roaming\Typora\typora-user-images\image-20201008201738254.png)





### 作业TODO

- [x] SETING页面功能
- [x] New Article页面功能
- [ ] 编辑文章
  - [ ] 更新文章
  - [ ] 删除文章
- [ ] 用户详情页功能
  - [ ] 头部个人信息
  - [ ] 导航栏切换
  - [ ] 文章内容获取