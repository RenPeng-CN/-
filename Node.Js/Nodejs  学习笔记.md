

[TOC]

#Nodejs 常用命令

**学习** **Node.js** **必须要掌握的基础知识** 

![image-20190127004407681](/Users/renpeng/Library/Application Support/typora-user-images/image-20190127004407681.png)

Node自己复习的时候，顺序：

Node.js特点：单线程、异步I/O（非阻塞I/O）、事件驱动（事件环）

适合的程序：就是没有太多的计算，I/O比较多的业务。举例：留言本、考试系统、说说、图片裁切服务器

 

Node.js原生： http、fs、path、url。 静态服务、简单路由、GET、POST请求。

 

模块：formidable、gm、express

 

Express：中间件、MVC建站、模板引擎ejs、静态服务、简单路由、GET、POST请求、MD5加密、图片上传。

 

服务器的一些概念：Cookie、Session

 

持久化NoSQL： 非关系型数据库，Not Only SQL。

特点：没有schema，没有行和列。用文档（JSON）来存储。

 

MongoDB：安装、开启、导入数据、Shell管理数据库、Mongo Vue、Node.js做CRUD（增删改查）、DAO层的封装、索引、操作符$set $lt $gt $push $pull

 

Mongoose： ODM，不用直接操作数据库，操作对象，这个对象自动持久。

 









node -v  **#** 查看node版本

console.log(“Hello World”);  **#** 在控制台输出 hello world

node hellowworld.js  **#** 通过node命令运行文件

var http = require(“http”);  **#**我们使用 **require** 指令来载入 http 模块，并将实例化的 HTTP 赋值给变量 http

node -v **#** 查看npm版本

npm -h  **#** npm 帮助

npm update  **#** 升级 npm 到最新版

npm install express —save **#**安装express模块，先定位到需要安装的地址，然后执行命令进行安装, 带 —save会自动升级项目 package.json 目录，自动添加依赖项 

npm list -g  **#**查看所有全局安装的模块

npm list grunt  **#**查看某个模块的版本号，本例为查看 grunt 的版本号

npm init  # 让你回答项目问题，自动生成这个项目的 package.json 文件。

npm install # 会检测项目的 package.json 文件，并自动安装项目需要的所有依赖包

下图是 package.json文件里 依赖包的版本控制，尖角号 表示版本号首位不再更新，其它为可以更新，如果不加尖角号，就可以把该包升级到最新版本。如果标记 三个尖角号，就不会再自动升级该包的版本了。 

 ![image-20190127004426410](/Users/renpeng/Library/Application Support/typora-user-images/image-20190127004426410.png)

![image-20190127004431703](/Users/renpeng/Library/Application Support/typora-user-images/image-20190127004431703.png)







Package.json 属性说明

​	name - 包名。

​	version - 包的版本号。

​	description - 包的描述。

​	homepage - 包的官网 url 。

​	author - 包的作者姓名。

​	contributors - 包的其他贡献者姓名。

​	dependencies - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。

​	repository - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。

​	main - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。

​	keywords - 关键字



npm uninstall express **#** 卸载模块，本例为卸载 express 模块。

npm ls  **#** 卸载后，你可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看

npm update express  **#** 升级express模块

npm search express  **#** 搜索模块

npm init  **#** 创建模块，package.json 文件是必不可少的。我们可以使用 NPM 生成 package.json 文件，生成的文件包含了基本的结果

npm publish **#** 发布自己创建的模块

npm cache clear  **#** 可以清空NPM本地缓存

npm unpublish <package>@<version>  **#** 可以撤销发布自己发布过的某个版本代码



node  #启动 Node 的终端



REPL 命令

​	ctrl + c - 退出当前终端。

​	ctrl + c 按下两次 - 退出 Node REPL。

​	ctrl + d - 退出 Node REPL.

​	向上/向下 键 - 查看输入的历史命令

​	tab 键 - 列出当前命令

​	.help - 列出使用命令

​	.break - 退出多行表达式

​	.clear - 退出多行表达式

​	.save filename - 保存当前的 Node REPL 会话到指定文件

​	.load filename - 载入当前 Node REPL 会话的文件内容。

停止 REPL

前面我们已经提到按下两次 ctrl + c 键就能退出 REPL:










**常用** **Node** **工具包** 



**通过** **npm** **网站搜索，了解以下各包的使用方法，通过** **npm** **下载各包**



1. 文件、表单 POST 上传到服务器： formidable 包。
2. http路由管理：express 框架。
3. 时间格式转换：silly-datetime 
4. 把数据绑定到字符串里，模板引擎，Embedded JavaScript templates ejs  ，效率不高
5. jade
6. npm -i —save [socket.io](http://socket.io)   #安装 socket 通路库 (  套接字，或插座  ) ，客户端实时连接服务器，不间断 ，走 TCP  






**建立** **Node** **项目**

Atom 使用Jshint时const is available in ES6 (use esversion:6)  #解决方法

在项目的根目录新建一个文件名为.jshintrc的文件，输入

{

​     "esversion": 6

}







\0.  在项目根目录，建立 MVC 文件夹 ，mvc 首字母开头的三个文件夹名，不能写错，否则报错

​					  models (存放模型) , 

​                                                             views (放前台视图) , 

​                                                             controller (也可以叫router, 就是放路由文件) ,

​       					  public (存放静态页面文件) , 

​					  uploads (存放上传的文件)

​					  app.js  入口文件

\1.  在 app.js  , 写入代码  var express = require(“express”)j;  var app = express();  app.listen(3002);

1. 进入项目根目录文件夹
2. npm init  # 初始化以下项目，回答一系列问题，会自动建立项目的  package.json 文件，存放扩展包记录表
3. npm install —save express  # 安装express框架，save参数会自动修改 package.json 文件，把express依赖绑上。并且会自动建立 node_modules 文件夹，将扩展包都存入其中以备调用。
4. 在 app.js ，写入代码 app.set(“view engine” : “ ejs ”); // 设置模板引擎
5. 在 app.js  ，app.use(express.static(“./public”)); // 将public文件夹设置为静态页面访问
6. npm i —save ejs  # 安装ejs模块
7. 使用 bootstrap 网站 提供的 前台样式模板，快速搭建前台页面
8. 倒入 jquery.js 文件到 /public/js/







### 在 MAC 上安装 nvm 需要注意的事项

- 在浏览器打开以下地址，按照说明进行安装及使用

  https://github.com/creationix/nvm#installation

- 安装后 在mac终端 输入 nvm --version  显示 nvm command nofound

  - 是因为你的 mac 系统没有  [`.bash_profile] 文件 

  - 通过命令创建一个文件

    ```
    touch ~/.bash_profile
    ```

- 打开 访达 ，按 shift + 苹果键 + G 键，打开要访问的地址窗口，输入 ~, 

- 打开 .bash_profile 文件，并输入

  ```js
  export NVM_DIR=~/.nvm
  source ~/.nvm/nvm.sh
  ```

- 最后，关闭终端，重新打开，输入 

  ```js
  command -v nvm
  ```

  如果输出 nvm，则表示可以正常使用了。 















###npm 使用

<font color='red'>npm -v</font>  #查看npm版本

<font color='red'>npm install -g npm</font> # npm 将自己升级到最新版本

<font color='red'>npm update -g</font> # 把npm管理的软件都升级到最新版

<font color="red">npm i</font> # 按照 package.json 里记录的模块，下载所有模块

<font color="red">npm un wepack -s && npm i webpack@3.6.0</font>  # 卸载webpack模块，并安装 webpack3.6版

<font color="red">npm init</font> #初始化 node 项目，需要填写一些列问题

<font color="red">npm info vue-loader versions</font>  #查看vue-loader模块的各种版本号

通过npm对 node 包的分类管理和分类恢复

1.  安装包的时候，做一个分类管理

​        \* 开发依赖 （打包相关的 webpack）npm i  包名 -D   #  -D 是 deveDependecies

​        \* 生产依赖 ( 不包含 webpack 打包依赖) npm i 包名 -S  # dependencies



\2. 恢复依赖

​       \* 如果包文件不小心删了，少了

​       \* 开发恢复 <font color="red">npm i</font>

​       \* 生产恢复 <font color="red">npm i —production </font>









###nrm 管理及使用

<font color="red">npm install -g nrm </font> #全局安装nrm。

<font color="red">nrm ls </font> #查看可选的源

<font color="red">nrm use taobao</font> #切换到淘宝源 

<font color="red">nrm del <registry></font>   #删除对应的源

<font color="red">nrm test npm </font> # 测试相应源的响应时间

// 建议不要用 cnpm , 会有各种各样诡异的问题，以下方法可以解决 npm 慢的问题

```js
npm install --registry=https://registry.npm.taobao.org
```





### node_modules 分类管理和恢复

1. npm install 安装包的时候，做一个分类管理

   * 与开发依赖相关的包 (打包相关 webpack ) <font color='red'>npm i 包名 -D</font>   #  代表 development ，

      分类名 <font color='blue'>devDependencies</font>

   * 生产依赖 ( 不包含 webpack 打包依赖) npm i 包名 -S  #正常安装

      分类名 <font color='blue'>dependencies</font>

2. 恢复依赖，如果包文件不小心删了，少了
   * 开发（ devDependencies 分类的包）恢复方法：<font color='blue'>npm i</font>
   * 生产恢复 （ dependencies 分类的包 ）<font color='blue'>npm i --production</font> 





### PM2 操作

到了这一步，可以先把程序代码文件通过FTP上传到centos服务器 /usr/share/nginx/

pm2 是一个带有负载均衡功能的Node应用的进程管理器

1. npm install -g pm2  #安装PM2

2. pm2 start bin/ www –name scienceKids  #启动node应用, 并给应用起个名字

3. pm2 start bin/www —watch  # 添加应用 并监控

4. pm2 stop - -watch 0  #停止监控 序列 0 的应用

5. pm2 stop www  #停止应用

6. pm2 stop all  #停止所有应用

7. pm2 delete www  #删除www应用

8. pm2 delete all  #删除所有应用

9. pm2 list  #列出所有应用

10. pm2 describe www  #查看www进程的具体情况

11. pm2 monit  # 查看应用的资源消耗情况

12. pm2 restart www  #重启应用

13. pm2 restart all  # 重启所有应用

14. pm2 logs  # 查看pm2的运行日志

15. pm2 start www -i 4 --watch  #通过 cluster 4线程 模式启动node 并监控

 

 

在项目更目录建立一个  xxx.json 文件，粘贴如下列内容，注意配置 log 路径

16. pm2 start app.js -i 3 #开启三个进程 pm2 start app.js -i max  #根据机器cpu核数，开启对应数目的进程

17. pm2 --help

18. 开机启动  pm2 startup  

​      pm2 save 保存当前进程状态

​      pm2 startup  生成开机启动命令

19. 更新pm2 

​     pm2 save  #先保存进程状态

​     Npm install pm2 -g

​     pm2 update

20. 如果手动重启了 服务器，pm2却没有重新启动，/usr/share/nginx/html/sciencekids/bin/www 

​       **pm2 start www -i 4 --name sciencekids --watch** 

​       --name sciencekids --watch 到这个地址可以启动进程

 

21. npm i   # 按照 package.json 里记录的模块，下载所有模块

 

{

  "apps": [

​    {

​      "name": "website",  #名称改成你项目的名称

​      "script": "./bin/www",

​      "cwd": "./",

​      "watch": [

​        "bin",

​        "config",

​        "routes",

​        "views"

​      ],

​      "error_file": "./logs/website-err.log",    # 如果项目根目录下没有logs文件夹及下面的文件

​      "out_file": "./logs/website-out.log",   #就自行创建log

​      "log_date_format": "YYYY-MM-DD HH:mm Z"

​    }

  ]

}

 

pm2  start  xxx.json  #通过 pm2 启动 node

 

pm2 start ./bin/www -i 0 –name sciencekids  # 启动小程序node后台

