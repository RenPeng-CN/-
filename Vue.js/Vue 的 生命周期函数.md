Vue 的 生命周期函数 (钩子函数)

复制到 Hbuilder 里 的 一个 HTML 文件里，执行查看

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>Vue Test</title>
		<style>
			li {
				list-style: none;
				float: left;
				width: 33%;
				border: 1px solid black;
				height: 150px;
				background-color: aquamarine;
				
			}
		</style>
		
	</head>
	<body>
		<div id="app"> </div>
		
		<!-- 引入 vue 的 js 包-->
		<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
		<script type="text/javascript">
		
		var Test = {
			template: `<div> 我是 Test 组件 {{ text }} ,
			
			 <button @click='text=text+1'>测试</button>
			
			</div>`,
			data:function(){
				return {
					text:'hello world'
				}
			},
			beforeCreate:function(){
				// 组件创建之前
				console.log('beforeCreate--->:'+this.text)
			},
			created:function(){
				// 组件创建之后
				console.log('created--->:'+this.text)
			},
			beforeMount:function(){
				// vue 起了作用，装载数据到 DOM 之前
				console.log('beforeMount，--->>>:'+this.text)
				console.log(document.body.innerHTML)
			},
			mounted:function(){
				// vue 起了作用，装载到 DOM 之后
				console.log('mounted--->>> mounted 只执行一次:'+this.text)
				console.log(document.body.innerHTML)
			},
			// update 的两个函数，是基于页面被改变时才会调用的函数, 
			// beforeUpdate 可以获取原 DOM, 
			// updated 可以获取新 DOM
			beforeUpdate:function(){  // 改变前
				console.log('beforeUpdated---->>:'+this.text) 
				console.log(document.body.innerHTML)
			},
			updated:function(){   // 改变后
				console.log('updated---->>:'+this.text) 
				console.log(document.body.innerHTML)
			},
			// 激活组件, 使用 keep-alive 可以在激活组件和停止组件之间循环，这样效率很高
			// 避免了重新创建组件和销毁组件，这样效率很低
			activated:function(){
				console.log('组件被激活了')
			},
			// 停止组件
			deactivated:function(){
				console.log('组件被停用了')
			},
			// 销毁，最终都是做一些其它功能的释放
			// 对应父组件 v-if false 就销毁当前组件
			// 销毁之前
			beforeDestroy:function() {  
				console.log('beforeDestroy--->>')
				
			},
			// 销毁之后
			destroyed:function(){
				console.log('destroyed--->>')
				
			}
		}
		
		var App = {
			data:function(){
				return {
					isExist:true
				}
			},
			components:{
				test: Test
			},
			template:
			`
			  <div>
				  <!-- 使用 keep-alive 必须与 v-if 一起使用，包含了 activated 和 deactivated , -->
				  <!-- 对整体性能会提高很多 -->
				  <!-- keep-alive 是针对子组件，可以有多个子组件，多个keep-alive,每个子组件只有一对 activated -->
				  <keep-alive>
					 <test v-if="isExist"></test>
				  </keep-alive>
					<button @click="isExist=!isExist">创建/销毁 事关子组件生死</button>
			  </div>
			
			
			`
		}
			
			
		new Vue({
			el:'#app',
			components:{
				app: App
			},
			template:`<app/>`
		})	
			
		</script>
		
	</body>
</html>
```



### Vue $refs  

###  $attrs

### $children

### $el:

### $options

### $parent

### $root

### $slots

 赋值下面代码，到 Hbuilder 的一个 HTML 页面，运行查看 vue 的 $ 取值

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
		
		var TempComponent = {
			template:`<div>我是子组件</div>`
		}
		
		
		Vue.component('temp',TempComponent)
		
		var App = {
			template:`<div>
			 <temp ref="tmp"/>
			  <input type="text" v-if="isShow" ref="input" />
			  <button ref="btn">按钮</button>
			  
			</div>`,
			data:function(){
			  return {
				  isShow : false
			  }	
			},
			mounted:function(){
				// 装载数据之后
				// 显示元素，并获取焦点
				this.isShow = true
				console.log(this.$refs.tmp)
				this.$nextTick(function(){
					this.$refs.input.focus()
					console.log(this.$refs.input)
					
				})
				
			}
		}
		
		 
		
		var vm = new Vue({
			el:'#app',
			components:{
				app:App
			},
			template:`<app/>`
		})
			
		</script>
		
	</body>
</html>
```

