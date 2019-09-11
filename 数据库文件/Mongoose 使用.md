# Mongoose

### Mongoose 序列自增长插件



1. 安装插件

```js
npm install --save mongoose-sequence // 安装插件，可以上 github 或 npmjs.com 去了解此插件
```



2. 设置, 在设置 某一集合 的时候引入

   ```js
   const mongoose = require('mongoose')
   const AutoIncrement = require('mongoose-sequence')(mongoose);
   ```



3. 使用

   ```js
   // 设置集合时的举例说明
   UserSchema = mongoose.Schema({
       name: String
   });
   // 你没有必要在 参数里加上  id 字段，插件会自动增加 id 字段，并每次调用时都自动增加一位数
   UserSchema.plugin(AutoIncrement, {inc_field: 'id'});
   mongoose.model('User', UserSchema);
   ```






## mongoose  入门以及 **mongoose** 实现数据 的增、删、改、查 



主讲教师:(大地) 

合作网站:**www.itying.com** (IT 营) 我的专栏:https://www.itying.com/category-79-b0.html 

目录
 一、mongoose 介绍..................................................................................................................................1 

二、 mongoose 的安装以及使用............................................................................................................2 

1. 安装.............................................................................................................................................. 2 
2. 引入 mongoose 并连接数据库............................................................................................... 2 
3. 定义 Schema............................................................................................................................. 2 
4. 创建数据模型........................................................................................................................... 2 
5. 查找数据................................................................................................................................... 3 
6. 增加数据................................................................................................................................... 3 
7. 修改数据................................................................................................................................... 3 
8. 删除数据................................................................................................................................... 3 
9. 保存成功查找........................................................................................................................... 4 

三、 mongoose 模块化............................................................................................................................4 





### 一、mongoose 介绍 

Mongoose 是在 node.js 异步环境下对 mongodb 进行便捷操作的对象模型工具。Mongoose 是 NodeJS 的驱动，不能作为其他语言的驱动。 

**Mongoose** 有两个特点 1、通过关系型数据库的思想来设计非关系型数据库 2、基于 mongodb 驱动，简化操作 



### 二、mongoose 的安装以及使用 

官网:https://mongoosejs.com/
 **1.** 安装

```js
npm i mongoose --save
```



 **2**、引入 **mongoose** 并连接数据库 

```js
// 1. 引入 mongoose 模块
const mongoose = require('mongoose'); 

 // 2. 通过 mongoose 连接 mongo 数据库
mongoose.connect('mongodb://127.0.0.1:27017/news',{useNewUrlParser:true},(err) => {
	if(err){
		console.log('连接数据库失败');
		console.log(err);
		return;
	}
	console.log('连接数据库成功')
});

// 如果有账户密码需要采用下面的连接方式mongoose.connect('mongodb://eggadmin:123456@localhost:27017/eggcms');
```



3、定义 **Schema** 

数据库中的 Schema，为数据库对象的集合。schema 是 mongoose 里会用到的一种数据模式， 可以理解为表结构的定义;每个 schema 会映射到 mongodb 中的一个 collection，它不具备 操作数据库的能力 

```js
var UserSchema = mongoose.Schema({
	name:String,
	age:Number,
	sex:String,
  status:{
		type:Number, // 指定类型
		default:1 // 指定默认值，每次增加数据时，会自动增加此字段
	}
})
```



4、创建数据模型, 以便进行操作数据库
 定义好了 Schema，接下就是生成 Model。model 是由 schema 生成的模型，可以对数据库的操作。 

注意:mongoose.model 里面可以传入两个参数也可以传入三个参数 

mongoose.model(参数 1:模型名称(首字母大写)，参数 2:Schema) 

mongoose.model(参数 1:模型名称(首字母大写)，参数 2:Schema，参数 3:数据库集合名 称) 

如果传入 **2** 个参数的话**:**这个模型会和模型名称相同的复数的数据库建立连接:如通过下面 方法创建模型，那么这个模型将会操作 users 这个集合。 

```js
// model 里面的第一个参数，要注意： 1 首字母要大写。 2. 要和数据库表(集合)名称对应。
//这个模型 model 会和模型名称相同的复数的数据库表建立连接 例如：Schema 叫 User . 数据库表必须叫 users
// model 里面的第二个参数，将定义好的 Schema 放进去
var User = mongoose.model('User',UserSchema);
```



如果传入 **3** 个参数的话**:**模型默认操作第三个参数定义的集合名称 

```js
// model 里放入第三个参数，意思是 指定要关联的 数据库中的表（集合）
var User = mongoose.model('User',UserSchema,'user');
```



5、查找数据 

```js
// 5. 通过 Schema 操作对应的数据库中的表(集合)
 User.find({},(err,result) => {
 	if(err){
 		console.log(err);
 		return;
 	}
 	console.log(result);
 })
```



6、增加数据 

```js
// 6. 增加数据 1.实例化 Model  2. 实例 .save()
 var u = new User({
 	name : '张张',
 	age : 20,
 	sex : '男'
 });

 u.save(function(err){
 	if(err){
 		console.log(err);
 		return;
 	}
 	console.log('增加成功');
 })
```



7、修改数据 

```js
 // 7.更新数据
 User.updateOne({'_id':'5cac01ccedffc4192e3827dd'},{'name':'新张三'},function(err,doc){
 	if(err){
 		console.log(err);
 		return;
 	}
 	console.log(doc);
 	console.log('修改成功')
 })
```



**8**、删除数据 

```js
User.deleteOne(
	{'_id':'5cac01ccedffc4192e3827dd'},
	(err,result) => {
		if(err){
			console.log(err);
			return;
		}
	console.log(result);
	console.log('删除成功')
	})
```



**9**、保存成功查找 

```js
var u=new User({ name:'lisi2222333', 

age:20, 

status:true //类型转换 }) 

u.save(function(err,docs){ if(err){ 

console.log(err); 

return; } 

//console.log(docs); User.find({},function(err,docs){ 

if(err){ console.log(err); 

return; } 

console.log(docs); }) 

}); 
```



三、**mongoose** 模块化 看教程演示 

1. 先设置连接数据库的模块

   ```js
   // mongooseDB.js 页面
   // 通过 mongoose 连接 mongoDB 数据库
   
   // 1. 引入 mongoose 模块
   const mongoose = require('mongoose'); 
   
    // 2. 通过 mongoose 连接 mongo 数据库
   mongoose.connect('mongodb://127.0.0.1:27017/vue_koa_demo', {useNewUrlParser:true},(err) => {
   	if(err){
   		console.log('连接数据库失败');
   		console.log(err);
   		return;
   	}
   	console.log('连接数据库成功');
   });
   
   module.exports = mongoose;
   ```

   

   

2. 设置 一个 表(集合)的 mongoose 模块映射

   ```js
   // 通过 mongoose 操控 mongo 数据库中的 lesson 表(集合)
   
   // 1. 引入数据库连接页面，与 mongo 数据库建立连接
   var mongoose = require('./mongooseDB.js');
   
   // 2. 建立 Schema ，与 lesson 表里的参数进行映射
   var LessonSchema = mongoose.Schema({
   	id:Number,
   	timestamp:Date,
   	author:String,
   	reviewer:String,
   	title:String,
   	content_short:String,
   	forecast:Number,
   	importance:Number,
   	type:String,
   	status:String,
   	display_time:Date,
   	comment_disabled:Boolean,
   	pageviews:Number,
   	image_url:String,
   	platforms:String,
   	pid:String
   })
   
   // 3. 建立 module 模型
   var LessonModel = mongoose.model('Lesson',LessonSchema,'lesson');
   
   module.exports = LessonModel;
   ```

3. 具体操作lesson表的页面

   ```js
   // 引入 数据库 lesson 集合的 mongoose 模块
   var LessonModel = require('./lesson.js');
   
   LessonModel.find(json1,{sessionCode:attr},function(err,doc){
   				if(err){
   					reject(err)
   					console.log(err);
   					return;
   				}
   				console.log(doc);
   				resolve(doc);
   })
   ```

   

### mongoose 预定义模式修饰符 

lowercase、uppercase 、trim 

mongoose 提供的预定义模式修饰符，可以对我们增加的数据进行一些格式化。 

```js
var UserSchema=mongoose.Schema({ name:{ 

type:String, 

trim:true }, 

age:Number, status:{ 

type:Number, 

default:1 } 

}) 
```



二、**Mongoose Getters** 与 **Setters** 自定义修饰符 

除了 mongoose 内置的修饰符以外，我们还可以通过 set(建议使用) 修饰符在增加数据的 时候对数据进行格式化。也可以通过 get(不建议使用)在实例获取数据的时候对数据进行 

格式化。 

```js
var NewsSchema=mongoose.Schema({ title:"string", 

author:String, pic:String, redirect:{ 

type:String, set(url){ 

if(!url) return url; if(url.indexOf('http://')!=0 

url = 'http://' + url; } 

return url; } 

}, content:String, status:{ 

type:Number, default:1 

} }) 

&& url.indexOf('https://')!=0){ 

var NewsSchema=mongoose.Schema({ title:"string", 

author:String, pic:String, redirect:{ 

type:String, set(url){ 

if(!url) return url; if(url.indexOf('http://')!=0 

url = 'http://' + url; 

&& url.indexOf('https://')!=0){ 

} 

return url; }, 

get: function(url){ if(!url) return url; 

if(url.indexOf('http://')!=0 url = 'http://' + url; 

} 

return url; } 

}, content:String, status:{ 

type:Number, default:1 

} }) 

&& url.indexOf('https://')!=0){ 
```



### 一、Mongoose 索引 

索引是对数据库表中一列或多列的值进行排序的一种结构，可以让我们查询数据库变得更 快。MongoDB 的索引几乎与传统的关系型数据库一模一样，这其中也包括一些基本的查询 优化技巧。 

mongoose 中除了以前创建索引的方式，我们也可以在定义 Schema 的时候指定创建索引。 

```js
var DeviceSchema = new mongoose.Schema({ sn: { 

type: Number, // 唯一索引 unique: true 

}, name: { 

type: String, // 普通索引 index: true 

} }); 
```



二、**Mongoose** 内置 **CURD** https://mongoosejs.com/docs/queries.html 

```js
Model.deleteMany()
Model.deleteOne()
Model.find()
Model.findById()
Model.findByIdAndDelete()
Model.findByIdAndRemove()
Model.findByIdAndUpdate()
Model.findOne()
Model.findOneAndDelete()
Model.findOneAndRemove()
Model.findOneAndUpdate()
Model.replaceOne()
Model.updateMany()
Model.updateOne()
```



三、扩展 **Mongoose CURD** 方法 

```js
var mongoose=require('./db.js');
 var UserSchema=mongoose.Schema({ 

name:{ type:String 

}, age:Number, 

status:{ type:Number, 

default:1 

} }) 

// 静态方法 

 UserSchema.statics.findByUid=function(uid,cb){ 

this.find({"_id":uid},function(err,docs){ cb(err,docs) 

}) } 


// 实例方法 

UserSchema.methods.print = function(){ 

console.log('这是一个实例方法'); 

console.log(this); }; 

module.exports=mongoose.model('User',UserSchema,'user'); 
```



### 一、Mongoose 校验参数 

**required** : 表示这个数据必须传入
**max**: 用于 Number 类型数据，最大值
**min**: 用于 Number 类型数据，最小值 

**enum**:枚举类型，要求数据必须满足枚举值 enum: ['0', '1', '2'] 

**match**:增加的数据必须符合 match(正则)的规则 

**maxlength**:最大值
**minlength**:最小值 



```js
var UserSchema = new mongoose.Schema({ name:{ 

type:String, 

required: true, 

}, 
```



```js
age: {
 type: Number,
 // 是否必须的校验器 required: true,
 // 数字类型的最大值校验器 max: 120,
 // 数字类型的最小值校验器 min: 0 

}, 
  
status: { 
type: String,
 // 设置字符串的可选值 enum: ['0', '1', '2'] 
}, 
  
phone:{ 
type:Number, 
match: /^\d{11}$/ 
}, 

desc: {
 type: String, 
 maxlength:20, 
 minlength:10 

} }); 
```



#### 二、Mongoose 自定义的验证器 

在缺省情况下创建的索引均不是唯一索引。下面的示例将创建唯一索引，如: 

```js
var UserSchema = new mongoose.Schema({ 
 name:{ 
     type:String, 
      required: true, 
 }, 
  
  age: { 
    type: Number,
      // 是否必须的校验器 required: true,
      // 数字类型的最大值校验器 
     max: 120,
     // 数字类型的最小值校验器 min: 0 
   }, 
  
  
  status: { 
      type: String,
      // 设置字符串的可选值 enum: ['0', '1', '2'] 
   }, 
  
  
  phone:{ 
     type:Number, 
     match: /^\d{11}$/ 
  }, 

  desc: {
     type: String, 
// 自定义的验证器，如果通过验证返回 true，没有通过则返回 false 
   
    validate: function(desc) { 
           return desc.length >= 10; 
     } 

} }); 
```









