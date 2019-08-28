

### Mac 终端安装 MySql

1.先安装brew

```js
https://brew.sh/  //网址：

//复制命令：
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```



2.brew 安装mysql

```js
brew install mysql     //  （可以指定版本安装，不指定版本默认最新版本）
```



3.修改环境变量

```js
// brew安装的东西都是在  
 
/usr/local/Cellar/  

// 路径下，所有需要进到里面找到mysql然后一层一层进去直到找到bin目录，获取这时路径

// 我的电脑是：

/usr/local/Cellar/mysql@5.7/5.7.23/bin    //（可能mysql版本不同，路径不同，自己切换目录找就ok）

//1）终端输入命令
sudo vim .bash_profile

//2）在文档的最下方输入：

export PATH=$PATH:/usr/local/Cellar/mysql@5.7/5.7.23/bin

//然后esc退出insert状态，并在最下方输入:wq保存退出。

//3）输入：

source .bash_profile

//回车执行，运行环境变量。
```



4.其他

```js

//上面的做法每次关掉终端在打开都需要重新source .bash_profile。于是 vi ~/.zshrc，在这里面添加了：

export PATH=${PATH}:/usr/local/Cellar/mysql@5.7/5.7.23/bin

//保存后 source ~/.zshrc 

//这样的话就可以一劳永逸了。
```



1. brew services start mysql (启动)
2. brew services stop mysql (停止)
3. 如果用brew services 启动有问题：`brew tap homebrew/boneyard` 再试一下？

## 中文问题

如要支持 emoji，需把下面的utf8 均改成 utf8mb4

```js
[client]
default-character-set = utf8

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8
collation-server = utf8_general_ci
```



### MySQL 操作命令

常用的MySQL命令大全

**一、连接MySQL**

格式： mysql -h主机地址 -u用户名 －p用户密码

1、例1：连接到本机上的MYSQL。

首先在打开DOS窗口，然后进入目录 mysqlbin，再键入命令mysql -uroot -p，回车后提示你输密码，如果刚安装好MYSQL，超级用户root是没有密码的，故直接回车即可进入到MYSQL中了，MYSQL的提示符是： mysql>。

2、例2：连接到远程主机上的MYSQL。假设远程主机的IP为：110.110.110.110，用户名为root,密码为abcd123。则键入以下命令：

mysql -h110.110.110.110 -uroot -pabcd123

（注:u与root可以不用加空格，其它也一样）

3、退出MYSQL命令： exit （回车）。

**二、修改密码**

格式：mysqladmin -u用户名 -p旧密码 password 新密码

1、例1：给root加个密码ab12。首先在DOS下进入目录mysqlbin，然后键入以下命令：

mysqladmin -uroot -password ab12

注：因为开始时root没有密码，所以-p旧密码一项就可以省略了。

2、例2：再将root的密码改为djg345。

mysqladmin -uroot -pab12 password djg345

**三、增加新用户。**（注意：和上面不同，下面的因为是MySQL环境中的命令，所以后面都带一个分号作为命令结束符）

格式：grant select on [数据库](http://www.cr173.com/k/sql/).* to 用户名@登录主机 identified by \"密码\"

例1、增加一个用户[test](http://www.cr173.com/test.html)1密码为abc，让他可以在任何主机上登录，并对所有数据库有查询、插入、修改、删除的权限。首先用以root用户连入MySQL，然后键入以下命令：

grant select,insert,update,

delete on *.* to test2@localhost identified by \"abc\";

如果你不想test2有密码，可以再打一个命令将密码消掉。

grant select,insert,update,delete on mydb

.* to test2@localhost identified by \"\";

在上面讲了登录、增加用户、密码更改等问题。下面我们来看看MySQL中有关数据库方面的操作。注意：你必须首先登录到MySQL中，以下操作都是在MySQL的提示符下进行的，而且每个命令以分号结束。

1、MySQL常用命令   <font color="red"> </font>

<font color="red"> create database name;</font> 创建数据库

 <font color="red">use databasename;  </font>选择数据库 

<font color="red"> drop database name </font>直接删除数据库，不提醒

<font color="red"> show tables; </font>显示表

<font color="red"> describe tablename; </font>表的详细描述

<font color="red"> select 中加上distinct去除重复字段</font>

<font color="red"> mysqladmin drop database name</font> 删除数据库前，有提示。

显示当前mysql版本和当前日期

<font color="red"> select version(),current_date;</font>

2、修改mysql中root的密码：

<font color="red"> shell>mysql -u root -p</font>

<font color="red"> mysql> update user set password=password(”xueok654123″) where user=’root’;</font>

<font color="red"> mysql> flush privileges</font> //刷新数据库

<font color="red"> mysql>use dbname；</font> 打开数据库：

<font color="red"> mysql>show databases;</font> 显示所有数据库

<font color="red"> mysql>show tables;</font> 显示数据库mysql中所有的表：先use mysql；然后

<font color="red"> mysql>describe user;</font> 显示表[mysql数据库](http://www.cr173.com/k/mysql/)中user表的列信息）；

3、grant

创建一个可以从任何地方连接服务器的一个完全的超级用户，但是必须使用一个口令something做这个

 <font color="red">mysql> grant all privileges on *.* to user@localhost identified by ’something’ with</font>

增加新用户

格式： <font color="red">grant select on 数据库.* to 用户名@登录主机 identified by “密码”</font>

 <font color="red">GRANT ALL PRIVILEGES ON *.* TO monty@localhost IDENTIFIED BY ’something’ WITH  GRANT OPTION;</font>

 <font color="red">GRANT ALL PRIVILEGES ON *.* TO monty@”%” IDENTIFIED BY ’something’ WITH GRANT OPTION;</font>

删除授权：

 <font color="red">mysql> revoke all privileges on *.* from root@”%”;</font>

 <font color="red">mysql> delete from user where user=”root” and host=”%”;</font>

 <font color="red">mysql> flush privileges;</font>

创建一个用户custom在特定客户端it363.com登录，可访问特定数据库fangchandb

 <font color="red">mysql >grant select, insert, update, delete, create,drop on fangchandb.* to custom@ it363.com identified by ‘ passwd’</font>

重命名表:

 <font color="red">mysql > alter table t1 rename t2;</font>

4、mysqldump

备份数据库

 <font color="red">shell> mysqldump -h host -u root -p dbname >dbname_backup.sql</font>

恢复数据库

 <font color="red">shell> mysqladmin -h myhost -u root -p create dbname</font>

 <font color="red">shell> mysqldump -h host -u root -p dbname < dbname_backup.sql</font>

如果只想卸出建表指令，则命令如下：

 <font color="red">shell> mysqladmin -u root -p -d databasename > a.sql</font>

如果只想卸出插入数据的sql命令，而不需要建表命令，则命令如下：

 <font color="red">shell> mysqladmin -u root -p -t databasename > a.sql</font>

那么如果我只想要数据，而不想要什么sql命令时，应该如何操作呢？

　　  <font color="red">mysqldump -T./ phptest driver</font>

其 中，只有指定了-T参数才可以卸出纯文本文件，表示卸出数据的目录，./表示当前目录，即与mysqldump同一目录。如果不指定driver 表，则将卸出整个数据库的数据。每个表会生成两个文件，一个为.sql文件，包含建表执行。另一个为.txt文件，只包含数据，且没有sql指令。

5、可将查询存储在一个文件中并告诉mysql从文件中读取查询而不是等待键盘输入。可利用外壳程序键入重定向实用程序来完成这项工作。例如，如果在文件my_file.sql 中存放有查

询，可如下执行这些查询：

例如，如果您想将建表语句提前写在sql.txt中:

mysql > mysql -h myhost -u root -p database < sql.txt

1、安装环境：

Windows XP

Mysql 4.0.17 从 下次就需要用mysql -uroot -proot才可以登陆

在远程或本机可以使用 mysql -h 172.5.1.183 -uroot 登陆，这个根据第二行的策略确定

权限修改生效：

1)net stop mysql

net start mysql

2)c:\mysql\bin\mysqladmin flush-privileges

3)登陆mysql后，用flush privileges语句

6、创建数据库staffer

create database staffer;

7、下面的语句在mysql环境在执行

显示用户拥有权限的数据库 show databases;

切换到staffer数据库 use staffer;

显示当前数据库中有权限的表 show tables;

显示表staffer的结构 desc staffer;

8、创建测试环境

1)创建数据库staffer

mysql> create database staffer

2)创建表staffer,department,position,depart_pos

create table s_position

(

id int not null auto_increment,

name varchar(20) not null default '经理', #设定默认值

description varchar(100),

primary key PK_positon (id) #设定主键

);

create table department

(

id int not null auto_increment,

name varchar(20) not null default '系统部', #设定默认值

description varchar(100),

primary key PK_department (id) #设定主键

);

create table depart_pos

(

department_id int not null,

position_id int not null,

primary key PK_depart_pos (department_id,position_id) #设定复和主键

);

create table staffer

(

id int not null auto_increment primary key, #设定主键

name varchar(20) not null default '无名氏', #设定默认值

department_id int not null,

position_id int not null,

unique (department_id,position_id) #设定唯一值

);

3)删除

mysql>

drop table depart_pos;

drop table department;

drop table s_position;

drop table staffer;

drop database staffer;

9、修改结构

mysql>

\#表position增加列test

alter table position add(test char(10));

\#表position修改列test

alter table position modify test char(20) not null;

\#表position修改列test默认值

alter table position alter test set default 'system';

\#表position去掉test默认值

alter table position alter test drop default;

\#表position去掉列test

alter table position drop column test;

\#表depart_pos删除主键

alter table depart_pos drop primary key;

\#表depart_pos增加主键

alter table depart_pos add primary key PK_depart_pos (department_id,position_id);

10、操作数据

\#插入表department

insert into department(name,description) values('系统部','系统部');

insert into department(name,description) values('公关部','公关部');

insert into department(name,description) values('客服部','客服部');

insert into department(name,description) values('财务部','财务部');

insert into department(name,description) values('测试部','测试部');

\#插入表s_position

insert into s_position(name,description) values('总监','总监');

insert into s_position(name,description) values('经理','经理');

insert into s_position(name,description) values('普通员工','普通员工');

\#插入表depart_pos

insert into depart_pos(department_id,position_id)

select a.id department_id,b.id postion_id

from department a,s_position b;

\#插入表staffer

insert into staffer(name,department_id,position_id) values('陈达治',1,1);

insert into staffer(name,department_id,position_id) values('李文宾',1,2);

insert into staffer(name,department_id,position_id) values('马佳',1,3);

insert into staffer(name,department_id,position_id) values('亢志强',5,1);

insert into staffer(name,department_id,position_id) values('杨玉茹',4,1);

11、查询及删除操作

\#显示系统部的人员和职位

select a.name,b.name department_name,c.name position_name

from staffer a,department b,s_position c

where a.department_id=b.id and a.position_id=c.id and b.name='系统部';

\#显示系统部的人数

select count(*) from staffer a,department b

where a.department_id=b.id and b.name='系统部'

\#显示各部门的人数

select count(*) cou,b.name

from staffer a,department b

where a.department_id=b.id

group by b.name;

\#删除客服部

delete from department where name='客服部';

\#将财务部修改为财务一部

update department set name='财务一部' where name='财务部';

12、备份和恢复

备份数据库staffer

c:\mysql\bin\mysqldump -uroot -proot staffer>e:\staffer.sql

得到的staffer.sql是一个sql脚本，不包括建库的语句，所以你需要手工

创建数据库才可以导入

恢复数据库staffer,需要创建一个空库staffer

c:\mysql\bin\mysql -uroot -proot staffer<staffer.sql

如果不希望后来手工创建staffer,可以

c:\mysql\bin\mysqldump -uroot -proot --databases staffer>e:\staffer.sql

mysql -uroot -proot >e:\staffer.sql

但这样的话系统种就不能存在staffer库，且无法导入其他名字的数据库，

当然你可以手工修改staffer.sql文件

13、从文本向数据库导入数据

1)使用工具c:\mysql\bin\mysqlimport

这个工具的作用是将文件导入到和去掉文件扩展名名字相同的表里，如

staffer.txt,staffer都是导入到staffer表中

常用选项及功能如下

-d or --delete 新数据导入数据表中之前删除数据数据表中的所有信息

-f or --force 不管是否遇到错误，mysqlimport将强制继续插入数据

-i or --ignore mysqlimport跳过或者忽略那些有相同唯一

关键字的行， 导入文件中的数据将被忽略。

-l or -lock-tables 数据被插入之前锁住表，这样就防止了，

你在更新数据库时，用户的查询和更新受到影响。

-r or -replace 这个选项与－i选项的作用相反；此选项将替代

表中有相同唯一关键字的记录。

--fields-enclosed- by= char

指定文本文件中数据的记录时以什么括起的， 很多情况下

数据以双引号括起。 默认的情况下数据是没有被字符括起的。

--fields-terminated- by=char

指定各个数据的值之间的分隔符，在句号分隔的文件中，

分隔符是句号。您可以用此选项指定数据之间的分隔符。

默认的分隔符是跳格符（Tab）

--lines-terminated- by=str

此选项指定文本文件中行与行之间数据的分隔字符串

或者字符。 默认的情况下mysqlimport以newline为行分隔符。

您可以选择用一个字符串来替代一个单个的字符：

一个新行或者一个回车。

mysqlimport命令常用的选项还有-v 显示版本（version），

-p 提示输入密码（password）等。

这个工具有个问题，无法忽略某些列，这样对我们的数据导入有很大的麻烦，虽然可以手工设置这个字段，但会出现莫名其妙的结果，我们做一个简单的示例

我们定义如下的depart_no.txt，保存在e盘，间隔为制表符\t

10 10

11 11

12 24

执行如下命令

c:\mysql\bin\mysqlimport -uroot -proot staffer e:\depart_pos.txt

在这里没有使用列的包围符号，分割采用默认的\t，因为采用别的符号会有问题，

不知道是不是windows的原因

2)Load Data INFILE file_name into table_name(column1_name,column2_name)

这个命令在mysql>提示符下使用，优点是可以指定列导入，示例如下

c:\mysql\bin\mysql -uroot -proot staffer

mysql>load data infile "e:/depart_no.txt" into depart_no(department_id,position_id);

 

这两个工具在Windows下使用都有问题，不知道是Windows的原因还是中文的问题，

而且不指定的列它产生了空值，这显然不是我们想要的，所以谨慎使用这些工具

进入MySQL:mysql -uuser -ppassword --port=3307

1:使用SHOW语句找出在服务器上当前存在什么数据库：

mysql> SHOW DATABASES;

2:2、创建一个数据库MYSQLDATA

mysql> Create DATABASE MYSQLDATA;

3:选择你所创建的数据库

mysql> USE MYSQLDATA; (按回车键出现Database changed 时说明操作成功！)

4:查看现在的数据库中存在什么表

mysql> SHOW TABLES;

5:创建一个数据库表

mysql> Create TABLE MYTABLE (name VARCHAR(20), sex CHAR(1));

6:显示表的结构：

mysql> DESCRIBE MYTABLE;

7:往表中加入记录

mysql> insert into MYTABLE values ("hyq","M");

8:用文本方式将数据装入数据库表中（例如D:/mysql.txt）

mysql> LOAD DATA LOCAL INFILE "D:/mysql.txt" INTO TABLE MYTABLE;

9:导入.sql文件命令（例如D:/mysql.sql）

mysql>use database;

mysql>source d:/mysql.sql;

10:删除表

mysql>drop TABLE MYTABLE;

11:清空表

mysql>delete from MYTABLE;

12:更新表中数据

mysql>update MYTABLE set sex="f" where name='hyq';

UPDATE [LOW_PRIORITY] [IGNORE] tbl_name

SET col_name1=expr1 [, col_name2=expr2 ...]

[WHERE where_definition]

[ORDER BY ...]

[LIMIT rows]

or

UPDATE [LOW_PRIORITY] [IGNORE] tbl_name [, tbl_name ...]

SET col_name1=expr1 [, col_name2=expr2 ...]

[WHERE where_definition]

UPDATE 以新的值更新现存表中行的列。SET 子句指出要修改哪个列和他们应该给定的值。WHERE

子句如果被给出，指定哪个记录行应该被更新。否则，所有的记录行被更新。如果 ORDER BY 子句被指定，记录行将被以指定的次序更新。

如果你指定关键词 LOW_PRIORITY，UPDATE 的执行将被延迟，直到没有其它的客户端正在读取表。

如果你指定关键词 IGNORE，该更新语句将不会异常中止，即使在更新过程中出现重复键错误。导致冲突的记录行将不会被更新。

如果在一个表达式中从 tbl_name 中访问一个列，UPDATE 使用列的当前值。举例来说，下面的语句设置 age 列值为它的当前值加 1 ：

mysql> UPDATE persondata SET age=age+1;

UPDATE 赋值是从左到右计算的。举例来说，下列语句将 age 列设置为它的两倍，然后再加 1 ：

mysql> UPDATE persondata SET age=age*2, age=age+1;

如果你设置列为其当前的值，MySQL 注意到这点，并不更新它。

UPDATE 返回实际被改变的记录行数目。在 MySQL 3.22 或更新的版本中，C API 函数 mysql_info()

返回被匹配并更新的记录行数目，以及在 UPDATE 期间发生的警告的数目。

在 MySQL 3.23 中，你可以使用 LIMIT # 来确保只有给定的记录行数目被更改。

如果一个 ORDER BY 子句被使用(从 MySQL 4.0.0 开始支持)，记录行将以指定的次序被更新。这实际上只有连同 LIMIT一起才有用。

从 MySQL 4.0.4 开始，你也可以执行一个包含多个表的 UPDATE 的操作：

UPDATE items,month SET items.price=month.price

WHERE items.id=month.id;

注意：多表 UPDATE 不可以使用 ORDER BY 或 LIMIT。

关键字: mysql

启动：net start mySql;

　　进入：mysql -u root -p/mysql -h localhost -u root -p databaseName;

　　列出数据库：show databases;

　　选择数据库：use databaseName;

　　列出表格：show tables；

　　显示表格列的属性：show columns from tableName；

　　建立数据库：source fileName.txt;

　　匹配字符：可以用通配符_代表任何一个字符，％代表任何字符串;

　　增加一个字段：alter table tabelName add column fieldName dateType;

　　增加多个字段：alter table tabelName add column fieldName1 dateType,add columns fieldName2 dateType;

　　多行命令输入:注意不能将单词断开;当插入或更改数据时，不能将字段的字符串展开到多行里，否则硬回车将被储存到数据中;

　　增加一个管理员帐户：grant all on *.* to user@localhost identified by "password";

　　每条语句输入完毕后要在末尾填加分号';'，或者填加'\g'也可以；

　　查询时间：select now();

　　查询当前用户：select user();

　　查询数据库版本：select version();

　　查询当前使用的数据库：select database();

　　

　　1、删除student_course数据库中的students数据表：

　　rm -f student_course/students.*

　　

　　2、备份数据库：(将数据库test备份)

　　mysqldump -u root -p test>c:\test.txt

　　备份表格：(备份test数据库下的mytable表格)

　　mysqldump -u root -p test mytable>c:\test.txt

　　将备份数据导入到数据库：(导回test数据库)

　　mysql -u root -p test

　　

　　3、创建临时表：(建立临时表zengchao)

　　create temporary table zengchao(name varchar(10));

　　

　　4、创建表是先判断表是否存在

　　create table if not exists students(……);

　　

　　5、从已经有的表中复制表的结构

　　create table table2 select * from table1 where 1<>1;

　　

　　6、复制表

　　create table table2 select * from table1;

　　

　　7、对表重新命名

　　alter table table1 rename as table2;

　　

　　8、修改列的类型

　　alter table table1 modify id int unsigned;//修改列id的类型为int unsigned

　　alter table table1 change id sid int unsigned;//修改列id的名字为sid，而且把属性修改为int unsigned

　　

　　9、创建索引

　　alter table table1 add index ind_id (id);

　　create index ind_id on table1 (id);

　　create unique index ind_id on table1 (id);//建立唯一性索引

　　

　　10、删除索引

　　drop index idx_id on table1;

　　alter table table1 drop index ind_id;

　　

　　11、联合字符或者多个列(将列id与":"和列name和"="连接)

　　select concat(id,':',name,'=') from students;

　　

　　12、limit(选出10到20条)<第一个记录集的编号是0>

　　select * from students order by id limit 9,10;

　　

　　13、MySQL不支持的功能

　　事务，视图，外键和引用完整性，存储过程和触发器

　　

　　

　　14、MySQL会使用索引的操作符号

　　<,<=,>=,>,=,between,in,不带%或者_开头的like

　　

　　15、使用索引的缺点

　　1)减慢增删改数据的速度；

　　2）占用磁盘空间；

　　3）增加查询优化器的负担；

　　当查询优化器生成执行计划时，会考虑索引，太多的索引会给查询优化器增加工作量，导致无法选择最优的查询方案；

　　

　　16、分析索引效率

　　方法：在一般的SQL语句前加上explain；

　　分析结果的含义：

　　1）table：表名；

　　2）type：连接的类型，(ALL/Range/Ref)。其中ref是最理想的；

　　3）possible_keys：查询可以利用的索引名；

　　4）key：实际使用的索引；

　　5）key_len：索引中被使用部分的长度（字节）；

　　6）ref：显示列名字或者"const"（不明白什么意思）；

　　7）rows：显示MySQL认为在找到正确结果之前必须扫描的行数；

　　8）extra：MySQL的建议；

　　

　　17、使用较短的定长列

　　1）尽可能使用较短的数据类型；

　　2）尽可能使用定长数据类型；

　　a）用char代替varchar，固定长度的数据处理比变长的快些；

　　b）对于频繁修改的表，磁盘容易形成碎片，从而影响数据库的整体性能；

　　c）万一出现数据表崩溃，使用固定长度数据行的表更容易重新构造。使用固定长度的数据行，每个记录的开始位置都是固定记录长度的倍数，可以很容易被检测到，但是使用可变长度的数据行就不一定了；

　　d）对于MyISAM类型的数据表，虽然转换成固定长度的数据列可以提高性能，但是占据的空间也大；

　　

　　18、使用not null和enum

　　尽量将列定义为not null，这样可使数据的出来更快，所需的空间更少，而且在查询时，MySQL不需要检查是否存在特例，即null值，从而优化查询；

　　如果一列只含有有限数目的特定值，如性别，是否有效或者入学年份等，在这种情况下应该考虑将其转换为enum列的值，MySQL处理的更快，因为所有的enum值在系统内都是以标识数值来表示的；

　　

　　19、使用optimize table

　 　对于经常修改的表，容易产生碎片，使在查询数据库时必须读取更多的磁盘块，降低查询性能。具有可变长的表都存在磁盘碎片问题，这个问题对blob数据类 型更为突出，因为其尺寸变化非常大。可以通过使用optimize table来整理碎片，保证数据库性能不下降，优化那些受碎片影响的数据表。 optimize table可以用于MyISAM和BDB类型的数据表。实际上任何碎片整理方法都是用mysqldump来转存数据表，然后使用转存后的文件并重新建数据 表；

　　

　　20、使用procedure analyse()

　　可以使用procedure analyse()显示最佳类型的建议，使用很简单，在select语句后面加上procedure analyse()就可以了；例如：

　　select * from students procedure analyse();

　　select * from students procedure analyse(16,256);

　　第二条语句要求procedure analyse()不要建议含有多于16个值，或者含有多于256字节的enum类型，如果没有限制，输出可能会很长；

　　

　　21、使用查询缓存

　　1）查询缓存的工作方式：

　　第一次执行某条select语句时，服务器记住该查询的文本内容和查询结果，存储在缓存中，下次碰到这个语句时，直接从缓存中返回结果；当更新数据表后，该数据表的任何缓存查询都变成无效的，并且会被丢弃。

　　2）配置缓存参数：

　 　变量：query_cache _type，查询缓存的操作模式。有3中模式，0：不缓存；1：缓存查询，除非与 select sql_no_cache开头；2：根据需要只缓存那些以select sql_cache开头的查询； query_cache_size：设置查询缓存的最大结果集的大小，比这个值大的不会被缓存。

　　

　　22、调整硬件

　　1）在机器上装更多的内存；

　　2）增加更快的硬盘以减少I/O等待时间；

　　寻道时间是决定性能的主要因素，逐字地移动磁头是最慢的，一旦磁头定位，从磁道读则很快；

　　3）在不同的物理硬盘设备上重新分配磁盘活动；

　　如果可能，应将最繁忙的数据库存放在不同的物理设备上，这跟使用同一物理设备的不同分区是不同的，因为它们将争用相同的物理资源（磁头）。

相信自己，总有一天你会是成功的。