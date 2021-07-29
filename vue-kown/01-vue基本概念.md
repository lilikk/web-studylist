- [ ] 库：小 巧，只提供了特定的API，是属性和方法的集合
- [ ] 框架：大 全 提供了一整套问题解决方案，有自己的一套语法和规则
- [ ] 渐进式：逐渐使用，集成更多的功能

### vue基础概念

------------------------------------------------------------

渐进的javascript框架,有自己的语法规则

1 vue写在哪？

（1）传统开发模式：`<script src="vue.js">`

（2）工程化开发方式：在webpack环境中开发vue   

优点：模块化开发（export/import） 

​            使用npm下包，集成更多功能 

​            使用webpack实时打包

### @vue/cli脚手架

------------------------------------------------------------------------------

**@vue/cli是官方提供的全局模块包，用于创建脚手架项目。**

1开箱即用（直接用）

2 零配置webpack 支持babel(自动降级) 支持css less 图片 字体打包  ）

3支持开发服务器

安装：`npm  i @vue/cli  -g `或者`sudo npm  i @vue/cli  -g` (mac本)

检测： `vue -V ` 看是不是出现版本号

**安装完成，检测到版本号，安装成功，就可以用vue命令了**

**@vue/cli创建项目**

1创建项目 [vue create + 项目名] 项目名不能有大写 中文 特殊字符

  `vue create  demo   `

2 选择模板和包管理，等待脚手架项目创建完成（先选vue2）

3 cd进入项目下，启动webpack本地热更新服务器 （1 cd 项目名  2 yarn  serve）

4 弹出浏览器网址就创建完成

**脚手架创建的项目文件夹内容**

1node_modules文件夹---- 第三方包存储位置

2public文件夹----index.html（浏览器的入口文件）----favicon.ico

3src文件夹 ---assets文件夹（静态文件）

​                     ---components文件夹(放组件的文件夹.vue结尾)

​                     ---App.vue 这个是vue的入口页面

​                     ---main.js   webpack打包入口

4.gitignore  git管理忽略

5 package.json 依赖包列表

6babel.config.js      babel配置（语法降级）

7yarn.lock 项目下载

**入口项目，代码执行顺序和引入关系**

App.vue   =>main.js  =>index.html

main.js 中导入了App.vue  并且Vue初始化（实例化）  因为App.vue 会连同main.js一起打包，

打包之后的代码会自动被引入到index.html 中，把App.vue页面的内容展示到index.html中id为app的结构中。

```js
-----------------------main.js
import Vue from 'vue'------------------导入Vue对象（工程化开发方式）
import App from './App.vue'-------------------导入App.vue页面
---------------------------------------------------------------------------
Vue.config.productionTip=false   关闭控制台的一句提示
---------------------------------------------------------------------------
 new Vue({
    render:h=>h(App),
}).$mount('#app') ---------------------------把App的内容渲染到id为app的index.html
```

**脚手架自定义配置**

在根目录下创建 **vue.config.js**(根目录下和src同级)  **vue的配置文件**

```js
module.exports={
    devServer:{
        port:3000,//改端口
        open:true //自动打开浏览器
    }
}
yarn serve //更改配置后一定要重启服务器
```

### eslint检查工具

会提示错误信息 错误行..先暂时关闭 熟悉语法后在开启

```js
vue.config.js文件中设置lintOnSave:false
关闭eslint校验，true表示开启 false表示关闭

module.exports={
   lintOnSave:false
}
yarn serve
```

### vue单文件

Vue推荐用.vue文件开发项目 有template(html)+scriprt(js)+style(css)

独立作用域 变量私有 互不影响

一个template标签内只有一个根标签

style 上一个scoped 让样式只在当前文件生效

### 插值表达式

--------------------

作用：在dom标签中，直接插入vue数据变量

语法：{{表达式}}  模板需要保持精简

可以放：数据变量（data里面的return对象），算数运算 ，三元表达式，常见的js方法（字符串数组），函数调用

不可以放：var let  if   自增

###MVVM设计模式

-------------

设计模式：被反复使用的一种，多少人知晓的，经过分类编目的，但设计经验总结（编写思想）

MVVM：**vue不提倡dom操作，是数据驱动视图，数据变量，视图自动跟着变**

**M** :  (model)  data中的数据变量  数据

**V**：（view） 在template中的  视图

**VM**：(ViewModel)vue实例     视图模型

### 指令（v-bind v-on v-model）

-----------------

#####v-bind  

**把vue的数据变量与标签的属性 动态绑定**

v-bind 语法和简写 ：v-bind:属性名=“vue数据变量”   ：属性名=“vue数据变量”

原生=>需要先获取元素  获取之后通过 setattribute设置属性

```vue
<template>
    <div>
    <a v-bind:href='url'>百度</a>
    <img :src='imgsrc' alt=''>
    </div>
</template>
<script>
    export default {
        data(){
            return {
                url:'http://baidu.com',
                imgsrc:'http://xxxx'
            }
        }
    }
</script>
```

#####v-on 给标签绑定事件

v-on 语法和简写: v-on:事件名=“要执行的少量代码”     简写：@事件名=“  ”

​                              v-on:事件名=“methods中的函数名“

​                              v-on:事件名=“methods中的函数名（实参）“

```vue
<template>
<div>
    <p>{{count}}</p>
    <button @click="count=count+1">加1</button>
    <button @click="add">加3</button>    
    <button @click="add(5)">加5</button> 
</div>
</template>
<script>
    export default {
         data(){
             return {
                 count:0
             }
         },
        methods:{
            add(){   //this 指向的是export default导出的对象 data和methods上面的属性                         和方法都会挂载，所以可以直接用this直接访问数据变量和methods
             this.count+=3   
            },
            add5(n){
              this.count+=n
            }
        }
     }
</script>
```

#####vue事件对象event

语法 ：没有参数： 实参不改变   直接形参接收（e占位）

​             有参数  ： 实参（自定义参数，$event） 形参（自定形参 ，e占位）

```vue
<template>
<div>
   <a href="http://www.baidu.com" @click="goto1">百度</a> 
   <a href="http://www.163.com" @click="goto2(10,$event)">新浪</a> 
</div>
</template>
<script>
    export default {
        methods:{
            goto1(e){
                形参e占位
                e.preventDefault()//阻止默认行为
            },
            goto2(n,e){
                实参是$event固定写法
                e.preventDefault()
            }
        }
    }
</script>
```

#####修饰符

**事件修饰符**   当时那触发后。让事件的功能变的更强，简化原生阻止冒泡，进制默认行为

@事件名.修饰符=“methods里的函数”

@事件名.修饰符 如果不需要做什么，可以不用methods的函数 

**.stop**          e.stopPropagation( )

**.prevent**    e.preventDefault( )

可以去官网看其他事件修饰符：.once 只触发一次  .passive 默认行为是滚动时触发  .self  .capture

```vue
<template>
<div>
    <div @click="goto">
        <button @click.stop>点他</button> 
         <a href="" @click.prevent></a>      
    </div>
    
</div>
</template>
<script>
</script>
```

**键盘修饰符**  监听用户用哪些键  .enter  .esc

.enter 监听回车键  按下enter 触发

.esc 监听取消键  按下esc 触发

.up .left .right .down .tab .delete .space .....

```vue
<template>
<div>
  <input type="text" @keydown.enter="keyDownFn">  
</div>
</template>
<script>
     export default{
         methods:{
             keyDownFn(){
                 console.log(1)
             }
         }
     }
</script>
```

系统修饰键里面还有**.exact修饰符**和鼠标按钮修饰符

小案例---翻转世界

```vue
<template>
<div>
  <h3>{{msg}}</h3>
  <button @click="reverse">逆转世界</button>    
</div>
</template>
<script>
    export default{
        data(){
        return{
             msg:'hello.world'  
        } 
        },
        methods:{
         reverse(){
            this.msg=this.msg.split('').reverse().join('')
          }  
        }    
    }
</script>
```

##### v-model双向绑定 （只有表单元素 和 组件上）

 **value和Vue数据变量 双向绑定到一起**

语法： v-model="Vue数据变量"  只有表单元素配合  input 框 range radio checkbox  select textarea .....

双向数据绑定： 变量变化->视图自动同步

​                            视图变化->变量自动同步

```vue
<template>
<div>
   <span>用户名</span>
    <input type="text" palceholder="请输入用户名" v-model="usename">
   <span>密码</span>
   <input type="password" palceholder="请输入密码" v-model="passward">
</div>
</template>
<script>
    export default{
        data(){
            return {
              usename:'',
              password:''
            }
        }
    }
</script>
```

**其他表单控件 select radio checkbox textarea** ...

```vue
<template>
<div>
   <select name="" id="">    v-model写在select上 关联的值在option的value
        <option value="BKN">篮网</option>
        <option value="PHX">太阳</option>
        <option value="LOS">快船</option>      
    </select>
-------------------------------------------------------------------------
    <input type="radio" v-model="gender" value='man'>男
    <input type="radio" v-model="gender" value='woman'>女
-------------------------------------------------------------------------
    <input type="checkbox" v-model="selected">同意协议
-------------------------------------------------------------------------
     <input type="checkbox" v-model="hobby" value="smoking">抽烟
     <input type="checkbox" v-model="hobby" value="drink">喝酒
     <input type="checkbox" v-model="hobby" value="codeing">写代码
------------------------------------------------------------------------    
  <textarea name="" v-model="info"></textarea>
    </div>
</template>
<script>
    export default{
        data(){
            return {
              team:'BKN' ,      这个是option上的value值
              gender:'woman' ， 这个是input上的value gender有值，就是默认选择
              selected:true ，  数据是布尔值的时候 v-model绑定的是checked属性
              hobby:['drink'],  数据是数组的时候，v-model绑定的是value属性 
              info:""           绑定的是textarea的value值
        }
        }
    }
</script>
数据的初始值会影响表单默认状态
```

------

js原生获取表单的值并赋值：获取表单元素dom节点，用.value( )获取到值，赋值给某个变量

js数据变量改变，同步显示给表单：把获取变量的值，再赋值给表单value

-----

#####v-model修饰符

**.number** 尝试以parseFloat转化成数字类型,同步个vue数据变量

**.trim**  去除收尾空在字符，同步给vue数据变量

**.lazy** 在change时触发非input时触发，（把input事件变成blur事件 失去焦点 获得焦点）

```vue
<input type="text" v-model.number="age">
<input type="text" v-model.teim="motto">
<textarea v-model.lazy="intro">
<script>
 export default{
   data(){
    return {
        age:'',
        motto:''
        intro:''
    }
    }
 }
 </script>
```

