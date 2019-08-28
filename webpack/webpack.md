

[TOC]



# webpack简介

**[webpack 中文文档](https://www.webpackjs.com/concepts/)**

 webpack 需要安装的包

```js
// 安装 webpack 包
npm i -s webpack

// 安装 webpack loader 包, 可以将不是 js 的模块转化为 js
npm install --save-dev css-loader // 用以支持 css 文件
npm install --save-dev ts-loader
npm install --save-dev url-loader file-loader  //用以支持图片链接和图片文件

```

```js
//安装 less 依赖包
npm i -s less
npm i -s url-loader
npm i -s style-loader
npm i -s less-loader
```

```js
// html-webpack-plugin 下载并配置此插件
npm i -s html-webpack-plugin
```

```js
// webpack-dev-server 实时热拔插 webpack插件，后台一改代码，前台页面可以实时渲染，酷啊！
// 所有目录有不要用汉字，标点符号，一律用字母，否则会报错。
// 开发的时候用，开发完成后就可以关闭了。 
npm i -s webpack-dev-server
```

```js
//安装 webpack vue 相关的插件
npm i --save vue
npm i --save vue-loader  
npm i --save vue-template-compiler   // vue 编译器
// npm i --save vue-template-compiler@2.5.16  // 如果要指定版本，就这样写
```



本质上，*webpack* 是一个现代 JavaScript 应用程序的*静态模块打包器(module bundler)*

- **入口(entry) **入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部*依赖图*的开始
- **输出(output)** **output** 属性告诉 webpack 在哪里输出它所创建的 *bundles*，以及如何命名这些文件，默认值为 `./dist`。
- **loader** loader 被用于转换某些类型的模块 ,让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）
- **插件(plugins)**



# webpack实例1

### webpack 基本配置

1. **配置 webpack.config.js 文件**

   ```js
   const config = {
   	entry: {'main':'./main.js'}, //main 这个名字可以随便起
   	output:{
   		filename:'./build.js'	// 文件名可以自定义 ，但要跟 项目页面内容引用一致		
   	},
   	watch:true, //监视文件发生改动，自动产出Build.js
   	mode: 'production' //设定好是 生产模式，还是开发模式
   }
   
   module.exports = config; 
   ```



2. **配置文件可以做成两中  **

   * webpack.dev.config.js  // 开发环境

     ```js
     // dev （ development ) 生产环境   去掉了 watch:true ，不用再监视了。 
     const config = {
     	entry: {'main':'./main.js'}, //main 这个名字可以随便起
     	output:{
     		filename:'./build.js'	// 文件名可以自定义	，但要跟 项目页面内容引用一致		
     	},
     	watch:true, //监视文件发生改动，自动产出Build.js
     	mode: 'development' //设定好是 生产模式，还是开发模式
     }
     
     module.exports = config; 
     ```

     

   

   * webpack.prod.config.js  // 生产环境

     ```js
     // prod ( production )生产环境   去掉了 watch:true ，不用再监视了。 
     const config = {
     	entry: {'main':'./main.js'}, //main 这个名字可以随便起
     	output:{
     		filename:'./build.js' // 文件名可以自定义 ，但要跟 项目页面内容引用一致			
     	},
     	mode: 'production' //设定好是 生产模式，还是开发模式
     }
     
     module.exports = config; 
     ```

     

**在 package.json 文件里，配置 开发 和 生产 两种环境的 地址**

```js
 "scripts": {
    "dev": "webpack --config ./webpack.dev.config.js",
	"prod": "webpack --config ./webpack.prod.config.js"
  },
```



用简写的方式 把 css文件引入 main.js 入口文件中, 必须引入 css-loader 模块，否则会报错

```js
import './main.css';  // 用简写的方式引入 css 文件
```

```js
//必须引入 node css-loader 支持模块，否则会报错.  
// npm i css-loader -s 
ERROR in ./main.css 1:4
Module parse failed: Unexpected token (1:4)
You may need an appropriate loader to handle this file type.
> body{
| 	background-color: red;
| }
 @ ./main.js 1:0-20
```

npm run dev #执行后，会产生一个 build.js 文件。 main 就是 build



###css-loader 的配置方法

webpack.dev.config.js

```js

const config = {
	entry: {'main':'./main.js'}, //main 这个名字可以随便起
	output:{
		filename:'./build.js'		
	},
	watch:true, //监视文件发生改动，自动产出Build.js
	
	//声明模块，包含着各个 loader
	module:{
		rules:[  // webpack 4 之前的版本 不叫 rules: , 而叫 loaders:
		     //声明 css格式的文件，使用 css-loader模块
			{ test: /\.css$/, loader:'style-loader!css-loader'} 
		]
	},
	mode: 'development'
}

module.exports = config; 
```



###Url-loader 的配置方法

以下代码，为通过 webpack 加载图片的配置方法

webpack.dev.config.js

```js
const config = {
	entry: {'main':'./main.js'}, //main 这个名字可以随便起
	output:{
		filename:'./build.js'		
	},
	
	
	//声明模块，包含着各个 loader
	module:{
		rules:[  // webpack 4 之前的版本 不叫 rules: , 而叫 loaders:
		     //声明 css格式的文件，使用 css-loader模块
			{ test: /\.css$/, loader:'style-loader!css-loader'},
			
			//为图片类文件 配置 模块, 
            //limit 的值 如果比原图大，就回自动生成一张新的图片，如果小，就不会生成
            //如果文件大于limit，就生成一个文件
            // 小于limit，则生成 base64 到 build.js 中
            //建议比较小的图片，使用 base64 的方式
            // 因为 base64 文件会比原先的文件大 30% 左右。占用流量
            // 但是如果图片比较小，就使用 base64, 因为减少了一次请求
            // 建议设置为 4096 , 就是 4kb 
			{ test:/\.(jpg|png|gif|svg)$/, loader:'url-loader?limit=2000000'}
		]
	},
	mode: 'development',
	watch:true  //监视文件发生改动，自动产出Build.js
}

module.exports = config; 
```

mian.js

```js
import './main.css';  // 用简写的方式引入 css 文件
import Vue from './node_modules/vue/dist/vue.js';
import App from './App.js';

new Vue({
	el:'#app',
	components:{
		app:App
	},
	template:'<app/>'
})
```

main.css

```css
body{
	background-color: red;
}
```

App.js

```js
import img from './a.jpg';
export default {
	template: `
		<div>
		   <img :src="imgSrc" />
		</div>
	`,
	data(){
		return {
			imgSrc:img
		}
	}
}
```

index.html

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app"></div>
		<script type="text/javascript" src="./dist/build.js"></script>
	</body>
</html>

```

package.json

```json
{
  "name": "vuetest",
  "version": "1.0.0",
  "description": "Vue.js Test",
  "main": "vuetest.html",
  "scripts": {
    "dev": "webpack --config ./webpack.dev.config.js",
	"prod": "webpack --config ./webpack.prod.config.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "axios": "^0.18.0",
    "nrm": "^1.0.2",
    "vue": "^2.5.22",
    "vue-router": "^3.0.2",
    "webpack": "^4.29.0"
  },
  "devDependencies": {
    "css-loader": "^2.1.0",
    "ts-loader": "^5.3.3",
    "webpack-cli": "^3.2.1"
  },
  "keywords": []
}

```



<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app"></div>
		<script type="text/javascript" src="./dist/build.js"></script>
	</body>
</html

### less-base64

Webpack.dev.config.js

```js
const path = require('path');//引入node的 path 包

const config = {
	entry: {'main':'./main.js'}, //main 这个名字可以随便起
	output:{
		// path.resolve 是把相对路径转换为绝对路径
		path:path.resolve('./dist'),
		filename:'build.js'		
	},
	
	
	//声明模块，包含着各个 loader
	module:{
		rules:[  // webpack 4 之前的版本 不叫 rules: , 而叫 loaders:
		     //声明 css格式的文件，使用 css-loader模块
			{ test: /\.css$/, loader:'style-loader!css-loader'},
			
			//为图片类文件 配置 模块, limit 的值 如果比原图大，就回自动生成一张新的图片，如果小，就不会生成
			{ test:/\.(jpg|png|gif|svg)$/, loader:'url-loader?limit=2000000'},
            //先解析less 成 Css，再解析css成 style，位置不能反。
			{ test:/\.less$/,loader:'style-loader!css-loader!less-loader'}
		]
	},
	mode: 'development',
	watch:true  //监视文件发生改动，自动产出Build.js
}

module.exports = config;   
```

main.js

```js
import './main.less';
```



main.less

```less
@imgPath:'a.jpg';

body{
	background-image: url(@imgPath);
}
```

package.json

```json
{
  "name": "vuetest",
  "version": "1.0.0",
  "description": "Vue.js Test",
  "main": "vuetest.html",
  "scripts": {
    "dev": "webpack --config ./webpack.dev.config.js",
	"prod": "webpack --config ./webpack.prod.config.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "axios": "^0.18.0",
    "nrm": "^1.0.2",
    "vue": "^2.5.22",
    "vue-router": "^3.0.2",
    "webpack": "^4.29.0"
  },
  "devDependencies": {
    "css-loader": "^2.1.0",
    "ts-loader": "^5.3.3",
    "webpack-cli": "^3.2.1"
  },
  "keywords": []
}
```



html-webpack-plugin

html的移动插件，会自动在 html 页面插入 js 引入代码

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app"></div>	
		
	</body>
</html>
```

成功后

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app"></div>	
        <!-- 会自动插入下面这行 -->
		<script type="text/javascript" src="build.js"></script> 
	</body>
</html>
```



Webpack.dev.config.js

```js
const path = require('path');//引入node的 path 包
const HtmlWebpackPlugin = require('html-webpack-plugin');

const config = {
	entry: {'main':'./main.js'}, //main 这个名字可以随便起
	output:{
		// path.resolve 是把相对路径转换为绝对路径
		path:path.resolve('./dist'),
		filename:'build.js'		
	},
	
	
	//声明模块，包含着各个 loader
	module:{
		rules:[  // webpack 4 之前的版本 不叫 rules: , 而叫 loaders:
		     //声明 css格式的文件，使用 css-loader模块
			{ test: /\.css$/, loader:'style-loader!css-loader'},
			
			//为图片类文件 配置 模块, limit 的值 如果比原图大，就回自动生成一张新的图片，如果小，就不会生成
			{ test:/\.(jpg|png|gif|svg)$/, loader:'url-loader?limit=2000000'},
			//先解析less 成 Css，再解析css成 style，位置不能反。
			{ test:/\.less$/,loader:'style-loader!css-loader!less-loader'}
		]
	},
	mode: 'development',
	plugins:[
		//插件的执行顺序与元素索引有关
		new HtmlWebpackPlugin({
			template:'./index.html', //参照物
		})
	],
	watch:true  //监视文件发生改动，自动产出Build.js
}

module.exports = config;  
```



### webpack-dev-server 还没有解决

* --open 自动打开浏览器
* --hot 热替换，在不刷新的情况下替换css样式
* --inline 自动刷新
* --port 9999 指定端口

可以热拔插插件，实时监控，实时渲染

```
//安装插件包
npm i -s webpack-dev-server

```

Package.jason

```json
{
  "name": "vuetest",
  "version": "1.0.0",
  "description": "Vue.js Test",
  "main": "vuetest.html",
  "scripts": {
    "dev": "webpack-dev-server --open --hot --inline --config ./webpack.dev.config.js",
    "prod": "webpack --config ./webpack.prod.config.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "axios": "^0.18.0",
    "html-webpack-plugin": "^3.2.0",
    "less": "^3.9.0",
    "less-loader": "^4.1.0",
    "nrm": "^1.0.2",
    "style-loader": "^0.23.1",
    "url-loader": "^1.1.2",
    "vue": "^2.5.22",
    "vue-router": "^3.0.2",
    "webpack": "^4.29.0",
    "webpack-dev-server": "^3.1.14"
  },
  "devDependencies": {
    "css-loader": "^2.1.0",
    "ts-loader": "^5.3.3",
    "webpack-cli": "^3.2.1"
  },
  "keywords": []
}


```



### ES 6  

webpack 处理  ES 6,7,8

```js
//安装插件, 注意，babel-loader 要安装7版本，否则会报错。 
npm install --save  babel-core babel-loader@7 babel-preset-env babel-plugin-transform-runtime
```



Webpack.dev.config.js

```js
const path = require('path');//引入node的 path 包
const HtmlWebpackPlugin = require('html-webpack-plugin');

const config = {
	entry: {'main':'./main.js'}, //main 这个名字可以随便起
	output:{
		// path.resolve 是把相对路径转换为绝对路径
		path:path.resolve('./dist'),
		filename:'build.js'	,
	},
	
	
	//声明模块，包含着各个 loader
	module:{
		rules:[  // webpack 4 之前的版本 不叫 rules: , 而叫 loaders:
		 //声明 css格式的文件，使用 css-loader模块
		{ test: /\.css$/, loader:'style-loader!css-loader'},
			
	//为图片类文件 配置 模块, limit 的值 如果比原图大，就回自动生成一张新的图片，如果小，就不会生成
		{ test:/\.(jpg|png|gif|svg)$/, loader:'url-loader?limit=2000000'},
		//先解析less 成 Css，再解析css成 style，位置不能反。
		{ test:/\.less$/,loader:'style-loader!css-loader!less-loader'},
		//处理 ES 6，7，8
		{ test:/\.js$/, loader:'babel-loader',exclude:/node_modules/, //排出包含 node_modules 目录
			options: {
				presets:['env'],// 指定处理关键字
				plugins:['transform-runtime'] // 处理函数
				
			}}
			
		]
	},
	mode: 'development',
	plugins:[
		//插件的执行顺序与元素索引有关
		new HtmlWebpackPlugin({
			template:'./index.html', //参照物
		})
	],
	watch:true  //监视文件发生改动，自动产出Build.js
}

module.exports = config;   
```



### single_file 单文件. -- 编写vue 的最终形式

webpack.dev.config.js

```js
const path = require('path');//引入node的 path 包
const HtmlWebpackPlugin = require('html-webpack-plugin');
const VueLoaderPlugin = require('vue-loader/lib/plugin');

const config = {
	entry: {'main':'./main.js'}, //main 这个名字可以随便起
	output:{
		// path.resolve 是把相对路径转换为绝对路径
		path:path.resolve('./dist'),
		filename:'build.js'	,
	},
	
	
	//声明模块，包含着各个 loader
	module:{
		rules:[  // webpack 4 之前的版本 不叫 rules: , 而叫 loaders:
		     //声明 css格式的文件，使用 css-loader模块
			{ test: /\.css$/, loader:'style-loader!css-loader'},
			
			//为图片类文件 配置 模块, limit 的值 如果比原图大，就回自动生成一张新的图片，如果小，就不会生成
			{ test:/\.(jpg|png|gif|svg)$/, loader:'url-loader?limit=2000000'},
			//先解析less 成 Css，再解析css成 style，位置不能反。
			{ test:/\.less$/,loader:'style-loader!css-loader!less-loader'},
			//处理 ES 6，7，8
			{ test:/\.js$/, loader:'babel-loader',exclude:/node_modules/, //排出包含 node_modules 目录
			options: {
				presets:['env'],// 指定处理关键字
				plugins:['transform-runtime'] // 处理函数
				
			}},
			
			//处理 vue
			{ test:/\.vue$/, loader:'vue-loader'}
			
		]
	},
	mode: 'development',
	plugins:[
		//插件的执行顺序与元素索引有关
		new HtmlWebpackPlugin({
			template:'./index.html', //参照物
		}),
		new VueLoaderPlugin()
	],
	watch:true  //监视文件发生改动，自动产出Build.js
}

module.exports = config;   
```



package.json

```json
{
  "name": "vuetest",
  "version": "1.0.0",
  "description": "Vue.js Test",
  "main": "vuetest.html",
  "scripts": {
    "dev": "webpack --config ./webpack.dev.config.js",
    "prod": "webpack --config ./webpack.prod.config.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "axios": "^0.18.0",
    "babel-core": "^6.26.3",
    "babel-loader": "^7.1.5",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-preset-env": "^1.7.0",
    "html-webpack-plugin": "^3.2.0",
    "less": "^3.9.0",
    "less-loader": "^4.1.0",
    "nrm": "^1.0.2",
    "style-loader": "^0.23.1",
    "url-loader": "^1.1.2",
    "vue": "^2.5.22",
    "vue-loader": "^15.6.2",
    "vue-router": "^3.0.2",
    "vue-template-compiler": "^2.5.22",
    "webpack": "^4.29.0",
    "webpack-dev-server": "^3.1.14"
  },
  "devDependencies": {
    "css-loader": "^2.1.0",
    "ts-loader": "^5.3.3",
    "webpack-cli": "^3.2.1"
  },
  "keywords": []
}

```



App.js

```js
import Vue from 'vue'; // vue 是 node_modules 下面的 vue,默认是 runtime阉割版
import App from './App.vue'

new Vue({
	el:'#app',
	render:c=>c(App) //相当于 creater=>creater(App)
// 	components:{
// 		app:App
// 	},
// 	template:`
// 		<div>
// 			
// 		</div>
// 	`
});
```



index.html

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app"></div>	
		
	</body>
</html>

```



App.vue

```vue
<template>
	<div> {{text}} </div>
</template>

<script>
	export default {
		data(){
			return{
				text:'Hello Vue!'
			}
		}
	}
</script>

<style>
	body{
		background-color: yellowgreen;
	}
</style>

```

