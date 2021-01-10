## 创建项目和基础配置

#### 创建项目

安装egg.js

``` js
//全局切换镜像到淘宝镜像
npm config set registry https://registry.npm.taobao.org
```

我们推荐直接使用脚手架，只需几条简单指令，即可快速生成项目（`npm >=6.1.0`）:

``` js
mkdir egg-example && cd egg-example // 创建项目文件夹，并进入文件夹
npm init egg --type=simple --registry https://registry.npm.taobao.org   // 用淘宝镜像初始化egg
npm i // 安装必要的组件
```

启动项目:

``` js
npm run dev // 运行项目
open http://localhost:7001
```



####关闭csrf开启跨域

- 安装 跨域插件

``` js
 npm i egg-cors --save // 安装跨域插件
```



- 配置插件

``` js
// {app_root}/config/plugin.js 在这个地址，加入下面的代码
cors:{
  enable: true,
  package: 'egg-cors',
},
```

``` js
 // config / config.default.js 目录下配置一下代码
config.security = {
    // 关闭 csrf，CSRF概念：CSRF跨站点请求伪造(Cross—Site Request Forgery)，跟XSS攻击一样，存在巨大的危害性
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

## 全局异常处理

#### 全局抛出异常处理

``` js
// app/middleware/error_handler.js
module.exports = (option, app) => {
    return async function errorHandler(ctx, next) {
      try {
        await next(); 
        // 404 处理
        if(ctx.status === 404 && !ctx.body){
           ctx.body = { 
               msg:"fail",
               data:'404 错误'
           };
        }
      } catch (err) {
        // 记录一条错误日志
        app.emit('error', error, ctx);

        const status = err.status || 500;
        // 生产环境时 500 错误的详细错误内容不返回给客户端，因为可能包含敏感信息
        const error = status === 500 && app.config.env === 'prod'
          ? 'Internal Server Error'
          : err.message;

        // 从 error 对象上读出各个属性，设置到响应中
        ctx.body = { 
            msg:"fail",
            data:error
        };
        ctx.status = status;
      }
    };
  };

// config/config.default.js
config.middleware = ['errorHandler'];
```

## 框架扩展

#### 封装api返回格式扩展

``` js
// app/extend/context.js
module.exports = {
  // 成功提示
  apiSuccess(data = '', msg = 'ok', code = 200) {
    this.body = { msg, data };
    this.status = code;
  },
  // 失败提示
  apiFail(data = '', msg = 'fail', code = 400) {
    this.body = { msg, data };
    this.status = code;
  },
};
```



## sequelize数据库和迁移配置

#### 数据库配置

1. 安装并配置[egg-sequelize](https://github.com/eggjs/egg-sequelize)插件（它会辅助我们将定义好的 Model 对象加载到 app 和 ctx 上）和[mysql2](https://github.com/sidorares/node-mysql2)模块：

``` js
npm install --save egg-sequelize mysql2
```

2. 在`config/plugin.js`中引入 egg-sequelize 插件

``` js
exports.sequelize = {
  enable: true,
  package: 'egg-sequelize',
};
```

3. 在`config/config.default.js`

``` js
config.sequelize = {
    dialect:  'mysql',
    host:  '127.0.0.1',
    username: 'root',
    password:  'root',
    port:  3306,
    database:  'egg-wechat',
    // 中国时区
    timezone:  '+08:00',
    define: {
        // 取消数据表名复数
        freezeTableName: true,
        // 自动写入时间戳 created_at updated_at
        timestamps: true,
        // 字段生成软删除时间戳 deleted_at
        // paranoid: true,
        createdAt: 'created_at',
        updatedAt: 'updated_at',
        // deletedAt: 'deleted_at',
        // 所有驼峰命名格式化
        underscored: true
    }
};
```

#### 迁移配置

1. sequelize 提供了[sequelize-cli](https://github.com/sequelize/cli)工具来实现[Migrations](http://docs.sequelizejs.com/manual/tutorial/migrations.html)，我们也可以在 egg 项目中引入 sequelize-cli。

``` js
npm install --save-dev sequelize-cli
```

2. egg 项目中，我们希望将所有数据库 Migrations 相关的内容都放在`database`目录下，所以我们在项目根目录下新建一个`.sequelizerc`配置文件：

``` js
'use strict';

const path = require('path');

module.exports = {
  config: path.join(__dirname, 'database/config.json'),
  'migrations-path': path.join(__dirname, 'database/migrations'),
  'seeders-path': path.join(__dirname, 'database/seeders'),
  'models-path': path.join(__dirname, 'app/model'),
};
```

3. 初始化 Migrations 配置文件和目录

``` js
npx sequelize init:config
npx sequelize init:migrations
// npx sequelize init:models
```

4. 行完后会生成`database/config.json`文件和`database/migrations`目录，我们修改一下`database/config.json`中的内容，将其改成我们项目中使用的数据库配置：

``` js
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

5. 创建数据库

``` js
npx sequelize db:create
```

``` js
# 升级数据库
npx sequelize db:migrate
# 如果有问题需要回滚，可以通过 `db:migrate:undo` 回退一个变更
# npx sequelize db:migrate:undo
# 可以通过 `db:migrate:undo:all` 回退到初始状态
# npx sequelize db:migrate:undo:all
```



#### 模型关联

``` js
User.associate = function(models) {
   // 关联用户资料 一对一
   User.hasOne(app.model.Userinfo);
   // 反向一对一关联
   // Userinfo.belongsTo(app.model.User);
   // 一对多关联
   User.hasMany(app.model.Post);
   // 反向一对多关联
   // Post.belongsTo(app.model.User);
   // 多对多
   // User.belongsToMany(Project, { as: 'Tasks', through: 'worker_tasks', foreignKey: 'userId' })
   // 反向多对多
   // Project.belongsToMany(User, { as: 'Workers', through: 'worker_tasks', foreignKey: 'projectId' })
}
```



## 登录注册退出

#### 数据表设计和迁移

``` js
npx sequelize migration:generate --name=user // 表名 user
```

1.执行完命令后，会在database / migrations / 目录下生成数据表迁移文件，然后定义

```js
'use strict';

module.exports = {
  up: async (queryInterface, Sequelize) => {
    const { INTEGER, STRING, DATE, ENUM } = Sequelize;
    // 创建表
    await queryInterface.createTable('user', {
      id: {
        type: INTEGER(20).UNSIGNED,
        primaryKey: true,
        autoIncrement: true
      },
      username: {
        type: STRING(30),
        allowNull: false,
        defaultValue: '',
        comment: '用户名称',
        unique: true
      },
      nickname: {
        type: STRING(30),
        allowNull: false,
        defaultValue: '',
        comment: '昵称',
      },
      email: {
        type: STRING(160),
        comment: '用户邮箱',
        unique: true
      },
      password: {
        type: STRING(200),
        allowNull: false,
        defaultValue: ''
      },
      avatar: {
        type: STRING(200),
        allowNull: true,
        defaultValue: ''
      },
      phone: {
        type: STRING(20),
        comment: '用户手机',
        unique: true
      },
      sex: {
        type: ENUM,
        values: ['男', '女', '保密'],
        allowNull: true,
        defaultValue: '男',
        comment: '用户性别'
      },
      status: {
        type: INTEGER(1),
        allowNull: false,
        defaultValue: 1,
        comment: '状态'
      },
      sign: {
        type: STRING(200),
        allowNull: true,
        defaultValue: '',
        comment: '个性签名'
      },
      area: {
        type: STRING(200),
        allowNull: true,
        defaultValue: '',
        comment: '地区'
      },
      created_at: DATE,
      updated_at: DATE
    });
  },

  down: async queryInterface => {
    await queryInterface.dropTable('user');
  }
};
```

执行 migrate 进行数据库变更

``` js
npx sequelize db:migrate
```

模型创建

``` js
// app/model/user.js
'use strict';
module.exports = app => {
    const { STRING, INTEGER, DATE, ENUM, TEXT } = app.Sequelize;
    // 配置（重要：一定要配置详细，一定要！！！）
    const User = app.model.define('user', {
        id: {
            type: INTEGER(20).UNSIGNED,
            primaryKey: true,
            autoIncrement: true
        },
        username: {
            type: STRING(30),
            allowNull: false,
            defaultValue: '',
            comment: '用户名称',
            unique: true
        },
        nickname: {
            type: STRING(30),
            allowNull: false,
            defaultValue: '',
            comment: '昵称',
        },
        email: {
            type: STRING(160),
            comment: '用户邮箱',
            unique: true
        },
        password: {
            type: STRING(200),
            allowNull: false,
            defaultValue: ''
        },
        avatar: {
            type: STRING(200),
            allowNull: true,
            defaultValue: ''
        },
        phone: {
            type: STRING(20),
            comment: '用户手机',
            unique: true
        },
        sex: {
            type: ENUM,
            values: ['男', '女', '保密'],
            allowNull: true,
            defaultValue: '男',
            comment: '用户性别'
        },
        status: {
            type: INTEGER(1),
            allowNull: false,
            defaultValue: 1,
            comment: '状态'
        },
        sign: {
            type: STRING(200),
            allowNull: true,
            defaultValue: '',
            comment: '个性签名'
        },
        area: {
            type: STRING(200),
            allowNull: true,
            defaultValue: '',
            comment: '地区'
        },
        created_at: DATE,
        updated_at: DATE
    });
    return User;
};
```





#### 注册功能

#### 参数验证

插件地址：

> https://www.npmjs.com/package/egg-valparams

安装

``` js
npm i egg-valparams --save
```

配置

``` js
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

###### 在控制器里使用

``` js
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

###### ValParams API 说明

###### 参数验证处理

Valparams.setParams(req, params, options);

| Param                       | Type                                               | Description                                                  | Example                                                      |
| --------------------------- | -------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| req                         | Object                                             | request 对象,这里我们就是取相应的三种请求的参数进行参数验证  | {params, query, body}                                        |
| params                      | Object                                             | 参数的格式配置 { pname: {alias, type, required, range: {in, min, max, reg, schema }, defValue, trim, allowEmptyStr, desc[, detail] } } | {sysID : {alias:'sid',type: 'int', required: true, desc: '所属系统id'}} |
| params[pname]               | String                                             | 参数名                                                       |                                                              |
| params[pname].alias         | String                                             | 参数别名，可以使用该参数指定前端使用的参数名称               |                                                              |
| params[pname].type          | String                                             | 参数类型                                                     | 常用可选类型有 int, string, json 等，其他具体可见下文或用 Valparams.vType 进行查询 |
| params[pname].required      | Boolean                                            | 是否必须                                                     |                                                              |
| params[pname].range         | Object                                             | 参数范围控制                                                 | {min: '112.80.248.10', max: '112.80.248.72'}                 |
| params[pname].range.min     | ALL                                                | 最小值、最短、最早（不同 type 参数 含义有所差异）            |                                                              |
| params[pname].range.max     | ALL                                                | 最大值、最长、最晚（不同 type 参数 含义有所差异）            |                                                              |
| params[pname].range.in      | Array                                              | 在XX中，指定参数必须为其中的值                               |                                                              |
| params[pname].range.reg     | RegExp                                             | 正则判断，参数需要符合正则                                   |                                                              |
| params[pname].range.schema  | Object                                             | jsonSchema，针对JSON类型参数有效，使用ajv对参数进行格式控制  |                                                              |
| params[pname].defValue      | ALL                                                | 默认值，没传参数或参数验证出错时生效，此时会将该值赋值到相应参数上 |                                                              |
| params[pname].trim          | Boolean                                            | 是否去掉参数前后空格字符，默认false                          |                                                              |
| params[pname].allowEmptyStr | Boolean                                            | 是否允许接受空字符串，默认false                              |                                                              |
| params[pname].desc          | String                                             | 参数含义描述                                                 |                                                              |
| options                     | Object                                             | 参数关系配置                                                 |                                                              |
| options.choices             | Array                                              | 参数挑选规则                                                 | [{fields: ['p22', 'p23', 'p24'], count: 2, force: true}] 表示'p22', 'p23', 'p24' 参数三选二 |
| options.choices[].fields    | Array                                              | 涉及的参数                                                   |                                                              |
| options.choices[].count     | Number                                             | 需要至少传 ${count} 个                                       |                                                              |
| options.choices[].force     | Boolean                                            | 默认 false，为 true 时，涉及的参数中只能传 ${count} 个, 为 false 时，可以多于 ${count} 个 |                                                              |
| options.equals              | Array                                              | 参数相等                                                     | [['p20', 'p21'], ['p22', 'p23']] 表示 'p20', 'p21' 两个值需要相等，'p22', 'p23' 两个值需要相等 |
| options.equals[]            | Array                                              | 涉及的参数(涉及的参数的值需要是相等的)                       |                                                              |
| options.compares            | Array                                              | 参数大小关系                                                 | [['p25', 'p26', 'p27']] 表示 'p25', 'p26', 'p27' 必须符合 'p25' <= 'p26' <= 'p27' |
| options.compares[]          | Array                                              | 涉及的参数(涉及的参数的值需要是按顺序从小到大的)             |                                                              |
| options.cases               | Object                                             | 参数条件判断                                                 | [{when: ['p30'], then: ['p31'], not: ['p32']}] 表示 当传了 p30 就必须传 p31 ,同时不能传p32 |
| options.cases.when          | Array                                              | 条件                                                         |                                                              |
| options.cases.when[]        | String                                             | 涉及的参数，（字符串）只要接收到的参数有这个字段即为真       |                                                              |
| options.cases.when[].field  | 涉及的参数的名（对象）                             | ---                                                          |                                                              |
| options.cases.when[].value  | 涉及的参数的值（对象）需要参数的值与该值相等才为真 | ---                                                          |                                                              |
| options.cases.then          | Array                                              | 符合when条件时，需要必传的参数                               |                                                              |
| options.cases.not           | Array                                              | 符合when条件时，不能接收的参数                               |                                                              |

``` js
const Valparams = require('path/to/Valparams[/index]');
Valparams.locale('zh-cn');

function list(req, res, next) {
  let validater = Valparams.setParams(req, {
    sysID : {alias:'sid',type: 'int', required: true, desc: '所属系统id'},
    page  : {type: 'int', required: false, defValue: 1, range:{min:0}, desc: '页码'},
    size  : {type: 'int', required: false, defValue: 30, desc: '页面大小'},
    offset: {type: 'int', required: false, defValue: 0, desc: '位移'}
  }, {
    choices : [{fields: ['sysID', 'page'], count: 1, force: false}],
  });
  if (validater.err && validater.err.length) {
    console.log(validater.err);
  }
  else {
    console.log(validater);
    //{ query: { page: 1, size: 30 },
    //  body: {},
    //  params: { sysID: 2 },
    //  all: { sysID: 2, page: 1, size: 30 },
    //  err: null }
    //  raw: { query: { page: 1, size: 30 },
    //         body: {},
    //         params: { sid: 2 },
    //       }
    //}
    //do something
  }
}
```

###### 返回支持的类型列表

``` js
Valparams.vType = {
  ALL        : 'all',
  STRING     : 'string',
  ARRAY      : 'array',
  DATE       : 'date',
  INT        : 'int',
  FLOAT      : 'float',
  LETTER     : 'letter',
  NUMBER     : 'number',
  IP         : 'ip',
  EMAIL      : 'email',
  PHONE      : 'phone',
  URL        : 'url',
  JSON       : 'json',
  BOOL       : 'bool',
  NULL       : 'null',
  RANGE      : 'range',
  DATERANGE  : 'dateRange',
  INTRANGE   : 'intRange',
  FLOATRANGE : 'floatRange',
  NUMBERRANGE: 'numberRange'
};
```

###### 自定义本地化文件

Valparams.defineLocale(key, value);

| Param | Type   | Description                                                  | Example |
| ----- | ------ | ------------------------------------------------------------ | ------- |
| key   | String | 语言标识                                                     | zh-cn   |
| value | Object | 本地化内容，可配置内容有 em_type, em_minmax, em_reg, em_in, em_schema, em_required, em_range_relation, em_choices, em_equals, em_compares, em_cases | ---     |

###### 更新已有本地化文件内容

Valparams.updateLocale(key, value);

参数含义同 defineLocale

 获取本地化文件内容

Valparams.localeData(key);

| Param | Type   | Description | Example |
| ----- | ------ | ----------- | ------- |
| key   | String | 语言标识    | zh-cn   |

###### 列出已加载的本地化文件

Valparams.locales(key);

目前已有 en 、 zh-cn

| Param | Type   | Description | Example |
| ----- | ------ | ----------- | ------- |
| key   | String | 语言标识    | zh-cn   |

###### 设置使用的本地化文件

Valparams.locale(locale); 如： `Valparams.locale('zh-cn')`;



#### crypto 数据加密

``` js
// 1. 安装
npm install crypto --save
```

``` js
// 2.配置文件配置 config / config.default.js
config.crypto = {
    secret:  'qhdgw@45ncashdaksh2!#@3nxjdas*_672'
};
```

``` js
// 3. 使用
// 引入
const crypto = require('crypto');

// 加密
async createPassword(password) {
    const hmac = crypto.createHash("sha256", this.config.crypto.secret);
    hmac.update(password);
    return hmac.digest("hex");
}

// 验证密码
async checkPassword(password, hash_password) {
    // 先对需要验证的密码进行加密
    password = await this.createPassword(password);
    return password === hash_password;
}
```



#### jwt 加密鉴权

插件地址：

> https://www.npmjs.com/package/egg-jwt

``` js
// 安装
npm i egg-jwt --save
```

``` js
// 配置
// {app_root}/config/plugin.js
exports.jwt = {
  enable: true,
  package: "egg-jwt"
};

// {app_root}/config/config.default.js
exports.jwt = {
  secret: 'qhdgw@45ncashdaksh2!#@3nxjdas*_672'
};
```

``` js
// 生成 token
async getToken(value) {
    return this.app.jwt.sign(value, this.config.jwt.secret);
}
```

``` js
// 验证token
try {
    user = app.jwt.verify(token, app.config.jwt.secret)
} catch (err) {
    let fail = err.name === 'TokenExpiredError' ? 'token 已过期! 请重新获取令牌' : 'Token 令牌不合法!';
    return ctx.apiFail(fail);
}
```



#### redis 缓存插件和封装

``` js
// 安装
npm i egg-redis --save
```

``` js
// 配置
// config/plugin.js
exports.redis = {
  enable: true,
  package: 'egg-redis',
};

// redis存储
config.redis = {
    client: {
        port: 6379,          // Redis port
        host: '127.0.0.1',   // Redis host
        password: '',
        db: 2,
    },
}
```

``` js
// 缓存库封装
// app/service/cache.js
'use strict';

const Service = require('egg').Service;

class CacheService extends Service {
    /**
     * 获取列表
     * @param {string} key 键
     * @param {boolean} isChildObject 元素是否为对象
     * @return { array } 返回数组
     */
    async getList(key, isChildObject = false) {
        const { redis } = this.app
        let data = await redis.lrange(key, 0, -1)
        if (isChildObject) {
            data = data.map(item => {
                return JSON.parse(item);
            });
        }
        return data;
    }
    /**
     * 设置列表
     * @param {string} key 键
     * @param {object|string} value 值
     * @param {string} type 类型：push和unshift
     * @param {Number} expir 过期时间 单位秒
     * @return { Number } 返回索引
     */
    async setList(key, value, type = 'push', expir = 0) {
        const { redis } = this.app
        if (expir > 0) {
            await redis.expire(key, expir);
        }
        if (typeof value === 'object') {
            value = JSON.stringify(value);
        }
        if (type === 'push') {
            return await redis.rpush(key, value);
        }
        return await redis.lpush(key, value);
    }

    /**
     * 设置 redis 缓存
     * @param { String } key 键
     * @param {String | Object | array} value 值
     * @param { Number } expir 过期时间 单位秒
     * @return { String } 返回成功字符串OK
     */
    async set(key, value, expir = 0) {
        const { redis } = this.app
        if (expir === 0) {
            return await redis.set(key, JSON.stringify(value));
        } else {
            return await redis.set(key, JSON.stringify(value), 'EX', expir);
        }
    }

    /**
     * 获取 redis 缓存
     * @param { String } key 键
     * @return { String | array | Object } 返回获取的数据
     */
    async get(key) {
        const { redis } = this.app
        const result = await redis.get(key)
        return JSON.parse(result)
    }

    /**
     * redis 自增
     * @param { String } key 键
     * @param { Number } value 自增的值 
     * @return { Number } 返回递增值
     */
    async incr(key, number = 1) {
        const { redis } = this.app
        if (number === 1) {
            return await redis.incr(key)
        } else {
            return await redis.incrby(key, number)
        }
    }

    /**
     * 查询长度
     * @param { String } key
     * @return { Number } 返回数据长度
     */
    async strlen(key) {
        const { redis } = this.app
        return await redis.strlen(key)
    }

    /**
     * 删除指定key
     * @param {String} key 
     */
    async remove(key) {
        const { redis } = this.app
        return await redis.del(key)
    }

    /**
     * 清空缓存
     */
    async clear() {
        return await this.app.redis.flushall()
    }
}

module.exports = CacheService;
```

``` js
// 缓存使用
// 控制器
await this.service.cache.set('key', 'value');
```



#### 登录功能实现

####全局权限验证中间件实现

``` js
module.exports = (option, app) => {
    return async (ctx, next) => {
        //1. 获取 header 头token
        const { token } = ctx.header;
        if (!token) {
            ctx.throw(400, '您没有权限访问该接口!');
        }
        //2. 根据token解密，换取用户信息
        let user = {};
        try {
            user = ctx.checkToken(token);
        } catch (error) {
            let fail = error.name === 'TokenExpiredError' ? 'token 已过期! 请重新获取令牌' : 'Token 令牌不合法!';
            ctx.throw(400, fail);
        }
        //3. 判断当前用户是否登录
        let t = await ctx.service.cache.get('user_' + user.id);
        if (!t || t !== token) {
            ctx.throw(400, 'Token 令牌不合法!');
        }

        //4. 获取当前用户，验证当前用户是否被禁用
        user = await app.model.User.findByPk(user.id);
        if (!user || user.status == 0) {
            ctx.throw(400,'用户不存在或已被禁用');
        }
        // 5. 把 user 信息挂载到全局ctx上
        ctx.authUser = user;

        await next();
    }
}
```



#### 退出登录功能

