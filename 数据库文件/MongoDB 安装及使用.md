[TOC]

### 新建项目，并设置 MongoDB ( Linux OS 上 进行操作 )

linux 安装 MongoDB 源

```js
// 1.配置MongoDB的yum源, 在 etc/yum.yepos.d / 下建立文件 mongodb-org
vim /etc/yum.repos.d/mongodb-org-3.4.repo

// vi 编辑这个空文件，把以下内容粘贴进去并保存退出
[mongodb-org-4.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.2.asc

#这里可以修改 gpgcheck=0, 省去gpg验证
[root@localhost ~]# yum makecache      
```



### Centos7下yum安装mongodb

https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/

```js
yum -y install mongodb-org // 安装 mongodb
whereis mongod // mongodb 安装的位置
vim /etc/mongod.conf // 查看配置文件
systemctl start mongod.service // 启动 mongodb
systemctl stop mongod.service  // 停止 mongodb
systemctl restart mongod.service // 重启 mongodb
systemctl status mongod.service // 查看状态
systemctl enable mongod.service // 随机启动

// 外网访问需要关闭防火墙？为什么不在防火墙开端口呢？
systemctl stop firewalld.service #停止firewall
systemctl disable firewalld.service #禁止firewall开机启动

mongo // 启动
show dbs // 查看数据库
exit // 退出 mongodb
vim /etc/mongod.conf //设置mongodb远程访问 编辑mongod.conf注释bindIp,并重启mongodb.(这句配置代表只能本机使用，所以需注释)
systemctl restart mongod.service // 重启mongodb使修改生效：

```



二、卸载*

*1、停止服务*

　　*service mongod stop*

*2、删除安装的包*

　　*yum erase $(rpm -qa | grep mongodb-org)*

*3、删除数据及日志*

　　*rm -r /var/log/mongodb*

　　*rm -r /var/lib/mongo*



```js
1. mkdir myproject  # 新建一个项目文件夹
2. cd myproject   # 进入该文件夹
3. npm init   #  npm 初始化该项目
4. npm install mongodb --save   # 安装 mongodb 的模块
5. mongod --dbpath=/data  #  配置数据存储路径

// 导出 备份 数据库 案例 -h host地址   -d 数据库名称  -o 输出地址
mongodump -h 127.0.0.1 -d vue_koa_demo -o /Volumes/RenPeng/Code/cms_demo/mongoDB

// 先将备份文件上传到服务器的一个地址，比如说 /opt/XXX
// 恢复 数据库 案例 -h host地址   -d 数据库名称  /数据库位置/
mongorestore -h 127.0.0.1 -d vue_koa_demo /opt/vue_koa_demo

```



###初次安装并启动 MongoDB，可观察到数据库报了4个警告

https://blog.csdn.net/vkingnew/article/details/81703707

可观察到数据库报了4个警告： 
1. WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine 
2. WARNING: Access control is not enabled for the database. 
3. WARNING: /sys/kernel/mm/transparent_hugepage/enabled is ‘always’. 
4. WARNING: /sys/kernel/mm/transparent_hugepage/defrag is ‘always’.



###下面来一一解决：

####1.WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine（强烈建议使用带WiredTiger存储引擎的XFS文件系统）
报这个错是因为我的虚拟环境使用的是EXT4文件系统，官方不建议，但不影响使用

在Linux上运行MongoDB时，官方建议使用Linux内核版本2.6.36或更高版本，使用XFS或EXT4文件系统。 如果可能，最好使用XFS，因为它通常与MongoDB表现更好。

使用WiredTiger存储引擎，强烈建议使用XFS，以避免在使用EXT4与WiredTiger时可能发生的性能问题。

使用MMAPv1存储引擎，MongoDB在使用它们之前预先分配其数据库文件，并经常创建大文件。 因此，官方建议使用XFS或EXT4文件系统。 如果可能，请使用XFS，因为它通常与MongoDB表现更好。

参考Kernel and File Systems



####2.WARNING: Access control is not enabled for the database.（数据库未启用访问控制）
此告警信息是说MongoDB需要有一个安全库来开启数据库访问控制，默认安装的启动的是mongod --dbpath /var/lib/mongo  此时无须用户认证登录的信息。

####解决办法：

在MongoDB部署上启用访问控制会强制执行身份验证，要求用户识别自己。当访问启用了访问控制的MongoDB部署时，用户只能执行由其角色确定的操作。

```js
mongod --dbpath /var/lib/mongo // 指定 数据库文件位置
mongo // 访问数据库
show dbs // 显示所有数据库
use admin // 使用 admin 数据库

// 创建用户和密码均为root，具有全部的权限。
db.createUser({user:"root",pwd:"root",roles: [{role:"userAdminAnyDatabase", db: "admin" } ]})

exit // 退出数据库
//重启mongoDB 使用用户名和密码登录：
systemctl stop mongod
systemctl restart mongod.service // 重启mongoDB 使用用户名和密码登录：
mongod --auth --dbpath /var/lib/mongo

// 用户名密码登录
mongo --username root --password root --port 27017 --authenticationDatabase admin

```



另外，可以测试，创建新的数据库，并在其中创建用户 

```js
use test // 创建新的数据库 test
db.createUser(
...   {
...     user: "myTester",
...     pwd: "xyz123",
...     roles: [ { role: "readWrite", db: "test" },
...              { role: "read", db: "reporting" } ]
...   }
... )


mongo --port 27017 -u "myTester" -p "xyz123" --authenticationDatabase "test"
```



3.WARNING: /sys/kernel/mm/transparent_hugepage/enabled is ‘always’.与4.WARNING: /sys/kernel/mm/transparent_hugepage/defrag is ‘always’.
这两个问题是CentOS7特有的，因为从CentOS7版本开始会默认启用Transparent Huge Pages(THP) 
Transparent Huge Pages(THP)本意是用来提升内存性能，但某些数据库厂商还是建议直接关闭THP(比如说Oracle、MariaDB、MongoDB等)，否则可能会导致性能出现下降。

- 查看THP状态

```js
[root@localhost ~]#  cat /sys/kernel/mm/transparent_hugepage/defrag
[always] madvise never
[root@localhost ~]# cat /sys/kernel/mm/transparent_hugepage/enabled
[always] madvise never

```



- 修改系统配置

```js
[root@localhost ~]# vim /etc/rc.d/rc.local

#!/bin/bash
# THIS FILE IS ADDED FOR COMPATIBILITY PURPOSES
#
# It is highly advisable to create own systemd services or udev rules
# to run scripts during boot instead of using this file.
#
# In contrast to previous versions due to parallel execution during boot
# this script will NOT be run after all other services.
#
# Please note that you must run 'chmod +x /etc/rc.d/rc.local' to ensure
# that this script will be executed during boot.

 // 修改系统配置加入如下配置重启操作系统：
touch /var/lock/subsys/local

if test -f /sys/kernel/mm/transparent_hugepage/enabled; then
 echo never > /sys/kernel/mm/transparent_hugepage/enabled
 fi
 if test -f /sys/kernel/mm/transparent_hugepage/defrag; then
 echo never > /sys/kernel/mm/transparent_hugepage/defrag
 fi


[root@localhost ~]# chmod +x /etc/rc.d/rc.local

```



重启CentOS

reboot   或者 init6

```js
mongod --auth --dbpath /var/lib/mongo
mongo // 进入数据库
show dbs // 显示所有数据库
```



修改数据库默认 ip

```js
vi /etc/mongod.conf  // 打开mongo 的默认配置文件

// 将 bindIp 后面的地址换成 服务器的 IP 地址
port:27017  // 默认端口还是27017
bindIp:1230206.30.43  
```



修改数据库文件目录 // 如果需要修改，就在这里修改，默认数据文件路径为/var/lib/mongo，这里我没做修改，有需要的可以按业务需求自行创建相应数据文件路径

```js
[root@localhost ~]# vim /etc/mongod.conf

# Where and how to store data.
storage:
  dbPath: /var/lib/mongo
  journal:
    enabled: true
```

重新mongodb 服务

```js
systemctl restart mongod.service 
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




