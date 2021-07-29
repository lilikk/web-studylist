###指令(v-text v-html v-if v-show :class :style v-for)

####v-text和v-html

更新dom对象的innertext 和innerhtml   **会覆盖原来标签中的内容**

v-text="Vue数据变量"    不会解析标签  纯文本

v-html="Vue数据变量"   会解析html标签 富文本

```vue
<template>
  <div>
      <p v-text="str"></p>  //不解析
      <p v-html="str"></p>  //解析html结构
   
  </div>
</template>

<script>
export default{
      data(){
      return{
          str:'<span style="color:red;">YYDS</span>'
      }
}
}</script>

```

---------------

####v-if 和v-show

控制标签的隐藏或出现

v-show=“Vue变量”

v-show=“Vue变量”  

Vue变量是布尔类型  true是显示  false是隐藏

```vue
<template>
  <div>
      <p v-show="isShow">这个是p</p>   
      <p v-if="isShow"> 这个是p </p>  
   
  </div>
</template>

<script>
export default {
data(){
    return {
        isShow:true
    }
}
}
</script>
```

控制可见性的原理不同：

v-show 通过css的display：none控制隐藏      **频繁切换**

 v-if通过创建和插入或移除dom节点实现显示隐藏  条件渲染  

v-if/v-else-if/v-else  和多分支语句很像  条件成立渲染成立的分支

```vue
<template>
  <div>
      <input type="text" v-model="score">
      <p v-if="scroe>=90">优秀</p>
      <p v-else-if="score>=70">良好</p>  
      <p v-else>一般</p>
  </div>
</template>

<script>
export default {
 data(){
 return{
     score:''  
}
 }
}
</script>
```

案例-折叠面板

```vue
<template>
  <div id="app">
    <h3>案例：折叠面板</h3>
    <div>
      <div class="title">
        <h4>芙蓉楼送辛渐</h4>
        <span class="btn" @click="toggle">
          {{isShow?"收起":"展开"}}    //这句三元表达式也非常重要
        </span>
      </div>
      <div class="container" v-show="isShow"> 
        <p>寒雨连江夜入吴,</p>
        <p>平明送客楚山孤。</p>
        <p>洛阳亲友如相问，</p>
        <p>一片冰心在玉壶。</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShow:true
    }
  },
  methods:{
    toggle(){
      this.isShow=!this.isShow        //可以通过去返来实现切换
    }
  }
}
</script>
样式是less的需要安装less和less-loader@5.0.0 -D 
装的webpack 4 所以得指定less-loader5
```

v-if和v-show区别:

1.控制元素的显示隐藏原理是不一样的:v-show是css样式的display属性控制的,v-if是dom的创建和销毁

2都接收boolean值,但是如果一开始是false ,v-show会渲染dom,而v-if只有是true的时候,才会渲染dom

3v-show的渲染成本较高,v-if元素创建销毁成本高,所以适用场景不一样,v-show适合频繁的显示隐藏,v-if适合不频繁的

---------------------

####遍历v-for

列表渲染所在的标签结构 ，根据数组、对象循环生成

v-for=“（值，索引）in  目标结构”

```vue
<template>
  <div>
      <ul>
          <li v-for="(item,index) in arr" :key="index">{{item}}-{{index}}</li>   
      </ul>
      <p v-for="obj in strArr" :key="obj.id">姓名{{obj.name}}-年龄{{obj.age}}</p>
  </div>
</template>

<script>
export default {
data(){
    return{
        arr:["a","b","c"],    数组
        strArr:[{             数组包对象
            id:1000,
            name:"a",
            age:'111'
        },{
            id:1001,
            name:"a",
            age:'111'
        }]
    }
}
}
</script>
想循环谁就在谁上面添加v-for
:key 必须要添加，提升vue更新元素的性能（让元素更新更可快的加速） 唯一的字符串或者数值
有id用id，没有id用下标
v-for有作用域，不能写到作用域之外
遍历对象 v-for="(val,key) in obj" :key="key"（很少用）
```

案例-小专栏

```vue
<template>
  <div>
      <h3>选择喜欢的专栏</h3>
      <div>  /*插值只能用在双标签中，所以给套了个父盒子，遍历父盒子，子元素可以使用*/
          <span v-for="(item,index) in arr" :key="index">        
          <input type="checkbox" :value="item" v-model="newArr">{{item}} 
    </span>
      </div>
      <ul>
          <li v-for="(item,index) in newArr" :key="index">{{item}}</li>
      </ul>
  </div>
</template>

<script>
export default {
data(){
    return{
        arr:["科幻","毛线","篮球","足球","动漫"],
        newArr:[]  这个收集的是动态的value值
    }
}
}
</script>
```

案例-帅哥美女走一走

```vue
<template>
  <div>
     <ul>
       <li v-for="(item,index) in arr" :key="index">{{item}}</li>
    </ul>
    <button @click="getlist">走一走</button>
  </div>
</template>

<script>
export default {
data(){
    return{
        arr:["科幻","毛线","篮球","足球","动漫"],
    }
},
methods:{
getlist(){
//  this.arr.reverse()
let a=this.arr.splice(0,1)
this.arr.push(a[0])
}
}
}
</script>
```

-----------

####v-bind:class

语法：直接绑定  :class="Vue数据变量"

​           三元绑定   :class="Vue数据变量布尔值？’类名‘：’ ‘ "

​           对象绑定   ：class="{类名：Vue数据变量布尔值}"

​         静态class和动态class可以共存 

```vue
<template>
  <div>
      <p :class="activeClass">直接绑定</p>
      <p :class="isActive?'active':''">三元表达式</p>
      <p :class="{active:isActive}">对象绑定</p>
  </div>
</template>

<script>
export default {
data(){
    return {
        activeClass:'active',
        isActive:true
    }
}
}
</script>
```

------------------

####v-bind:style

动态绑定css样式

:style=“Vue数据变量对象”

:style="{css样式：Vue数据变量值}"

```vue
<template>
  <div>
      <p :style="{color,color}">我想要颜色是红色</p>
      <p :style=" styles">我想要颜色是红色</p>
  </div>
</template>

<script>
export default {
 data(){
     return {
         color:'red'
         styles:{
         color:'red'
     }
     }
 }
}
</script>
```

想要往数据的数组里面添加对象的话 需要构建和数据中的对象一样的形式

```vue
<template>
  <div>
  </div>
</template>
<script>
export default {
 data() {
    return {
      name: '', // 名称
      price:'', // 价格
      list: [
        { id: 100, name: "外套", price: 199, time: new Date('2010-08-12')},
        { id: 101, name: "裤子", price: 34, time: new Date('2013-09-01') },
        { id: 102, name: "鞋", price: 25.4, time: new Date('2018-11-22') },
        { id: 103, name: "头发", price: 19900, time: new Date('2020-12-12') }
      ],
    };
  },
 methods:{
    add(){
      if(this.name==''||this.price==""){  
        alert("资产名和价格不能为空")
        return 
      }
      let id
      if(this.list.length!=0){ id=this.list[this.list.length-1].id+1}
       else{id=1}
      let obj={
      id,
      name:this.name ,
      price:this.price,
      time:new Date()

}
      this.list.push(obj) 
      this.name=this.price=''
    }
}
 }
}
</script>

收集的数据如果是数值型的可以用  v-model.number尝试转化一下数据类型
如果是字符串类型的考虑一下头尾部空格需不需要去除 v-model.trim
表单按钮，a标签会有默认跳转的行为 可以添加@click.prevent 阻止默认行为
```

点击删除，删除所在行的列表：一定注意要有index 

注意删除的是数据 不是dom元素了，通过数据驱动视图！！！

```vue
del(index){
  if(window.confirm("确定删除么")){
  this.list.splice(index,1)}     
}
```

###filter过滤器

作用：格式转化传入值返回处理后的值 用于处理字符串格式的函数

先定义 后调用

应用：插值表达式 或者 v-bind 一起使用

全局过滤器：任意vue文件

定义过滤器：  在main.js中

` Vue.filter('过滤器的名字'，function(value){ return value处理过程 })`

调用过滤器：

 在模板中调用   **{{ 数据变量|过滤器名}}** 

 **:属性=“数据变量|过滤器名”**

局部过滤器  当前vue文件

声明在当前vue文件中

` filters：{ 过滤器名(value){ return  value的处理过程}    }`

局部小案例

```vue
1下载moment包 yarn add moment -S
2在当前的vue文件中 引入 import moment from 'moment'
3定义过滤器 
4使用过滤器
<script>
    export default{
    filters:{
    formattime(data){
    return   moment(data).format('YYYY-MM-DD')
    }
  }
   }
</script>
调用  （1）{{数据变量|过滤器名}}
     （2）:属性=“数据变量|过滤器名
```

----

###计算属性computed

**一个变量的值，依赖于另外一些数据计算而来的结果**

`computed:{计算属性名字(){return 值}}`

```vue
<template>
<p>班级一共{{getsum}}</p>  声明的时候是函数 使用时候是属性 不能加括号
</template>
<script>
export default{
    data(){
      return {
          boy:89,
          girl:2
      }
    },
    computed:{
        getsum(){
            return this.boy+this.girl   //return 值 和data属性上的变量值用法一样
        }                               //data里面的值怎么用 这个就可以怎么用
    }    
}
</script>
当计算属性依赖（就是使用到了）的数据发生变化 计算属性会重新计算
compulted计算属性不能和data数据变量重名
可以插值  可以用指令 
声明的时候是函数  调用的时候是属性
```

computed计算属性和methods可以实现相同的功能。但是有区别：

computed计算属性**带缓存**的，**会根据依赖进行缓存，多次调用同一个计算属性，会从缓存取结果**

**当依赖发生变化，计算属性会重新计算结果，并缓存新的结果**

而methods里是方法，每次调用每次执行

```vue
<script>
    export default{
    computed:{
      getadd(){
          return this.list.reduce((sum,obj)=>sum+obj.price,0)
          //let sum=0  
          //this.list.forEach(obj=>sum+=obj.price)
          //return sum
              },
      avgprice(){
          return this.getadd/this.list.length
        }
     }
    }
</script>
```

案例

1 小选影响全选 用数组的every 判断是不是每个小选被选中

2全选影响小选 ，监听全选的change事件 ，用事件对象e.target的checked属性的true和false，遍历循环小选 ，同步属性

3反选  监听点击事件，循环遍历小选中的属性，取反

### 侦听器

可以侦听data/computed属性变化

语法：`watch:{被侦听的属性名(newval,oldval){}}`

**侦听基本数据类型**

```vue
<template>
  <div>
      <p>{{count}}</p>
      <button @click="count++">加1</button>
  </div>
</template>

<script>
export default {
data(){
    return {
        count:0
    }
},
watch:{
    count(newval,oldval){
      console.log(newval,oldval)  
    }
}
}
</script>
<style>
</style>
```

**侦听复杂数据类型**

简单写法没有办法监听到数据变化

```vue
<template>
  <div>
     <input type="text" v-model="obj">
  </div>
</template>
<script>
export default {
data(){
    return {
       obj:'0' 
    }
},
 watch:{
    obj:{
        immediate:true   立即执行
        deep:true,   深度监视，对象属性值发生变化，可以监听到
        handler(newval,oldval){
            console.log(newval)
        }
    }
}
}
</script>
```

增加数据缓存功能

```vue
<template>
  <div>
      <input type="text" v-model="list">
  </div>
</template>
<script>
export default {
 data() {
     const list=Json.parse(window.localStroage.get('aa'))
    return {
      name: '', 
      price:'', 
      list:list||[
        { id: 100, name: "外套", price: 199, time: new Date('2010-08-12')},
        { id: 101, name: "裤子", price: 34, time: new Date('2013-09-01') },
        { id: 102, name: "鞋", price: 25.4, time: new Date('2018-11-22') },
        { id: 103, name: "头发", price: 19900, time: new Date('2020-12-12') }
      ],
    };
  },
  watch:{
      list:{
          deep:true,
          handler(newval){
              window.localStroage.setItem('aa',Json.stringify(newval))
          }
      }
  }
 }
}
</script>
1监听list的变化，list是复杂数据类型，所以侦听器要完整的写
2需要把最新的值存储到本地 用window.localStroage.setItem("名"，"值（字符串）")
3再次进入页面的时候，尝试去本地存储获取window.localStroage.getItem("名")
取到了就直接用取到的值，如果没取到就用默认值
```