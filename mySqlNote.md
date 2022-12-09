---
title: MySQL笔记
date: 2022-09-11 16:34:31
tags: MySQL, sql, mysql
toc: true
comment: true	
---

## MySQL笔记

---

### MySQL数据库的安装与配置
*   window安装，到官网下载社区版安装包执行安装后，会有服务端和客户端（C/S）
*   Ubuntu安装
	-   安装mysql服务端`sudo apt install mysql-server`，通过apt安装的mysql，服务已经默认开启，服务名为mysql不是mysqld。
	```shell
	# mysql服务的重启，及其它操作，status参数为查看服务状态
	service mysql [restart | start | stop | stutus] [ systemctl [restart | start | stop | status] mysql]
	# 查看默认账户密码
	sudo cat /etc/mysql/debian.cnf
	```
	-   进入linux的root用户，登录mysql的root用户，可以免密码登录mysql，之后新建mysql普通用户，并把同等root权限赋予它。获取修改mysql的root用户密码和root用户登录的方式限制，使用密码登录mysql的root用户进行操作
	```sql
	# 本地shell客户端使用密码登录mysql-cli，这里选择的密码加密方式插件和root用户的登录ip限制，
	alter user 'root'@'localhost' identified with mysql_native_password by '123456';
	flush privileges;
	# 远程客户端根据虚拟机（服务器）ip地址登录mysql-cli，再在mysql的配置文件(/etc/mysql/mysql.conf.d/mysqld.cnf)中开启root用户远程登录
	# 查看root@%用户是否存在，不在则创建
	select user, host from mysql.user where user  = 'root';
	create user 'root'@'%' identified with mysql_native_password by '123456';
	grant all privileges on *.* to 'root'@'%' with grant option; 
	flush privileges;
	```
	-   
*   Centos7安装
	-   用yum安装mysql
		+   因为yum仓库没有提供mysql所以需要到mysql官网下载提供的yum仓库地址（.rpm包文件）[MySQLrmp包下载](https://dev.mysql.com/downloads/repo/yum/)，选择对应的centos版本下载
		+   使用wget下载[rpm包](https://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm)到虚拟机（服务器）本地
		+   使用rpm命令安装rpm包
		```shell
		# -U 表示升级软件包， -v 表示显示详细消息， -h 表示显示安装进度
		rpm -Uvh mysql80-community-release-el7-7.noarch.rpm
		# 切换mysql版本
		yum repolist all | grep 'mysql'
		yum -y install yum-utils
		yum-config-manager --disable mysql80-community
		yum-config-manager --enable mysql57-community
		# 安装mysql，服务端mysqld，客户端mysql
		yum -y install mysql-community-server

		# 查看版本
		mysql -V
		mysqld -V

		# MySQL服务启停
		systecmtl [restart|start|stop|status] mysqld
		service mysqld [restart|start|stop|status]
		
		# 查看监听端口
		ss -tnlp | grep 'mysql'
		netstat -atnlp | grep 'mysql' 
		lsof -i:3306
		```
		+   mysqld初次启动时会初始化数据库，为root用户自动设置一个随机的复杂密码，保存在错误日志中。
		```shell
		# 查看mysql用户root用户密码
		grep 'temporary password' /var/log/mysqld.log
		2022-11-07T10:29:32.687728Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: fQXqGrI+h2vv
		```
		+   用初始密码登录mysql重新设置mysql密码
		```
		mysql -u root -pfQXqGrI+h2vv
		# 登录成功之后，操作数据库提示需要修改密码才能操作
		# 新密码验证要求为大于8位有大小写字母、数字和特殊符号组成
		alter user 'root'@'localhost' identified with mysql_native_password by 'AWD123+@q1';

		# 因为是本地测试，这里为root用户设置简单密码，线上和测试环境不能做如下操作
		set global validate_password.policy = 0;
		set global validate_password.length = 4;
		alter user 'root'@'localhost' identified with mysql_native_password by 'qwe123';
		```
		+   为了保证安全，连接数据库都是通过ssh方式（堡垒机）再由ssh通过unix socket再登录上mysql服务器
		+   本地测试，为了使用宿主机的mysql客户端连接虚拟机的mysql服务端，防火墙需要开放数据库服务器端口（默认3306），需要登录的账户必须要设置为`user@%`
	-   编译安装mysql
		+   下载MySQL的源码，然后使用编译工具（cmake、gcc-c++）将源码编译成二进制文件，然后再进行安装。优势在于灵活性更强，用户可以修改MySQL的源码后重新编译，或者在编译时指定详细的参数。
		+   下载MySQL的[源码](https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-boost-8.0.31.tar.gz)
		+   解压源码压缩包
		```shell
		tar -zxvf mysql-boost-8.0.31.tar.gz
		```
		+   安装编译工具
		```shell
		yum -y install gcc-c++ cmake ncurses-devel
		```
		+   
*   mysql的文件目录
	-   客户端程序`/usr/bin/mysql`
	-   服务端程序`/usr/sbin/mysqld`
	-   配置文件目录，总配置文件为`/etc/mysql/my.cnf` 包含服务端和客户端配置，总配置文件目录为`/etc/mysql/mysql.conf.d/`，服务端配置文件`/etc/mysql/mysql.conf.d/mysqld.cnf`，客户端配置文件`/etc/mysql/mysql.conf.d/mysql.cnf`
	-   数据文件目录，默认为`/var/lib/mysql`，可以自定义修改
	-   错误日志文件目录，默认为`/var/log/mysql/`，可以在配置文件中自定义修改
	-   字符集、语言等，默认为`/usr/share/mysql`

### MySQL的配置文件
*   MySQL的配置文件时my.cnf（或者my.ini），可以利用它进行数据库调优、性能优化、主从复制等配置。
*   配置区段，在MySQL的配置文件中分为多个区段（section）。其中`[mysqld]`区段指的时服务端的配置，`[client]`，指向的是客户端的配置
```cnf
# 配置客户端和服务端的sock文件，使其能进行socket连接
[client]
socket=/tmp/mysql.sock
[mysqld]
socket=/tmp/mysql.sock
```
*   服务端的基本配置（可以在配置文件my.cnf中修改，也可使用set对全局变量修改）
	-   常用的基本配置有：SQL模式、默认字符集、默认校对集、默认存储引擎等
	-   sql_mode：服务器SQL模式，用于定义MySQL支持的SQL语法以及执行哪种数值校验检查。一般使用默认即可
	-   character_set_server：服务器默认校对集，默认为utf8mb4
	-   collation_server：服务器的默认校对集，默认为utf8mb4_general_ci
	-   explicit_defaults_for_timestamp：服务器对TIMESTAMP列的默认值和NULL值的处理方式，默认为0。
	-   max_connentions：允许客户端的最大同时连接数，默认为151，如果连接数过多会消耗大量系统资源。如果连接数已满，则会报Too many connetions错误
	-   open_files_limit：操作系统允许MySQL打开的文件数，默认为5000
	-   default_storage_engine：默认的存储引擎，默认为InnoDB
*   内存和优化配置，通过分配较大的内存空间有利于提高性能，但是不能过高，以免造成系统频繁换页，降低系统性能。
	-   key_buffer_size：索引缓冲区大小，由所有的线程共享。增加大小可以获得更好处理的所有（对于读取和多次写入），默认值为8M
	-   table_open_cache：所有线程的打开表的缓存数量，用于更快地访问表内容，默认为2000。设置过大会占用过多的文件描述符
	-   sort_buffer_size：为每个需要进行排序的会话分配的缓冲区大小，默认为256K。增加该值可以提高ORDER BY 和 GROUP BY速度
	-   read_buffer_size：对MyISAM表执行顺序扫描的每个线程分配的缓冲区，默认为128K，如果对表的顺序扫描频繁，增加此值可提高速度
	-   thread_cache_size：服务器应缓存多少线程以供复用，默认为-1（自动调整大小）当客户端断开连接时，如果有客户端线程少于指定值，则将其放入缓存
	-   query_buffer_size：查询缓冲区的大小，默认为1M，需要和query_cache_type配合使用，当query_cache_type值为0时不使用查询缓冲区，为1时使用。使用查询缓冲区可以提高查询速度
	-   tmp_table_size：内部临时表的最大值，默认为16M。如果内存中的临时表超出限制，MySQL会自动将其转换为磁盘上的临时表
	-   innodb_buffer_pool_size：InnoDB缓存表和索引数据的内存区域，默认为128M。对于多次访问相同的表数据的情况，配置较大的缓冲池可以减少磁盘I/O，提高查询速度
	-   innodb_log_file_size：InnoDB重做日志的日志组中每个日志文件的大小，默认为48M。该值越大，缓冲池中必要的检查点刷新活动就会越少，节省磁盘I/O。但是日志文件越大，MySQL崩溃恢复的速度就越慢
	-   innodb_lock_wait_timeout：InnoDB的锁等待超时时间，默认为50秒。发生锁定等待超时时，将回滚当前语句（而不是整个事务）。
*   日志配置
	-   MySQL中的日志文件分为：错误日志、常规日志、二进制日志、慢查询日志
	-   error_log：错误日志的文件路径，保存MySQL启动、运行、停止时的日志信息
	-   general-log：是否开启常规日志，用于记录客户端连接和执行的sql语句
	-   general-log_file：常规日志的文件路径
	-   log_slow_queries：慢查询日志文件路径，用于记录超过long_query_time时间或者没有使用索引的查询
	-   long_query_time：当查询时间超过该值时将会记录到慢查询日志中
	-   log_queries_not_using_indexes：是否在慢查询日志中记录未使用索引的查询
	-   log-bin：二进制日志保存路径，主要用于复制环境和数据恢复
	-   max_binlog_size：二进制日志单个文件的大小限制
	-   expire_logs_days：自动清除超过指定天数的过期日志
	```
	[mysqld]
	genetal-log = on
	general-log_file = /var/log/mysql/general.log
	```
	-   
*   

### 数据库
*   数据库三大特性
	1.   实体（数据表）
	2.   属性（数据表中的字段）
	3.   关系（数据表与数据表之间的关系）
*   数据库系统维护事务以下特性（ACID）
	1.   原子性（Atomicity），一个事务的所有操作，要么全部完成，要么全部不完成，不会结束在中间的某个环节。事务在执行过程中发生错误，会被回滚到事务开始的状态，就行这个事务从来没执行过一样。
	2.   一致性（Consistency），一个事务执行之前和执行之后数据库都必须处于一致性状态。如果事务成功的完成，那么系统中所有的变化都将正确的应用，系统处于有效状态。如果在事务中出现错误，那么系统中的所有变化都将自动的回滚，系统返回原始状态
	3.   隔离性（Isolation），指并发环境中，当不同的事务操纵相同的数据时，每个事务都有各自的完整数据空间。由并发事务所做的修改必须与任何其他并发事务所做的修改隔离。事务查看数据更新时，数据所处的状态要么是另一个事务修改它之前的状态，要么就是另一个事务修改它之后的状态，事务不会查看到中间状态的数据。
	4.   持久性（Durability），指只要事务成功结束，它对数据库所做的更新就必须永久的保存下来。即使发生系统崩溃，重新启动数据库系统后，数据库还能恢复到事务成功结束时的状态。
*   数据库设计三大范式
	1.   原子性
	2.   主键相关
	3.   主键直接相关
*   数据库五大约束

### 常用查看数据库状态
*   查看当前会话信息和数据库信息。`status;`
*   查看数据表详细信息。`show table status from db_name like 'test_%table'\G`  
*   查看数据表的建表语句。`show create table tb_name\G`
*   查看数据表结构。`desc|describe tb_name;`，查看指定字段`desc tb_name col;`也可以用`show (full) columns from (tb_name from db_name)|db_name.tb_name;`
*   以一行为单位格式化打印，在	sql语句末尾加`\G` 

### 对数据表的操作(DDL)
*   新建数据表
```sql
create table `tb_name` (
	`first_col` int(11) not null auto_increment comment 'first_col',
	`second_col` varchar(255) not null default '' comment 'second_coll'
	) engine=MyISAM default charset=utf8mb4 row_format=compact;
```
*   删除数据表
```sql
drop table `tb_name`;
drop table if exists `tb_name`;
create table `tb_name` (....)...;
```
*   复制表结构作为新表（复制表数据见数据操作）
```sql
create table `tb_name_cp` like `tb_name`;
create table `another_db`.`tb_name_cp` like `current_db`.`tb_name`;
```
*   
### 对表结构的操作(DDL)
*   修改表名。`rename table old_tbname to new_tbname; /  alter table old_tbname rename to new_tbname;` 
*   修改字段名。`alter table tb_name change old_col new_col varchar(255)';` 
*   修改字段类型。`alter table tb_name modify old_col char(255);`
*   修改字段位置。`alter table tb_name modify old_col varchar(255) after first_col;` 
```sql
alter table `tb_name`
modify old_col varchar(50) character set utf8,
modify second_col varchar(255) character set utf8;
```
*   新增字段。`alter table tb_name add new_col int after second_col;`
```sql
alter table tb_name add col1 varchar(123);
alter table tb_name add (col1 varchar(23), col2 varchar(45));
alter table tb_name
add column col1 varchar(23),
add column col2 varchar(45);
```
*   删除字段。`alter table tb_name drop old_col;`
*   修改表字段默认约束。就是重新声明表字段类型。所以修改和删除默认约束都是重新声明字段属性。`alter table tb_name modify col int unsigned default 1;`
*   修改表字段非空约束。和修改默认约束一样
*   添加和删除唯一索引。需要按照索引的方式来操作。
```sql
create table my_unique_3 (id int);
alter table my_unique_3 modify id int unique;
show create table my_unique_3;
alter table my_unique_3 drop index id;
show create table my_unique_3;
```
*   添加和删除主键约束。
```sql
create table my_primary (id  int unsigned, primary key (id));
alter table my_primary drop primary key;
alter table my_primary modify id int unsigned;
alter table my_primary add primary key (id);
```
*   修改、删除和添加表的自动增长
```sql
alter table my_auto auto_increment = 10;
alter table my_auto modify id int unsigned;
alter table my_auto modify id int unsigned auto_increment;
```
*   

### 对数据表的记录进行操作(DML)
*   插入数据（新增记录）
	1.   insert into goods values (2, 'notebook', 4998, 'High cost performance');
	2.   insert into goods(id, name) values (3, 'Mobile phone');
	3.   insert into goods set id = 3, name = 'Mobile phone';
	4.   insert into goods values (2, 'notebook', 4998, 'High cost performance'), (3, 'Mobile phone', NULL, NULL), (4, 'thinkpad', 7999, 'high performance');
*   复制表数据插入到指定表
```sql
insert into `another_db`.`tb_name_cp` select * from `current_db`.`tb_name`;
# 主键冲突，可以指定字段数据插入数据，指定的字段需要和查询的字段相同
insert into `another_db`.`tb_name_cp` (`col1`, `col2`, ..., `coln`) select `col1`, `col2`, ..., `coln` from `current_db`.`tb_name`; 
```
*   插入时主键冲突
	1.   主键冲突更新
	```sql
	insert into `db`.`tb` (`pri_key`, `col1`, `col2`, ..., `coln`) values 
		('pri_key_val', 'val1', 'val2', ..., 'valn') 
		on duplicate key 
		update `col1` = 'val1', `col2` = 'val2', ..., `coln` = 'valn' ; 
	```
	2.   主键冲突替换
	```sql
	replace into `db`.`tb` (`pri_key`, `col1`, `col2`, ..., `coln`) values
		('pri_key_val', 'val1', 'val2', ..., 'valn'); 
	```
*   修改数据(加where子句限定)
	```sql
	update goods set description = 'Higher performance' where name = 'thinkpad';

	update `tb_name` set `col1` = 'val1', `col2` = 'val2', ..., `coln` =  'valn' [where ....] order by `col1` desc limit 3;
	```
*   删除数据(加where子句限定)
	```sql
	delete from goods where id = 4;

	#删除表的所有数据，不影响默认增长值
	delete from `tb_name`; 
	delete from `tb_name ` [where ...] order by `col1` desc limit  3;
	```
*   清空数据(全表数据删除的时候使用，会从默认的初始值开始增长/DDL)
	*   truncate `tb_name`;
	```sql
	DROP TABLE IF EXISTS `subscribe`;
	CREATE TABLE `subscribe`(
		`id` INT(10) NOT NULL AUTO_INCREMENT COMMENT '编号id，自增',
		`email` VARCHAR(60) NOT NULL DEFAULT '' COMMENT '用户订阅邮箱',
		`status` TINYINT(2) NOT NULL DEFAULT 0 COMMENT '用户是否确认订阅：0-未确认，1-已确认',
		`code` VARCHAR(10) NOT NULL DEFAULT '' COMMENT '邮箱确认验证码',
		PRIMARY KEY(`id`)
	) ENGINE=MyISAM DEFAULT CHARSET=utf8;

	LOCK TABLES `subscribe` WRITE;
	INSERT INTO `subscribe`(`id`, `email`, `status`, `code`) VALUES
	(1, 'tom123@163.com', 1, 'TRBXPO'),
	(2, 'lucy123@163.com', 1, 'LOICPE'),
	(3, 'lily123@163.com', 0, 'JIXDAMI'),
	(4, 'jimmy123@163.com', 0, 'QKOLPH'),
	(5, 'joy123@163.com', 1, 'JSMWNL');
	UNLOCK TABLES;

	DROP TABLE IF EXISTS `topic`;
	CREATE TABLE `topic` (
		`id` INT COMMENT '专题编号',
		`title` VARCHAR(255) COMMENT '专题名称',
		`intro` VARCHAR(255) COMMENT '专题介绍',
		`start_time` INT COMMENT '专题开始时间',
		`end_time` INT COMMENT '专题结束时间'
	) ENGINE=MyISAM DEFAULT CHARSET=utf8;

	ALTER TABLE `mydb`.`goods` ADD (`total` INT, `add_time` INT) AFTER `description`;
	```

### 对数据表的记录进行查询(DML/DQL)
*   SQL语句的结构和执行顺序
*   基本查询
*   带条件查询
	-    where子句，是从数据表中获取数据，再将数据从磁盘存储到内存中
	-    where之后的所有语句都是对内存中的数据进行操作
	-    having子句，是对已存放到内存中的数据进行操作，和group by子句一起使用
	-    having根据条件表达式对分组后的内容进行过滤
	-    where位于group by前，having 位于group by 后
	-    having子句可以用聚合函数，而where不可以
*   去除重复记录
	-   distinct关键字。 `select distinct col1 from tb_name;`
*   对查询记录进行排序
	-   order by。`select col1, col2, ..., coln from tb_name order by col1 [asc|desc], col2 [asc|desc];`
*   对查询记录进行限量(也可以作为条件语句的补充)
	-   limit。`select col1, col2, ..., coln from tb_name limit begin_num, record_num;`
	-   order by 和 limit也可作为条件的替代，因为它是对获取的结果进行操作集，可以与update和delete子句组合使用（见数据表记录操作），但是执行语句的顺序是怎样的？（select、update、delete）
*   LIKE查询(模糊查找)
	-   通配符%, 匹配零个到多个字符
	-   通配符_, 只匹配一个字符
	-   正则表达式
*   正则表达式查询
*   带索引查询
	-   如果表中的字段有创建了索引，并且select语句中用使用到创建了索引的字段，则数据库默认使用索引进行查询，否则使用全表扫描
*   分组查询
	-   group by 子句，用于指定字段进行分组。5.7严格模式下，只能select 到 group by子句中的字段，对于其他字段，可以使用聚集函数进行操作，也可用group_concat(col)，函数把指定的字段用逗号拼接为一个新的字符串作为结构显示。
	-   8.0版本弃用了`group by col [asc|desc]`，隐式排序方式，需要对结果集进行排序，可以用order by 子句对指定列进行排序
	-   多分组统计。`group by col1, col2`
	```sql
	# 对sh_goods商品表，按score评分字段进行分组再降序排序，再按comment_count评论数进行分组后升序排序
	select score, count(*), group_concat(name), comment_count  from sh_goods group by score, comment_count order by score desc, comment_count asc;
	```
	-   回溯统计，根据指定字段分组后，系统又自动对分组的字段向上进行了一次新的统计并产生一个新的统计数据，且该数据对应的分组字段为null值。
	```sql
	select category_id, count(*) from sh_goods group by category_id with rollup;
	select score, comment_count, count(*) from sh_goods group by score, comment_count with rollup;
	```
	-   
*   聚合函数，多与group by子句一起使用
	-   count()，统计指定字段中的记录数量，不计算字段为NULL的记录
	-   sum()，返回指定字段值的总和
	-   min()，返回指定字段值最小的值
	-   max()，返回指定字段值最大的值
	-   avg()，返回指定字段值的平均值
	-   group_concat()，拼接指定字段值的作为字符串返回
	-   json_arrayagg()，将指定字段值作为单个json数据返回
	-   json_objectagg()，将指定字段值作为单个json对象返回
*   多表查询
	-   联合查询（union），用于联合多个select结果集（所有结果集的字段数必须相同，且只显示第一个结果集的字段），可以与left join和right join组合实现外全连接结果。需要排序的时候需要在每个select子句中实现（order by col limit n）左右加上括号
	```
	select ...
	union [all|distinct] select ...
	[union [all|distinct] select ...];
	select id, name, price from sh_goods where category_id = 9 
	union 
	select id, name, keyword, price from sh_goods where category_id = 6;

	select * from sh_goods g left join sh_goods_comment c on g.id = c.goods_id 
	union 
	select * from sh_goods right join sh_goods_comment c on g.id = c.goods_id; 

	select * from sh_goods g left join sh_goods_comment c on g.id = c.goods_id 
	union all 
	select * from sh_goods g right join sh_goods_comment c on g.id = c.goods_id where c.goods_id is null;
	```
	-   交叉连接（cross join，两个表的笛卡尔乘积）
	-   内连接（\[inner\] join ... on ...）使用on设置条件
	```sql
	select distinct g1.id, g1.name, g1.category_id from sh_goods g1 inner join sh_goods g2 on g1.category_id = g2.category_id where g2.name = '钢笔';
	select distinct g1.id, g1.name, g1.category_id from sh_goods g1 inner join sh_goods g2 on g1.category_id = g2.category_id and g2.name = '钢笔';
	```
	-   外连接
		1.   左外连接（left \[outer\] join ... on ...） 
		2.   右外连接（right \[outer\] join ... on ...）
		3.   左/右外连接能以左/右的表位基准用on提供的条件来补全数据，没有符合条件的会以null值填充，内连接只会返回两表中，符合条件的，其他行均不显示
		3.   全外连接（用左右外连接和union实现），左右表不符合条件的均以null值填充
	-   USING关键字，当数据表连接的字段同名，连接时的匹配条件可以使用USING替代ON
	```sql
	# 查询商品表中与钢笔同分类下有哪些商品
	select distinct g1.id, g1.name, g1.category_id from sh_goods g1 join sh_goods g2 on g1.category_id = g2.category_id on g2.name = '钢笔';
	select distinct g1.id, g1.name, g1,category_id from sh_goods g1 join sh_goods g2 using (category_id) where g2.name = '钢笔';
	```
	-   子查询
		+   按功能分
			1.   标量子查询，子查询返回结果是一个数据（一行一列），多与比较运算符（=、<>等）一起用
			2.   列子查询，子查询返回结果是一个字段中符合条件的所有数据（一列多行），多与（in、not in）一起用
			3.   行子查询，子查询返回结果是一条包含多个字段的记录（一行多列），
			```sql
			select * from tb_1 where (col1, col2, ..., coln) = (select col1, col2, ..., coln from tb_2 [where] [group by] [having] [order by] [limit]);
			select id, name, price, score, content from sh_goods where (price, score) = (select max(price), min(score) from sh_goods);
			```
			4.   表子查询，子查询返回结果是一个符合二维表结构的数据（一行一列、一列多行、一行多列、多行多列），多用于from数据源
			```sql
			select * from (select * from tb) as aria [where] [group by] [having] [order by ] [limit]
			select g1.id, g1.name, g1.price, g1.category_id from sh_goods g1, (select max(price) max_price, category_id from sh_goods group by category_id) as g2 where g1.price = g2.max_price;
			```
		+   按位置分
			1.   where子查询（标量子查询、列子查询、行子查询）
			2.   from子查询（表子查询）
		+   子查询关键字
			1.   带exists关键字的子查询，返回结果之后1和0，子查询返回为空时返回0，否则为1
			2.   带any关键字的子查询，只要符合any()子查询结果中的任意一个则返回1，否则为0
			3.   带all关键字的子查询，只有所有符合all()子查询结果时才返回1，否则返回0
*   复杂查询

### 数据库基本数据类型
*   数字类型：常用来储存商品的库存，销量和价格等经常要用上数学运算的字段，表主键
	-   整数类型，计算机基本是用二进制存储的，所以用字节（1个字节/Byte=8个比特/bit）来存在数据，最大存储范围为字节数的十进制取值-1（2^8-1），无符号的在数据类型右边加上UNSIGNED关键字来修饰，分符号（正+/负-）来存储，有符号的符号占一位（2^7-1）。宽度（在字段类型后面用小括号中的数字），默认位该类型的最大取值范围所能表示的最大宽度
		1.   TINYINT，1Byte
		2.   SMALLINT，2Byte
		3.   MEDIUMINT，3Byte
		4.   INT，4Byte
		5.   BIGINT，4Byte
	-   浮点数类型，用浮点数或定点数来表示存储的小数，也分符号存储，浮点数虽然取值范围很大但是精度不高，也可以设置位数和精度
		1.   浮点数类型
			1.   FLOAT（单精度浮点数），4Byte
			2.   DOUBLE（双精度浮点数），8Byte
		2.   定点数类型（DECIMAL）， 通过`DECIMAL(M, D)` 来设置位数和精度，M表示数字的总位数（不包括‘.’和‘-’），最大值位65，默认值为10；D表示小数点后的位数，最大值为30，默认值为0。推荐使用定点数设置合适的范围。
	-   BIT（位）类型
		1.   BIT，用来存储二进制数据，`BIT(M)`  M表示位数，范围1~64。存储的时候会转换为二进制，
	-   无符号的类型进行数学运算之后也为无符号，如用减号与无符号的字段进行运算结果为有符号的数时，会报错，这是可以先把字段的值强制转换为有符号的在进行运算。
	```
	# select id - 1 from sh_goods limit 5;
	# select id - 3 from sh_goods limit 5;
	select cast(id as signed) - 3 from sh_goods limit 5;
	```
	-    带精度的运算，加减运算取所有操作数中最大精度的，乘法运算中取所有操作数的精度相加，除法运算中运算结果为浮点数，精度为被除数的精度加上系统变量（div_precision_increment）设置的除法精度增长值。（如果除数为0，则返回NULL）
	-    NULL参与的运算结果都为NULL
	-    div与/，div和/都可以用来实现除法运算，div会去点小数部分，只返回结果的整数部分。/，则返回整个计算结果
	-    mod与%，都用来取模运算（取余），取模运算的结果正负符号只与被模数（div或%左边的数）符号相关
*   字符串类型：常用来显示的数据，如用户的身份证、电话号码
	1.   CHAR（固定长度字符串），`CHAR(M)` 所占存储空间都为M字节
	2.   VARCHAR（可变长度字符串）`CHAR(M)` 所占存储空姐都为M+1字节
	3.   TEXT（大文本数据）
		1.   TINYTEXT
		2.   MEDIUMTEXT
		3.   TEXT
		4.   LONGTEXT
	4.   ENUM（枚举类型），`ENUM(M1, M2, ..., Mn)`，枚举类型的数据只能从枚举类型中取值，并且只能取一个。
	5.   SET（字符串对象），`SET(M1, M2, ..., Mn)`，SET类型和ENUM类型相似，区别在于SET可以选取一个或者多个值，用‘,’来隔开
	6.   BINARY（固定长度的二进制数据），用来存储的是二进制数据。和CHAR、VARCHAR类似
	7.   VARBINARY（可变长度的二进制数据）
	8.   BLOB（二进制大对象/Bianry Large Object），用来存储数据量很大的二进制数据，如图片、PDF文档
		1.   TINYBLOB
		2.   MEDIUMBLOB
		3.   BLOB
		4.   LONGBLOB
	9.   JSON
*   时间和日期类型
	1.   YEAR
	2.   DATE
	3.   TIEM
	4.   DATETIME
	5.   TIMESTAMP，默认会设置字段属性（NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP）
*   直接常量，指在MYSQL中直接编写的字面常量。
	1.   十进制数，如123、1.23、-1.23和科学计数法
	2.   二进制数，在二进制字符串前加前缀b，如b'10000001'
	3.   十六进制数，如0x41，x41
	4.   字符串，支持单引号和双引号定界符，如'abc'，"abc"。可使用转义字符来表示字符串中的单/双引号。
	5.   布尔值，TRUE和FALSE，不区分大小写。通常用于逻辑判断
	6.   NULL值，用来表示没有值、值不确定等含义】

### 数据类型的运算
*   数学运算
	-   无符号的类型进行数学运算之后也为无符号，如用减号与无符号的字段进行运算结果为有符号的数时，会报错，这是可以先把字段的值强制转换为有符号的在进行运算。
	```
	# select id - 1 from sh_goods limit 5;
	# select id - 3 from sh_goods limit 5;
	select cast(id as signed) - 3 from sh_goods limit 5;
	```
	-    带精度的运算，加减运算取所有操作数中最大精度的，乘法运算中取所有操作数的精度相加，除法运算中运算结果为浮点数，精度为被除数的精度加上系统变量（div_precision_increment）设置的除法精度增长值。（如果除数为0，则返回NULL）
	-    NULL参与的运算结果都为NULL
	-    div与/，div和/都可以用来实现除法运算，div会去点小数部分，只返回结果的整数部分。/，则返回整个计算结果
	-    mod与%，都用来取模运算（取余），取模运算的结果正负符号只与被模数（div或%左边的数）符号相关
*   比较运算，条件表达式中对结果的限定，返回的结果有true（1），false（0）和null。
	-   =, 用于相等的比较
	-   <=>, 可以进行与NULL值比较的相等运算符
	-   \>, 用于大于比较
	-   <, 用于小于比较
	-   \>=, 用于大等于比较
	-   <=, 用于小等于比较
	-   <>, != 用于不相等比较
	-   between ... and ... , 比较一个数据是否在指定的闭区间范围内, 若在则返回1, 不在则返回0
	-   not between ... and ... , 比较一个数据是否不在指定的闭区间范围内, 若不在则返回1, 若在则返回0
	-   is, 比较一个数据是否为true, false, unknow, 若是返回1, 否则返回0
	-   is not, 比较一个数据是否不是true, false, unknow, 若不是返回1, 否则返回0
	-   is null, 比较一个数据是否为null, 是返回1, 否则返回0
	-   is not null, 比较一个数据是否不为null, 若不是则返回1, 否则返回0
	-   like 'parrent', 获取匹配到的数据
	-   not like 'parrent', 获取匹配不到的数据
	-   在基础运算符与null比较时, 结果都为null
	-   in(), 比较一个值是否在一组给定的集合内
	-   not in(), 比较一个值是否不在以组给定的集合内
	-   greatest(), 返回最大的一个参数值, 至少要两个参数
	-   least(), 返回最小的一个参数值, 至少要两个参数
	-   isnull(), 测试参数是否为空
	-   coalesce(), 返回第一个非空参数
	-   interval(), 返回小于第一个参数的参数索引
	-   strcmp(), 比较两个字符串
*    逻辑运算符
	-   and/&&, 逻辑与, 操作数全部为真, 则结果为1, 否则为0
	```
	select id, name, price from sh_goods where keyword = '电子产品' and id = 5;
	select id, name, price from sh_goods where keyword = '电子产品' && id = 5;
	select id, name, price from sh_goods where (keyword, id) = ('电子产品', 5);
	# 在与null的add比较中, 如果有1, 则返回null, 有0则返回0
	select 1&&null, null&&1, 0&&null, null&&0;
	```
	-   or/||, 逻辑或, 操作数只要有一个为真, 则结果为1, 否则为0
	```
	# 在与null的or比较中, 如果有1, 则返回1, 有0则返回null
	select 1||null, null||1, 0||null, null||0;	
	```
	-   not/!, 逻辑非, 操作数为0, 则结果为1, 操作数为1, 则结果为0, ! 运算符优先度比 not 高
	-   xor, 逻辑异或, 操作数一个为真, 一个为假, 则结果为1, 若操作数全为真或全为假, 则结果为0
*   赋值运算符
	-   :=, 在insert ... set ... 和update ... set ...语句中=, 认为是赋值运算, 其他情况中若需要赋值, 应使用:= 赋值
*   位运算符(TBD)
*   运算符优先级(TBD)

### 数据表的约束
*   为了防止数据表中插入错误的数据，MYSQL定义了一些维护数据库完整性的规则（表的约束）。
	-   默认约束（DEFAULT），用于为数据表中的字段指定默认值，TEXT, BLOB数据类型不支持设置默认值
	-   非空约束（NOT NULL），指定字段的值不能为空
	-   唯一约束（UNIQUE），用于保证字段的值的唯一性。允许出现多个NULL值。
		1.    列级约束，在声明字段属性中加入UNIQUE关键字。
		2.    表级约束，在表声明最后一个字段下一行，使用多行UNIQUE(col1), UNIQUE(col2)指定表中多个字段的唯一约束
		3.    UNIQUE KEY (col1)，声明字段的唯一约束时创建索引
		4.    创建复合唯一约束，在表级唯一性约束创建时，添加多个字段，组成复合唯一索引 `UNIQUE(col1, col2, ..., coln)` 特点为只有多个字段的值相同时才视为重复记录
	-   主键约束（PRIMARY KEY），在MYSQL中为了快速查找表中的某条记录，可以通过设置主键来实现。主键可以唯一标识表中的记录。主键约束通过`PRIMARY KEY()` 定义，它相当于唯一约束和非空约束的组合，要求被约束的字段不允许重复也不允许出现NULL值，每个表最多只允许含有一个主键
		1.   列级主键约束，指定字段为当前表的主键`...(col INT PRIMARY KEY,....)`
		2.   表级主键约束，若指定的字段只有一个，则和单字段主键一样，如果有多个则为符合主键。符合主键需要用多个字段来确定记录的唯一性，类似复合唯一约束
		```sql
		CREATE TABLE my_primary (
			id INT UNSIGNED,
			username VARCHAR(10),
			PRIMARY KEY(id, username)
		);
		```
		3.   自动增长（AUTO_INCREMENT），在表中设置主键约束后，每次插入记录都要检查主键的值，避免因重复值插入失败。可以使用MYSQL提供的自动增长来自动生成主键的值。一个表中只能有一个能自动增长的字段，该字段为整数类型，且必须定义为键。若为自动增长字段插入NULL、0、DEFAULT或者在插入时省略该字段，则会使用自动增长值。插入的时一个具体值则不会。自动增长从1开始自增，每次加1。使用DELETE删除记录时，自动增长值不会减少或者空缺。
	-   外键约束（FOREIGN KEY）（实际需要多表操作）保证不同表中相同含义的数据的一致性和完整性
		+   指一个表中引用另外一个表中的一列或者多列，被引用的列应该具有主键约束或唯一性约束，从而保证数据的唯一性和完整性，被引用的表为主表，引用外键的表为从表
		+   创建外键约束
		```sql
		# 在创建数据表(create table)或者修改数据结构(alter table)时添加外键约束，在相应的位置添加一下语句即可。
		[constraint symbot] foreign key [index name] (index_col name, ...)
		references tb_name (index_col name, ...)
		[on delete (restrict | cascade | set null | no action | set default)]
		[on update (restrict | cascade | set null | no action | set default)]

		create table if not exists `mydb`.`department`(
			`id` int unsigned primary key auto_increment comment '部门编号', 
			`name` varchar(50) not null default '' comment '部门名称'
		) engine=InnoDB default charset=utf8mb4 collate=utf8mb4_general_ci comment '部门表';

		create table if not exists `mydb`.`employees`(
			`id` int unsigned primary key auto_increment comment '员工编号',
			`name` varchar(120) not null default '' comment '员工姓名',
			`dept_id` int unsigned not null default 0 comment '部门编号，外键',
			constraint FK_ID foreign key `dept_id` (`dept_id`) references `department` (`id`)
			on delete restrict on update cascade
		) engine=InnoDB default charset=utf8mb4 collate=utf8mb4_general_ci comment '员工表';

		alter table `mydb`.`employees` 
		add constraint FK_ID foreign key `dept_id` (`dept_id`) references `department` (`id`)
		on delete restrict on update cascade;	
		```
		+   关联表操作，当设置了外键约束之后，关联表的操作（数据的插入、更新和删除）会受到限制
		+   使用外键约束的好处
			1.   外键开发可以节省开发量
			2.   外键能约束数据的有效性，防止非法数据的插入
		+   使用外键约束的缺点
			1.   带来额外的开销
			2.   主表被锁定时，从表也会被锁定
			3.   删除主表的数据时要先删除从表的数据
			4.   含有外键约束的从表字段不能修改表结构
		+   删除外键约束
		```sql
		alter table sub_tb drop foreign key foreign_key_name;
		alter table employees drop foreign key FK_ID;
		```
		+   

### 数据库的字符集和校对集
*   字符集（CHARACTER/CHARSET）
*   校对集（COLLATE）
*   设置字符集和校对集
	1.   从MYSQL配置
	2.   从数据库设置
	3.   从数据表设置
	4.   从字段设置

```sql
CREATE TABLE IF NOT EXISTS `my_user` (
	`id` INT UNSIGNED PRIMARY KEY AUTO_INCREMENT COMMENT '用户id',
	`username` VARCHAR(20) NOT NULL UNIQUE COMMENT '用户名',
	`mobile` CHAR(11) NOT NULL COMMENT '手机号码',
	`gender` ENUM('male', 'female', 'privated') NOT NULL COMMENT '性别',
	`reg_time` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间',
	`level` TINYINT UNSIGNED NOT NULL COMMENT '会员等级'
) ENGINE=MyISAM DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
LOCK TABLES `my_user` WRITE;
INSERT INTO `my_user` ( `username`, `mobile`, `gender`, `reg_time`, `level`) VALUES 
('小明', '12311112222', 'male', '2018-01-01 11:11:11', 1);
UNLOCK TABLES;

CREATE TABLE IF NOT EXISTS `my_student` (
	`stu_id` INT UNSIGNED PRIMARY KEY AUTO_INCREMENT COMMENT '主键id',
	`stu_num` CHAR(10) NOT NULL UNIQUE COMMENT '学号',
	`stu_name` VARCHAR(10) NOT NULL COMMENT '姓名',
	`stu_gender` ENUM('male', 'female') NOT NULL COMMENT '性别',
	`stu_birthday` DATE NOT NULL COMMENT '出生日期',
	`stu_reg_time` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '入学时间',
	`stu_address` VARCHAR(50) NOT NULL COMMENT '家庭住址' 
) ENGINE=MyISAM DEFAULT CHATSET=utf8mb4 COLLATE=utf8mb4_general_ci;
CREATE TABLE IF NOT EXISTS `my_comment` (
	`com_id` INT UNSIGNED PRIMARY KEY AUTO_INCREMENT COMMENT '留言主键id', 
	`com_content` TEXT NOT NULL COMMENT '留言内容',
	`com_create_time` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '留言时间',
	`user_id`INT UNSIGNED COMMENT '游客id',
) ENGINE=MyISAM DEFAULT CHATSET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

### 数据库设计

### 数据库用户操作(DDL+DCL)
*   创建用户，创建用户时如果没有赋予用户权限的话，登录了也不能进行操作
```sql
create user [if not exists] username [option] [with ];
# 指定加密插件为用户设置密码
create user 'test1'@'localhost' identified with mysql_native_password by '123456';
# 修改密码
alter user 'test1'@'localhost' identified with caching_sha2_password by '123456';
```
*   解锁用户
```sql
alter user 'user'@'host' account unlock;
```
*   重命名用户
```sql
rename user 'user'@'host' to 'new_user_name'@'host';
```
*   删除用户
```sql
drop user if exists 'user'@'host';
```
*   权限管理
	-   权限分类，数据权限、结构权限和管理权限
| 分类     | 权限                    | 权限级别                         | 描述                                         |
| -------- | ----------------------- | -------------------------------- | -------------------------------------------- |
| 数据权限 | select                  | 全局、数据库、表、列             | select                                       |
|          | update                  | 全局、数据库、表、列             | update                                       |
|          | delete                  | 全局、数据库、表                 | delete                                       |
|          | insert                  | 全局、数据库、表、列             | insert                                       |
|          | show database           | 全局                             | show database                                |
|          | show view               | 全局、数据库、表                 | show create view                             |
|          | process                 | 全局                             | show process list                            |
| 结构权限 | drop                    | 全局、数据库、表                 | 允许删除数据库、表和视图                     |
|          | create                  | 全局、数据库、表                 | 允许创建数据库、表                           |
|          | create routine          | 全局、数据库                     | 创建存储过程                                 |
|          | create tablespace       | 全局                             | 允许创建、修改、删除表空间和日志文件组       |
|          | create temporary tables | 全局、数据库                     | 允许创建临时表                               |
|          | create view             | 全局、数据库、表                 | 允许创建或修改视图                           |
|          | alter                   | 全局、数据库、表                 | 允许修改表                                   |
|          | alter routine           | 全局、数据库、存储过程           | 允许删除、修改存储过程                       |
|          | index                   | 全局、数据库、表                 | 允许创建、删除索引                           |
|          |                         |                                  |                                              |
|          | trigger                 | 全局、数据库、表                 | 允许触发器的所有操作                         |
|          | reference               | 全局、数据库、表、列             | 允许创建外键                                 |
| 管理权限 | super                   | 全局                             | 允许使用其他管理操作                         |
|          | create user             | 全局                             | 允许对用户的操作                             |
|          | grant option            | 全局、数据库、表、存储过程、代理 | 允许授予、删除用户权限                       |
|          | reload                  | 全局                             | flush操作                                    |
|          | proxy                   |                                  | 与代理的用户权限相同                         |
|          | replication client      | 全局                             | 允许用户访问主服务器或从服务器               |
|          | replication slave       | 全局                             | 允许赋值从服务器读取的主服务器二进制日志事件 |
|          | shutdown                | 全局                             | 允许使用mysqladmin shutdown                  |
|          | lock tables             | 全局、数据库                     | 允许在有select表权限上使用lock tables        |

*   查看权限`show grants for 'user'@'host';`
*   授予权限
```sql
grant all privileges on *.* to 'user'@'host' with grant option
```
*   回收权限
```sql
revoke all privileges on *.*  from 'user'@'host'
```
*   刷新权限（回收权限等操作不会将数据同步到内存中，需要手动刷新权限）

```sql
flush privileges;

create user 'shop'@'127.0.0.%' identified with mysql_native_password by '123456';
alter user 'shop'@'127.0.0.%' password expire;
alter user 'shop'@'%' identified with mysql_native_password by '2c5-q8h';
grant select on `db_shop`.`sh_goods` to 'shop'@'%' with grant option;	
revole select on `db_shop`.`sh_goods` from 'shop'@'%';
flush privileges;
drop user if exists 'shop'@'localhost';
```
*   

### 视图
*   使用视图的有点
	1.   简化查询语句
	2.   安全性
	3.   逻辑数据独立性
*   创建视图
```sql
create [or replace] [algorithm = {undefined | merge | temptable}] [definer = { user | current_user}]
	[sql securty {definer | invoker}]
view view_name [{column_list}]
as select_statement
[with [cascaded | local] check option]
```
*   查看视图
```sql
# 查看用view_前缀定义的视图
show tables like 'view_';
desc view_goods_cate;
# 查看视图状态
show table status like 'view_goods_cate'\G
# 查看定义视图的结构
show create table view_goods_cate\G
```
*   修改视图
```sql
create or replace view view_name as select col1, col2, ..., coln from tb_name;
alter view view_name as select col1, col2, ..., coln from tb_name;
```
*   删除视图
```sql
drop view [if exists] view_name [, ..., view_name_n];
```
*   视图数据操作（实际上就是对基本表的数据操作）
	-   insert into .. 
	-   update ... set
	-   delete from 

```sql
-- sh_goods_attr, sh_goods_attr_value, sh_goods_category
create view sh_view_attr as 
	select ga.sort sort1 ga.id ga_id, ga.name attr, ga.sort sort2, ga1.name sub_name, gav.attr_value attr_val, gc.id gc_id, gc.name gc_name  
	from sh_goods_attr ga join sh_goods_attr ga1 on ga.id = ga1.parent_id 
	join sh_goods_attr_value gav on ga1.id = gav.attr_id 
	join sh_goods_category gc on ga.category_id = gc.id 
	where ga.parent_id != 0 order by ga.sort asc, ga1.sort asc;

create view sh_view_goods_attr as 
	select ga.id ga_id, ga.name ga_name, ga1.name sub_name, gav.attr_value attr_val, g.id g_id, g.name g_name 
	from sh_goods_attr ga join sh_goods_attr ga1 on ga.id = ga1.parent_id 
	join sh_goods_attr_value gav on ga1.id = gav.attr_id 
	join sh_goods g on gav.goods_id = g.id
	where ga.parent_id != 0 order by ga.sort asc, ga1.sort asc;

create table if not exists `mydb`.`student` (
	`id` int unsigned primary key auto_increment comment '学生学号',
	`name` varchar(50) not null default '' comment '学生姓名',
	`math` decimal(10, 2) not null default 0 comment '数学成绩', 
	`chinese` decimal(10, 2) not null default 0 comment '语文成绩', 
	`english` decimal(10, 2) not null default 0 comment '英语成绩', 
) engine=InnoDB default charset=utf8mb4 collate=utf8mb4_general_ci comment '学生成绩表';
create view `mydb`.`view_score` as select math, chinese,  english, sum(math, chinese, english) total from `mydb`.`stutdent`;
```

### 数据库事务
*   事务，
*   事务的四个特性（ACID）
	-   原子性（Atomicity）
	-   一致性（Consistency）
	-   隔离性（Isolation）
	-   持久性（Durability）
*   事务的基本操作
	-   开启一个事务（start transaction;），结束一个事务（commit;/rollback;）
	```sql
	# 用户Alex转账100给用户Bill
	start transaction;
	update user set money = money - 100 where name = 'Alex';
	update user set money = money + 100 where name = 'Bill';
	commit;
	```
	-   MySQL中的事务不允许嵌套，若在start transaction;之后的语句没有commit;/rollback;，就继续start transaction;，那么mysql将会隐式提交上一个事务
	-   事务的操作对象时，数据表中的数据处理（DML、DQL），不包括创建或删除数据库、数据表，修改表结构等操作（DDL、DCL），在数据库执行这些操作时，会隐式提交
	-   mysql5.7的默认表引擎时InnoDB，该表引擎支持事务处理，MyISAM则不支持事务处理。对于MyISAM引擎的表进行数据操作都会立即提交，不能回滚
	-   事务的保存点，在回滚事务时，事务内的所有操作都将撤销。可以使用建立保存点来使事务回滚到某个保存点
	```sql
	start transaction;
	savepoint sp1;
	rollback to savepoint sp1;
	# 在一个事务中，当不需要一个保存点时，可以删除/释放一个保存点
	release savepoint sp1;
	# 在一个事务提交之后，事务中的保存点就会被删除
	commit;
	```	
	-   
*   事务隔离级别
	-   事务隔离级别
		1.   read uncommitted（读取未提交），该事务级别下，能读取还未提交的事务操作后的数据，会造成脏读。该级别等级最低。为避免脏读情况，可以把事务隔离设置为read committed级别或其以上
		2.   read committed（读取提交），该事务级别下，不能读取到未提交的事务操作，但是会造成同一事务中读取数据不一致的问题（数据不可重复读）。为了避免不可重复的情况，可以把事务隔离设置为repeatable read。
		3.   repeatable read（可重复读），是mysql下的默认事务级别
		4.   serializable（可串行化），是事务隔离的最高级别，它在每个读的数据行上加锁，使其不会发生冲突，避免了脏读、不可重复读、幻读的问题，但是由于加锁可能导致超时（timeout）和锁竞争（lock contention）现象，所以是性能最低的一种隔离级。除非为了数据的稳定性，需要强制减少并发的情况时才选择此种隔离级别
	-   查看事务隔离级别
	```sql
	# 查看全局事务隔离级别
	select @@global.transaction_isolation;
	# 查看当前会话事务隔离级别
	select @@session.transaction_isolation;
	# 查看下一个事务隔离级别
	select @@transaction_isolation;
	```
	-   修改事务隔离级别
	```sql
	set [global | session] transaction isolation level [repeatable read | read uncommitted | read committed | serializable]; 
	set session transction isolation level read uncommitted;
	```

	```sql
	# 用户下订单的事务处理, sh_order, sh_order_goods, sh_goods
	start transaction;
	select id, order_id, goods_id, goods_num from sh_order_goods;
	savepoint sp1;
	insert into sh_order_goods (order_id, goods_id, goods_name, goods_num) values
	(1, 4, 1);
	update sh_goods set stock = stock - 1 where id = 4;
	rollback;
	```
	-   

### 自定义函数和存储过程
*   语句结束分隔符（;）修改结束语句分割符
*   创建自定义函数
```sql
delimiter $$
	create function sayHello (name varchar(30)) returns varchar(50)
	begin
		return concat('Hello ', name, '!');
	end
$$
delimiter ;

需要指定函数为：
DETERMINISTIC 不确定的
NO SQL 没有SQl语句
READS SQL DATA 只是读取数据
MODIFIES SQL DATA 要修改数据
CONTAINS SQL 包含SQL语句
```
*   创建存储过程
```sql
delimiter $$
	create procedure proc(in sid int) 
		begin
			select id, name from sh_goods_category where id > sid;
		end
$$
delimiter ;

show create procedure proc\G
show procedure status like '%proc%'\G
call proc(5);
```
*   修改和删除存储过程`alter procedure proc feature;`
	-   特征内容
		+   COMMENT ''，为存储过程设置注释内容
		+   LANGUAGE SQL，存储过程体是sql语句，未来可能会支持其他类型语句
		+   CONTAINS SQL，表示子程序中包含除读或写数据的sql语句
		+   NO SQL，表示子程序中不包含sql语句
		+   READS SQL DATA，表示子程序中包含读取数据的语句
		+   MODIFIES SQL DATA，表示子程序中包含写数据的语句
		+   SQL SECURITY DEFINER，表示只有定义者才有权执行存储过程
		+   SQL SECURITY INVOKER，表示调用者有权执行存储过程
	```sql
	alter procedure proc 
	sql sercurity invoker
	comment '从商品分类表中获取大于指定id值的数据';
	drop procedure if exists proc;
	```
*   存储过程的错误处理
	-   自定义错误名称
	```sql
	delimiter $$
		create procedure proc()
			begin
				declare command_not_allow condition for sqlstate '42000';
			end
	$$
	delimiter ;
	```
	-   错误的指定程序
	```sql
	delimiter $$
		create procedure proc_demo()
			begin
				declare continue handler for sqlstate '23000'
				set @num = 1;
				insert into sh_goods_category (id, name ) values (20, '运动');
				set @num = 2;
				insert into sh_goods_category (id, name ) values (20, '运动');
				set @num = 3;
			end
	$$
	delimiter ;
	```

### 变量
*   系统变量
	-   查看系统变量`show [global | session ] variables like '%...%';` `select @@variables_name;`
*   修改系统变量的值
```sql
# 局部修改，设置为当前会话可以用变量
set var_name = 'local';
# 全局修改
set global var_name = 'global';
set @@global.var_name = 'global';
```
*   会话变量（用户变量）`@val_name = 'name';`
```
# 使用set赋值
set @name = 'Tom';
# 在select语句中使用赋值符号（:=）赋值
select @price := price from sh_goods limit 2;
# select ... into ... 赋值
select id, name, price from sh_goods limit 1 into @ids, @names, @prices;
```
*   局部变量
	-   局部变量的作用范围仅在复合语句语法BEGIN和END中，保证局部变量在除BEGIN和END之间外的任何地方不能被获取和修改，方便MYSQL的函数和存储过程中保存需要操作的数据
	```sql
	begin
		declare val1, ..., valn var_type
	end
	delimiter $$
		create function func() returns int
			begin
				declare age int default 10;
				return age;
			end
	$$
	delimiter ;
	```

### 流程控制
*   判断语句
	-   if语句
	```sql
	# 用于sql语句结构中的if语句结构
	if (condition, true_return, false_return);
	select id, name from sh_goods where if(score=5, score, 0);

	# 用于函数或存储过程中的if语句结构
	if condition 
		then ...;
	[elseif condition1 then ...;]
	else
		...;
	end if;

	delimiter $$
		create procedure isnull (in val int)
			begin
				if val is null
					then select 'The parameter is null';
				else 
					select 'The parameter is not null';
				end if;
			end
	$$
	delimiter ;
	```
	-   case语句
	```sql
	# 用于sql语句中的case语句结构
	# case语句结构中不能省略else语句
	case condition
		when x then 'x'
		when x1 then 'x1'
	else 'xx'
	end

	(case 
		when condition then 'x'
		when condition1 then 'x1'
	else 'xx'
	end)

	select 
		id, name,
		(case
			when price <= 50 then '小额商品'
			when price <= 100 then '低价商品'
			when price <= 200 then '平价商品'
			else '中等价格商品'
		end) level_good
	from sh_goods;

	select 
		id, name,
		(case gender 
			when 1 then 'male'
			when 0 then 'female'
			else 'none'
		end) gender
	from sh_user;

	# 用于函数和存储过程的case语句结构
	case condition 
		when x then ...;
		[when x1 then ...;]
		else 'xx';
	end case;

	delimiter $$
		create procedure scoreLevel(in score decimal(10, 2))
		begin 
			case 
				when score >= 90 then select '优秀';
				when score >= 80 then select '良好';
				when score >= 70 then select '中等';
				when score >= 60 then select '及格';
				else select '不及格';
			end case;
		end
	$$
	delimiter ;
	```
	-   
*   循环语句
	-   loop语句
	```sql
	loop_name: loop
		...
	end loop loop_name;

	delimiter $$
		create procedure proc_sum()
			begin
				declare  i, sum int default 0;
				sign: loop
					if i >= 10 
						then 
							select i, sum;
							leave sign;
					else
						set sum = sum + i;
						set i  = i + 1;
					end if;
				end loop sign;
			end
	$$
	delimiter ;
	```
	-   repeat语句
	```sql
	repeat_name: repeat
		...
	until condition end repeat repeat_name;

	delimiter $$
		# 10以内的奇数和
		create procedure proc_odd()
			begin
				declare i, sum int default 0;
				sign: repeat
					if i % 2 = 1
						then set sum = sum + i;
					end if;
					set i = i + 1;
				until i >= 10 end repeat sign;
				select i, sum;
			end
	$$
	delimiter ;
	```
	-   while语句
	```sql
	while_name: while condition do
		...
	end while while_name;

	delimiter $$
		# 10以内的奇数和
		create procedure proc_even()
			begin
				declare i, sum int default 0;
				sign: while i <= 10 do
					if i % 2 = 0 
						then set sum = sum + i;
					end if;
					set i = i + 1;
				end while sign;
				select i, sum;
			end
	$$
	delimiter ;
	```
*   跳转语句
	-   leave语句
	```sql
	# iterate 只能用在循环结构中（loop, repeat, while）, leave还可用在begin ... end 中
	[leave | iterate] ...;

	delimiter $$
		create procedure proc_jump()
		begin
			declare i, sum int default 0;
			my_loop: loop
				set i = i + 1;
				if i > 5 
					then 
						set sum = sum + i;
						select i, sum;
						leave my_loop;
				else 
					select i, sum;
					iterate my_loop;
				end if;
				set num = 1;
			end loop my_loop;
		end
	$$
	delimiter ;
	```
	-   iterate语句
	```sql

	```
### 游标
*   游标是对于查询结果集的指针
*   游标的操作流程
	-   定义游标，定义游标的select语句并没有执行
	-   打开游标，游标打开时才进行select语句的查询
	-   利用游标检索数据
	-   关闭游标
	```sql
	declare cursor_name cursor for select ...;
	open cursor_name;
	fetch [[next] from ] cursor_name into ...;
	close cursor_name;

	delimiter $$
		# 将库存不足400的5星评分的商品库存增加到1500
		create procedure shgs_proc_cursor()
		begin
			# 定义局部变量，存储值
			declare mark, cur_id, cur_num int default 0;
			# 定义游标
			declare cur cursor for select id, stock from sh_goods where score = 5;
			# 自定义错误处理程序，赋值标记，结束游标遍历
			declare continue handler for sqlstate '02000' set mark = 1;
			open cur;
			repeat
				fetch cur into cur_id, cur_num;
				if cur_num >= 0 && cur_num <= 400 then
					set cur_num = 1500;
					update sh_goods set stock = cur_num where id = cur_id;
				end if;
			until mark end repeat;
			close cur;
		end
	$$
	delimiter ;
	```

### 触发器
*   触发器可以看作是一种特殊的存储过程，只不过它是当预定义好事件（insert, delete等）发生后才自动调用
*   使用触发器的优缺点
	-   优点
		1.   触发器可以通过数据库中的相关表实现级联无痕更改操作
		2.   保证数据安全，进行安全校验 
	-   缺点
		1.   触发器的使用会影响数据库结构，同时增加了维护的复杂度
		2.   触发器的无痕操作会造成数据库在程序层面的不可控
*   触发器的基本操作
	-   创建触发器，对于每个事件只能创建一个触发器
	```sql
	create trigger trigger_name [before | after] [insert| update | delete] on ta_name for each row [follows | precedes]
	begin
		...
	end

	delimiter $$
		# 商品添加购物车后自动减少商品库存
		create trigger insert_tri before insert on sh_user_shopcart for each row 
		begin
			# 定义局部变量stocks存储当前商品库存
			declare stocks int default 0;
			# 查询商品库存赋值给局部变量stocks
			# new/old关键字
			select stock into stocks from sh_goods where id = new.goods_id;
			# 根据商品库存和用户下单商品库存判断
			if stocks <= new.goods_num then
				set new.goods_num := stocks;
				update sh_goods set stock = 0 where id = new.goods_id;
			else 
				update sh_goods set stock = stocks - new.goods_num where id = new.goods_id;
			end if;
		end
	$$
	delimiter ;
	```
	-   查看触发器
	```sql
	show trigger [{from | in} db_name] [like 'pattern' | where condition]
	show triggers\G
	```
	-   触发器的触发，当触发器定义的关联的表的相关事件触发时，自动调用触发器
	-   删除触发器
	```sql
	drop trigger if exists trigger_name;
	```

### 事件
*   事件指定时在特定时间根据计划自动完成指定的任务或者是每隔多长时间根据计划做一次指定任务。事件是由mysql提供的特殊事件调度程序执行或管理的，它适用每隔一段时间就有固定需求的操作任务
*   mysql中的事件是由事件调度器Event Scheduler（特殊的线程）执行和管理的。
*   事件的基本操作
	-   开启事件调度器`set global event_scheduler = on;`
	-   创建事件
	```sql
	create event [if not exists] event_name
	on schedule time_times
	[on completion [not] preserve]
	[enable | disable]
	[comment 'event_comment']
	do begin ... end

	# 定时任务：在1分20秒后在sh_goods_category表中插入一条记录。at time
	create event insert_data_event
		on schedule at current_timestamp + interval 1 minute + interval 20 second
	do insert into sh_goods_category(id, name) values (50, '食物');

	# 定时重复任务：	从现在开始的1年时间内每天删除sh_goods表中is_on_sale为0且update_time大于30天的商品。every time
	create event if not exists delete_event
		on schedule every 1 day 
		ends current_timestamp + interval 1 year 
		on completion preserve
	do call delete_proc();

	delimiter $$
	create procedure delete_proc()
	begin
		delete from sh_goods
			where is_on_sale = 0 and to_days(now()) - to_days(update_time) >= 30;
	end
	$$
	delimiter ;
	```
	-   查看事件
	```sql
	show events\G
	show create event event_name\G
	```
	-   修改事件
	```sql
	alter event event_name
	[on schedule time_times]
	[on completion [not] perserve]
	[rename to new_event_name]
	[enable | disable]
	[comment 'event_comment']
	[do ...]

	alter event delete_event
		on schedule at current_timestamp
		on completion preserve
		rename to d_event_onetime
	do call delete_proc();
	```
	-   删除事件
	```sql
	drop [if exists] event event_name;
	```

### mysql预处理语句（sql模板技术）
*   准备预处理语句（编辑sql模板）
```sql
prepare pre_name from 'pre_sql';
prepare stmt from 'select name, price from sh_goods where id = ?';
# 将预处理语句定义在一个会话变量中
set @sql = 'select name, price from sh_goods where id=?';
prepare stmt from @sql;
```
*   执行预处理语句
```sql
execute pre_name [using @var1 [, @var2]];
```
*   释放预处理语句
```sql
[deallocate | drop] prepare pre_name;
```

```sql
# 实现sh_goods表的分页查询功能
# keyword: 存储过程、参数定义，局部变量声明
delimiter $$
	# 入参cur_page当前页数，per_page每一页的记录数
	create procedure page_proc (in cur_page int, in per_page int) 
	begin
		# 声明总记录数，总页数，开始记录数
		declare total_records, total_page, start int;
		# 查询总记录数并赋值给total_records
		select count(*) into total_records from sh_goods;
		# 入参per_page校验，如果不符合设为默认值3
		if per_page <= 1 then set per_page = 3;
		end if;
		# 计算总页数并赋值给total_page
		set total_page = ceil(total_records/per_page);

		# 入参cur_page校验，如果小于1则设为1，如果大于总页数则为最大总页数
		if cur_page < 1 then set cur_page = 1;
		elseif cur_page > total_page then set cur_page = total_page;
		end if;

		# 设置从第几条记录开始查询
		set start = (cur_page - 1) * per_page;
		# 预处理
		set @sql_stmt = concat('select * from sh_goods limit ', start, '\,', per_page);	
		prepare paging from @sql_stmt;
		execute paging;
		deallocate prepare paging;
	end
$$
delimiter ;

delimiter $$
	create procedure order_info(in o_num int)
	begin 
		set @sql_stmt = concat('select * from sh_goods where id = ', o_num);
		prepare ord_info from @sql_stmt;
		execute ord_info;
		deallocate prepare execute;
	end
$$
delimiter ;
```

### 存储引擎
*   存储引擎可以看作是数据表存储数据的一种格式。InnoDB支持事务、外键、行级锁。MyISAM支持压缩机制
*   MySQL中支持的存储引擎
	-   查看Mysql中支持的存储引擎`show engines\G`
| 存储引擎           | 默认是否支持 | 是否支持事务 | 是否支持分布式事务 | 是否支持保存点 |                     描述                     |
| ------------------ | ------------ | ------------ | ------------------ | -------------- | :------------------------------------------: |
| InnoDB             | default      | yes          | yes                | yes            |            支持事务、行级锁和外键            |
| MyISAM             | yes          | no           | no                 | no             |             支持表级锁、全文索引             |
| MRG_MYISAM         | yes          | no           | no                 | no             |              相同MyISAM表的集合              |
| MEMORY             | yes          | no           | no                 | no             | 内存存储，速度快但数据容易丢失，适用于临时表 |
| CSV                | yes          | no           | no                 | no             |          数据以文本方式存储在文件中          |
| ARCHIVE            | yes          | no           | no                 | no             | 适用于存储海量数据，有压缩功能，但不支持索引 |
| BLOCKHOLE          | yes          | no           | no                 | no             | 黑洞引擎，写入的数据都会消失，适合做中继存储 |
| PERFORMANCE_SCHEMA | yes          | no           | no                 | no             |                适用于性能框架                |
| FEDERATED          | no           | null         | null               | null           |          用于访问远程的MySQL数据库           |
*   表容量的扩容
*   表文件（表结构，表数据）

### 索引
*   索引，类似于书的目录。为了加快表数据的检索。索引是一种特殊的数据结构，将数据表中的某个字段或者多个字段与记录的位置建立一个对应的关系，并按照一定的顺序排列好
*   mysql查询的时候默认使用全表扫描，对于创建了索引的表，当查询条件中有用到索引创建时关联的字段，就会自动使用索引进行查询。mysql会对查询进行判断，如果此次查询全表扫描的时间比使用索引的短，则使用全表扫描否则使用索引进行查询
*   索引分类
	-   普通索引，mysql中的基本索引类型，使用key或index定义，不需要添任何限制条件，作用是加快对数据的访问速度
	-   唯一性索引，由unique index 定义，创建唯一性索引的字段需要添加唯一性约束，用于防止用户添加重复的值
	-   主键索引，由primary key定义的一种特殊的唯一性索引，用于根据主键自身的唯一性标识每条记录，防止添加主键索引的字段值重复或者为NULL。如果是在InnoDB表中数据保存的顺序与主键索引字段的顺序一致时，这种主键索引可称为“聚簇索引”。一般的聚簇索引指的是表的主键因此，一张数据表只能由一个聚簇索引
	-   全文索引，由fulltext index定义，用于根据查询字符提高数据量较大的字段查询速度。字段类型必须为char、varchar、text中的一种
	-   空间索引，由spatial index定义在空间数据类型字段上的索引，提高系统获取空间数据的效率
	-   前缀索引
	-   根据创建索引的字段个数分为，单列索引和复合索引
	-   单列索引，指的是在表中单个字段上创建的索引，可以时普通索引、唯一索引、主键索引或全文索引，只要保证索引对应表中一个字段即可
	-   复合索引，在表的多个字段上创建一个索引，且只有在查询条件中使用这些字段的一个，该索引才会被使用。
*   索引的基本操作
	-   创建索引，在创建数据表（create table）或在已有的数据表（alter table、create index）进行添加，向已有表添加索引时，create index不能添加主键索引
	-   确定创建索引方式、创建什么索引、索引选项。
	| 索引选项 |                             语法                             |
	| -------- | :----------------------------------------------------------: |
	| 索引类型 |                    using {BTREE \| HASH}                     |
	| 字段列表 |                col [ (length) [asc \| desc]]                 |
	| 索引选项 | key_block_size [ = ] val \| index_type \| with parser plugin_name \| comment 'comment' |
	| 算法选项 |         algorithm [ = ] {default \| inplace \| copy}         |
	| 锁选项   |      lock [=] { default \| none \| shared \| exclusive}      |

	```sql
	create table tb_name (
		col1 col_type [restriction],
		...,
		primary key index_type (col_name, ...) [index_option],
		{index | key } [index_name] [index_type] (col_name) [index_option],
		unique [index | key] [index_name] [index_type] (col_name) [index_option],
		{fulltext index | spatial} [index | key] [index_name] [index_type] (col_name) [index_option]
	) [tb_option];

	alter table tb_name 
		add primary key [index_type] (col_name, ) [index_option],
		| {index | key } [index_name] [index_type] (col_name) [index_option],
		| unique [index | key] [index_name] [index_type] (col_name) [index_option],
		| {fulltext index | spatial} [index | key] [index_name] [index_type] (col_name) [index_option], ...,

	create [unique | fulltext | spatial] index index_name 
		[index_type] on tb_name (col,...) [index_option] {alg_type | lock_option};

	alter table sh_goods add index name_index(name);	
	alter table sh_goods add unique index unique_index(id);
	alter table sh_goods add fulltext index ft_index(content);
	# 使用全文索引，match (col) against('word')

	create table mydb.sp_table (space geometry not null);
	alter table mydb.sp_table add spatial index (space);

	alter table sh_goods add index multi_index (name, price, keyword);
	```
	-   查看索引
	```sql
	show {indexes | index | keys} from tb_name\G

	{explain | descirbe | desc} {select | delete | insert | replace | update } statement 
	```
	-   删除索引
		+   删除主键索引
		```sql
		# auto_increment属性会使删除主键索引失败，先修改主键字段属性，再删除主键索引
		alter table tb_name modify pmk_col col_type ;
		alter table tb_name drop primary key;  
		[drop index `primary` on tb_name;]
		```
		+   删除非主键索引
		```sql
		alter table tb_name drop index index_name;
		drop index index_name on tb_name [alg_option] [lock_option];

		drop index name_index on sh_goods;
		```
	-   索引使用原则
		+   查询条件中频繁使用的字段适合建立索引
		+   数字型的字段适合建立索引
		+   存储空间较小的字段适合建立索引
		+   重复值比较高的字段不合适建立所以
		+   更新频繁的字段不适合建立索引
	-   对表的查询使用索引
		+   保证查询字段的独立性（不应将字段和关系运算符一起使用做为条件表达式）
		+   模糊查找中通配符的使用（在要like后面的字符串开头不应使用通配符，使用了之后就默认全表扫描）
		+   分组查询时排序的设置（在进行数据分组时默认会对数据进行排序，如果不需要排序可以在分组后使用order by null禁止排序）

### 锁机制
*   锁机制，为了防止并发情况下，不同用户同时对同一资源（数据表中的记录）进行的操作，导致数据不一致。	
*   锁分类
	-   按锁的作用范围（锁的粒度）分
		+   表级锁（MyISAM、MEMORY、InnoDB），它锁定的是用户操作资源所在的整张数据表，有效避免了死锁的发生，且具用加锁速度、消耗资源小的特点，但因其锁定的粒度大，并发操作时发生锁冲突的概率也大。
		+   行级锁（InnoDB），是锁粒度最小的一种锁，它仅仅锁定用户操作锁涉及的记录资源，有效的减少了锁定资源竞争的发生，具有较高的处理并发能力，提升系统的整体性能。但因为多锁粒度过小，每次加锁和解锁消耗的资源也会更多，发生死锁的可能性更高。
	-   按锁的状态分
		+   隐式锁，指mysql服务器本身对数据资源的争用进行管理，它完全由服务器自动执行
		+   显式锁，指用户根据实际需求，对操作数据显式地添加锁，同样在使用完数据资源后也需要用户对其进行解锁
	-   锁等待与死锁，锁等待指的是一个用户（线程）等待其他用户（线程）释放锁的过程。而死锁就是两个或多个用户（线程）在互相等待对方释放锁而出现的一种“僵持”状态，此时就可以说系统产生了死锁或者处于死锁状态。
*   表级锁
	-   在实际应用中，表级锁根据操作的不同可以分为读锁和写锁。
	-   读锁（共享锁），表示用户在读取（如select查询）数据资源时添加的锁，此时其他用户虽然不可以修改或增加数据资源，但是可以读取该数据资源，因此读锁也被称为共享锁。
	-   写锁（排他锁/独占锁），表示用户对数据执行写（insert、update、delete）操作时添加的锁，此时除了当前添加写锁的用户外，其他用户都不能对其进行读/写操作，因此写锁被称为排他锁或独占锁。
	-   隐式读/写的表级锁，当用户对表进行（select）查询操作时，服务器会自动地为其添加一个表级的读锁，执行（insert、update、delete）写操作时，服务器会自动为其添加一个表级的写锁，直到执行完成，服务器才为其自动解锁。执行时间可以看作时隐式读/写锁的生命周期，其该生命周期都比较短暂。服务器在自动添加读写锁时，表的更新操作优先级高于表的查询操作。在添加写锁时，如果表中没有任何锁则添加，否则将其插入到写锁等待的队列中，添加读锁时，若表中没有写锁则添加，否则则将其插入到读锁等待的队列中
	-   显式读/写的表级锁
	```sql
	# 显式添加表级锁，可以同时锁定多张表
	# read表示表级的读锁，加锁的用户可以读取该表，但不能进行写操作。其他用户可以读取该表，进行写操作的话会进入等待队列
	# write表示表级的写锁，加锁的用户可以读写该表，在释放锁之前，其他用户不能访问与操作
	lock tables tb_name read [local] | write, ...
		....
	unlock tables; 
	# 在用户A添加读锁时，只能读取加锁的表，不能进行写操作，不能操作其他未加锁的表，
	# 其他用户可以读取表，但是在写操作时，会进入等待，当用户A解锁表之后从执行操作

	# 并发插入，用于减少读操作与写操作对表竞争的情况
	# 客户端A
	lock tables tb_name read local;
	# 客户端B，插入操作不用等待
	insert into tb_name value (4);

	lock tables tb_name write;
	```
	-   
*   行级锁（InnoDB）
	-   InnoDB中的表级锁和MyISAM中的一样，只有通过索引条件检索的数据InnoDB才会使用行级锁，否则将使用表级锁
	-   InnoDB的行级锁根据操作的不同也分为共享锁和排他锁。
	-   隐式行级锁，当用户对InnoDB存储引擎表执行insert、update、delete等写操作时，服务器会自动通过索引条件检索的记录添加行级排他锁，直到操作语句执行完毕，服务器在为其自动解锁，语句的执行时间可以看成隐式行级锁的生命周期，且该声明周期持续的时间一般较短。若要增加行级锁的生命周期，通常使用事务操作，让其在事务提交和回滚之后在释放行级锁，使行级锁的生命周期与事务相同。
	```sql
	create table mydb.row_lock(
		`id` int unsigned primary key auto_increment,
		`name` varchar(30) not null,
		`cid` int unsigned,
		key `cid`(`cid`)
	) default charset=utf8mb4;

	insert into mydb.row_lock (name, cid) values 
	('铅笔', 3),
	('风扇', 6),
	('绿萝', 1),
	('书包', 9),
	('纸巾', 20);
	```
	-   显式行级锁，
	```sql
	# mysql的锁定读取方式为查询操作显式地添加行级锁
	# 为查询操作时添加行级排他锁，
	select statement ... for update;

	# 为查询操作时添加行级共享锁，
	select statement ... lock in share more;
	```
	-   在向InnoDB中显式添加行级锁时，InnoDB会先自动为表添加一个意向锁，然后再为资源（记录）添加行级锁。意向锁是mysql服务器根据行级锁是共享锁还是排他锁，自动添加意向共享锁或意向排他锁，不能认为干预。该意向锁是一个隐式表级锁，多个意向锁之间不会产生冲突并互相兼容。意向锁就是标识表中的某些记录正在被锁定或其他用户将要锁定表中的某些记录。相对行级锁，意向锁的粒度更大，用于在行级锁中添加表级锁时判断他们是否能够互相兼容。大大节约了存储引擎对锁处理的性能，更加方便地解决了行级锁和表级锁之间的冲突。
	|            | 表级共享锁 | 表级排他锁 | 意向共享锁 | 意向排他锁 |
	| :--------: | :--------: | :--------: | :--------: | :--------: |
	| 表级共享锁 |    兼容    |    冲突    |    兼容    |    冲突    |
	| 表级排他锁 |    冲突    |    冲突    |    冲突    |    冲突    |
	| 意向共享锁 |    兼容    |    冲突    |    兼容    |    兼容    |
	| 意向排他锁 |    冲突    |    冲突    |    兼容    |    兼容    |
	-   InnoDB表中当前用户的意向锁若与其他用户要添加的表级锁冲突时，有可能造成死锁
	-   默认情况下，InnoDB处于repeatable read（可重复读）的隔离级别时，行级锁实际上是一个next-key锁，它是由间隙锁（gap lock）和记录锁（record lock）组成的。其中记录锁就是前面说的行锁，间隙锁指的是在记录索引之间的间隙、负无穷到第一个索引记录之间或最后一个索引记录到正无穷之间添加的锁，它的作用就是并发时防止其他事务在间隙插入记录，解决了事务幻读的问题
	```sql
	# client A
	start transaction;
	select * from mydb.row_lock where id = 3 for update;

	# clientB
	insert into mydb.row_lock (name, cid) value ('电视', 1);
	insert into mydb.row_lock (name, cid) value ('电视', 2);
	insert into mydb.row_lock (name, cid) value ('电视', 5);
	insert into mydb.row_lock (name, cid) value ('电视', 6);
	```
	-   

### 分表技术
*   分表技术
	-   水平分表
		+   水平分表使单张表的数据能够保持在一定的量级，在操作时又因其表机构相同，只需增加获取对应分表名称的运算，就可以提高系统的稳定性和负载能力。但同时，水平分表使得数据分散存储，加大了数据的维护难度
	-   垂直分表
		+   垂直分表后业务逻辑更加清晰，方便数据进行整合与扩展，还可以根据实际需求实现动静分离，为各分表选择不同的存储引擎（如查询操作多可以使用MyISAM），但垂直分表后，需要管理冗余字段、查询所有数据需要进行连接
*   分区技术
	-   分区概述
	-   分区管理
	-   

### 数据库慢查询
*   慢查询日志时mysql提供的一种记录所有时间超过指定时间界限的sql语句，然后可以根据写入日志内的语句对mysql进行优化
*   mysql中默认是没有开启慢查询日志的，需要在mysql配置文件my.ini/my.cnf中修改对应的配置

```
# 修改配置文件开启慢查询 ubuntu /etc/mysql/my.cnf 需要重启mysql
# 需要写入的日志文件记得要所有者和所在的组要赋予mysql(chown mysql:adm log_file.log)
# [mysqld]
# 在/etc/mysql/mysql.conf.d/mysqld.cnf文件（这个文件才是mysql服务端的配置文件）中加入以下选项
# mysql slow query
slow_query_log = on
slow_query_log_file = /var/log/mysql/slow.log
long_query_time = 0.5

# mysql-cli修改全局变量开启慢查询
set global slow_query_log = on;
set  global slow_query_log_file = '/var/log/mysql/slow.log';
set global long_query_time = 0.5;
```
*   因为慢查询日志用来优化和调试sql语句，但开启后会占用一定的系统资源和空间，推荐在开发调试环境中开启，项目上显示要将其关闭
*   开启记录查询的精确时间，如果想要获取的精度到毫秒级别，可以启动mysql的profile机制，它会记录每次操作的具体时间（精度为小数点后8位）
```
set profiling=1;
# run sql statement
select pid, count(*) from my_user group by pid;
show profiles\G
```
*   

### 数据碎片与维护
*   mysql中，delete删除一条记录时，仅仅删除了数据表中保存的数据，而记录占用的存储空间则会被保留。因此，长期删除数据、添加数据数据过程中，索引文件和数据记录文件都将产生“空洞”，形成很多不连续的碎片，造成数据表占用空间变大，但表中的记录却很少的情况发生
*   使用mysql提供的optimize table（MyISAM、InnoDB）重新组织表中的数据和关联索引数据的物理存储，减少存储空间并提高访问表时的I/O效率

```sql

# 创建新表并填入测试数据（50万条）
create table mydb.my_optimize(
	id int unsigned primary key auto_increment,
	name varchar(20) not null default ''
);
insert into mydb.my_optimize (name) values ('TOM'), ('JIMMY'), ('LUCK'), ('CAKE');

# 重复执行该语句
insert into mydb.my_optimize (name) select name from mydb.my_optimize;

delimiter $$
	create procedure insert_test(in t int)
	begin
		declare i int default 0;
		my_loop: loop
			if i = t then leave my_loop;
			end if;

			set i = i + 1;
			insert into mydb.my_optimize (name) select name from mydb.my_optimize;
		end loop my_loop;
	end
	$$
delimiter ;

call insert_test(20);
# 查看数据表的物理大小，在mysql的data目录下
delete from mydb.my_optimize where name = 'LUCK';
# 再次查看数据表的物理大小，在mysql的data目录下

optimize table mydb.my_optimize\G
*************************** 1. row ***************************
   Table: mydb.my_optimize
      Op: optimize
Msg_type: note
Msg_text: Table does not support optimize, doing recreate + analyze instead
*************************** 2. row ***************************
   Table: mydb.my_optimize
      Op: optimize
Msg_type: status
Msg_text: OK
2 rows in set (7.60 sec)

# InnoDB引擎不支持optimize table 操作，系统自动使用alter table ... force 重新构建表并重新整理相关的数据碎片
# 释放为使用的存储空间
# OP: optimize 表示执行optimize操作
# Msg_type: status 表示信息的类型[error|info|warning] 
# Msg_text: 表示具体的返回信息内容
 
# 还可以使用alter table将数据表的存储引擎改为当前数据表的存储引擎，实现对数据碎片的整理
alter table mydb.my_optimize engine='InnoDB'; 
# 修复数据表的数据及索引碎片时，会把所有的数据文件重新整理一遍。如果数据表过大，会消耗一定的资源
# 因此不能频繁地对数据碎片进行维护，可根据情况按周、月、季度进行维护
```
*   动手数据库优化实践

```sql
# 1. 在mydb数据库中创建my_user表并添加两百万条数据用于测试。my_user表字段有id、name、pid
# 2. 开启慢查询日志，获取查询时间超过0.5 秒的查询语句信息
# 3. 开启profile机制，获取语句执行的精确时间
# 4. 为pid添加索引，优化查询语句，增强查询效率
# 5. 优化limit分页查询效率
# 6. 启动查询缓存功能，优化查询效率

drop table if exists mydb.my_user;
create table if not exists mydb.my_user(
	`id` int unsigned primary key auto_increment comment '用户id',	
	`name` varchar(30) not null default '' comment '用户名',
	`pid` int unsigned not null default 0 comment '部门编号'
) engine=InnoDB default charset=utf8mb4 collate=utf8mb4_general_ci comment '测试用-用户表';

drop function if exists mydb.rand_str;
delimiter $$
	create function mydb.rand_str(len int) returns varchar(50)
	no sql
	begin
		declare chars_str varchar(100) default 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
		declare res varchar(50) default '';
		declare i int default 0;
		while i < len do
			set res = concat(res, substring(chars_str, floor(1 + rand() * 52), 1));
			set i = i + 1;
		end while;
		return res;
	end
	$$
delimiter ;

drop procedure if exists mydb.my_user_pro;
delimiter $$
	create procedure mydb.my_user_pro(in max_num int)
	begin
		declare i int default 0;
		repeat
			set i = i + 1;
			insert into mydb.my_user(name, pid) values (rand_str(5), floor(1 + rand() * 10));
		until i = max_num end repeat;
	end
$$
delimiter ;

# 为pid字段添加索引，增加表存储的物理大小，但是使查询速度增快了
alter table mydb.my_user add index (pid);

# limit 分页优化
select * from mydb.my_user limit 90000, 10;
select * from mydb.my_user limit 900000, 10;
show profiles\G

# 通过修改sql语句的方式优化limit分页，要求排序字段从1开始，并且能够连续自增，
# 一般都是在业务上限制页数方式优化分页
select * from mydb.my_user where id > 90000 order by id limit 10;
select * from mydb.my_user where id > 900000 order by id limit 10;

# 查询缓存，是mysql服务器提供的一种对select语句查询结果的缓存机制
# 对于数据更新不频繁、数据结构不经常变动可以采用查询缓存。
# 查看缓存相关配置变量信息
show variables like '%query_cache%';

alter table db_shop.sh_order_goods add unique index multi_index(order_id, goods_id);
-- insert into mydb.my_user(name, pid) values
-- (''),(''),(''),('');
-- delimiter $$
-- 	create procedure inrt_user_test(in t int)
-- 	begin
-- 		declare i, s_num, mark int default 0;	
-- 		if t > 20 then set t = 20;
-- 		end if;	

-- 		select count(*) into s_num from mydb.my_user;

-- 		if s_num > 1000000 then set mark  = 1;
-- 		end if;

-- 		my_loop: loop
-- 			if mark then leave my_loop;
-- 			elseif i > t then leave my_loop;
-- 			end if;
-- 			set i = i + 1;
-- 			insert into mydb.my_user (name, pid) select name, pid from mydb.my_user;
-- 		end loop my_loop;
-- 	end
-- $$
-- delimiter ;
```
*   

### 数据备份与还原
*   数据备份
	-   mysqldump
	```
	# 备份单个数据库时不含创建单个数据库的语句，所以需要手动创建数据库然后导入
	# 备份多个数据库时，会有自动创建数据库的语句
	# 备份单个数据库，备份一个或者多个数据表
	mysqldump -uusername -ppassword dbname [tbname1 [, tbname2....]]
	# 备份一个或多个数据库
	mysqldump -uusername -ppassword --datebase dbname1 [dbname2...]
	# 备份所有数据库
	mysqldump -uusername -ppassword --all-datebases
	# 因为mysqldump执行后带出的sql语句，会直接输出到终端上，所有加上 > db_name.sql，输出重定向写入到文件中
	# 为了确保数据的一致性，mysqldump在备份时会先进行锁表，对于InnoDB的数据表可以使用参数--single-transaction来利用事务避免锁表
	```
*   数据还原
	-   输入重定向
	```shell
	mysql -uusername -ppassword [dbname] < db_name.sql
	```
	-   source命令（mysql-cli命令）
	```sql
	source db_name.sql;
	```
	-   
*   二进制日志
	-   二进制日志用于记录MySQL数据库的变化，如表的创建、对表数据的更改，记录涉及修改的sql、数据修改的行改变、执行时间
	-   二进制日志可以应用于搭建复制架构（如主从复制）、数据恢复等场景
	-   开启二进制日志，默认情况下二进制日志时关闭的
	```cnf
	# my.cnf
	[mysqld]
	log-bin = binlog
	server-id = 1
	# binlog是二进制日志的文件名
	# 在开启二进制日志时需要使用server-id指定服务器id，用于在复制架构中区分服务器
	# 确保每个服务器的id不同即可
	```
	-   查看二进制日志，上面的配置把mysql的二进制文件放在 `/var/lib/mysql` 文件中
	```shell
	ls -al /var/lib/mysql | grep binlog
	| rw-r-----.  1 mysql mysql      760 11月  8 03:03 binlog.000001
	| -rw-r-----.  1 mysql mysql      180 11月  8 16:09 binlog.000002
	| -rw-r-----.  1 mysql mysql      180 11月  8 16:14 binlog.000003
	| -rw-r-----.  1 mysql mysql     4395 11月  8 17:32 binlog.000004
	| -rw-r-----.  1 mysql mysql      157 11月  8 17:32 binlog.000005
	| -rw-r-----.  1 mysql mysql       80 11月  8 17:32 binlog.index

	# binlog.xxxx文件用于保存二进制文件的内容，当MySQL服务器重新启动，或者文件大小达到了
	# max_binlog_size配置的上限（默认为1GB），就会创建一个新的日志文件，新的日志文件扩展名加1递增
	# mysql-cli也可以使用show binary logs查看二进制日志
	```
	-   查看二进制日志文件内容，二进制日志使用二进制方式保存，使用mysqlbinlog工具将其转换为文本格式的sql脚本
	
	```
	mysqlbinlog /var/lib/mysql/binlog.000001
	# 将二进制日志转换为sql脚本后就可以将其用管道传入到mysql-cli中运行
	mysqlbinlog /var/lib/mysql/binlog.000001 | mysql -uuser -ppassword
	# 利用二进制日志可以灵活地进行数据地备份和恢复，尤其是当数据库中地数据量非常大时
	# 如果使用完整备份的方式，每次备份的数据量会非常大，速度也慢，而二进制日志记录了数据库发生过的变化
	# 定期备份二进制日志文件就可以实现增量备份的效果
	mysqlbinlog [option] binlog_name
	# 通过选项指定根据日期、位置等来恢复指定区间的数据变化
	# --start-date: 指定开始日期 
	# --stop-date: 指定结束日期
	# --start-position: 指定开始位置
	# --stop-positon: 指定结束位置
	mysqlbinlog --stop-date='2022-10-11 23:23:23' /var/lib/mysql/binlog.000001 | mysql -uuser -ppassword
	# mysql-cli中使用show master status\G 来查看当前数据库位置
	```
	-   暂停二进制日志
	```sql
	set bin-log = 0;
	```
	-   删除二进制日志，MySQL服务器运行中若直接删除日志文件，会导致程序出错。可以使用reset或purge删除
	```sql
	reset master;
	purge master logs to 'binlog.000002';
	purge master logs before '20180723'; 
	```
	-   

### 多实例部署
*   多实例是指在一台服务器中运行多个MySQL实例，通过监听不同的端口来区分。当一台服务器资源有剩余时，以利用这些资源来提供更多的服务
*   部署操作
	1.   更改配置文件
	```cnf
	# /etc/my.cnf
	[mysqld@replica01]
	port = 3307
	datadir = /var/lib/mysql-replica01
	socket = /var/lib/mysql-replica01/mysql.sock
	log-error = /var/lib/mysql-replica01/mysqld.log
	[mysqld@replica02]
	port = 3308
	datadir = /var/lib/mysql-replica02
	socket = /var/lib/mysql-replica02/mysql.sock
	log-error = /var/lib/mysql-replica02/mysqld.log
	```
	2.   初始化数据库
	```shell
	# 创建mysql实例的datadir目录，并授权给mysql
	mkdir -p /var/lib/mysql-replica0{1,2}
	chown -R mysql:mysql /var/lib/mysql-replica*
	# 初始化mysql实例
	mysqld --initialize-insecure --datadir=/var/lib/mysql-replica01
	mysqld --initialize-insecure --datadir=/var/lib/mysql-replica02
	```
	3.   启动多实例服务
	```shell
	# 首先要先关闭selinux，否则启动服务的时候会报错
	setenforce 0
	sed -i '/^SELINUX=/cSELINUX=disabled' /etc/selinux/config
	# 启动实例
	systemctl start mysqld@seplica01
	systemctl start mysqld@seplica02
	# 设置开机自启动
	systemctl enable mysqld@replica01
	systemctl enable mysqld@replica02
	# 查看实例绑定端口
	ss -atnlp | grep mysql
	```
	4.   登录设置密码
	```
	# 因为初始化时设置参数 -insecure 所以登录实例的root用户是不用密码的，登录修改密码
	mysql -h localhost -P 3307 -p -S /var/lib/mysql-replica01/mysql.sock
	alter user 'root'@'localhost' identified with mysql_native_password by 'qwe123';

	mysql -h localhost -P 3308 -p -S /var/lib/mysql-replica02/mysql.sock
	alter user 'root'@'localhost' identified with mysql_native_password by 'qwe123';
	```
	5.   

### 主从复制
*   MySQL支持数据库复制（replication）功能，从一台MySQL主服务器（master）将数据复制到另一台或多台MySQL从服务器（salves），这个过程就是主从复制
*   主从复制是通过二进制日志实现的，主服务器的二进制日志将会发送给从服务器执行，从而使从服务器的数据与主服务器的数据同步。主从复制的优点就是从服务器可以分担主服务器的压力，并且从服务器可以作为主服务器的备份，防止其中一台服务器故障而数据丢失
*   主从复制操作
	-   修改主从服务器配置文件，并重启两个服务
	```cnf
	# 开启主服务器的二进制日志并配置主服务器id
	# /etc/my.cnf
	[mysqld@replica01]
	log-bin = binlog
	server_id = 2

	mysqlbinlog --start-date '2022-10-11' /var/lib/mysql/binlog.000001 | mysql -h localhost -P 3308 -S /var/lib/mysql-replica02/mysql.sock -uroot -pqwe123 

	# 配置从服务器id
	server_id = 3
	```
	-   登录MySQL主服务器，并创建创建用户slave并赋予权限。
	```
	mysql -h localhost -P 3307 -S /var/lib/mysql-replica01/mysql.sock -u root -pqwe123

	create user 'slave'@'localhost' identified with mysql_native_password by 'slave123';
	grant replication slave on *.* to 'slave'@'localhost';
	```
	-   查看主服务器当前二进制日志状态
	```
	# 记住当前二进制文件file(binlog.000003)和位置信息position(969) 
	show master status;
	```
	-   登录从服务器同步数据。
	```
	mysql -h localhost -P 3308 -S /var/lib/mysql-replica02/mysql.sock -u root -pqwe123

	change master to master_host = 'localhost', master_port = 3307, master_log_file = 'binlog.000003', master_log_pos = 969;
	start slave user = 'slave' password = 'slave123';
	show slave status\G

	Slave_IO_State: Waiting for source to send event
	# 主服务器的主机
	Master_Host: localhost
	# 主服务器的用户名
	Master_User: slave
	# 主服务器的端口好
	Master_Port: 3307
	# 连接失败重试秒数
	Connect_Retry: 60
	# 主服务器二进制日志文件
	Master_Log_File: binlog.000003
	# 主服务器二进制日志读取位置
	Read_Master_Log_Pos: 2080
	# 从服务器的中继日志文件
	Relay_Log_File: localhost-relay-bin.000002
	# 从服务器的中继日志的位置
	Relay_Log_Pos: 1148
	# 从服务器的中继日志文件
	Relay_Master_Log_File: binlog.000003
	# 从服务器I/O线程是否运行
	Slave_IO_Running: Yes
	# 从服务器SQL线程是否运行
	Slave_SQL_Running: Yes
	```
	-   主从复制测试。
	```
	# 登录主服务器，进行操作，添加数据测试
	mysql -h localhost -P 3307 -S /var/lib/mysql-replica01/mysql.sock -u root -pqwe123

	create database `mydb`default charset = utf8mb4 collate = utf8mb4_general_ci;
	create table `mydb`.`test_tb` (`id` int unsigned primary key auto_increment, `name` varchar(30) not null default '');
	insert into `mydb`.`test_tb`(`name`) values ('Tom'), ('Jerry'), ('Coki');

	# 登录从服务器，查看从服务器状态和数据
	mysql -h localhost -P 3308 -S /var/lib/mysql-replica02/mysql.sock -u root -pqwe123

	show databases;
	select * from `mydb`.`test_tb`;
	```
	-   查看数据库线程
	```sql
	show processlist;
	```
	-   
*   可以看到在主服务器中写入数据后，从服务器也会有相同的数据，但是从服务器中写入数据，并没有同步到主服务器。所以实际使用中，从服务器负责查询操作，而对于添加、修改、删除等操作则是在主服务器中进行。
*   主从数据库数据不一致
	-   可能原因
		+   网络延迟
		+   主从服务器除了异常
		+   mysql自带bug
		+   从库进行了非正当的操作（从库执行了写操作，如添加、修改、删除，使主库写进程断开）
		+   主键自增id不一致
		+   使用了有关MySQL随机生成函数作为字段值（可以将数据库配置参数binlog_format改为row避免）
	-   解决方法
		+   重做主从复制，使数据完全同步
		+   忽略错误继续同步（不推荐），主从库数据相差不大，或者要求数据可以不完全统一的情况，数据要求不严格的情况
*   动手操作：组复制（）
	-   MySQL组复制（MySQL Group Replication）是MySQL的高可用、易于扩展、可靠性强的部署方案，它具有良好的扩展能力，可以动态增加和删除成员，确保组内数据的一致性。MySQL组复制要求使用事务存储引擎InnoDB
	-   在组复制中，主节点并不是固定的，若主节点发生宕机，会自动选举一个从节点替代主节点，允许写入数据并与其他从节点同步数据
	-   组复制中，只允许主节点写入，如果在从节点中执行写操作，则会报错，提示当前处于只读模式，无法执行语句

```
# MySQL组复制支持单主模式和多主模式，该实例下用的是单主模式
# 方案配置为：服务器1为主节点，服务器2、服务器3为从节点，从节点的数据和主节点是同步的，并且只允许读取数据

# 1.  更改配置文件，/etc/my.cnf
[mysqld]
# GTID
# 服务器id，用于区分服务器
server-id = 1
# 开启GTID（全局事务id）模式
gtid_mode  = on
# 保证GTID的安全
enforce_gtid_consistency = on
# 主服务器信息记录为表
master_info_repository = TABLE
# 中继日志信息记录为表
relay_log_info_repository = TABLE
# 二进制日志不进行校验
binlog_checksum = none

# binlog
# 开启二进制日志
log_bin = binlog
# 接收的更新是否记录到自己的二进制日志中
log-slave-updates = 1
# 设置二进制日志格式为行
binlog_format = row
sync-master-info = 1
sync_binlog = 1

# 组复制配置
# 记录事务的算法，此处使用64位的xxHash标识来记录事务
transaction_write_set_extraction = XXHASH64
# 配置组名称，需要使用UUID格式
#loose-group_replication_group_name = 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
loose-group_replication_group_name = '1cdfe742-08c0-4a46-8566-02231f5e62a7'
# 是否随服务器启动而自动启动组复制
loose-group_replication_start_on_boot = off
# 本地的ip地址和端口号
loose-group_replication_local_address = '127.0.0.1:33060'
# 组中的各成员的ip地址和端口号
loose-group_replication_group_seeds = '127.0.0.1:33060,127.0.0.1:33061,127.0.0.1:33062'
# 是否是组中的引导节点
loose-group_replication_bootstrap_group = off
# loose-group_replication_group_seeds中配置了3个成员，ip都是127.0.0.1，端口分别位33060、33061、33062，对应的是mysqld、mysqld@replica01、mysql@replica02实例，并不是MySQL服务的端口号3306、3307、3308，在组复制中，成员服务器之间会使用专门的ip地址和端口号进行通信，因此在loose-group_replaction_local_address配置中使用33060作为端口号


[mysqld@replica01]
# GTID
# 服务器id，用于区分服务器
server_id = 2
# 开启GTID（全局事务id）模式
gtid_mode  = on
# 保证GTID的安全
enforce_gtid_consistency = on
# 主服务器信息记录为表
master_info_repository = TABLE
# 中继日志信息记录为表
relay_log_info_repository = TABLE
# 二进制日志不进行校验
binlog_checksum = none

# binlog
# 开启二进制日志
log_bin = binlog
# 接收的更新是否记录到自己的二进制日志中
log-slave-updates = 1
# 设置二进制日志格式为行
binlog_format = row
sync-master-info = 1
sync_binlog = 1

# 组复制配置
# 记录事务的算法，此处使用64位的xxHash标识来记录事务
transaction_write_set_extraction = XXHASH64
# 配置组名称，需要使用UUID格式
#loose-group_replication_group_name = 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
loose-group_replication_group_name = '1cdfe742-08c0-4a46-8566-02231f5e62a7'
# 是否随服务器启动而自动启动组复制
loose-group_replication_start_on_boot = off
# 本地的ip地址和端口号
loose-group_replication_local_address = '127.0.0.1:33061'
# 组中的各成员的ip地址和端口号
loose-group_replication_group_seeds = '127.0.0.1:33060,127.0.0.1:33061,127.0.0.1:33062'
# 是否是组中的引导节点
loose-group_replication_bootstrap_group = off

[mysqld@replica02]
# GTID
# 服务器id，用于区分服务器
server_id = 3
# 开启GTID（全局事务id）模式
gtid_mode  = on
# 保证GTID的安全
enforce_gtid_consistency = on
# 主服务器信息记录为表
master_info_repository = TABLE
# 中继日志信息记录为表
relay_log_info_repository = TABLE
# 二进制日志不进行校验
binlog_checksum = none

# binlog
# 开启二进制日志
log_bin = binlog
# 接收的更新是否记录到自己的二进制日志中
log-slave-updates = 1
# 设置二进制日志格式为行
binlog_format = row
sync-master-info = 1
sync_binlog = 1

# 组复制配置
# 记录事务的算法，此处使用64位的xxHash标识来记录事务
transaction_write_set_extraction = XXHASH64
# 配置组名称，需要使用UUID格式
#loose-group_replication_group_name = 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
loose-group_replication_group_name = '1cdfe742-08c0-4a46-8566-02231f5e62a7'
# 是否随服务器启动而自动启动组复制
loose-group_replication_start_on_boot = off
# 本地的ip地址和端口号
loose-group_replication_local_address = '127.0.0.1:33062'
# 组中的各成员的ip地址和端口号
loose-group_replication_group_seeds = '127.0.0.1:33060,127.0.0.1:33061,127.0.0.1:33062'
# 是否是组中的引导节点
loose-group_replication_bootstrap_group = off

# 2.  重启三个节点的mysqld服务实例，创建用户并指定复制通道，组复制和主从复制一样也要创建用于其他服务器访问的用户，组复制中每台服务器都需要创建用户，因为每一台服务器都可能是主节点
# 新建三个命令窗口，分别登录对应mysql服务的mysql-cli并创建rpl用户，赋予replication slave权限
# 暂停二进制日志，不记录创建用户操作
set sql_log_bin = 0;
create user 'rpl'@'%' identified by 'Slave@123';
grant replication slave on *.* to 'rpl'@'%';
set sql_log_bin = 1;
# 不指定主服务器，而通过指定组复制通道‘group_replication_recovery’进行数据同步
change master to master_user = 'rpl', master_password = 'Slave@123', get_master_public_key = 1 for channel 'group_replication_recovery';
change master to master_user = 'rpl', master_password = 'Slave@123' for channel 'group_replication_recovery';
# 安装组复制插件（搭建组复制需要依赖此插件）
install plugin group_replication soname 'group_replication.so';

# 3.  启动组复制，安装插件之后， 在新建的组中，必须有一个是引导节点，负责创建组，创建组之后其他成员才可以加入组
# 在第一个实例的mysql-cli，作为引导节点，创建组
set global group_replication_bootstrap_group = on;
start group_replication;
set global group_replication_bootstrap_group = off;
# 创建组之后第一个加入的节点被选为主节点，查看组成员信息
select * from performance_schema.replication_group_members\G

mysql> select * from performance_schema.replication_group_members\G
*************************** 1. row ***************************
# 组通道名称
CHANNEL_NAME: group_replication_applier
# 组成员id
MEMBER_ID: a6881284-5ec8-11ed-9cc2-08002771e340
# 组成员主机
MEMBER_HOST: localhost.localdomain
# 组成员端口
MEMBER_PORT: 3306
# 组成员状态
MEMBER_STATE: ONLINE
MEMBER_ROLE: PRIMARY
MEMBER_VERSION: 8.0.31
MEMBER_COMMUNICATION_STACK: XCom
```
*   

### 常用函数
*   ASCII()，查询字符的ASCII码
*   BIN()，把字符转换为二进制数
*   OCT()，把字符转换为八进制数
*   HEX()，把字符转换为十六进制数
*   CONV(target, con1, con2)，把target从con1进制数转换为con2进制数。 
*   CONVERT(x, type)，以type类型返回x
*   CONVERT(x USING charset)，以指定字符集返回x数据
*   CAST(x AS type)，以type类型返回x
*   UNHEX(x)，将x转为十六进制数字，然后再转为由数字表示的字符
*   CONCAT()，拼接字符串
*   GROUP_CONCAT()，用”,“拼接指定列的值，用于group by中不包含的列
*   常用数学函数
	-   CEIL(x)，返回小于等于x的最小整数
	-   FLOOT(x)，返回大于等于x的最小整数
	-   FORMAT(x, y)，返回小数点后保留y位的x（进行四舍五入）
	-   ROUND(x\[, y\])，计算离x最近的整数；若设置参数y，则与FORMAT(x, y)功能相同
	-   TRUNCATE(x, y)，返回小数点后保留y位的x（舍弃多余小数点，不进行四舍五入）
	-   ABS(x)，获取x的绝对值
	-   MOD(x, y)，求模运算，与x%y的功能相同
	-   PI()，计算圆周率
	-   SQRT(x)，求x的平方根
	-   POW(x, y)，幂运算函数，计算x的y次方，与POWER(x, y)功能相同
	-   RAND()，默认返回0~1之间的随机数，包括0和1。
	```sql
	# 取指定范围内的随机数 floor(min + rand()*(max - min))
	select floor(1 + rand() * (100 - 1));
	# 取相同随机数 rand(random_num_id) random_num_id只能时整数
	select rand(4);
	# 与order by 子句一起用，取随机获取一条数据
	select group_concat(id), group_concat(name), category_id from sh_goods group by category_id order by rand() limit 1;
	```
*   常用字符串函数
	-   CHAR_LENGTH()，获取字符串长度
	-   LENGTH()，获取字符串占用的字节数。
	-   REPEAT()，重复指定次数的字符串，并保存到一个新的字符串中
	-   SPACE()，重复指定次数的空格，并保存到一个新的字符串中
	-   UPPER()，把字符串全部转换为大写字母，和UCASE()函数等价
	-   LOWWER()，把字符串全部转换为小写字母，和LCASE()函数等价
	-   STRCMP()，比较两个字符串的大小
	-   REVERSE()，颠倒字符串的顺序
	-   SUBSTRING()，从字符串的指定位置开始获取指定长度的字符串。和MID()函数等价
	-   LEFT()，截取并返回字符串的左侧指定个字符
	-   RIGHT()，截取并返回字符串的右侧指定个字符
	-   LPAD()，按照限定长度从左到右截取字符串，当字符串的长度小于限定长度时在左侧填充指定的字符
	-   RPAD()，按照限定长度从左到右截取字符串，当字符串的长度小于限定长度时在右侧填充指定的字符
	-   INSTR()，返回子串在一个字符串中第一次出现的位置。与LOCATE()和POSITION(... IN ...)函数等价，但参数不同
	-   FIND_IN_SET()，获取子串在含有英文都好分隔的字符串中开始的位置
	-   LTRIM()，删除字符串左边的空格，并返回处理后的结果
	-   RTRIM()，删除字符串右边的空格，并返回处理后的结果
	-   TRIM()，删除字符串两边的空格，并返回处理后的结果
	-   INSERT()，从字符串的指定位置开始使用子串替换指定长度的字符串
	-   REPLACE()，使用指定子串替换字符串中出现的所有指定字符
	-   CONCAT()，将参数连接成一个新的字符串
	-   CONCAT_WS()，使用指定分割符将参数连接成一个新字符串
*   日期和时间函数
	-   NOW()，获取当前系统日期和时间（当前语句执行时的时间）
	-   SYSDATE()，获取当前系统日期和时间（动态获取当前系统日期和时间）
	-   CURRENT_TIMESTAMP()，获取当前系统日期和时间
	-   UNIX_TIMESTAMP()，用于获取mysql服务器当前的UNIX时间戳
	-   UNIX_TIMESTAMP(date)，将指定日期date时间转为UNIX时间戳并返回
	-   CURRENT_DATE()，获取当前系统日期，CURDATE()
	-   CURRENT_TIME()，获取当前系统时间，CURTIME()
	-   DATEDIFF()，判断两个日期之间的天数差距，参数日期格式必须使用字符串形式
	-   DATE()，获取日期或日期时间的日期部分
	-   TIME()，获取日期或日期时间的时间部分
	-   WEEK()，返回指定日期的周数
	-   DAYNAME()，返回日期对应的星期几（英文全称）
	-   DAYOFMONTH()，返回日期对应的当月中的第几号（用数字表示），等价于DAY()
	-   DAYOFYEAR()，返回日期对应的当年中的第几天（用数字表示）
	-   DAYOFWEEK()，返回日期对用的星期几（用数字类型表示，1-周日，...，7-周六）
	-   FROM_UNIXTIME()，将指定的时间戳转成对应的日期时间格式
	-   EXTRACT()，按照指定参数提取日期中的数据
	-   DATE_SUB()，从日期中减去时间指（时间间隔）
	-   DATE_ADD()，在指定的日期中添加上日期时间
*   USER()，获取当前客户端提供的用户和主机地址
*   CURRENT_USER()，获取当前通过mysql服务器验证的用户和用户地址
*   PASSWORD()，使用当前用户的加密插件为用户设置密码
*   CONNECTION_ID()，获取当前mysql服务器连接id
*   BENCHMARK()，重复执行一个表达式
*   LAST_INSERT_ID()，获取当前会话中最后一个插入的AUTO_INCREMENT列的值
*   JSON函数