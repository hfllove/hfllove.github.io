

# Vue 项目

#### 1.Vue项目初始化

1. 以项目名称作为命名，建一个文件夹
2. 在这个文件夹下面，打开`cmd`命令行窗口(shift键+右键)。在命令行窗口中，输入`vue create + 项目名称`
3. 初始化的过程中，要求根据需要，选择Vue的版本(Vue2/Vue3)，初始化完成，可以按照提示，输入后续指令

#### 2.Vue项目结构

- node_modules 文件夹：项目依赖文件

- public 文件夹：一般放置一些静态资源(图片)，放在public中的静态资源，webpack进行打包的时候，会原封不动的打包到dist文件夹中

- src 文件夹(程序员源代码文件夹)：
	+ <font color="#228B22" style="font-weight:700">assets</font> 文件夹：一般是放置静态资源(放置<font color="red">多个组件的共用</font>的静态资源)，放置在assets文件夹里面的静态资源，在webpack打包的时候，webpack会把静态资源当做一个模块，打包到<font color="#C76114" style="font-weight:700">JS</font>文件里面
    + <font color="#228B22" style="font-weight:700">api</font> 文件夹：放置api接口配置文件
	+ <font color="#228B22" style="font-weight:700">components</font> 文件夹：一般放置的是非路由组件(全局组件)
    + <font color="#228B22" style="font-weight:700">store</font> 文件夹：一般放置的是 <font color="#C76114" style="font-weight:700">vuex</font>状态管理库文件
	+ <font color="#228B22" style="font-weight:700">pages/views</font> 文件夹：经常放置<font color="#C76114" style="font-weight:700">路由</font>组件 
	+ <font color="#228B22" style="font-weight:700">router</font> 文件夹：一般放置 <font color="#C76114" style="font-weight:700">配置路由</font>的JS文件
	+ <font color="#228B22" style="font-weight:700">App.vue</font>：唯一的根组件，Vue当中的组件(.vue)
	+ <font color="#228B22" style="font-weight:700">main.js</font>：程序入口文件，也是整个程序当中最先执行的文件

- babel.config.js：配置文件(babel相关)

- package.json文件：为项目的"身份证"，记录了项目有哪些依赖

- package-lock.json：缓存性文件

- README.md：说明性文件	

#### 3.项目的预先配置

- 让项目运行起来时，浏览器自动打开项目
在package.json中找到"script"这个名字，在里面的名为 <font color="#228B22" style="font-weight:700">serve</font>的配置项中，添加`--open`到`vue-cli-service serve`后面

- eslint校验工具的关闭
在根目录下，创建一个vue.config.js，关闭eslint的校验：
```javascript
module.exports = {
	// 关闭eslint
	lintOnSave:false
}
```

- 简化写法，为 <font color="#C76114" style="font-weight:700">src</font>配置别名
在根目录下，创建一个名为`jsconfig.json`的文件【@代表的就是 <font color="#C76114" style="font-weight:700">src</font>】：
```javascript
{
	"compilerOptions":{
		"baseUrl":"./",
		"paths":{
	       "@/*":["src/*"]    		
		}				
	},
	// 不能在以下文件中设置
	"exclude":["node_modules","dist"]
}	
```	

#### 4.项目路由分析【pages/views文件夹】

##### 1.前端路由
+ <font style="font-weight:700">概念 </font>：
前端路由就是 <font color="#C76114" style="font-weight:700">KV键值对</font>，其中key为 <font color="#ff4500" style="font-weight:700">URL</font>(地址栏中的路径)，value就是 <font color="#ff4500" style="font-weight:700">相应的路由组件</font>
+ <font style="font-weight:700">使用步骤</font>：
	1. 书写静态页面(HTML + CSS)
	2. 拆分组件
	3. 获取服务器的数据动态展示
	4. 完成相应的动态业务逻辑

##### 2. 非路由组件
+ 创建或者定义
+ 引入
+ 注册
+ 使用

##### 3. 路由组件的搭建

3.1 下载与当前项目版本合适的`vue-router`插件

```javascript
npm install vue-router@3 -S
// 或
npm install --save vue-router@3
```

3.2 在`src`文件夹下，创建名为`pages`或`views`的文件夹，用来放置路由组件

3.3 在`src`文件夹下，创建名为`router`的文件夹，在router文件下创建一个`index.js`，用来进行路由配置，`index.js`配置如下：

```javascript
// 配置路由
import 'Vue' from 'vue'
import 'VueRouter' from 'vue-router'
// 使用插件
Vue.use(VueRouter)
// 引入路由组件，例如
import Home from '@/pages/Home'
// 暴露路由组件
export default new VueRouter({
    // 配置路由
    routes:[
        {
            path:"/home",
            component:Home
        }
    ]
})
```
然后在`main.js` 入口文件处引入，

```javascript
// 引入路由
import router from '@/router'
// 注册router
new Vue({
render: h=>h(App),
    // 注册路由，由于KV一致，所以省略V
    router
})
```
最后，可以在组件中进行展示，

```
<!-- 路由组件出口的地方 -->
<router-view></router-view>
```
3.4 $route 与 $router的区别

- <font color="#C76114" style="font-weight:700">$route</font>: 一般获取路由信息【路径、query、params等等】
- <font color="#C76114" style="font-weight:700">$router</font>: 一般进行编程式导航的路由跳转【replace|push】
><font color="#228B22" style="font-weight:700">注意</font>：不管是路由组件，还是非路由组件，身上都有$route,$router属性

3.5 路由的重定向

- 当项目初始运行时，为了方便，需要定义一个初始的路径

```javascript
{
    path:"*",
    redirect:"/home"
}
```

##### 4.路由的跳转

- 路由的跳转有两种形式：
    1. 声明式导航`router-link`
    2. 编程式导航`replace`/`push`

##### 5.路由的元信息

- 配置路由时，可以添加路由元信息【meta】，给路由附加一些限制信息

```
<!-- vue的router配置文件中 -->
{
    path:"/home",
    component:Home,
    meta:{show:true}
}
<!-- 对应组件上 -->
<Footer v-show="$route.meta.show" />
```

##### 6.路由参数params&query

- <font color="#C76114" style="font-weight:700">params</font>参数：属于路径当中的一部分，在配置路由时，需要占位

```
<!-- 路由配置文件 -->
{
    path:"/home/:id/:name",
    name: 'home',
    component:()=>import('@/views/home')
}
```
>1.使用params传参只能使用name进行引入

>2.配置路由的时候，在占位(/home/:keyword)的后面加上一个？，代表 <font style="font-weight:700">params参数可传可不传</font>

- <font color="#C76114" style="font-weight:700">query</font>参数：不属于路径当中的一部分，类似于ajax中的queryString `/home?k=v&kv=,`，不需要占位

- 路由传递参数方式(组件中)：
    1. 字符串形式
    ```
    this.$router.push("/search"+this.keyword+"?k="+this.keyword.toUpperCase());
    ```
    2. 模板字符串
    ```
    this.$router.push(`/search/${this.keyword}?k=${this.keyword.toUpperCase()}`)
    ```
    3. 对象(<font color="#C76114" style="font-weight:700">常用</font>)
    ```
    <!-- 在路由配置文件中 -->
    {
        path:"/search/:keyword",
        component:()=>import('@/views/search'),
        name:'search'
    }

    <!-- search组件中 -->
    this.$router.push({name:"search",params:{keyword:this.keyword},query:{k:this.keyword.toUpperCase()}})
    ```
- 路由组件传递 <font color="#C76114" style="font-weight:700">props参数</font>方式
    1. 布尔值写法：params
    ```
    <!-- 配置路由中 -->
    {
        path:"/search/:keyword",
        component:()=>import('@/views/search'),
        name:'search',
        // 布尔值写法
        props:true
    }

    <!-- ；路由组件中 -->
    export default {
        name:'',
        props:["keyword"]
    }
    ```
    2. 对象写法
    ```
    <!-- 额外的去给组件传递参数 -->
    <!-- 配置路由中 -->
    {
        ......
        props:{a:1,b:3}
    }
    <!-- 路由组件中 -->
    export default {
        ...
        props:['a','b']
    }
    ```
    3. 函数写法(不常用)
    ```
    <!-- 配置路由中 -->
    <!-- 可以用函数的形式拿到params参数，或者query参数，通过props传递给路由组件 -->
    {
        ...
        props:($route)=>{
            return {
                keyword:$route.params.keyword,
                k:$route.query.k
            }
        }
    }

    <!-- 路由组件中 -->
    export default {
        ...
        props:['keyword','k']
    }
    ```
    >($route)=>{...} <font color="#C76114" style="font-weight:700">去掉return之后</font>，箭头函数右边被认为是一个 <font style="font-weight:700">函数体</font> ，所以要加上 <font color="#C76114" style="font-weight:700">()</font>,即`($route)=>({...})`

##### 7.重写push/replace方法

- 由于vue-router的编程式导航，返回的是一个Promise的对象，需要 <font style="font-weight:700">接收成功或失败的返回结果</font>。所以，需要对路由导航的push/replace进行重写
```javascript
VueRouter.prototype.push = function (location,resolve,reject) {
    if(resolve&&reject) {
        // call || apply 区别
        // 相同点：可以调用函数一次，都可以篡改函数的上下文
        // 不同点：call传参是用逗号分隔，apply是数组
        // 若不传参数，则this的指向为windows
        originPush.call(this,location,resolve,reject)
    }else{
        originPush.call(this,location,()=>{ },()=>{ })
    }
}
```

##### 8.axios二次封装(插件)【api文件夹】

- 服务器发请求的方式： <font style="font-weight:700">XMLHttpRequest</font>、 <font style="font-weight:700">fetch</font>、 <font style="font-weight:700">JQ</font>、 <font style="font-weight:700">axios</font>
- 请求拦截器、响应拦截器：
    1. 请求拦截器：可以在发请求之前，处理一些业务
    2. 响应拦截器：可以在服务器返回数据之后，处理一些业务

- 安装`axios`： <font style="font-weight:700" size=4>`npm install axios -S`</font>
    1. 在`src`文件夹下，新建一个`api` 文件夹，里面再创建一个`request.js`文件
    2. 配置`request.js`
    
    >1. 利用axios对象的create方法，创建一个axios实例
    
    ```javascript
    // 对于axios进行二次封装
    // 引入axios插件
    import axios from "axios"

    // 利用axios对象的create方法，创建一个axios实例
    const requests = axios.create({
        // 配置对象
        // 基础路径，发请求的时候，路径自动添加/api
        baseURL:"/api"
        // 代表请求超时的时间,5s
        timeout: 5000,
    })
    ```
    >2. 设置请求拦截器，响应拦截器
    
    ```javascript
    requests.interceptors.request.use((config)=>{
        // config:配置对象，有个headers请求头属性
        return config
    })

    requests.interceptors.response.use((res)=>{
        // 成功的回调函数，
        return res.data
    },(error)=>{
        // 响应失败的回调
        return Promise.reject(new Error('fail'))
    })
    ```
    >3. 对外暴露axios的实例对象，requests
    
    ```javascript
    export default requests
    ```

- API接口统一管理
    1. 在 <font style="font-weight:700">`api`</font>文件夹下面新建一个 <font style="font-weight:700">`index.js`</font>，并对其进行配置
    >在index.js中，实现对 <font color="#C76114" style="font-weight:700">API接口</font>的统一管理
    ```javascript
    // 引入axios实例对象，requests
    import requests from './requests'

    // 三级联动接口
    //  URL:/api/product/getBaseCategoryList  method：get  参数 ：无
    // 发请求：axios发请求返回结果为Promise对象
    export const requestCategoryList = () => requests({url:'/product/getBaseCategoryList',method: 'get'})
    ```

- 跨域问题
    1. 当前台项目服务器与后台服务器出现： <font style="font-weight:700">协议</font>、 <font style="font-weight:700">端口号</font>、 <font style="font-weight:700">域名</font>等不同时，就会有跨域问题
    2. 解决方式： <font style="font-weight:700">JSONP</font>、 <font style="font-weight:700">CROS</font>、 <font color="#C76114" style="font-weight:700">代理</font>
    3. 代理方式解决跨域
    >在 <font color="#C76114" style="font-weight:700">Vue.config.js</font>中配置webpack跨域
    
    ```javascript
    // 代理跨域
    devServer:{
        proxy:{
            '/api':{
                target:'http://gmall-h5-api.atguigu.cn',
                // 因为所有的请求都带有api前缀，所以不用重写路径
                // pathRewrite:{'^/api':''}
            }
        }
    }
    ```

#### 5.nprogress 进度条插件

- 安装： <font style="font-weight:700">`npm install nprogress -S`</font>
- 引入进度条及样式：

    >1. 在 <font size=4>`src/api/request.js`</font> 中引入
    
    ```javascript
    // 引入进度条
    import nprogress from 'nprogress'
    // 引入进度条样式
    import "nprogress/nprogress.css"
    ```
    >2. nprogress 有两个属性：start、done
    >3. start：进度条开始，done：进度条结束
    
    >1. 在请求拦截器中使用start
    >2. 在响应拦截器中使用done
    
    ```javascript
    requests...request...{
        // 进度条开始
        nprogress.start()
    }

    requests...response...{
        // 进度条结束
        nprogress.done()
    }
    ```

#### 6.vuex状态管理库(插件)【store文件夹】

##### 1.概念：vuex是官方提供的一个插件，状态管理库， <font color="#228B22" style="font-weight:700">集中式管理</font>项目中组件共用的数据
##### 2.配置项： <font style="font-weight:700">state</font>、 <font style="font-weight:700">mutations</font>、 <font style="font-weight:700">actions</font>、 <font style="font-weight:700">getters</font>

    > 1. state：存放数据的仓库
    > 2. actions：处理业务逻辑和处理异步的地方
    > 3. mutations：执行代码的地方
    > 4. getters：简化仓库数据，让组件使用更加方便

##### 3.安装 <font style="font-weight:700">vuex</font>: <font size=4>`npm install vuex@3 -S`</font>
##### 4.在 <font style="font-weight:700">`/src`</font>文件夹下面，新建一个 <font style="font-weight:700">store</font>文件夹，然后再在 <font style="font-weight:700">store</font>文件夹下面，建一个 <font style="font-weight:700">index.js</font>文件，并对其进行配置：

1. 引入vuex插件，并对外暴露Store类的一个实例

    ```javascript
    import Vue from 'vue'
    import Vuex from 'vuex'
    // 使用插件
    Vue.use(Vuex)

    const state = {}
    const mutations = {}
    const actions = {}
    const getters = {}

    export default new Vuex.Store({
        state,
        mutations,
        actions,
        getters
    })
    ```

2. 在入口文件 <font style="font-weight:700">`main.js`</font>中引入并注册：
    
    >在vue实例对象上注册完成，会在组件实例上多一个属性 <font color="#C76114" style="font-weight:700">$store</font>
    
    ```javascript
    import store from './store'

    new Vue({
        //注册仓库 KV一致，省略V
        store,
    })
    ```

3. 实现Vuex仓库模块式开发存储数据
    
    - 在 <font color="#C76114" style="font-weight:700">store</font>文件夹下面，新建一个文件夹，用组件的名字命名，比如home，再在home文件夹下面新建一个文件 <font color="#C76114" style="font-weight:700">index.js</font>
    - 完成对 <font color="#C76114" style="font-weight:700">index.js</font>的配置
    - 在 <font style="font-weight:700">/store/index.js</font>中，对home文件进行引入(引入小仓库)
    
    ```javascript
    import home from './home'
    ```

    - 实现Vuex的模块式开发存储数据
    
    ```javascript
    export default new Vuex.Store({
        modules:{
            home,
            search
        }
    })
    ```
