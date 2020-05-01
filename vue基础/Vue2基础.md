# Vue2基础.md

## 环境搭建
vue-cli@3 搭建环境
***这些环境都是可以在cmd中的，没必要非要在gitbash内
现在可以使用npm来安装vue-cli了，你可以使用npm install -g @vue/cli，也可以使用$ npm install -g @vue/cli@3.0.1来安装指定版本的，这里我们是使用的3.0.1版本
当然如果你想卸载vue-cli，你可以使用 npm uninstall -g @vue/cli
搭建完成以后就可以创建项目了
使用 vue create [项目名字]，可能要等待几分钟，当出现[successful created project [项目名字]]证明创建成功
然后根据提示操作
cd [项目名字] npm run serve 
- Local: http://localhost:8080/
- Network: http://192.168.0.101:8080/

## vue基础
### 模板语法
1、使用 {{变量}}, 这就是模板语法，但是该变量一定要在props或者data内有定义
### 指令
#### v-html
要渲染成html中的样式，要使用v-html指令 <span v-html="变量"> 我想渲染成span的样式</span> data() {return {变量: '<span style="color:red;">hello</span>'}}
/* data是一个函数，不是一个语法，多实例共享data的问题。
组件是可以被复用的，注册一个组件本质上是创建一个组件构造器的引用，当我们真正使用组件的时候才会将组件实例化，如果data是一个对象的化，虽然data是在构造器原型链上被创建的，但是实例化的每个组件都是共享同一个data对象，当修改一个实例对象的data对象的属性的时候，原型上的data对象也会被修改。data是方法的时候，每个组件实例都有自己的作用域，每个实例就不会相互影响。
*/ 这里也是我自己百度的，并没有自己实现（汗）
#### v-bind 缩写 :
要想绑定一些属性的时候使用 v-bind:id="val"
#### v-if 条件渲染
v-once 只渲染一次 v-show条件渲染
#### 自定义指令
创建一个文件 touch 文件
/*directive*/是自定义指令，
vue.directive('自定义的指令的名字',{

})
使用钩子来告诉该指令在什么时候声明什么事情
bind : 只调用一次，第一次绑定到元素的时候调用，进行一次初始化设置 
inserted  
update 当指令的变量发生变化的时候调用
componentUpdated 指令所在的虚拟DOM节点全部更新的时候调用
unbind 元素与指令解绑的时候调用
钩子指令的参数 （el, binding, vnode, oldVnode）
el: 指令所绑定的元素，可以用来直接操作DOM
binding 一个对象 binding-name，绑定的属性的值，binding-value绑定的属性的值，
vnode Vue编译生成的虚拟节点
oldVnode 上一个虚拟节点，仅在update和componentUpdated钩子中使用
/*一个简单的自定义属性的例字*/
import Vue from 'vue'
Vue.directive('n', {
    bind: function (el, binding) { 
        el.textContent = Math.pow(binding.value, 2)
     },
    update: function (el, binding) { 
        el.textContent = Math.pow(binding.value, 2)
     }
}) 
使用 ： <div v-n="4"></div> 该操作以后就会返回 16
### 计算属性
1、具有依赖关系的数据监听,当某一个值发生变化的时候，跟他有依赖关系的别的值都会发生变化
### 类与样式
:class = "{类名：控制类名显示（true或false）}"
:class = "[类]"，data(){return {类:类名}}
### 深入了解组件
组件传值，分为静态传值和动态传值
闭环数据流： 父组件向子组件传值，子组件通过props接受，子组件使用$emit('transmitfunction',value) 向父组件传值
父组件使用 @transmitfunction="fatherhandlefunction",
在methods中定义fatherhandlefunction
在父组件内使用子组件的时候是不能往里面加东西的
<chile-component><h1>我想在这里边加一个h1</h1></child-component>
但是，加入的h1 是没有用的。要让他生效使用插槽
在原先定义的子组件内使用<slot></slot>标签，给出要插入的位置
当想在任何地方加插槽的时候，可以使用name属性
<slot name="na"> </slot>
<slot name="ba"> </slot>
<chile-component>
<h1 slot="na">我想在这里边加一个h1</h1>
<p slot="ba">这个是用组件的</p>
</child-component>

### 路由基础
第一步安装路由 npm i vue-router
配置 --使用的是ES6语法

引入
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)

配置
const routes = [
    {
        path:'/',
        components: componentsName
    },
    {
        path: '/lll',
        components:....
    }
]

实例化
const router = new VueRouter({
    routes
})

导出
export default router

路由的实例化使用
最后使用的时候还是要去main.js中的new Vue({})中进行配置

只在new Vue({router})中配置以后，其实vue-router这时候是运行时的，要把配置文件给配置上。
在根目录下创建一个 vue.config.js 然后配置其编译器 
module.exports = {
    runtimeCompiler: true
}
然后找index.html 在挂在的app内使用<router-veiw></router-veiw>标签
# Koa2的基础
此处的安装应该使用gitbash
使用 npm i -g koa2-generator
创建一个koa的项目
。。 koa2 -e 项目名称
进入该项目 cd 项目名称
然后 npm install
启动一个koas的文件
 DEBUG=koa2-learn:* npm start
使用koa2来开始一个项目的时候，
可以使用 npm start/ npm test/ npm run dev/ npm run prd
## 异步开发 async await
异步是执行一个事件，并不是立即执行，而是过一会执行。
await 必须在async函数中
await 其实就是使用同步表达式书写了异步的方式，每次都是等await输出结果才会接着执行下面的语句
## koa2的中间件
const Koa = require('koa') // 引入'koa'这个包
const app = new Koa() // 初始化一个实例，名字叫做app
这些都是中间件 koa-views koa-json
const views = require('koa-views')
const json = require('koa-json')
使用的时候
app.use(json()) 即可
创建目录 mkdir 目录名字
创建一个文件 touch 文件.类型

写一个中间件
function pv(ctx) { 
    global.console.log(ctx.path)
 }
 module.exports = function () { 
     return async function (ctx, next) { 
         pv(ctx)
         await next()
      }
  }
首先 ctx ，ctx是一个全局的控制变量，koa2的中间件就想是一个洋葱，从一个中间件进入另一个中间件，一级一级进入，然后再一级一级的出去。
所以需要一个全局对象，使得在任何一个中间件内都能取到。ctx就是这个对象
然后是导出。为什么导出的是一个函数呢
就要看前边引入使用的时候，看json使用的时候是use(json()),是一个函数的模式。
因为在该函数内还要等待下一个函数的执行。
返回的就是一个异步函数。next()交给另一个中间件处理
## koa-router
主页的 路由是使用
router.get('/', async (ctx, next) => {
  await ctx.render('index', {
    title: 'Hello Koa 2!'
  })
})
其他页面的路由只需要提供给一个接口就行
接口的提供方式是ctx.body = {}
其他的模块的路由可以模块封装
使用router.prefix('/users')
这个的例子是封装了一个users模块的路由。
里面的/user/bar 什么的就使用正常的模式书写
router.get('/bar', function (ctx, next) {
  ctx.body = 'this is a users/bar response'
})
对于其他模块的分装的路由先要引入 require
然后
app.use(users.routes(), users.allowedMethods())
## cookie 和 session
1、ctx.cookies.set()
2 ctx.cookies.get(name,[options])
## mongodb