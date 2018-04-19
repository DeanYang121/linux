

	Student(S#,Sname,Sage,Ssex) 学生表
	Course(C#,Cname,T#) 课程表
	SC(S#,C#,score) 成绩表
	Teacher(T#,Tname) 教师表

2、查询平均成绩大于60分的同学的学号和平均成绩；

	select S#,avg(score) from sc group by S# having avg(score) >60; 
1、查询“001”课程比“002”课程成绩高的所有学生的学号；
	
	select a.S#
	from (select s#,score from SC where C#=’001′) a,
	(select s#,score from SC where C#=’002′) b
	where a.score>b.score and a.s#=b.s#; 

3、查询所有同学的学号、姓名、选课数、总成绩；

	select Student.S#,Student.Sname,count(SC.C#),sum(score) from Student left Outer join SC on Student.S#=SC.S# group by Student.S#,Sname 

4、查询姓“李”的老师的个数；

	select count(distinct(Tname))
	from Teacher
	where Tname like ‘李%’; 、

5、查询没学过“叶平”老师课的同学的学号、姓名；

	select Student.S#,Student.Sname
	from Student
	where S# not in (select distinct( SC.S#) from SC,Course,Teacher where SC.C#=Course.C# and Teacher.T#=Course.T# and Teacher.Tname=’叶平’);


6、查询学过“001”并且也学过编号“002”课程的同学的学号、姓名；

	 select Student.S#,Student.Sname
	 from Student,SC
	 where Student.S#=SC.S# and SC.C#=’001′and exists( Select * from SC as SC_2 where SC_2.S#=SC.S# and SC_2.C#=’002′);
		
7、查询学过“叶平”老师所教的所有课的同学的学号、姓名；

	select S#,Sname
	from Student
	where S# in
	(select S#
	from SC ,Course ,Teacher
	where SC.C#=Course.C# and Teacher.T#=Course.T# and Teacher.Tname=’叶平’ group by S# having count(SC.C#)=(select count(C#) from Course,Teacher where Teacher.T#=Course.T# and Tname=’叶平’)); 

8、查询所有课程成绩小于60分的同学的学号、姓名；

	select S#,Sname
	from Student
	where S# not in (select Student.S# from Student,SC where S.S#=SC.S# and score>60); 

9、查询没有学全所有课的同学的学号、姓名；

	select Student.S#,Student.Sname
	from Student,SC
	where Student.S#=SC.S#
	group by Student.S#,Student.Sname having count(C#) <(select count(C#) from Course); 

10、查询至少有一门课与学号为“1001”的同学所学相同的同学的学号和姓名；

	select S#,Sname
	from Student,SC
	where Student.S#=SC.S# and C# in （select C# from SC where S#='1001'）;


11、删除学习“叶平”老师课的SC表记录；

	Delect SC
	from course ,Teacher
	where Course.C#=SC.C# and Course.T#= Teacher.T# and Tname='叶平'; 

12、查询各科成绩最高和最低的分：以如下形式显示：课程ID，最高分，最低分

	SELECT L.C# 课程ID,L.score 最高分,R.score 最低分
	FROM SC L ,SC R
	WHERE L.C# = R.C#
	and
	L.score = (SELECT MAX(IL.score)
	FROM SC IL,Student IM
	WHERE IL.C# = L.C# and IM.S#=IL.S#
	GROUP BY IL.C#)
	and
	R.Score = (SELECT MIN(IR.score)
	FROM SC IR
	WHERE IR.C# = R.C#
	GROUP BY IR.C# ); 

13、查询学生平均成绩及其名次
	
	SELECT 1+(SELECT COUNT( distinct 平均成绩)
	FROM (SELECT S#,AVG(score) 平均成绩
	FROM SC
	GROUP BY S# ) T1
	WHERE 平均成绩 > T2.平均成绩) 名次, S# 学生学号,平均成绩
	FROM (SELECT S#,AVG(score) 平均成绩 FROM SC GROUP BY S# ) T2
	ORDER BY 平均成绩 desc;

15、查询每门功成绩最好的前两名
	
	SELECT t1.S# as 学生ID,t1.C# as 课程ID,Score as 分数
	FROM SC t1
	WHERE score IN (SELECT TOP 2 score
	FROM SC
	WHERE t1.C#= C#
	ORDER BY score DESC )
	ORDER BY t1.C#;

在面试过程中多次碰到一道SQL查询的题目，查询A(ID,Name)表中第31至40条记录，ID作为主键可能是不是连续增长的列，完整的查询语句如下：

	方法一：
	select top 10 *
	from A
	where ID >(select max(ID) from (select top 30 ID from A order by ID ) T) order by ID
	方法二：
	select top 10 *
	from A
	where ID not In (select top 30 ID from A order by ID)
	order by ID

mysql> SELECT * FROM table LIMIT 5,10; //检索记录行6-15 

mysql> SELECT * FROM table LIMIT 95,-1; // 检索记录行 96-last.

 mysql> SELECT * FROM table LIMIT 5;     //检索前 5 个记录行

3. 解释存储过程和触发器
	答案：
	存储过程是一组Transact-SQL语句，在一次编译后可以执行多次。因为不必重新编译Transact-SQL语句，所以执行存储过程可以提高性能。
	触发器是一种特殊类型的存储过程，不由用户直接调用。创建触发器时会对其进行定义，以便在对特定表或列作特定类型的数据修改时执行。

5. 数据库日志干什么用，数据库日志满的时候再查询数据库时会出现什么情况。
	答案：每个数据库都有事务日志，用以记录所有事务和每个事务对数据库所做的修改。

6. 存储过程和函数的区别?
	答案：存储过程是用户定义的一系列SQL语句的集合，涉及特定表或其它对象的任务，用户可以调用存储过程，而函数通常是数据库已定义的方法，它接收参数并返回某种类型的值并且不涉及特定用户表

7. 事务是什么?
	答案：事务是作为一个逻辑单元执行的一系列操作，一个逻辑工作单元必须有四个属性，称为 ACID（原子性、一致性、隔离性和持久性）属性，只有这样才能成为一个事务：
	(1)原子性
	事务必须是原子工作单元；对于其数据修改，要么全都执行，要么全都不执行。
	(2) 一致性
	事务在完成时，必须使所有的数据都保持一致状态。在相关数据库中，所有规则都必须应用于事务的修改，以保持所有数据的完整性。事务结束时，所有的内部数据结构（如 B 树索引或双向链表）都必须是正确的。
	(3) 隔离性
	由并发事务所作的修改必须与任何其它并发事务所作的修改隔离。事务查看数据时数据所处的状态，要么是另一并发事务修改它之前的状态，要么是另一事务修改它之后的状态，事务不会查看中间状态的数据。这称为可串行性，因为它能够重新装载起始数据，并且重播一系列事务，以使数据结束时的状态与原始事务执行的状态相同。
	(4) 持久性
	事务完成之后，它对于系统的影响是永久性的。该修改即使出现系统故障也将一直保持。

11. 提高数据库运行效率的办法有哪些?
	答案：在给定的系统硬件和系统软件条件下，提高数据库系统的运行效率的办法是：
	(1) 在数据库物理设计时，降低范式，增加冗余, 少用触发器, 多用存储过程。
	(2) 当计算非常复杂、而且记录条数非常巨大时(例如一千万条)，复杂计算要先在数据库外面，以文件系统方式用C++语言计算处理完成之后，最后才入库追加到表中去。这是电信计费系统设计的经验。
	(3) 发现某个表的记录太多，例如超过一千万条，则要对该表进行水平分割。水平分割的做法是，以该表主键PK的某个值为界线，将该表的记录水平分割为两个表。若发现某个表的字段太多，例如超过八十个，则垂直分割该表，将原来的一个表分解为两个表。
	(4) 对数据库管理系统DBMS进行系统优化，即优化各种系统参数，如缓冲区个数。
	(5) 在使用面向数据的SQL语言进行程序设计时，尽量采取优化算法。总之，要提高数据库的运行效率，必须从数据库系统级优化、数据库设计级优化、程序实现级优化，这三个层次上同时下功夫。

12. 通俗地理解三个范式

	答案：通俗地理解三个范式，对于数据库设计大有好处。在数据库设计中，为了更好地应用三个范式，就必须通俗地理解三个范式(通俗地理解是够用的理解，并不是最科学最准确的理解)：
	第一范式：1NF是对属性的原子性约束，要求属性具有原子性，不可再分解； 第二范式：2NF是对记录的惟一性约束，要求记录有惟一标识，即实体的惟
	一性；
	第三范式：3NF是对字段冗余性的约束，即任何字段不能由其他字段派生出来，它要求字段没有冗余。没有冗余的数据库设计可以做到。但是，没有冗余的数据库未必是最好的数据库，有时为了提高运行效率，就必须降低范式标准，适当保留冗余数据。具体做法是：在概念数据模型设计时遵守第三范式，降低范式标准的工作放到物理数据模型设计时考虑。降低范式就是增加字段，允许冗余。

13. 简述存储过程的优缺点

	优点：
	1. 更快的执行速度：存储过程只在创造时进行编译，以后每次执行存储过程都不需再重新编译，而一般SQL语句每执行一次就编译一次,所以使用存储过程可提高数据库执行速度；
	2. 与事务的结合，提供更好的解决方案：当对数据库进行复杂操作时(如对多个表进行Update、Insert、Query和Delete时），可将此复杂操作用存储过程封装起来与数据库提供的事务处理结合一起使用；
	3. 支持代码重用：存储过程可以重复使用,可减少数据库开发人员的工作量；4. 安全性高：可设定只有某此用户才具有对指定存储过程的使用权。
	
	缺点：
	1. 如果更改范围大到需要对输入存储过程的参数进行更改，或者要更改由其返回的数据，则您仍需要更新程序集中的代码以添加参数、更新 GetValue() 调用，等等，这时候估计比较繁琐了。
	2. 可移植性差由于存储过程将应用程序绑定到 SQL Server，因此使用存储过程封装业务逻辑将限制应用程序的可移植性。如果应用程序的可移植性在您的环境中非常重要，则将业务逻辑封装在不特定于 RDBMS 的中间层中可能是一个更佳的选择。

14. 主键和唯一索引有什么区别?

	答案：
	相同点:它们都属于实体完整性约束。
	不同点:
	(1) 唯一性约束所在的列允许空值，但是主键约束所在的列不允许空值。
	(2) 可以把唯一性约束放在一个或者多个列上，这些列或列的组合必须有唯一的。但是，唯一性约束所在的列并不是表的主键列。
	(3) 唯一性约束强制在指定的列上创建一个唯一性索引。在默认情况下，创建唯一性的非聚簇索引，但是，也可以指定所创建的索引是聚簇索引。
	(4) 建立主键的目的是让外键来引用。
	(5) 一个表最多只有一个主键，但可以有很多唯一键。

15. 简述索引存取的方法的作用和建立索引的原则

	作用：加快查询速度。
	原则：
	(1) 如果某属性或属性组经常出现在查询条件中，考虑为该属性或属性组建立索引；
	(2) 如果某个属性常作为最大值和最小值等聚集函数的参数，考虑为该属性建立索引；
	(3) 如果某属性经常出现在连接操作的连接条件中，考虑为该属性或属性组建立索引；

18. 什么是基本表?什么是视图?

	答案：基本表是本身独立存在的表，在 SQL 中一个关系就对应一个表。
	视图是从一个或几个基本表导出的表。视图本身不独立存储在数据库中，是一个虚表

如何提高MySql的安全性

	1.如果MYSQL客户端和服务器端的连接需要跨越并通过不可信任的网络，那么需要使用ssh隧道来加密该连接的通信。

	2.使用set password语句来修改用户的密码，先“mysql -u root”登陆数据库系统，然后“mysql> update mysql.user set password=password(‘newpwd’)”，最后执行“flush privileges”就可以了。

	3.Mysql需要提防的攻击有，防偷听、篡改、回放、拒绝服务等，不涉及可用性和容错方面。对所有的连接、查询、其他操作使用基于acl即访问控制列表的安全措施来完成。也有一些对ssl连接的支持。

	4.设置除了root用户外的其他任何用户不允许访问mysql主数据库中的user表;

	5.使用grant和revoke语句来进行用户访问控制的工作;

	6.不要使用明文密码，而是使用md5()和sha1()等单向的哈系函数来设置密码;

	7.不要选用字典中的字来做密码;

	8.采用防火墙可以去掉50%的外部危险，让数据库系统躲在防火墙后面工作，或放置在dmz区域中;

	9.从因特网上用nmap来扫描3306端口，也可用telnet server_host 3306的方法测试，不允许从非信任网络中访问数据库服务器的3306号tcp端口，需要在防火墙或路由器上做设定;

	10.为了防止被恶意传入非法参数，例如where id=234，别人却输入where id=234 or 1=1导致全部显示，所以在web的表单中使用”或”"来用字符串，在动态url中加入%22代表双引号、%23代表井号、%27代表单引号;传递未检查过的值给mysql数据库是非常危险的;

	11.在传递数据给mysql时检查一下大小;

	12.应用程序需要连接到数据库应该使用一般的用户帐号，开放少数必要的权限给该用户;

	14.学会使用tcpdump和strings工具来查看传输数据的安全性，例如tcpdump -l -i eth0 -w -src or dst port 3306 strings。以普通用户来启动mysql数据库服务;

	15.不使用到表的联结符号，选用的参数 –skip-symbolic-links;

	16.确信在mysql目录中只有启动数据库服务的用户才可以对文件有读和写的权限;

	19.如果不相信dns服务公司的服务，可以在主机名称允许表中只设置ip数字地址;

	20.使用max_user_connections变量来使mysqld服务进程，对一个指定帐户限定连接数;

为什么group by 和order by会使查询变慢

	group by 和 order by操作通常需要创建一个临时表来处理查询的结果，所以如果查询结果很多的话会严重影响性能。

数据库的约束含义

	数据库约束是防止非法记录的规则， 约束保存在数据字典(data dictionary)中， 约束可以被定义在列级或者表级。
	Oracle中包括一下集中约束：
	1. Not Null – 明确一列数据不能包含null值
	2. Unique – 强制所有数据行不能有重复值
	3. Primary Key – 每一行数据的唯一标示
	4. Foreign Key – 强制一列数据与引用表的外键约束关系
	5. Check – 检查，明确规定一个必须为true的condition


ddl,dml,dcl的含义：

	DDL :数据定义语言，用于定义和管理 SQL 数据库中的所有对象的语言
	1.CREATE – to create objects in the database 创建数据库对象
	2.ALTER – alters the structure of the database 修改数据库对象
	3.DROP – delete objects from the database 删除数据库对象
	4.TRUNCATE – remove all records from a table, including all spaces allocated for the records are removed

	Truncate table 表名 速度快,而且效率高,因为:
	TRUNCATE TABLE 在功能上与不带 WHERE 子句的 DELETE 语句相同：二者均删除表中的全部行。但 TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少。

	DELETE 语句每次删除一行，并在事务日志中为所删除的每行记录一项。TRUNCATE TABLE 通过释放存储表数据所用的数据页来删除数据，并且只在事务日志中记录页的释放。

	TRUNCATE TABLE 删除表中的所有行，但表结构及其列、约束、索引等保持不变。新行标识所用的计数值重置为该列的种子。如果想保留标识计数值，请改用 DELETE。如果要删除表定义及其数据，请使用 DROP TABLE 语句。

	对于由 FOREIGN KEY 约束引用的表，不能使用 TRUNCATE TABLE，而应使用不带 WHERE 子句的 DELETE 语句。由于 TRUNCATE TABLE 不记录在日志中，所以它不能激活触发器。
	
	TRUNCATE TABLE 不能用于参与了索引视图的表。

	5.COMMENT – add comments to the data dictionary 注释

	6.GRANT – gives user’s access privileges to database 授权

	7.REVOKE – withdraw access privileges given with the GRANT command 收回已经授予的权限

	DML:数据操作语言，SQL中处理数据等操作统称为数据操纵语言：
	
	1.SELECT – retrieve data from the a database 查询数据
	2.INSERT – insert data into a table 添加数据
	3.UPDATE – updates existing data within a table 更新数据
	4.DELETE – deletes all records from a table, the space for the records remain 删除
	5.CALL – call a PL/SQL or Java subprogram
	6.EXPLAIN PLAN – explain access path to data
	Oracle RDBMS执行每一条SQL语句，都必须经过Oracle优化器的评估。所以，了解优化器是如何选择(搜索)路径以及索引是如何被使用的，对优化SQL语句有很大的帮助。Explain可以用来迅速方便地查出对于给定SQL语句中的查询数据是如何得到的即搜索路径(我们通常称为Access Path)。从而使我们选择最优的查询方式达到最大的优化效果。
	7.LOCK TABLE – control concurrency 锁，用于控制并发

	DCL:数据控制语言，用来授予或回收访问数据库的某种特权，并控制数据库操纵事务发生的时间及效果，对数据库实行监视等
	COMMIT – save work done 提交
	SAVEPOINT – identify a point in a transaction to which you can later roll back 保存点
	ROLLBACK – restore database to original since the last COMMIT 回滚
	SET TRANSACTION – Change transaction options like what rollback segment to use 设置当前事务的特性，它对后面的事务没有影响．

介绍一下SQL中union,intersect和minus：

	Union用来返回多个查询的结果的总和去掉重复的结果

	语法：
	SELECT column1, column2 FROM tablename1
	UNION
	SELECT column1, column2 FROM tablename2;	

	Intersect 用来返回多个查询中共同的结果，intersect会忽略null值
	
	语法：
	
	SELECT column1, column2 FROM tablename1
	INTERSECT
	SELECT column1, column2 FROM tablename2;
	
	MUNUS返回出现在第一个查询结果中但是不出现在第二个查询结果的结果集。

	语法：
	
	SELECT column1, column2 FROM tablename1
	MINUS
	SELECT column1, column2 FROM tablename2;

Oracle中delete,truncate和drop的区别：

	Delete命令用来删除表的全部或者一部分数据行，执行delete之后，用户需要提交(commmit)或者回滚(rollback) transaction 来执行删除或者撤销删除， delete命令会触发这个表上所有的delete触发器。
	Truncate删除表中的所有数据， 这个操作不能回滚，也不会触发这个表上的触发器，TRUNCATE比delete更快，占用的空间更小。
	Drop命令从数据库中删除表， 所有的数据行，索引和权限也会被删除，所有的DML触发器也不会被触发，这个命令也不能回滚。

rowid和rownum有什么不同：

	RowId是一个数据库内部的概念，表示表的一行，用来快速的访问某行数据
	Rownum是结果集的一个功能， 例如select * from Student where rownum = 2 就是得到结果集的第二行。

视图的作用：

	数据库视图的作用只要有：
	1. 数据库视图隐藏了数据的复杂性。
	2. 数据库视图有利于控制用户对表中某些列的访问。
	3. 数据库视图使用户查询变得简单。
	视图是一个虚拟表，其内容由查询定义。同真实的表一样，视图包含一系列带有名称的列和行数据。但是，视图并不在数据库中以存储的数据值集形式存在。行和列数据来自由定义视图的查询所引用的表，并且在引用视图时动态生成。
	对其中所引用的基础表来说，视图的作用类似于筛选。定义视图的筛选可以来自当前或其它数据库的一个或多个表，或者其它视图。分布式查询也可用于定义使用多个异类源数据的视图。如果有几台不同的服务器分别存储组织中不同地区的数据，而您需要将这些服务器上相似结构的数据组合起来，这种方式就很有用。
	通过视图进行查询没有任何限制，通过它们进行数据修改时的限制也很少。

2、 谈谈你对索引的理解?
	索引是若干数据行的关键字的列表，查询数据时，通过索引中的关键字可以快速定位到要访问的记录所在的数据块，从而大大减少读取数据块的I/O次数，因此可以显著提高性能。
3、 说说索引的组成?
	索引列、rowid

