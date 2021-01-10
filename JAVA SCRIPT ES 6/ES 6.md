#[ECMAScript 官网](http://www.ecma-international.org/ecma-262/6.0/)



###[ECMAScript 6 入门](http://es6.ruanyifeng.com/) 



ES6 模板字符串

```js
var name = '张三';
var age = 20;
console.log(`${name}的年龄是${age}`); // 注意 `` 不是 单引号哦，而是 ESC 按钮下方的按钮
```



属性的简写

```js
var name = '张三';
var app = {
	name
}
console.log(app.name);
```



方法的简写

```js
var name = '张三';
var app = {
	name,
	run() {
		console.log(`${this.name}在跑步`);
	}
}
app.run();
```



箭头函数

```js
以前的写法
setTimeout(function(){
	console.log('以前的写法');
},1000)



现在的写法
setTimeout(()=> {
	console.log('箭头函数');
},1000)
```





回调函数

```js
function getData(callback){
	setTimeout(function(){
		var name = '回调函数';
		callback(name);
	}, 1000);
}

// 外部获取异步方法里面的数据
getData((data)=>{
	console.log(data);
});
```



Promise 处理异步函数的第一种方法

```js
// resolve 是成功的回调函数
// reject 是失败的回调函数
var p = new Promise(function(resolve, reject){
	setTimeout(function(){
		var name = 'Promise 解决回调函数';
		if(Math.random()<0.7){
			resolve(name);
		}else{
			reject('失败了');
		}		
	}, 1000)
});

p.then((data)=> {
	console.log(data);
})
```



Promise 处理异步函数的第一种方法

```js
function getData(resolve, reject){
	setTimeout(function(){
		var name = '张三';
		resolve(name);
	}, 1000)
}

var p = new Promise(getData);

p.then((data)=> {
	console.log(data);
})
```



Async 可以将一个普通的方法，变成一个 Promise 的异步方法

```js
以下是一个普通方法
function getData(){
	return '这是一个数据';
}
console.log(getData());
// 打印显示结果： 这是一个数据





在 函数前加了一个  async ，将该函数变成了 Promise 异步函数
async function getData(){
	return '这是一个数据';
}
console.log(getData())

// 打印显示结果： Promise { '这是一个数据' }





Promise 函数取值的方法
async function getData(){
	return '这是一个数据';
}

var p = getData();
p.then((data)=> {
	console.log(data);
});
// 打印显示结果： 这是一个数据
```



await 用于等待一个异步方法执行完成, 可以获取异步方法里的值，但是必须用在一个异步方法里

```js
async function getData(){
	return '这是一个 await 异步 调用异步数据函数的数据';
}

async function test(){
	var d = await getData(); // await 可以把一个异步函数变成同步函数
	console.log(d);
}

test();
```



返回 Promise 的方法，也可以使用 await 获取数据

```js
function getData() {
	return new Promise( (resolve, reject)=> {
		setTimeout( ()=> {
			var username = '张三';
			resolve(username);
		})
	}, 1000);
}

async function test(){
	var data = await getData();
	console.log(data);
}

test();
```

