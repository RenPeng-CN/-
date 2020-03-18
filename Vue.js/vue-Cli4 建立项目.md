## vue-cli4  (vue 脚手架3 的项目搭建)

```js
// MAC 电脑终端 升级 vue-cli
sudo npm install -g @vue/cli  

// windows 电脑升级 vue-cli
npm install -g @vue/cli

// 查看 vue-cli 版本
vue -V  或者 vue --version

// 在 mac 终端 cd 到要创建项目目录的 位置，然后开始创建项目
// cli3.XXX 的用法（推荐） 直接连 git 都自动创建好了，Element-UI 也自动安装好了
$ vue create my-app
cd my-app // 进入创建好的项目
npm run server  // 运行此项目
npm run build // 打包项目
vue add element // 安装 element,用此方法安装 element-ui 不一定会是最新版
npm i -S element-ui // 安装最新版本的 element-ui

// 配置 vue.config.js (关闭保存代码错误提示)
module.exports = {
	lintOnSave: false
}

// 在终端，先停止服务，然后输入
vue ui // 打开项目管理的图形界面，关闭 ESlint 每次保存不在检查格式
```


#### 引入 BootStrap 4 CSS (因为 Element-UI 有缺陷的地方，需要使用 BootStrap 4 CSS 来弥补)
[点击这里，打开BootStrap 官网，去复制 CSS 引入地址](https://v4.bootcss.com/docs/4.3/getting-started/introduction/)
``` js
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">

以上link连接就是复制好的 bootstrap 4 css 的引入地址
把此地址粘贴到新建立好的 vue-cli3 项目里的 public/index 页面里的 头部 <head> 里
```

### 如果不想通过引入的方式，也可以手工复制 css 文件到项目里，使用起来更方便
``` js
// 第一步，打开此地址，复制 css 内容
https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css

// 第二步、在项目根目录建立 common 文件夹 并建立 bootstrap4.css 文件，粘贴进去就完成了
```

### 安装 vue-router 并使用
``` js
// 第一步 npm 安装 
npm install vue-router --save

// 第二步 在项目 src 目录下，新建 router.js 文件
import Vue from 'vue'; // 引入 vue.config.js 文件
import Router from 'vue-router'; // 引入 vue-router 路由文件

Vue.use(Router); // 在 Vue 中注册以下 Router,就可以全局使用 vue-router 了

export default new Router({
	routes:[]
})


// 第三步、在 main.js 文件里 引入 router.js 文件
import Vue from 'vue'
import App from './App.vue'
import './plugins/element.js'
import router from './router.js' // 引入 router.js 路由文件

Vue.config.productionTip = false

new Vue({
  router, // 在 vue 中 注册以下，就可以全局使用了
  render: h => h(App),
}).$mount('#app')



// 第四步、路由的简化设计，在 src/common/config/router.js 里粘贴下面的路由设计
//路由设计规则：
// 一、例如： index/index, shop/index 以index结尾的， path 和 name默认去除index
// 二、例如：shop/list. 默认生成 name为 shop_list (如果结尾为 index，例如 shop/index 则是 shop)
// 三、自定义填写 name 和 path 后，则不再自动生成
let routes = [
		{
			path:'/',
			name:'layout',
			redirect:{name:'index'},
			component:'layout',
			children:[
				{
					component:'index/index'
				}
			]
		},
		{
			component:'login/index'
		},
		{
			path:'*',
			redirect:{name:'index'}
		}
	]

// 获取路由信息的方法
let getRoutes = function()
{   // 生成 name
    
    // 生成 path
	// 自动生成路由component
	createRoute(routes)
	return routes
}

// 自动生成路由component
function createRoute(arr){
	for (let i = 0; i < arr.length; i++) {
		if(!arr[i].component) return
		// 去除路由结尾的 index
		let val = removeIndex(arr[i].component)
		// 生成路由的name, 把字符串中的 / 换成 _ 
		arr[i].name = arr[i].name || val.replace(/\//g,'_')
		// 生成 path, 如果用户自动生成了 path，就不再生成了
		arr[i].path = arr[i].path || `/${val}`
		// 自动生成 component
		let componentFun = import(`@/views/${arr[i].component}.vue`)
		arr[i].component = ()=>componentFun
		if(arr[i].children && arr[i].children.length > 0){
			createRoute(arr[i].children)
		}
	}
}

// 去除结尾的 index
function removeIndex(str){
	// str = login/index
	// 获取最后一个 / 在字符串中的索引
	let index = str.lastIndexOf('/')
	// 获取最后一个 / 后面的值
	let val = str.substring(index+1,str.length)
	// 判断是不是index结尾
	if(val === 'index'){
		return str.substring(index,-1)
	}
	return str
	
}


export default getRoutes()
```