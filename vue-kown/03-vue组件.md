### vue组件

- [x] 组件概念，创建，使用
- [x] 组件通信
- [ ] 

----------

#### 组件概念

站在代码层看：可复用的Vue实例，组件的组成=封装标签+样式+js代码==web的三大组成==

视图看：独立工作的小型视图

思想：把页面上'可重用的部分'封装为’组件‘，从而方便项目的开发和维护

**基础使用**

1创建组件：创建.vue文件，封装当前组件的html+css+js

2导入组件（被其他vue文件使用）：import  组件对象 from 'vue文件相对路径'

3注册组件：（1）局部注册：当前导入的vue文件中：`components:{'组件名'：组件对象}`

​                      （2）全局注册：在main.js中 ·、`Vue.component（'组件名',组件对象）`

4使用组件：把注册时候的组件名，当成一个自定义标签使用（在模板中使用）在APP.Vue中用

组件最终运行结果：最终会被组件自身的template的标签替换掉(组件中的html结构样式)

```vue
组件放在components文件夹里面 大驼峰命名vue文件 只放相关的html和css的代码
*组件文件名：components/Add.vue* 
                                         1创建组件
<template>
  <div>
      <p>{{num}}</p>
      <button @click="add">加1</button>
  </div>
</template>

<script>
export default {
data(){
    return {
        num:0
    }
},
 methods:{
     add(){
         this.num+=1
     }
 },
}
</script>

<style>
    p{color:'red';
    font-size:'20px'}
</style>
```

```vue
这个是App文件（或者是说要引入add组件的文件）
<template>
  <div>
      <Add></Add>                        4使用组件
  </div>
</template>

<script>
import Add from './components/Add.vue'   2引入组件
export default {
components:{                             3注册组件
    Add:Add
},
data(){
    return {
        num:0
    }
},
 methods:{
     add(){
         this.num+=1
     }
 },
}
</script>
全局组件和这个步骤是一样的，只不过在main.js 上引入组件 注册
import Vue from 'vue'
import 组件对象 from 'vue文件路径'
Vue.component("组件名", 组件对象)
在Vue实例化之前
```

**组件的命名规范**

组件创建时的名字（vue文件名）----大驼峰

组件对象的名字和创建时的名字一样

组件注册的名字----推荐大驼峰

组件使用的时候：标签名和组件名保持一致（组件名，组件对象）单标签：<组件名 />  双标签 ：<组件名>可包含内容</组件名>

**组件 scoped原理作用**

**原理**：webpack打包的时候，给当前组件内的所有标签都加一个data-v-[hash:8]的值的属性

**获取**：css选择器都被添加[data-v-hash值]的属性选择器

-----

####组件通信

从一个组件传值到另一个组件

**1 父组件->子组件**（在父引子     被引入的是子组件 ）----------**子接父给**

 步骤： （1）子组件内用props选项接收自己需要的数据变量

​              （2）父组件内，在组件上，用自定义属性方式传给(属性min必须和接收的名字保持一致)

```vue
步骤：1子组件用props选项接收自己需要的数据变量
---------------------------------------------------------------------子文件PContent.vue
<template>
<div>
     <p>{{num}}</p> 
     <p>{{info}}</p>
     <p>{{title}}</p>
</div>
</template>
<script>
    export default{
       props:['num','info','title']
    }
</script>
2父组件内，在组件上，用自定义属性方式传
----------------------------------------------------------------------父文件App.vue

<template>
<div>
    <PContent :num="obj.num" :info="obj.info" :title="obj.title"></PContent>
</div>
</template>
<script>
    import PContent from './components/PContent.vue'
    export default{
       components:{
          PContent:PContent
       } ,
        data(){
            return {
                obj:{
                    num:1,
                    info:'haha',
                    title:'oo~'
                }
            }
        }
       
    }
</script>
```

**2父传子配合循环**    组件+循环

组件就是一个标签，和原生标签上使用的指令效果一样，每次循环的item和组件都是独立的，互不影响

**3子组件->父组件**

子组件不能直接修改来自父组件的数据，直接修改了子组件，父组件数据并不会发生改变

**单项数据流** ：从**父到子**的数据流向，叫单项数据流

Vue 规定**props**里的变量**只能读取**，**不能修改**

---

**通过自定义事件方法实现子向父传递数据**（通过事件机制）

思路：可以通过下标，确定数据发生变化的是哪个

步骤：

1子组件内，恰当时机，派发（自定义）事件，通知父组件 `this.$emit('自定义事件名',传值)`

2父组件内，子组件上，监听（自定义）事件，`@自定义事件名称=“父组件methods函数”`

```vue
this.$emit('update',index)
@update="methods函数"
```

**子派发，父监听**

```vue
.......................................子组件
Jian.vue
<template>
<div>
    <section v-for="(obj,index) in list" :key="index">
     <p>{{obj.num}}</p> 
     <p>{{obj.info}}</p>
     <button @click="jian(index)">减</button>
    </section>      
</div>
</template>
<script>
    export default{
       props:['list'],
       methods:{
           jian(index){
               this.$emit('del',index)
           }
       }
    }
</script>
---------------------------------------父组件
<template>
<div>
    <Jian v-for="(item,index) in list" 
          :list="list"
          @del="del"></Jian>
</div>
</template>
<script>
    import Jian from './components/Jian.vue'
    export default{
      data(){
          return {
              list:[{num:66,info:'xiao'},
                    {num:33,info:'ao'},
                    {num:46,info:'xi'}]
          }
      },
        components:{
            Jian:Jian
        },
        methods:{
            del(index){
                this.list.num-=1
            }
        }
    }
</script>
```

