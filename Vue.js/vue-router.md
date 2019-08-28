复制下面代码，在HBuilder中，输入到 html 页面，运行查看

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>		
		
	</head>
	<body>
		<div id="app"> </div>
		
		<!-- 引入 vue 的 js 包-->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 第一步、引入 vue-router （核心插件）对象 -->
		<script type="text/javascript" src="./node_modules/vue-router/dist/vue-router.js"></script>
		<script type="text/javascript">
		var Login = {
			template:`<div>我是登录页面</div>`
		}	
			
			
		// 第二步、安装插件 =>？
		Vue.use(VueRouter)
			
		
		// 第三步、创建一个路由对象, VueRouter 是 vue 内部元素
		console.log(VueRouter) // 先看一下 控制台 本元素
		var router = new VueRouter({
			
			// 第四步、配置路由对象
			routes:[ {path:'/login',component:Login}]
		})
		
		
		
		// 创建根节点
		var App = {
			template:
			`
				<div>
				  <!--第六步、指定路由改变局部的位置-->
					<router-view></router-view>
				</div>
						
			`
		}
		
		
		
		// 第五步、将配置好的路由对象关联到vue实例中
		new Vue({
			el:'#app',
			// 第七步、配置到这里，否则报错
			router:router, // 没有这一步会报错：TypeError: Cannot read property 'matched' of undefined
			components:{
				app:App
			},
			template:`<app/>`
		})
		
		
		
		
		
		</script>
		
	</body>
</html>
```



复制到 HBuilder 里的 HTML 页面，运行查看

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>		
		
	</head>
	<body>
		<div id="app"> </div>
		
		<!-- 引入 vue 的 js 包-->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 第一步、引入 vue-router （核心插件）对象 -->
		<script type="text/javascript" src="./node_modules/vue-router/dist/vue-router.js"></script>
		<script type="text/javascript">
		var Login = {
			template:`<div>我是登录页面</div>`
		}	
		
		var Register = {
			template:`<div>我是注册页面 </div>`
		}
			
			
		// 第二步、安装插件 =>？
		Vue.use(VueRouter)
			
		
		// 第三步、创建一个路由对象, VueRouter 是 vue 内部元素
		console.log(VueRouter) // 先看一下 控制台 本元素
		var router = new VueRouter({
			
			// 第四步、配置路由对象
			routes:[ 
				// 路由对象有了名称，就等于有了变量名， router-link 只需要说明这个变量名就可以了
				{name:"login", path:'/login',component:Login},
			    {name:"register", path:'/register',component:Register}
			]
		})
		
		
		
		// 创建根节点
		var App = {
			template:
			`
				<div>
					<!-- vue-router内置组件
					 <router-link to="/login">登录去</router-link>
					 <router-link to="/register">注册去</router-link>
					 
					 -->
					<router-link :to="{name:'login'}">登录去</router-link>
					<router-link :to="{name:'register'}">注册去</router-link>
					
				  <!--第六步、指定路由改变局部的位置-->
					<router-view></router-view>					
				</div>						
			`
		}		
		
		
		// 第五步、将配置好的路由对象关联到vue实例中
		new Vue({
			el:'#app',
			// 第七步、配置到这里，否则报错
			router:router, // 没有这一步会报错：TypeError: Cannot read property 'matched' of undefined
			components:{
				app:App
			},
			template:`<app/>`
		})
			
		</script>
		
	</body>
</html>
```

