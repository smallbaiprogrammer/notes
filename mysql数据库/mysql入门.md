`Mysql`数据库的命令

// 数据库的基本操作

dos 窗口进入数据库

`mysql -uroot -p`

```mysql
#查看当前数据库下面的所有数据
show databases;
#创建数据库
create database mydb1;
#创建数据库并设置编码形式
create datebase mydb2 character set gbk;
#给如果数据库名字不存在，才会创建该数据库
create database if not exists mydb3;
#查看当前数据库信息
show create database mydb2;
#修改数据库的信息
alter database mydb2 character set utf8;
#删除数据库
drop database mydb2;
#查看当前所使用的数据
select database();
#改变当前操作的位置
use mydb1;

```

数据查询

返回一张虚拟表



```mysql
#查询语法 一个表中的部分列
#select 列名 from 表明
#查询一个表中的所有列
select * from my;
#查询所有员工的年薪
select 列名 as '列明的别名'
#查询去重
distinct 列名
# 必须放在第一行 
select distinct 列名 from 表名；
#排序查询
select 列明 from 表名 order by Desc # 降序排序 DESC 升序排序  ASC
# 不会影响原表的数据
# 可以根据多列名进行排序
select 列名，列名 from 表明 order by 列名 DESC ，列名 desc ;
# 条件查询
# 查询符合条件的数据
SELECT 列名 from 表名 where 条件 
# 逻辑运算是 or and not 
# 不等于 ！= 和 <>  ,二者意义是意义的
# 区间判断 
# 前面小，后面大
 列名 between 6000 and 10000;
 select id,title,apply_date from test where id between 10 and 50;
# null 值判断 空的 
# 查询这个值为空的数据
# 枚举查询,效率偏低
select t_employees WHERE department_id  IN(70,80,90);
select id,title,apply_date from test where id in(1,2,3);
# 模糊查询
# L 开头的员工信息
SELECT EMPLOYEE_EE,FIRST_NAME,salary
FROM t_employees
WHERE FIRST_NAME LIKE 'L%'
# % 百分号代表占位符，'L%'表示为首字母为L的字符串 %代表 1个到N个
'L_'
# _ 代表一个

# 分支结构查询
SELECT EMPLOYEE_ID,FIRST_NAME,salary,
CASE 
	WHEN 条件一 THEN 结果1
	WHEN 条件二 THEN 结果2
	WHEN 条件三 THEN 结果3
	ELSE 结果 4 
END AS '想要的列'
FROM t_employees;

select id,title,apply_date ,case 
 			when id > 20 then 'little'
 			else 'else'
 		end as 's'
 		from test;
# 时间查询
SELECT 时间函数 
SELECT SYSDATE(); # 当前系统时间 年月日 时分秒 
SELECT CURDATE(); # 当前系统时间 年月日
SELECT CURTIME(); # 当前系统时间 时分秒
SELECT WEEK(DATE); # 指定日期为该年的第几周
SELECT YEAR(DATE); # 指定日期的年
SELECT HOUR(TIME);
SELECT MINUTE(TIME);
SELECT DATEDIFF(DATE1,DATE2); # 两个日期相差的天数
SELECT ADDDATE(DATE,N);  # 指定日期date 加上 N天后的日期 

# 字符串查询 
# 多个字符串做拼接 CONCAT(FIRST_NAME,LAST_NAME) 
SELECT CONCAT(FIRST_NAME,LAST_NAME) AS '姓名' FROM t_employees;
select concat('第',id,'个') as '编号',title , apply_date from test where id < 30 order by id desc;
INSERT(str,pos,len,newStr);# 将字符串str中pos开始len位换成newStr
select id , insert(title,2,2,'--') as '编号' ,apply_date from test where id < 30;
LOWER(str); # 将字符串转换成小写
SELECT LOWER('SSSSSS');
UPPER(str); # 将字符串转换位大写
SELECT UPPER('ssssss');
SUBSTRING(str,start,len); # 字符串截取，将str字符串中start开始长度为len位的字符串截下来
select id,substring(title,2,3),apply_date from test where id < 30;
# 字符串中的下标都1开始的

```

 聚合函数

```mysql
#聚会函数
对列进行操作
# 队列中的数据进行操作
sum();
select sum(id) from test where id < 30;
avg();
select avg(id) from test where id < 30;
min();
select min(id),title from test;
max();
select max(id),title from test;
# 求列中有多少行数据
# 计数
count();
# 记行数
# 会自动忽略null值

select 
    CLASS 
    FROM 
    (select 
    CLASS , COUNT(DISTINCT STUDENT) 
    AS NUM 
    FROM COURSES 
    GROUP BY CLASS) AS TEMP 
    WHERE NUM >= 5;
```



分组查询

```mysql
SELECT 列名 FROM 表明 WHERE 条件 GROUP BY 分组依据（列）
# 这些结合起来用
```

限制查询

```mysql
# 列是从第0 行开始的

SELECT * FROM TEST LIMIT 0,5 ;

```

数据库不难，难的是Java和数据库的连接，所有出现了spring boot 框架

SQL语句的整体编写顺序

```MYSQL
SELECT 'lieming' from 'biaoming' where 'tiaojian' group by 分组查询 Having 过滤条件 ORDER BY 列名 DESC LIMIT 起始行总条数
# having 一定是在 分组之后，是对组进行分的
```

```MYSQL
SELECT ID,TITLE,APPLY_DATE,APPLICANT_CH AS '目录' from test where id < 30 order by id desc limit 1,5;

select id,title,apply_date,options_start,count(options_start) as 'num' from test group by options_star;
select id,title,options_start,count(options_start) as 'num' from test group by options_start having count(options_start) > 2;
```

执行顺序

```mysql
1. from  表来源
2. where  条件 第一次过滤
3. group by 分组
4. having 过滤 第二次过滤
5. SELECT 查询各字符端的值
6. ORDER BY 排序
7. LIMIT 限制查询结果
# having 一般和 函数一起使用
```



子查询  条件判断

```MYSQL
SELECT 'Liming' from '表名' where 
```

```mysql
# 子查询 
# 有括号先算括号里面的
# 子查询作为条件查询

mysql> select * from test where id >= (select max(id) from test);
# 子查询为一个数据的时候
select id,title from test where id = (select max(id) from test);
# 若子查询结果很多，则可以作为枚举查询的条件
# 子查询有很多数据的时候

# 同时也可以采用ALL 关键字 
mysql> SELECT * FROM TEST WHERE id > all(select id from test Where title = '正负零 0 ');
大于 最大值
# 或者 ANY
大于 最小值 
SELECT * FROM TEST WHERE id > any(select id from test Where title = '正负零 0 ');
# 多行多列时，可以采用查询结果作为目标路径
 select id,title,apply_date from (select * from TEST WHERE MOD(ID,2) = 1) as temp WHERE ID = 1; 
 # 子查询表必须要有一个临时的名字
```

合并查询

```mysql
# 合并查询
# 将两张表的查询出来的结果合并在一起
# 要求列数要求必须相同，表明可以不同
# 后者的列名会将被前面的列名给吞了
SELECT * FROM TEST UNION SELECT * FROM TEST;

# 合并 
# 保留重复的数据
SELECT * FROM T1 UNION ALL SELECT * FROM T2;
```

表连接查询

将多个表查询结果连接在一起，连接方式

```mysql
1.内连接查询
# 两个表查询
select * from '表一' inner join '表二' on '内连接条件'
select * from t1 inner join t2 on t1.id = t2.id;
select * from t1,t2 where t1.id = t2.id;
#多个表查询
select * from t1,t2,t3 where t1.id = t2.id and t1.id = t3.id;
select * from t1 inner join t2 on t1.id = t2.id inner join t3 on t2.id = t3.id;
select t1.id,t2.name,t3.nianjie from t1 inner join t2 on t1.id = t2.id inner join t3 on t3.id = t1.id; 
```

左外连接查询

右外连接查询

```mysql
# 左外连接查询
# 左外连接以左表为主表，查不到信息，以null值替代
select * from t1 left join t2 on t1.id = t2.id;
# 右外连接以右表为主表
select * from t1 right join t2 on t1.id = t2.id;
```





数据库中的`DML`操作

```MYSQL
# 数据库新增数据
INSERT INTO 表名（列1，列2，列3.。。。） values(值1，值2，值3.。。。)
insert into t2(id,name) values(7,'bb');
# 修改数据库中的值
UPDATE 表名 set 列1 = 新值 1 ，列2 = 新值2 where 条件；
update t2 set name = 'ssbb' where id = 7;
# 删除数据操作
delete from 表名 where 条件
delete from t2 where id = 7;
# 清空表，同时创建一个新表
TRUNCATE TABLE 表名；
```

数据表的操作

```MYSQL
# 数据类型 
# 三类 数值 日期/时间 和 字符串
# 数值类型
int 
double
double(m,d) # m 表示长度，d表示小数的范围，精度收到m和d的影响，m表示整个数值共多少位，d表示小数占几位
decimal(m,n) # 大数类，更精确

# 时间类型
DATE 1000-9999
TIME 0:00 - 23:59
YEAR 1901 - 2155
DATETIME 包含两个

# 字符串类型
char 定长字符串 10 个字符的长度
varchar 可变化字符 最大10个字符
blob 二进制长文本数据，二进制数据的存储
blob 又分四种长度
text 不限长度

```



数据表的创建

```mysql
CREATE TABLE 表名（列名 数据类型[约束]，。。。）
[character = utf8]
关键字冲突前面添加
·
```

数据表的修改

```mysql
ALTER TABLE 表名 操作
# 添加列
ALTER TABLE `SUBJECT` ADD GRADE INT;
# 修改表中的列 
alter table `subject` modify subject_name varchar(10);
# 删除表中的列
ALTER TABLE `SUBJECT` DROP ID;
# 修改表中的列名
ALTER TABLE `SUBJECT` CHANGE SUBJECT_NAME CLASSHOUST INT;
# 修改表名
ALTER TABLE `SUBJECT` RENAME SUB;
# 删除表名
DROP TABLE t1;
```

约束

```mysql
数据约束条件
1.实体完整性约束
（1）主键约束
# 表明该键不可重复 且 不可以为空 
PRIMARY KEY;
（2） 唯一约束
# 表明改键不能重复，但是可以为空
UNIQUE 
（3) 自动增长列
# 该列的值初始是1，然后自动增长的，
AUTO_INCREMENT
2.域完整性约束
限制单元格数据类型
（1) 非空约束
NOT NULL 
(2) 默认值约束 
default 值 ，赋予默认值 
3.引用完整性约束
CONSTRAINT 引用名 FOREIGN (列名) REFERENCES 被引用表明（列名）
# 一个表中的列的值必须是被引用表中的指定的列的值


```

事务

原子操作

事务中一个`SQL`语句执行失败，相当于全部是失败，全部`SQL`语句执行成功才表示成功，解决的并发问题

事务执行成功 称为提交

a.显示提交 commit 

b.隐士提交 正常退出，

事务执行失败 称为回滚

a.显示回滚 rollback

b.隐士回滚 非正常退出，回滚为语句执行开始之前的状态



原理就是每个客户端的事务在数据库中存在一个语句执行的缓存区，一个事务的全部执行完成之后，数据才会传给数据库，若事务出现问题，就会回滚到事务执行之前的状态

```mysql
#事务的特性  ACID
1.Atomicity 原子性
2.Consistency 一致性
3.Isolation 隔离性  语句执行过程中的结果，不会被其他语句获取
4.Durablity 持久性
```

```mysql
# %%%%%%
# 开启事务
START TRANSACTION;
# 执行的操作
UPDATE account set money = money - 1000 where id = 1;
UPDATE account set money = money + 1000 where id = 2;
# 正常执行结束 事务结束
COMMIT; 
# 非正常执行 
ROLLBACK;
```

权限管理

```mysql
# 创建用户
CREATE USER `张三` IDENTIFIED BY '123';
# 授权 
# 刷新就会得到权限
GRANT ALL ON COMPANY.* TO `张三`;
# 撤销权限
# 断开数据库才会生效
REMOVE ALL ON CAMPANY.* FROM `张三`；
# 删除用户
DROP USER `张三`;
```

视图

虚拟表，从一个表中或多个表中查询出来的表，作用和真是表一样，建立一个中间临时表，可以进行各自原表进行的操作

```mysql
# 创建一个视图 
CREATE VIEW TEMP AS SELECT * FROM TEST;
# 可以直接采用 temp名 就得到想要的表

# 视图的修改
CREATE OR REPLACE VIEW temp as 
alter view temp as 

# 视图的删除
DROP VIEW 名
# 没有优化任何查询性能
# 视图好多形式不能进行修改
```

`MYSQL` 数据库的运用分类

```mysql
1.数据查询语言  DQL  select where order by group by having
2.数据定义语言 DDL create alter drop 
3.数据操作语言 DML insert update delete
4.事务处理语言 TPL commit rollback
5.数据控制语言DCL grant revoke;
```

```mysql
# 创建用户表
CREATE TABLE USER(
		USERID INT PRIMARY KEY AUTO_INCREMENT,
    	USERNAME VARCHAR(20) NOT NULL,
    	PASSWORD VARCHAR(20) NOT NULL,
    	ADDRESS VARCHAR(100) NOT,
    	PHONE VARCHAR(11)
)CHARSET UTF8;

# 创建分类表
```

