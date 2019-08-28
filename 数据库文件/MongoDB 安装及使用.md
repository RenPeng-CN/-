[TOC]

### 新建项目，并设置 MongoDB ( Linux OS 上 进行操作 )

```js
1. mkdir myproject  # 新建一个项目文件夹
2. cd myproject   # 进入该文件夹
3. npm init   #  npm 初始化该项目
4. npm install mongodb --save   # 安装 mongodb 的模块
5. mongod --dbpath=/data  #  配置数据存储路径

// 导出 备份 数据库 案例 -h host地址   -d 数据库名称  -o 输出地址
mongodump -h 127.0.0.1 -d cms -o /Volumes/RenPeng/Code/cms_demo/mongoDB

// 导出 数据库 案例 -h host地址   -d 数据库名称  
mongorestore -h 127.0.0.1 -d koademo /Volumes/RenPeng/Code/cms_demo/mongoDb/cms

```



代码页面

```js
// 1. 引入 mongodb 模块
const MongoClient = require('mongodb').MongoClient;

// 2. 设置连接数据库的地址
const dbUrl = 'mongodb://localhost:27017/';

// 3. 设置数据库的名称
const dbName = 'koa';

// 4. 连接数据库
MongoClient.connect(dbUrl, (err, client)=> {
	if(err){
		console.log('数据库连接失败');
		console.log(err);
		return;
	}
	
	var db = client.db(dbName);
	
	// 增加数据
	db.collection('news').insertOne({'username':'董l六','age':40,'sex':'男','status':'1'},function(err, result){
		if(!err){
			console.log('增加数据成功！')；
            client.close();
		}
	})
	
});
```





### Mac 上的 MongoDB 的操作

```js
brew install mongodb  # 安装 mongodb

# mongoDB 安装的目录就是 
/usr/local/Cellar/mongodb/4.0.3_1

# mongoDb 的全局环境变量位置 
/usr/local/Cellar/mongodb/4.0.3_1/bin

# mongoDB 的配置文件默认在 
/usr/local/etc 下的 mongod.conf

# systemLog mongoDB系统日志:
  destination: file
  path: /usr/local/var/log/mongodb/mongo.log
  logAppend: true

# storage 数据库存储位置:
  dbPath: /usr/local/var/mongodb


brew services start mongodb  # 启动mongodb服务：在终端执行
mongo  # 在终端输入 mongo ，进入数据库
cls  # 清屏
show dbs  # 显示数据库
use sciencekids  # 使用 或者 建立 sciencekids 数据库
db.user.insert({'name':'张三','age':20})  # 建立user表，并插入数据
use sciencekids  # 使用 数据库
show collections  # 显示 sciencekids 数据库中的所有集合
db.admin.insert({'username':'admin','password':'123456'})  # 插入一条数据

# 增加，循环插入 1000 条数据
for(let i=0; i<1000; i++){
    db.user.insert({'username':'张三'+i});
}

db.user.find().limit(10);  # 查询 user 表里的前 10条数据
db.user.find().skip(10).limit(10); # 查询 user 表，跳过10条后的10条数据


# 查询所有内容
db.user.find(); # 查询 user 集合里的内容

#查询 age = 20 岁的数据
db.user.find({'age':20});  # 查找年龄等于 20 岁的数据

#查询 age > 20 岁的数据
db.user.find({'age':{$gt:20}});  # 查找 age > 20 ,年龄 大于 20 岁 的所有数据

#查询 age < 50 岁的数据
db.user.find({'age':{$lt:50}});  # 查找 age < 50  ,年龄 小于 50 岁的所有数据

#查询 age >= 25 岁的数据
db.user.find({'age':{$gte:25}}); # 查找 age >= 25 ,年龄 大于等于 25岁的所有数据

#查询 age <= 36 岁的数据
db.user.find({'age':{$lte:36}}); # 查找 age <= 36 ,年龄 小于等于 36岁的所有数据

#查询 age >=20 并且 age <= 26岁的数据
db.user.find({'age':{$gte:20,$lte:30}}); # 查找 age >= 20 并且 age <= 26 ,的所有数据

# 模糊查询
db.artical.find({'title':/文章/});  # 模糊查询 title 里 有文章两个字的 所有数据

#模糊查询 name 字段里 z 开头的内容
db.user.find({'name':/^z/});  # 模糊查询 name 字段里 z 开头的内容

#指定查询某一列
db.user.find({},{name:1});  # 查询指定列 
db.user.find({'age':{$gt:20}},{name:1});  # 查询年龄大于 20岁 并且只显示name字段

#指定查询两个列
db.user.find({},{name:1,age:1});  # 指定查询列

#升序排序
db.user.find({}).sort({'age':1});  # 把 age 列 正序排序

#降序排序
db.user.find({}).sort({'age':-1});   # 把 age 列 降序排序

#限制只查询前两条
db.user.find({}).limit(2);  # 只显示搜索出来的 前两条 内容

#跳过两条，查出3条， 分页需要的语法
db.user.find().skip(2).limit(3);  # 跳过 前两条 ，只显示 跳过后的 三条 内容

# 搜索年龄是 20 和 26岁的搜索内容
db.user.find({$or:[{'age':20},{'age':26}]});  

# 搜索name 是 张三  age 是 20 的所有内容
db.user.find({'name':'张三', 'age':20});  

 # 搜索 age 是 20 或者 30 的所有内容
db.user.find({$or:[{'age':20},{'age':30}]}); 

 # 搜索出 第一条 内容
db.user.findOne(); 

# count 计数
db.user.find().count();  # count 出列数

# count 出 age 大于 30 的有多少数量
db.user.find({'age':{$gt:30}}).count();  

# 删除集合
db.artical.drop();  # 删除 artical 集合(表), 数据库里的表都删除完了，数据库也就自动删除了。

#删除数据库
switched to db itying # 要删除 itying 数据库，就先 use itying, 然后执行下面的代码
> db.dropDatabase();


# 修改
db.user.update({'name':'张三'},{$set:{'name':'张三四五'}});  # 将 name 张三 改为 张三四五
db.user.update({'name':'李四'},{$set:{'age':32,'name':'李四五六'}}); # 修改年龄和姓名

# 删除行
db.user.remove({});  # 删除 user 表里所有内容
db.user.remove({'age':'35'});   # 删除user表里 age 等于 '35' 的内容
db.user.remove({'name':'张三四五'},{justOne:true});  # 删除名字是 张三四五 的行，只删除一行


# 索引, 索引是对数据库表中一列或多列的值进行排序的一种结构，可以让我们查询数据库变得 更快。MongoDB 的索引几乎与传统的关系型数据库一模一样，这其中也包括一些基本的查 询优化技巧。

db.user.getIndexes();   # 查看 user 集合例所有列的索引
db.user.find({'name':'张三'}).explain('executionStats');  # 搜索并查看搜索状态
db.user.ensureIndex({'name':1});  # 给 name 列 设置索引。1 的意思是 按 正序排序，-1 按倒序排序
db.user.dropIndex({'name':1});  # 删除 name 列的索引
db.user.ensureIndex({'name':1,'age':1});  # 设置 联合索引 
db.user.dropIndex({'name':1,'age':1});  # 删除复合索引
db.user.ensureIndex({'name':1},{'unique':true});  # 设置唯一索引，name 的值 必须是唯一的





mongod --help  # 查看所有帮助
--dbpath # $dbpath(数据库数据文件路径)
--logpath # $logpath(日志文件的路径)
--logappend # (以追加的方式打开文件)
--fork #(将数据库服务放在后台运行)

脚本启动或配置文件启动

mongo 127.0.0.1:27017  # 访问远程 mongodb
```











```js
General options:
  -v [ --verbose ] [=arg(=v)]           #be more verbose (include multiple times
                                        #for more verbosity e.g. -vvvvv)
  --quiet                              # quieter output
  --port arg                           # specify port number - 27017 by default
  --logpath arg                         #log file to send write to instead of 
                                        #stdout - has to be a file, not 
                                        #directory
  --syslog                              #log to system's syslog facility instead
                                        #of file or stdout
  --syslogFacility arg                  #syslog facility used for mongodb syslog
                                        #message
  --logappend                           #append to logpath instead of 
                                        #over-writing
  --logRotate arg                       #set the log rotation behavior 
                                       # (rename|reopen)
  --timeStampFormat arg                # Desired format for timestamps in log 
                                       # messages. One of ctime, iso8601-utc or 
                                        #iso8601-local
  --setParameter arg                    #Set a configurable parameter
  -h [ --help ]                        # show this usage information
  --version                             #show version information
  -f [ --config ] arg                  # configuration file specifying 
                                        #additional options
  --bind_ip arg                        # comma separated list of ip addresses to
                                       # listen on - localhost by default
  --bind_ip_all                        # bind to all ip addresses
  --ipv6                                #enable IPv6 support (disabled by 
                                       # default)
  --listenBacklog arg (=128)           # set socket listen backlog size
  --maxConns arg                       # max number of simultaneous connections 
                                       # - 1000000 by default
  --pidfilepath arg                   #  full path to pidfile (if not set, no 
                                       # pidfile is created)
  --timeZoneInfo arg                   # full path to time zone info directory, 
                                       # e.g. /usr/share/zoneinfo
  --keyFile arg                        # private key for cluster authentication
  --noauth                             # run without security
  --transitionToAuth                   # For rolling access control upgrade. 
                                        #Attempt to authenticate over outgoing 
                                       # connections and proceed regardless of 
                                       # success. Accept incoming connections 
                                       # with or without authentication.
  --clusterAuthMode arg                # Authentication mode used for cluster 
                                       # authentication. Alternatives are 
                                       # (keyFile|sendKeyFile|sendX509|x509)
  --nounixsocket                        #disable listening on unix sockets
  --unixSocketPrefix arg               # alternative directory for UNIX domain 
                                        #sockets (defaults to /tmp)
  --filePermissions arg                # permissions to set on UNIX domain 
                                       # socket file - 0700 by default
  --fork                               # fork server process
  --slowms arg (=100)                  # value of slow for profile and console 
                                       # log
  --slowOpSampleRate arg (=1)          # fraction of slow ops to include in the 
                                       # profile and console log
  --networkMessageCompressors [=arg(=disabled)] (=snappy)
                                       # Comma-separated list of compressors to 
                                       # use for network messages
  --auth                               # run with security
  --clusterIpSourceWhitelist arg       # Network CIDR specification of permitted
                                        #origin for `__system` access.
  --profile arg                        # 0=off 1=slow, 2=all
  --cpu                                # periodically show cpu and iowait 
                                        #utilization
  --sysinfo                             #print some diagnostic system 
                                        #information
  --noIndexBuildRetry                  # don't retry any index builds that were 
                                       # interrupted by shutdown
  --noscripting                        # disable scripting engine
  --notablescan                        # do not allow table scans

Replication options:
  --oplogSize arg                       #size to use (in MB) for replication op 
                                        #log. default is 5% of disk space (i.e. 
                                        #large is good)
  --master                              #Master/slave replication no longer 
                                       # supported
  --slave                               #Master/slave replication no longer 
                                       # supported

Replica set options:
  --replSet arg                        # arg is <setname>[/<optionalseedhostlist
                                       # >]
  --replIndexPrefetch arg               #specify index prefetching behavior (if 
                                       # secondary) [none|_id_only|all]
  --enableMajorityReadConcern [=arg(=1)] (=1)
                                       # enables majority readConcern

Sharding options:
  --configsvr                           #declare this is a config db of a 
                                       # cluster; default port 27019; default 
                                       # dir /data/configdb
  --shardsvr                           # declare this is a shard db of a 
                                       # cluster; default port 27018

SSL options:
  --sslOnNormalPorts                    #use ssl on configured ports
  --sslMode arg                        # set the SSL operation mode 
                                       # (disabled|allowSSL|preferSSL|requireSSL)
  --sslPEMKeyFile arg                   #PEM file for ssl
  --sslPEMKeyPassword arg              # PEM file password
  --sslClusterFile arg                  #Key file for internal SSL 
                                        #authentication
  --sslClusterPassword arg              #Internal authentication key file 
                                       # password
  --sslCAFile arg                      # Certificate Authority file for SSL
  --sslClusterCAFile arg               # CA used for verifying remotes during 
                                       # outbound connections
  --sslCRLFile arg                     # Certificate Revocation List file for 
                                        #SSL
  --sslDisabledProtocols arg           # Comma separated list of TLS protocols 
                                       # to disable [TLS1_0,TLS1_1,TLS1_2]
  --sslWeakCertificateValidation       # allow client to connect without 
                                       # presenting a certificate
  --sslAllowConnectionsWithoutCertificates 
                                        #allow client to connect without 
                                       # presenting a certificate
  --sslAllowInvalidHostnames           # Allow server certificates to provide 
                                       # non-matching hostnames
  --sslAllowInvalidCertificates        # allow connections to servers with 
                                       # invalid certificates
  --sslFIPSMode                        # activate FIPS 140-2 mode at startup
  --sslCertificateSelector arg         # SSL Certificate in system store
  --sslClusterCertificateSelector arg  # SSL Certificate in system store for 
                                       # internal SSL authentication

Storage options:
  --storageEngine arg                  # what storage engine to use - defaults 
                                       # to wiredTiger if no data files present
  --dbpath arg                         # directory for datafiles - defaults to 
                                       # /data/db
  --directoryperdb                     # each database will be stored in a 
                                       # separate directory
  --noprealloc                         # disable data file preallocation - will 
                                       # often hurt performance
  --nssize arg (=16)                   # .ns file size (in MB) for new databases
  --quota                              # limits each database to a certain 
                                       # number of files (8 default)
  --quotaFiles arg                     # number of files allowed per db, implies
                                       # --quota
  --smallfiles                         # use a smaller default file size
  --syncdelay arg (=60)                 #seconds between disk syncs (0=never, 
                                        #but not recommended)
  --upgrade                            # upgrade db if needed
  --repair                              #run repair on all dbs
  --repairpath arg                      #root directory for repair files - 
                                       # defaults to dbpath
  --journal                            # enable journaling
  --nojournal                          # disable journaling (journaling is on by
                                       # default for 64 bit)
  --journalOptions arg                 # journal diagnostic options
  --journalCommitInterval arg         #  how often to group/batch commit (ms)

Free Monitoring options:
  --enableFreeMonitoring arg           # Enable Cloud Free Monitoring 
                                       # (on|runtime|off)
  --freeMonitoringTag arg              # Cloud Free Monitoring Tags

WiredTiger options:
  --wiredTigerCacheSizeGB arg           #maximum amount of memory to allocate 
                                        #for cache; defaults to 1/2 of physical 
                                       # RAM
  --wiredTigerJournalCompressor arg (=snappy)
                                       # use a compressor for log records 
                                        #[none|snappy|zlib]
  --wiredTigerDirectoryForIndexes      # Put indexes and data in different 
                                       # directories
  --wiredTigerCollectionBlockCompressor arg (=snappy)
                                        #block compression algorithm for 
                                        #collection data [none|snappy|zlib]
  --wiredTigerIndexPrefixCompression arg (=1)
                                       # use prefix compression on row-store 
                                       # leaf pages

```



### MongoDB 数据库的导出、导入

在MongoDB中，我们使用 mongodump 命令来备份 MongoDB 数据。该命令可以导出所有数据到指定目录中。 mongodump 命令可以通过参数指定导出的数据量级转存的服务器。 使用 mongorestore 命令来恢复备份的数据。

```js
// 导出
mongodump -h dbhost -d dbname -o dbdirectory

// 导入
mongorestore -h dbhost -d dbname <path>
```



### 二、Mongodb账户权限配置

 

**1、第一步创建超级管理用户**

 ```js
 use admin 
 
 db.createUser({
   user:'admin',
   pwd:'123456',
   roles:[{role:'root',db:'admin'}]	
 }) 
 ```





**2、第二步修改Mongodb数据库配置文件**

```js
路径**：C:\Program Files\MongoDB\Server\4.0\bin\mongod.cfg  **配置：** security:  	 authorization: enabled 
```

**路径**：C:\Program Files\MongoDB\Server\4.0\bin\mongod.cfg  **配置：** security:  	 authorization: enabled 



**3、第三步重启mongodb服务**（此方法为 在 windows 上操作）

![img](file:////tmp/wps-renpeng/ksohtml/wpsRtrJ3x.jpg) 

 

 

**4、第四步用超级管理员账户连接数据库**

 ```js
mongo admin -u 用户名 -p 密码 mongo 192.168.1.200:27017/test -u user -p password 
 ```





**5、第五步给eggcms数据库创建一个用户   只能访问eggcms不能访问其他数据库**

 ```js
 use eggcms
 
 db.createUser({  
   user: "eggadmin",  
   pwd: "123456",  
   roles: [ { role: "dbOwner", db: "eggcms" } ] 
 }) 
 ```





### 三、Mongodb账户权限配置中常用的命令

 ```js
1、show users;    #查看当前库下的用户 
2、db.dropUser("eggadmin");  #删除用户 
3、db.updateUser( "admin",{pwd:"password"}); #修改用户密码	
4、db.auth("admin","password");  #密码认证	, 某个数据库设置管理员和密码后，需要到 admin里进行备案，
 ```



  

### 四、Mongodb数据库角色

```js
1.数据库用户角色：read、readWrite;

2.数据库管理角色：dbAdmin、dbOwner、userAdmin；

3.集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；

4.备份恢复角色：backup、restore；

5.所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase

6.超级用户角色：root  
```



 

参考：<https://www.cnblogs.com/zzw1787044/p/5773178.html>

 

### 五、连接数据库的时候需要配置账户密码

 ```js
const url = 'mongodb://admin:123456@localhost:27017/';
 ```





### MongoDB的高级查询 **aggregate** 聚合管道 



主讲教师:(大地) 

合作网站:**www.itying.com** (IT 营) 我的专栏:https://www.itying.com/category-79-b0.html 

一、 MongoDB 聚合管道(Aggregation Pipeline)..............................................................................1 

二、 MongoDB Aggregation 管道操作符与表达式...............................................................................2 

三、 数据模拟........................................................................................................................................4 

四、 $project...........................................................................................................................................4 

五、 $match............................................................................................................................................5 

六、 $group.............................................................................................................................................6 

七、 $sort................................................................................................................................................6 

八、 $limit...............................................................................................................................................7 

九、 $skip................................................................................................................................................8 

十、 $lookup 表关联.............................................................................................................................9 



一、**MongoDB** 聚合管道(**Aggregation Pipeline**) 

使用聚合管道可以对集合中的文档进行变换和组合。 **实际项目:**表关联查询、数据的统计。 

MongoDB 中使用 db.COLLECTION_NAME.**aggregate([{<stage>},...])** 方法 来构建和使用聚合管道。先看下官网给的实例，感受一下聚合管道的用法。 

二、**MongoDB Aggregation** 管道操作符与表达式 

| 管道操作 符 | Description                                       |
| ----------- | ------------------------------------------------- |
| $project    | 增加、删除、重命名字段                            |
| $match      | 条件匹配。只满足条件的文档才能进入下一阶段        |
| $limit      | 限制结果的数量                                    |
| $skip       | 跳过文档的数量                                    |
| $sort       | 条件排序。                                        |
| $group      | 条件组合结果 统计                                 |
| $lookup     | $lookup 操作符 用以引入其它集合的数据(表关联查询) |

 

**SQL** 和 **NOSQL** 对比**:** 

| NOSQL    | SQL      |
| -------- | -------- |
| $match   | WHERE    |
| $group   | GROUP BY |
| $match   | HAVING   |
| $project | SELECT   |
| $sort    | ORDER BY |
| $limit   | LIMIT    |
| $sum     | SUM()    |
| $sum     | COUNT()  |
| $lookup  | join     |



**管道表达式:**

管道操作符作为“键”,所对应的“值”叫做管道表达式。 

例如 {$match:{ status:"A" }}, ​$Match 称为管道操作符，而 status:"A"称为管道表达式， 是管道操作符的操作数(Operand)。 

每个管道表达式是一个文档结构，它是由字段名、字段值、和一些表达式操作符组成的。 

| 常用表达式操作符 | **Description**        |
| ---------------- | ---------------------- |
| $addToSet        | 将文档指定字段的值去重 |
| $max             | 文档指定字段的最大值   |
| $min             | 文档指定字段的最小值   |
| $sum             | 文档指定字段求和       |
| $avg             | 文档指定字段求平均     |
| $gt              | 大于给定值             |
| $lt              | 小于给定值             |
| $eq              | 等于给定值             |

 

三、 数据模拟 , 将以下数据输入 mongoDB 数据库，以便进行模拟

```js
db.order.insert({"order_id":"1","uid":10,"trade_no":"111","all_price":100,"all_num":2}); db.order.insert({"order_id":"2","uid":7,"trade_no":"222","all_price":90,"all_num":2}); db.order.insert({"order_id":"3","uid":9,"trade_no":"333","all_price":20,"all_num":6}); 

db.order_item.insert({"order_id":"1","title":"商品鼠标 1","price":50,num:1}); db.order_item.insert({"order_id":"1","title":"商品键盘 2","price":50,num:1}); db.order_item.insert({"order_id":"1","title":"商品键盘 3","price":0,num:1}); 

db.order_item.insert({"order_id":"2","title":"牛奶","price":50,num:1}); db.order_item.insert({"order_id":"2","title":"酸奶","price":40,num:1}); 

db.order_item.insert({"order_id":"3","title":"矿泉水","price":2,num:5}); db.order_item.insert({"order_id":"3","title":"毛巾","price":10,num:1}); 
```



四、 **$project** 

修改文档的结构，可以用来重命名、增加或删除文档中的字段。 

要求查找 order 只返回文档中 trade_no 和 all_price 字段 

```js
db.order.aggregate([ { 

$project:{ trade_no:1, all_price:1 } } 

]) 
```



五、 **$match** 

作用
 用于过滤文档。用法类似于 find() 方法中的参数。 

```js
db.order.aggregate([ 
{ 

     $project:{ trade_no:1, all_price:1 } // 只查找两个字段
}, 

{
    $match:{"all_price":{$gte:90}}  // 再查找出 all_price 大于等于 90 的数据

} 
]) 
```



六、 **$group** 

```
将集合中的文档进行分组，可用于统计结果。
统计每个订单的订单数量，按照订单号分组
```

```js
db.order_item.aggregate( [ 
{
 $group: {_id: "$order_id", total: {$sum: "$num"}} 

} ] 

) 
```



七、 **$sort** 

将集合中的文档进行排序。 

```js
db.order.aggregate([ { 

$project:{ trade_no:1, all_price:1 } },  // 查询两个字段

{
 $match:{"all_price":{$gte:90}} // 匹配出 大于等于90的数据

}, { 

$sort:{"all_price":-1} } // 再按照从大到小的降序排序，

]) 
```



八、 **$limit** 

```js
db.order.aggregate([ { 

$project:{ trade_no:1, all_price:1 } }, 

{
 $match:{"all_price":{$gte:90}} 

}, { 

$sort:{"all_price":-1} }, 

{
 $limit:1 

} ]) 
```



九、 **$skip** 

```js
db.order.aggregate([ { 

$project:{ trade_no:1, all_price:1 } }, 

{
 $match:{"all_price":{$gte:90}} 

}, { 

$sort:{"all_price":-1} }, 

{
 $skip:1 

} ]) 
```



十、**$lookup** 表关联查询，非常重要哦

```js
db.order.aggregate([ {  // order 声明为主表
  $lookup: {  // 关联查询的命令
    from: "order_item",  // 声明要关联的表示 order_item
    localField: "order_id",  // 声明 主表字段
    foreignField: "order_id", // 声明要匹配的 关联表 字段
    as: "items" // 将关联结果放到 items 数组里
   } } 
]) 



// 以下是查询出的结果

{
 "_id": ObjectId("5b743d8c2c327f8d1b360540"), "order_id": "1",
 "uid": 10,
 "trade_no": "111",
 "all_price": 100,
 "all_num": 2,
 "items": [{ 

"_id": ObjectId("5b743d9c2c327f8d1b360543"), "order_id": "1",
 "title": "商品鼠标 1",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da12c327f8d1b360544"), "order_id": "1",
 "title": "商品键盘 2",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b74f457089f78dc8f0a4f3b"), "order_id": "1",
 "title": "商品键盘 3",
 "price": 0, 

"num": 1 }] 

}{
 "_id": ObjectId("5b743d902c327f8d1b360541"), "order_id": "2",
 "uid": 7,
 "trade_no": "222",
 "all_price": 90,
 "all_num": 2,
 "items": [{ 

"_id": ObjectId("5b743da52c327f8d1b360545"), "order_id": "2",
 "title": "牛奶",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da92c327f8d1b360546"), "order_id": "2", 

"title": "酸奶", "price": 40, "num": 1 

}] }{ 

"_id": ObjectId("5b743d962c327f8d1b360542"), "order_id": "3",
 "uid": 9,
 "trade_no": "333", 

"all_price": 20, "all_num": 6, "items": [{ 

"_id": ObjectId("5b743dad2c327f8d1b360547"), "order_id": "3",
 "title": "矿泉水",
 "price": 2, 

"num": 5 }, { 

"_id": ObjectId("5b743dff2c327f8d1b360548"), "order_id": "3",
 "title": "毛巾",
 "price": 10, 

"num": 1 }] 

} 
```



```js
db.order.aggregate([ { 

$lookup: 

{ 

} }, 

{
 $match:{"all_price":{$gte:90}} 

} 

]) 

{
 "_id": ObjectId("5b743d8c2c327f8d1b360540"), "order_id": "1",
 "uid": 10,
 "trade_no": "111",
 "all_price": 100,
 "all_num": 2,
 "items": [{ 

"_id": ObjectId("5b743d9c2c327f8d1b360543"), "order_id": "1",
 "title": "商品鼠标 1",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da12c327f8d1b360544"), "order_id": "1",
 "title": "商品键盘 2",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b74f457089f78dc8f0a4f3b"), "order_id": "1",
 "title": "商品键盘 3",
 "price": 0, 

"num": 1 }] 

from: "order_item", 

localField: "order_id", foreignField: "order_id", as: "items" 

}{
 "_id": ObjectId("5b743d902c327f8d1b360541"), "order_id": "2",
 "uid": 7,
 "trade_no": "222",
 "all_price": 90,
 "all_num": 2,
 "items": [{ 

"_id": ObjectId("5b743da52c327f8d1b360545"), "order_id": "2",
 "title": "牛奶",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da92c327f8d1b360546"), "order_id": "2",
 "title": "酸奶",
 "price": 40, 

"num": 1 }] 

} 
```





```js
db.order.aggregate([ 

{
 $lookup: 

{ 

} }, 

{
 $project:{ trade_no:1, all_price:1,items:1 } 

}, { 

$match:{"all_price":{$gte:90}} }, 

{
 $sort:{"all_price":-1} 

from: "order_item", 

localField: "order_id", foreignField: "order_id", as: "items" 

}, ]) 

{
 "_id": ObjectId("5b743d8c2c327f8d1b360540"), "trade_no": "111",
 "all_price": 100,
 "items": [{ 

"_id": ObjectId("5b743d9c2c327f8d1b360543"), "order_id": "1",
 "title": "商品鼠标 1",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da12c327f8d1b360544"), "order_id": "1",
 "title": "商品键盘 2",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b74f457089f78dc8f0a4f3b"), "order_id": "1",
 "title": "商 品键盘 3",
 "price": 0, 

"num": 1 }] 

}{
 "_id": ObjectId("5b743d902c327f8d1b360541"), "trade_no": "222",
 "all_price": 90,
 "items": [{ 

"_id": ObjectId("5b743da52c327f8d1b360545"), "order_id": "2",
 "title": "牛奶",
 "price": 50, 

"num": 1 }, { 

"_id": ObjectId("5b743da92c327f8d1b360546"), "order_id": "2",
 "title": "酸奶",
 "price": 40, 

"num": 1 

}] }  
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





###一、mongoose 介绍 

Mongoose 是在 node.js 异步环境下对 mongodb 进行便捷操作的对象模型工具。Mongoose 是 NodeJS 的驱动，不能作为其他语言的驱动。 

**Mongoose** 有两个特点 1、通过关系型数据库的思想来设计非关系型数据库 2、基于 mongodb 驱动，简化操作 



###二、mongoose 的安装以及使用 

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

   

###mongoose 预定义模式修饰符 

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



###一、Mongoose 索引 

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



###一、Mongoose 校验参数 

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



####二、Mongoose 自定义的验证器 

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











