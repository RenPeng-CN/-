# koa2从搭建项目到实现API

koa2，以前没有接触过，只知道是express的原班人马开发的，在一些方面优于express，又经历了一次从koa到koa2的升级，应该说是比较成熟的了。
根据现在的技术实现方案，现在大部分的web服务基本都是前后端分离模式的，所以koa2的让web应用开发和api的使用更加简便优势，更值得我们去学习。

> 本文只简单介绍将环境搭建起来，可以让初次学习koa或者web应用的同学能够更快的看到学习成果，增加学习的自信心，其中涉及到的很多的知识和技术点，我们后续补充，不在本文做过多的阐述
> 本文的目的就是做知识的普及，文中可能会有些代码是从网络中复制的，但是都是亲手调试可运行，保证代码的正常运行的

### 搭建环境

1. 安装node
   这个不用想也不用考虑，koa项目是基于node的，上来什么都别说，先安装node。但这里有个细节需要注意，node需要最低版本为7.6.0，主要是为了一些es6的语法支持。

```js
brew install node
```

> 我这里是在mac环境上安装的node，由于本人习惯使用homebrew安装，如果不是mac环境的同学，也可以直接到官网下载安装包安装。这个步骤比较简单，一般情况下看下官网的文档介绍，可以直接搞定。
> node安装完成后，要查看下node的版本，有两个目的，一是查看node是否安装成功，再就是检查是否满足koa的版本需求。

```js
node -v
npm -v
```

> npm集成到node中了，只要成功的安装了node，npm也会被成功安装。

1. 安装koa
   我们学习和应用的是koa项目，就先安装上koa吧。
   npm install koa --save
2. 安装koa2项目生成器并创建项目

```js
npm install koa-generator -g
koa2 bbs
```

这里的koa-generator，并不是官方的项目生成器，而是狼叔-桑世龙为我们贡献的财富，在这里，我们感谢狼叔。
koa2 bbs，这行代码创建了基于koa2的项目，并生成基本的项目架构。基本的项目架构如下：

![image-20190323072940376](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/image-20190323072940376.png)

koa2项目基本架构图

1. 安装依赖
   项目也创建完了，但是只是完成了一个基本的骨架，还没有血肉，接下来就需要安装项目的依赖了

```js
cd bbs
npm install
```

这个步骤可能消耗的时间有点长，这个和网络环境有关，由于网络的原因，还有可能某些依赖会安装失败，这个时候，我们可以通过修改npm镜像来减少由于网络环境导致安装依赖失败的问题。
具体可参考[淘宝NPM镜像](http://npm.taobao.org/)。

1. 启动服务

```js
npm start
```

npm可以启动服务，但不支持热更新，我们这里只是测试项目能否正常启动，执行，热更新的内容，我们后续再讲。
服务正常启动之后，浏览器输入：localhost:3000,如果页面中展示了如"Hello Koa 2"的内容，说明项目搭建成功。

### 搭建项目

这里的搭建项目，不是前面的使用koa2初始化一个项目，而是把前面通过koa2初始化的项目补充营养，让它丰满起来。

1. 安装sequelize
   暂且不要关心sequelize是什么，以后会有专题细讲，只需要了解一点就可以了：Sequelize是一个基于promise的nodejs ORM，目前支持Postgres、mysql、SQLite和Microsoft SQL Server。它具有强大的事务支持，关联关系，读取和复制等功能。

```js
npm install sequelize --save
```

1. 安装mysql、mysql2
   项目需要mysql的数据库支持

```js
npm install mysql mysql2 --save
```

1. 配置Sequelize的数据库链接
   在项目的根目录下创建一个config目录，config目录中创建db.js，该文件主要用来创建mysql的数据库链接的。

```js
const Sequelize = require('sequelize');
const sequelize = new Sequelize('dbname','dbusername','password',{
    host:'localhost',
    dialect:'mysql',
    operatorsAliases:false,
    dialectOptions:{
        //字符集
        charset:'utf8mb4',
        collate:'utf8mb4_unicode_ci',
        supportBigNumbers: true,
        bigNumberStrings: true
    },
    pool:{
        max: 5,
        min: 0,
        acquire: 30000,
        idle: 10000
    },
    timezone: '+08:00'  //东八时区
});

module.exports = {
    sequelize
};
```

这些代码可以直接使用，只需要将代码中实例化Sequelie对象语句中的dbname更改为你的数据库名，dbusername更改为你的数据库用户名，passoword更改为你的数据库密码，其中数据库名和数据库用户名不能为空，密码可以为空，为空时则为空的字符串就可以了。

```js
const sequelize = new Sequelize('bbs','root','',{
……
```

1. 创建schema、modules、controllers
   schema:数据表模型实例
   modules：实体模型
   controllers：控制器

   

   3个目录下分别创建article.js。

   

   ![image-20190323073111432](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/image-20190323073111432.png)

   目录结构

2. schema数据表模型
   在schema目录下新建一个article.js文件，该文件的主要作用就是建立与数据表的对应关系，也可以理解为代码的建表。
   首先来分析表结构：

| 字段     | 说明             | 是否必填     |
| -------- | ---------------- | ------------ |
| id       | 文章自增ID，主键 | 否，自动填的 |
| title    | 文章标题         | 是           |
| author   | 作者             | 是           |
| content  | 文章内容         | 是           |
| category | 文章分类         | 是           |

分析表结构，主要是为了用来建表的，表结构，是我们根据实体的关系抽象出来的实体关系，根据这些关系建立表，将实体的关系存储到表中。建表有两种方式：一是使用mysql数据库的命令行工具或者UI工具建表，再就是使用我们前面介绍的Sequelize，让程序去创建表。
使用mysql工具建表方式：

```js
DROP TABLE IF EXISTS `article`;
CREATE TABLE `article` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(255) NOT NULL,
  `author` varchar(255) NOT NULL,
  `content` varchar(255) NOT NULL,
  `category` varchar(255) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8;
SET FOREIGN_KEY_CHECKS = 1;
```

这是使用sql语句创建表的方式，我们看另一种借助Sequelize建表方式(Sequelize不光是能建表，还能做数据库的管理)；
我们刚才在schema目录下的article.js用来创建数据表模型的，也可以理解为创建一张数据表，代码如下：

```js
const moment = require("moment");
module.exports = function(sequelize,DataTypes){
    return sequelize.define('article',{
        id:{
            type: DataTypes.INTEGER,
            primaryKey: true,
            allowNull: true,
            autoIncrement: true
        },
        //文章标题
        title:{
            type: DataTypes.STRING,
            allowNull: false,
            field: 'title'
        },
        //作者
        author:{
            type: DataTypes.STRING,
            allowNull: false,
            field: 'author'
        },
        //内容
        content:{
            type: DataTypes.STRING,
            allowNull: false,
            field:'content'
        },
        //文章分类
        category:{
            type: DataTypes.STRING,
            allowNull: false,
            field: 'category'
        },
        // 创建时间
        createdAt:{
            type: DataTypes.DATE
        },
        // 更新时间
        updatedAt:{
            type: DataTypes.DATE
            }
        }
    },{
        /**
         * 如果为true，则表示名称和model相同，即user
         * 如果为fasle，mysql创建的表名称会是复数，即users
         * 如果指定的表名称本身就是复数，则形式不变
         */
        freezeTableName: true
    });
}
```

1. 模型应用、使用
   在项目中modules目录下创建article.js文件，为文章表，该文件为文章的实例。

```js
// 引入mysql的配置文件
const db = require('../config/db');

// 引入sequelize对象
const Sequelize = db.sequelize;

// 引入数据表模型
const Article = Sequelize.import('../schema/article');
Article.sync({force: false}); //自动创建表

class ArticleModel {
    /**
     * 创建文章模型
     * @param data
     * @returns {Promise<*>}
     */
    static async createArticle(data){
        return await Article.create({
            title: data.title, //标题
            author: data.author,  //作者
            content: data.content,  //文章内容
            category: data.category //文章分类
        });
    }

    /**
     * 查询文章的详情
     * @param id 文章ID
     * @returns {Promise<Model>}
     */
    static async getArticleDetail(id){
        return await Article.findOne({
            where:{
                id
            }
        });
    }
}

module.exports = ArticleModel;
```

1. controller 控制器
   控制器的主要作用为功能的处理，项目中controller目录下创建article.js，代码如下：

```js
const ArticleModel = require("../modules/article");

class articleController {
    /**
     * 创建文章
     * @param ctx
     * @returns {Promise.<void>}
     */
    static async create(ctx){
        //接收客服端
        let req = ctx.request.body;
        if(req.title && req.author && req.content && req.category){
            try{
                //创建文章模型
                const ret = await ArticleModel.createArticle(req);
                //使用刚刚创建的文章ID查询文章详情，且返回文章详情信息
                const data = await ArticleModel.getArticleDetail(ret.id);

                ctx.response.status = 200;
                ctx.body = {
                    code: 200,
                    msg: '创建文章成功',
                    data
                }
            }catch(err){
                ctx.response.status = 412;
                ctx.body = {
                    code: 412,
                    msg: '创建文章失败',
                    data: err
                }
            }
        }else {
            ctx.response.status = 416;
            ctx.body = {
                code: 200,
                msg: '参数不齐全'
            }
        }
    }

    /**
     * 获取文章详情
     * @param ctx
     * @returns {Promise.<void>}
     */
    static async detail(ctx){
        let id = ctx.params.id;
        if(id){
            try{
                // 查询文章详情模型
                let data = await ArticleModel.getArticleDetail(id);
                ctx.response.status = 200;
                ctx.body = {
                    code: 200,
                    msg: '查询成功',
                    data
                }
            }catch(err){
                ctx.response.status = 412;
                ctx.body = {
                    code: 412,
                    msg: '查询失败',
                    data
                }
            }
        }else {
            ctx.response.status = 416;
            ctx.body = {
                code: 416,
                msg: '文章ID必须传'
            }
        }
    }
}

module.exports = articleController;
```

1. 路由
   路由，也可以简单理解为路径，主要是作为请求的url，请求的路径来处理一些请求，返回数据。一般情况下，基于node的项目，路由都是在一个叫做routes的目录下面。

```js
const Router = require('koa-router');
const ArtileController = require('../controllers/article');

const router = new Router({
  prefix: '/api/v1'
});

/**
 * 文章接口
 */
//创建文章
router.post('/article/create',ArtileController.create);

//获取文章详情
router.get('/article/:id',ArtileController.detail)

module.exports = router
```

1. 启动服务
   这里主要测试服务能否正常启动，能否正常运行。

```js
npm start
```

如果启动过程中出现下面的结果，说明服务启动成功

```js
➜  bbs npm start

> bbs@0.1.0 start /usr/local/var/www/koa/bbs
> node bin/www

koa deprecated Support for generators will be removed in v3. See the documentation for examples of how to convert old middleware https://github.com/koajs/koa/blob/master/docs/migration.md app.js:20:5
Ignoring invalid configuration option passed to Connection: collate. This is currently a warning, but in future versions of MySQL2, an error will be thrown if you pass an invalid configuration options to a Connection
Ignoring invalid configuration option passed to Connection: collate. This is currently a warning, but in future versions of MySQL2, an error will be thrown if you pass an invalid configuration options to a Connection
Executing (default): CREATE TABLE IF NOT EXISTS `article` (`id` INTEGER auto_increment , `title` VARCHAR(255) NOT NULL, `author` VARCHAR(255) NOT NULL, `content` VARCHAR(255) NOT NULL, `category` VARCHAR(255) NOT NULL, `createdAt` DATETIME, `updatedAt` DATETIME, PRIMARY KEY (`id`)) ENGINE=InnoDB;
Executing (default): SHOW INDEX FROM `article`
```

接下来，就可以测试接口了。
我使用的测试接口工具为postman，以前postman好像是作为浏览器的一个插件存在的，现在不作为浏览器的插件了，直接做成了应用了，可以直接从postmna官网下载。 官网地址<https://www.getpostman.com/>,然后我们根据我们的系统平台来选择合适的版本下载就可以了。

1. 解决跨域
   跨域是web开发中不可避免的一个必须要解决的问题了。跨域问题，主要是要解决服务器端的通信问题。在node的开发中，只需要实现一个CORS标准就可以了。

```js
npm install koa-cors --save
```

然后在根目录下的app.js加入koa-cors的引用：

```js
const cors = require('koa-cors')
app.use(cors()) //使用cors
```

然后重新启动服务。到这里为止，还不能支持热更新，现在主要测试功能，暂且先这么手动重启吧，后面会有专题描述热更新服务。

1. 测试接口
   前面我下载并且安装好了postman，工具有了，代码层面也准备就绪，解决了跨域问题，下面就可以测试接口是否可用了。
   测试接口，我们先看下我们的路由管理文件routes目录下的index.js文件。

```js
//创建文章
router.post('/article/create',ArtileController.create);

//获取文章详情
router.get('/article/:id',ArtileController.detail)
```

这里有2个文章相关的接口，一个是创建新文章，一个是获取文章详情。我们先来测试创建新的文章。
看路由，创建新文章需要的请求方式为post，url为/article/create，那我们就根据这些信息使用postman进行测试吧。

![image-20190323073305473](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/image-20190323073305473.png)

创建新文章

这是我创建新文章的接口测试，然后看测试结果：

![image-20190323073340292](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/image-20190323073340292.png)

接口测试成功

运行结果告诉我们创建新文章的接口已经测试成功了，说明这个接口可以走通了。如果我们现在是前后端分离的开发模式，我们需要提供api的话，那么这个api就可以提供给前端同学使用了。

我们也可以从数据库中验证一下执行结果的正确性：

![image-20190323073409098](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/image-20190323073409098.png)

数据添加成功

数据也成功插入到数据库中了，我们也更加验证了我们接口的正确性和可执行性了，这个接口到此结束。
然后看下一个获取文章详情的接口。需要使用get的请求方式，请求的URL为/article/:id。

![image-20190323073431843](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/image-20190323073431843.png)

获取文章详情

![image-20190323073459913](/Users/renpeng/Library/Mobile Documents/com~apple~CloudDocs/编程笔记/img/image-20190323073459913.png)

成功获取文章详情

### 总结

到现在为止，我们基本已经完成了从0开始搭建一个koa项目，并完成一些简单的api接口的实现，当然了，只是最基本的实现，里面还有很多可优化的空间，对于一个作为入门案例来说，已经够了。对于有深入了解或学习的朋友，希望我们功能学习，共同提高。