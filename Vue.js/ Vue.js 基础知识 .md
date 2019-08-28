[TOC]

一句话解释vue，就是  Object.defineProperty 双向绑定

# vue.js 安装

### npm 安装 vue.js

```js
npm i --save vue
```



### HTML页面引入 vue.js 文件

```html
<script type="text/javascript" src="./node_modules/vue/dist/vue.js"> </script>
```







***

# Vue.js 起步基础

### Vue.js 标签说明

+ options的根属性

​              <font color="red"> el：</font>目的地（string || DOM元素）

​                 <font color="red"> template</font> 模板

​               <font color="red">   data</font>是一个函数，return一个对象，对象中的key，可以直接在页面中使用,  

​                                          data中的属性，在DOM中直接使用， 在js中  this.xxx

​                <font color="red">   components :key</font> 是组件名， value 是组件对象

​                 <font color="red">  methods：</font>一般用来配合 xxx 事件

​                  <font color="red"> propos：</font>子组件，接受的参数设置['title']   

* 指令

  ​         <font color="red"> v-if / v-show: </font> if就是插入或移除的问题，show是否隐藏的问题

  ​         <font color="red"> v-else-if / v-else </font> 必须和 v-if 是相邻的元素

  ​          <font color="red">v-bind / v-on: </font> bind是给属性赋值， v-on是给事件进行绑定

  ​                <font color="red"> V-bind:</font>属性=“变量”   =====>   :属性名='xxx'

  ​                 <font color="red">V-on:</font>事件名=“函数”   ======>  @事件名

  ​         <font color="red"> v-bind / v-model: bind </font>就是单向数据流( vue->页面)  model是双向的(vue->页面，页面 ->vue)











### Vue.js基本传值方式

1. [Vue.js](https://v1-cn.vuejs.org/guide/) 最基本语法，从服务器端向前台发送数据

```html
<!-- 前台 HTML页面 -->
<!-- 1.引包 -->
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<!-- 前台接受后台传来的数据 id="app" -->
<div id="app">  </div>


<script> //服务器端
new Vue({
    el: '#app', // el 发生行为的目的地
    template:'<h1>你好 {{message}}</h1>', //装载的模板
    data: {  //动态数据的声明
        message: ' Vue.js!' // template中要使用的数据
    }
})
</script>

// 显示： 你好 Vue.js!
```

```html
<!-- 前台 HTML页面 -->
<!-- 1.引包 -->
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<!-- 前台接受后台传来的数据 id="app" -->
<div id="app">   </div>


<script>//服务器端
new Vue({
    el:document.getElementById('app'), ///////////////极度的优化才用，这样的方式更优化，不用动用 vue
    template:'<h1>你好 {{message}}</h1>', //装载的模板
    data: {  //动态数据的声明
        message: ' Vue.js!' // template中要使用的数据
    }
})
</script>
```

下面代码，也可以按照类名，或 标签名查找

```html
<!-- 前台 HTML页面 -->
<!-- 1.引包 -->
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<!-- 前台接受后台传来的数据 id="app" -->
<div id="app">  </div>


<script>//服务器端
new Vue({
    el:'div', ///////////////////////////////////////////// el 按照标签名查找
    template:'<h1>你好 {{message}}</h1>', //装载的模板
    data: {  //动态数据的声明
        message: ' Vue.js!' // template中要使用的数据
    }
})
</script>
```



### V-text 标签

只能用在双标签中，其实就是给元素的innerText 赋值

```html
<!--  -->
<!-- 1.引包 -->
<script src="https://npmcdn.com/vue/dist/vue.js"></script>
<!-- 前台接受后台传来的数据 id="app" -->
<div id="app"> </div>

<script> // v-text 标签，只能用在双标签中，其实就是给元素的innerText 赋值
new Vue({
    el:'div', // el 发生行为的目的地
    template: `<div> <h1 v-text="myText"></h1> </div>`,
    data:function(){
      return{
          myText:'我是v-text的值！'
      }
    }    
})
</script>
```



### V-html 标签

只能用在双标签中，其实就是给元素的innerHTML 赋值

```html
<!--  -->
<!-- 1.引包 -->
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<!-- 前台接受后台传来的数据 id="app" -->
<div id="app">  </div>


<script> // v-text 标签，只能用在双标签中，其实就是给元素的innerText 赋值
new Vue({
    el:'#app', // el 发生行为的目的地
    template: `<div> <span v-html="myHtml"></span> </div>`,
    data:function(){
      return{
          myHtml:'<h1>我是v-HTML的值！</h1>'
      }
    }    
})
</script>
```



### V-if 标签

如果值为 false ， 会留下一个 <!-- --> 作为标记，万一 v-if 的值变为 true 了，就在这里插入元素。

```html
<!--  -->
<!-- 1.引包 -->
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<!-- 前台接受后台传来的数据 id="app" -->
<div id="app"> </div>


<script> // v-text 标签，只能用在双标签中，其实就是给元素的innerText 赋值
new Vue({
    el:'#app', // el 发生行为的目的地
    template: `<div> <button v-if='isExit'> 测试v-if </button> </div>`, ///// v-if 标签用来判断
    data:function(){
      return{
          isExit:true  // 改变 true 为 false 可以显示按钮和隐藏按钮
      }
    }    
})
</script>
```



### V-else-if

如果 if 和 else 就不需要单独留坑了。

```html
<!--  -->
<!-- 1.引包 -->
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<!-- 前台接受后台传来的数据 id="app" -->
<div id="app">  </div>


<script> // v-text 标签，只能用在双标签中，其实就是给元素的innerText 赋值
new Vue({
    el:'#app', // el 发生行为的目的地
    template: `
    <div>  <!---以下的三个指令的使用，必须是相邻的DOM元素 -->
    	<button v-if='num == 1'> 测试v-if </button> 
        <button v-else-if='num == 2'> 测试 v-else-if </button>
        <button v-else> 测试 v-else </button>    
    </div>    
    `,
    data:function(){
      return{
          num:1  // 改变 num 的值 为 1 或者 2 或者 其它，看看效果
      }
    }    
})
</script>
```



### v-show 标签

 是否显示，值为 false 就不显示，true 就显示

```html
<!--  -->
<!-- 1.引包 -->
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<!-- 前台接受后台传来的数据 id="app" -->
<div id="app">  </div>



<script> // v-text 标签，只能用在双标签中，其实就是给元素的innerText 赋值
new Vue({
    el:'#app', // el 发生行为的目的地
    template: `
    <div> 
    	 <button v-show='isShow'>我是v-isShow</button>    
    </div>    
    `,
    data:function(){
      return{
         isShow:true
      }
    }    
})
</script>
```

总结
v-text: innerText

 v-html: innerHTML
v-if: 是否隐藏元素
 v-show:是否隐藏元素



### v-bind 标签

给元素的属性赋值       **v-bind:属性名=‘常量’ || 变量名**      简写形式   **:属性名=‘变量名’**

```html
<!-- <div v-bind:原属性名="变量"> </div>    简写的模式为：<div :属性名='变量'></div> -->
<!-- 1.引包 -->
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<!-- 前台接受后台传来的数据 id="app" -->
<div id="app">  </div>

<script> // v-text 标签，只能用在双标签中，其实就是给元素的innerText 赋值
new Vue({
    el:'#app', // el 发生行为的目的地
    template: `
    <div> 
       <input type='text' v-bind:value='myValue' />
       </br>
       <input type='text' :value='myValue' />
       </br>
       <input type='text' :value='myValue' :file=" 'xxx' "/>
    </div>    
    `,
    data:function(){
      return{
         myValue:'你好 Vue.js'
      }
    }    
})
</script>
```



### v-on 标签

处理自定义原生事件的， 给按钮添加 click 并让 使用变量的样式改变

v-on:原生事件名=‘给变量进行操作||函数名’

简写形式：  @原生事件名='给变量进行操作'

```html
<script src="https://npmcdn.com/vue/dist/vue.js"></script>
<div id="app">  </div>



<script>
	 new Vue({
      el:'#app',
      template:`
      		<div>
          	  <input type='text' v-bind:value="myValue" :file=" 'xxx' " />
              </br>
              <button v-on:click=" myValue='abc' "> 点我改变 myValue </button>
          </div>
      `,
      data:function(){
        return{
           myValue:' Hello Vue.Js'
        }
      }
   })

</script>
```

@click 简写绑定

```html
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<div id="app">  </div>


<script>
	 new Vue({
      el:'#app',
      template:`
      		<div>
          		<input type='text' v-bind:value="myValue" :file=" 'xxx' " />
              </br>
              <button v-on:click=" myValue='abc' "> 点我改变 myValue </button>
              <button @click=" myValue='defg' "> 点我简写改变 myValue </button>
          </div>
      `,
      data:function(){
        return{
           myValue:' Hello Vue.Js'
        }
      }
   })

</script>
```



### v-model 标签

Vue的数据双向绑定

**V-bind** 可以给任何属性赋值，是从 vue到页面的单向数据流

**v-model** 只能给具备 value属性的元素进行双向数据绑定（ <font color='green'>必须使用的是有 value的元素</font> ）

下面代码：当用户输入 xxxx 时，显示按钮。双向绑定

```html
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<div id="app">  </div>


<script>
	 new Vue({
      el:'#app',
      template:`
      	 <div>
          	  <input type='text' v-model="myValue" /> 用户输入 xxxx 时，显示按钮
              </br> 
              <button v-show =" myValue=='xxxx' "> 用户输入的是 xxxx </button>
              
          </div>
      `,
      data:function(){
        return{
           myValue:' sdf sd '
        }
      }
   })

</script>
```



### v-for 标签

1: v-for 的使用中，除了item属性，也还有一些辅助属性

 如果下面的代码里，stus 是数组：( item, index )  in stus 奇数，偶数，不同样式

：class=" index%2==0?'red':'green' "

```html
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<div id="app">  </div>


<script>
	 new Vue({
      el:'#app',
      template:`
      		<div>
          		<ul>
              	 <li v-for="item in stus"> 
                 		{{item.name}}
                 </li>
              </ul>              
          </div>
      `,
      data:function(){
        return{
           stus:[{name:'张三'},{name:'李四'},{name:'王五'}]
        }
      }
   })

</script>
```

```html
<head>
  <style>
     .a{
       background-color:red;
     }
     .b{
       background-color:green;
     }
  </style>
</head>
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<div id="app">  </div>


<script>
	 new Vue({
      el:'#app',
      template:`
      		<div>
          		<ul>
              	 <li v-for="item in stus" :class='item.myClass'> 
                 		{{item.name}}
                 </li>
              </ul>              
          </div>
      `,
      data:function(){
        return{
           stus:[{name:'张三',myClass='a'},{name:'李四',myClass='b'},{name:'王五',myClass='c'}]
        }
      }
   })

</script>
```

```html

<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<div id="app">  </div>


<script>
	 new Vue({
      el:'#app',
      template:`
      		<div>
          		<ul>
              	 <li v-for="(val,key,index) in stus"> 
                 		key:{{key}}
                    val:{{val}}
                    index:{{index}}
                 </li>
              </ul>              
          </div>
      `,
      data:function(){
        return{
           stus:{'A':'张三','B':'李四','C':'王五'}
        }
      }
   })

</script>
```



### Methods 的用法

```html

<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<div id="app">  </div>

<script>
	 new Vue({
      el:'#app',
      template:`
      		<div>
          		<h1 v-show='isShow'>1 v-show</h1>
              <h1 v-show='isShow'>2 v-show</h1> 
              <h1 v-show='isShow'>3 v-show</h1>               
              <h1 v-if='isShow'>4 v-if</h1> 
              </br>
              <button @click='changeIsShow'>点我更酷</button>
          </div>
      `,
      data:function(){
        return{
           isShow:true
        }
      },
      methods:{
         changeIsShow:function(){
            this.isShow = !this.isShow
         }
      }
   })

```



### vue的组件化

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
			//1. 生出子，声明入口组件
			var App={  //App 首字母要大写
				template:'<center><h1>我是入口组件</h1></center>'
			};			
			
			
			new Vue({
				el:'#app',
				components:{ // 2.声明子，声明要用的组件们
					// key是组件名,value是组件对象
					app:App
				},
				template: ' <app/> ' //3.使用子，入口组件				
			});
			// 父组件用子组件事项
			// 1. 要生出子,声明子,使用子			
		</script>
	</body>
</html>
```

组件化的调用方式

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
            
            // 父组件用子组件事项
			//要生出子,声明子,使用子
			//1. 生出子，声明入口组件
			//声明头组件
			var MyHeader = {
				template:'<div><center><h1>我是头部</h1></center></div>'
			}
// 			var MyBody = Vue.extend({  //函数调用的方式很少用,了解以下就行
// 				template:'<div><center><h1>我是函数调用的中部</h1></center></div>'
// 			})
			
			var MyBody = {
				template:'<div><center><h1>我是中部</h1></center></div>'
			}
			var MyFooter = {
				template:`<div>
				<center>
					<h1>我是底部</br><button @click='showNum(123)'>点我</button></h1>
				</center>				
				</div>`,
				methods:{
					showNum: function(num){
						alert(num);
					}
				}
			}
			
			var App={  //App 首字母要大写
			    components:{
					'my-header':MyHeader,
					'my-body' : MyBody,
					'my-footer':MyFooter,
				},
				template:`
					<div>
						<my-header> </my-header>
						<my-body> </my-body>
						<my-footer> </my-footer>
					</div>				
				`
			};		
			
			new Vue({
				el:'#app',
				components:{ // 2.声明子，声明要用的组件们
					// key是组件名,value是组件对象
					app:App
				},
				template: ' <app/> ' //3.使用子，入口组件				
			});
			// 父组件用子组件事项
			// 1. 要生出子,声明子,使用子
			
		</script>
	</body>
</html>
```



###父传子总结

父组件向子组件传递数据

​    父用子  先有子，声明子，使用子

​    父传子  父传子（属性），子声明（收），子直接用（就像自己的一样）

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
			
			var Son={
				template:`
				   <div>
				      接收到父组件的数据是:{{ title }} //传变量到子
				          <h1>1</h1>
						  <h1>2</h1>
						<ul>
							<li></li>
						</ul>
						<button>改变显示</button>
				   </div>
				`,
				//声明接收参数
				props:['title'],
			}
			
			//父
			//其实，父向子传递，就是 v-bind 给元素赋值
			var App = {
				components:{
					son:Son
				},
				template:`
					<center>
						<div>
						   <son :title='xxx'> </son>
						   <!-- 传递常量的方法 <son title="xxx"></son> -->
						</div>
					</center>
				`,
				data:function(){
					return {xxx:'我是xxx数据'}
				}
			}
			
			
			
			new Vue({
				//data就不在这里用了
				el:'#app',
				
				//声明子组件
				components:{ // 2.声明子，声明要用的组件们
					// key是组件名,value是组件对象
					app:App
				},
				
				template: ' <app/> ' //3.使用子，入口组件				
			});
			// 父组件用子组件事项
			// 1. 要生出子,声明子,使用子
			
		</script>
	</body>
</html>
```



### 单向数据流 和 双向数据流的区别

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
			var Son={
				template:`
				   <div>				     
						<hr/>
						<br/>
						单向数据流(vue->html): <!-- :value 就是 v-bind:value 的缩写 -->
						<input type="text" :value="text"/>
						<br/>
						双向数据流(vue->html->vue):
						<input type="text" v-model="text"/>
						<br/>
						<h1>主要看下面</h1>
						<br/>
						{{text}}						
				   </div>
				`,
				data:function(){
					return {
						xxx:true,
						text:'hello'
					}
				}
			}
			
			//父
			//其实，父向子传递，就是 v-bind 给元素赋值
			var App = {
				components:{
					son:Son
				},
				template:`
					<center>
						<div>
						   <son :title='xxx'> </son>
						   <!-- 传递常量的方法 <son title="xxx"></son> -->
						</div>
					</center>
				`,
				data:function(){
					return {xxx:'我是xxx数据', text:'hello'}
				}
			}			
			
			
			new Vue({
				//data就不在这里用了
				el:'#app',
				
				//声明子组件
				components:{ // 2.声明子，声明要用的组件们
					// key是组件名,value是组件对象
					app:App
				},
				
				template: ' <app/> ' //3.使用子，入口组件				
			});
			// 父组件用子组件事项
			// 1. 要生出子,声明子,使用子
			
		</script>
	</body>
</html>
```

