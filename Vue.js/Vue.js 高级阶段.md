[TOC]



# Axios.js 

- 从浏览器中创建 [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- 从 node.js 创建 [http](http://nodejs.org/api/http.html) 请求
- 支持 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 [XSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)

安装 Axios.js

```js
npm i --save axios
```

### axios向服务器发起请求

以下代码是axios向服务器端发起请求，因为没有配置服务器端，所以没法验证是否正确从服务器端返回数据

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<!-- 留坑，或者叫做留插槽,等待数据的插入 -->
		<div id="app"></div>
		
		<!-- 引入 vue.js 包 -->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 1. 引入 axios 对象 -->
		<script type="text/javascript" src="./node_modules/axios/dist/axios.js"></script>
		
		<script type="text/javascript">
			// 组件内的每一个 this 对象,都是Vue的孩子
			//Vue祖宗的原型数据,就会共享给所有的孩子
			Vue.prototype.$axios = axios;
			
			
			var App = {
				template:`
				  <div> 
					  <button @click='sendAjax'>发请求</button>
				   </div>
				`,
				methods:{
					sendAjax:function(){
						// 让组件具备 axios对象引用
						//axios.get||post||put||delete(url,options)
						axios.get('http://192.168.31.181:3003/vuetest/upload')
						.then(function(res){
							console.log(res)
						})
						.catch(function(err){
							console.log(err);
						});
					}
				}
			}
			
			
			
			new Vue({  
				el:'#app',				
				components:{
					app:App
				},
				template:'<app/>'
			});
			
		</script>		
	</body>
</html> 
```



### axios 合并请求

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<!-- 留坑，或者叫做留插槽,等待数据的插入 -->
		<div id="app"></div>
		
		<!-- 引入 vue.js 包 -->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 1. 引入 axios 对象 -->
		<script type="text/javascript" src="./node_modules/axios/dist/axios.js"></script>
		
		<script type="text/javascript">
			// 组件内的每一个 this 对象,都是Vue的孩子
			//Vue祖宗的原型数据,就会共享给所有的孩子
			Vue.prototype.$axios = axios;			
			
			var App = {
				template:`
				  <div> 
				      响应1:{{ res1 }}
					  响应2:{{ res2 }}
					  <button @click='sendAjax'>合并请求</button>
				   </div>
				`,
				data:function(){
					return { res1:'', res2:''}
				},
				methods:{
					sendAjax:function(){
						//请求1 get /
						//请求2 post /add 
						// this.$axios.get(url,options);
						// this.$sxios.post(url,[data],options);
						
						//配置公共数据
						this.$axios.defaults.baseURL='http://192.168.31.181:3003/vuetest/';
						
						var q1 = this.$axios.get('');
						var q2 = this.$axios.get('http://192.168.31.181:3003/vuetest/', 'a=1');
											
						
						//合并这两个请求，并处理其成功和失败
						this.$axios.all([q1,q2])
						.then(this.$axios.spread((res1,res2)=>{
							//全成功
							this.res1=res1.data;
							this.res2=res2.data;
						}))
						.catch(err=>{
							//其一失败
							console.log(err);
						}); 
					}
				}
			}
			
			
			
			new Vue({  
				el:'#app',				
				components:{
					app:App
				},
				template:'<app/>'
			});			
		</script>		
	</body>
</html> 
```



### transformResponse   transformRequest

以下代码了解就行，没有验证

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<!-- 留坑，或者叫做留插槽,等待数据的插入 -->
		<div id="app"></div>
		
		<!-- 引入 vue.js 包 -->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 1. 引入 axios 对象 -->
		<script type="text/javascript" src="./node_modules/axios/dist/axios.js"></script>
		
		<script type="text/javascript">
			// 组件内的每一个 this 对象,都是Vue的孩子
			//Vue祖宗的原型数据,就会共享给所有的孩子
			Vue.prototype.$axios = axios;
			
			
			var App = {
				template:`
				  <div> 
				      响应1:{{ res1 }}
					  响应2:{{ res2 }}
					  <button @click='sendAjax'>合并请求</button>
				   </div>
				`,
				data:function(){
					return { res1:'', res2:''}
				},
				methods:{
					sendAjax:function(){
						this.$axios.defaults.baseURL='http://192.168.31.181:3003/';
						//所有请求自带的头信息
						//this.$axios.headers={};
						
						//走默认够，但是修改个别
						this.$axios.defaults.headers.accept = 'abc';
						
						//请求1 get请求
						this.$axios.get('/',{
							params:{id:1},
							
							// 改响应数据
							transformResponse:function(res){
								// 就是 res.data
								data='改了数据呀';
								return data;
							}
						})
						.then(res=>{
							console.log(res);
						})
						.catch(err=>{
							console.log(err);
						});
						
						//请求2 post请求
						this.$axios.post('add','name=jack',{
							timeout:1000,
							transformRequest:function(data){
								// 修改请求数据
								return 'name=rose';
							}
						})
						.then(res=>{
							console.log(res);
						})
						.catch(err=>{
							console.log(err);
						});
					}
				}
			}			
			
			
			new Vue({  
				el:'#app',				
				components:{
					app:App
				},
				template:'<app/>'
			});			
		</script>		
	</body>
</html> 
```



### 跨域cookie







# webpack

打包工具，占市场主流

在 node_modules 里的 package.json 文件夹里进行设置，

**webpack  ./main.js(声明入口文件)  ./build.js(声明出口文件)**

![image-20190126161907734](/Users/renpeng/Library/Application Support/typora-user-images/image-20190126161907734.png)

![image-20190126163440259](/Users/renpeng/Library/Application Support/typora-user-images/image-20190126163440259.png)

![image-20190126164525182](/Users/renpeng/Library/Application Support/typora-user-images/image-20190126164525182.png)

![image-20190126170255089](/Users/renpeng/Library/Application Support/typora-user-images/image-20190126170255089.png)



### 安装webpack和webpack-dev-server

