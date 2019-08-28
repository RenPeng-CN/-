# [认识 JWT](https://www.cnblogs.com/cjsblog/p/9277677.html)

## 1. JSON Web Token是什么

JSON Web Token (JWT)是一个开放标准(RFC 7519)，它定义了一种紧凑的、自包含的方式，用于作为JSON对象在各方之间安全地传输信息。该信息可以被验证和信任，因为它是数字签名的。

## 2. 什么时候你应该用JSON Web Tokens

下列场景中使用JSON Web Token是很有用的：

- **Authorization** (授权) : 这是使用JWT的最常见场景。一旦用户登录，后续每个请求都将包含JWT，允许用户访问该令牌允许的路由、服务和资源。单点登录是现在广泛使用的JWT的一个特性，因为它的开销很小，并且可以轻松地跨域使用。
- **Information Exchange** (信息交换) : 对于安全的在各方之间传输信息而言，JSON Web Tokens无疑是一种很好的方式。因为JWTs可以被签名，例如，用公钥/私钥对，你可以确定发送人就是它们所说的那个人。另外，由于签名是使用头和有效负载计算的，您还可以验证内容没有被篡改。

## 3. JSON Web Token的结构是什么样的

![img](https://images2018.cnblogs.com/blog/874963/201807/874963-20180709124807031-664967381.png)

JSON Web Token由三部分组成，它们之间用圆点(.)连接。这三部分分别是：

- Header
- Payload
- Signature

因此，一个典型的JWT看起来是这个样子的：

xxxxx.yyyyy.zzzzz

接下来，具体看一下每一部分：

### Header

header典型的由两部分组成：token的类型（“JWT”）和算法名称（比如：HMAC SHA256或者RSA等等）。

例如：

![img](https://images2018.cnblogs.com/blog/874963/201807/874963-20180707143936465-1142974441.png)

然后，用Base64对这个JSON编码就得到JWT的第一部分

 

### Payload

JWT的第二部分是payload，它包含声明（要求）。声明是关于实体(通常是用户)和其他数据的声明。声明有三种类型: registered, public 和 private。

- Registered claims : 这里有一组预定义的声明，它们不是强制的，但是推荐。比如：iss (issuer), exp (expiration time), sub (subject), aud (audience)等。
- Public claims : 可以随意定义。
- Private claims : 用于在同意使用它们的各方之间共享信息，并且不是注册的或公开的声明。

下面是一个例子：

![img](https://images2018.cnblogs.com/blog/874963/201807/874963-20180707144153274-292205768.png)

对payload进行Base64编码就得到JWT的第二部分

注意，不要在JWT的payload或header中放置敏感信息，除非它们是加密的。

###  

### Signature

为了得到签名部分，你必须有编码过的header、编码过的payload、一个秘钥，签名算法是header中指定的那个，然对它们签名即可。

例如：

HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)

签名是用于验证消息在传递过程中有没有被更改，并且，对于使用私钥签名的token，它还可以验证JWT的发送方是否为它所称的发送方。

 

看一张官网的图就明白了：

![img](https://images2018.cnblogs.com/blog/874963/201807/874963-20180707150229764-2037235703.png)

## 4. JSON Web Tokens是如何工作的

在认证的时候，当用户用他们的凭证成功登录以后，一个JSON Web Token将会被返回。此后，token就是用户凭证了，你必须非常小心以防止出现安全问题。一般而言，你保存令牌的时候不应该超过你所需要它的时间。

无论何时用户想要访问受保护的路由或者资源的时候，用户代理（通常是浏览器）都应该带上JWT，典型的，通常放在Authorization header中，用Bearer schema。

header应该看起来是这样的：

Authorization: Bearer <token>

服务器上的受保护的路由将会检查Authorization header中的JWT是否有效，如果有效，则用户可以访问受保护的资源。如果JWT包含足够多的必需的数据，那么就可以减少对某些操作的数据库查询的需要，尽管可能并不总是如此。

如果token是在授权头（Authorization header）中发送的，那么跨源资源共享(CORS)将不会成为问题，因为它不使用cookie。

下面这张图显示了如何获取JWT以及使用它来访问APIs或者资源：

![img](https://images2018.cnblogs.com/blog/874963/201807/874963-20180707144719025-1833412608.png)

1. 应用（或者客户端）想授权服务器请求授权。例如，如果用授权码流程的话，就是/oauth/authorize
2. 当授权被许可以后，授权服务器返回一个access token给应用
3. 应用使用access token访问受保护的资源（比如：API）

## 5. 基于Token的身份认证 与 基于服务器的身份认证

### 5.1. 基于服务器的身份认证

在讨论基于Token的身份认证是如何工作的以及它的好处之前，我们先来看一下以前我们是怎么做的：

> HTTP协议是无状态的，也就是说，如果我们已经认证了一个用户，那么他下一次请求的时候，服务器不知道我是谁，我们必须再次认证

传统的做法是将已经认证过的用户信息存储在服务器上，比如Session。用户下次请求的时候带着Session ID，然后服务器以此检查用户是否认证过。

这种基于服务器的身份认证方式存在一些问题：

- Sessions : 每次用户认证通过以后，服务器需要创建一条记录保存用户信息，通常是在内存中，随着认证通过的用户越来越多，服务器的在这里的开销就会越来越大。
- Scalability : 由于Session是在内存中的，这就带来一些扩展性的问题。
- CORS : 当我们想要扩展我们的应用，让我们的数据被多个移动设备使用时，我们必须考虑跨资源共享问题。当使用AJAX调用从另一个域名下获取资源时，我们可能会遇到禁止请求的问题。
- CSRF : 用户很容易受到CSRF攻击。

### 5.2. JWT与Session的差异

相同点是，它们都是存储用户信息；然而，Session是在服务器端的，而JWT是在客户端的。

Session方式存储用户信息的最大问题在于要占用大量服务器内存，增加服务器的开销。

而JWT方式将用户状态分散到了客户端中，可以明显减轻服务端的内存压力。

Session的状态是存储在服务器端，客户端只有session id；而Token的状态是存储在客户端。

*![img](https://images2018.cnblogs.com/blog/874963/201807/874963-20180707160150557-1205884800.png)*

 

### 5.3. 基于Token的身份认证是如何工作的

基于Token的身份认证是无状态的，服务器或者Session中不会存储任何用户信息。

> 没有会话信息意味着应用程序可以根据需要扩展和添加更多的机器，而不必担心用户登录的位置。

虽然这一实现可能会有所不同，但其主要流程如下：

1. 用户携带用户名和密码请求访问
2. 服务器校验用户凭据
3. 应用提供一个token给客户端
4. 客户端存储token，并且在随后的每一次请求中都带着它
5. 服务器校验token并返回数据

注意：

1. 每一次请求都需要token
2. Token应该放在请求header中
3. 我们还需要将服务器设置为接受来自所有域的请求，用Access-Control-Allow-Origin: *

### 5.4. 用Token的好处

- 无状态和可扩展性：Tokens存储在客户端。完全无状态，可扩展。我们的负载均衡器可以将用户传递到任意服务器，因为在任何地方都没有状态或会话信息。
- 安全：Token不是Cookie。（The token, not a cookie.）每次请求的时候Token都会被发送。而且，由于没有Cookie被发送，还有助于防止CSRF攻击。即使在你的实现中将token存储到客户端的Cookie中，这个Cookie也只是一种存储机制，而非身份认证机制。没有基于会话的信息可以操作，因为我们没有会话!

> ![img](https://images2018.cnblogs.com/blog/874963/201807/874963-20180707162551160-1143708148.png)

还有一点，token在一段时间以后会过期，这个时候用户需要重新登录。这有助于我们保持安全。还有一个概念叫token撤销，它允许我们根据相同的授权许可使特定的token甚至一组token无效。

### 5.5. JWT与OAuth的区别

1. OAuth2是一种授权框架 ，JWT是一种认证协议
2. 无论使用哪种方式切记用HTTPS来保证数据的安全性
3. OAuth2用在使用第三方账号登录的情况(比如使用weibo, qq, github登录某个app)，而**JWT是用在前后端分离**, 需要简单的对后台API进行保护时使用。

### 5.6. 关于OAuth可以参考下面几篇

《[OAuth 2.0](http://www.cnblogs.com/cjsblog/p/9174797.html)》

《[Spring Security OAuth 2.0](http://www.cnblogs.com/cjsblog/p/9184173.html)》

《[OAuth 2.0 授权码请求](http://www.cnblogs.com/cjsblog/p/9230990.html)》

## 6. 参考

https://jwt.io/

https://scotch.io/tutorials/the-ins-and-outs-of-token-based-authentication#toc-why-tokens-came-around

https://tools.ietf.org/html/rfc7519#section-3

[http://blog.leapoahead.com/2015/09/06/understanding-jwt/](http://blog.leapoahead.com/2015/09/06/understanding-jwt/)

https://cnodejs.org/topic/557844a8e3cc2f192486a8ff

[http://blog.leapoahead.com/2015/09/07/user-authentication-with-jwt/](http://blog.leapoahead.com/2015/09/07/user-authentication-with-jwt/)





**JWT的原则**

JWT的原则是在服务器身份验证之后，将生成一个JSON对象并将其发送回用户，如下所示。

{

"UserName": "Chongchong",

"Role": "Admin",

"Expire": "2018-08-08 20:15:56"

}

之后，当用户与服务器通信时，客户在请求中发回JSON对象。服务器仅依赖于这个JSON对象来标识用户。为了防止用户篡改数据，服务器将在生成对象时添加签名（有关详细信息，请参阅下文）。

服务器不保存任何会话数据，即服务器变为无状态，使其更容易扩展。

**3. JWT的数据结构**

典型的，一个JWT看起来如下图。





改对象为一个很长的字符串，字符之间通过"."分隔符分为三个子串。注意JWT对象为一个长字串，各字串之间也没有换行符，此处为了演示需要，我们特意分行并用不同颜色表示了。每一个子串表示了一个功能块，总共有以下三个部分：

JWT的三个部分如下。JWT头、有效载荷和签名，将它们写成一行如下。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=563276735,1576219691&fm=173&app=25&f=JPEG?w=640&h=237&s=D8243D7287E04D011E54B1CF0000A0B3)





我们将在下面介绍这三个部分。

**3.1 JWT头**

JWT头部分是一个描述JWT元数据的JSON对象，通常如下所示。

{

"alg": "HS256",

"typ": "JWT"

}

在上面的代码中，alg属性表示签名使用的算法，默认为HMAC SHA256（写为HS256）；typ属性表示令牌的类型，JWT令牌统一写为JWT。

最后，使用Base64 URL算法将上述JSON对象转换为字符串保存。

**3.2 有效载荷**

有效载荷部分，是JWT的主体内容部分，也是一个JSON对象，包含需要传递的数据。 JWT指定七个默认字段供选择。

iss：发行人

exp：到期时间

sub：主题

aud：用户

nbf：在此之前不可用

iat：发布时间

jti：JWT ID用于标识该JWT

除以上默认字段外，我们还可以自定义私有字段，如下例：

{

"sub": "1234567890",

"name": "chongchong",

"admin": true

}

请注意，默认情况下JWT是未加密的，任何人都可以解读其内容，因此不要构建隐私信息字段，存放保密信息，以防止信息泄露。

JSON对象也使用Base64 URL算法转换为字符串保存。

**3.3签名哈希**

签名哈希部分是对上面两部分数据签名，通过指定的算法生成哈希，以确保数据不会被篡改。

首先，需要指定一个密码（secret）。该密码仅仅为保存在服务器中，并且不能向用户公开。然后，使用标头中指定的签名算法（默认情况下为HMAC SHA256）根据以下公式生成签名。

HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload),

secret)

在计算出签名哈希后，JWT头，有效载荷和签名哈希的三个部分组合成一个字符串，每个部分用"."分隔，就构成整个JWT对象。

**3.4 Base64URL算法**

如前所述，JWT头和有效载荷序列化的算法都用到了Base64URL。该算法和常见Base64算法类似，稍有差别。

作为令牌的JWT可以放在URL中（例如api.example/?token=xxx）。 Base64中用的三个字符是"+"，"/"和"="，由于在URL中有特殊含义，因此Base64URL中对他们做了替换："="去掉，"+"用"-"替换，"/"用"_"替换，这就是Base64URL算法，很简单把。

**4. JWT的用法**

客户端接收服务器返回的JWT，将其存储在Cookie或localStorage中。

此后，客户端将在与服务器交互中都会带JWT。如果将它存储在Cookie中，就可以自动发送，但是不会跨域，因此一般是将它放入HTTP请求的Header Authorization字段中。

Authorization: Bearer

当跨域时，也可以将JWT被放置于POST请求的数据主体中。

**5. JWT问题和趋势**

1、JWT默认不加密，但可以加密。生成原始令牌后，可以使用改令牌再次对其进行加密。

2、当JWT未加密方法是，一些私密数据无法通过JWT传输。

3、JWT不仅可用于认证，还可用于信息交换。善用JWT有助于减少服务器请求数据库的次数。

4、JWT的最大缺点是服务器不保存会话状态，所以在使用期间不可能取消令牌或更改令牌的权限。也就是说，一旦JWT签发，在有效期内将会一直有效。

5、JWT本身包含认证信息，因此一旦信息泄露，任何人都可以获得令牌的所有权限。为了减少盗用，JWT的有效期不宜设置太长。对于某些重要操作，用户在使用时应该每次都进行进行身份验证。

6、为了减少盗用和窃取，JWT不建议使用HTTP协议来传输代码，而是使用加密的HTTPS协议进行传输。