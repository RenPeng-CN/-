#[点击这里查看 Nginx 中文文档](http://www.nginx.cn/doc/general/overview.html)

Nginx的作者是俄罗斯人 Igor Sysoev  Email：[igor@sysoev.ru](/Users/renpeng/Library/Application Support/typora-user-images/19091A6E-46BB-4619-8508-430109B48F51/mailto:igor@sysoev.ru)

这里默认 安装 Nginx 的系统是 CentOS 7.5 版本



**1  安装Nginx 之前要提前装  gcc c++**

```js
yum -y install gcc automake autoconf libtool make    # 安装make

yum install gcc gcc-c++   #安装 gcc gcc- c++
```



**2  一般我们都需要先装pcre (为了重写rewrite ),  zlib (为了gzip压缩)**

**安装pcre cd /usr/local/src   #选定一个目录，以便把 pcre压缩文件下载到这里**

```js
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.37.tar.gz   #通过 wget 下载 pcre

tar -zxvf pcre-8.37.tar.gz  #解压缩

cd pcre-8.34    # 进入解压后的一个文件夹

./configure

make

make install
```



**3  安装 Zlib**

cd /usr/local/src   # 选定一个目录，以便把 Zlib 压缩文件下载到这里

```js
wget <http://zlib.net/zlib-1.2.8.tar.gz>   # 通过 wget 下载 zlib

tar -zxvf zlib-1.2.8.tar.gz     # 解压 zlib

cd zlib-1.2.8     #进入解压后产生的一个文件夹

./configure

make

make install
```





**4.**  **安装ssl（某些vps 默认没装ssl)**

cd /usr/local/src

```js
wget <https://www.openssl.org/source/openssl-1.0.1t.tar.gz>

tar -zxvf openssl-1.0.1t.tar.gz

cd XXXX

./configure

make

make install 
```





**5.** **通过yum安装nginx最新版, Nginx一般有两个版本，分别是稳定版和开发版，您可以根据您的目的来选择这两个版本的其中一个**

```js
1. nginx -v  #查看是否最新的 nginx版本，也看看是否安装过 nginx 
2. yum remove nginx  #如果已经安装过老版本的 nginx，就卸载掉
3. 在centos上建立一个文件，位置为：/etc/yum.repos.d/nginx.repo    在/etc/yum.repos.d 文件夹下建立  nginx.repo 文件。 将以下内容粘贴进该文件，此步骤很重要，可以使 centos 7 自动升级 nginx 到最新版本，[具体可以点击这里查看nginx官网的指导](http://nginx.org/en/linux_packages.html#stable)                              
```



[nginx]

name=nginx repo

baseurl=http://nginx.org/packages/centos/7/$basearch/

gpgcheck=0 

enabled=1

```js
1. yum list|grep nginx   #用 yum 命令查询一下 nginx 的 yum 源配置好了没有
2. sudo yum install epel-release   # 添加CentOS 7 EPEL仓库， EPEL 是 yum 的一个软件源,
3. ll /etc/yum.repos.d/  # 验证 epel 仓库
4. sudo yum search w3m  # 查看 安装 epel 后，是否有了 w3m
5. yum install nginx –y      #安装最新稳定版的 nginx
6. nginx -v    #安装后查看是否最新稳定版 nginx

安装成功后 nginx 在  /etc/nginx
```



**6.** **启动确保系统的80 端口没被其他程序占用，运行/usr/local/nginx/nginx命令来启动Nginx**

```js
netstat -ano | grep 80   # 查看80端口是否被占用

systemctl start nginx.service  # 开启nginx服务器
```



如果您正在运行防火墙，请运行以下命令以允许HTTP和HTTPS通信：

```js
sudo firewall-cmd --permanent --zone=public --add-service=http   #给防火墙 firewall 添加 http 通道

sudo firewall-cmd --permanent --zone=public —add-service=https   # 给防火墙 添加 https 通道  复制本命令注意 - -  不是 —

sudo firewall-cmd --reload 
```

重启防火墙后，打开浏览器访问服务器的 IP 地址 或 域名，如果浏览器出现 Welcome to nginx! 则表示 Nginx 已经安装并运行成功。







### Mac 上的 Nginx 操作

```js
brew search nginx   # 查询要安装的软件是否存在
brew install nginx  # 安装 nginx
brew upgrade nginx  # 升级 nginx
```

```js
open /usr/local/etc/nginx/  # 查看nginx安装目录（是否如info所说）：
```

```js
// mac上点击 访达 跳出窗口后 按 shift+苹果键+G  跳出路径小窗口，输入路径，直达目的地
open  /usr/local/var/www  # Docroot 
open  /usr/local/etc/nginx/nginx.conf # nginx 配置地址
open  /usr/local/etc/nginx/servers/ # nginx will load all files in
open /usr/local/Cellar/nginx  # 其实这个才是nginx被安装到的目录

//会看到一个以当前安装的nginx的版本号为名称的文件夹，这个就是我们安装的nginx根目录啦。进入1.12.2_1/bin 目录，会看到nginx的可执行启动文件。
　　//同样的，我们在1.12.2_1/目录下还可以看到一个名字为html的快捷方式文件夹（暂且就这么叫吧），进入该目录我们会发现其实它指向的就是/usr/local/var/www目录，这个在上面我们查看的info信息中有提到（Dcroot）
```

```js
nginx  # 启动 nginx 
brew services stop nginx  # 停止nginx服务
brew services restart nginx # 重启动nginx服务

nginx -h # 查看 nginx 搜索操作命令
nginx -t  # 查看nginx配置有木有生效
nginx -T  # 查看nginx所有的配置
```



##Nginx解决前端跨域(CORS)问题

**举个栗子**

例如我们在开发一个Vue应用。

**原先：**

调试页面是：http://192.168.1.100:8080/

请求的接口是：http://ni.hao.sao/api/get/info

**步骤一：**

请求的接口是：http://192.168.1.100:8080/api/get/info

PS：这样就解决了跨域问题。

**步骤二：**

安装好Nginx后，去到/usr/local/etc/nginx/目录（这是Mac的），修改nginx.conf文件。

**步骤三：**

把默认的server配置注释掉。

在下面增加：

```js
server{
        listen 8888;
        server_name  192.168.1.100;
 
        location /{
            proxy_pass http://192.168.1.100:8080;
        }
 
        location /api{
            proxy_pass http://ni.hao.sao/api;
        }
    }
```

















**Nginx****命令行参数**

**不像许多其他软件系统，****Nginx** **仅有几个命令行参数，完全通过配置文件来配置**

```js
systemctl start nginx.service  # 开启nginx服务器
systemctl enable nginx.service   # 使 nginx 服务器可以随着 CentOS 的启动而自动启动。
systemctl status nginx.service #查看 nginx 状态
systemctl stop nginx.service # 停止 nginx 服务器
systemctl restart nginx.service  # 重启 nginx
yum update nginx -y #完成nginx的更新升级
nginx -t  # 不运行，而仅仅测试配置文件是否有语法错误。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。

nginx - ?  nginx -?vVtq | nginx –s signal | nginx –c filename  | nginx –p prefix| nginx – g directives—+-     # nginx 语法

         -?, -h        : 打开帮助信息 

        -v             : 显示 nginx 版本信息并退出

        -V             : 显示版本和配置选项信息，编译器版本和配置参数。然后退出

         -q             : 在检测配置文件期间屏蔽非错误信息

         -s signal   : 给一个nginx主进程发送信号： stop（停止） ，quit（退出）reopen （重启） , reload （重新加载配置文件）

         -p prefix      : 设置前缀路径 （默认是：/usr/local/Cellar/nginx/1.2.6/）

        -c  </path/to/config>    #指定一个配置文件地址，来代替默认地址，不改也行 （默认是：/etc/nginx/nginx.conf）

       -g directives  : 设置配置文件外的全局指令
```





**Nginx停止，有三种方式**



1. **从容停止，简单型，先关闭进程，修改你的配置后，重启进程**

​      ps -ef | grep nginx   #查看 nginx 的进程号

​       ps aux | egrep ‘(PID|nginx)’   # 另一种方法查看 nginx 的进程号（ process status 进程状态 -aux 显示所有包含其他使用者的行程 ）

​       kill -QUIT 2032        # 从容停止 nginx 进程





1. **快读停止**

​        ps -ef | grep nginx    #查看 nginx 的进程号

​          kill -TERM 2032       # 快速停止 nginx 进程

​          Kill -INT 2032           # 这个也是快速停止 nginx 的命令



1. **强制停止**

​      pkill -9 nginx    # 强制停止 nginx 服务器









**Nginx** **的重启**

修改 nginx 配置文件后 需要重新启动 nginx，但重启之前，必须先检测 nginx 的配置是否有错误，所以

1.  nginx -t                         # 测试 nginx 是否有错，如果有就修改好
2.  ./nginx  -s reload          # 必须先进入到 nginx 可执行文件的目录地址，才可以执行此重启命令
3.  ps -ef | grep nginx        #查看 nginx 的进程号
4.  kill -HUP  2031             # 第二种方法重启 Nginx ，发送信号的方式重启当 nginx， 接收到 HUP 信号，它会尝试先解析配置文件（如果指定配置文件，就使用指定的，否则使用默认的），成功的话，就应用新的配置文件（例如：重新打开日志文件或监听的套接 字）。之后，nginx 运行新的工作进程并从容关闭旧的工作进程。通知工作进程关闭监听套接字但是继续为当前连接的客户提供服务。所有客户端的服务完成后，旧的工作进程被关闭。 如果新的配置文件应用失败，nginx 将继续使用旧的配置进行工作。



**3.**[平滑更新nginx二进制，不会停止处理请求](http://www.nginx.cn/nginxchscommandline#reload%20bin)



**Nginx** **常用目录**

```js
/usr/share/nginx/html  #Nginx默认站点目录

/etc/nginx/conf.d/default.conf    #Nginx 网站默认站点配置

/etc/nginx/    #自定义 Nginx 站点配置文件存放目录：
 /var/run/nginx.pid    #PID目录：
 /var/log/nginx/error.log     #错误日志：
 /var/log/nginx/access.log    #访问日志：
  /etc/nginx/conf.d/      #自定义 Nginx 站点配置文件存放目录

/nginx –c nginx.conf      #Nginx 启动

/etc/nginx/nginx.conf     #Nginx 全局配置
```







**如果是本地端服务器设置虚拟网址，访问自己服务器上的web页面**



在mac os 终端，创建虚拟主机，添加主机记录

在mac电脑上，设置一个新的域名，添加一个新的 dns 记录，让某一个主机名指向我们的IP地址



在mac终端，打开并编辑 hosts，用编辑器打开

atom /etc/hosts  #用编辑器，打开mac上的 hosts 配置



打开文件后，添加 一个虚拟的 ip地址，和随便写一个 域名

127.0.0.1   sciencdkids.cn

127.0.0.1   www.sciencdkids.cn

127.0.0.1   [php.sciencdkids.cn](http://php.sciencdkids.cn)















**Nginx** **的** **七个模块**



**1、主模块( Main Module )**

**2、事件模块（Events Module** ）

**3、Nginx标准HTTP模块（Standard HTTP Modules）**

**4、Nginx可选HTTP模块（Optional HTTP Modules ）**

**5、Nginx邮件模块(Mail Modules）**

**6、第三方模块（3rd Party Modules ）**

**7、Nginx部分优化 ( 哈希表与事件模型 )（ Nginx Optimizations ）**



**以下为完整实例**



\#!nginx



**# Nginx** **的主模块** ***\**\**\**\**\**\**\**\**\**\**\**\**\**\**\**\**\**\**\**\****

user nobody;     # nobody 为地权限的用户，更为安全

\# user  www www;            # 使用的用户和组

\# 如果主进程以root运行，Nginx将会调用setuid()/setgid()来设置用户/组，如果没有指定组，那么将使用与用户名相同的组，

\# 默认情况下会使用nobody用户与nobody组（或者nogroup），或者在编译时指定的--user=USER和--group=GROUP的值。

\# 使用的用户和组 语法：user xxx xxx;



worker_processes  auto;     # 指定 worker 的工作衍生进程数，通常应该为当前主机的 CPU 的物理核心数，也可以是 CPU 核数的两倍，最大或最小都不好，设置过多，反而会更慢。



\# worker_cpu_affinity 0001 0010 0100 1000;   # 0001 是将worker指定到第 0 个 cpu 上，0010 第 1个，0100 第 2个，1000第 3 个

worker_cpu_affinity 0001;

\# 指定每个进程到一个CPU：指定第一个进程到CPU0/CPU2，指定第二个进程到CPU1/CPU3，对于HTT处理器来说是一个不错的选择。



worker_priority 20; 

\# 这个选项可以用来设置所有工作进程的优先级（即linux系统中的nice），它调用setpriority()。 设定 worker 进程的优先级 [-20,20]







\# worker_rlimit_core 2;

\# 允许的每个 worker 进程核心文件最大值。





\# worker_rlimit_sigpending limit;

\# linux内核2.6.8以后，限制调用的 worker 进程中真实用户队列可能使用的信号数量。



\# working_directory path;

\# 程序的工作目录，一般只用来指定核心文件位置，Nginx仅使用绝对路径，所有在配置文件中的相对路径会转移到--prefix==PATH





\# daemon off;

\# 语法：daemon on | off 。生产环境中永远不要使用"daemon"和"master_process"指令，这些指令仅用于开发调试





\# master_process off;

\# 语法：master_process on | off ,生产环境中永远不要使用"daemon"和"master_process"指令，这些指令仅用于开发调试





\# env  MALLOC_OPTIONS;               # 语法：env VAR|VAR=VALUE; 这个命令允许其限定一些环境变量的值, 1.·在不停机情况下升级、增加或删除一些模块时继承的变量  2.使用嵌入式perl模块

\# env  PERL5LIB=/data/site/modules;  #

\# env  OPENSSL_ALLOW_PROXY_CERTS=1;  #





\# debug_points stop;

\# 语法：debug_points [stop | abort];  默认值：none（无）

\# 在Nginx内部有很多断言，如果debug_points的值设为stop时，那么触发断言时将停止Nginx并附加调试器。如果debug_point的值设为abort,那么触发断言时将创建内核文件。







error_log  /var/log/nginx/error.log warn;  # 错误日志配置 语法：error_log file [ debug | info | notice | warn | error | crit ]

\# 默认值：${prefix}/logs/error.log

\# 指定Nginx服务（与FastCGI）错误日志文件位置。

\# 每个字段的错误日志等级默认值  1、main字段 - error      2、HTTP字段 - crit      3、server字段 - crit

\# error_log LOGFILE [debug_core | debug_alloc | debug_mutex | debug_event | debug_http | debug_imap];

\#

\# 注意error_log off并不能关闭日志记录功能，而会将日志文件写入一个文件名为off的文件中，如果你想关闭错误日志记录功能，应使用以下配置：

\# error_log /dev/null crit;

\#

\# 同样注意0.7.53版本，nginx在使用配置文件指定的错误日志路径前将使用编译时指定的默认日志位置，如果运行nginx的用户对该位置没有写入权限，nginx将输出如下错误：

\# [alert]: could not open error log file: open() "/var/log/nginx/error.log" failed (13: Permission denied)





\# log_not_found off;

\# 语法: log_not_found  on | off ; 使用字段：location  这个参数指定了是否记录客户端的请求出现404错误的日志，通常用于不存在的robots.txt和favicon.ico文件





\# include vhosts/*.conf;

\# include   # 语法：include file | *  默认值：none  你可以包含一些其他的配置文件来完成你想要的功能。 0.4.4版本以后，include指令已经能够支持文件通配符：





\# lock_file  /var/log/lock_file;

\# 语法: lock_file file  Nginx使用连接互斥锁进行顺序的accept()系统调用，











pid /var/run/nginx.pid;    # 指定 pid 存放的路径

\# [ debug | info | notice | warn | error | crit ] 

\# 可以在下方直接使用 [ debug | info | notice | warn | error | crit ]  参数

\# pid文件就是一个纯文本文件，里面记录的是进程的pid号（ master process id ）指定 pid 存放的路径

\# 可以使用kill命令来发送相关信号，例如你如果想重新读取配置文件，则可以使用：kill -HUP `cat /var/log/nginx.pid`

\# openssl engine -t;

\# 这里可以指定你想使用的OpenSSL引擎，你可以使用这个命令找出哪个是可用的：openssl engine -t



\# timer_resolution  100ms;

\# 这个参数允许缩短gettimeofday()系统调用的时间，默认情况下gettimeofday()在下列都调用完成后才会被调用：kevent(), epoll, /dev/poll, select(), poll()。

\# 如果你需要一个比较准确的时间来记录$upstream_response_time或者$msec变量，你可能会用到timer_resolution







**# Nginx** **的事件模块** ***\**\**\**\**\**\**\**\**\**\**\**\**\**\**\**\**\**\**\**\***

events {



​    accept_mutex on;

  \# accept_mutex on | off;

  \# Nginx使用连接互斥锁进行顺序的accept()系统调用

  \# 处理新的连接请求的方法； on 指由各个 worker 轮流处理新请求， off 指每个新请求的到达都会通知（唤醒）所有的 worker 进程，

  \# 但只有一个进程可获得连接，造成 “惊群”，影响性能。



​    \# accept_mutex_delay 500ms;  # 如果一个进程没有互斥锁，它将至少在这个值的时间后被回收，默认是500ms



​    \# debug_connection 192.168.1.1;  # 0.3.54版本后，这个参数支持CIDR地址池格式。这个参数可以指定只记录由某个客户端IP产生的debug信息。当然你也可以指定多个参数。



​    \# multi_accept  off;   # multi_accept在Nginx接到一个新连接通知后调用accept()来接受尽量多的连接



​    \# rtsig_signo  ;  # Nginx在rtsig模式启用后使用两个信号，该指令指定第一个信号编号，第二个信号编号为第一个加1,  默认rtsig_signo的值为SIGRTMIN+10 (40)。



​    \#



​    use epoll;   #使用epoll的I/O 模型，指明并发连接请求的处理方法，默认自动选择最优方法，Nginx 比 Apache 的优势就是 epoll 

​    \# 语法：use [ kqueue | rtsig | epoll | /dev/poll | select | poll | eventport ]

​    \# 如果你在./configure的时候指定了不止一个事件模型，你可以通过这个参数告诉nginx你想使用哪一个事件模型，默认情况下nginx在编译时会检查最适合你系统的事件模型。

​    \# 你可以在这里看到所有可用的事件模型并且如果在./configure时激活它们。



​    \# 补充说明:



​    \#与apache相类，nginx针对不同的操作系统，有不同的事件模型



​    \#A）标准事件模型



​    \#Select、poll属于标准事件模型，如果当前系统不存在更有效的方法，nginx会选择select或poll



​    \#B）高效事件模型



​    \#Kqueue：使用于FreeBSD 4.1+, OpenBSD 2.9+, NetBSD 2.0 和 MacOS X.使用双处理器的MacOS #X系统使用kqueue可能会造成内核崩溃。



​    \#Epoll:使用于Linux内核2.6版本及以后的系统。



​    \#/dev/poll：使用于Solaris 7 11/99+, HP/UX 11.22+ (eventport), IRIX 6.5.15+ 和 Tru64 UNIX 5.1A+。



​    \#Eventport：使用于Solaris 10. 为了防止出现内核崩溃的问题， 有必要安装安全补丁



​    worker_connections  15000;   # 每个 worker 进程所能够打开的最大并发连接数。

​    \# use [ kqueue | rtsig | epoll | /dev/poll | select | poll ] ;

​    \# 具体内容查看 http://wiki.codemongers.com/事件模型

​    \#工作进程的最大连接数量，根据硬件调整，和前面工作进程配合起来用，尽量大，但是别把cpu跑到100%就行

​    \#每个进程允许的最多连接数， 理论上每台nginx服务器的最大连接数为worker_processes * worker_connections





}







**# Nginx** **标准** **HTTP****模块，跟网页先关*****\**\**\**\**\**\**\**\**\**\**\***



http {



gzip on;   #该指令用于开启或关闭gzip模块(on/off)

gzip_buffers 16 16k;  # 申请内存空间为 16个 16k 的数据流。设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。16 8k代表以8k为单位，安装原始数据大小以8k为单位的16倍申请内存

gzip_comp_level 6;  #gzip压缩比，数值范围是1-9，1压缩比最小但处理速度最快，9压缩比最大但处理速度最慢

gzip_http_version 1.1;   # gzip 识别http的协议版本为 1.1 版本

gzip_min_length 1k;  # 低于 1k 不压缩。设置允许压缩的页面最小字节数，页面字节数从header头得content-length中进行获取。默认值是0，不管页面多大都压缩。

 gzip_proxied any;  #这里设置无论header头是怎么样，都是无条件启用压缩

 gzip_vary on;  # 开启判断客户端的浏览器是否支持 gzip 压缩技术，如果支持就使用该技术，如果不支持就不使用。 在http header中添加Vary: Accept-Encoding ,给代理服务器用的



 gzip_types   #进行压缩的文件类型,这里特别添加了对字体的文件类型

​        text/xml application/xml application/atom+xml application/rss+xml application/xhtml+xml image/svg+xml

​        text/javascript application/javascript application/x-javascript

​        text/x-json application/json application/x-web-app-manifest+json

​        text/css text/plain text/x-component

​        font/opentype font/ttf application/x-font-ttf application/vnd.ms-fontobject

​        image/x-icon;





​    gzip_disable "MSIE [1-6]\.(?!.*SV1)";  #禁用IE 6 gzip





include       conf/mime.types;

default_type  application/octet-stream;



log_format  main  '$remote_addr - $remote_user [$time_local] - "$request" -'

​                      '$status $body_bytes_sent - "$http_referer" -'

​                      '"$http_user_agent" - “$http_x_forwarded_for"'-'"$gzip_ratio"';







log_format download  '$remote_addr - $remote_user [$time_local]  '

'"$request" $status $bytes_sent '

'"$http_referer" "$http_user_agent" '

'"$http_range" "$sent_http_content_range"';



client_header_timeout  3m;

client_body_timeout    3m;

send_timeout           3m;



client_header_buffer_size    1k;

large_client_header_buffers  4 4k;





output_buffers   1 32k;

postpone_output  1460;



  \# access_log off;  #关闭日志记录

​    access_log  /var/log/nginx/access.log  main;   #用户访问日志存储地址



\# keepalive_timeout  65;

​    \# 设定保持连接超时时间，0表示禁止长连接，默认为75s，75s过长，应该改短。



​    \# keepalive_request 100;

​    \# 在一次长连接上所允许请求的资源的最大数量，默认为100.



​    \# 	keepalive_disable none | browser ...;

​    \# 对哪种浏览器禁用长连接



  \# send_timeout time;

​    \# 向客户端发送响应报文的超时时长，此处是指两次写操作之间的间隔时长，而非整个响应过程的传输时长。

​    \# sendfile        on;

​    \# 是否启用 sendfile 功能，在内核中封装报文直接发送，速度更快，默认 off





  \# tcp_nodelay on;

​    \# 在 keepalived 模式下的连接是否启用 TCP_NODELAY 选项，

​    \# 当为 off 时，延迟发送，合并多个请求后再一起发送

​    \# 默认 On 时，不延迟发送

​    \# 可用于：http, server, location



​    \# tcp_nopush     on;



send_lowat       12000;



  include /etc/nginx/conf.d/*.conf;



\# keepalive_timeout  65;

\# 设定保持连接超时时间，0表示禁止长连接，默认为75s，75s过长，应该改短。



\#lingering_time     30;

\#lingering_timeout  10;

\#reset_timedout_connection  on;



\#server_tokens off;

​    \# 是否在响应报文的Server首部显示nginx的版本



​    \# client_body_buffer_size size;

​    \# 用于接收每个客户端请求报文的 body 部分的缓冲区大小；默认为16k；超出此大小时，

​    \# 其将被暂存到磁盘上的由下面 client_body_temp_path 指令所定义的位置



​    \# client_body_temp_path path [level1 [level2 [level3]]];

​    \# 设定存储客户端请求报文的body部分的临时存储路径及子目录结构和数量目录名为16进制的数字；

​    \# client_body_temp_path /var/tmp/client_body 1 2 2

​    \# 1 是 1级目录占1位16进制，即2^4=16个目录 0-f

​    \# 2 是 2级目录占2位16进制，即2^8=256个目录 00-ff

​    \# 2 是 3级目录占2位16进制，即2^8=256个目录 00-ff



​    \# 	limit_rate rate;

​    \# 限制响应给客户端的传输速率，单位是 bytes/second,默认值0表示无限制





​    \# aio on | off | threads[=pool];

​    \# 是否启用 aio 功能， 异步功能





​    \# directio size | off;

​    \#	当文件大于等于给定大小时，例如 directio 4m , 同步（直接）写入磁盘，而非写入缓存



​    \# open_file_cache off;

​    \# open_file_cache max=N [inactive=time];

​    \# nginx 可以缓存一下三种信息：

​    \# 1. 文件元数据：文件的描述、文件的大小和最近一次的修改时间

​    \# 2. 打开的目录结构

​    \# 3. 没有找到的或者没有权限访问的文件的相关信息

​    \# max = N： 可缓存的缓存项上限；达到上限后会使用LRU算法实现管理

​    \# inactive=time：缓存项的非活动时长，在此处指定的时长内未被命中的或命中的次数少于 open_file_cache_min_uses指令所指定的次数的缓存项即为非活动项，将被删除



  \#通过upstream 来实现 nginx 的负载均衡

​    \#upstream  B2C_WEB {

​     \#ip_hash;     #让同一用户每次都访问同一个服务器，目的保证客户端的ip地址不变，请求就只送到固定的一个服务器上



​     \#server 192.168.126.1:8080 weight=1 max_fails=0 fail_timeout=10s;

​     \#server 192.168.126.15:8080 weight=1 max_fails=0 fail_timeout=10s;



​     \#weight=3; 表示相关服务器的权重，权重的数值越大，被分配的客户就越多，反之亦然。

​     \#表示如果服务器192.168.126.1 | 15在30秒内出现了3次错误，

​     \#那么就认为这个服务器工作不正常，从而在接下来的30秒内nginx不再去访问这个服务器。

​    \#}







server {

listen        one.example.com;

server_name   one.example.com  www.one.example.com;



access_log   /var/log/nginx.access_log  main;



location / {

proxy_pass         http://127.0.0.1/;

proxy_redirect     off;



proxy_set_header   Host             $host;

proxy_set_header   X-Real-IP        $remote_addr;

\#proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;



client_max_body_size       10m;

client_body_buffer_size    128k;



client_body_temp_path      /var/nginx/client_body_temp;



proxy_connect_timeout      90;

proxy_send_timeout         90;

proxy_read_timeout         90;

proxy_send_lowat           12000;



proxy_buffer_size          4k;

proxy_buffers              4 32k;

proxy_busy_buffers_size    64k;

proxy_temp_file_write_size 64k;



proxy_temp_path            /var/nginx/proxy_temp;



charset  gb2312;   # 设置字符为 gb2312

}



error_page  404  /404.html;



location /404.html {

root  /spool/www;



charset         on;

source_charset  koi8-r;

 }



location /old_stuff/ {

rewrite   ^/old_stuff/(.*)$  /new_stuff/$1  permanent;

}



location /download/ {



valid_referers  none  blocked  server_names  *.example.com;



if ($invalid_referer) {

\#rewrite   ^/   http://www.example.com/;

return   403;

}



\#rewrite_log  on;



\# rewrite /download/*/mp3/*.any_ext to /download/*/mp3/*.mp3

rewrite ^/(download/.*)/mp3/(.*)\..*$

/$1/mp3/$2.mp3                   break;



root         /spool/www;

\#autoindex    on;

access_log   /var/log/nginx-download.access_log  download;

}



location ~* ^.+\.(jpg|jpeg|gif)$ {

root         /spool/www;

access_log   off;

expires      30d;

}

}

: }











**Nginx** **配置多台虚拟主机**

Ifconfig     # 在centos 输入 ifconfig，查看端口信息，看 eth0  以太网端口的 IP 地址

Ifconfig eth0 192.168.1.9  netmask 255.255.255.0     # 给 eth0 以太网端口配置新的 IP 地址和 子网掩码

Ifconfig    # 重新查看一下 IP 地址是否配置成功

ifconfig eth0:1 192.168.1.7   broadcast 192.168.1.255  netmask 255.255.255.0  # 配置第一个虚拟主机的IP地址

ifconfig  # 重新查看一下 第一个 虚拟主机是否配置成功

ifconfig eth0:2 192.168.1.8   broadcast 192.168.1.255  netmask 255.255.255.0  # 配置第二个虚拟主机的IP地址

ifconfig  # 重新查看一下 第二个 虚拟主机是否配置成功



 *#导入外部服务器配置文件存放地址*
include /usr/local/nginx/conf/vhosts/*.conf;



然后在 Nginx 的配置文件里 设置服务器

\# 虚拟主机1

server {

​	listen 192.168.1.7:80;

​              server_name 192.168.1.7

​              access_log logs/server1.access.log combined;   # combined 是使用此日志文件的默认格式

​              location / {

​                                index  index.html  index.htm;    # 设置默认首页

​                                root html/server1;    # 设定 server1 绑定到那个目录下

​               }



}





\# 虚拟主机2

server {

​	listen 192.168.1.8:80;

​              server_name 192.168.1.8

​              access_log logs/server2.access.log combined;   # combined 是使用此日志文件的默认格式

​              location / {

​                                index  index.html  index.htm;    # 设置默认首页

​                                root html/server2;    # 设定 server1 绑定到那个目录下

​               }



}











mkdir server1   # /usr/share/nginx/html  # 在 Nginx 默认站点目录 下面新建 server1 文件夹

cd server1  # 进入server1文件

touch index.html   #新建 server1 的首页



在浏览器地址栏输入 192.168.1.7   看看是否能打开刚配置的虚拟机首页











**Nginx** **日志文件的配置**



http{

​        log_format combined;  # combined 是 日志 默认的格式。

​                                              \# $remote_addr   # 是客户端的 IP 地址

​                                              \# $remote_user   # 是客户端的 用户名

​                                              \# $time_local       # 是客户端访问的时间

 			   # $request           # 是客户访问的具体页面地址 url

​			   # $status              # 是请求状态的意思，是用户正在请求中，还是已经请求完成了

​			   # $body_bytes_sent   #返回给客户文件的大小

​			   # $http_referer      #是原网页，就是记录客户是从哪里访问过来的

​			   # $http_user_agent    # 是客户端浏览器的信息

​			   # $http_x_forwarded_for   #是客户端的 IP 地址，跟 $remote_addr 类似

}











**Nginx** **日志文件按照日期自动切割，以防止单个文件过大**



**手动切割日志的方法**

mv access.log 20181213.log  # 移动日志文件到一个新的文件里

ps -ef | grep nginx   # 查询 nginx 的进程号

kill -USR1 2514   # 给 2514进程号 发送一个 切割日志的信号，这是就进行了切割，也同时新开了一个日志





**自动切割日志的方法**



touch cutlog.sh  # 在 var/log/nginx/下面建立 批处理文件, 用于存储切割命令，按时执行自动切割日志的命令



打开 cutlog.sh 文件，输入以下命令

D=$( date +%Y%m%d)  # 获取系统时间，规定好格式，赋值给变量 D

mv /var/log/nginx/access.log  ${ D }.log    # 移动当前日志文件到 以当天日期为名称的log文件里 

kill -USR1 $(cat /var/run/nginx.pid)            # $(cat /var/run/nginx.pid) 是 nginx 的进程号



crontab -e   # cromtab 就是定时执行的命令，打开后将下面一样内容放入其中

23 59 *** /bin/bash /var/log/nginx/cutlog.sh    # 这是 要放入crontab里的内容，23 59 代表 23:59分，*** 是格式，/bin/bash/是格式，后面的  就是要执行的文件的地址



然后保存退出即可













**Nginx** **的缓存配置**



在 nginx.conf 文件中配置

server（

 location ~.*\.(jpg| png| swf)${     #  ~  表示文件路径，什么路径都认可的意思。*\ 表示什么文件名都可以，jpg…表示文件格式

  expires   2d;               # 2 天到期后自动清除

  }  

 ）



实例：

  \# 图片文件，字体文件，js和css都是些可以用来缓存的文件，这里通过设置Expires和Cache-Control头实现，直接在配置文件中配置location即可

​       location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|ico)$

​       {

​            expires 30d; #释放 gif|jpg|jpeg|png|bmp|swf|flv|ico 缓存文件期限为 30天

​            access_log off;

​        }

   

​       location ~ .*\.(eot|ttf|otf|woff|svg)$

​        {

​             expires 30d;  #释放 eot|ttf|otf|woff|svg 缓存文件期限为 30天

​             access_log off;

​         }





​         location ~ .*\.(js|css)?$

​         {

​              expires 7d;  #释放 js css 缓存文件期限为 7天

​              access_log off;

​          }











**Nginx** **的压缩技术，把客户请求的，大于** **1k** **的文件压缩到****30%****，传输给客户端**

gzip on;   #该指令用于开启或关闭gzip模块(on/off)



​    gzip_buffers 16 16k;  # 申请内存空间为 16个 16k 的数据流。设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。16 8k代表以8k为单位，安装原始数据大小以8k为单位的16倍申请内存



​    gzip_comp_level 6;  #gzip压缩比，数值范围是1-9，1压缩比最小但处理速度最快，9压缩比最大但处理速度最慢

​    gzip_http_version 1.1;   # gzip 识别http的协议版本为 1.1 版本

​    gzip_min_length 1k;  # 低于 1k 不压缩。设置允许压缩的页面最小字节数，页面字节数从header头得content-length中进行获取。默认值是0，不管页面多大都压缩。

​    gzip_proxied any;  #这里设置无论header头是怎么样，都是无条件启用压缩

​    gzip_vary on;  # 开启判断客户端的浏览器是否支持 gzip 压缩技术，如果支持就使用该技术，如果不支持就不使用。 在http header中添加Vary: Accept-Encoding ,给代理服务器用的



​    gzip_types   #进行压缩的文件类型,这里特别添加了对字体的文件类型

​        text/xml application/xml application/atom+xml application/rss+xml application/xhtml+xml image/svg+xml

​        text/javascript application/javascript application/x-javascript

​        text/x-json application/json application/x-web-app-manifest+json

​        text/css text/plain text/x-component

​        font/opentype font/ttf application/x-font-ttf application/vnd.ms-fontobject

​        image/x-icon;





​    gzip_disable "MSIE [1-6]\.(?!.*SV1)";  #禁用IE 6 gzip











**Nginx** **的缓存配置** **- Nginx** **的其它配置** **-** **自动列目录配置**

autoindex on;  # 开启自动链目录 







**Nginx** **的反向代理与负载均衡**



user nobody;

worker_processes 4;

events{

​            worker_connections 1024;

}



http{

​         Upstream mypro {

​                 ip_hash;  # 让同一个用户每次都可以访问同一个服务器

​         	   server 182.18.79.243;

​                 Server 140.205.140.234;

​         }



​         Server {

​	 listen 8080; 

​               location / {

​		proxy_pass http://mypro;

​               }

​          }



}





**HTTP Upstream** **模块**









**Nginx** **在****CentOS****里的常用命令**

nginx -h  # 查看帮助

nginx -t   # 测试

nginx      # 启动nginx

ss -ntl     # 查看端口

ss -ntlp   # 查 看端口

ps axo pid,cmd,psr | grep nginx     # 查看nginx 的worker 工作在哪个 CPU 上









**在****CentOS****上进行压力测试**



在 CentOS 上 安装 yum install httpd-tools，Apache 有压力测试工具，所以先安装apache

yum install httpd-tools # 安装 httpd-tools 工具



然后用 httpd-tools 下的 watch 工具来查看

watch -n 0.5 ‘ps axo pid,cmd,psr | grep nginx’   # 最好手动输入此行，’ 符号容易出错，每0.5秒观察一次 nginx 



然后另外开一个终端窗口执行 ab 命令，加压访问 nginx 

ab -c 1000 -n 20000 <http://123.206.30.43/>    #Apache Benchmark( Apache 用基准问题测试计算机系统  简称ab) 是Apache安装包中自带的压力测试工具 ，简单易用











pstree -p   #在CentOS终端，显示进程目录树

tree nginx   # 查看 nginx 目录的目录树

curl -i [www.jylpcn.cn](http://www.jylpcn.cn)   # 查看 网站 头报文信息











**location**



= : 对 URL做精确匹配

​       location = / {

​         …..

​        }



^~ :  对 URL 的最左边部分做匹配检查，不区分字符大小

~ ：对 URL 做正则表达式模式匹配，区分字符大小写

~* ：对URL做正则表达式模式匹配，不区分字符大小写

不带符号： 匹配启示于此 URL 的所有 URL



匹配优先级从高到低： =，^~, ~/ ~*, 不带符号







Nginx 服务器压力测试，用 Apache 的 ab 工具，安装好后进行测试

ab -n1000 -c10 <http://192.168.31.181:8888/>    #-n 是 发出一千个请求，-c10是并发数是10个请求，url结尾要有/ 斜线。





![image-20190127004311047](/Users/renpeng/Library/Application Support/typora-user-images/image-20190127004311047.png)