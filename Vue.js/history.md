history 简单实例

history.html 客户端

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		显示区域
		<div id="app"></div>
		<!-- 类似 router.link -->
		<a href="javascript:void(0);" onclick="goHistory('/user')">去看User</a>
		<a href="javascript:void(0);" onclick="goHistory('/goods')">去看User</a>
		
		<script type="text/javascript">
			function goHistory(url){
				let text = '';
				
				// 判断用户点击的是哪个按钮
				switch(url) {
					case '/user':
					text = '用户页面';
						alert('/user');
						break;
					case '/goods':
					text = '商品页面';
						alert('/goods');
				}
				// 判断之后,让页面的地址栏做一个改变
				// {} 描述当前跳转的一些信息,
				// '' 所谓的 title,用不着
				// url 真正需要的传参
				history.pushState({},'',url);
				
				// 改变页面效果
				document.getElementById('app').innerText = text;
			}
			
			// 当页面加载的时候
			
			// 获取当前 path 路劲
			window.onload = function() {
				let text2 = '';
				var path = location.pathname;
				switch(path) {
					case '/user':
					text2 = '用户页面';						
						break;
					case '/goods':
					text2 = '商品页面';
						break;
				}
				document.getElementById('app').innerText = text2;
			}
		</script>		
	</body>
</html>
```



node 服务器端, history.js   在终端   node ./history.js 启动node服务器，浏览器地址输入： localhost:8888 就可以浏览了。 

```js
const http = require('http');
const fs = require('fs');

http.createServer((req, res)=>{
	fs.readFile('./history.html', (err, data)=> {
		res.end(data);
	})
})
.listen(8888);
```

