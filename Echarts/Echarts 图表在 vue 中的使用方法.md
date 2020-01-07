### uni-app中使用Echarts的实践总结
[说明文档的连接](https://blog.csdn.net/CherryLee_1210/article/details/83016706)
[另一个说明文档](https://www.jianshu.com/p/5537e5053dc1)
1. 首先在uni-app中不支持包下载所以得自己先新建一个项目，然后进入到这个目录下，执行 npm init，接下来一路回车即可。

#### 第一步、打开cmd命令窗口 安装echarts依赖
``` js
npm install echarts -S // 安装全部 echats 图片库

npm install echarts mpvue-echarts --save // 安装 echats  的 vue 的插件
```
2. 进入 node_modules 目录，里面的三个目录：echarts、mpvue-echats 、zrender 就是我们需要的第三方库。

3. 把这三个库copy到自己项目的根目录下。

4. 接下来要做的事儿就是导入库。在自己需要图表显示的组件中导入所需要的库。
``` js
import * as echarts from 'echarts'
import mpvueEcharts from 'mpvue-echarts'
```
5. 导入库之后就要使用了
我在同一个页面中使用了两次mpvueEcharts组件，这里需要注意的是，使用多次就要给每一个mpvueEcharts组件指定一个特有的canvasId。（必须要的，否则不好使，我在这个地方踩坑了）
``` vue
// 视图层
<!-- 外层预留的图表容器 -->
<view class="box1">
	<!-- 引入的mpvue-echarts组件 -->
	<mpvue-echarts canvasId="chat1" :echarts="echarts" :onInit="fn1OnInit" />
</view>

<view class="box2">
	<mpvue-echarts canvasId="chat2" :echarts="echarts" :onInit="fn2OnInit" />
</view>

```

``` js
// 业务层
//导入库
import * as echarts from 'echarts'
import mpvueEcharts from 'mpvue-echarts'

//命名一个方法名称，方便自己识别，也方便多个图表引用时易区别。
//切记这方法不能写到export default中。
function fn1(canvas, width, height) {
	const chart = echarts.init(canvas, null, {
		width: width,
		height: height
	})
	canvas.setChart(chart);

	var option = {
		...一些表格配置（参考echarts官方文档根据自己需求配置即可）
	}

	chart.setOption(option)
	return chart
};


function fn2(canvas, width, height) {
	const chart = echarts.init(canvas, null, {
		width: width,
		height: height
	})
	canvas.setChart(chart);

	var option = {
		...一些表格配置（参考echarts官方文档根据自己需求配置即可）
	}

	chart.setOption(option)
	return chart
};

//这是vue的导出对象
export default {
	data() {
		return {
			//初始化图表
			echarts,
			//图表数据绑定（我们定义的两个方法绑定）
			fn1OnInit: fn1,
			fn2OnInit: fn2
		}
	},
	//导入mpvue的mpvueEcharts组件。
	components: {
		mpvueEcharts
	}
}
```