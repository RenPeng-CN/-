[TOC]



#LEMN 服务器搭建

###Linux + Nginx + MongoDB or MairaDB + Node + PM2 + HTTPS



```js
journalctl -xe // 查看日志
```



###第一步：安装 **Cent OS** 

云服务器: 在腾讯云 手动安装 Cent OS 最新版

自己下载：制作最新版CentOS系统安装U盘，并安装好CentOS系统，连接好网路（如果是局域网服务器，最好配置好静态获取IP），进行下列操作



1. **<font color="red">sudo yum update -y</font> ** #安装好CentOS 后，升级一下yum( Yellow dog Updater Modified )是一个在Fedora和RedHat以及SUSE中的Shell前端软件包管理器资源包（ sudo= super user do）
2. **<font color="red">pwd</font> ** # Print Working Directory  打印工作目录
3. **<font color="red">ls</font> **    # list 列出当前目录下的所有文件
4. **<font color="red">cat /etc/redhat-release</font> **   #concatenate 连接   查看CentOS 版本信息
5.  **<font color="red">httpd –v</font> **  #version  (httpd是Apache[超文本传输协议](https://baike.baidu.com/item/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE/8535513)(HTTP)服务器的主程序。被设计为一个独立运行的后台进程，它会建立一个处理请求的子进程或线程的池。查看是否安装了 Apache 服务器)
6. **<font color="red">rpm –qa | grep httpd</font> **  # 查看是否安装了 Apache 服务器 ( RedHat Package Management)
6. **<font color="red">yum erase httpd.x86_64</font> **  # 卸载 httpd apache 服务器

​             rpm 即 RedHat Package Management

​             q 使用询问模式，当遇到任何问题时，rpm指令会先询问用户。

​             -a 查询所有套件

​             grep 即 Globally Search a Regular Expression and print 用正则表达全局搜索，并打印出匹配的行

1. **<font color="red">sudo service mysqld start</font> **  #检查是否安装了 mysql. 
2. **<font color="red">yum remove mysql </font> ** #卸载掉默认安装的mysql        rm –f /etc/my.cnf
3. **<font color="red">-yum –y install gcc-c++</font> **  #安装nginx依赖环境Gcc，因为nginx也是C语言写的  GCC（GNU Compiler Collection )，是由 GNU 开发的编程语言编译器，支持C语言和C++
4. **<font color="red">yum install –y pcre pcre-devel</font> **   # 安装 pcre       PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。     nginx 的 http模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，      pcre-devel 是使用 pcre 开发的一个二次开发库。nginx也需要此库。
5. **<font color="red">yum install –y zlib zlib-devel</font> ** #安装zlib  zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip ，所以需要在 Centos 上安装 zlib 库。
6. **<font color="red">yum install –y openssl openssl-devel</font> **  #安装 OpenSSL     OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http），所以需要在 Centos 安装 OpenSSL 库。
7. **<font color="red">unman -r </font> **#查询Linux 内核版本
8. **<font color="red">uname -a </font> **# 查询 Linux centos 具体参数
9. **<font color="red">top </font> ** # 查询服务器CPU使用情况









**Cent Os 7** **防火墙** **配置**

**<font color="red">systemctl start firewalld.service</font> **  # 启动一个服务：

**<font color="red">systemctl stop firewalld.service</font> **  # 关闭一个服务：

**<font color="red">systemctl restart firewalld.service</font> **  # 重启一个服务：

**<font color="red">systemctl status firewalld.service</font> **  # 显示一个服务的状态：

**<font color="red">systemctl enable firewalld.service</font> **  # 在开机时自动启用防火墙服务：

**<font color="red">systemctl disable firewalld.service</font> **  # 在开机时禁用防火墙服务：

**<font color="red">systemctl is-enabled firewalld</font> **  # 查询防火墙服务是否开机启动：

**<font color="red">systemctl is-enabled firewalld.service;echo $?</font> **  # 查看服务是否开机启动：

**<font color="red">systemctl list-unit-files|grep enabled</font> **  # 查看已启动的服务列表：

**<font color="red">systemctl —failed</font> **  # 查询启动失败的服务列表：

**<font color="red">firewall-cmd --zone=public --add-port=3306/tcp --permanent</font> **  # 在安装软件或列库时，除了直接开启和关闭防火墙，也可以通过对端口的操作直接开放连接；添加端口

**<font color="red">semanage port -a -t http_port_t -p tcp 3003</font> **  # 声明端口类型，否则有可能不被防火墙通过

**<font color="red">firewall-cmd --reload</font> **  # 更新防火墙规则

**<font color="red">firewall-cmd --zone=public --query-port=80/tcp</font> **  # 查看端口状态：

**<font color="red">firewall-cmd --zone=public --remove-port=80/tcp —permanent</font> **  # 删除开放的端口：

**<font color="red">firewall-cmd --reload</font> ** # 每次都更新防火墙规则，都需要重新更新：

**<font color="red">firewall-cmd --zone=public --list-ports</font> **  #此外，在更新完防火墙的设置后，也可以查看所有开启的端口：

**<font color="red">firewall-cmd --query-port=80/tcp</font>** # 查看80端口是否开启， 如果是华为云服务器，默认关闭 80 端口，需要单独开启

**<font color="red">firewall-cmd --permanent --add-port=80/tcp</font>** # 在防火墙添加 80 端口

**<font color="red">firewall-cmd --reload</font> **  # 重启防火墙





**Linux centos 重启命令：**

　　1、reboot

　　2、shutdown -r now #立刻重启(root用户使用)

　　3、shutdown -r 10  #过10分钟自动重启(root用户使用)

　　4、shutdown -r 20:35 #在时间为20:35时候重启(root用户使用)

​        \5.   Init 6

　　如果是通过shutdown命令设置重启的话，可以用shutdown -c命令取消重启



　**Linux centos****关机命令：**

　　1、halt # 立刻关机

　　2、poweroff  # 立刻关机

　　3、shutdown -h now # 立刻关机(root用户使用)

　　4、shutdown -h 10 # 10分钟后自动关机

​       \5.    Init 0 





[**CentOS**](http://www.linuxidc.com/topicnews.aspx?tid=14) **之** **SSH** **的安装与配置**



SSH 为 Secure Shell 的缩写，由 IETF 的网络工作小组（Network Working Group）所制定SSH 为建立在应用层和传输层基础上的安全协议。传统的网络服务程序，如FTP、POP和Telnet其本质上都是不安全的。因为它们在网络上用明文传送数据、用户帐号和用户口令，很容易受到中间人（man-in-the-middle）攻击方式的攻击。存在另一个人或者一台机器冒充真正的服务器接收用户传给服务器的数据，然后再冒充用户把数据传给真正的服务器。而 SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。透过 SSH 可以对所有传输的数据进行加密，也能够防止 DNS 欺骗和 IP 欺骗。





1. yum install ssh # 安装ssh  
2. systemctl start sshd.service  # 启动 ssh
3. systemctl enable sshd.service # 使 ssh 可以随着centos的启动而启动，开机启动。
4. systemctl status sshd.service # 查看 ssh 当前状态，如果远程连接不上centos，就查看状态里的log记录
5. systemctl stop sshd.service  # 停止 ssh
6. systemctl restart sshd.service  # 重启 ssh
7. systemctl reload sshd.service # 重启









SSH相关配置文件的修改. 首先修改SSH的配置文件，用vim打开SSH的配置文件，如下：

vim /etc/ssh/sshd_config # 可以用 ftp 软件 找到并打开此文件进行配置，可以百度 ssh_config 配置  关键词来查询配置



**如果新安装的** **CentOS 7**  **连接不上** **SSH**

1. yum list installed | grep openssh-server  # 查看系统是否安装了 ssh 的服务器端软件

\2.  yum install openssh-server  # 如果没有安装，就安装一个， 如果已经安装了，忽略此行



  netstat -antp | grep sshd  #查看是否启动22端口（可略）。

  netstat -an | grep 22 #查看22端口的情况

  Ifconfig #查看网络连接地址

  打开此地址的文件进行编辑：vim /etc/ssh/sshd_config



要解决此问题，请进行如下配置检查：

​	通过 SSH 客户端或 [管理终端](https://help.aliyun.com/document_detail/25433.html) 登录服务器。

​	通过 cat 等指令查看异常登录模式，对应的 PAM 配置文件。说明如下：

PAM（Pluggable Authentication Modules）可动态加载验证模块，因为可以按需要动态的对验证的内容进行变更，所以可以大大提高验证的灵活性。









| **文件**               | **功能说明**                   |
| ---------------------- | ------------------------------ |
| /etc/pam.d/login       | 控制台（管理终端）对应配置文件 |
| /etc/pam.d/sshd        | 登录对应配置文件               |
| /etc/pam.d/system-auth | 系统全局配置文件               |

​	注：每个启用了 PAM 的应用程序，在 /etc/pam.d 目录中都有对应的同名配置文件。

​              例如，login 命令的配置文件是 /etc/pam.d/login，可以在相应配置文件中配置具体的策略。


​	检查前述配置文件中，是否有类似如下配置信息： auth        required      pam_succeed_if.so uid >= 1000


​	如果需要修改相关策略配置，在继续之前建议进行文件备份。

​	使用 vi 等编辑器，修改相应配置文件中的上述配置，或者整个删除或注释（在最开头添加 # 号）整行配置，

​             比如： 

​                   auth        required      pam_succeed_if.so uid <= 1000      # 修改策略

​	# auth        required      pam_succeed_if.so uid >= 1000   #取消相关配置


​	尝试重新登录服务器。





CentOS 的安全问题  SSH 的安全问题

​     \1. 设置用户名密码一定要大小写字母数字及一些特殊符号的组合，增加暴力破解的难度,有可能的话可以定期更换密码。

​     \2. 禁止ssh root 登录

​     \3. 定期维护hosts.deny文件，可以选择安装一些第三方的工具自动根据一些规则维护hosts.deny 比如 "DenyHosts"

​     \4. 为了避免骇客扫描已知服务器程序端口漏洞，修改服务程序的默认端口好，比如ssh服务不使用22端口

​     \5. 设置 iptables 开启一些通用的防火墙规则，ubuntu系统可以使用 ufw

​     \6. 禁止密码登陆，同样修改 sshd_config 设置属性“PasswordAuthentication no” ，然后公钥和私钥的方式进行登陆。

​     \7. 使用fail2ban ，fail2ban可以监视你的系统日志，然后匹配日志的错误信息（正则式匹配）执行相应的屏蔽动作（一般情况下是调用防火墙屏蔽），如:当有人在试探你的SSH、SMTP、FTP密码，只要达到你预设的次数，fail2ban就会调用防火墙屏蔽这个IP，而且可以发送e-mail通知系统管理员，是一款很实用、很强大的软件！









**SSH** **通过密码远程登录云服务器：** **ssh -q -l root 123.206.30.43 -p 22**   **再输入密码登录**

**ssh (secure shell** **安全壳协议****)**





**SSH****密钥免密登录**



**第一步：**先在客户端创建密钥 公钥和私钥 ，在 Mac 终端输入：**ssh-keygen -t rsa -C  ‘renpeng@192.168.31.181’   -t**   指定密钥类型，默认即 rsa ，可以省略    -C 设置注释文字，比如你的邮箱，可以省略， 生成秘钥对后，一般会在下载文件夹，复制到Mac电脑的 home/renpeng/.ssh 文件夹下，如果没有.ssh 文件夹，就创建一个。将私钥改名为 sciencekids_key



打开mac终端，输入

cd ~/.ssh #进入目录，这是会看到有个 config 文件，编辑该文件（或者不通过终端，直接找到并打开 .ssh 文件夹进行编辑 config文件，打开mac上的访达，按 shift+花键+G ，输入 Users/renpeng/.ssh   就可以打开 .ssh 文件夹了）

vim config   # 编辑config 文件，并在文件中添加下面语句并保存





Host sciencekids    # 通过该昵称，便捷访问服务器

HostName 123.456.789.00    # 这里是你的服务器的 IP 地址

User root   # 这里是登录服务器的用户名，如果是root，就写root

IdentityFile ~/.ssh/sciencekids_key     # 设置密钥文件路径



**第二步：**在服务器端 /home/任鹏/  文件夹下创建 .ssh 文件夹      

**第三步：**将公钥 上传至.ssh文件夹下

**第四步：**在服务器界面输入   cat  id_rsa.pub >> authorized_keys   将公钥内容 复制合并到authorized_keys文件里面

**第五步：**mac终端输入在config文件里设置的 Host 名称    ssh sciencekids    即可秘钥连接服务器



### Mac 终端 SSH 访问远端服务器配置

打开<font color="red"> **访达** </font>，跳出窗口，然后按  <font color="red"> **苹果键 + shift +G** </font> 跳出窗口，输入<font color="red">  **~/.ssh**  </font>跳出窗口，进行配置即可

```js
// 在 ~/.ssh/ 文件夹下 的 config 文件里 添加以下内容，通过 science 代理下面的配置

Host            science         
HostName        123.206.30.43       
Port            22            
User            root            
IdentityFile    ~/.ssh/science.dms
```



```js
ssh science // 在MAC 终端输入 看是否能访问远程服务器
```

```js
// 如果MAc终端报错
Permissions 0644 for '/Users/renpeng/.ssh/science.dms' are too open.
It is required that your private key files are NOT accessible by others
This private key will be ignored.
Load key "/Users/renpeng/.ssh/science.dms": bad permissions
root@123.206.30.43: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).

// 说明 science.dms 私钥的权限问题，修改权限就可以了
chmod 600 ~/.ssh/science.dms // 重要哦
ssh science // 再重新登录
```











**必须要学的知识点**

**1.** **什么是****repo****文件**

​        repo（repository 仓库）文件是Fedora中yum源（软件仓库）的配置文件，通常一个repo文件定义了一个或者多个软件仓库的细节内容，例如我们将从哪里下载需要安装或者升级的软件包，repo文件中的设置内容将被yum读取和应用！

​         YUM的工作原理并不复杂，每一个 RPM软件的头（header）里面都会纪录该软件的依赖关系，那么如果可以将该头的内容纪录下来并且进行分析，可以知道每个软件在安装之前需要额外安装 哪些基础软件。也就是说，在服务器上面先以分析工具将所有的RPM档案进行分析，然后将该分析纪录下来，只要在进行安装或升级时先查询该纪录的文件，就可 以知道所有相关联的软件。所以YUM的基本工作流程如下：

​        服务器端：在服务器上面存放了所有的RPM软件包，然后以相关的功能去分析每个RPM文件的依赖性关系，将这些数据记录成文件存放在服务器的某特定目录内。

​        客户端：如果需要安装某个软件时，先下载服务器上面记录的依赖性关系文件(可通过WWW或FTP方式)，通过对服务器端下载的纪录数据进行分析，然后取得所有相关的软件，一次全部下载下来进行安装。



**2.** **什么是****epel**

如果既想获得 RHEL 的高质量、高性能、高可靠性，又需要方便易用(关键是免费)的软件包更新功能，那么 Fedora Project 推出的 EPEL(Extra Packages for Enterprise Linux)正好适合你。EPEL(http://fedoraproject.org/wiki/EPEL) 是由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。



epel的使用心得：

1，不用去换原来yum源，安装后会产生新repo

2，epel会有很多源地址，如果一个下不到，会去另外一个下

   http://mirror.suhu.com/fedora-epel/6/x86_64/epel-release-6-8.noarch.rpm

3，更新时如果下载的包不全，就不会进行安装。这样的话，依赖关系可以保重





**3.** **什么是** **RPM** 

RPM [1]  是Red-Hat Package Manager（RPM软件包管理器）的缩写，这一文件格式名称虽然打上了RedHat的标志，但是其原始设计理念是开放式的，现在包括OpenLinux、S.u.S.E.以及Turbo Linux等Linux的分发版本都有采用，可以算是公认的行业标准了。

相关命令

在Terminal中，基本的安装指令如下：

**rpm** **－****i xv****－****3.10a****－****13.i386.rpm**

如果你的连网速度足够快，也可以直接从网络上安装应用软件，只需要在软件的文件名前加上适当的[URL](https://baike.baidu.com/item/URL)路径。

作为一个软件包管理工具，RPM管理着系统已安装的所有RPM程序组件的资料。我们也可以使用RPM来卸载相关的应用程序。

rpm －e xv

RPM的常用参数还包括：

－vh：显示安装进度；

－U：升级软件包；

－qpl：列出RPM软件包内的文件信息；

－qpi：列出RPM软件包的描述信息；

－qf：查找指定文件属于哪个RPM软件包；

－Va：校验所有的RPM软件包，查找丢失的文件；

－qa: 查找相应文件，如 rpm -qa mysql









RPM主要功能

安装、[卸载](https://baike.baidu.com/item/%E5%8D%B8%E8%BD%BD)、升级和管理软件，组件查询功能，验证功能，软件包GPG和MD5数字签名的导入、验证和发布，软件包依赖处理，选择安装，网络远程安装功能

rpm 命令：遵循GPL协议且功能强大的包管理，它可以建立、安装、请求、确认、和卸载软件包。间接的提升了Linux 的易用性

-e 卸载rpm包

-q 查询已安装的软件信息

-i 安装rpm包

-u 升级rpm包

--replacepkgs 重新安装rpm包

--justdb 升级数据库，不修改文件系统

--percent 在软件包安装时输出百分比

--help 帮助

--version 显示版本信息

-c 显示所有配置文件

-d 显示所有文档文件

-h 显示安装进度

-l 列出软件包中的文件

-a 显示出文件状态

-p 查询/校验一个软件包文件

-v 显示详细的处理信息

--dump 显示基本文件信息

--nomd5 不验证文件的md5支持

--nofiles 不验证软件包中的文件

--nodeps 不验证软件包的依赖关系

--whatrequires 查询/验证需要一个依赖性的软件包

--whatprovides 查询/验证提供一个依赖性的软件包





（选用）为centos挂载新的磁盘

\2. 输入 df -h 查看磁盘状况  disk free

\3. 输入 fdisk -l 列出所有磁盘信息

\4. 输入 fdisk /dev/vdb  为vdb磁盘创建一个分区,输入n 输入p 输入1 输入wq退出。

\5. 输入 mkfs.ext3 /dev/vdb1 格式化vdb磁盘 用 ext3格式。 make file system

\6. 输入 echo "/dev/vdb1 /mnt ext3 defaults 0 0" >> /etc/fstab 将另外一个磁盘挂载到fstab文件里，这样每次启动就可以自动启动该磁盘。

\7. cat /etc/fstab  查看 fstab这个文件





**************************************************************************************



###第二步：安装 Nginx



1. nginx -v  #查看是否最新的 nginx版本，也看看是否安装过 nginx 

2. yum remove nginx  #如果已经安装过老版本的 nginx，就卸载掉

3. 你在新服务器上安装 Nginx 之前，需要设置 Nginx 包管理仓库，随后你就可以从这个包管理仓库升级nginx了

   在centos上建立一个文件，位置为：/etc/yum.repos.d/nginx.repo    在/etc/yum.repos.d 文件夹下建立  nginx.repo 文件。 将以下内容粘贴进该文件，此步骤很重要，可以使 centos 7 自动升级 nginx 到最新版本，[具体可以点击这里查看nginx官网的指导](http://nginx.org/en/linux_packages.html#stable)                                        

```js
sudo yum install yum-utils // 安装 yum 软件包
```

```js
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
```

把上面的代码粘贴到 新建立的 nginx.repo 文件里面

默认连接的是 nginx stable repo , 如果你想换成 nginx mainline repo 的话，进行以下操作

```js
sudo yum-config-manager --enable nginx-mainline
```



1. yum list | grep nginx   #用 yum 命令查询一下 nginx 的 yum 源配置好了没有



3.1. sudo yum install epel-release  # 添加CentOS 7 EPEL仓库， EPEL 是 yum 的一个软件源, 里面包含了许多基本源里没有的软件了, 但在我们在使用 epel 时是需要安装它才可以了, 下文来介绍 CentOS7/RHEL7 安装 EPEL 步骤

EPEL, 即 Extra Packages for Enterprise Linux 的简称, 是为企业级 Linux 提供的一组高质量的额外软件包, 包括但不限于 Red Hat Enterprise Linux (RHEL), CentOS and Scientific Linux (SL), Oracle Enterprise Linux (OEL).



3.2. ll /etc/yum.repos.d/  # 验证 epel 仓库

3.3. sudo yum search w3m  # 查看 安装 epel 后，是否有了 w3m



​            

1. sudo yum install nginx –y  #安装最新稳定版的 nginx
2. nginx -v  #安装后查看是否最新稳定版 nginx
3. systemctl start nginx.service  # 开启nginx服务器

​     如果您正在运行防火墙，请运行以下命令以允许HTTP和HTTPS通信：

​    sudo firewall-cmd --permanent --zone=public --add-service=http 

​    sudo firewall-cmd --permanent --zone=public --add-service=https

​    sudo firewall-cmd --reload 

​    重启防火墙后，看看网站首页 nginx 是否能显示出来



1. systemctl enable nginx.service   # 使 nginx 服务器可以随着 CentOS 的启动而自动启动。
2. systemctl status nginx.service #查看 nginx 状态
3. systemctl stop nginx.service # 停止 nginx 服务器
4. systemctl restart nginx.service  # 重启 nginx
5. yum update nginx -y #完成nginx的更新升级
6. nginx -t  # 测试nginx 配置是否有语法错误

​       Nginx - ?  nginx -?vVtq | nginx –s signal | nginx –c filename  | nginx –p prefix| nginx – g directives—+-

​         -?, -h        : 打开帮助信息 

​         -v             : 显示版本信息并退出

​         -V             : 显示版本和配置选项信息，然后退出

​          -t             : 检测配置文件是否有语法错误，然后退出

​         -q             : 在检测配置文件期间屏蔽非错误信息

​         -s signal   : 给一个nginx主进程发送信号： stop（停止） ，quit（退出）reopen （重启） , reload （重新加载配置文件）

​         -p prefix      : 设置前缀路径 （默认是：/usr/local/Cellar/nginx/1.2.6/）

​        -c filename    : 设置配置文件 （默认是：/usr/local/etc/nginx/nginx.conf）

​        -g directives  : 设置配置文件外的全局指令







**Nginx****常用目录**



/usr/share/nginx/html  #Nginx默认站点目录

/etc/nginx/conf.d/default.conf    #Nginx 网站默认站点配置

/etc/nginx/    #自定义 Nginx 站点配置文件存放目录：
 /var/run/nginx.pid    #PID目录：
 /var/log/nginx/error.log     #错误日志：
 /var/log/nginx/access.log    #访问日志：
  /etc/nginx/conf.d/      #自定义 Nginx 站点配置文件存放目录

/nginx –c nginx.conf      #Nginx 启动

/etc/nginx/nginx.conf     #Nginx 全局配置







VI 编辑器操作命令



vi  编辑器的简单操作命令

vi  进入编辑模式

:w   保存文件但不退出vi 编辑

:w!    强制保存，不退出vi 编辑

:w file    将修改另存到file中，不退出vi 编辑



:wq   保存文件并退出vi 编辑

:wq!   强制保存文件并退出vi 编辑



q:    不保存文件并退出vi 编辑

:q!    不保存文件并强制退出vi 编辑

:e!    放弃所有修改，从上次保存文件开始在编辑







**如果是本地端服务器设置虚拟网址，访问自己服务器上的****web****页面**



在mac os 终端，创建虚拟主机，添加主机记录

在mac电脑上，设置一个新的域名，添加一个新的 dns 记录，让某一个主机名指向我们的IP地址



在mac终端，打开并编辑 hosts，用 atom编辑器打开

atom /etc/hosts  #用 atom编辑器，打开mac上的 hosts 配置



打开文件后，添加 一个虚拟的 ip地址，和随便写一个 域名

127.0.0.1   sciencdkids.cn

127.0.0.1   www.sciencdkids.cn

127.0.0.1   php.sciencdkids.cn





**************************************************************************************



###第三步：安装 Node js



\1.  sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel -y  #安装git依赖包

\2.  sudo yum install  gcc perl-ExtUtils-MakeMaker -y  # git 依赖包

\3. yum remove git  # 如果安装过旧版的 git ，就卸载掉

\4.  cd /usr/src  #进入 改文件地址，将git 下载到这里

\5.  wget -O git.zip <https://github.com/git/git/archive/master.zip>  #下载最新版 git 源码 wget 是一个从网络上自动下载文件的自由工   具，支持通过 HTTP、HTTPS、FTP 三个最常见的 [TCP/IP协议](https://baike.baidu.com/item/TCP%2FIP%E5%8D%8F%E8%AE%AE) 下载，并可以使用 HTTP 代理。"wget" 这个名称来源于 “World Wide Web” 与 “get” 的结合。



\6.  unzip git.zip  #解压

\7.  cd git-master/  #进入解压好的文件

\8. make prefix=/usr/local/git all   #编译 git

\9. make prefix=/usr/local/git install   #安装 git

\10. echo 'export PATH=$PATH:/usr/local/git/bin' >> /etc/bashrc  #将 git 加入 Path or echo'export PATH=$PATH:/usr/local/git/bin' > /etc/profile.d/git.sh

\11. source /etc/bashrc  #查看git版本

\12. yum install git  #先安装 git ，有了git，直接从 github上clone项目到本地

\13. yum remove nodejs –y  #如果已经安装过node，就卸载掉

在github 上搜索 mvn ，找到官网发布地址 

\14. curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash #nvm 源装完nvm后，重启centos服务器

\15.  nvm help  #查看nvm 帮助文档，操作nvm  如果nvm不是最新版，就手动升级nvm

\1. change to the $NVM_DIR

\2. pull down the latest changes

\3. check out the latest version

\4. activate the new version

(   cd "$NVM_DIR"   git fetch --tags origin   git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)` ) && \. "$NVM_DIR/nvm.sh"

更新后，需要重启服务器

 

\16.  nvm ls-remote   #通过nvm 查看远程node版本.  Nvm ( node version manager )

\17.  nvm install stable #安装最新稳定版

\18.  nvm use vx.xx.x  #切换喜欢的node版本

\19.  npm install express –g  #全局安装express.   NPM的全称是Node Package Manager，是随同NodeJS一起安装的包管理和分发工具，它很方便让JavaScript开发者下载、安装、上传以及管理已经安装的包。



**************************************************************************************



###第四步：安装 PM2

到了这一步，可以先把程序代码文件通过FTP上传到centos服务器 /usr/share/nginx/

pm2 是一个带有负载均衡功能的Node应用的进程管理器

\1. npm install -g pm2  #安装PM2

\2. pm2 start bin/ www –name scienceKids  #启动node应用, 并给应用起个名字

\3. pm2 start bin/www —watch  # 添加应用 并监控

\4. pm2 stop - -watch 0  #停止监控 序列 0 的应用

\5. pm2 stop www  #停止应用

\6. pm2 stop all  #停止所有应用

\7. pm2 delete www  #删除www应用

\8. pm2 delete all  #删除所有应用

\9. pm2 list  #列出所有应用

\10. pm2 describe www  #查看www进程的具体情况

\11. pm2 monit  # 查看应用的资源消耗情况

\12. pm2 restart www  #重启应用

\13. pm2 restart all  # 重启所有应用

\14. pm2 logs  # 查看pm2的运行日志

\15. pm2 start www -i 4 --watch  #通过 cluster 4线程 模式启动node 并监控

 

 

在项目更目录建立一个  xxx.json 文件，粘贴如下列内容，注意配置 log 路径

\16. pm2 start app.js -i 3 #开启三个进程 pm2 start app.js -i max  #根据机器cpu核数，开启对应数目的进程

\17. pm2 --help

\18. 开机启动  pm2 startup  

​      pm2 save 保存当前进程状态

​      pm2 startup  生成开机启动命令

\19. 更新pm2 

​     pm2 save  #先保存进程状态

​     Npm install pm2 -g

​     pm2 update

\20. 如果手动重启了 服务器，pm2却没有重新启动，/usr/share/nginx/html/sciencekids/bin/www 

​       **pm2 start www -i 4 --name sciencekids --watch** 

​       --name sciencekids --watch 到这个地址可以启动进程

 

 \21.  npm i   # 按照 package.json 里记录的模块，下载所有模块

 

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











**nrm** **管理及使用**

npm install -g nrm  #全局安装nrm。

nrm ls  #查看可选的源

rm use taobao #切换到淘宝源 

nrm del <registry>  #删除对应的源

nrm test npm  # 测试相应源的响应时间







###第五步：安装MongoDB 或 MariaDB或者MySql 数据库 

推荐使用 MariaDB，免费，搜索更快速, 效率更高

MariaDB 的安装使用和配置

**一、准备**



\1. 为保持系统运行轻便简洁，CentOS7.5操作系统选择“最小化安装”选项安装。安装过程略。



\2. MariaDB10.3是一个主要版本，将支持到2023年5月。

（1）官方网址：http://mariadb.org/

（2）下载页：https://downloads.mariadb.org/

（3）技术白皮书Release Notes（安装说明文档）：二进制Tarballs包安装说明



\3. 标记为glibc_214的tar包，在运行时需要glibc2.14或更高版本的支持。

ldd --version # 查看本机的 glibc 版本号的方法：

ldd (GNU libc) 2.17



\4. 查看CentOS是否自带MariaDB

​     rpm -qa|grep mariadb # 查看CentOS是否安装了MariaDB

​     mariadb-libs-5.5.60-1.el7_5.x86_64 都是5.5 的老版本，需要更新到最新版 10.3



​     rpm -qc mariadb-libs-5.5.60-1.el7_5.x86_64  # 查看MariaDB安装包配置文件

​     /etc/my.cnf  第一个位置

​     /etc/my.cnf.d/mysql-clients.cnf  第二个位置



另外：可以使用rpm -qi查看安装包信息、使用rpm -ql查看安装包所有文件的位置。



rpm -e --nodeps mariadb-libs-5.5.60-1.el7_5.x86_64  # 卸载已安装的低版本 MariaDB

rpm -qa|grep mariadb # 再查看一次是否全部卸载完成，如果没有就继续卸载，直到完成







1.创建/etc/yum.repos.d/MariaDB.repo文件，这里用到了刚刚发布正式版的10.3 稳定版



 提示：MariaDB 不推荐以下第一条安装方法，因为安装的是旧版本的

1.  yum install mariadb mariadb-server  # 安装 mariadb , 请不要这样安装，因为都是旧版本



\2. [点击这里查看升级 MariaDB 到最新版本的英文文章](https://www.vultr.com/docs/how-to-install-mariadb-10-1-on-centos-7)     

​     [MariaDB 5.5  升级到 10 的官方说明](https://mariadb.com/kb/en/library/upgrading-from-mariadb-55-to-mariadb-100/)



 推荐用此方法安装最新版本的 MariaDB 

  在 /etc/yum.repos.d/文件夹下建立  MariaDB.repo 文件

  创建/etc/yum.repos.d/MariaDB.repo文件，这里用到了刚刚发布正式版的10.0

  将下列内容粘贴到 新建立的 MariaDB.repo 文件里


 \# MariaDB 10.1 CentOS repository list

\# http://downloads.mariadb.org/mariadb/repositories/

[mariadb]

name = MariaDB

baseurl = http://yum.mariadb.org/10.4/centos7-amd64

gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB

gpgcheck=1







**二、** **安装** **MariaDB** 


 sudo yum install MariaDB-server MariaDB-client -y  # 安装 MariaDB

sudo systemctl start mariadb.service   # 启动 MariaDB

sudo /usr/bin/mysql_secure_installation  # 安全地安装 MariaDB。Secure the installation of MariaDB



Answer questions as below, and ensure that you will use your own MariaDB root password:

Enter current password for root (enter for none): Just press the Enter button

Set root password? [Y/n]: Y   #是否设置密码

New password: your-MariaDB-root-password  #你的MariaDB数据库的新密码

Re-enter new password: your-MariaDB-root-password  #重新输入一遍新密码

Remove anonymous users? [Y/n]: Y   # 去除匿名用户

Disallow root login remotely? [Y/n]: Y  #不允许 root用户远程登录

Remove test database and access to it? [Y/n]: Y   #去除测试数据库和对它的访问？

Reload privilege tables now? [Y/n]: Y  #现在重新启动特权表？





sudo systemctl start mariadb.service   # 启动 MariaDB

sudo systemctl enable mariadb.service  # 使 MariaDB 可以开机启动



 

\2.   启动 停止 重启 MariaDB 等命令

​      systemctl start mariadb.service #启动MariaDB

​      systemctl stop mariadb.service #停止MariaDB

​      systemctl restart mariadb.service #重启MariaDB

​      systemctl enable mariadb.service #设置开机启动

​      mysql -uroot -p            #进入数据库，-u是登陆用户，-p该用户密码



​      show databases;  # 显示所有数据库



\3. 卸载 MariaDB 

​    rpm -qa | grep Maria*  # 搜索查询所安装的MariaDB组件

   yum -y remove mari*  # 卸载 MariaDB 组件

   rm -rf /var/lib/mysql/*  # 删除数据库文件







**三、配置****MariaDB**

为 MariaDB 配置远程访问权限

在第二步中如果禁用 root 远程登录选择 Y 的话就不能在别的电脑通过navicat等工具连接到数据库，这时就需要给对应的 MariaDB 账户分配权限，允许使用该账户远程连接到MariaDB。可以输入以下命令查看账号信息：

select User, host from mysql.user;  # 查询 MariaDB 当前允许登录的用户名

![pastedGraphic.png](/Users/renpeng/Library/Application Support/typora-user-images/7D633328-DBA9-430E-BA17-685AF70581A1/pastedGraphic.png)

root账户中的host项是localhost表示该账号只能进行本地登录，我们需要修改权限，输入命令：

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;

修改权限。%表示针对所有IP，password表示将用这个密码登录root用户，如果想只让某个IP段的主机连接，可以修改为：

GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.71.%' IDENTIFIED BY 'my-new-password' WITH GRANT OPTION;



GRANT REPLICATION slave ON *.* TO 'root'@'%' IDENTIFIED BY 'my-new-password' WITH GRANT OPTION;  

\#添加备份的账号(记录一下，以备后用)



CREATE USER 'git'@'localhost' IDENTIFIED BY ‘$password’;   #创建新用户，其中$password填写自己设置的密码。当然后面也可以修改；



最后别忘了：

FLUSH PRIVILEGES;  #mysql 新设置用户或更改密码后需用flush privileges刷新MySQL的系统权限相关表，否则会出现拒绝访问，还有一种方法，就是重新启动mysql服务器，来使新设置生效。



show master status\G;   # 查看记录 log_bin文件名和pos



保存更改后，再看看用户账号信息：

![pastedGraphic_1.png](/Users/renpeng/Library/Application Support/typora-user-images/7D633328-DBA9-430E-BA17-685AF70581A1/pastedGraphic_1.png)

这个时候发现相比之前多了一项，它的host项是%，这个时候说明配置成功了，我们可以用该账号进行远程访问了。





**CentOS 7** **为** **MariaDB** **开放防火墙端口**

在第四步后如果还是不能远程连上数据库的话应该就是3306端口被防火墙拦截了，这时我们就需要关闭防火墙或者开放防火墙端口。

关闭防火墙：

systemctl stop firewalld.service            #停止firewall    不推荐关闭防火墙

systemctl disable firewalld.service        #禁止firewall开机启动  不推荐关闭防火墙



开放防火墙端口，开启后要重启防火墙：

firewall-cmd --zone=public --add-port=3306/tcp --permanent

firewall-cmd --reload 



**设置数据库字母大小写不敏感**

vi /etc/my.cnf.d/ server.cnf  # 用 FTP 软件打开编辑该文件更方便

在[ mysqld ]下加上

lower_case_table_names=1

默认是等于0的,即大小写敏感。改成1就OK了。如果之前已经建了数据库要把之前建立的数据库删除，重建才生效。



**设置****MariaDB****数据库默认编码**

MariaDB的默认编码是latin1，插入中文会乱码，因此需要将编码改为utf8。

1.登录，使用以下命令查看当前使用的字符集，应该有好几个不是utf8格式。

SHOW VARIABLES LIKE 'character%';  #查看是否字符集有 utf8, 如果没有，就进行以下操作



2.修改的配置文件

打开并编辑该地址文件 vi /etc/my.cnf.d/client.cnf  在[client]字段里加入

default-character-set=utf8



打开并编辑该地址文件  vi /etc/my.cnf.d/server.cnf   在[mysqld]字段里加入

character-set-server=utf8



systemctl restart mariadb  # 重启 MariaDB 配置生效。



SHOW VARIABLES LIKE 'character%'; # 再检查一遍字符集，看有没有 utf8





**四、数据库架构简介与使用**            

数据库管理系统有多关系型数据库（DataBase），关系型数据库是由一个或多个数据表单（Table）组成，数据表单保存着多个数据记录（Record）



\1. 数据库管理

show databases;   #1.展示已有关系型数据库           

create database database-name;   # 创建新的数据库                

drop database database-name;   #删除数据库                

use database-name;   #指定使用数据库            



2.展示所用关系数据库（database）的表单信息及数据记录操作                

show tables;  #显示当前关系数据库中的表单信息。                

create table table-name (field1 type,filed2 type);   # 创建表单并规定格式                

describe table-name;   # 查看表单结构描述                

drop table table-name;   # 删除表单                

delete from table-name;   # 删除表单所有内容                

delete from table-name where filed条件;   # 删除满足where条件的所在行                

select * from table-name;  # 查看表单数据                

select field1,filed2 from table-name;   # 只查看field1，2所在列内容数据                

select * from table-name where field条件;  # 查找出filed满足条件的行的数据                

update table-name set filed=?;    #修改field所有数据为？                

update table-name set filed=? where field条件;    # 修改满足where的field数据变为？                   

下表是where使用的参数作用。 

​                         

3.用户管理及用户权限管理          

1.用户管理（用户信息存储于mysql关系数据库的user表单中,）             

create user 用户名@主机名 identified by '密码';   #创建新用户            

select host,user,pass[Word](http://www.knowsky.com/article.asp?typeid=117) from user;  # 查看用户信息(几项重要信息), 其实就是查询mysql数据库中user表单信息,其它对用户修改，删除操作类似操作普通表单。          

2.用户权限管理              

grant 权限 on database-name.table-name to 用户名@主机;   # 对某关系数据库中的某表单赋权限                

权限：select, update, delete, insert              

show grants for 用户名@主机;   # 查看用户权限





五、在客户端 配置 MySQL WorkBench 图形界面工具，远程操作 MariaDB

​      [点击这里，在 MacOS 上 安装 WorkBench 工具](https://dev.mysql.com/downloads/workbench/)

​      如果CentOS 服务器 只用普通的 SSH 登录，则使用密码登录即可







**MariaDB** **数据库到此就安装完成了！**





如果要手动安装 MariaDB

**下载****MariaDB,** **到** [**https://downloads.mariadb.com/MariaDB**](https://downloads.mariadb.com/MariaDB) **页面，找到相应的最新版本的链接，**

wget https://downloads.mariadb.com/MariaDB/mariadb-10.4.0/bintar-linux-glibc_214-x86_64/mariadb-10.4.0-linux-glibc_214-x86_64.tar.gz





———————————————————————————————

**MySQL** **的使用和配置**



<https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/>   #mysql 官网指导安装

\1. 添加MySQL Yum仓库, 到MySql官网找到最新版本号 更改 /get/后面的内容

wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm

 

\2. 执行

sudo rpm -Uvh mysql80-community-release-el7-1.noarch.rpm

 

\3. 安装MySQL

sudo yum install mysql-community-server -y

 

\4. 启动MySQL

sudo systemctl start mysqld.service

 

5.查看MySQL服务状态

sudo systemctl status mysqld.service

 

\6. 查看mysql登陆密码 

sudo grep 'temporary password' /var/log/mysqld.log

 

mysql –u root –p   #用获取到的默认密码登陆mysql

 

\7. 更换新mysql登陆密码

ALTER USER 'root'@'localhost' IDENTIFIED BY 'Jylp@cn123456789';

 

\8. 通过 FileZilar 编辑 my.cnf 更改数据库默认字符 utf8

编辑/etc/my.cnf 
 在[mysqld]之前添加 
 [client] 
 default-character-set=utf8 
 在[mysqld]之后添加 
 character-set-server=utf8

sudo systemctl restart mysqld.service   #重启服务器

 

\9. sudo systemctl enable mysqld.service  #设置开机启动mariadb数据库

\10. sudo systemctl start mysqld.service  #启动数据库

\11. sudo systemctl stop mysqld.service  #停止数据库

\12. sudo systemctl restart mysqld.service  #重启数据库

 

默认配置文件路径： 
 配置文件：/etc/my.cnf 
 日志文件：/var/log/var/log/mysqld.log 
 服务启动脚本：/usr/lib/systemd/system/mysqld.service 
 socket文件：/var/run/mysqld/mysqld.pid

配置  my.cnf        vim /etc/my.cnf

 

\14.  show databases;  #显示数据库

\15.  use mysql;  # 使用mysql 数据库

\16. show tables;  #显示数据库中的数据表

\17. select * from event;  # 搜索event表中的所有数据。

 

\18. 给数据库授权远程访问： 这个坑很深啊！，我花了一天的功夫才找到下面的句子。

ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'Jylp@cn123456789';

 

\19. 安装 node-mysql 支持模块

npm install node-mysql  #重要，否则会报错

 

如果18 句子没有解决远程 mysql workbench 访问，再试试下面的方法。

MySql HostName : VM_0_7_centos

MySql Server Port ：3306

SSH UserName：root

SSH Hostname:123.206.30.43



\18. mysql 授权远程访问 

\1. mysql> use mysql;   

\2. mysql> update user set host = '%' where user = 'root';   

\3. mysql> select host, user from user;   

\4. mysql> flush privileges;  

 



###第六步：配置HTTPS验证

安装完 MariaDB 数据库后，运行小程序，会出现以下错误

GET <https://XXXXX>   net::ERR_CONNECTION_REFUSED    #这是因为https / SSL 没有配置成功的原因



**CentOS** **上安装** **Nginx 1.14.0** **以上版本配置** **SSL/https**

正式开始：

**一、****nginx** **开启****SSL****模块**

centos7上配置https之前需要先配置 ngx_http_ssl_module,否则即使在nginx.conf中配置了https，但在启动nginx的时候会报错：



**1.1 nginx****如果未开启****SSL****模块，配置****https****时提示错误**

nginx: [emerg] the "ssl" parameter requires ngx_http_ssl_module in /usr/local/nginx/conf/nginx.conf:118

原因也很简单，nginx缺少http_ssl_module模块，编译安装的时候带上 --with-http_ssl_module 配置就行了，但往往实际的情况是我们的nginx已经安装过了，在这种情况要怎么添加模块，其实很简单，往下看：做个说明：我的nginx安装目录是/usr/local/nginx这个目录，我的源码包在/usr/local/nginx-1.14.0 (有的人喜欢把源码包放在/usr/local/src/nginx-1.14.0)



**1.2** **切换到源码包**

cd /usr/local/nginx-1.14.0

查看nginx原有的模块

/usr/local/nginx/sbin/nginx -V



生成csr（CSR,Cerificate Signing Request，CSR是您的公钥证书原始文件，包含了您的服务器信息和您的单位信息，需要提交给CA认证中心。）的命令如下：



**在****nginx.conf****中配置****https****加密证书**   /etc/nginx/ssl 建立 ssl 文件夹，把证书和私钥 放入其中，然后在下面配置地址

server {

​    listen       443 ssl;

​    server_name  192.168.0.244;

​    ssl_certificate      /usr/local/nginx/conf/ssl/server.crt;

​    ssl_certificate_key  /usr/local/nginx/conf/ssl/server.key;

 

​    ssl_session_cache    shared:SSL:1m;

​    ssl_session_timeout  5m;

 

​    ssl_ciphers  HIGH:!aNULL:!MD5;

​    ssl_prefer_server_ciphers  on;

​		

 

​    location / {

​        root   html;

​        index  index.html index.htm;

​    }

}



### 第七步：将本地项目部署到服务器

1、 将前端代码和后端代码上传到nginx指定的位置

/usr/share/nginx/html/admin  // 前台代码位置

/usr/share/nginx/html/server // 后台 代码位置

2、安装好 数据库 MongoDB，配置好端口

3、在nginx服务器端设置代理，将 带有 api 的路由 都转发到 指定的端口:9528

server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;
    
        location / {
    	root /root/www/;
    	index index.html index.htm;	
        }
    	location /api/ {
    		proxy_pass	http://127.0.0.1:9528; //表示监听
    		proxy_set_header Host	$host;
    	}
        error_page 404 /404.html;
            location = /40x.html {
        }
    
        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }


