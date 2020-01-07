## 创建纯 nvue 项目的步骤
#### 1. 在 HBuilderX 编译器里创建 uni-app 新项目
#### 2. 打开新项目中的 manifest.json 文件，添加 
``` js
        "renderer" : "native", //App端启用纯原生渲染模式
        "nvueCompiler" : "uni-app", //启用 uni-app 编译模式,而不是 weex 编译模式
```

#### 3. 引入全局样式文件 **.css ，并在 app.vue 中引入 
``` js
    <style>
    	/*每个页面公共css */
    	@import "./common/customize.css";
    	@import "./common/common.css";
    </style>
```

#### 4. 在 阿里巴巴的 图标库 (https://www.iconfont.cn/) 建立并引入项目图标
``` js
// 在 app.vue 页面的 onLaunch: function()（）函数里加载 图标引入，此方法只支持 nvue，不支持小程序等其它方式
            // #ifdef APP-PLUS-NVUE
			// 加载 iconfont 公共图标库 此方法只适合 NVUE 不适合 小程序和 H5
			const domModule = weex.requireModule('dom');
			domModule.addRule('fontFace', {
				fontFamily: 'iconfont',
				src: "url('https://at.alicdn.com/t/font_1489524_f4uyeo9ngdc.ttf')"
			});
			// #endif
```
		 
小程序 图标库引入方法, 在项目 common 下面建立 自定义名称 css 文件，比如说 free-icon.css, 将 阿里巴巴图标库 相应的项目图标 下载到本地，将 iconfont.css 文件内容复制到 free-icon.css 文件里, 删除多余的代码，留下以下内容
``` js
/* 此页面方法为 小程序 和 H5 端 的 iconfont 引入方法. App.vue 页面也有相同链接,需要同步修改 ,项目完成后也可以下载到本地，从本地引入*/
@font-face {font-family: "iconfont";
  src:url('https://at.alicdn.com/t/font_1489524_f4uyeo9ngdc.ttf') format('truetype')
}

.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

// 在 app.vue 页面的 style 里引入该文件
 //以下小程序 图标的引入方法，为 非 nvue 方式的
 
 /* #ifndef APP-PLUS-NVUE */
 @import "./common/free-icon.css"; /* 引入 小程序 和 H5 端的 iconfont, 如果不在 nvue环境，就使用 */
 /* #endif */
```

#### 5.底部导航开发,在 uni-app 官网里可以看到 tabbar的配置方法
1. 从 阿里巴巴图标库，下载底部导航的相应 图标，点击前和点击后的两种效果图片，放到 static文件夹下面的 tabbar 文件夹里
2. 在 pages.json 文件里写入
``` js
// 底部导航条
	"tabBar":{
		"borderStyle":"black",  // 边框颜色
		"backgroundColor":"#F7F7F7", // 背景颜色
		"color":"#000000", // 字体颜色
		"selectedColor":"#08C261", // 选中后的颜色
		"list":[
			{
				"iconPath":"static/tabbar/index.png",
				"selectedIconPath":"static/tabbar/index-select.png",
				"text":"首页",
				"pagePath":"pages/tabbar/index/index"
			},
			{
				"iconPath":"static/tabbar/mail.png",
				"selectedIconPath":"static/tabbar/mail-select.png",
				"text":"通讯录",
				"pagePath":"pages/tabbar/mail/mail"
			},
			{
				"iconPath":"static/tabbar/find.png",
				"selectedIconPath":"static/tabbar/find-select.png",
				"text":"发现",
				"pagePath":"pages/tabbar/find/find"
			},
			{
				"iconPath":"static/tabbar/my.png",
				"selectedIconPath":"static/tabbar/my-select.png",
				"text":"我的",
				"pagePath":"pages/tabbar/my/my"
			}
		]
		
	}
```

#### 6. 在 pages.json 里配置全局样式
``` js
// 全局设置
	"globalStyle": {
		"navigationBarTextStyle": "black",
		"navigationBarTitleText": "微信",
		"navigationBarBackgroundColor": "#F8F8F8",
		"backgroundColor": "#F8F8F8",
		"app-plus":{
			"titleNView":false, // 取消原生导航
			"scrollIndicator":"none"  //取消原生滚动条
		}
	},
```
#### 7. 在 github.com 建立新的库房
#### 8. git remote add origin(仓库名称) git@github.com:RenPeng-CN/learngit.git  // 添加远程仓库
   git push -u XiaoMiStore master // 推向 远程仓库 github

**************************************************
### nvue 中 css 的注意事项
  1. 单位只支持 px ，不支持 em,rem,pt,%,upx,rpx
  2. 标准宽度：750px 就等于100%， 标准高度: 1250px 就等于100%
  3. <div></div>默认 flex 布局 <div style='dispaly:flex; flex-direction:column; flex-wrap:nowrap'></div> 默认不换行，不在一行
         如果要各个元素在一行上就需要写成 <div style="flex-direction:row"></div>
	     <div style="flex-direction:row; flex-wrap:wrap"></div> 在一行，满了之后可换行
  4. 不能合起来写，例如 <div style="height:300px; width:375px; border:1px solid red"><text>这样效果出不来</text></div>
       <div style="height:300px;width:375px; border-width:1px; border-style:solid; border-color:red; margin-left:100px; margin-right:100px;"><text>这样才能出来</text></div>
  5. 背景颜色只能用 background-color:xxx
  6. 选择器只支持 单类
  7. 引入要用 <style src="@/common/nvue-common.css"></style>


### nvue和vue之间的通讯(一)
   1. 在 nvue 里，使用 uni.postMessage(data) 发送数据通讯，data 为 JSON 格式。
   2. 在 App.vue 里，使用 onUniNViewMessage 进行监听。

### vue 页面和 nvue 页面通讯(二)
   1. 在 vue 里，使用 plus.webview.postMessageToUniNView(data,nvueId) 发送消息，data 为 JSON 格式，
      nvueId 为 nvue 所在 webview 的 id， webview 的 id 获取方式参考 $getAppwebview()
   2. 在 nvue 里 引用 globalEvent 模块监听 plusMessage 事件


### vue 和 nvue 共享变量和数据

     nvue 不支持 vuex
	 1. uni.storage vue 和 nvue 页面都可以使用相同的 uni.storage存储。 这个存储是持久化的。比如登录状态可以保存在这里。
	 2. globalData 小程序有 globalData机制，这套机制在 uni-app 里可以使用，全端通用。在 App.vue 文件里定义 globalData，通过 getApp().globalData 获取数据

## 引入全局公共样式
   ### 1. 引入样式库
``` javascript
uni.css      // 官方的 ui 库，在 官方的 Hello uni-app 演示框架的组件、接口、模板 里 复制 common/uni.css
animate.css  // css 动画库 [点击这里下载](https://daneden.github.io/animate.css/) 有很好的动画效果可以用 也放到 common 文件夹下
icon.css     // 图标库 在 common 文件夹下 手动创建
common.css   //公共样式  在 common 文件夹下 手动创建

最后再 App.vue 文件里引入以上四个文件

```