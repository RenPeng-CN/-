[TOC]

# 总结

全局：组件/过滤器 让大家直接用 全局不带s

过滤器：function（原数据,参数1,参数2..）{ return 结果}

​         调用{{ '数据' | 过滤器名(参数1, 参数2) }}

watch 是单个监视

computed 是群体监视





# Vue.js 全局组件

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
		
		<script type="text/javascript">	
			
			//注册公共的全局按钮组件，省去很多的 components:{xxx:xxx}
			Vue.component('my-btn',{template:`<button style="background-color:red; ">漂亮的按钮</button>`})			
			
			var MyHeader = {
				template:`<div> 我是header组件<my-btn/> </div>`
			}
			var MyFooter = {
				template:`<div> 我是footer组件<my-btn/> </div>`
			}		
			
			
			//入口组件,也就是父组件
			var App = {
				components:{
					'my-header': MyHeader,
					'my-footer': MyFooter,
				},
				template:`
				  <div>
						<my-header/>
						<hr/>
						<my-footer/>
						<hr/>
						App入口组件使用全局组件
						<my-btn/>						
				  </div>
				`
			}		
			
			
			new Vue({
				el:'#app',
				components:{
				   app:App
				},
				template:'<app/>'
			})			
		</script>
	</body>
</html>
```



# 附件功能：过滤器&监视改动

###过滤器 filter || filters 

全局过滤器 (给数据添油加醋显示)  `Vue.filter('过滤器名',过滤方式fn);`

组件内的过滤器 `filters{'过滤器名','过滤方式fn'}`

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
		
		<script type="text/javascript">	
			
			//1: 过滤器可以给数据显示进行添油加醋
			// 需求:原本显示的数据 abc... 添油加醋后反转成 cba 
			//需求实现: 1:为了互动性更好,用Input + v-model来获取数据到vue中..
			         //2: 输出: {{内容 | 使用过滤器输出}}
			//全局过滤器 {{'xxx' | myreverse('arg1) }}
            Vue.filter('myreverse',function(data,arg1){
                return 'xxxx';
            })
            
            
			var App={
				template:`
					<div>
						<input type="text" v-model="myText" />
						{{ myText | reverse('英文版','$') }}
					</div>
				`,
				data:function(){
					return {myText:''}
				},
				//组件内的过滤器
				filters:{
					reverse:function(dataStr,lang,arg1){ //参数1就是传递的源数据
					                   //变数组      反转    变字符串    
						return lang+':'+arg1+dataStr.split('').reverse().join(''); //return 就是显示的内容
					}
				}
			}
			
			
			new Vue({
				el:'#app',
				components:{
					app:App
				},
				template:'<app/>'
			})
			
			
		</script>
	</body>
</html>
```

![image-20190124153518253](/Users/renpeng/Library/Application Support/typora-user-images/image-20190124153518253.png)



### 监视  watch监视单个   computed监视多个

V-model 只能监视数据变化    watch可以有更多的行为

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
		
		<script type="text/javascript">	
		
			
			var App={
				template:`
					<div>
						<input type='text' v-model='myText'  />
						{{myText}}
					</div>
				`,
				data:function(){
					return {myText:'111'}
				},
				watch:{
					//key是属性data属性的属性名,value是监视后的行为
					myText:function(newV,oldV){
						console.log(newV,oldV);
						if(newV == 'I love you'){
							alert('我也爱你😙！')
						}
					}
				}				
			}			
			
			new Vue({
				el:'#app',
				components:{
					app:App
				},
				template:'<app/>'
			})			
		</script>
	</body>
</html>
```

### 深度监视

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
		
		<script type="text/javascript">	
		
			
			var App={
				template:`
					<div>
						<input type='text' v-model='myText'  />
						{{myText}}
						<br/>
						<button @click="stus[0].name='rose'"> 改变stus[0].name属性 </button>
					</div>
				`,
				data:function(){
					return {myText:'111', stus:[{name:'jack'}]}
				},
				watch:{
					//监视复杂类型，不能监视对象的地址，而要监视同地址属性的值
					//深度监视： object || array 
					stus:{
					   deep:true, //深度监视
					   handler:function(newV, oldV){
						   console.log('监视复杂数据类型成功！')
					   }
						   
					},
					
					//key是属性data属性的属性名,value是监视后的行为
					myText:function(newV,oldV){
						console.log(newV,oldV);
						if(newV == 'I love you'){
							alert('我也爱你😙！')
						}
					}
				}				
			}
			
			
			new Vue({
				el:'#app',
				components:{
					app:App
				},
				template:'<app/>'
			})			
		</script>
	</body>
</html>
```



### computed监视多个

computed:{监视的业务名：function(){  return 显示一些内容 }}

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
		
		<script type="text/javascript">			
			
			var App={
				template:`
					<div>
						<input type='text' v-model='n1' />
						+
						<input type='text' v-model='n2' />
						*
						<input type='text' v-model='rate' />
						= {{ result }}
					</div>
				`,
				data:function(){
					return { n1:0, n2:0, rate:0 }
				},
				computed:{
					result:function(){
				//监视对象，卸载了函数内部， 凡是函数内部有 this. 相关的属性，改变都会触发当前的函数
						return ((this.n1-0) + (this.n2-0))*this.rate;
					}
				}			
			}
			
			
			new Vue({
				el:'#app',
				components:{
					app:App
				},
				template:'<app/>'
			})
			
			
		</script>
	</body>
</html>
```



### slot（插槽,留坑） 是 Vue提供的内置的组件

slot 是 vue 提供的内置组件 <slot></slot>

总结：slot其实就是父组件传递的DOM结构

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		<style >
			li{ 
				list-style: none;
				float:left;
				width:150px;
				height:150px;
				background-color:yellowgreen;
				border:1px solid black;				
				}
		</style>
	</head>
	<body>
		<div id="app"> </div>
		
		<!-- 引入 vue 的 js 包-->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<script type="text/javascript">	
			
			//slot是留坑,外部填入html内容
			var MyLi={
				template:`<li>,
					<slot></slot>
				</li>`
			}
			Vue.component('my-li',MyLi);
		
			//总结：slot其实就是父组件传递的DOM结构
			//slot 是 vue 提供的内置组件 <slot></slot>
			
			//入口父组件:包含 头 中 底
			var App={
				template:`
					<div>
						<ul>
							<my-li><button> 111 </button></my-li>
							<my-li><h1>222</h1></my-li>
							<my-li>3</my-li>
							<my-li>4</my-li>
							<my-li>5</my-li>
							<my-li>6</my-li>
							<my-li>7</my-li>
							<my-li><h1>888</h1></my-li>
							<my-li><button> 999 </button></my-li>
						</ul>
					</div>
				`				
			}
			
			
			new Vue({
				el:'#app',
				components:{
					app:App
				},
				template:'<app/>'
			})
			
			
		</script>
	</body>
</html>
```

 

### 具名slot

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		<style >
			li{ 
				list-style: none;
				float:left;
				width:50%;
				
				background-color:yellowgreen;
				border:1px solid black;
				
				}
		</style>
	</head>
	<body>
		<div id="app"> </div>
		
		<!-- 引入 vue 的 js 包-->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<script type="text/javascript">	
			
			//slot是留坑,外部填入html内容
			var MySlot={
				template:`<li>
				    以下预留第一个坑
					<slot name="one"></slot>
					<hr/>
					以下预留第二个坑
					<slot name="two"></slot>
					
				</li>`
			}
			Vue.component('my-slot',MySlot);
		
			//总结：slot其实就是父组件传递的DOM结构
			//slot 是 vue 提供的内置组件 <slot></slot>
			
			//入口父组件:包含 头 中 底
			var App={
				template:`
					<div>
						<my-slot>										
						   <h1 slot="one">我是一，对应第一个坑</h1>
						   <h1 slot="two">我是二，对应第二个坑</h1>								
						</my-slot>
					</div>
				`				
			}
			
			
			new Vue({
				el:'#app',
				components:{
					app:App
				},
				template:'<app/>'
			})			
		</script>
	</body>
</html>
```



### 组件生命周期  beforeCreate    created

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
		
		<script type="text/javascript">	
			
			// beforeCreate
			// created
			// beforeMount
			// mounted
			// beforeUpdate
			// updated
			// activated
			// deactivated
			// beforeDestroy
			// destroyed
			var Test = {
				template:`
					<div>
						我是test组件
					</div>
				`,
				data:function(){
					return {
						text:'hello world'
					}
				},
				beforeCreate:function(){ //组件创建之前					
					console.log(this.text);
				},
				created:function(){ //组件创建之后					
					 console.log(this.text);
				},
				//使用该组件，就会触发以上的事件函数（钩子函数）
				// created 中 可以操作数据.. 并且可以实现 vue->页面 的影响，
				// 可以发起 ajax 请求
			}
			
			
			var App={
				components:{
					test:Test
				},
				template:`
				  <div>
					 <test></test>
				  </div>`			
            }
			
			
			new Vue({
				 el:'#app',
				 components:{
					 app:App
				 },
				 template:'<app/>'
			})			
		</script>
	</body>
</html>
```





### beforeMount   mounted 区别

// beforeMount 是执行vue启动之前的 DOM

// mounted 是执行 vue 启动之后的 DOM，只执行一次

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
		
		<script type="text/javascript">	
			
			// beforeCreate
			// created
			// beforeMount
			// mounted
			// beforeUpdate
			// updated
			// activated
			// deactivated
			// beforeDestroy
			// destroyed
			var Test = {
				template:`
					<div>
						我是test组件
					</div>
				`,
				data:function(){
					return {
						text:'hello world'
					}
				},
				beforeMount:function(){
					// vue 起作用,装载数据到 DOM 之前
					console.log(document.body.innerHTML);
				},
				mounted:function(){
					// vue 起作用,装载数据到 DOM 之后
					console.log(document.body.innerHTML);
				}
			}
					
			
			
			var App={
				components:{
					test:Test
				},
				template:`
				  <div>
					 <test></test>
				  </div>`
			}			
			
			new Vue({
				 el:'#app',
				 components:{
					 app:App
				 },
				 template:'<app/>'
			})
			
			
		</script>
	</body>
</html>
```



![image-20190124215327113](/Users/renpeng/Library/Application Support/typora-user-images/image-20190124215327113.png)





### beforeUpdate（改变前）   updated（改变后）

这两个是当有更改数据才会触发改变，

beforeUpdate (获取改变之前的 DOM)

update（获取改变之后的DOM）





### beforeDestroy  destroyed

//不管销毁前还是销毁后，最终都是做一些其他功能的释放

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
		
		<script type="text/javascript">	
			
			// beforeCreate
			// created
			// beforeMount
			// mounted
			// beforeUpdate
			// updated
			// activated
			// deactivated
			// beforeDestroy
			// destroyed
			var Test = {
				template:`
					<div>
						我是test组件
					</div>
				`,
				data:function(){
					return {
						text:'hello world'
					}
				},
				beforeDestroy:function(){ //销毁前
					console.log('beforeDestroy')
				},
				destroyed:function(){  //销毁后
					console.log('destroyed')
				},
				beforeCreate:function(){ //组件创建前
					console.log('beforeCreate')
				},
				created:function(){  //组件创建之后
					console.log('created')
				}
				//不管销毁前还是销毁后，最终都是做一些其他功能的释放
			}			
			
			
			var App={
				components:{
					test:Test
				},
				data:function(){
					return{
						isExist:true
					}
				},
				template:`
				  <div>
					 <test v-if="isExist"></test>
					 <br/>
					 <button @click="isExist = !isExist">事关子组件生死</button>
				  </div>`
			}
			
			
			new Vue({
				 el:'#app',
				 components:{
					 app:App
				 },
				 template:'<app/>'
			})			
		</script>
	</body>
</html>
```



### Keep-alive

keep-alive 只能和  v-if 配套使用

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
		
		<script type="text/javascript">	
			
			// beforeCreate
			// created
			// beforeMount
			// mounted
			// beforeUpdate
			// updated
			// activated
			// deactivated
			// beforeDestroy
			// destroyed
			var Test = {
				template:`
					<div>
						我是test组件
					</div>
				`,
				data:function(){
					return {
						text:'hello world'
					}
				},
				beforeDestroy:function(){ //销毁前
					console.log('beforeDestroy')
				},
				destroyed:function(){  //销毁后
					console.log('destroyed')
				},
				beforeCreate:function(){ //组件创建前
					console.log('beforeCreate')
				},
				created:function(){  //组件创建之后
					console.log('created')
				}
				//不管销毁前还是销毁后，最终都是做一些其他功能的释放
			}
			
			
			
			
			var App={
				components:{
					test:Test
				},
				data:function(){
					return{
						isExist:true
					}
				},
				template:`
				  <div>
					 <keep-alive> 
						 <test v-if="isExist"></test>
					 </keep-alive>
					 <br/>
					 <button @click="isExist = !isExist">事关子组件生死</button>
				  </div>
				`
			}
			
			
			new Vue({
				 el:'#app',
				 components:{
					 app:App
				 },
				 template:'<app/>'
			})
			
			
		</script>
	</body>
</html>
```



### actived   deactived

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
		
		<script type="text/javascript">	
			
			// beforeCreate
			// created
			// beforeMount
			// mounted
			// beforeUpdate
			// updated
			// activated
			// deactivated
			// beforeDestroy
			// destroyed
			var Test = {
				template:`
					<div>
						我是test组件
					</div>
				`,
				data:function(){
					return {
						text:'hello world'
					}
				},
				beforeDestroy:function(){ //销毁前
					console.log('beforeDestroy')
				},
				destroyed:function(){  //销毁后
					console.log('destroyed')
				},
				activated:function(){
					console.log('组件被激活了')
				},
				deactivated:function(){
					console.log('组件被停用了')
				},
				beforeCreate:function(){ //组件创建前
					console.log('beforeCreate')
				},
				created:function(){  //组件创建之后
					console.log('created')
				}
				//不管销毁前还是销毁后，最终都是做一些其他功能的释放
			}	
			
			
			
			var App={
				components:{
					test:Test
				},
				data:function(){
					return{
						isExist:true
					}
				},
				template:`
				  <div>
					 <keep-alive> 
						 <test v-if="isExist"></test>
					 </keep-alive>
					 <br/>
					 <button @click="isExist = !isExist">事关子组件生死</button>
				  </div>
				`
			}
			
			
			new Vue({
				 el:'#app',
				 components:{
					 app:App
				 },
				 template:'<app/>'
			})
			
			
		</script>
	</body>
</html>
```

created 和 actived  都是 v-if='true' 子组件的状态

created 没有被 keep-alive 内置组件包裹，actived被包裹了

销毁和停用同上



###vue补充 获取DOM元素

mounted:function(){ }//此处才能获取 this.$refs.btn

```html
关于 $ 属性
// $ 属性：$refs 获取组件内的元素，
// $parent : 获取当前组件对象的父组件
// $children : 获取子组件
// $root : 获取 new Vue 的实例 vm
// $el : 获取组件对象的 DOM 元素

var vm = new Vue({

})
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<div id="app"> </div> <!-- 留坑，或叫做留下插槽，等待后台插入DOM-->
		
		<!-- 引入 vue 的 js 包-->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<script type="text/javascript">	
			// 1. 在组件的DOM部分,任意的标签中,写上 ref="xxx"
			// 2. 通过组件对象 this.$refs.xxx 获取到元素			
			
			// 子组件, $refs 获取
			var TempComponent = {
				template:`
					<div>
						我是子组件
					</div>`
			}
			// 把 TempComponent 变成全局组件
			Vue.component('temp', TempComponent);
				
			var App={
				template:`
				  <div>
					  <temp ref='tmp'/>
					 <button ref='btn'>我是按钮</button>
				  </div>`,
			
				mounted:function(){ //此处才能获取 this.$refs.btn
					 //装载数据之后
					 console.log(this.$refs.tmp);
				}
			}
			
			
			new Vue({
				 el:'#app',
				 components:{
					 app:App
				 },
				 template:'<app/>'
			})			
		</script>
	</body>
</html>
```

 

### 给DOM元素添加事件的特殊情况

获取输入焦点

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<div id="app"> </div> <!-- 留坑，或叫做留下插槽，等待后台插入DOM-->
		
		<!-- 引入 vue 的 js 包-->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<script type="text/javascript">	
			
			var App={
				template:`
				  <div>
					  <input type="text" v-if="isShow" ref="input" />		 
				  </div>`,
				data:function(){
					return { isShow: false }
				},
			
				mounted:function(){ //此处才能获取 this.$refs.btn
					 //装载数据之后
					 // 显示元素，并给与获取焦点
					 this.isShow = true; // 这句话会触发input元素的插入
					 // 给input元素获取焦点
					 //我们希望在 vue 真正的渲染DOM到页面以后，才做的事情
					 this.$nextTick(function(){
						  this.$refs.input.focus();
					 });					
				}
			}			
			
			new Vue({
				 el:'#app',
				 components:{
					 app:App
				 },
				 template:'<app/>'
			})			
		</script>
	</body>
</html>
```







# Vue.js 前端路由

### 前端路由 spa 的原理

hashchange 能够在页面地址改变的时候,页面不刷新

前端路由，就是给 url地址 加上锚点值时，页面不跳转

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<div id="app"> </div> <!-- 留坑，或叫做留下插槽，等待后台插入DOM-->
		
		<!-- 引入 vue 的 js 包-->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<script type="text/javascript">	
		// onhashchange事件.  当url上的部分锚点数据(#xxx)改变时,可以获取到这个事件
		// hashchange 能够在页面地址改变的时候,页面不刷新
			window.addEventListener('hashchange', function(){				
				console.log(location.hash);
			});					
		</script>
	</body>
</html>
```



给 url 加上锚点值，页面不跳转

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<a href="#/login">点我登录</a>
		<a href="#/register">点我注册</a>
		<div id="content"> </div> <!-- 留坑，或叫做留下插槽，等待后台插入DOM-->
		
		<!-- 引入 vue 的 js 包-->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<script type="text/javascript">	
		// hashchange事件.  当url上的部分锚点数据(#xxx)改变时,可以获取到这个事件
		// hashchange 能够在页面地址改变的时候,页面不刷新
		
		var div = document.getElementById('content');
		
			window.addEventListener('hashchange', function(){
				// 根据不同的锚点值做出不同的显示 
				switch(location.hash){
					case '#/login':
						div.innerHTML='<h1>登录页面</h1>';
						break;
					case '#/register':
					    div.innerHTML='<h1>注册页面</h1>';
						break;
				}
			});
					
		</script>
	</body>
</html>
```



### Vue.js 的路由

Vue-router 在 vue当中是主流的开发路由的方式

* **vue-router使用步骤**    1: 引入   2：安装插件   3:创建路由实例  4:配置路由规则  5:将路由对象关联vue  6: 留坑

* router-link to="/xxx" 命名路由。 

  1.  在路由规则对象中 加入 name 属性  
  2.  在 router-link 中 :to="{ name:'xxx'}"

* 生僻 API 梳理：

  - 1. Vue.use(插件对象); //过程中会注册一些全局组件，及给vm或者组件对象挂载属性

    2. 给 vm 及组件对象挂载的方式：Object.defineProperty(Vue.prototype, '$router',{

       ​	get:function(){

       ​	return 自己的 router 对象

       }

       })

vue-router 是 Vue 组织出的一个核心插件，在官网上可以看到。

```js
//通过 npm 下载 vue-router 包
npm i --save vue-router
```

VueRouter 根据不跳转路由，改变页面局部

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<!-- 留坑，或者叫做留插槽 -->
		<div id="app"></div>
		
		<!-- 引入 vue.js 包 -->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 1. 引入 vue-router 对象 -->
		<script type="text/javascript" src="./node_modules/vue-router/dist/vue-router.js"></script>
		
		<script type="text/javascript">
			// 2. 创建一个路由对象
			// 3. 配置这个路由对象
			// 4. 安装插件
			// 5. 将配置好的路由对象关联到 vue 实例中
			// 6. 指定路由改变局部的位置
			
			var Login = {
				template:`
					<div>
						我是登录页面
					</div>
				`
			}
			
			// 4.安装插件
			Vue.use(VueRouter);
			
			// 1. 创建一个路由对象
			var router = new VueRouter({
				
				//2.配置路由对象, //注意别写错 routes:[{}]
				routes:[{ path:'/login', component: Login }]			
			});
			
			
			var App = {
				template:`
				<div>
              <!--以下<router-view>标签实在 Vue.use(VueRouter)的时候产生的-->
					<router-view></router-view>
				</div>`,
			}
			
			
			
			new Vue({  
				el:'#app',
				router:router, //如果不加这一步，会导致运行时报错：undefined,对象中取不到 matched
				components:{
					app:App
				},
				template:'<app/>'
			});
			
		</script>		
	</body>
</html> 
```



### router-link

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<!-- 留坑，或者叫做留插槽 -->
		<div id="app"></div>
		
		<!-- 引入 vue.js 包 -->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 1. 引入 vue-router 对象 -->
		<script type="text/javascript" src="./node_modules/vue-router/dist/vue-router.js"></script>
		
		<script type="text/javascript">
			// 2. 创建一个路由对象
			// 3. 配置这个路由对象
			// 4. 安装插件
			// 5. 将配置好的路由对象关联到 vue 实例中
			// 6. 指定路由改变局部的位置
			
			var Login = {
				template:`
					<div>
						我是登录页面
					</div>`
			};
			
			var Register = {
				template:`
					<div>我是注册页面</div>`
			};
			
			// 4.安装插件
			Vue.use(VueRouter);
			
			// 1. 创建一个路由对象
			var router = new VueRouter({
				
				//2.配置路由对象, //注意别写错 routes:[{}]
				routes:[
					{ path:'/login', component: Login },
					{ path:'/register', component: Register }				
					]			
			});
			
			
			var App = {
				template:`
				<div>
					 <!-- vue-router 内置组件 -->
                     <!--以下<router-view>和<router-link>标签是在 Vue.use(VueRouter) 的时候产生的-->
					 <router-link to='/login'>登录去</router-link>
					 <router-link to='/register'>注册去</router-link>		 
					<router-view></router-view>
					
				</div>`,
			}			
			
			
			new Vue({  
				el:'#app',
				router:router, //如果不加这一步，会导致运行时报错：undefined,对象中取不到 matched
				components:{
					app:App
				},
				template:'<app/>'
			});
			
		</script>		
	</body>
</html> 
```





### 命名路由

1. 给路有对象一个名称

   ```html
   { name:'home', path:'/home', component: Home}
   ```

   

2. 在router-link 的 to 属性中描述这个规则 

   ```html
     <router-link :to="{name:'home'}"></router-link>
   ```

   通过名称找路由对象，获取其 path, 生成自己的 href

   大大降低了维护成本，锚点值改变值用在 main.js 中改变 path属性即可

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<!-- 留坑，或者叫做留插槽 -->
		<div id="app"></div>
		
		<!-- 引入 vue.js 包 -->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 1. 引入 vue-router 对象 -->
		<script type="text/javascript" src="./node_modules/vue-router/dist/vue-router.js"></script>
		
		<script type="text/javascript">
			// 2. 创建一个路由对象
			// 3. 配置这个路由对象
			// 4. 安装插件
			// 5. 将配置好的路由对象关联到 vue 实例中
			// 6. 指定路由改变局部的位置
			
			var Login = {
				template:`
					<div>
						我是登录页面
					</div>
				`
			};
			
			var Register = {
				template:`
					<div>我是注册页面</div>
				`
			};
			
			// 4.安装插件
			Vue.use(VueRouter);
			
			// 1. 创建一个路由对象
			var router = new VueRouter({
				
				//2.配置路由对象, //注意别写错 routes:[{}]
				
				routes:[
					// 路由对象有了名称，就等于有了变量名，router-link 只需说明这个变量名就可以了
					{ name:'login', path:'/login', component: Login },
					{ name:'register', path:'/register', component: Register }					
					]			
			});
			
			
			var App = {
				template:`
				<div>
					 <!-- vue-router 内置组件 -->
                  <!--以下<router-view>和<router-link>标签是在 Vue.use(VueRouter) 的时候产生的-->
					 <router-link :to="{ name:'login'}">登录去</router-link>
					 <router-link :to="{ name:'login'}">登录去</router-link>
					 <router-link :to="{ name:'register'}">注册去</router-link>			 
					 
					 <router-view></router-view>
					
				</div>`,
			}
			
			
			
			new Vue({  
				el:'#app',
				router:router, //如果不加这一步，会导致运行时报错：undefined,对象中取不到 matched
				components:{
					app:App
				},
				template:'<app/>'
			});
			
		</script>		
	</body>
</html> 
```



### Router-link 传递参数

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<!-- 留坑，或者叫做留插槽 -->
		<div id="app"></div>
		
		<!-- 引入 vue.js 包 -->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 1. 引入 vue-router 对象 -->
		<script type="text/javascript" src="./node_modules/vue-router/dist/vue-router.js"></script>
		
		<script type="text/javascript">
				

			var Login = {
				template:`
					<div>
						我是登录页面
					</div>
				`,
				created:function(){
					console.log(this.$route.query);
				}
			};
			
			var Register = {
				template:`
					<div>我是注册页面</div>
				`,
				created:function(){
					console.log(this.$route.params);
				}
			};
			
			// 4.安装插件
			Vue.use(VueRouter);
			
			// 1. 创建一个路由对象
			var router = new VueRouter({
				
				//2.配置路由对象, //注意别写错 routes:[{}]
				
				routes:[
					// 路由对象有了名称，就等于有了变量名，router-link 只需说明这个变量名就可以了
					{ name:'login', path:'/login', component: Login },
					{ name:'register', path:'/register/:name', component: Register }					
					]			
			});
			
			
			var App = {
				template:`
				<div>
					 <!-- vue-router 内置组件 -->
					 <router-link :to="{ name:'login', query:{id:1}}">登录去</router-link>
					 
					 <router-link :to="{ name:'register', params:{ name:'abc' }}">注册去</router-link>			 
					 
					 <router-view></router-view>
					
				</div>`,
			}
			
			
			
			new Vue({  
				el:'#app',
				router:router, //如果不加这一步，会导致运行时报错：undefined,对象中取不到 matched
				components:{
					app:App
				},
				template:'<app/>'
			});
			
		</script>		
	</body>
</html> 
```



### 嵌套路由

// 1: router-view 中包含 router-view		
// 2: 路由规则中存在子路由

![image-20190125202033555](/Users/renpeng/Library/Application Support/typora-user-images/image-20190125202033555.png)



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<!-- 留坑，或者叫做留插槽 -->
		<div id="app"></div>
		
		<!-- 引入 vue.js 包 -->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 1. 引入 vue-router 对象 -->
		<script type="text/javascript" src="./node_modules/vue-router/dist/vue-router.js"></script>
		
		<script type="text/javascript">
			// 1: router-view 中包含 router-view		
			// 2: 路由规则中存在子路由
			
			var Women = {
				template:`
					<div>
						欢迎女士登录......
					</div>
				`
			}
			var Man = {
				template:`
					<div>
						欢迎男士登录......
					</div>
				`
			}

			var Login = {
				template:`
					<div>
					我是login显示的路由,下面是子路由显示的路由
						<router-view></router-view>
					</div>
				`,
				created:function(){
					console.log(this.$route.query);
				}
			};
			
			var Register = {
				template:`
					<div>
					我是register显示的路由,下面是子路由显示的路由
						<router-view></router-view>
					</div>
				`,
				created:function(){
					console.log(this.$route.params);
				}
			};
			
			// 4.安装插件
			Vue.use(VueRouter);
			
			// 1. 创建一个路由对象
			var router = new VueRouter({
				
				//2.配置路由对象, //注意别写错 routes:[{}]
				
				routes:[
					// 路由对象有了名称，就等于有了变量名，router-link 只需说明这个变量名就可以了
					// 保证 /login 显示 login组件
					// 保证 /login/abc 显示abc
					{ name:'login', path:'/login', component: Login ,
					  children :[{name:'login.women', path:'women', component: Women},
                                 {name:'login.man', path:'man', component: Man}],
					 
					  
					},
					 
					{ name:'register', path:'/register/:name', component: Register }					
					]			
			});
			
			
			var App = {
				template:`
				<div>
					<router-link :to="{name:'login.women'}">去女性登录点</router-link>
					<router-link :to="{name:'login.man'}">去男士登录点</router-link>
					 <router-view></router-view>
					
				</div>`,
			}
			
			
			
			new Vue({  
				el:'#app',
				router:router, //如果不加这一步，会导致运行时报错：undefined,对象中取不到 matched
				components:{
					app:App
				},
				template:'<app/>'
			});
			
		</script>		
	</body>
</html> 
```



### meta 和全局路由的渲染前事件

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		
	</head>
	<body>
		<!-- 留坑，或者叫做留插槽 -->
		<div id="app"></div>
		
		<!-- 引入 vue.js 包 -->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		
		<!-- 1. 引入 vue-router 对象 -->
		<script type="text/javascript" src="./node_modules/vue-router/dist/vue-router.js"></script>
		
		<script type="text/javascript">
			
			var isLogin = false; // 全局登录变量,默认false

			var Login = {
				template:`
					<div>
					我是login登录页面
						<router-view></router-view>
					</div>
				`,
				created:function(){
					isLogin=true;
				}
				
			};
			
			var Music = {
				template:`
					<div>
					我是音乐页面
						<router-view></router-view>
					</div>
				`,
				
			};
			
			// 4.安装插件
			Vue.use(VueRouter);
			
			// 1. 创建一个路由对象
			var router = new VueRouter();
			
			// router.addRoutes(); 可以多次的追加路由规则，动态的获取路由规则
			router.addRoutes([
					// 默认首页路由
					{ path:'/', redirect:{name:'login'} },
				
				
					{ name:'login', path:'/login', component: Login },	
					// meta:{isChecked:true} 给未来路由的权限控制
					// 全局路由守卫来做参照条件
					{ name:'music', path:'/music', component: Music ,meta:{isChecked:true}}									
			]); //更为灵活，可以方便地调用追加路由规则
			
			router.beforeEach(function( to, from, next ){
				//login等..放行
				if(!to.meta.isChecked){
					next(); // 不调用next 就回被卡住
				}else{
					//音乐访问，需要判断访问
					if(isLogin){
						next();
					}else{
						alert('请先登录'); 
						//重定向到 /login
						next({name:'login'});
						//next(); //是放行
						//next(false); //是取消用户导航行为
					}
				}
				
			});
			
			var App = {
				template:`
				<div>
					<router-link :to="{name:'login'}">登录</router-link>
					<router-link :to="{name:'music'}">去听歌</router-link>
					 <router-view></router-view>
					
				</div>`,
			}
			
			
			
			new Vue({  
				el:'#app',
				router:router, //如果不加这一步，会导致运行时报错：undefined,对象中取不到 matched
				components:{
					app:App
				},
				template:'<app/>'
			});
			
		</script>		
	</body>
</html> 
```



### 编程导航

* 1：跳到指定的锚点，并显示页面 this.$router.push({ name:'xxx',query:{id:1},params:{name:'abc'} });

  2: 配置规则 {name:'xxx', path:'/xxx/:name'}

  3: 根据历史记录，前进或后退

  - this.$router.go(-1|1);
  - 1 代表进一步，-1是退一步





