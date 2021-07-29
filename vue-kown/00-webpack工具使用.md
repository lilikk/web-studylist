# webpack工具使用

#### 课堂小知识

**1  webpack本质：node.js第三方模块包(express,moment ,mysql..)，分析打包代码。**

1支持所有类型的打包！

2支持less/sass.stylus=>css

3支持将高级语法（ES6/7/8）转成ES5

4压缩代码，提高网站加载速度

**2  webpack 使用**

1 初始化包环境       npm init （或者 yarn inti）(或者 npm init  -y)

2 安装    npm i webpack webpack-cli -D  或者 yarn add webpack webpack-cli -D

**3在package.json配置： scripts 自定义配置 “scripts”：{“build”：“webpack”}**，

（**安装过程-D："devDependencies"    -D：--save-dev的简写，安装到开发依赖，不会被打包--线下**）

（**安装过程-S："dependencies"    -S： --save的简写，安装到生产依赖，会被打包--线上**）

（配置：这个自定义命令是接下来让webpack干活用的，打包用的命令）

yarn

```js
（yarn 需要用npm 装一下  npm i yarn -g  这个比npm速度快）（mac: sudo npm yarn -g）

装包： yarn add 包名          npm i  包名
      yarn add 包名 -D       npm i  包名 -D
卸载： yarn remove 包名        npm un 包名

```

ES6模块化语法 导入导出

```js
//1 export导出   import导入
export{ add }
导出add函数

import {add} from './module/add.js'
导入add.js中的  导出add函数
```

####**webpack打包文件步骤**

1新建**src文件夹** 文件夹内有个**index.js** 这个是用来打包入口文件

2 把需要的代码**引入到入口文件**才会参与打包

3执行**package.json里build**命令 执行webpack打包命令 **yarn build**  或者  **npm run build**

4默认输出**dist文件夹 main.js**

-----------------------------------------------------------------------------------------------------------------------------------------------

**注意**：src文件夹是默认的 所有要打包的文件在src下的文件夹中  

​           index.js 是默认是入口文件 在src内

-----------------------------------------------------------------------------------------------------------------------------------------------

#### webpack配置

可以修改入口打包和 压缩文件夹名和压缩文件

步骤 

1在src同级目录下建立一个配置文件 **webpack.config.js**     (固定)

2填入配置 module.exports={配置项}

3修改入口文件名 设置出口文件路径和文件名

```js
新建  webpack.config.js  文件 （和src文件夹在相同目录下）-------用来设置配置项
新建  在src文件夹下建一个自己指定的文件名 比如content.js--------- 用来做入口文件 
在webpack.config.js文件
----------------------------------------------------------------------------------
webpack是基于node.js所以可以使用内置模块
----------------------------------------------------------------------------------
const path=require("path")
module.exports={
    entry:'./src/content.js',             --------------------入口文件设置
    output:{                               -------------------出口文件设置
       path:'path.resolve(__dirname,'dist')',----------------指定压缩文件的路径
        filename:'main.js'                  ----------------指定压缩文件的文件名
    },
     mode:'production'                   -----------------生产模式 清除注释和换行
}
```

####webpack打包模式：

消除webpack打包警告

1生产模式 mode:'production'   清除注释和换行 体积最小化

2开发模式mode:'develoment'  保留注释和换行

--------------------------------------------------------------------------------------------

案例：隔行变色 有10个小li 奇数行变红色  偶数行变绿色

分析：

​       1先构建：静态页面html（比如：在src同目录下建一个文件夹 public  创建一个index.html 写静态）

​       2下载jquery包  npm i jquery -S（yarn add jquery -S） 

​          引入到主文件 import $ from 'jquery'

​        3编写隔行变色代码 （js代码）

​        4 打包（npm run build 或者 yarn build）

​        5 在静态页面引入打包后的js文件(手动复制html到打包文件夹  在html文件中引入打包后的js文件)





----------------------------------------------------------------------------------

#### webpack插件

**1  html-webpack-plugin**

------------自动生成一个html文件在dist文件夹中

-------------在生成的html引入打包文件

```js
npm i html-webpack-plugin -D      ---------------下载插件
const htmlWebpackPlugin=require("html-webpack-plugin")  --------------在配置文件移入

在配置文件中配置
module.exports={
    entry:'./src/content.js',             --------------------入口文件设置
    output:{                               -------------------出口文件设置
       path:'path.resolve(__dirname,'dist')',----------------指定压缩文件的路径
        filename:'main.js'                  ----------------指定压缩文件的文件名
    },
     mode:'production',                -----------------生产模式 清除注释和换行
     plugins:[                       -------------------html-webpack-plugin插件配置
         new htmlWebpackPlugin({
             template:'./public/index.html'  
             // 以piblic/index.html为模板，赋值一份到dist中
         })
     ]
}
```

如果是js 文件 通过  import/require 引入  需要定义变量接   **import  $  from "jquery"**

如果是css文件 通过 improt 引入    import  "相对入口js文件的路径"

**webpack只能打包js和json格式的文件！！！！！**

如果想要打包css 文件的话 **需要加载器**：css-loader  和 style-loader

**css-loader**  可以把css文件打包到js文件中

**style-loader** 可以把打包好的css代码插入到DOM中

```js
npm i css-loader style-loader -D  下载加载器
yarn add css-loader style-loader -D
加入webpackconfig.js配置项中
module:{
    rules:[{                                //css文件解析规则
        test:/\.css$/i,      
        use:['style-loader','css-loader']  //使用顺序是css到style 但是书写顺序不变
    }]
}
一定要将css文件和入口文件建立联系
```

**less less-loader**

```js
下载加载器  npm i less less-loader -D

{test:/\.less$/i,
 use:['style-loader','css-loader','less-loader']
}
一定要将less文件和入口文件建立联系
```

less-loader :   识别less文件
less :   将less编译为css

------------------------------------------------------------------------------------------------

*import 接收 变量和css less 动态创建的图片 背景图区别*

*module.export*

*require  区别？？？*

*base64 知识点*

*正则*

-------------------------------------------------------------

####webpack无法处理图片和字体 需要用资源模块！！！

```js
{test:/\.(png|jpg|gif|svg|webp|jpe?g)$/i,
type:'asset'  //处理图片到资源类型
 //asset资源类型会根据图片是不是超过8k 进行不同的打包  大
 //于8k 复制一份放到压缩文件夹中
 //小于8k 转化成base64位 打包到js文件中
}
```

base64位图片流：优点：减小浏览器请求次数 ，直接读取，速度快

​                                缺点：转化成base64位，图片大小增大30%，图片太大，得不偿失

####webpack字体图标打包

**步骤**

1准备代码

2准备环境

```js
{test:/\.(eot|svg|ttf|woff2?)$/i,
type：'asset/resource',
generator:{  ----------------------------------指定打包字体输出的目录名和文件名
    filename:'font/[name].[hash:8].[ext]'
}
}
[name] 保留原来的文件名
[hash:8] 随机的8位字符串 防止命名重复
[ext] 原来文件的扩展名
字体图标文件打包之后会在打包文件夹中生成文件
```

#### webpack降级js语法

借助**babel编译工具**将js高级语法变成低级

1准备代码

2准备环境

2.1**下载 babel-loader   @babel/core  @babel/preset-env**      **-D**

2.2配置高级js降级规则

```js
{
    test:/\.js$/i,              //所有的js文件 
    exclude:/node_modules/,     //除了node-modules文件的js
    use:{
        loader:'babel-loader',  //babel-loader 处理高级语法
        options:{
        presets:['@babel/preset-env']   //降级规则
        }
    }
}
```

#### webpack开发服务器

打包在内存中 速度快

自动更新 实时更新

**webpack-dev-server模块包**

1下载模块包

yarn add  webpack-dev-server -D  或npm i webpack-dev-server -D

2配置开发服务器的启动命令 serve

**在package.json配置： scripts 自定义配置 “scripts”：{“serve”：“webpack sever”}**

3执行命令 yarn serve   或者npm run serve

**webpack服务器配置**

```js
module.exports={
devServer:{
port:3000,  // 可以修改端口号
open:ture   //自动打开浏览器
}
}
配置完开发服务器 需要运行一下yarn sever  自动就会将修改的内容打包实时展示
```

yarn build和yarn server区别

webpack 构建的流程