### **Koa Cms 增加数据前的准备工作 koa-multer 实现图片上传** 

 

一、 Koa 上传文件模块 koa-multer 介绍............................................................................. 1 

二、 Koa 上传文件模块 koa-multer 使用............................................................................. 1 



主讲教师:(大地) 

合作网站: http://www.itying.com/ 中文文档: http://www.itying.com/koa 



#### 一、 **Koa** 上传文件模块 **koa-multer** 介绍

 koa-multer 是一个 node.js 中间件，用于处理 multipart/form-data 类型的表单数据，它主要 

用于上传文件。 

注意**:** Multer 不会处理任何非 multipart/form-data 类型的表单数据，意思就是我们要上传图 片必须在 form 表单上面加 multipart/form-data 

**koa-multer** 基于 **multer** 这个模块:https://github.com/expressjs/multer 参考文档:https://github.com/expressjs/multer/blob/master/doc/README-zh-cn.md 



#### 二、 **Koa** 上传文件模块 **koa-multer** 使用 

**1.** 安装 **Koa2** 的 **koa-multer** : npm install --save koa-multer 

**2.** 引入配置**koa-multer**模块: 

```js
const multer = require('koa-multer'); 
const file= require('file); 

//配置
 var storage = multer.diskStorage({ 

//文件保存路径
 destination: function (req, file, cb) { 

cb(null, 'public/uploads/') 

},
 //修改文件名称
 filename: function (req, file, cb) { 

//注意路径必须存在 

var fileFormat = (file.originalname).split("."); 

cb(null,Date.now() + "." + fileFormat[fileFormat.length - 1]); } 

}) 

//加载配置
 var upload = multer({ storage: storage }) 
```



**3.**使用 **koa-multer** 

```js
router.post('/doAdd', upload.single('face'), async (ctx, next) => { ctx.body = { 

filename: ctx.req.file.filename,//返回文件名 

body:ctx.req.body } 

}); 
```



**4.**注意**:在客户端代码****Form** 表单加上 **enctype="multipart/form-data"** 