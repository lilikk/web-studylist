### vant组件库

1提供60多个高质量组件，覆盖移动端各类场景

2组件平均体积不到1k

3自定义主题和按需引入 支持v2 v3

#####使用步骤：

**1下载**

` npm i vant -S`  `yarn add vant -S`

**2导入**

**全部导入**：

 `import Vant from 'vant'`

`import 'vant/lib/index.css'`

`Vue.use(Vant)`

**按需导入：**

下载babel-plugin-import插件---将import引入的文件解析成按需导入方式

`npm i babel-plugin-import -D`

在 babel.config.js 中配置

```
module.exports = {
  plugins: [
    ['import', {
      libraryName: 'vant',
      libraryDirectory: 'es',
      style: true
    }, 'vant']
  ]
};
```

可以在代码中直接引入 Vant 组件 

 插件会自动将代码转化为按需引入形式 `import { Button } from 'vant';`

vant常用功能:导航栏,弹出框,轻提示,下拉刷新,上拉加载,动作面板,表单,cell单元格,tabbar切换...

### 网易云案例

服务器和服务器之间不存在跨域 本地服务器和项目本身是同源的,也不存在跨域

反向代理：本地建一个服务器（cors）负责请求转发 --接收回传

==路由规则中添加meta项 路由元信息==，通过meta字段给路由对象添加更多的信息恰当时机通过$route或者配合全局前置路由守卫获取信息

```js
{
    path:'/login',
    component:Login,
    meta:{
        title:'首页'
    }
}
可以通过this.$route.meta.title就可以取到里面的值
```



比如克隆下一个项目,看一下package.json,安装一下依赖(npm i)

网易的nodejs服务器是==伪造了请求头==实现请求真正服务器的数据

==S是核心依赖包,上线之后也需要   D是开发阶段使用的,上线之后不需要==



#####导入导出

**commonJs(nodejs规范)**

默认导入导出

导出:module.exports  只能写一个

导入:require('xxxx')

按需导出 exports bfn=xxx

按需导入 const {xxx}=require('xxxx')

**ES6规范**

默认导出 export default 只能写一次

默认导入 impor x from'xxx'

按需导出export const=xxx(定义并导出)  export  {xxx} (先定义,再导出)

按需导入 import {xx} from 'xxx'

###v-for 更新机制级原理

v-for数组发生变化:

原数组发生改变的方法(pop,push,unshift,shift,splice,reverse,sort)

有的数组方法不会导致v-for页面更新,需要拿到返回的新数组重新赋值回去,或者调用this.$set()方法,更新某个值

==v-for更新原理==

==循环出新的虚拟dom和旧的虚拟dom结构对比,尝试复用标间就地更新内容==

真实dom是:在document对象上,渲染到浏览器上显示的标签

虚拟dom是:本质是保存了==标签类型==,==属性==,==子节点==的一个js对象--在template标签内写的就是虚拟dom

vue先将template标签转成虚拟dom,在将虚拟dom映射成真实dom,然后在渲染

真实dom上的属性很多(一个p标签上就有将近300个属性),如果对比新旧差异,要遍历300多次,性能不佳

但是虚拟dom,本身是一个js对象,只需要遍历一次就可以了,性能提升

####diff算法

同级比较(就是找不同)

vue中是同级比较的  ---根元素变化:整个dom树删除重建

​                                        ---根元素不变--属性改变更新属性

==:key的作用:基于key来比较新旧虚拟dom,移除key不存在的元素==

key值是不重复的字符串或数字,提高vue更新性能

先产生新旧虚拟dom,根据key比较,就地更新

#### v-model原理

v-bind和@input事件

v-bind绑定value值,添加一个input事件,当输入值之后,触发input事件,获取value值赋值到数据中

#### watch和computed区别

watch是监听某个数据的改变,可以侦听异步请求(比如某个数据变了,可以发请求),没有缓存

computed:是依赖数据发生变化,改变数据,只能是同步的,有缓存

####mvc和mvvm区别

m是模型  v是视图 c是控制器 mvc 是后端是一种开发思想

mvvm中 m和v没有变化,vm实例是个组件,把数据和视图进行双向绑定,当数据发生变化,vm更新视图,当视图发生变化,vm更新数据

vm视图模型对视图进行数据绑定,指令,事件绑定,等,一旦视图发生变化,更新数据,当数据发生变化,驱动视图变化

#### 扁平化

把多维转化成一维

let arr =[1,2,[3,4,[5,6]]]  ==>[1,2,3,4,5,6]

可以将任意层转成1层:arr.flat(Infinity)

#### Object.defineProperty

监测对象,也是vue2响应式原理

```js
Object.defineProperty(对象,属性,{
get(){//获取触发 return},
set(){//设置触发}
})
//监听某个对象的某个属性 在对应的get 或 set
```

#### 发布订阅者模式

vue中的子传父就是发布订阅者模式,一边监听,一边派发

####防抖和节流

lodash官网,封装了很多好用的函数

防抖:在一段时间,只执行最后一次---(表单输入的时候常用)

节流:在一段时间,只执行一次---(滚动事件常用,触发频繁)

#### 深拷贝

只对引用类型来说:function array object ,浅拷贝只是复制的在栈上的引用地址,深拷贝是在栈上开一个新的.两个不会有关联,一个变化,不会改变另一个

通过JSON.parse()和JSON.stringify()实现深拷贝

递归

#### 热更新

用户在无感的情况下更新

1开发阶段(webpack)

2上线的时候用cdn(内容分发网络（CDN）是一种新型网络构建方式,基本原理是广泛采用各种缓存服务器，将这些缓存服务器分布到用户访问相对集中的地区或网络中，在用户访问网站时，利用全局负载技术将用户的访问指向距离最近的工作正常的缓存服务器上，由缓存服务器直接响应用户请求)

#### 浏览器(http)缓存机制

永久缓存和协商缓存通过不同的字段进行判断

#### 路由懒加载

用户没有进页面,就不加载,把component写法换成函数的形式()=>import('路径')