# 单页面应用spa (single page application)

**路由：vue本身是没有路由的，需要加载路由模块vue-router.js。

<font color='blue'>Single Page Application</font>指一种基于web的应用或者网站, 这种single page在和用户交互的时候当用户点击某个物件或者按键的时候不会跳转到其他的页面. 举个例子, 在知乎里, 如果你点击一个问题或者一个人名的时候, 网页会自动跳转到该页面. 反之, 比如说我们现在有一个基于网页版的计算器应用, 不管你点数字还是运算符号, 都不会跳转到其他页面, 仅仅是在网页的某处(计算器显示数字的地方)进行更新, 显示你刚刚所按的键或者计算的结果.



下面我们分七步来阐述spa的简单过程：

1. 引入vue-router.js模块

2. 页面中定义相关元素

3. 定义组件

4. 整合组件

5. 声明一个路由

6. 获取关联（component关联数据）

7. 开启路由

关于spa应用的几个方法：

```js
1.当在css中使用 .v-link-active 时，指的是链接活跃时的class

2.<router-view></router-view> 渲染HTML的DOM结构

3.Vue.extend({template:'模板内容'}) 用于定义一个组件

4.Vue.extend({}) 整合组件

5.new VueRouter() 声明一个路由

6.Router.map({
    '/关联的路径':{
        component:'组件名称' 
        }
    });  
获取关联（component关联数据）

7.Router.start(路由名,'作用域') 在哪个作用域下开启路由。
```

下面是具体的实例：

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
    /*.v-link-active:链接活跃时的 class */
    .v-link-active{
        color: red;
        font-size: 30px;
    }
    </style>
    <script src="js/vue.js"></script>
    <script src="js/vue-router.js"></script>
</head>
<body>
    <div id="nav">
        <ul>
            <!-- 比如：一个带有指令v-link='/a'的元素，只要当前路径以/a开头，此元素就会被判断为活跃 -->
            <li><a href="javascript:;" v-link="{path:'/index'}">首页</a></li>
            <li><a href="javascript:;" v-link="{path:'/content'}">内容</a></li>
        </ul>
        <!-- 内容展示区 -->
        <router-view></router-view>
    </div>
</body>
</html>
<script>
    //定义两个组件
    var Index=Vue.extend({
        template:'<h2>我是首页</h2>'
    });
    var Content=Vue.extend({
        template:'<h2>我是内容页</h2>'
    });
    //整合组件
    var app=Vue.extend({});
    //声明一个路由
    var Router=new VueRouter();
    //获取全部关联
    Router.map({
        '/index':{
            component:Index
        },
        '/content':{
            component:Content
        }
    });
    //开启路由
    Router.start(app,'#nav');
</script>
--------------------- 
作者：jyz6246 
来源：CSDN 
原文：https://blog.csdn.net/jyz6246/article/details/54195141 
版权声明：本文为博主原创文章，转载请附上博文链接！

