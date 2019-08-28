[TOC]



### koa 的安装和部署

进入项目后台文件夹

```js
第一步、
       npm init   // 初始化项目后台，

第二步、安装 koa
        npm i -S koa
        
第三步、建立 app.js 页面，并输入以下内容
var koa = require('koa');
var app = new koa();

// 配置路由

// 中间件
app.use(async(ctx)=> {
	ctx.body = '你好 koa2.x'
});
app.listen(8888);

第四步、在浏览器输入 localhost:8888 会有显示
```



###安装 并引入 koa-router 路由模块

```js
npm i -S koa-router;  // 安装

const Koa= require('koa'); // 引入 koa
const router = require('koa-router')();  // 引入并实例化 koa-router 路由
const app = new Koa(); // 调用koa中间件，来启动路由
```



###Koa-router 路由实例

```js
// 第一步 引入 koa 模块
var Koa = require('koa');

// 第二步 引入 koa-router
var Router = require('koa-router');

// 第三步 实例化一个 koa
var app = new Koa();

// 第四步 实例化一个 Router
var router = new Router();

// 第五步  配置路由, ctx (context) 上下文，包含了 request , response 等信息
 router.get('/', async (ctx)=> {
	 ctx.body = '这是首页';  // 返回数据，相当与 原生例的 res.writeHead()   res.end()
 }).get('/news', async (ctx)=> {
	 ctx.body = '这是一个新闻页面';
 })
 
 
 // 第六步  启动路由
 app
  .use(router.routes())  // 表示启动路由
  .use(router.allowedMethods());  
  
  /**
   .use(router.allowedMethods()); 可以配置，也可以不配置，建议配置，此行的作用是：
   这是官方文档推荐用法，我们可以看到 router.allowedMethods()用在了路由匹配 router.routes() 之后，
   所以在当所有中间件最后调用，此时根据 ctx.status 设置 response响应头
  **/

// 监听端口
app.listen(8888);

/*
最后进行测试，在终端输入  node app.js  开启node服务器
在浏览器输入 localhost:8888   或者 localhost:8888/news  会出现相应的变化
**/
```



###Koa-router 路由的另一种写法

```js
// 第一步 引入 koa 模块
var Koa = require('koa');

// 第二步 引入 koa-router 并且实例化
var router = require('koa-router')();

// 第三步 实例化一个 koa
var app = new Koa();

// 第四步 配置路由
router.get('/', async(ctx)=>{
	ctx.body = '这是首页';
})

router.get('/news', async(ctx)=>{
	ctx.body = '这是新闻页面';
})

router.get('/newscontent', async(ctx)=>{
	ctx.body = '这是新闻详情页面';
})


// 第五步  启动路由
app.use(router.routes());  // 表示启动路由
app.use(router.allowedMethods()); 
/**
   .use(router.allowedMethods()); 可以配置，也可以不配置，建议配置，此行的作用是：
   这是官方文档推荐用法，我们可以看到 router.allowedMethods()用在了路由匹配 router.routes() 之后，
   所以在当所有中间件最后调用，此时根据 ctx.status 设置 response响应头
  **/


// 第六步 指定端口
app.listen(8889);
```



###get传值，以及获取 get 传值

```js
// 本页作用： 获取 get 传值
// 第一步 引入 koa 模块
var Koa = require('koa');

// 第二步 引入 koa-router 并且实例化
var router = require('koa-router')();

// 第三步 实例化一个 koa
var app = new Koa();

// 第四步 配置路由
router.get('/', async(ctx)=>{
	ctx.body = '这是首页';
})

router.get('/news', async(ctx)=>{
	ctx.body = '这是新闻页面';
})

router.get('/newscontent', async(ctx)=> {
	/* 在 koa2 中 Get 传值通过 request 接收， 但是接收的方法有两种： query 和 querystring
	query : 返回的是 格式化好的参数对象。
	querysting : 返回的是请求字符串 */
	// 从 ctx 中获取 get 传值
	console.log(ctx.query); // *****推荐使用此种方法  ctx.query 这种是用的最多的方式，获取的值已经格式化好了 { id: '123', name: 'zhansan' }
	console.log(ctx.querystring); // 获取到的是没有格式化的数据： id=123&name=zhansan
	console.log(ctx.url); //获取 url 地址 /newscontent?id=123&name=zhansan
	
	console.log(ctx.request); // 返回以下信息
	/*
	{ method: 'GET',
	  url: '/newscontent?id=123&name=zhansan',
	  header:
	   { host: 'localhost:9000',
		 connection: 'keep-alive',
		 'cache-control': 'max-age=0',
		 'upgrade-insecure-requests': '1',
		 'user-agent':
		  'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.119 Safari/537.36',
		 accept:
		  'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,
		 'accept-encoding': 'gzip, deflate, br',
		 'accept-language': 'en-US,en;q=0.9,ja;q=0.8' ,
		} 	
	}
	**/
	console.log(ctx.request.url); // 获取 url 地址 /newscontent?id=123&name=zhansan
	console.log(ctx.request.query); // 也可以获取 get 传值，就是写法 麻烦
	console.log(ctx.request.querystring); // 也可以获取 get 传值，就是写法 麻烦
	ctx.body = '这是新闻详情页面';
})


// 第五步  启动路由
app.use(router.routes());  // 表示启动路由
app.use(router.allowedMethods()); 
/**
   .use(router.allowedMethods()); 可以配置，也可以不配置，建议配置，此行的作用是：
   这是官方文档推荐用法，我们可以看到 router.allowedMethods()用在了路由匹配 router.routes() 之后，
   所以在当所有中间件最后调用，此时根据 ctx.status 设置 response响应头
  **/


// 第六步 指定端口
app.listen(9000);
```



###动态路由

```js
// 本页作用： 获取 get 传值
// 第一步 引入 koa 模块
var Koa = require('koa');

// 第二步 引入 koa-router 并且实例化
var router = require('koa-router')();

// 第三步 实例化一个 koa
var app = new Koa();

// 第四步 配置路由
router.get('/', async(ctx)=>{
	ctx.body = '这是首页';
})

router.get('/news', async(ctx)=>{
	ctx.body = '这是新闻页面';
})

// 动态路由
router.get('/newscontent/:aid', async(ctx)=> {
	// 获取动态路由的传值
    console.log(ctx.params);
 
	ctx.body = '这是新闻详情页面';
})

// 动态路由,传多个值
router.get('/newscontent/:aid/:cid', async(ctx)=> {
	// 获取动态路由的传值
    console.log(ctx.params);
 
	ctx.body = '这是新闻详情页面';
})

// 第五步  启动路由
app.use(router.routes());  // 表示启动路由
app.use(router.allowedMethods()); 
/**
   .use(router.allowedMethods()); 可以配置，也可以不配置，建议配置，此行的作用是：
   这是官方文档推荐用法，我们可以看到 router.allowedMethods()用在了路由匹配 router.routes() 之后，
   所以在当所有中间件最后调用，此时根据 ctx.status 设置 response响应头
  **/


// 第六步 指定端口
app.listen(9000);
```



### koa 的中间件

通俗的讲:  中间件就是匹配路由之前或者匹配路由完成做的一系列的操作，我们就可以把它叫做中间件。 

**中间件的功能包括:** 

执行任何代码。 

修改请求和响应对象。 

终结请求-响应循环。 

调用堆栈中的下一个中间件。 

如果我的 get、post 回调函数中，没有 next 参数，那么就匹配上第一个路由，就不会往下匹
配了。如果想往下匹配的话，那么需要写 **next()**

### 应用级中间件

```js
// 第一步 引入 koa 模块
var Koa = require('koa');

// 第二步 引入 koa-router
var Router = require('koa-router');

// 第三步 实例化一个 koa
var app = new Koa();

// 第四步 实例化一个 Router
var router = new Router();

// 应用级中间件
// koa 中间件
// 这个中间件可以匹配任何一个中间件
app.use(async (ctx, next)=> {
	console.log(new Date());
	await next(); // 当前路由匹配完成后，继续向下匹配，如果不写 next，路由就会终止
})


// 第五步  配置路由, ctx (context) 上下文，包含了 request , response 等信息
 router.get('/', async (ctx)=> {
	 ctx.body = '这是首页';  // 返回数据，相当与 原生例的 res.writeHead()   res.end()
 })
 
 router.get('/news', async (ctx)=> {
	 ctx.body = '这是一个新闻页面';
 })
 
router.get('/login', async (ctx)=> {
 	 ctx.body = '登录页面';
 })
 
 
 // 第六步  启动路由
 app
  .use(router.routes())  // 表示启动路由
  .use(router.allowedMethods());  

app.listen(9000);
```



### 路由级中间件

```js
// 第一步 引入 koa 模块
var Koa = require('koa');

// 第二步 引入 koa-router
var Router = require('koa-router');

// 第三步 实例化一个 koa
var app = new Koa();

// 第四步 实例化一个 Router
var router = new Router();

// 应用级中间件
// koa 中间件
// 这个中间件可以匹配任何一个中间件
app.use(async (ctx, next)=> {
	console.log(new Date());
	await next(); // 当前路由匹配完成后，继续向下匹配，如果不写 next，路由就会终止
})


// 第五步  配置路由, ctx (context) 上下文，包含了 request , response 等信息
 router.get('/', async (ctx)=> {
	 ctx.body = '这是首页';  // 返回数据，相当与 原生例的 res.writeHead()   res.end()
 })
 
 // 匹配到 new 后，路由继续向下匹配
 router.get('/news', async (ctx, next)=> {
	 console.log('这是一个新闻');
	 await next();
 })
 
router.get('/news', async (ctx)=> {
 	 ctx.body = '这是一个新闻页面';
 })
 
 
 // 第六步  启动路由
 app
  .use(router.routes())  // 表示启动路由
  .use(router.allowedMethods());  
  
  /**
   .use(router.allowedMethods()); 可以配置，也可以不配置，建议配置，此行的作用是：
   这是官方文档推荐用法，我们可以看到 router.allowedMethods()用在了路由匹配 router.routes() 之后，
   所以在当所有中间件最后调用，此时根据 ctx.status 设置 response响应头
  **/

// 监听端口
app.listen(9000);
```



### 404 错误处理的中间件

```js
// 第一步 引入 koa 模块
var Koa = require('koa');

// 第二步 引入 koa-router
var Router = require('koa-router');

// 第三步 实例化一个 koa
var app = new Koa();

// 第四步 实例化一个 Router
var router = new Router();



 // 以下代码块，不论放到任何位置，都会先被执行
 app.use(async (ctx, next)=> {
	 console.log('这是一个中间件')
	 next();
	 
	 // 如果页面找不到
	 if(ctx.status == 404){  
		 ctx.status = 404;
		 ctx.body = '这是一个 404 页面！'
	 }else{
		 console.log(ctx.url);
	 }
	 
 });



// 应用级中间件
// koa 中间件
// 这个中间件可以匹配任何一个中间件
app.use(async (ctx, next)=> {
	console.log(new Date());
	await next(); // 当前路由匹配完成后，继续向下匹配，如果不写 next，路由就会终止
})


// 第五步  配置路由, ctx (context) 上下文，包含了 request , response 等信息
 router.get('/', async (ctx)=> {
	 ctx.body = '这是首页';  // 返回数据，相当与 原生例的 res.writeHead()   res.end()
 })
 
 // 匹配到 new 后，路由继续向下匹配
 router.get('/news', async (ctx, next)=> {
	 console.log('这是一个新闻');
	 await next();
 })
 
router.get('/news', async (ctx)=> {
 	 ctx.body = '这是一个新闻页面';
	 
 })
 

 
 
 // 第六步  启动路由
 app
  .use(router.routes())  // 表示启动路由
  .use(router.allowedMethods());  
  
  /**
   .use(router.allowedMethods()); 可以配置，也可以不配置，建议配置，此行的作用是：
   这是官方文档推荐用法，我们可以看到 router.allowedMethods()用在了路由匹配 router.routes() 之后，
   所以在当所有中间件最后调用，此时根据 ctx.status 设置 response响应头
  **/

// 监听端口
app.listen(9000);
```



### koa 路由的执行顺序，流程

```js
// 第一步 引入 koa 模块
var Koa = require('koa');

// 第二步 引入 koa-router
var Router = require('koa-router');

// 第三步 实例化一个 koa
var app = new Koa();

// 第四步 实例化一个 Router
var router = new Router();


 // 以下代码块，不论放到任何位置，都会先被执行
 app.use(async (ctx, next)=> {
	console.log('1. 这是第一个中间件 1');
	await next();	
	console.log('5. 匹配路由完成后，又会回来执行中间件');	 
 });


 app.use(async (ctx, next)=> {
	console.log('2. 这是第二个中间件 2');
	await next();	
	console.log('4. 匹配路由完成后，又会回来执行中间件');	 
 });



// 第五步  配置路由, ctx (context) 上下文，包含了 request , response 等信息
 router.get('/', async (ctx)=> {
	 ctx.body = '这是首页';  // 返回数据，相当与 原生例的 res.writeHead()   res.end()
 })
 
 // 匹配到 new 后，路由继续向下匹配
 router.get('/news', async (ctx)=> {
	 console.log('3. 匹配到了news 这个路由');
	 ctx.body = '这是一个新闻'
 })
 
 
 
 // 第六步  启动路由
 app
  .use(router.routes())  // 表示启动路由
  .use(router.allowedMethods());  
  
  /**
   .use(router.allowedMethods()); 可以配置，也可以不配置，建议配置，此行的作用是：
   这是官方文档推荐用法，我们可以看到 router.allowedMethods()用在了路由匹配 router.routes() 之后，
   所以在当所有中间件最后调用，此时根据 ctx.status 设置 response响应头
  **/

// 监听端口
app.listen(9000);
```



### koa 中 post 提交数据  koa-bodyparser 中间件获取表单提交的数据

```js
/*
 Koa 中 koa-bodyparser 中间件获取表单提交的数据

    1.npm install --save koa-bodyparser

    2.引入var bodyParser = require('koa-bodyparser');

    3.app.use(bodyParser());

    4.ctx.request.body;  获取表单提交的数据
* */
var Koa=require('koa'),
    router = require('koa-router')(),
    views = require('koa-views'),
    bodyParser = require('koa-bodyparser');

var app=new Koa();
/*应用ejs模板引擎*/
app.use(views('views',{
    extension:'ejs'
}))

//配置post bodyparser的中间件
app.use(bodyParser());

router.get('/',async (ctx)=>{
    await  ctx.render('index');
})

//接收post提交的数据
router.post('/doAdd',async (ctx)=>{
    console.log(ctx.request.body);
    ctx.body=ctx.request.body;  //获取表单提交的数据
})

app.use(router.routes());   /*启动路由*/
app.use(router.allowedMethods());
app.listen(3000);

```



### koa-static 静态资源中间件

```js

```



### koa art-template 模板引擎

```js

```



### Koa 应用生成器

1. 全局安装 koa 生成器	

```js
npm i -g koa-generator  // 全局安装 koa-generator 生成器
```



2. 创建项目

   ```js
   koa koa_demo  // 创建项目
   ```



3. 安装依赖

```js
cd koa_demo  // 进入该文件夹
npm install  // 安装koa-generator 生成后，指定需要的所有依赖
```

