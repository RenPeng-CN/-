#Linux CentOS 文件目录结构

###Linux centos 重启命令：

　　1、<font color="red">reboot</font>

　　2、<font color="red">shutdown -r now</font> #立刻重启(root用户使用)

　　3、<font color="red">shutdown -r 10</font>  #过10分钟自动重启(root用户使用)

　　4、<font color="red">shutdown -r 20:35</font> #在时间为20:35时候重启(root用户使用)

​        5、<font color="red"> init 6</font>

　　如果是通过shutdown命令设置重启的话，可以用shutdown -c命令取消重启



###Linux centos关机命令

　　1、<font color="red">halt</font> # 立刻关机

　　2、<font color="red">poweroff</font>  # 立刻关机

　　3、<font color="red">shutdown -h now</font> # 立刻关机(root用户使用)

　　4、<font color="red">shutdown -h 10</font> # 10分钟后自动关机

​        5、 <font color="red">init 0</font>

​           

### Linux CentOS 查看日志命令

```js
journalctl -xe // 日志控制，查看日志
```





对于linux新手来说，最感到迷惑的问题之一就是文件都存在哪里呢?特别是对于那些从windows转过来的新手来说，linux的目录结构看起来有些奇怪哦。所以，在这里讲一下linux下的主要目录以及它们都是用来干什么的。

 

 **/**  : **这就是根目录**。对你的电脑来说，有且只有一个根目录。所有的东西，我是说所有的东西都是从这里开始。举个例子：当你在终端里输入“/home”，你其实是在告诉电脑，先从/(根目录)开始，再进入到home目录。

根目录，文件的最顶端，/etc、/bin、/dev、/lib、/sbin 应该和根目录放在同一个分区，/usr/local 可以单独放置一个分区

 

**/bin** = User Binaries （用户的基本命令）。 这里存放了标准的(或者说是缺省的)linux的工具，比如像“ls”、“vi”还有“more”等等。通常来说，这个目录已经包含在你的“path”系统变量里面了。什么意思呢?就是：当你在终端里输入ls，系统就会去/bin目录下面查找是不是有ls这个程序。ls，cp，mkdir 等，usr/bin 也存放了一些系统命令，这些命令对应的文件都是可执行的，普通用户可以使用大部分的命令，但不用于基本的启动（譬如，在紧急维护中）

 

**/boot**  = Boot Loader Files  （系统引导装载文件，内核）# Linux的内核及引导系统程序所需要的文件目录，比如 vmlinuz initrd.img 文件都位于这个目录中。在一般情况下，GRUB或LILO系统引导管理器也位于这个目录。

 

**/data**  = 存放每天不断增长的日志

**/dev** = Device Files（设备文件）。这里主要存放与设备(包括外设)有关的文件(unix和linux系统均把设备当成文件)。想连线打印机吗?系统就是从这个目录开始工作的。另外还有一些包括磁盘驱动、USB驱动等都放在这个目录。下面的是一些硬件设备驱动

 

**/etc** = Configuration Files 。（配置文件）（etcetera 等等；附加物；附加的人；以及其它）  这里主要存放了 CentOS系统配置方面的文件, 和安装在 CentOS 上的各种软件的配置文件。 举个例子：你安装了samba这个套件，当你想要修改samba配置文件的时候，你会发现它们(配置文件)就在/etc/samba目录下。 # 等等其它的意思。目录存放着各种系统配置文件, 类似于windows下的system

 

**/home**  = Home Directories （用户主目录）系统默认的用户主目录, 这里主要存放你的个人数据。具体每个用户的设置文件，用户的桌面文件夹，还有用户的数据都放在这里。每个用户都有自己的用户目录，位置为：/home/用户名。当然，root用户除外。

 

**/lib** =System Libraries #存放着系统最基本的动态链接共享库，其作用类似于Windows里的.[dll文件](https://www.baidu.com/s?wd=dll%25E6%2596%2587%25E4%25BB%25B6&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)。几乎所有的应用程序都须要用到这些共享库。

**/lib64**  = 

 

**/lost+found** = （碎片文件）在ext2或ext3文件系统中，当系统意外崩溃或机器意外关机，而产生一些文件碎片放在这里。当系统启动的过程中fsck工具会检查这里，并修复已经损 坏的文件系统。 有时系统发生问题，有很多的文件被移到这个目录中，可能会用手工的方式来修复，或移到文件到原来的位置上。

 

**/media**  = Removable Devices （移动设备挂在点） 有些linux的发行版使用这个目录来挂载那些usb接口的移动硬盘(包括U盘)、CD/DVD驱动器等等。

 

**/mnt**  = Mount Directory（临时文件系统挂载点）这个目录一般是用于存放挂载储存设备的挂载目录的，比如有cdrom 等目录。可以参看/etc/fstab的定义。有时我们可以把让系统开机自动挂载文件系统，把挂载点放在这里也是可以的。主要看/etc/fstab中怎 么定义了；比如光驱可以挂载到/mnt/cdrom 。

 

**/opt** = Optional add-on Apps （第三方应用程序的安装位置） 这里主要存放那些可选的程序。你想尝试最新的firefox测试版吗?那就装到/opt目录下吧，这样，当你尝试完，想删掉firefox的时候，你就可以直接删除它，而不影响系统其他任何设置。安装到/opt目录下的程序，它所有的数据、库文件等等都是放在同个目录下面。/opt表示的是可选择的意思，有些软件包也会被安装在这里，也就是自定义软件包，比如在Fedora Core 5.0中，OpenOffice就是安装在这里。有些我们自己编译的软件包，就可以安装在这个目录中；通过源码包安装的软件，可以通过 ./configure --prefix=/opt/目录 。在Linux中，/opt目录是存放某些大型软件或者某些特殊软件的目录，比如谷歌浏览器(Google Chrome)默认就是安装在/opt中。但是我们一般不会把opt单独分在一个区，因为/opt中大多数时候是空的，即使安装了软件也不会太多，而且有 些软件的容量还比较大，这样就会占用/的容量，我们可以在其它你愿意的地方建立一个目录来将/opt“转移”到别处，比如我的的/usr是单独分在一个 区，容量有50G，这么大的空间不要浪费了不是？而且/usr本来就是安装软件的地方，所以我可以/usr下建立一个叫opt的文件夹，然后右键点击这个 /usr下的opt，选择“创建链接”，得到一个名为“到 opt 的链接”文件，然后把这个文件剪切到/下，将原来的/opt删除，再将“到 opt 的链接”改名为opt就可以了，以后我们安装在/opt的软件实际上是安装到了/usr/opt下（实际上是一个符号链接）。

​    举个例子：刚才装的测试版firefox，就可以装到/opt/firefox_beta目录下，/opt/firefox_beta目录下面就包含了运 行firefox所需要的所有文件、库、数据等等。要删除firefox的时候，你只需删除/opt/firefox_beta目录即可，非常简单。

 

**/proc** = Process Information (用于输出内核与进程信息相关的虚拟文件系统) 存放操作系统运行时的运行信息，如进程信息、内核信息、网络信息，如/etc/cpuinfo存放CPU的相关信息。 #这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。也就是说，这个目录的内容不在硬盘上而是在内存里。操作系统运行时，进程信息及内核信息（比如cpu、硬盘分区、内存信息等）存放在这里。/proc目录伪装的文件系统proc的挂载目录，proc并不是真正的文件系统，它的定义可以参见 /etc/fstab 。

 

**/root** = Boot Loader Files （系统管理员(root user)的目录）对于系统来说，系统管理员就好比是上帝，它能对系统做任何事情，甚至包括删除你的文件。因此，请小心使用root帐号。

 

**/run** =

**/sbin** = System Binaries（管理类的基本命令）也就是说这里存放的是系统管理员使用的管理程序。大多是涉及系统管理的命令的存放，是超级权限用户root的可执行命令存放地，普通用户无权限执行这个目录下的命令，这个目录和/usr/sbin; /usr/X11R6/sbin或/usr/local/sbin目录是相似的；我们记住就行了，凡是目录sbin中包含的都是root权限才能执行的。

 

**/srv** = Service Data （系统运行的服务用到的数据）

**/sys** = （用于输出当前系统上硬件设备相关信息的虚拟文件系统）目录与/proc类似，是一个虚拟的文件系统，主要记录与系统核心相关的信息，入系统当前已经载入的模块信息等。这个目录实际不占磁盘容量。

 

**/tmp** = Temporary Files （临时文件存放地）这是临时目录。对于某些程序来说，有些文件被用了一次两次之后，就不会再被用到，像这样的文件就放在这里。有些linux系统会定期自动对这个目录进行清理，因此，千万不要把重要的数据放在这里。

 

**/usr** = User Programs （共享的只读数据）这是最庞大的目录，我们要用到的应用程序和文件几乎都存放在这个目录下。在这个目录下，你可以找到那些不适合放在/bin或/etc目录下的额外的工具。比如像游戏阿，一些打印工具拉等等。/usr目录包含了许多子目录： /usr/bin目录用于存放程序;/usr/share用于存放一些共享的数据，比如音乐文件或者图标等等;/usr/lib目录用于存放那些不能直接 运行的，但却是许多程序运行所必需的一些函数库文件。你的软件包管理器(应该是“新立得”吧)会自动帮你管理好/usr目录的。/usr这个是系统存放程序的目录，比如命令、帮助文件等。这个目录下有很多的文件和目录。当我们安装一个Linux发行版官方提供的软件包时，大多安装在这里。 如果有涉及服务器配置文件的，会把配置文件安装在/etc目录中。/usr目录下包括涉及字体目录/usr/share/fonts ，帮助目录 /usr/share/man或/usr/share/doc，普通用户可执行文件目录/usr/bin 或/usr/local/bin 或/usr/X11R6/bin ，超级权限用户root的可执行命令存放目录，比如 /usr/sbin 或/usr/X11R6/sbin 或/usr/local/sbin 等；还有程序的头文件存放目录/usr/include。

 

 

**/usr** 包含以下子目录； 

**/usr/X11R6** 　　#存放[X-Window](https://www.baidu.com/s?wd=X-Window&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)的目录； 
 **/usr/bin** 　#存放着许多应用程序； 
 **/usr/sbin**  # 给超级用户使用的一些管理程序就放在这里； 
 **/usr/doc** 　#这是Linux文档的大本营； 
 **/usr/include**  #Linux下开发和编译应用程序需要的头文件，在这里查找； 
 **/usr/lib**  #存放一些常用的动态链接共享库和静态档案库； 


 **/usr/local**  　#这是提供给一般用户的/usr目录，在这里安装软件最适合；这里主要存放那些手动安装的软件，即不是通过“新立得”或apt-get安装的软件。它和/usr目录具有相类似的目录结构。让软件包管理器来管理/usr目录，而把自定义的脚本(scripts)放到/usr/local目录下面，我想这应该是个不错的主意。/usr/local这个目录一般是用来存放用户自编译安装软件的存放目录；一般是通过源码包安装的软件，如果没有特别指定安装目录的话，一般是安装在这个目录中。这个目录下面有子目录。自己看看吧。

 

**/usr/share** 系统共用的东西存放地，比如 /usr/share/fonts 是字体目录，/usr/share/doc和/usr/share/man帮助文件。 
 **/usr/man**  #manual手册，这里就是帮助文档的存放目录； 
 **/usr/src**  #Linux开放的源代码就存在这个目录，爱好者们别放过哦！ 

**/var = Variable Files （变化的数据文件**）这个目录中存放着那些不断在扩充着的东西，为了保持/usr的相对稳定，那些经常被修改的目录可以放在这个目录下，实际上许多系统管理员都是这样干的。顺带说一下系统的日志文件就在/var/log目录中。这个目录的内容是经常变动的，看名字就知道，我们可以理解为vary的缩写，/var下有/var/log 这是用来存放系统日志的目录。/var/www目录是定义Apache服务器站点存放目录；/var/lib 用来存放一些库文件，比如MySQL的，以及MySQL数据库的的存放地； 

**/var/log** 系统日志存放，分析日志要看这个目录的东西；

**/var/spool** 打印机、邮件、代理服务器等假脱机目录；







































**Linux 命令大全**



**一、用于文件管理的命令**



**cat** #( concatenate 连锁 ) concatenate files and print on the standard output
 **chattr**     # （change attributes 改变属性）改变文件属性  chattr +i /etc/resolv.conf   防止系统中某个关键文件被修改：

**chgrp**   # ( change group 改变群组 ) 命令用于变更文件或目录的所属群组， chgrp -v bin log2012.log  改变文件的群组属性：

**chmod**  # ( change mode 改变模式 ) chmod ugo+r file1.txt

**chown**   #  ( change owner 改变所属者 ) chown runoob:users file1.txt  将文件 file1.txt 的拥有者设为 users 群体的使用者 runoob

**cksum**    # ( check sum )  cksum testfile1 计算文件”testfile1”的完整性,  确保文件从一个系统传输到另一个系统的过程中不被损坏

**cmp**   # （ compare 比较）cmp prog.o.bak prog.o 



**diff**    # （ difference 区别）diff log2014.log log2013.log  以逐行的方式，比较文本文件的异同处



**diffstat**   **# (** **differential status 不同状态** **)**  diff test1 test2 | diffstat   #进行比较结果的统计显示 读取diff的输出结果，然后统计各文件的插入，删除，修改等差异计量



**file**  （**file** 文件 ） **#** 实例  file -b install.log  通过file指令，我们得以辨识该文件的类型。



**find**   **#** **（** find **查找）实例**  find . -name “*.c"    find命令用来在指定目录下查找文件



**git**   **#** （**git** 饭桶，无用的人） git命令是文字模式下的文件管理员  git是用来管理文件的程序



**gitview**   **( git view** 观察 **)** **#** gitview -c /home/rootlocal/demo.txt      #使用gitview指令以彩色模式观看指定文件内容。 会同时显示十六进制和ASCII格式的字码。



**Indent**   **#** （**indent**  缩进，缩排）  indent可辨识C的原始代码文件，并加以格式化，以方便程序设计师阅读。



**cut**    **#**（**cut** 剪切 ）  cut命令用于显示每行从开头算起 num1 到 num2 的文字



**ln**   **#**   （ link  链接 ）ln -s log2013.log link2013    为log2013.log文件创建软链接link2013，如果log2013.log丢失，link2013将失效, 去掉 -s 参数，则为硬链接 log2013.log与ln2013的各项属性相同



**less**    **#** （**less** 少） less log2013.log 浏览文件 less 与 more 类似，但使用 less 可以随意浏览文件，而 more 仅能向前移动，却不能向后移动，而且 less 在查看之前不会加载整个文件。



**locate**    **#** ( locate 定位 )  用于查找符合条件的文档，他会去保存文档和目录名称的数据库内，查找合乎范本样式条件的文档或目录



**lsattr**    **# (** **list** **attributes** 列出属性**)**  lsattr /etc/resolv.conf 显示文件属性



**mattrib**    **# (** **MS-DOS** **attributes || DOS**文件属性 **)**mattrib -h -s -r a:msdos.sys 除去 A 槽磁片上 msdos.sys 档案的隐藏、系统与唯读属性, mattrib为mtools工具指令  用来变更或显示MS-DOS文件的属性。



**mc**    **#** (  Midnight Commander 午夜指挥官 ) mc命令用于提供一个菜单式的文件管理程序



**mdel**    **# （** **MS-DOS** delete m 删除）mdel a:autoexec.bat 将 A 槽磁片根目录中的 autoexec.bat 删除。  mdel命令用来删除 MSDOS 格式的文件



**mdir**   **# （** **MS-DOS** directory   目录**）**mdir命令用于显示MS-DOS目录



**mktemp**    **# (** make temporary 建立临时 **)**  mktemp命令用于建立暂存文件



**more**     **# （** **more 更多** ）命令类似 cat ，不过会以一页一页的形式显示，更方便使用者逐页阅读



**mmove**    **#  (** **MS-DOS** move 移动 ) mmove autorun.bat test  将文件”autorun.bat"移动到目录"test"中, 命令用于在MS-DOS文件系统中，移动文件或目录，或更改名称。



**mread**   **# (** **MS-DOS** read 读取**)**    mread a:\* ./     #将a盘上的所有文件复制到当前工作目录 ,用于将MS-DOS文件复制到Linux/Unix的目录中。



**mren**   **# (** **MS-DOS** rename 改名**)** mren a:\autorun.bat auto.bat  #将文件autorun.bat重命名为auto.bat  。 mren命令用于更改MS-DOS文件或目录的名称，或是移动文件或目录。



**mtools**   **# （** **MS-DOS tools** **）**mtools命令用于显示mtools支持的指令。#显示所支持的MS-DOS命令



**mtoolstest**   **# （** **MS-DOS** tools test || DOS 工具测试**）**用于测试并显示mtools的相关设置, mtoolstest命令用于测试并显示mtools的相关设置



**mv**    **# (** move 移动 **)** mv aaa bbb  将文件 aaa 更名为 bbb ，mv命令用来为文件或目录改名、或将文件或目录移入其它位置。



**od**    **# （** Octal Dump 八进制 转储**）**od -b tmp od指令会读取所给予的文件的内容，并将其内容以八进制字码呈现出来。



**paste**    **#** paste -s file   paste指令会把每个文件以列对列的方式，一列列地加以合并



**patch**   **# (** 修补 **)** patch -p0 testfile1 testfile.patch  将文件"testfile1"升级，其升级补丁文件为"testfile.patch   patch指令让用户利用设置修补文件的方式，修改，更新原始文件



**rcp**    **#** （remote file copy 远程文件拷贝）rcp root@218.6.132.5:./ testfile testfile  #复制远程文件到本地 



**rm**   **#  （** remove 移除 **）** rm  test.txt    删除文件可以直接使用rm命令，若删除目录则必须配合选项”-r"

​              <font color='red'>rm -rf 目录名称</font>  #删除文件夹及所有子文件

​                     -r （ recursion 递归）就是向下递归，不管有多少级目录，一并删除
​                     -f 就是直接强行删除，不作任何提示的意思 

**slocate**   **# (** Secure Locate 安全查找/定位  Security Enhanced version of the GNU Locate **)** slocate命令查找文件或目录。

slocate本身具有一个数据库，里面存放了系统中文件与目录的相关信息。 slocate fdisk #显示文件名中含有fdisk关键字的文件的路径信息

**split**    **# (** 分离 **)**  split -6 README       #将README文件每六行分割成一个文件  split命令用于将一个文件分割成数个。



**tee**    **#** T (T形管分离器) tee命令用于读取标准输入的数据，并将其内容输出成文件。**T-shaped pipe splitter.**

 























**tmpwatch**   **# (** temporary watch 临时监督 **)**tmpwatch 24 /tmp/ #删除/tmp目录中超过一天未使用的文件，使用指令”tmpwatch”删除目录"/tmp"中超过一天未使用的文件



**touch**   **#** ( touch触摸 )**例：touch testfile**  用于修改文件或者目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会建立一个新的文件。



**umask**   **# (**  **User's MASK 用户的掩码** **)**umask命令指定在建立文件时预设的权限掩码。



**which**   **#** （which 哪个）**例：which bash**   which指令会在环境变量$PATH设置的目录里查找符合条件的文件。



**cp**   **# （** **copy 复制** **）**cp –r test/ newtest  使用指令"cp"将当前目录"test/"下的所有文件复制到新目录"newtest"下



**whereis**   **#**（**where is** 哪里是**...**）  **例：whereis bash**   whereis命令用于查找文件



**mcopy**   **# (** **MS-DOS copy** **)****例： mcopy a:autoexec.bat**   mcopy命令用来复制 MSDOS 格式文件到 Linux 中





**mshowfat**   # ( MS-DOS **show fat || DOS** 显示 **fat** ) **例：mshowfat autorun.bat**   用于显示MS-DOS文件在FAT中的记录。



**rhmask**   **# (** **red hat mask** **)** **例：rhmask code.txt demo.txt**  rhmask命令用于对文件进行加密和解密操作



**scp**   **# （****secure copy 安全复制****）**例：scp local_file remote_username@remote_ip:remote_folder 。 scp命令用于Linux之间复制文件和目录



**awk**   **#** **（三位创始人 Alfred Aho，Peter Weinberger, 和 Brian Kernighan 的Family Name的首字符）**例：awk '{print $1,$4}' log.txt   每行按空格或TAB分割，输出文本中的1、4项



**read**   **#**（**read** 读取） **例：read -p "输入网站名:" website**     read命令用于从标准输入读取数值



**updatedb**   **# （****update database 升级数据库****）****例：updatedb -U /root/runoob/**  updatedb 命令用来创建或更新 slocate/locate 命令所必需的数据库文件







**二、文档编辑**

**col**   **#** **（ Character Outline Limit  字符轮廓限制 ）例：man man | col-b > man_help** 将man 命令的帮助文档保存为man_help，使用-b 参数过滤所有控制字符， 过滤文本中的控制格式的字符



**colrm**    **# （** **column remove 删除列** **）****例：colrm 4 6**  删除指定列的内容。如删除第4列到第6列的内容



**comm**   **#** ( **common** **共同点** ) **例：comm aaa.txt bbb.txt**  comm命令用于比较两个已排过序的文件。



**csplit**   **# (** **context split 分割文件**  **)** **例：csplit testfile 2**  将文件依照指定的范本样式予以切割后，分别保存成名称为xx00,xx01,xx02…的文件



**ed**   **# (** **editor 编辑器** **)** ed命令是文本编辑器，用于文本编辑



**egrep**   **# (** **extended global search a regular expression and print 扩展的正则表达** **)** **例：egrep Linux \*** 查找当前目录下所有文件中包含字符串”Linux"的文件



**ex**   **# (**  **extended 扩展** **)** 例：**ex testfile 编辑testfile文件** ex命令用于在Ex模式下启动vim文本编辑器。ex执行效果如同vi -E，使用语法及参数可参照vi指令，如要从Ex模式回到普通模式，则在vim中输入”:vi"或":visual"指令即可



**fgrep**   **# (** **Fixed GREP  固定的** **grep** **)** 相当于执行grep指令加上参数”-F"



**fmt**   **# (** **format 格式化** **)** **例：fmt -w 85 testfile** 将文件testfile重新排成85 个字符一行，并在标准输出设备上输出。  fmt命令用于编排文本文件



**fold**   **# (** **fold 折叠** **)** **例：fold -w 30 testfile**  将一个名为testfile 的文件的行折叠成宽度为30  fold命令用于限制文件列宽。



**grep**   **# (** **global search regular expression(RE) and print out the line, 全面搜索正则表达式并把行打印出来** **)** **例：grep -r update /etc/acpi #以递归的方式查找“etc/acpi”**



**ispell**   **# (** **interactive spelling checking 交互式拼写检查** **)** **例：ispell testfile 检查testfile文件的拼写**，ispell命令用于拼写检查程序



**jed**   **# (** **jed 是一个文本编译器的名字** **)** **例：jed main.c  用jed编辑器打开main.c 文件****。**  jed命令用于编辑文本文件。Jed是以Slang所写成的程序，适合用来编辑程序原始代码



**joe**   **# (** **Joe's Own Editor Joe这个人的文本编译器** **)** **例：joe main.c** 利用joe命令编辑文本文件



**join**  **# (** **join 连接 合并** **)** **例：join testfile_1 testfile_2**  **将两个文件中指定字段的内容相同的行连接起来**    join命令用于将两个文件中，指定栏位内容相同的行连接起来



**look**  **# (** **look 看****)** **例：look L testfile  查找以“L”开头的单词** look命令用于查询单词。look指令用于英文单字的查询。您仅需给予它欲查询的字首字符串，它会显示所有开头字符串符合该条件的单字。



**mtype**  **# (** **MS-DOS type  显示文件内容****)** **例：mtype dos.txt 打开名为dos.txt 的MS-DOS文件** mtype为mtools工具指令，模拟MS-DOS的type指令，可显示MS-DOS文件的内容



**pico**   **# (** **PIne's message Composition editor | Plne的消息作文编辑器** **)** **例：pico testfile 编辑 testfile文件**  pico命令用于编辑文字文件。pico是个简单易用、以显示导向为主的文字编辑程序，它伴随着处理电子邮件和新闻组的程序pine而来。



**rgrep**   **# (** **recursive grep 递归grep** **) 例：****rgrep Hello \* 在当前目录下查找句子中包含“Hello”字符串的文件** rgrep命令用于递归查找文件里符合条件的字符串



**sed**   **# (** **Stream EDitor  流编辑器** **)** **例：sed -e 4a\newline testfile  #使用sed 在第四行后添加新字符串**   sed命令是利用script来处理文本文件，它一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。文件内容并没有改变，除非你使用重定向存储输出。sed主要用来自动编辑一个或多个文件，简化对文件的反复操作，编写转换程序等。



**sort**   **# (** **sort 分类 排序** **)** **例：sort testfile 用sort命令以默认的式对文件的行进行排序** sort可针对文本文件的内容，以行为单位来排序



**spell**   **# (** **spell 拼写** **)** **例：spell testfile  检查文件testfile是否有拼写错误** spell命令可建立拼写检查程序。spell可从标准输入设备读取字符串，结束后显示拼错的词汇。



**tr**   **# (** **translate character 转化字符****)** **例：cat testfile |tr a-z A-Z 将文件testfile中的小写字母全部转换成大写字母**   tr 命令用于转换或删除文件中的字符。tr 指令从标准输入设备读取数据，经过字符串转译后，将结果输出到标准输出设备



**expr**   **# (** **expression 表达式** **)** **例：expr length “this is a test”  计算字串长度**  expr命令是一个手工命令行计数器，用于在UNIX/LINUX下求表达式变量的值，一般用于整数值，也可用于字符串。



**uniq**   **# (** **unique 唯一的，独有的** **)** **例：uniq testfile  删除testfile内容里重复的行列**  uniq 命令用于检查及删除文本文件中重复出现的行列，一般与 sort 命令结合使用。



**wc**   **# (** **Word Count 词计数** **)** **例：wc testfile    wc将计算指定文件的行数、字数，以及字节数**   wc命令用于计算字数。利用wc指令我们可以计算文件的Byte数、字数、或是列数，若不指定文件名称、或是所给予的文件名为”-"，则wc指令会从标准输入设备读数据。



**let**   **# (** **let** **让 )** **例：let no+=10，let no-=20，分别等同于 let no=no+10，let no=no-20。** let 命令是 BASH 中用于计算的工具，用于执行一个或多个表达式，变量计算中不需要加上 $ 来表示变量。如果表达式中包含了空格或其他特殊字符，则必须引起来。





**三、文件传输**

**lprm**   **# (** **Line Printer Remove 行式打印机移除** **)****例：lprm -Phpprinter 1123**  **将打印机 hpprinter 中的第 1123 号工作移除**   **lprm命令用于将一个工作由打印机贮列中移除**



**lpr**   **# (** **Line Printer 行式打印机 按行打印** **)** **例： lpr -P mailroom report**  **将在名为mailroom的打印机上打印report文件**  实用程序用来将一个或多个文件放入打印队列等待打印



**lpq**   **# (** **Line Printer Queue，Statistics 行式打印机队列,统计数据** **)**  lpq命令用于查看一个打印队列的状态，该程序可以查看打印机队列状态及其所包含的打印任务



**lpd**   **# (** **Line Printer Daemon 行式打印机守护进程****)** lpd命令 是一个常驻的打印机管理程序，它会根据 /etc/printcap 的内容来管理本地或远端的打印机



**bye**   **# (** **bye 再见** **)** bye命令用于中断FTP连线并结束程序。



**ftp**   **# (** **File Transfer Protocol 文件传输协议****)** **例：ftp ftp.kernel.org #发起链接请求**    tp命令设置文件系统相关功能



**uuto**   **# (** **Unix to Unix** **)** uuto命令将文件传送到远端的UUCP主机



**uupick**   **# (** **Unix to Unix pick || uu 拾取****)** **例： uupick-s localhost 处理由主机localhost传送过来的文件**  uupick命令处理传送进来的文件。

当其他主机通过UUCP将文件传送进来时，可利用uupick指令取出这些文件

**uucp**   **# (** **Unix to Unix Copy Protocol || Unix 到 Unix 拷贝协议** **)** **例：uucp-d-R temp localhost ~/Public/    将temp/目录下所有文件传送到远程主机localhost的uucp公共目录下的Public/目录下**  uucp命令用于在Unix系统之间传送文件。UUCP为Unix系统之间，通过序列线来连线的协议。uucp使用UUCP协议，主要的功能为传送文件。



**uucico**   **# ( Unix to Unix 通信程序)**  uucico命令UUCP文件传输服务程序。uucico是用来处理uucp或uux送到队列的文件传输工具



**tftp**   **# (** **Trivial File Transfer Protocol  简单文件传输协议** **)** **例：tftp 218.28.188.288 #连接远程服务器**  tftp命令用于传输文件， tftp是简单的文字模式ftp程序，它所使用的指令和FTP类似



**ncftp**   **# (****NcFTP是一个文件传输客户端程序****)** **例：ncftp ftp.kernel.org    使用ncftp命令匿名连接FTP服务器。** NcFTP是文字模式FTP程序的佼佼者，它具备多样特色， 包括显示传输速率，下载进度，自动续传，标住书签，可通过防火墙和代理服务器等。当不指定用户名时，ncftp 命令会自动尝试使用匿名账户anonymous 去连接远程FTP 服 务器，不需要用户输入账号和密码



**ftpshut**   **# (** **FTP shut || FTP关闭** **)** **例：ftpshut-d 3 -1 5 1100 "Server will be shutdown at 23:00:00” 在晚上11:00 关闭FTP服务器，并在关闭前5 分钟拒绝新的FTP登录，前3 分钟关闭所有ftp的链接，且给出警告信息**   ftpshut命令在指定的时间关闭FTP服务器



**ftpwho**   **# (** **FTP Who || FTP 谁** **)** ftpwho命令用于显示目前所有以FTP登入的用户信息



**ftpcount**   **# (** **FTP Count || FTP 计数** **)**  ftpcount 查询当前FTP用户的人数







**四、磁盘管理**

**cd**   **# (** **Change Directory 改变目录** **)** cd命令用于切换当前工作目录至 dirName(目录参数)

**df**   **# (** **Disk Free** **磁盘空闲** **)** Disk Total=容量总计 Disk Free=可用容量 ，显示目前在Linux系统上的文件系统的磁盘使用情况统计。



**dirs**   **# (** **directories 目录** **)****例：dir -l /home/cc/Ruijie  列出”/Ruijie”里所有内容的详细信息**   dirs命令用于显示目录记录。显示目录堆叠中的记录。



**du**   **# (** **Disk Usage 磁盘使用** **)** du命令用于显示目录或文件的大小



**edquota**   **# (** **edit quota 编辑配额** **)** edquota命令用于编辑用户或群组的磁盘配额。edquota预设会使用vi来编辑使用者或群组的磁盘配额设置



**eject**   **# (** **eject 弹出** **)** eject命令用于退出抽取式设备。若设备已挂入，则eject会先将该设备卸除再退出



**mcd**   **# (** **MS-DOS Change Directory || doc下改变工作目录** **)** mcd为mtools工具指令，可在MS-DOS文件系统中切换工作目录。若不加任何参数，则显示目前所在的磁盘与工作目录



**mdeltree**   **# (** **MS-DOS delete tree**   **删除DOS目录及其内容  tree 可能是 树目录****)** mdeltree命令可用来删除 MSDOS 格式档案及目录。



**mdu**   **# (** **MS-DOS Disk Usage || dos磁盘使用** **)** mdu命令用于显示MS-DOS目录所占用的磁盘空间。



**mkdir**   **# (** **Make Directory 创建目录** **)** **例：mkdir -p BBB/Test     在工作目录下的 BBB 目录中，建立一个名为 Test 的子目录。 若 BBB 目录原本不存在，则建立一个**    mkdir命令用于建立名称为 dirName 之子目录



**mlabel**   **# (** **make an MSDOS volume label 创建一个dos容量标签** **)** **例：mlabel a:newlabel  将 A 盘的标签更改为 newlabel** mlabel命令用于设定磁盘的标签 (Label)



**mmd**   **# (** **MS-DOS make an MSDOS subdirectory 创建一个DOS子目录** **)** mmd命令用于在MS-DOS文件系统中建立目录



**mrd**   **# (** **MS-DOS** **remove an MSDOS subdirectory 删除一个DOS子目录** **)** mrd命令用于删除MS-DOS文件系统中的目录。



**mzip**   **# (** **MSDOS Zip** **)** mzip命令是Zip/Jaz磁盘驱动器控制指令。mzip为mtools工具指令，可设置Zip或Jaz磁盘驱动区的保护模式以及执行退出磁盘的动作



**pwd**   **# (** **Print Working Directory 打印工作目录** **)** pwd指令可立刻得知您目前所在的工作目录的绝对路径名称。



**quota**   **# (** **quota 配额，限额** **)** quota指令，可查询磁盘空间的限制，并得知已使用多少空间



**mount**   **# (** **mount 挂载** **)****例：mount /dev/hda1/mnt     将 /dev/hda1 挂在 /mnt 之下。** mount命令是经常会使用到的命令，它用于挂载Linux系统外的文件。



**mmount**   **# (** **MSDOS** **mount || DOS 挂载** **)** mmount命令用于挂入MS-DOS文件系统  mmount为mtools工具指令，可根据[mount参数]中的设置，将磁盘内容挂入到Linux目录中



**rmdir**   **# (** **remove directory 删除目录** **)** **例：rmdir AAA   将工作目录下，名为 AAA 的子目录删除**   rmdir命令删除空的目录



**rmt**   **# (** **remote magatape protocol | module 远程磁带协议** **)** mt命令通过进程间通信远程控制磁带机。通过rmt指令，用户可通过IPC连线，远端操控磁带机的倾倒和还原操作



**stat**   **# (** **status 状态** **)** stat命令用于显示inode内容。stat以文字的格式来显示inode的内容。



**tree**   **# (** **tree 树** **)** tree命令用于以树状图列出目录的内容



**umount**   **# (** **Unmount 卸载** **)****例：umount -v /mnt/mymount/   通过挂载点卸载**  umount可卸除目前挂在Linux目录中的文件系统



**ls**   **#  (** **list 列表 列出** **)**  ls命令用于显示指定工作目录下之内容（列出目前工作目录所含之文件及子目录)



**quotackeck**   **# (** **quota check 限额检查** **)** quotacheck命令用于检查磁盘的使用空间与限制



**quotaoff**   **# (** **quota off 限额关闭** **)** **例：quotaoff -a   关闭配额限制**   quotaoff指令可关闭用户和群组的磁盘空间限制



**lndir**   **# (** **link directory 链接目录** **)** **例：lndir /home/uptech abc   给目录下所有的文件或者子文件目录建立链接**  lndir指令，可一口气把源目录底下的文件和子目录统统建立起相互对应的符号连接



**repquota**  **# (** **report quota 报告配额** **)** repquota指令，可报告磁盘空间限制的状况，清楚得知每位用户或每个群组已使用多少空间n 



**quotaon**   **# (****quota on 开启限额** **)** quotaon指令可开启用户和群组的才磅秒年空间限制，各分区的文件系统根目录必须有quota.user和quota.group配置文件。





**五、磁盘维护**

**badblocks**  **# (** **bad blocks 坏区块** **)** **例：badblocks -s -v /dev/sdnx  通过命令扫描硬盘**  检查磁盘装置中损坏的区块



**cfdisk**   **# (** **Curses based Format DISK 基于多视窗处理方式的格式化磁盘** **)** cfdisk是用来磁盘分区的程序，它十分类似DOS的fdisk，具有互动式操作界面而非传统fdisk的问答式界面，您可以轻易地利用方向键来操控分区操作。



**dd**   **# (** **Disk Dump** **磁盘转储** **)** **例：dd if= boot.img of=/dev/fd0 bs=1440k  在Linux 下制作启动盘，可使用如下命令** dd可从标准输入或文件中读取数据，根据指定的格式来转换数据，再输出到文件、设备或标准输出



**e2fsck**   **# (** **ext2/ext3/ext4 file system check || 检查linux ext2文件系统** **)** **例：e2fsck -a -y /dev/hda5   检查 /dev/hda5 是否正常，如果有异常便自动修复，并且设定若有问答，均回答[是] :** e2fsck命令用于检查使用 Linux ext2 档案系统的 partition 是否正常工作



**ext2ed**   **# (** **ext2/ext3/ext4 file system edit** **)** ext2ed可直接处理硬盘分区上的数据，这指令只有Red Hat Linux才提供



**fsck**   **# (** **file system check 文件系统检查** **)** **例：fsck -t msdos -a /dev/hda5  检查 msdos 档案系统的 /dev/hda5 是否正常，如果有异常便自动修复 :**    fsck命令用于 检查与修复 Linux 档案系统，可以同时检查一个或多个 Linux 档案系统。



**fsck.minix**   **# (** **file system check. Minix  || minix 是文件系统的名称** **)** fsck.minix命令用于检查文件系统并尝试修复错误。当minix文件系统发生错误时，可用fsck.minix指令尝试加以参考。



**fsconf**   **# (** **file system config  文件系统配置** **)** fsconf命令用于设置文件系统相关功能。 f conf 是Red Hat Linux发行版专门用来调整Linux各项设置的程序



**fdformat**   **# (** **floppy disk format  软盘格式化** **)** **例：fdformat -n /dev/fd0h1440  将磁碟机 A 的磁片格式化成 1.4MB 的磁片。并且省略确认的步骤。** Low-level formats a floppy disk   用于对指定的软碟机装置进行低阶格式化



**hdparm** **# (** **hard disk parameters 硬盘参数** **) 例：****hdparm /dev/sda  显示硬盘的相关设置**  get/set SATA/IDE device parameters   显示/设定硬盘的参数**。**



**mformat**  **# (** **MS-DOS format || DOS 格式化** **)** mformat命令用于对MS-DOS文件系统的磁盘进行格式化



**mkbootdisk**  **# (** **make boot disk 建立系统启动盘** **)** mkbootdisk命令用于建立目前系统的启动盘



**mkdosfs**  **# (** **make DOS File System 建立Dos 文件系统** **)** mkdosfs命令用于建立DOS文件系统



**mke2fs**   **# (** **make ext2 File System 建立ext2文件系统** **)** mke2fs命令用于建立ext2文件系统



**mkfs.ext2**   **# ()** **功能说明：与** **mke2fs命令** **相同**



**mkfs.msdos**   **# ()** **功能说明：与** **mkdosfs 命令** **相同**



**mkinitrd**   **# (** **make initial ramdisk images  制作初始化内存盘的映像文件****)** creates initial ramdisk images for preloading modules  为预加载模块创建初始内存盘映像  建立要载入ramdisk的映像文件



**mkisofs**   **# (** **make iso file system 制作iso文件系统** **)** mkisofs可将指定的目录与文件做成ISO 9660格式的映像文件，以供刻录光盘



**mkswap**   **# (****make swap  || 制作交换区** **)**set up a Linux swap area  设置一个linux的交换区域     可将磁盘分区或文件设为Linux的交换区



**mpartition**   **# (** **MS-DOS partition  || DOS 分区** **)** mpartition为mtools工具指令，可建立或删除磁盘分区



**swapon**  **# (** **swap On  || 激活swap交换区** **)** swapon命令用于激活Linux系统中交换空间，Linux系统的内存管理必须使用交换区来建立虚拟内存



**symlinks**   **# (** **symbolic link 符号链接** **)**  symbolic link maintenance utility  符号链路维护实用程序   可检查目录中的符号连接，并显示符号连接类型。



**sync**   **# (** **sync 同步** **)**  sync命令用于数据同步,sync命令是在关闭Linux系统时使用的



**mbadblocks**   **# (**  **MS-DOS bad blocks** **|| DOS 坏区块** **)**   mbadblocks命令用于检查MS-DOS文件系统的磁盘是否有损坏的磁区



**mkfs.minix**   **# (** **make file system.minix 建立文件系统. Minix** **)** mkfs.minix命令用于建立Minix文件系统



**fsck.ext2**   **# (** **file system check.ext2  文件系统检查.ext2** **)**  fsck.ext2命令用于检查文件系统并尝试修复错误。当ext2文件系统发生错误时，可用fsck.ext2指令尝试加以修复。



**fdisk**   **# (** **format disk  格式化磁盘** **) Partition table manipulator for Linux  用于Linux的分区表操作器**     fdisk是一个创建和维护分区表的程序，它兼容DOS类型的分区表、BSD或者SUN类型的磁盘列表



**losetup**   **# (** **loop setup 循环设置** **) set up and control loop devices  建立和控制循环设备**    losetup命令用于设置循环设备。循环设备可把文件虚拟成区块设备，籍以模拟整个文件系统，让用户得以将其视为硬盘驱动器，光驱或软驱等设备，并挂入当作目录来使用



**mkfs**   **# (** **make file system 建立文件系统** **) build a Linux file system  构建一个Linux文件系统**   mkfs命令用于在特定的分区上建立 linux 文件系统



**sfdisk**   **# ( Smart Format Disk** 聪明格式化磁盘 **) Partition table manipulator for Linux 用于Linux的分区表操作器**  sfdisk为硬盘分区工具程序，可显示分区的设置信息，并检查分区是否正常



**swapoff**   **# (** **swap off  关闭交换区** **) swapon, swapoff - enable/disable  devices and files for paging and swapping**            || swapoff实际上为swapon的符号连接，可用来关闭系统的交换区。











**六、通讯网络**

**apachectl**  **# (** **apache control 阿帕奇服务器控制** **)** apachectl是 slackware 内附Apache HTTP服务器的script文件，可供管理员控制服务器，但在其他Linux的Apache HTTP服务器不一定有这个文件。



**arpwatch**   **# (** **ARP watch || ARP监听** **)** **例：arpwatch -i eth0 监听网卡eth0的ARP信息**   arpwatch命令用于监听网络上ARP的记录， ARP(Address Resolution Protocol 地址解决协议 )是用来解析IP与网络装置硬件地址的协议



**dip**   **# (** **dial up ip  拨号IP** **)** **例：dip -t 建立拨号连接**  dip命令用于IP拨号连接。dip可控制调制解调器，以拨号IP的方式建立对外的双向连接。



**getty**   **# (** **get TeleType 获得电传打字设备** **)** **例：getty tty7 开启终端**  **getty“Get TTY”( TTY=****TeleType** **)的处理过程是:一个程序监视物理的TTY/终端接口。** getty命令用于设置终端机模式，连线速率和管制线路。getty指令是UNIX之类操作系统启动时所必须的3个步骤之一。



**mingetty**   **# (** **mini getty  精简版的getty** **)** mingetty命令是精简版的getty。mingetty适用于本机上的登入程序。



**uux**   **# (** **Unix to Unix execute** **执行** **)** **例：uux hnlinux date  /// 在远程主机 指定date命令查看系统时间**  uux可在远端的UUCP主机上执行指令或是执行本机上的指令，但在执行时会使用远端电脑的文件



**telnet**   **# (** **Terminal over Network 终端越过网络 ，理解为越过网络登录终端** **)** **例： telnet 192.168.0.5  //登录IP为 192.168.0.5 的远程主机**   telnet命令用于远端登入。执行telnet指令开启终端机阶段作业，并登入远端主机



**uulog**   **# (** **Unix to Unix  log 记录、日志** **)**  uulog命令用于显示UUCP记录文件。uulog可用来显示UUCP记录文件中记录。



**uustat**   **# (** **status 状态** **)****例：uustat -a 显示所有任务**   uustat命令用于显示UUCP目前的状况。执行uucp与uux指令后，会先将工作送到队列，再由uucico来执行工作。uustat可显示，删除或启动队列中等待执行的工作



**ppp-off**  **# (** **Point to Point Protocol Off 点对点协议关闭** **)** ppp命令用于关闭ppp连线。这是Slackware发行版内附的程序，让用户切断PPP的网络连线



**netconfig**   **# ( net config**  **网络配置** **)** netconfig命令用于设置网络环境。这是Slackware发行版内附程序，它具有互动式的问答界面，让用户轻易完成网络环境的设置。



**nc**   **# (** **netcat 网猫** **)** **例：nc -v -z -w2 192.168.0.3 1-100  扫描192.168.0.3 的端口 范围是 1-100**   设置路由器的相关参数



**httpd**   **# (** **Hyper Text Transfer Protocol Daemon 超文本传输协议守护进程** **)** httpd命令是Apache HTTP服务器程序。httpd为Apache HTTP服务器程序。直接执行程序可启动服务器的服务



**ifconfig**   **# (** **interface config 接口设置** **)** ifconfig命令用于显示或设置网络设备。ifconfig可设置网络设备的状态，或是显示目前的设置



**minicom**   **# (** **mini communication 迷你通信** **)** minicom命令是用于调制解调器通信的程序。minicom是一个相当受欢迎的PPP拨号连线程序



**mesg**   **# (** **message** **消息** **)** mesg命令用于设置终端机的写入权限。将mesg设置y时，其他用户可利用write指令将信息直接显示在您的屏幕上



**dnsconf**   **# (** **Domain Name Service Config 域名服务器配置** **)** dnsconf命令用于设置DNS服务器组态。dnsconf实际上为linuxconf的符号连接，提供图形截面的操作方式，供管理员管理DNS服务器。



**wall**   **# (** **write all 写给全部** **)** **例：wall I love  广播消息 I Love**   wall命令会将讯息传给每一个 mesg 设定为 yes 的上线使用者。当使用终端机介面做为标准传入时, 讯息结束时需加上 EOF (通常用 Ctrl+D)。



**netstat**   **# (** **net status 网络状态** **)** **例：netstat -a 显示详细的网络状况**   netstat命令用于显示网络状态。利用netstat指令可让你得知整个Linux系统的网络情况



**ping**   **# (** **拟声词** **)** ping命令用于检测主机。执行ping指令会使用ICMP传输协议，发出要求回应的信息，若远端主机的网络功能没有问题，就会回应该信息，因而得知该主机运作正常。 ping这个名字本身就有些色彩缤纷。一些人声称它是一个缩写， packet internet groper 代表包互联网探索者，但事实并非如此。ping是以声纳跟踪系统发出的声音命名的。甚至有一个故事声称，一个系统管理员编写了一个脚本，该脚本反复ping网络上的主机，并对每次成功发出“ping”警报。然后，系统管理员能够系统地检查网络中的BNC连接器，直到找到一直困扰他的网络的可疑连接器。当噪音停止时，他找到了元凶



**pppstats**   **# (** **point to point protocol status 点对点协议状态** **)** 可让你得知PPP连接网络的相关信息。



**samba**   **# ( samba** 是一种服务器名称 **)** **samba命令用于Samba服务器控制**。samba为script文件，可启动，停止Samba服务器或回报目前的状态，SMB（Server Message Block）通信协议是微软（Microsoft）和[英特尔](https://www.baidu.com/s?wd=%25E8%258B%25B1%25E7%2589%25B9%25E5%25B0%2594&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)(Intel)在1987年制定的协议，主要是作为Microsoft网络的通讯协议，Samba则是将[SMB协议](https://www.baidu.com/s?wd=SMB%25E5%258D%258F%25E8%25AE%25AE&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)搬到UNIX上来应用；通过“NetBIOS over [TCP/IP](https://www.baidu.com/s?wd=TCP%252FIP&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)”使得Samba不但能与局域网络主机分享资源，更能与全世界的电脑分享资源；因为互联网上千千万万的主机所使用的通讯协议就是[TCP/IP](https://www.baidu.com/s?wd=TCP%252FIP&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)。



**setserial**   **# (** **Setup Serial 设置串口** **)** **例：setserial -g /dev/ttyS2**   **显示串口信息**    setserial命令用于设置或显示串口的相关信息。setserial可用来设置串口或显示目前的设置。



**talk**   **# (** **talk 谈话** **)****例：talk Rollaend  与 Rollaend 谈话**   talk命令用于与其他使用者对谈。使用权限：所有使用者。



**traceroute**   **# (** **trace route 追踪路由** **)** **例：traceroute** [**www.google.com**](http://www.google.com)  **显示到达目的地的数据包路由**  traceroute命令用于显示数据包到主机间的路径。traceroute指令让你追踪网络数据包的路由途径，预设数据包大小是40Bytes，用户可另行设置。



**tty**   **# (** **teletypewriter 电传打字机** **)** tty命令用于显示终端机连接标准输入设备的文件名称。在Linux操作系统中，所有外围设备都有其名称与代号，这些名称代号以特殊文件的类型存放于/dev目录下。你可以执行tty(teletypewriter)指令查询目前使用的终端机的文件名称。



**newaliases**   **# (** **new aliases  新的别名** **)**newaliases命令会使用一个在 /etc/aliases 中的档案做使用者名称转换的动作。当 sendmail 收到一个要送给 xxx 的信时，它会依据 aliases档的内容送给另一个使用者。



**uuname**   **# (****uucp name  UUCP服务器的名字** **)** uuname命令用于显示全部的UUCP远端主机。uuname可显示UUCP远端主机。



**netconf**  **# (** **net config 网络配置** **)** netconf命令用于设置各项网络功能。netconf是Red Hat Linux发行版专门用来调整Linux各项设置的程序



**write**   **# (** **write 写** **)** write命令用于传讯息给其他使用者。使用权限：所有使用者



**statserial**   **# (** **status serial 状态串口** **)** **例：statserial /dev/tty1  显示串口状态**   statserial命令用于显示串口状态。statserial(status ofserial port)可显示各个接脚的状态，常用来判断串口是否正常。



**efax**   **# (** **e fax 网络传真，e 不知是什么单词的缩写** **)**   efax命令用于收发传真。支持Class 1与Class 2的调制解调器来收发传真



**pppsetup**   **# (** **point to point protocol setup || PPP设置** **)** pppsetup命令用于设置PPP连线。这是Slackware发行版内附程序，它具有互动式的问答界面，让用户轻易完成PPP的连线设置



**tcpdump**  **# (** **Transmission Control Protocol dump 传输控制协议倾倒** **)**  tcpdump命令用于倾倒网络传输数据。执行tcpdump指令可列出经过指定网络界面的数据包文件头，在Linux操作系统中，你必须是系统管理员。



**ytalk**   **# (** **y talk 交谈  y 不知道是什么单词的缩写** **)** ytalk是一个多用户交谈程序。通过ytalk指令，你可以和其他用户线上交谈，如果想和其他主机的用户交谈，在用户名称后加上其主机名称或IP地址即可



**cu**   **# (** **call up 打电话给** **)** cu命令用于连接另一个系统主机。cu(call up)指令可连接另一台主机，并采用类似拨号终端机的接口工作，也可执行简易的文件传输作业。



**smbd**   **# (** **samba daemon 守护进程** **)** smbd命令用于Samba服务器程序。smbd为Samba服务器程序，可分享文件与打印机等网络资源供Windows相关的用户端程序存取



**testparm**   **#**  ( **test parameter 测试参数** )指令可以简单测试Samba的配置文件，假如测试结果无误，Samba常驻服务就能正确载入该设置值，但并不保证其后的操作如预期般一切正常。



**smbclient**   **# (** **samba client || samba客户** **)**smbclient命令可存取SMB/CIFS服务器的用户端程序。SMB与CIFS为服务器通信协议，常用于Windows95/98/NT等系统。smbclient(samba client)可让Linux系统存取Windows系统所分享的资源。



**shapecfg**   **# ( shaper configuration 定型配置 )**  shapecfg命令用于管制网络设备的流量。自Linux-2.15开始，便支持流量管制的功能。







**七、系统管理命令**



**adduser**   **# (** **add user 增加用户** **)**

**chfn**  **# (** **change finger information 改变finger信息****)**

**useradd**   **# (** **user add 用户增加** **)**

**date**   **# (** **date 日期** **)**

**exit**   **# (** **exit 退出** **)**

**finger**   **# (** **finger** 手指**)**

**fwhios**   **# (** **find WhoIs ||查找 WhoIS数据库** **)**

**suspend**   **# (** **suspend 挂起、暂停** **)**

**groupdel**   **# (** **group delete 群组删除** **)**

**groupmod**  **# (** **group modify 群组修改** **)**

**halt**   **#  (** **halt 停止** **)**

**kill**  **# (** **kill 杀死，终止** **)**

**last**   **# ()**

**lastb**   **# (** **last boot** **)**

**login**   **# (** **login 登录** **)**

**logname**   **# (** **login name 登录名** **)**

**logout**   **# (** **login out 登出** **)**

**ps**    **# (** **process status 进程状态** **)**

**nice**   **# ()**

**procinfo**  **# ( process information** 进程信息 **)**

**top**   **# ()**

**pstree**   **# (** **process status tree 进程状态树** **)**

**reboot**   **# ()**

**rlogin**   **# (** **remote login 远程登录** **)**

**rsh**   **# ( remote shell 远端壳)**

**sliplogin**   **# (** **SLIP login 串行线路网际协议 登录** **)**

**screen**   **# ()**

**shutdown**   **# ()**

**rwho**   **# (** **r who 谁  不知道r是什么缩写** **)**

**sudo**   **# (** **super user do 超级用户做** **)**

**gitps**  #( **gnu interactive tools process status  || gnu 交互工具进程状态** )

**swatch**   **# (** **simple watcher 简单的监视器** **)**

**tload**   **# ( t load   负载 t 或许是 text )**

**logrotate**   **# ( rotates, compresses, mails system logs  旋转、压缩和邮件系统日志)**

**uname**   **# (** **Unix name 名称** **)**

**chsh**   **# (** **change shell 更换壳** **)**

**userconf**   **# (** **user config 用户设置** **)**

**userdel**  **# ()**

**usermod**   **# (** **user modify用户修改** **)**

**vlock**   **# ( virtual console lock 虚拟控制台锁定 )**

**who**   **# ( who 谁 )**

**whoami**   **# (** **who am I 我是谁** **)**

**whois**   **# (** **who is  谁是** **)**

**newgrp**   **# (** **new group 新群组** **)**

**renice**   **# (** **renice 调整已存在进程的nice** **)**



**su**   **# (** **switch user 变更用户** **)**

**skill**   **# ()**

**w**  **# (** **what 什么 Show who is logged on and what they are doing 显示谁登陆了和他们正在做什么** **)**

**id**   **# (** **identification 身份证明** **)**

**free**   **# ( free 空闲的)**









**八、系统设置**

**reset**  **# (** **reset命令其实和 tset 是一同个命令，它的用途是设定终端机的状态** **)**

**clear**   **# (** **clear 清除** **)**

**alias**   **# (** **alias 别名** **)**

**dircolors**   **# (** **directory colors 目录颜色** **)**

**aumix**   **# ( audio mixer 音频混合器 )**

**bind**   **# (** **bind 绑定** **)**

**chroot**   **# (** **change root 变更根目录** **)**

**clock**   **# (** **clock 时钟** **)**

**crontab**   **# (** **crontab 定时任务** **)**

**declare**   **# (** **declare 声明** **)**

**depmod** **#(** **depend module 依赖模块** **)**

**dmesg**   **# (** **display message (or display driver), 即显示消息** **)**

**enable**   **# (** **enable 启用** **)**

**eval**   **# ( eval 重新运算求出参数的内容 )**

**export**   **# (** **export 导出** **)**

**pwunconv**   **# (** **PassWord Unconvert 密码无转换** **)**

**grpconv**   **# (** **group convert to shadow password 群组转换投影密码** **)**

**rpm**   **# （** **redhat package manager 红帽子包管理器** **）**

**insmod**   **# (** **install module 安装模块**  **)**

**kbdconfig**  **# (**  **keyboard config 键盘设置** **)**

**lilo**   **# (** **linux loader || linux装载器** **)**

**liloconfig**   **# (** **linux loader** **config || linux装载器配置** **)**

**lsmod**   **# (** **list modules 列表模块** **)**

**minfo**   **# (** **MS-DOS Information || DOS 信息** **)**

**set**   **# (** **set 设置** **)**

**modprobe**   **# (** **module probe 模块探测** **)**   

**ntsysv**   **# (** **ntsysv -> NeWT + SysV ，它是使用 newt 库的 SysV 风格的 runlevel 配置工具** **)**

**mouseconfig**   **# (** **mouse config 鼠标设置** **)**

**passwd**   **# (** **password 密码** **)**

**pwconv**   **# ( password convert || 密码转换 )** 

**rdate**   **# (** **receive date 接收日期** **)**

**resize**   **# (** **resize 调整大小** **)**

**rmmod**   **# (** **remove module 移除模块** **)**

**grpunconv**   **# (** **group unconvert from shadow password 关闭群组的投影密码** **)**

**modinfo**   **# (** **module information 模块信息** **)**

**time**   **# (** **time 时间** **)**

**setup**   **# (** **setup 设置** **)**

**sndconfig**   **# (** **sound config 声音配置** **)**

**setenv**   **# (** **set environment variable 设置环境变量** **)**



**setconsole**   **# (** **set console 设置控制台** **)**

**timeconfig**   **# (** **time config 时间配置** **)**

**ulimit**   **# (** **User's LIMIT 用户限制 ？？****有疑问 )**

**unset**   **# (** **unset 解除设置** **)**

**chkconfig**   **# (** **check config 检查配置** **)**

**apmd**   **# (** **advanced power management BIOS daemon 进阶电源管理服务程序** **)**

**hwclock**   **# (** **hardware clock 硬件时钟** **)**

**mkkickstart**   **# (** **make kick start 建立kickstart 组态文件** **)**

**fbset**   **# (** **frame buffer setup  景框缓冲区的设置** **)**

**unailas**   **# ()**

**SVGATextMode**   **# (** **super video graphics array Text Mode 超级视频图形阵列文字模式** **)**







**九、备份压缩**

 **ar**   **# ( archive 存档 )**

**bunzip2**   **# (** **bunzip2实际上是bzip2的符号连接** **)**

**bzip2**   **# (** **bzip2命令是.bz2文件的压缩程序** **)**

**bzip2recover**  **# ( bzip2 recover || bzip2 恢复 )**

**gunzip**   **# ( gnu unzip )**

**unarj**   **# ( 解压缩 arj 文件)**

**compress**   **# (** **compress 压缩** **)**

**cpio**   **# (** **copy in/out 拷贝 入/出** **)**

**dump**  **# (** **dump 转储** **)**

**uuencode**   **# (** **Unix to Unix encode 编码** **)**

**gzexe**  **# (** **gzip executable || gzip 可执行的** **)**

**gzip**  **# ( gnu zip )**

**lha**   **# ()**

**restore**  **# (** **restore 恢复** **)**

**tar**  **# (** **tape archive 磁带档案卷** **)**

**uudecode**  **# (** **Unix to Unix decode 解码** **)**

**unzip**   **# ()**

**zip**   **# ()**

**zipinfo**   **# (** **zip information 压缩信息** **)**



**十、设备管理**

**setleds**   **# (** **set LED 设置 LED灯** **)**

**loadkeys**   **# (** **load keys 输入键** **)**

**rdev**   **# (** **root device 根设备** **)**

**dumpkeys**   **# (** **dump keys 转变键** **)**

**MAKEDEV**   **#** **( make device )**









**十一、其它命令**

**ss**  **#****（socket statistics  socket统计信息 ）**

网络上的两个程序通过一个双向的通信连接实现数据的交换，这个连接的一端称为一个socket。

建立网络通信连接至少要一对端口号(socket)。socket本质是编程接口(API)，对TCP/IP的封装，TCP/IP也要提供可供程序员做网络开发所用的接口，这就是Socket编程接口；HTTP是轿车，提供了封装或者显示数据的具体形式；Socket是发动机，提供了网络通信的能力。

Socket的英文原义是“孔”或“插座”。作为BSD UNIX的[进程通信](https://baike.baidu.com/item/%E8%BF%9B%E7%A8%8B%E9%80%9A%E4%BF%A1/9796867)机制，取后一种意思。通常也称作"[套接字](https://baike.baidu.com/item/%E5%A5%97%E6%8E%A5%E5%AD%97)"，用于描述IP地址和端口，是一个通信链的句柄，可以用来实现不同虚拟机或不同计算机之间的通信。在Internet上的[主机](https://baike.baidu.com/item/%E4%B8%BB%E6%9C%BA/455151)一般运行了多个服务软件，同时提供几种服务。每种服务都打开一个Socket，并绑定到一个端口上，不同的端口对应于不同的服务。Socket正如其英文原义那样，像一个多孔插座。一台主机犹如布满各种插座的房间，每个插座有一个编号，有的插座提供220伏交流电， 有的提供110伏交流电，有的则提供有线电视节目。 客户软件将插头插到不同编号的插座，就可以得到不同的服务。



ss命令可用于查看系统的socket的状态。socket（也叫套接字）最初是在Unix系统上开发的网络通信的接口，它就是一个函数库，里面包括大量的函数和相应的数据结构，已经实现好了。它支持网络通信。程序开发人员可以通过阅读相关的函数文档，了解函数的使用方法，进行网络的编程。两种形式的socket：流式套接字，对应与[TCP协议](https://www.baidu.com/s?wd=TCP%E5%8D%8F%E8%AE%AE&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)。数据报套接字，对应与[UDP协议](https://www.baidu.com/s?wd=UDP%E5%8D%8F%E8%AE%AE&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)。centos系统下，SS指令为系统自带，无需安装。ss命令用于显示socket状态. 他可以显示PACKET sockets, TCP sockets, UDP sockets, DCCP sockets, RAW sockets, Unix domain sockets等等统计. 它比其他工具展示等多tcp和state信息. 它是一个非常实用、快速、有效的跟踪IP连接和sockets的新工具.SS命令可以提供如下信息：

n——不解析服务名称。

r——解析主机名。

l——显示监听状态的套接字。

a——显示所有套接字。

o——显示计时器信息

e——显示详细的套接字（socket）的内存使用情况。

p——显示使用套接字的进程。

i——显示 tcp 内部信息。

s——显示套接字（socket）使用概况。

4——仅显示 IPv4的套接字。

6——只显示IPv6 套接字（-f inet6的别名）。

0——显示 PACKET 套接字。

t——仅显示 TCP 套接字。

u——仅显示 UDP套接字。

d——仅显示 DCCP 套接字。

w——RAW 套接字。

x——仅显示 Unix 套接字。

f——显示 FAMILY 类型的套接字。

D——将原始TCP 套接字信息转储到文件。

F——从文件中都去过滤信息。



**ss -t -a**  # 显示系统目前ICP连接的连接情况

**ss -aA udp**   #显示目前系统中UDP的连接情况。

**ss -lnt**   #只显示监听的套接字

















**Linux CentOS** **操作实用命令**



yum info nginx   # 通过yum查看软件详细信息

lscpu                  # 显示 cpu 相关信息

ps aux                # 显示进程状态

netstat -lnp | grep 80   # 查看 80 端口被哪个程序占用



curl -i [www.jd.com](http://www.jd.com)   # 查看京东网站首页响应报头内容

-A/--user-agent <string>              设置用户代理发送给服务器

-b/--cookie <name=string/file>    cookie字符串或文件读取位置

-c/--cookie-jar <file>                    操作结束后把cookie写入到这个文件中

-C/--continue-at <offset>            断点续转

-D/--dump-header <file>              把header信息写入到该文件中

-e/--referer                                  来源网址

-f/--fail                                          连接失败时不显示http错误

-o/--output                                  把输出写到该文件中

-O/--remote-name                      把输出写到该文件中，保留远程文件的文件名

-r/--range <range>                      检索来自HTTP/1.1或FTP服务器字节范围

-s/--silent                                    静音模式。不输出任何东西

-T/--upload-file <file>                  上传文件

-u/--user <user[:password]>      设置服务器的用户和密码

-w/--write-out [format]                什么输出完成后

-x/--proxy <host[:port]>              在给定的端口上使用HTTP代理

-#/--progress-bar                        进度条显示当前的传送状态











**linux****下的各种端口用途**



一、查看进程端口  


\# 查看所有打开的端口及服务名（注意这里显示的服务名只是标准端口对应的服务名，可能并不准确）

nmap localhost



\# 查看哪些进程打开了指定端口port（对于守护进程必须以root用户执行才能查看到）

lsof -i:port           #  +sudo比较好



\# 查看哪些进程打开了指定端口port，最后一列是进程ID（此方法对于守护进程作用不大）

netstat -nap | grep port



\# 查看端口号对应的系统服务名称, 还可以查看缩写的全称

cat /etc/services



\# 启动｜停止｜重启系统服务

sudo /etc/init.d/service start|stop|restart



\#我通常使用 sudo netstat -anp > port.txt  


二、常见端口详细说明 
 服务及对应端口　　　 　　　　　　服务及对应端口　　　 
 Echo（7）　　　　　　　　　　　　　　　　　FTP（21） 
 Ssh（22）　　　　　　　　　　　　　　　　　Telnet（23） 
 SMTP（25）　　　　　　　　　　　　　　　　DNS（53） 
 HTTP（80）　　　　　　　　　　　　　　　　 MTA-X.400 over TCP/IP（102） 
 pop3（110）　　　　　　　　　　　　　　　　NETBIOS Name Service（137、138、139） 
 IMAP v2（143）　　　　　　　　　　　　　　  SNMP（161） 
 LDAP、ILS（389）　　　　　　　　　　　　　 Https（443） 
 IMAP(993)                                　　　　　　　　SQL(1433) 
 NetMeeting T.120(1503)                   　　　　　  NetMeeting(1720) 
 NetMeeting Audio Call Control(1731)      　　　  超级终端(3389)  
 QQ客户端(4000)                           　　　　　　  pcAnywere(5631) 
 RealAudio(6970)                           　　　　　　  Sygate (7323) 
 OICQ(8000)                               　　　　　　　  Wingate(8010) 
 代理端口(8080) 
 1、端口：7 
 服务：Echo 
 说明：能看到许多人搜索Fraggle放大器时，发送到X.X.X.0和X.X.X.255的信息。 


 2、端口：21 
 服务：FTP 
 说明：FTP服务器所开放的端口，用于上传、下载。最常见的攻击者用于寻找打开anonymous的FTP服务器的方法。这些服务器带有可读写的目录。木马Doly Trojan、Fore、Invisible FTP、WebEx、WinCrash和Blade Runner所开放的端口。


 3、端口：22 
 服务：Ssh 
 说明：PcAnywhere建立的TCP和这一端口的连接可能是为了寻找ssh。这一服务有许多弱点，如果配置成特定的模式，许多使用RSAREF库的版本就会有不少的漏洞存在。 


 4、端口：23 
 服务：Telnet 
 说明：远程登录，入侵者在搜索远程登录UNIX的服务。大多数情况下扫描这一端口是为了找到机器运行的操作系统。还有使用其他技术，入侵者也会找到密码。木马Tiny Telnet Server就开放这个端口。


 5、端口：25 
 服务：SMTP 
 说明：SMTP服务器所开放的端口，用于发送邮件。入侵者寻找SMTP服务器是为了传递他们的SPAM。入侵者的帐户被关闭，他们需要连接到高带宽的E-MAIL服务器上，将简单的信息传递到不同的地址。木马Antigen、Email Password Sender、Haebu Coceda、Shtrilitz Stealth、WinPC、WinSpy都开放这个端口。


 6、端口：53 
 服务：Domain Name Server（DNS）
 说明：DNS服务器所开放的端口，入侵者可能是试图进行区域传递（TCP），欺骗DNS（UDP）或隐藏其他的通信。因此防火墙常常过滤或记录此端口。 


 7、端口：80 
 服务：HTTP 
 说明：用于网页浏览。木马Executor开放此端口。 


 8、端口：102 
 服务：Message transfer agent(MTA)-X.400 over TCP/IP 
 说明：消息传输代理。 


 9、端口：110 
 服务：pop3
 说明：POP3(Post Office Protocol 3)服务器开放此端口，用于接收邮件，客户端访问服务器端的邮件服务。POP3服务有许多公认的弱点。关于用户名和密码交换缓冲区溢出的弱点至少有20个，这意味着入侵者可以在真正登陆前进入系统。成功登陆后还有其他缓冲区溢出错误。


 10、端口：137、138、139 
 服务：NETBIOS Name Service 
 说明：其中137、138是UDP端口，当通过网上邻居传输文件时用这个端口。而139端口：通过这个端口进入的连接试图获得NetBIOS/SMB服务。这个协议被用于windows文件和打印机共享和SAMBA。还有WINS Regisrtation也用它。


 11、端口：143 
 服务：Interim Mail Access Protocol v2 
 说明：和POP3的安全问题一样，许多IMAP服务器存在有缓冲区溢出漏洞。记住： 
 一种LINUX蠕虫（admv0rm）会通过这个端口繁殖，因此许多这个端口的扫描来自不知情的已经被感染的用户。当REDHAT在他们的LINUX发布版本中默认允许IMAP后，这些漏洞变的很流行。这一端口还被用于IMAP2，但并不流行。


 12、端口：161 
 服务：SNMP 
 说明：SNMP允许远程管理设备。所有配置和运行信息的储存在数据库中，通过SNMP可获得这些信息。许多管理员的错误配置将被暴露在Internet。Cackers将试图使用默认的密码public、private访问系统。他们可能会试验所有可能的组合。
 SNMP包可能会被错误的指向用户的网络。 


 13、端口：389 
 服务：LDAP、ILS 
 说明：轻型目录访问协议和NetMeeting Internet Locator Server共用这一端口 。


 14、端口：443 
 服务：Https 
 说明：网页浏览端口，能提供加密和通过安全端口传输的另一种HTTP。 


 15、端口：993 
 服务：IMAP 
 说明：SSL（Secure Sockets layer） 


 16、端口：1433 
 服务：SQL 
 说明：Microsoft的SQL服务开放的端口。 


 17、端口：1503 
 服务：NetMeeting T.120 
 说明：NetMeeting T.120 


 18、端口：1720 
 服务：NetMeeting 
 说明：NetMeeting H.233 call Setup。 


 19、端口：1731 
 服务：NetMeeting Audio Call Control 
 说明：NetMeeting音频调用控制。 


 20、端口：3389 
 服务：超级终端
 说明：WINDOWS 2000终端开放此端口。 


 21、端口：4000 
 服务：QQ客户端
 说明：腾讯QQ客户端开放此端口。 


 22、端口：5631
 服务：pcAnywere 
 说明：有时会看到很多这个端口的扫描，这依赖于用户所在的位置。当用户打开pcAnywere时，它会自动扫描局域网C类网以寻找可能的代理（这里的代理是指agent而不是proxy）。入侵者也会寻找开放这种服务的计算机。，所以应该查看这种扫描的源地址。一些搜寻pcAnywere的扫描包常含端口22的UDP数据包。


 23、端口：6970 
 服务：RealAudio 
 说明：RealAudio客户将从服务器的6970-7170的UDP端口接收音频数据流。这是由TCP-7070端口外向控制连接设置的。 


 24、端口：7323 
 服务：[NULL] 
 说明：Sygate服务器端。 


 25、端口：8000 
 服务：OICQ 
 说明：腾讯QQ服务器端开放此端口。 


 26、端口：8010 
 服务：Wingate 
 说明：Wingate代理开放此端口。 


 27、端口：8080
 服务：代理端口
 说明：WWW代理开放此端口。







[国际标准化组织](https://baike.baidu.com/item/%E5%9B%BD%E9%99%85%E6%A0%87%E5%87%86%E5%8C%96%E7%BB%84%E7%BB%87)ISO 于1981年正式推荐了一个网络系统结构----七层参考模型 [1]  ，叫做[开放系统互连](https://baike.baidu.com/item/%E5%BC%80%E6%94%BE%E7%B3%BB%E7%BB%9F%E4%BA%92%E8%BF%9E)模型(Open System Interconnection，OSI)。由于这个标准模型的建立,使得各种计算机网络向它靠拢，大大推动了[网络通信](https://baike.baidu.com/item/%E7%BD%91%E7%BB%9C%E9%80%9A%E4%BF%A1)的发展。





[**网络七层协议的通俗理解**](https://www.cnblogs.com/carlos-mm/p/6297197.html)



![pastedGraphic.png](/Users/renpeng/Library/Application Support/typora-user-images/1E1F9A93-7506-4556-A6F1-AF5CC8BFB185/pastedGraphic.png)

 

![pastedGraphic_1.png](/Users/renpeng/Library/Application Support/typora-user-images/1E1F9A93-7506-4556-A6F1-AF5CC8BFB185/pastedGraphic_1.png)



OSI七层模式简单通俗理解

 

这个模型学了好多次，总是记不住。今天又看了一遍，发现用历史推演的角度去看问题会更有逻辑，更好记。本文不一定严谨，可能有错漏，主要是抛砖引玉，帮助记性不好的人。总体来说，OSI模型是从底层往上层发展出来的。

 

这个模型推出的最开始，是是因为美国人有两台机器之间进行通信的需求。

 

需求1： 物理层  Physical （网线，无线信道）

科学家要解决的第一个问题是，两个硬件之间怎么通信。具体就是一台发些比特流，然后另一台能收到。

 

于是，科学家发明了物理层：

 

主要定义物理设备标准，物理层的媒体包括架空明线、平衡电缆、光纤、无线信道等，网线的接口类型、光纤的接口类型、各种传输介质的传输速率等。它的主要作用是传输比特流(就是由1、0转化为电流强弱来进行传输，到达目的地后在转化为1、0，也就是我们常说的数模转换与模数转换)。这一层的数据叫做比特。

 



需求2： 数据链路层  DATA link  （网卡、网桥）

数据链路可以粗略地理解为数据通道。现在通过电线我能发数据流了，链路层是为网络层提供[数据传送](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E4%BC%A0%E9%80%81)服务的,这种服务要依靠本层具备的功能来实现。然后我还要保证传输过去的比特流是正确的，要有纠错功能。  

于是，发明了数据链路层： 

定义了如何让格式化数据以进行传输，以及如何让控制对物理介质的访问。这一层通常还提供错误检测和纠正，以确保数据的可靠传输。

链路层的功能：链路连接的建立，拆除，分离

独立的链路产品中最常见的当属网卡,网桥也是链路产品。



 

需求3： 传输层 Transport（ 用 TCP 或 UDP 协议 ）

现在我能发正确的发比特流数据到另一台计算机了，但是当我发大量数据时候，可能需要好长时间，例如一个视频格式的，网络会中断好多次（事实上，即使有了物理层和数据链路层，网络还是经常中断，只是中断的时间是毫秒级别的）。

 

那么，我还须要保证传输大量文件时的准确性。于是，我要对发出去的数据进行封装。就像发快递一样，一个个地发。

 

于是，先发明了传输层（传输层在OSI模型中，是在网络层上面）

 

例如TCP，是用于发大量数据的，我发了1万个包出去，另一台电脑就要告诉我是否接受到了1万个包，如果缺了3个包，就告诉我是第1001，234，8888个包丢了，那我再发一次。这样，就能保证对方把这个视频完整接收了。

 

例如UDP，是用于发送少量数据的。我发20个包出去，一般不会丢包，所以，我不管你收到多少个。在多人互动游戏，也经常用UDP协议，因为一般都是简单的信息，而且有广播的需求。如果用TCP，效率就很低，因为它会不停地告诉主机我收到了20个包，或者我收到了18个包，再发我两个！如果同时有1万台计算机都这样做，那么用TCP反而会降低效率，还不如用UDP，主机发出去就算了，丢几个包你就卡一下，算了，下次再发包你再更新。

 

TCP协议是会绑定IP和端口的协议，下面会介绍IP协议。



 

需求4：网络层：Network （ 用 IP协议 网络硬设备主要有[网关](https://baike.baidu.com/item/%E7%BD%91%E5%85%B3)和[路由器](https://baike.baidu.com/item/%E8%B7%AF%E7%94%B1%E5%99%A8)）

 

传输层只是解决了打包的问题。但是如果我有多台计算机，怎么找到我要发的那台？或者，A要给F发信息，中间要经过B，C，D,E，但是中间还有好多节点如K.J.Z.Y。我怎么选择最佳路径？这就是路由要做的事。

 

于是，发明了网络层。即路由器，交换价那些具有寻址功能的设备所实现的功能。这一层定义的是IP地址，通过IP地址寻址。所以产生了IP协议。

  



需求5：会话层 Session ()

 

现在我们已经保证给正确的计算机，发送正确的封装过后的信息了。但是用户级别的体验好不好？难道我每次都要调用TCP去打包，然后调用IP协议去找路由，自己去发？当然不行，所以我们要建立一个自动收发包，自动寻址的功能。

 

于是，发明了会话层。会话层的作用就是建立和管理应用程序之间的通信。

 



需求6：表示层 presentation 

现在我能保证应用程序自动收发包和寻址了。但是我要用Linux给window发包，两个系统语法不一致，就像安装包一样，exe是不能在linux下用的，shell在window下也是不能直接运行的。于是需要表示层（presentation），帮我们解决不同系统之间的通信语法问题。

 



需求7：应用层 Application  ( 用 HTTP 协议 )

 

OK，现在所有必要条件都准备好了，我们可以写个android程序，web程序去实现需求把。





补充： 

Socket：

 

这不是一个协议，而是一个通信模型。其实它最初是伯克利加州分校软件研究所，简称BSD发明的，主要用来一台电脑的两个进程间通信，然后把它用到了两台电脑的进程间通信。所以，可以把它简单理解为进程间通信，不是什么高级的东西。主要做的事情不就是：

 

A发包：发请求包给某个已经绑定的端口（所以我们经常会访问这样的地址182.13.15.16:1235，1235就是端口）；收到B的允许；然后正式发送；发送完了，告诉B要断开链接；收到断开允许，马上断开，然后发送已经断开信息给B。

 

B收包：绑定端口和IP；然后在这个端口监听；接收到A的请求，发允许给A，并做好接收准备，主要就是清理缓存等待接收新数据；然后正式接收；接受到断开请求，允许断开；确认断开后，继续监听其它请求。

 

可见，Socket其实就是I/O操作。Socket并不仅限于网络通信。在网络通信中，它涵盖了网络层、传输层、会话层、表示层、应用层——其实这都不需要记，因为Socket通信时候用到了IP和端口，仅这两个就表明了它用到了网络层和传输层；而且它无视多台电脑通信的系统差别，所以它涉及了表示层；一般Socket都是基于一个应用程序的，所以会涉及到会话层和应用层。



MAC地址（Media Access Control Address），直译为媒体访问控制地址，也称为局域网地址（LAN Address），以太网地址（Ethernet Address）或物理地址（Physical Address），它是一个用来确认网上设备位置的地址。在OSI模型中，第三层网络层…



二进制：Binary system

八进制：octal Number system

十进制：Decimal system

十六进制：Hexadecimal

arp: 地址解析协议（Address Resolution Protocol）是根据IP地址获取物理地址的一个TCP/IP协议





**Linux** **常用命令**



**ss -ntlp**  **#centos** **查看端口占用情况** **（****socket statistics  socket****统计信息** **）**

**ss -t -a**  **#** **显示系统目前****ICP****连接的连接情况**

**ss -aA udp**   **#****显示目前系统中****UDP****的连接情况。**

**ss -lnt**   **#****只显示监听的套接字**

**lscpu**   **#** **列出** **cpu** **的详情**

**ps -aux**  **#** **显示所有进程状态**

**ps axo pid,cmd,psr | grep nginx**  **#** **查看** **nginx** **的进程号****,****进程名称，在哪个****CPU** **上工作。**

**pstree -p**  **#** **以书目录方式查看进程**

**curl** [**www.mengjie.com**](http://www.mengjie.com)  **#** **查看该网址**

**curl I** [**www.jylpcn.cn**](http://www.jylpcn.cn) **#** **查看网站报头信息**

**vim /etc/hosts**    **#** **修改域名服务器列表，在这里可以添加虚拟网址，允许本地端访问。**

**ip a**  **#** **查看网卡不同接口对应的** **ip** **地址。**