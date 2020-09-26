## 准备知识
1. 在cmd中输入 vue ui 启动项目管理器//
使用 node ./app.j跑通接口
接口说明
接口基准地址
http://127.0.0.1:8888/api/private/v1/
接口认证采用token认证

登陆接口
请求路径 login 请求方法 post
username:admin
password:123456

//
前端与服务器之间没有跨域的问题，使用cookie或者session状态
有跨域的问题，使用token的状态
## 项目
main.js是项目的入口文件。
app.vue 是项目的基础组件。

#### 
启动的服务器
\GithubStore\Simulation-Development-Meituan\forwordandback\vue_api_server>node .\app.js

启动phpStudy

启动 vue ui
E:\GithubStore\Simulation-Development-Meituan\forwordandback\vue_shop>vue ui