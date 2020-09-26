# 基本知识

数据驱动视图

## 基本指令

1、v-text-----innerText
2、v-show ----- innerHTML
3、v-if=“isshow” 就像是添加删除DOM操作一样。如果操作的值为false则不渲染，如果为true则渲染 v-if是真正的条件渲染相当于appendChild()和removeChild() v-if比v-show的切换开销更大
4、v-show=“isshow” 只要由则都会渲染，但是是根据样式display来显示隐藏display:none|block
5、v-bind绑定标签上的属性（系统的属性，自定义的属性）
v-bind:class=“” v-bind:src=“” v-bind:title v-bind:href
简写就是: :class=""
6、v-on绑定事件 v-on:click=“” 简写@,绑定的是元素js事件
7、v-for 循环 v-for in 可以遍历数组的数据，也可以遍历对象
8、 v-model 表单的双向绑定

## vue的双向数据绑定

数据劫持加发布者和订阅者模式

双向绑定只能体现在UI表单控件上
使用 v-model进行表单数据的双向绑定
data就是model，input等html标签就是视图

v-bind="msg", 起初data中的msg变量会使视图显示的内容，然后你修改input的显示内容，同样就会影响data中的msg的内容。

## 组件

局部组件不能在其子组件内使用，如果要使用就需要使用
```javascript
var componentA = {}
var componentB = {
    components: {
        'component-a':componentA
    }
}
```
全局组件声明完就挂在根app上了，就可以在任何组件内使用。
```javascript
Vue.component('ComponentC',{})
```

### 父子组件的传值

父组件传子组件

1. 一级嵌套
$emit + props 
1 在父组件data内声明给子组件的数据，然后进行绑定
2 在子组件内使用props进行接受
3 在子组件内任意使用
子组件传父组件
1 在父组件绑定自定义事件
2 在子组件中触发原生的事件，在函数内使用$emit触发在父元素中绑定的自定义事件

2. 多级嵌套
组件间传 $attrs + $listener 
v-bind="$attrs" v-on="$listenners"
起始组件还是使用的$emit + props

3. 兄弟组件
中央事件总线, 传递的时候必须使用bus事件绑定
var bus = new Vue()
接受 bus.$on('事件')  传递bus.$emit()

1\$ 比如 APP --> A --> B -->C 传值的顺序
App给C传值 ，给A组件使用 v-bind的方式进行传值，则也是在A内使用props接受。
然后是给B绑定$attrs即 v-bind="$attrs"，后再给C绑定，子啊C组件内就可以使用$attrs得到

如果是C往APP传，首先是先绑定一个click事件，然后使用在该事件内使用 $emite将事件触发出去，让后在B内使用$listeners接受
在A内也使用$listener接受 
插槽的作用是当声明一个组件的时候，还想再调用的时候在向组件内传值，所以使用一个插槽先站好位置。
比如声明一个按钮的组件，在不同的组件内使用可能有不同的名字，比如提交、撤回等，所以可以提前使用一个插槽，方便往里边传递信息
为了设置不同的内容插在组件的不同的位置，还可以使用具名插槽 <slot name="position"> </slot> 使用的时候就是 <h1 slot="position"></h1>

## 过滤器

局部过滤器
> 在组件内部使用filters{myfilter(){return ''}}定义一个过滤器。使用 {{value | myfilters}}
全局过滤器
> Vue.filter('filtername',function(value){})

## watch 监听
```javascript
watch: {
    监听基本类型属性: function(newvalue,oldvalue){

    }
}
watch: {
    数组/对象类型 {
    deep: true,
    handler: function(newvalue,oldvalue){
        }
    }
}
```

## 计算属性

computed是一个对象，默认返回的是一个getter方法。
> 即 getIndex: function(){return 监听的对象}

## 生命周期钩子

beforecreate
created 已经实现数据观测和方法的运算。但是还没有实现挂载$el。
在created钩子中一般进行ajax的数据获取
beforemount  在挂载开始之前被调用，相关的render函数第一次被调用
mounted 实例被挂载后调用，这时候el被新创建的vm.$el替换，如果根实例被挂载到一个文档内的元素上，那么当mounted 被调用的时候vm.$el也在文档内、
[mounted不会保证所有的子组件也被一起挂载，如果要等到整个视图被渲染完毕，可以在mounted内部使用vm.$nextTick]
beforeUpdate 数据更新的时候调用，在这里时候在更新之前访问现有的DOM
updated 数据更新以后会导致虚拟DOM重新渲染和打补丁，在这之后会调用该钩子
当钩子调用的时候，组件DOM已经更新，可以执行依赖DOM的操作。最好是避免在此期间更改状态
可以使用计算属性或者watche取代
[update不会保证所有的子组件被一起渲染，如果希望等到整个视图都重绘完毕，可以在update里卖弄使用nextTick]
beforeDestroy
destroyed

## vue router

### vue中获取DOM元素
使用 ref属性来获取原生DOM对象，但是在mounted钩子才能找到
 > <button ref='btn'> console.log(this.$refs.btn)

如果是给组件绑定的ref，那么this.refs获取的是当前的组件对象

### vue-router

1. 单页面应用(SPA)。
锚点值（hash）改变后，不会立刻发送请求，而是在某个合适的时机，发起Ajax请求，页面局部渲染。 优点： 页面不立刻跳转，用户体验好

在非模块开发的时候，Vue.use(VueRouter)可写可不写，因为源码中就有写

2. 使用的时候
```javascript
var router = new VueRouter({
    routes:[
        {path:'login',
        component:},
        {
            path: 'rigister',
            name:'rigister',
            component:Rigister
        }
    ]
})
```
在new Vue 的组件内挂载， router 
vue-router 抛出两个全局组件，
router-link --> 相当于 a,其中 to===href 
router-view 路由组件的出口 
3. 一般路由使用方式
<router-link to="/login"></router-link>
<router-view></router-view>
命名路由的使用方式
<router-link :to="{name:'rigister}"></router-link>

地址栏上的路由范式
（1）xxx.html#/user/1 ----params 动态路由
在使用vue-router写路由的时候是使用冒号的形式
path:'/user/:id' 
id就是动态路由，1，2...等内容
<router-link :to="{name:'user', params:{id:1}}"></router-link>
（2）xxxx.html#/user?userId=1 -----query 查询路由
路由查询的方式，也是动态的路由
<router-link :to="{name:'user',query:{userId:1}}"></router-link>

4. router-vue 有两个可以操作的对象，$router,$route
可以使用$route获取到路由的 name, path, hash, query,params等信息
比如this.$route.params.id就获取到了id
$router 就是 VueRouter对象
5. 嵌套路由
在组件内还有组件内容
在该组件内使用<router-link to="home/song">
路由定义位
```javascript
{
    path:'/home',
    name:'home',
    components: Home,
    children:[{
        path:'song',
        component:Song
    }]
}

```
6. 当不同的params渲染的是同一个组件的时候，原来的组件会被复用，因为两个路由渲染着同个组件，比起销毁再创建，复用则会显得更有效果，这也意味着生命周期钩子不会再被调用。
<router-link :to="{name:'comDesc', params:{id:'frontend'}}">前端 </router-link>
<router-link :to="{name:'comDesc', params:{id:'backend'}}">后端 </router-link>
 { name:'comDesc', path: '/timeline/:id', component: ComDes}
不同的id的时候渲染的是同一个页面ComDes
可以使用
watch进行监听。watch:{'$route'(to,from){}}

7. keep-alive, 是将组件缓存起来。本来组件点击切换的过程就有重新创造和销毁的过程，但是如果使用keep-alive以后就不会重新创建和修改

8. 重定向 redirect:'/home'
 > { path: '/', redirect: '/home'} 当访问 ‘/’的时候就会重定向到'/home'

9. meta 做用户认证的时候使用
在路由切换的时候使用全局守卫beforeEach()
具体见 axios.html或者beforeEach.html两个

## axios 

## webpack

1. 打包过程
webpack ./main.js build.js,这样就实现了打包，其中build.js是编译以后输出的内容。需要在index.html中使用
再package.json 中设置 "build":"webpack ./main.js ./build.js"
然后 npm run build 就可运行

2. 声明导出引用方式
声明并导出  export var num1 = 2
声明再导出 var num2 = 3; export{num2}
声明导出函数 export function add(x,y){return console.log(x+y)}

使用 import {num1,num2} from './app.js'

3. webpack 打包模块的源码，执行顺序
（1）把所有模块的代码放入到函数中，用一个源码数组保存起来
（2）根据require传入的数组索引，能够知道需要哪一段代码
（3）从数组中，根据索引取出包含我们代码的函数
（4）执行该函数，传入一个对象module.exports
（5）调用该函数以后，module.exports从原来的空对象就变成有值的对象
（6）最终return module.exports作为require函数的返回值

4. 自动编译webpack，配置一个webpack.config.js文件，文件内容为
```javascript
module.exports = {
    // 入口，可以有多个入口
    entry: {
        "main":'./main.js'
    },
    output:{
        filename:'./build.js'
    },
    watch:true, // 问价监视改动自动产生build.js
} 
```
webpack 其他config的配置
在package.json中配置
> "dev": "webpack --config ./webpack.dev.config.js",
> "prod":"webpack --config ./webpack.prod.config.js"

在cmd中执行 npm run dev 或者 npm run prod就可以

对于 webpack.config 的配置的时候，对于css'文件需要不同的loader
loader，对于css文件和js文件需要 style-loader css-loader

[解析图片使用的是url-loader和file-loader]
[解析css使用的是style-loader和css-laoder]
[对于比较小的文件建议使用base-64编码]
配置loader的时候
```javascript
module:[
    {
        rules:[
            {
                test:/\.css$/,
                use:[
                    'style-loader',
                    {loader:'css-loader',options:{module:true}}
                    ]
            },
            {
                test:/\.(jpg|png|jpeg|svg|gif)$/,
                 use:['url-loader']
            }

        ]
    }
]
```
'url-loader?limit=60000'当图片大小小于限制的就会认为是一张小图，就会生成base64,大于的时候就会编码和html混合在一起，加大了html文件的大小

配置less-loader的时候非常麻烦，需要先下载less
然后配置的时候还要一起配置style-loader和css-loader
```javascript
{
    test:/\.less$/,
    use:[
     'style-loader',
     'css-loader',
     'less-loader'
    ]
}
```
### webpack-dev-server 
-open 自动打开浏览器
-hot 热更新，在不刷新的时候替换css
-inline 自动刷新
-port 9999 指定端口号
-process 显示编译进度
-dev-server --open --hot --inline
配置的时候,webpack的package.json文件内配置
"dev":"webpack-dev-server --open --hot --inline --config ./webpack.dev.config.js"

主要是版本问题，新版本的webpack和新的webpack-cli，以及webpack-dev-server的版本都要差不带多，如果新旧程度不一就会报错

### 解析es6的语法
 babel-loader 将es6的代码进行转译
 babel不能解析iterator、generator、set、map、Symbol、Promise等内容，需要使用babel-plugin-transform-runtime
 还要下载依赖包babel-core 以及babel-preset-env
,然后配置babel的loader部分
```javascript
{
            test:/\.js$/,
            exclude:/node_modules/,
            use:[
              {loader:'babel-loader',
              options:{
                  presets:['env'], // 处理关键字的
                  plugins:['transform-runtime'] // 处理es6函数的
              }}
            ]
          }
```
### 单文件引入

引入.vue格式的组件，需要在module的rules里边进行相应的配置
```javascript
{
    test:/\.vue$/
    use:['vue-loader']
}
```
在入口函数main.js中需要使用运行时编译，即render:c=>c(Components)

### chunk

### webpack 异步加载

## vue-cli的基本内容

1. 当数据变化的时候，vue是怎么更新节点的？
渲染真实的DOM开销很大，当修改的某个数据渲染到真实的DOM上可能会引起整个DOM树的重绘和重排，vue为了提高渲染效率使用的是虚拟DOM，即根据真实的DOM生成一颗虚拟DOM树，当某个节点的数据发生变化的时候，就会将事件添加到异步更新队列，在事件结束就会去清空队列，在清空队列的的过程中所有的watcher就会去执行更新函数，更新函数在执行的时候就会调用组件的渲染函数和组件的更新函数，从新渲染页面，这个就会触发diff去对新的vnode和oldvnode进行对比,对于不一样的地方就直接修改到真实的DOM上.
