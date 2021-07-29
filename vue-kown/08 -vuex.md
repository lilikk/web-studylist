### vuex

**---数据同步，集中管理**

跨组件通信 非父子组件之间

**集中式的状态管理模式**  ---确保数据保持同步，数据修改是可追踪的

不是所有数据都要集中管理，只有多个组件共享数据，存储在vuex中

state

mutations   唯一能修改state的地方，确保调试工具可以追踪变化

只能同步代码，调试工具可以追踪变化过程

**storte使用：**

| state    | mutations    | actions      | getters      | modules  |
| -------- | ------------ | ------------ | ------------ | -------- |
| 公共数据 | 同步数据管家 | 异步数据管家 | 全局计算属性 | 模块拆分 |

> 1创建工程 vue create 项目名  创建组件
>
> 2创建store仓库 在src/store/index.js文件
>
> 下载：`yarn add vuex `
>
>  引入 Vue 和 Vuex `import Vue from 'vue'`  `import Vuex from 'vuex'`
>
> 注册Vuex `Vue.use(Vuex)`
>
> 创建store对象 ` const store =new Vuex.Store({})`
>
> 导出store对象 `export default store`
>
> 在main.js中引入`import store from '路径'`
>
> 注入vue实例`new Vue({store})`
>
> 配置store对象

基础用法

```js
const stroe=new Vuex.Store({
    state:{变量名：变量值},//映射到vue的computed:{ }
    mutations：{函数名(state,可选){修改同步代码}},//映射到vue的methods:{ }
    actions：{函数名(store,可选){异步代码，把结果给mutations}},//映射到vue的methods:{ }                       
    getter:{计算属性名(state){return 值给计算属性}},//映射到computed:{  }
    modules:{模块名:引入的模块对象}
})
```

基础取值

| 配置项    | 直接使用                           | 映射使用                     |
| :-------- | ---------------------------------- | ---------------------------- |
| state     | this.$store.state.变量名           | ...mapState(['变量名'])      |
| mutations | this.$store.commit（‘方法名’，值） | ...mapMutations(['方法名'])  |
| actions   | this.$store.dispatch('方法名'，值) | ...mapActions(['方法名'])    |
| getter    | this.$store.getter.计算属性名      | ...mapGetter(['计算属性名']) |

分模块

| 配置项 | 直接使用                        | 映射使用                                            |
| ------ | ------------------------------- | --------------------------------------------------- |
| state  | this.$store.state.模块名.变量名 | ...mapState('原变量名'，state=>state.模块名.变量名) |

开启命名空间 ---namespaced:true

| 配置项    | 直接使用                                  | 映射使用                             |
| --------- | ----------------------------------------- | ------------------------------------ |
| state     | this.$store.state.模块名.变量名           | ...mapState('模块名'，['变量名'])    |
| mutations | this.$store.commit('模块名/函数名'，值)   | ...mapMutations('模块名',['函数名']) |
| actions   | this.$store.dispatch('模块名/函数名'，值) | ...mapActions('模块名',['函数名'])   |
| getters   | this.$store.getters['模块名/计算属性名']  | ...mapGetters('模块名',['函数名'])   |

