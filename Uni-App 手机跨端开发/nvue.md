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
