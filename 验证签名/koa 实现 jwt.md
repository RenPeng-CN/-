## 什么是JWT？

> 这篇文章描述的很详细了，大家可以看下 [什么是JWT](https://www.jianshu.com/p/576dbf44b2ae)

我在简单说下：

1. json web token 简称JWT，是基于JSON的一种开放标准。服务器和客户端可以通过JWT来建立一座通信的桥梁。
2. JWT主要分为三部分。header(头部)，payload(载体)， signature(签名)。
    header: 头部
    声明加密方式和声明类型
    payload: 载体
    存放JWT定义的一些声明（例如：过期时间，签发者等等），我们将用户的一些信息存放在这里（例如：用户名，用户ID等，千万不要存在用户密码等敏感信息）
    signature: 签名

```js
  signature = [header(加密方式)](base64编码(header) + '.' + base64编码(payload), [服务器的私钥])
```

最后将上面三个组成部分用'.'连接起来就构成了JWT：

```js
  JWT = base64编码(header) + '.' + base64编码(payload) + '.' + signature
```

## 实现jwt认证

1. 下载koa及相关模块

```js
npm i koa koa-bodyparser koa-router
```

1. 下载node-jsonwebtoken

```js
npm i jsonwebtoken  // 一个实现jwt的包
```

1. 下载koa/jwt

```js
npm i koa-jwt  //验证jwt的koa中间价
```

1. 实现：
    app.js

```js
const Koa = require('koa')
const Router = require('koa-router')
const bodyParser = require('koa-bodyparser')
const jwt = require('jsonwebtoken')
const jwtKoa = require('koa-jwt')
const util = require('util')
const verify = util.promisify(jwt.verify) // 解密
const secret = 'jwt demo'
const app = new Koa()
const router = new Router()
app.use(bodyParser())
app
    .use(jwtKoa({secret}).unless({
        path: [/^\/api\/login/] //数组中的路径不需要通过jwt验证
    }))
router
    .post('/api/login', async (ctx, next) => {
        const user = ctx.request.body
        if(user && user.name) {
            let userToken = {
                name: user.name
            }
            const token = jwt.sign(userToken, secret, {expiresIn: '1h'})  //token签名 有效期为1小时
            ctx.body = {
                message: '获取token成功',
                code: 1,
                token
            }
        } else {
            ctx.body = {
                message: '参数错误',
                code: -1
            }
        }
    })
    .get('/api/userInfo', async (ctx) => {
        const token = ctx.header.authorization  // 获取jwt
        let payload
        if (token) {
            payload = await verify(token.split(' ')[1], secret)  // // 解密，获取payload
            ctx.body = {
                payload
            }
        } else {
            ctx.body = {
                message: 'token 错误',
                code: -1
            }
        }
    })
app
    .use(router.routes())
    .use(router.allowedMethods())
app.listen(3000, () => {
    console.log('app listening 3000...')
})
```

5.启动node

```js
node app.js
```

6.通过curl模拟请求
 访问 [http://127.0.0.1:3000/api/userInfo](https://link.jianshu.com?t=http://127.0.0.1:3000/api/userInfo)

```js
curl http://127.0.0.1:3000/api/userInfo
```

返回如下内容：

```js
Authentication Erro  //验证错误，说明我们的jwt已经生效
```

访问 [http://127.0.0.1:3000/api/login](https://link.jianshu.com?t=http://127.0.0.1:3000/api/login) 带上参数name

```js
curl -d "name=tiptoe" http://127.0.0.1:3000/api/login
```

结果如下：

```js
{"message":"获取token成功","code":1,"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoidGlwdG9lIiwiaWF0IjoxNDk2Mzg4NzgwLCJleHAiOjE0OTYzOTIzODB9.N2e-84Pmf466DQJ2x3ldd1AWC1IL97ZRWwiDR-Oebhs"}
```

然后在header中加入token，在访问[http://127.0.0.1:3000/api/userInfo](https://link.jianshu.com?t=http://127.0.0.1:3000/api/userInfo)

```js
// Authorization: Bearer  授权：令牌类型为Bearer
curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoidGlwdG9lIiwiaWF0IjoxNDk2Mzg4NzgwLCJleHAiOjE0OTYzOTIzODB9.N2e-84Pmf466DQJ2x3ldd1AWC1IL97ZRWwiDR-Oebhs" http://127.0.0.1:3000/api/userInfo
```

返回如下内容，说明认证已经通过：

```js
{"payload":{"name":"tiptoe","iat":1496398614,"exp":1496402214}}
```

1. 介绍一个jwt解密的网站，

   jwt.io

   。我们可以在这个网站输入假面的token和私钥，然后可以查看解密的JWT

   

   ![img](https:////upload-images.jianshu.io/upload_images/6283108-4e891d27ffe7bcd6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

   jwt.io

