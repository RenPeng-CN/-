[TOC]

### vuex 到底是什么

vuex 就是解决不同组件之间的 数据共享，以及数据持久化



---



### Vuex 初始化配置

在 MAC终端，执行以下代码，全局安装 vue-cli 插件

```js
 sudo npm install --global vue-cli
```



初始化 webpack 项目

```js
vue init webpack test_vuex
```



安装 vuex 插件

```js
npm install --save vuex
```



安装 axios插件，axios基于Promise的HTTP请求客户端，可同时在浏览器和node.js中使用功能特性在浏览器中发送 XMLHttpRequests 请求在node.js中发送 http请求支持 Promise API拦截请求和响应转换请求和响应数据自动转换JSON数据客户端支持保护安全免受 XSRF 攻击 .

```js
npm i -S axios
```





进入开发模式

```js
npm run dev
```



开发完成后，建立编译项目, 将自动产生 dist 文件夹，里面包含了 index.html 以及其它文件夹，将这些一起部署到服务器就可以了

```js
npm run build
```





---



### vuex案例

mail.js 页面

```js
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router'

Vue.config.productionTip = false

import Vuex from 'vuex' // 第一步、引入 Vuex
Vue.use(Vuex) // 第二步、安装 vuex 插件

// 引入 axios
import Axios from 'axios'
Vue.prototype.$axios = Axios;

import numModule from '@/modules/numModule';
   

let store = new Vuex.Store( // 第三步、创建 store 
	{  // 第四步、配置 store 中的数据/存/取
		modules: {
			numModules: numModule
		}
	}
);

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  store, // 将 store 对象关联到 vue 实例
  components: { App },
  template: '<App/>'
})

```



App.vue 页面

```vue
<template>
  <div id="app">
    num的数量 {{ appShowNum }}--><button @click="changeNum">改变</button>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
	computed: {
		appShowNum() {
			return this.$store.getters.getNum
		}		
	},
	methods: {
		changeNum(){
			//一般写代码都不直接提交，除非明知是同步操作，没有后台请求
			// 完美的写法是
			this.$store.dispatch('addNumByServerRate', { num:10 })
		}		
	}
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```



numModule.js 页面

```js
export default {
	state:{
		num: 30
	},
	getters: {
		getNum(state) {
			return state.num
		}
	},
	mutations: { // 只能同步，不能异步
		addNum(state, payload){			
			state.num += payload.num
		}
	},
	// actions 是行为, 在行为中，可以保存异步操作，但最终还是需要通知 mutations
	actions:{ 
		addNumByServerRate(store, payload) {
			// 这里有异步没有关系，可以记录快照
			setTimeout(function(){
			 let rate = 1;
			 store.commit('addNum', { num:payload.num * rate})
			},1000)
		}
	}
	
}
```







###vuex的使用：


​	

```js
第一步、src目录下面新建一个vuex的文件夹
第二步、vuex 文件夹里面新建一个store.js
第三步、安装vuex  
	npm install vuex --save

第四步、在刚才创建的store.js引入vue  引入vuex 并且use vuex
	import Vue from 'vue'
	import Vuex from 'vuex'
	Vue.use(Vuex)

第五步、定义数据
		/*1.state在vuex中用于存储数据*/
		var state={
		    count:1
		}
      
第六步、定义方法	 mutations里面放的是方法，方法主要用于改变state里面的数据

	var mutations={
	    incCount(){
		   ++state.count;
	    }
	}

第七步、暴露
	const store = new Vuex.Store({
	    state,
	    mutations
	})	
	export default store;
```


​	


在组建里面使用vuex：


```js
第一步、引入 store
		 import store from '../vuex/store.js';

第二步、注册
		 export default{
			data(){
			    return {               
			       msg:'我是一个home组件',
				value1: null,			     
			    }
			},
			store,
			methods:{
			    incCount(){			      
				this.$store.commit('incCount');   /*触发 state里面的数据*/
			    }
			}
		    }

第三步、获取state里面的数据  

		this.$store.state.数据

第四步、触发 mutations 改变 state里面的数据
		
		this.$store.commit('incCount');
```



再学一遍 Vuex记录 2019年8月20日

```js
// 打开项目的 main.js 页面
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

// 第一步、引入 Vuex
import Vuex from 'vuex'

// 第二步、安装插件
Vue.use(Vuex)

// 第三步、创建 store
let store = new Vuex.Store({
  // 第四步、配置 store中的数据/存/取
  // 1. 设置数据
  state:{
    num:30
  },
  // 定义取数据的功能
  getters:{
    // 2. 获取数据
    getNum(state){
      return state.num
    }
  },
  // 3. 操作数据，切记，mutations 只能用 同步的方法，不能用异步，异步会丢失记录
  mutations:{
    addNum(state,payload){ // payload 为客户传入的数据，用以改变addNum里的值
      state.num += payload.num; // payload.num 是用户指定的数值
    }
  },
  // actions ,行为，在 actions中，可以存在异步操作，但是最终还是通知mutations
  actions:{
    addNumByServerRate(store,payload){
      store.commit('addNum',{num:5})
    }
  }
})



new Vue({
  el:'#app',
  // 第五步、将store 对象关联 vue 实例
  store,
  components:{App},
  templatge:'<App/>'
})
// 配置完成，在其它页面就可以放心的使用了










// 提示，以下为其它页面哦，不能整体复制，只供参考
// 在computed: 里面实时监控
computed:{
  appShowNum(){
    return this.$store.getters.getNum
  }
}

<template>
 {{ appShowNum }} // 就可以获取数值了
 <button @click='change'> 改变数据 </button>
 </template>
<script>
	methods:{
    // 提示：this.$store.state.xxx 也可以改值，但不是官方推荐的，这样用会被揍的
    change(){
      this.$store.dispatch('addNumByServerRate',{num:10})
    }
  }   
</script>
```































