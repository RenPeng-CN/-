## 快速初始化
我们推荐直接使用脚手架，只需几条简单指令，即可快速生成项目（npm >=6.1.0）:
``` js
// 创建项目文件夹 并 进入该文件夹
mkdir stock-practice-api && cd stock-practice-api

// npm 切换到淘宝镜像，因为 EggJS就是 阿里开发的，如果用 npm 原定的镜像下载，速度会非常慢，而且会报错
npm config set registry https://registry.npm.taobao.org

// 如果上面这行命令出错，就用这行命令初始化 egg，临时使用淘宝镜像
npm init egg --type=simple --registry https://registry.npm.taobao.org

// 如果上一条命令可用，就不用执行此行命令了。 初始化 EggJS
npm init egg --type=simple  // --type=simple 意思是 项目的模板用的是 simple（简单模板），项目初始化的时候使用，

// 安装
npm install  // 或者 npm i ，安装 egg 需要的相关依赖包

// 运行 egg
npm run dev // 开发环境使用
npm run start // 生产项目里使用


// 关闭 csrf 开启跨域
npm i egg-cors --save

// 安装 mongodb 数据库
npm i egg-mongodb -S

// 安装 mongoose
npm i egg-mongoose -S

// 安装模板引擎
npm i egg-view-ejs -S 
```

安装egg.js

```
全局切换镜像： 
npm config set registry https://registry.npm.taobao.org
```



我们推荐直接使用脚手架，只需几条简单指令，即可快速生成项目（`npm >=6.1.0`）:

```
mkdir egg-example && cd egg-example
npm init egg --type=simple --registry https://registry.npm.taobao.org
npm i
```

启动项目:

```
npm run dev
open http://localhost:7001
```

# 写第一个api接口

## 安装vscode扩展
## 创建控制器

```js
async index() {
    const { ctx } = this;
    // 获取路由get传值参数（路由:id）
    ctx.params;
    // 获取url的问号get传值参数
    ctx.query;
    // 响应
    ctx.body = '响应';
    // 状态码
	ctx.status = 201;
}
```



## 编写路由

### 基础用法

```js
// router.js
router.get('/admin/:id', controller.admin.index);

// controller
async index() {
    const { ctx } = this;
    // 获取路由get传值参数（路由:id）
    ctx.params;
    // 获取url的问号get传值参数
    ctx.query;
}
```

### 资源路由

```js
// app/router.js
module.exports = app => {
  const { router, controller } = app;
  router.resources('posts', '/api/posts', controller.posts);
  // app/controller/v1/users.js
  router.resources('users', '/api/v1/users', controller.v1.users); 
};
```

上面代码就在 `/posts` 路径上部署了一组 CRUD 路径结构，对应的 Controller 为 `app/controller/posts.js` 接下来， 你只需要在 `posts.js` 里面实现对应的函数就可以了。

| Method | Path            | Route Name | Controller.Action             |
| ------ | --------------- | ---------- | ----------------------------- |
| GET    | /posts          | posts      | app.controllers.posts.index   |
| GET    | /posts/new      | new_post   | app.controllers.posts.new     |
| GET    | /posts/:id      | post       | app.controllers.posts.show    |
| GET    | /posts/:id/edit | edit_post  | app.controllers.posts.edit    |
| POST   | /posts          | posts      | app.controllers.posts.create  |
| PUT    | /posts/:id      | post       | app.controllers.posts.update  |
| DELETE | /posts/:id      | post       | app.controllers.posts.destroy |

```js
// app/controller/posts.js

// 列表页
exports.index = async () => {};
// 新增表单页
exports.new = async () => {};
// 新增逻辑
exports.create = async () => {};
// 详情页
exports.show = async () => {};
// 编辑表单页
exports.edit = async () => {};
// 更新逻辑
exports.update = async () => {};
// 删除逻辑
exports.destroy = async () => {};
```

### 路由分组

```js
// app/router.js
module.exports = app => {
  require('./router/news')(app);
  require('./router/admin')(app);
};

// app/router/news.js
module.exports = app => {
  app.router.get('/news/list', app.controller.news.list);
  app.router.get('/news/detail', app.controller.news.detail);
};

// app/router/admin.js
module.exports = app => {
  app.router.get('/admin/user', app.controller.admin.user);
  app.router.get('/admin/log', app.controller.admin.log);
};
```



# 关闭csrf开启跨域

文档：https://www.npmjs.com/package/egg-cors

- 安装  npm i egg-cors --save
- 配置插件

```js
// {app_root}/config/plugin.js
exports.cors = {
  enable: true,
  package: 'egg-cors',
};
```

- config / config.default.js 目录下配置

```js
  config.security = {
    // 关闭 csrf
    csrf: {
      enable: false,
    },
     // 跨域白名单
    domainWhiteList: [ 'http://localhost:3000' ],
  };
  // 允许跨域的方法
  config.cors = {
    origin: '*',
    allowMethods: 'GET, PUT, POST, DELETE, PATCH'
  };
```

# 数据库

## 配置和创建迁移文件

### 配置



1. 安装并配置[egg-sequelize](https://github.com/eggjs/egg-sequelize)插件（它会辅助我们将定义好的 Model 对象加载到 app 和 ctx 上）和[mysql2](https://github.com/sidorares/node-mysql2)模块：

```js
npm install --save egg-sequelize mysql2
```

2. 在`config/plugin.js`中引入 egg-sequelize 插件

```
exports.sequelize = {
  enable: true,
  package: 'egg-sequelize',
};
```

3. 在`config/config.default.js`

```js
config.sequelize = {
    dialect:  'mysql',
    host:  '127.0.0.1',
    username: 'root',
    password:  'root',
    port:  3306,
    database:  'eggapi',
    // 中国时区
    timezone:  '+08:00',
    define: {
        // 取消数据表名复数
        freezeTableName: true,
        // 自动写入时间戳 created_at updated_at
        timestamps: true,
        // 字段生成软删除时间戳 deleted_at
        paranoid: true,
        createdAt: 'created_at',
        updatedAt: 'updated_at',
        deletedAt: 'deleted_at',
        // 所有驼峰命名格式化
        underscored: true
    }
};
```

4. sequelize 提供了[sequelize-cli](https://github.com/sequelize/cli)工具来实现[Migrations](http://docs.sequelizejs.com/manual/tutorial/migrations.html)，我们也可以在 egg 项目中引入 sequelize-cli。

```js
npm install --save-dev sequelize-cli
```

5.  egg 项目中，我们希望将所有数据库 Migrations 相关的内容都放在`database`目录下，所以我们在项目根目录下新建一个`.sequelizerc`配置文件：

```js
'use strict';

const path = require('path');

module.exports = {
  config: path.join(__dirname, 'database/config.json'),
  'migrations-path': path.join(__dirname, 'database/migrations'),
  'seeders-path': path.join(__dirname, 'database/seeders'),
  'models-path': path.join(__dirname, 'app/model'),
};
```

6. 初始化 Migrations 配置文件和目录

```js
npx sequelize init:config
npx sequelize init:migrations
// npx sequelize init:models
```

7. 行完后会生成`database/config.json`文件和`database/migrations`目录，我们修改一下`database/config.json`中的内容，将其改成我们项目中使用的数据库配置：

```json
{
  "development": {
    "username": "root",
    "password": null,
    "database": "eggapi",
    "host": "127.0.0.1",
    "dialect": "mysql",
    "timezone": "+08:00"
  }
}
```

8. 创建数据库

```js
npx sequelize db:create
```

### 创建数据迁移表

```js
npx sequelize migration:generate --name=init-user
```

1.执行完命令后，会在database / migrations / 目录下生成数据表迁移文件，然后定义

```js
'use strict';

module.exports = {
    up: async (queryInterface, Sequelize) => {
        const { INTEGER, STRING, DATE, ENUM } = Sequelize;
        // 创建表
        await queryInterface.createTable('user', {
            id: { type: INTEGER(20).UNSIGNED, primaryKey: true, autoIncrement: true },
            username: { type: STRING(30), allowNull: false, defaultValue: '', comment: '用户名称', unique: true},
            password: { type: STRING(200), allowNull: false, defaultValue: '' },
            avatar_url: { type: STRING(200), allowNull: true, defaultValue: '' },
            sex: { type: ENUM, values: ['男','女','保密'], allowNull: false, defaultValue: '男', comment: '用户性别'},
            created_at: DATE,
            updated_at: DATE
        });
    },

    down: async queryInterface => {
        await queryInterface.dropTable('user')
    }
};
```

- 执行 migrate 进行数据库变更

```php
# 升级数据库
npx sequelize db:migrate
# 如果有问题需要回滚，可以通过 `npx sequelize db:migrate:undo` 回退一个变更
# npx sequelize db:migrate:undo
# 可以通过 `db:migrate:undo:all` 回退到初始状态
# npx sequelize db:migrate:undo:all
```



# 模型

### 创建模型
模型是用来操作数据库表的，进行 增删改查 用的
```js
// app / model / user.js

'use strict';
module.exports = app => {
  const { STRING, INTEGER, DATE, ENUM } = app.Sequelize;
  // 配置（重要：一定要配置详细，一定要！！！）
  const User = app.model.define('user', {
    id: { type: INTEGER(20).UNSIGNED, primaryKey: true, autoIncrement: true },
      username: { type: STRING(30), allowNull: false, defaultValue: '', comment: '用户名称', unique: true},
      password: { type: STRING(200), allowNull: false, defaultValue: '' },
      avatar_url: { type: STRING(200), allowNull: true, defaultValue: '' },
      sex: { type: ENUM, values: ['男','女','保密'], allowNull: true, defaultValue: '男', comment: '用户性别'},
      created_at: DATE,
      updated_at: DATE
  },{
    timestamps: true, // 是否自动写入时间戳
    tableName: 'user', // 自定义数据表名称
 });

  return User;
};
```

这个 Model 就可以在 Controller 和 Service 中通过 `app.model.User` 或者 `ctx.model.User` 访问到了，例如我们编写 `app/controller/user.js`：

```js
// app/controller/user.js
const Controller = require('egg').Controller;

function toInt(str) {
  if (typeof str === 'number') return str;
  if (!str) return str;
  return parseInt(str, 10) || 0;
}

class UserController extends Controller {
  async index() {
    const ctx = this.ctx;
    let keyword = this.ctx.params.keyword;
    let Op = this.app.Sequelize.Op;
    ctx.body = await ctx.model.User.findAll({ 
        where:{
            id:{
                [Op.gt]:6
            },
            username: {
              	[Op.like]:'%'+keyword+'%'
            }
        },
        attributes:['id','username','sex'],
        order:[
            ['id','DESC']
        ],
        limit: toInt(ctx.query.limit), 
        offset: toInt(ctx.query.offset) 
    });
  }

  async show() {
    let id = parseInt(this.ctx.params.id);
      // 通过主键查询单个数据
      // let detail = await this.app.model.User.findByPk(id);
      // if (!detail) {
      //     return this.ctx.body = {
      //         msg: "fail",
      //         data: "用户不存在"
      //     }
      // }

      // 查询单个
      let detail = await this.app.model.User.findOne({
          where: {
              id,
              sex: "女"
          }
      });

      this.ctx.body = {
          msg: 'ok',
          data: detail
      };
  }

  async create() {
    const ctx = this.ctx;
    const { name, age } = ctx.request.body;
    // 创建单条记录
    const user = await ctx.model.User.create({ name, age });
    // 批量创建
    await ctx.model.User.bulkCreate([{
        name: "第一个",
        age: 15
    },
    {
        name: "第二个",
        age: 15
    },
    {
        name: "第三个",
        age: 15
    }]);
    ctx.status = 201;
    ctx.body = user;
  }

  async update() {
    const ctx = this.ctx;
    const id = toInt(ctx.params.id);
    const user = await ctx.model.User.findByPk(id);
    if (!user) {
      ctx.status = 404;
      return;
    }

    const { name, age } = ctx.request.body;
    await user.update({ name, age });
    ctx.body = user;
  }

  async destroy() {
    const ctx = this.ctx;
    const id = toInt(ctx.params.id);
    const user = await ctx.model.User.findByPk(id);
    if (!user) {
      ctx.status = 404;
      return;
    }

    await user.destroy();
    ctx.status = 200;
  }
}

module.exports = UserController;
```

最后我们将这个 controller 挂载到路由上：

```js
// app/router.js
module.exports = app => {
  const { router, controller } = app;
  router.resources('user', '/user', controller.user);
};
```

针对 `users` 表的 CURD 操作的接口就开发完了







# 错误和异常处理

```js
// app/middleware/error_handler.js
module.exports = (option, app) => {
    return async function errorHandler(ctx, next) {
      try {
        await next();
      } catch (err) {
        // 所有的异常都在 app 上触发一个 error 事件，框架会记录一条错误日志
        ctx.app.emit('error', err, ctx);
  
        const status = err.status || 500;
        // 生产环境时 500 错误的详细错误内容不返回给客户端，因为可能包含敏感信息
        const error = status === 500 && ctx.app.config.env === 'prod'
          ? 'Internal Server Error'
          : err.message;
  
        // 从 error 对象上读出各个属性，设置到响应中
        ctx.body = { error };
        if (status === 422) {
          ctx.body.detail = err.errors;
        }
        ctx.status = status;
      }
    };
  };
```



# 中间件

```js
config.middleware = ['errorHandler'];

config.errorHandler = {
    ceshi: 123,
    // 通用配置（以下是重点）
    enable:true, // 控制中间件是否开启。
    match:'/news', // 设置只有符合某些规则的请求才会经过这个中间件（匹配路由）
    ignore:'/shop' // 设置符合某些规则的请求不经过这个中间件。

    /**
        注意：
        1. match 和 ignore 不允许同时配置
        2. 例如：match:'/news'，只要包含/news的任何页面都生效
        **/

    // match 和 ignore 支持多种类型的配置方式：字符串、正则、函数（推荐）
    match(ctx) {
        // 只有 ios 设备才开启
        const reg = /iphone|ipad|ipod/i;
        return reg.test(ctx.get('user-agent'));
    },
};
```



# 参数验证

https://www.npmjs.com/package/egg-valparams

```
npm i egg-valparams --save
```

```js
// config/plugin.js
valparams : {
  enable : true,
  package: 'egg-valparams'
},
// config/config.default.js
config.valparams = {
    locale    : 'zh-cn',
    throwError: false
};
```

```js
class XXXController extends app.Controller {
  // ...
  async XXX() {
    const {ctx} = this;
    ctx.validate({
      system  : {type: 'string', required: false, defValue: 'account', desc: '系统名称'},
      token   : {type: 'string', required: true, desc: 'token 验证'},
      redirect: {type: 'string', required: false, desc: '登录跳转'}
    });
    // if (config.throwError === false)
    if(ctx.paramErrors) {
      // get error infos from `ctx.paramErrors`;
    }
    let params = ctx.params;
    let {query, body} = ctx.request;
    // ctx.params        = validater.ret.params;
    // ctx.request.query = validater.ret.query;
    // ctx.request.body  = validater.ret.body;
    // ...
    ctx.body = query;
  }
  // ...
}
```

Egg中使用egg-mongoose和常用的Mongoose 方法
Mongoose
Mongoose就是一套操作MongoDB数据库的接口，而Egg中有对应的插件egg-mongoose。

安装
$ npm install egg-mongoose --save
配置
改变Egg项目中的配置文件{workplace}/config/plugin.js中来启用 egg-mongoose 插件:

exports.mongoose = {
  enable: true,
  package: 'egg-mongoose',
};
Egg连接mongoose
在Egg项目中的配置文件{workplace}/config/default.js配置项config添加属性

config.mongoose = {
    url: process.env.EGG_MONGODB_URL || 'mongodb://127.0.0.1/website',
    options: {
      server: {
        poolSize: 40,
      },
    },
  };
定义数据表
在{workplace}/app/model/article.js定义数据表

'use strict';
module.exports = app => {
  const mongoose = app.mongoose;
  const Schema = mongoose.Schema;
  const PostSchema = new Schema({
    wid: {
      type: String,
    },
    release: {
      type: Boolean,
    },
    sort: {
      type: Number,
    },
    img: {
      type: String,
    },
    abstract: {
      type: String,
    },
    text: {
      type: String,
    },
    isSetTop: {
      type: Number,
    },
    title: {
      type: String,
    },
    keywords: {
      type: String,
    },
    describe: {
      type: String,
    },
    updateTime: {
      type: Date,
    },
    num: {
      type: Number,
    },
    uid: {
      type: String,
    },
    editors: {
      type: String,
    },
    disable: {
      type: Boolean,
    },
    columnId: {
      type: Schema.Types.ObjectId,
    },
  });
  return mongoose.model('Article', PostSchema);
};

备注：其中type表示字段类型，Mongoose 有以下几种类型Number（数字），String（字符串），Boolean（布尔值），ObjectId（对象ID），Array（数组），Object（对象），Date（日期）。。。

常用的Mongoose 方法
一，增加数据
this.ctx.model.Article.create(post,callback);
备注：其中post为json数据结构，callback为操作后的回调函数

二，查询数据
1，获取所有数据，返回是一个数组
this.ctx.model.Article.find()
2，获取一个数据，返回是一个对象
this.ctx.model.Article.findOne()
3，条件查询
this.ctx.model.Article.find(conditions,callback);
condition有以下几种类型

1），根据具体数据进行查询
this.ctx.model.Article.find({_id：5c4a819fb87ba4002a47bc4f,title:"123"},callback);
返回_id为5c4a819fb87ba4002a47bc4f，title为123的结果
2），条件查询
"$lt"	小于
"$lte"	小于等于
"$gt"	大于
"$gte"	大于等于
"$ne"	不等于
this.ctx.model.Article.find({“sort”:{ $get:18 , $lte:30 });
返回Article表中sort 大于等于18并小于等于30的结果
3），或查询 OR
"$in" 一个键对应多个值
"$nin" 同上取反, 一个键不对应指定值
"$or" 多个条件匹配, 可以嵌套 $in 使用
"$not"	同上取反, 查询与特定模式不匹配的文档
this.ctx.model.Article.find({"title":{ $in:[20,21,22."haha"]} );
返回Article表中title等于20或21或21或"haha"的结果
this.ctx.model.Article.find({"$or" :  [ {"age":18} , {"name":"wxw"} ] });
返回Article表中age等于18或 name等于"wxw"的结果
4），类型查询（"$exists"条件判定）
this.ctx.model.Article.find({name: {$exists: true}},function(error,docs){
  //返回Article表中所有存在name属性的结果
});
this.ctx.model.Article.find({telephone: {$exists: false}},function(error,docs){
  //返回Article表中所有不存在telephone属性的结果
});
5），匹配正则表达式查询
MongoDb 是使用 Prel兼容的正则表达式库来匹配正则表达式

this.ctx.model.Article.find( {"name" : /joe/i } );
返回Article表中name为 joe 的结果, 并忽略大小写
6），查询数组
this.ctx.model.Article.find({"array":10} );
返回Article表中array(数组类型)键中有10的文档, array : [1,2,3,4,5,10] 会匹配到
this.ctx.model.Article.find({"array[5]":10}  );
返回Article表中array(数组类型)键中下标5对应的值是10, array : [1,2,3,4,5,10] 会匹配到
this.ctx.model.Article.find({"array":[5,10]});
返回Article表中查询匹配array数组中既有5又有10的结果
this.ctx.model.Article.find({"array":{$size : 3} });
返回Article表中查询匹配array数组长度为3 的的结果
this.ctx.model.Article.find({"array":{$slice : 10} });
返回Article表中查询匹配array数组的前10个元素
this.ctx.model.Article.find({"array":{$slice :  [5,10]} });
返回Article表中查询匹配array数组的第5个到第10个元素
7)，where
用它可以执行任意javacript语句作为查询的一部分,如果回调函数返回 true 文档就作为结果的一部分返回

this.ctx.model.Article.find( {"$where" :  "this.x + this.y === 10" } );
this.ctx.model.Article.find( {"$where" : " function(){ return this.x + this.y ===10; } " } )
其中this为数据表中的数据，上述返回Article表中属性x+属性y=10的所有数据
三，删除数据
this.ctx.model.Article.remove(conditions,callback);
备注：conditions为查询条件，与查询数据介绍的一样，eg：{ _id：5c4a819fb87ba4002a47bc4f }，找到_id为5c4a819fb87ba4002a47bc4f的数据，callback为操作成功后的回调函数

四，更新数据
this.ctx.model.Article.update(conditions, update, callback)
参数1:查询条件, 参数2:更新对象,可以使用MondoDB的更新修改器
备注：conditions与查询数据中介绍的一样

1，update为更新对象
let post = {
    wid: '5c492c57acbe363fd4824446',
    column: [ '新闻' ],
    titleHead: '',
    img: '',
    isAbstract: 'false',
}
this.ctx.model.Article.update({ _id: '5c4a819fb87ba4002a47bc4f ' }, post)
查询Article表中特定_id，并对post中所包含的属性进行更新。
2，update使用MondoDB的更新修改器，有以下几种使用场景
1），"$inc"增减修改器,只对数字有效
this.ctx.model.Article.update({"age":22}, {$inc:{"age":1} }  );
找到age=22的文档,修改文档的age值自增1
2），'$set' 指定一个键的值,这个键不存在就创建它.可以是任何MondoDB支持的类型.
this.ctx.model.Article.update({ _id：5c4a819fb87ba4002a47bc4f }, { $set: { isDelete: true } });
对5c4a819fb87ba4002a47bc4f 表进行软删除，找到特定_id数据，增加或者修改isDelete属性
3），"$unset"同上取反,删除一个键
this.ctx.model.Article.update({age:22}, {$unset:{age:18} } );
执行后age键不存在
4），'$push'给一个键push一个数组成员,键不存在会创建,对数组有效
this.ctx.model.Article.update({name:'wxw'}, {$push:{array:10} } );
返回Article表中name为wxw的数据，增加一个array键,类型为数组,有一个成员 10
5），'$addToSet'向数组中添加一个元素,如果存在就不添加
this.ctx.model.Article.update({name:'wxw'},{$addToSet:{array:10} } );
返回Article表中name为wxw的数据，array中有10所以不会添加
6），'$each'遍历数组和 $push 修改器配合可以插入多个值
this.ctx.model.Article.update({name:'wxw'}, {$push:{array:{$each: [1,2,3,4,5]}} } );
返回Article表中name为wxw的数据，执行后array : [10,1,2,3,4,5]
7），'$pop' 向数组中尾部删除一个元素
this.ctx.model.Article.update({name:'wxw'}, {$pop:{array:1} } );
返回Article表中name为wxw的数据，其中array : [10,1,2,3,4,5]，执行后 array : [10,1,2,3,4]
tip:将1改成-1可以删除数组首部元素
8），'$pull' 向数组中删除指定元素
this.ctx.model.Article.update({name:'wxw'}, {$pull:{array:10} });
返回Article表中name为wxw的数据，匹配到array中的10后将其删除。
五，排序（sort）
this.ctx.model.Article.sort({ isSetTop: -1, sort: 1, editTime: -1 });
对Article表中的数据进行排序，先按“isSetTop”降序，再按“sort”升序，最后按“editTime”降序
备注：键对应数据中的键名，值代表排序方向，1 升序, -1降序。

六，限制返回结果的数量（limit）
this.ctx.model.Article.limit(3);
对Article表中的数据进行返回，返回为前面3条数据
七，跳过前3个文档,返回其余的（skip）
this.ctx.model.Article.skip(3);
对Article表中的数据进行返回，跳过前面3条数据，返回其余数据
附：综合使用最后三个方法进行分页查询

this.ctx.model.Article.find({ _id：5c4a819fb87ba4002a47bc4f }).skip(pageSize * (pageNum - 1)).limit(parseInt(pageSize)).sort({ isSetTop: -1, sort: 1, editTime: -1 });
其中pageSize和pageNum为动态传递数据，返回Article表中特定_id在每页数据为pageSize条件下的第pageNum页中的数据，并按照“isSetTop”降序，再按“sort”升序，最后按“editTime”降序进行排序。



