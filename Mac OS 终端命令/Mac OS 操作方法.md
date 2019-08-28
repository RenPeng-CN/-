[TOC]



### Mac OS 安装 HomeBrew

[Homebrew ](https://brew.sh/index_zh-cn)  是 MacOS 缺失的软件包的管理器

将以下代码粘贴到 mac 终端，回车执行安装

```js
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```



###操作 Homebrew 

```js
brew update     # 更新 Homebrew 
```

```js
brew outdated   # 查看哪些包可以更新。
```

```js
brew upgrade    # 更新所有的包
```

```js
brew upgrade $FORMULA  # 更新指定的 FORMULA 包, 更新指定的包
```

```js
brew cleanup # 清理所有包的旧版本 
```

```js
brew cleanup $FORMULA # 清理指定包的旧版本 
```

```js
brew cleanup -n # 查看可清理的旧版本包，不执行实际操作 
```

```js
brew pin FORMULA      # 锁定 FORMULA 个包 不要升级
brew unpinFORMULA     # 取消锁定 
```

```js
brew info #可以查看包的相关信息，最有用的应该是包依赖和相应的命令。比如 Nginx 会提醒你怎么加 launchctl ，PostgreSQL 会告诉你如何迁移数据库。这些信息会在包安装完成后自动显示，如果忘了的话可以用这个命令很方便地查看。
```

```js
brew info             # 显示安装了包数量，文件数量，和总占用空间 
brew info $FORMULA    # 显示 FORMULA 包的信息 , 显示某个包的内容
```

```js
brew deps     #可以显示包的依赖关系，我常用它来查看已安装的包的依赖，然后判断哪些包是可以安全删除的。
brew deps –installed –tree # 查看已安装的包的依赖，树形显示 
```

```js
man brew    #还有很多有用的命令和参数，没事 man brew 一下可以涨不少知识。
```

```js
brew services list  #  查看目前执行的服务
```





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



### Mac 上的 MongoDB 的操作

```js
brew install mongodb  # 安装 mongodb
brew services start mongodb  # 启动mongodb服务：在终端执行
mongo  # 在终端输入 mongo ，进入数据库
show dbs  # 显示数据库
```



### Mac 上的 Node 版本 操作

```js
nvm ls  // 通过nvm ls 命令可以查看已经安装的 node 版本。
nvm v // 查看 node 版本 => 1.1.1
nvm install node  // 安装最新版本的 node
node --version // 查看已经安装的 node 的版本号
nvm uninstall 6.9.2 // 卸载指定的版本
https://github.com/nvm-sh/nvm/blob/master/README.md // 安装 nvm 最新版要到github上查看
```





目录操作**

| **命令名** | **功能描述**                                     | **使用举例**     |
| ---------- | ------------------------------------------------ | ---------------- |
| **mkdir**  | ( **make directory** )创建一个目录               | mkdir dirname    |
| **rmdir**  | ( **remove directory** )删除一个目录             | rmdir dirname    |
| **mvdir**  | ( **move directory** )移动或重命名一个目录       | mvdir dir1 dir2  |
| **cd**     | ( **change directory** )改变当前目录             | cd dirname       |
| **pwd**    | ( **print working directory**)显示当前目录的路径 | pwd              |
| **ls**     | ( **list** )显示当前目录的内容                   | ls -la           |
| **dircmp** | ( **directory compaire )** 比较两个目录的内容    | dircmp dir1 dir2 |



**文件操作**

| **命令名** | **功能描述**                               | **使用举例**              |
| ---------- | ------------------------------------------ | ------------------------- |
| **cat**    | ( **catenate** )显示或连接文件             | cat filename              |
| **pg**     | ( **page** )分页格式化显示文件内容         | pg filename               |
| **more**   | 分屏显示文件内容                           | more filename             |
| **od**     | ( **Octal** **Dump** )显示非文本文件的内容 | od -c filename            |
| **cp**     | ( **copy** )复制文件或目录                 | cp file1 file2            |
| **rm**     | ( **remove** )删除文件或目录               | rm filename               |
| **mv**     | ( **move** )改变文件名或所在目录           | mv file1 file2            |
| **ln**     | ( **link** )联接文件                       | ln -s file1 file2         |
| **find**   | 使用匹配表达式查找文件                     | find . -name "*.c" -print |
| **file**   | 显示文件类型                               | file filename             |
| **open**   | 使用默认的程序打开文件                     | open filename             |





**选择操作**

| **命令名** | **功能描述**                                                 | **使用举例**                 |
| ---------- | ------------------------------------------------------------ | ---------------------------- |
| **head**   | 显示文件的最初几行                                           | head -20 filename            |
| **tail**   | 显示文件的最后几行                                           | tail -15 filename            |
| **cut**    | 显示文件每行中的某些域                                       | cut -f1,7 -d: /etc/passwd    |
| **colrm**  | ( **column** **remove** ) 滤掉指定的列                       | colrm 8 20 file2             |
| **paste**  | 横向连接文件                                                 | paste file1 file2            |
| **diff**   | ( **diffrence** )比较并显示两个文件的差异                    | diff file1 file2             |
| **sed**    | ( **Stream Editor** )非交互方式流编辑器                      | sed "s/red/green/g" filename |
| **grep**   | 在文件中按模式查找                                           | grep "^[a-zA-Z]" filename    |
| **awk**    | ( **三个创始人名字首字母，是处理文本的编程工具**)在文件中查找并处理模式 | awk '{print $1 $1}' filename |
| **sort**   | 排序或归并文件                                               | sort -d -f -u file1          |
| **uniq**   | ( **unique** )去掉文件中的重复行                             | uniq file1 file2             |
| **comm**   | 显示两有序文件的公共和非公共行                               | comm file1 file2             |
| **wc**     | ( **Word** **Count** )统计文件的字符数、词数和行数           | wc filename                  |
| **nl**     | ( **Number** **of** **Lines** )给文件加上行号                | nl file1 >file2              |



**安全操作**

| **命令名** | **功能描述**                                           | **使用举例**            |
| ---------- | ------------------------------------------------------ | ----------------------- |
| **passwd** | 修改用户密码                                           | passwd                  |
| **chmod**  | ( **change** **mode** )改变文件或目录的权限            | chmod ug+x filename     |
| **umask**  | ( **User**'**s** **MASK**)定义创建文件的权限掩码       | umask 027               |
| **chown**  | ( **change** **owner** )改变文件或目录的属主           | chown newowner filename |
| **chgrp**  | ( **Change GRouP** )改变文件或目录的所属组             | chgrp staff filename    |
| **xlock**  | Locks the local X display until a password is entered. | xlock -remote           |



**编程操作**

| **命令名** | **功能描述**             | **使用举例**               |
| ---------- | ------------------------ | -------------------------- |
| **make**   | 维护可执行程序的最新版本 | make                       |
| **touch**  | 更新文件的访问和修改时间 | touch -m 05202400 filename |
| **dbx**    | 命令行界面调试工具       | dbx a.out                  |
| **xde**    | 图形用户界面调试工具     | xde a.out                  |





**进程操作**

| **命令名** | **功能描述**                                          | **使用举例**         |
| ---------- | ----------------------------------------------------- | -------------------- |
| **ps**     | **(** **Prompt** **String** **)****显示进程当前状态** | **ps u**             |
| **kill**   | **终止进程**                                          | **kill -9 30142**    |
| **nice**   | **改变待执行命令的优先级**                            | **nice cc -c \*.c**  |
| **renice** | **改变已运行进程的优先级**                            | **renice +20 32768** |



**时间操作**

| **命令名** | **功能描述**                         | **使用举例**   |
| ---------- | ------------------------------------ | -------------- |
| **date**   | **显示系统的当前日期和时间**         | **date**       |
| **cal**    | **(** **calendar** **)****显示日历** | **cal 8 1996** |
| **time**   | **统计程序的执行时间**               | **time a.out** |

**网络与通信操作**

| **命令名** | **功能描述**                                                 | **使用举例**                |
| ---------- | ------------------------------------------------------------ | --------------------------- |
| **telnet** | ( **Teminal** **over** **Network** )远程登录                 | telnet hpc.sp.net.edu.cn    |
| **rlogin** | ( **Remote** **login** )远程登录                             | rlogin hostname -l username |
| **rsh**    | ( **Remote** **shell** )在远程主机执行指定命令               | rsh f01n03 date             |
| **ftp**    | ( **File** **Transfer** **Protocol** )在本地主机与远程主机之间传输文件 | ftp ftp.sp.net.edu.cn       |
| **rcp**    | ( **remote** **file** **copy** )在本地主机与远程主机 之间复制文件 | rcp file1 host1:file2       |
| **ping**   | ( **Packet** **internet** **groper** )网际包探测器, 给一个网络主机发送 回应请求 | ping hpc.sp.net.edu.cn      |
| **mail**   | 阅读和发送电子邮件                                           | mail                        |
| **write**  | 给另一用户发送报文                                           | write username pts/1        |
| **mesg**   | ( **message** )允许或拒绝接收报文                            | mesg n                      |





**Korn Shell** **命令**

| **命令名**  | **功能描述**                    | **使用举例**    |
| ----------- | ------------------------------- | --------------- |
| **history** | 列出最近执行过的 几条命令及编号 | history         |
| **r**       | 重复执行最近执行过的 某条命令   | r -2            |
| **alias**   | 给某个命令定义别名              | alias del=rm -i |
| **unalias** | 取消对某个别名的定义            | unalias del     |



**其它命令**

| **命令名** | **功能描述**                                              | **使用举例** |
| ---------- | --------------------------------------------------------- | ------------ |
| **uname**  | ( **unix** **name** )用来打印unix或类unix系统的name等信息 | uname -a     |
| **clear**  | 清除屏幕或窗口内容                                        | clear        |
| **env**    | ( **environment** )显示当前所有设置过的环境变量           | env          |
| **who**    | 列出当前登录的所有用户                                    | who          |
| **whoami** | ( **who am i** )显示当前正进行操作的用户名                | whoami       |
| **tty**    | ( **teletypewriter** 电传打字机)显示终端或伪终端的名称    | tty          |
| **stty**   | (  **Set** **TTY** )显示或重置控制键定义                  | stty -a      |
| **du**     | ( **Disk** **Usage** )查询磁盘使用情况                    | du -k subdir |
| **df**     | ( **Disk** **Free** )显示文件系统的总空间和可用空间       | df /tmp      |
| **w**      | 显示当前系统活动的总信息                                  | w            |



* 显示隐藏文件

<font color="red">Command+Shift+.</font>   可以显示隐藏文件、文件夹，再按一次，恢复隐藏

![image-20190131112210626](/Users/renpeng/Library/Application Support/typora-user-images/image-20190131112210626.png)

* 在终端输入

  在终端（Terminal）输入如下命令，即可显示隐藏文件和文件夹

  defaults write com.apple.finder AppleShowAllFiles -boolean true; killall Finder

* 如需再次隐藏原本隐藏的文件和文件夹，可以输入如下命令

  defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder





### 在 MAC 上安装 nvm 需要注意的事项

- 在浏览器打开以下地址，按照说明进行安装

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



### Mac Spotlight搜索快捷键

强大的本地搜索引擎，下面列举了常见的Spotlight快捷键的用法。



打开Spotlight菜单：<font color="red">Control+空格</font>


在Finder中打开Spotlight：<font color="red">Command+Option+空格</font>

清空Spotlight搜索框：<font color="red">ESC</font>

关闭Spotlight菜单：<font color="red">ESC按两次</font>

7个Spotlight使用&导航快捷键：

打开第一个搜索项目：<font color="red">回车</font>

在搜索结果中导航移动：<font color="red">上箭头、下箭头</font>

在Finder中国定位第一个搜索项目：<font color="red">Command+回车</font> （其他项可以将光标移过去再这么按）

查看搜索项的信息：<font color="red">Command+I</font>

显示搜索结果的快速查看预览：<font color="red">Command</font>，或者将鼠标光标游移到相应的搜索结果上（10.7以上）

显示搜索结果的路径：将光标游移到相应的搜索结果上并按
<font color="red">Command+Option</font>

在搜索结果中跳转分类：<font color="red">Command+上箭头；Command+下箭头</font>



### Mac 终端 SSH 访问远端服务器配置

打开<font color="red"> **访达** </font>，跳出窗口，然后按  <font color="red"> **苹果键 + shift +G** </font> 跳出窗口，输入<font color="red">  **~/.ssh**  </font>跳出窗口，进行配置即可