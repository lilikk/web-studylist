### CDN配置--内容分发网络

依靠部署在各地边缘服务器,通过中心平台的负载均衡,内容分发,调度等功能模块,==使用户就近获取所需要的内容,降低网络拥塞,提高用户访问响应速度和命中率==

**作用**

>1减少应用打包出来的包体积
>
>2加快静态资源的访问
>
>3利用浏览器缓存,不会变动的文件长期缓存

==使用环境变量进行区分==

在开发环境时,文件资源还是在本地node_modules中取出

在生产环境时,需要使用外部资源

使用CDN步骤

1创建需要排除包对象

```js
let externals={}
```

2需要配置的CDN链接

```js
let CDN={css:[],js:[]}
```

3判断是不是生产环境

```js
const isProduction = process.env.NODE_ENV === 'production'
或者
const isProduction = process.env.ENV === 'production'
//ENV是自定义的,在.env文件中,定义的环境变量
```

```js
if (isProduction) {
  externals = {
    /**
      * externals 对象属性解析：
      * '包名': '在项目中引入的名字'
      * 以 vue 举例 我再 main.js 里是以
      * import Vue from 'vue'
      * Vue.use(Vue)
      * 这样引入的，所以我的 externals 的属性值应该是 Vue
    */
    'vue': 'Vue',
    'element-ui': 'ELEMENT',
    'xlsx': 'XLSX'
  }
//*注意,element的话暴露出的名字是ELEMENT --从element官网CDN安装那直接复制
```



```js
CDN = {
    css: [
      'https://unpkg.com/element-ui/lib/theme-chalk/index.css' 
        // element-ui css 样式表
    ],

    js: [
    
      'https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js',
      
      'https://unpkg.com/element-ui/lib/index.js', // element-ui js

      'https://cdn.jsdelivr.net/npm/xlsx@0.17.0/dist/xlsx.full.min.js'
    ]
  }
```

