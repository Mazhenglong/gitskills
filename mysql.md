# gitskills
#一.查询数据

1.基本查询  
假设表明是students，要查询students表的所有行，我们用如下SQL语句：
SELECT * FROM students;
格式：SELECT *FROM <表名>
使用SELECT * FROM students 时，SELECT是关键字，表示将要执行一个查询，
*表示”所有列“，FROM表示将要从哪个表查询，本例中是students表。

2.条件查询
SELECT 语句可以通过WHERE条件来设定查询条件，
例如，要指定条件“分数在80分以上的学生”, 如下查询语句
SELECT * FROM students WHERE score>=80 .
格式：SELECT * FROM <表名>  WHERE  score <条件表达式>
上述语句中，WHERE 关键词后面的 score>=80 就是条件。score是列名，该列存储了学生的成绩，因此，score>=80 就筛选了指定条件的记录。
——————————————————————————
例子：SELECT * FROM students WHERE score >= 80 AND gender = 'M';
上述列子是取score大于等于80并且gender字段等于M
SELECT * FROM students WHERE score >= 80 OR gender = 'M';
上述列子取score大于等于80或gender等于M
SELECT * FROM students WHERE NOT class_id = 2;
上述列子取class_id不等于2的值
SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender
 = 'M';
 上述列子取score小于80或大于90并且gender等于M的值

3.投影查询
如果我们只希望返回某些列的数据，而不是所有列的数据，我们可以用
格式：SELECT 列1，列2，列3 FROM <表名>
例如： 从students 表返回 id,score和name这三列：
SELECT id, score, name FROM students;
——————————————————————————
使用投影查询，并将列名重命名：下面是列子
SELECT id , score opints , name FROM students;
格式：SELECT id , score <重命名> , name FROM students;
——————————————————————————
使用投影查询+WHERE 条件：
SELECT id, score, name FROM students WHERE gender ="M"

4.排序
SELECT查询时，查询结果通常是按照id排序的，也就是根据主键排序的。这也是大部分数据库的做法，如果我们演根据其他条件拍讯怎么办？可以加上ORDER BY子句
例如按照成绩从低到高进行排序：
按score从低到高：
SELECT id,score,name FROM students ORDER BY score;
按score从高到底：
SELECT id,score,name FROM students ORDER BY score DESC;
——————————————————————————
如果score列有相同的数据，要进一步排序，可以继续添加列名。例如，使用ORDER BY score DESC, gender表示先按score列倒序，如果有相同分数的，再按gender列排序：添加先决条件排序
-- 按score, gender排序:
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
——————————————————————————
如果有WHERE子句，那么ORDER BY子句要放到WHERE子句后面。例如，查询一班的学生成绩，并按照倒序排序
-- 带WHERE条件的ORDER BY:
SELECT id, name, gender, score
FROM students WHERE class_id = 1 ORDER BY score DESC;

5.分页查询
如果查询数据量较大，页面显示全部数据不利于观察，采用分页方法将数据分成多页
--查询第一页 (将第一页显示几条语句使用 LIMIT <显示几条>  OFFSET <从第几条>)
SELECT id,name,gender,score  
FROM studnets
ORDER BY score DESC
LIMIT 3 OFFSET
上述查询 LIMIT 3 OFFSET 0 表示，对结果集从0号记录开始，最多取3条，注意SQL记录集的索引从0开始

6.聚合查询
如果我们想要查询一张表里有条多少数据，SQL提供了专门的聚合函数，
SELECT COUNT(*)FROM students;
上述事例只会返回一行一列数据，如果我们要设置结果集的列名
SELECT COUNT(*) NUM FROM students;
——————————————————————————
使用聚合查询并设置WHERE条件：
SELECT COUNT(*) NUM FROM students WHERE gender ="M";
——————————————————————————
除了COUNT()函数外，SQL还提供了如下聚合函数：

函数        说明
SUN         计算某一列的合计值，该列必须为数值类型
AVG         计算某一列的平均值，该列必须为数值类型
MAX         计算某一列的最大值，
MIN         计算某一列的最小值，
——————————————————————————
分组
SELECT COUNT(*) NUM FROM students GRUOP BY class_id
执行这个查询，COUNT()的结果不再是一个二十3个，这是因为，GRUOP BY句子指定了按class_id
分组，因此，执行该SELECT语句时，会把class_id相同的列先分组，再分别计算，因此会得到3个结果
按 class_id 分组：SELECT class_id, COUNT(*) NUM FROM students GRUOP BY class_id

7.多表查询
SELECT *FROM <表1>, <表2>
在两张表中有相同字段时，比如说查出来都有id和name,我们仍可以使用投影查询的“设置列的别名”
来给两个表各自的id和name列起别名：
SELECT  
      students.id sid,
      students.name,
      students.gender,
      students.score,
      classes.id cid,
      classes.name cname,
FROM students,classes;
——————————————————————————
我们还可以为表设置别名：如下
SELECT  
      s.id sid,
      s.name,
      s.gender,
      s.score,
      c.id cid,
      c.name cname,
FROM students s,classes c;
——————————————————————————
多表查询也可以设置WHERE条件：如下
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c
WHERE s.gender = 'M' AND c.id = 1;

8.链接查询 
 SELECT s.id, s.name, s.class_id, s.gender, s.score FROM students s;
 上述语句查询学生的这些信息
 如果想要classe表中的班级字段和学生表的s.class_id链接
 这就用上了链接查询
 --选出所有学生,同事返回班级名称(JOIN内连 INNER)
 SELECT s.id, s.name, s.class_id,s.gender,s.score FROM students s
 INNER JOIN classes c
 ON s.class_id =c.id
 注意上述句子只能返回两张表id列同时存在的数据

 注意INNER JOIN查询的写法是：

①先确定主表，仍然使用FROM <表1>的语法；
②再确定需要连接的表，使用INNER JOIN <表2>的语法；
③然后确定连接条件，使用ON <条件...>，这里的条件是s.class_id = c.id，表示students表的class_id列与classes表的id列相同的行需要连接；
④可选：加上WHERE子句、ORDER BY等子句
————————————————————————————————
--使用OUTER JOIN  
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
< RIGHT > OUTER JOIN classes c
ON s.class_id = c.id;
上述语句会将匹配表多余的信息也显示出来
————————————————————————————————
有RIGHT OUTER JOIN，就有LEFT OUTER JOIN，以及FULL OUTER JOIN。它们的区别是：

INNER JOIN只返回同时存在于两张表的行数据，由于students表的class_id包含1，2，3，classes表的id包含1，2，3，4，所以，INNER JOIN根据条件s.class_id = c.id返回的结果集仅包含1，2，3。

RIGHT OUTER JOIN返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以NULL填充剩下的字段。

LEFT OUTER JOIN则返回左表都存在的行。如果我们给students表增加一行，并添加class_id=5，由于classes表并不存在id=5的行，所以，LEFT OUTER JOIN的结果会增加一行，对应的class_name是NULL
#二.修改数据

关系数据库的基本操作就是增删改查，即CRUD：Create、Retrieve、Update、Delete
数据库的增、删、改，对应的SQL语句分别是：
INSERT：插入新记录；
UPDATE：更新已有记录；
DELETE：删除已有记录
————————————————————————————
插入新的记录 INSERT 基本语法是：
INSERT INTO <表名> (字段1,字段2，字段3,...) VALUES(值1，值2，值3，...) 
上述语句字段的顺序可以打乱，但是值必须与前面的字段顺序一致。
插入多条记录操作语句
INSERT INTO students (class_id,name,gender,score) VALUES
  (1,"大宝","M",87),
  (2,"二宝","G",85);
——————————————————————————————

更新数据记录 UPDATE基本语法是：
UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
UPDATE students SET name='大牛', score=66 WHERE id=1;
修改id为1学生的姓名和成绩并赋值
——————————————————————————————
UPDATE students SET name='小牛', score=77 WHERE id>=5 AND id<=7;
-- 查询并观察结果:
将id大于等于5小于等于7的成绩修改为77，姓名修改为小牛
______________________________
UPDATE students SET score=score+10 WHERE score<80;
将所有成绩小于80分的同学成绩加10分

DELETE 语句的基本语法是：
DELETE FROM <表名> WHERE...
--删除id=1的记录
DELETE FROM students WHERE id=1;
注意：特别小心的是不带WHERE条件DELETE语句会删除整个表的数据：
例如：DELETE FROM students;

#二.管理数据库

列出所有数据库,使用命令
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| shici              |
| sys                |
| test               |
| school             |
+--------------------+
其中，information_schema、mysql、performance_schema和sys是系统库，不要去改动它们。其他的是用户创建的数据库
————————————————————————————————————
要创建一个新数据库，使用命令：

mysql> CREATE DATABASE test;
Query OK, 1 row affected (0.01 sec)

要删除一个数据库，使用命令：

mysql> DROP DATABASE test;
Query OK, 0 rows affected (0.01 sec)

对一个数据库进行操作时，要首先将其切换为当前数据库：

mysql> USE test;
Database changed
表
列出当前数据库的所有表，使用命令：

mysql> SHOW TABLES;
+---------------------+
| Tables_in_test      |
+---------------------+
| classes             |
| statistics          |
| students            |
| students_of_class1  |
+---------------------+
要查看一个表的结构，使用命令：

mysql> DESC students;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | bigint(20)   | NO   | PRI | NULL    | auto_increment |
| class_id | bigint(20)   | NO   |     | NULL    |                |
| name     | varchar(100) | NO   |     | NULL    |                |
| gender   | varchar(1)   | NO   |     | NULL    |                |
| score    | int(11)      | NO   |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
还可以使用以下命令查看创建表的SQL语句：

mysql> SHOW CREATE TABLE students;
+----------+-------------------------------------------------------+
| students | CREATE TABLE `students` (                             |
|          |   `id` bigint(20) NOT NULL AUTO_INCREMENT,            |
|          |   `class_id` bigint(20) NOT NULL,                     |
|          |   `name` varchar(100) NOT NULL,                       |
|          |   `gender` varchar(1) NOT NULL,                       |
|          |   `score` int(11) NOT NULL,                           |
|          |   PRIMARY KEY (`id`)                                  |
|          | ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 |
+----------+-------------------------------------------------------+
1 row in set (0.00 sec)
创建表使用CREATE TABLE语句，而删除表使用DROP TABLE语句：

mysql> DROP TABLE students;
Query OK, 0 rows affected (0.01 sec)
修改表就比较复杂。如果要给students表新增一列birth，使用：

ALTER TABLE students ADD COLUMN birth VARCHAR(10) NOT NULL;
要修改birth列，例如把列名改为birthday，类型改为VARCHAR(20)：

ALTER TABLE students CHANGE COLUMN birth birthday VARCHAR(20) NOT NULL;
要删除列，使用：

ALTER TABLE students DROP COLUMN birthday;
退出MySQL
使用EXIT命令退出MySQL：

mysql> EXIT
Bye
注意EXIT仅仅断开了客户端和服务器的连接，MySQL服务器仍然继续运行
插入或替换
如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就先删除原记录，再插入新记录。此时，可以使用REPLACE语句，这样就不必先查询，再决定是否先删除再插入：

REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
若id=1的记录不存在，REPLACE语句将插入新记录，否则，当前id=1的记录将被删除，然后再插入新记录。

插入或更新
如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就更新该记录，此时，可以使用INSERT INTO ... ON DUPLICATE KEY UPDATE ...语句：

INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;
若id=1的记录不存在，INSERT语句将插入新记录，否则，当前id=1的记录将被更新，更新的字段由UPDATE指定。

插入或忽略
如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就啥事也不干直接忽略，此时，可以使用INSERT IGNORE INTO ...语句：

INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
若id=1的记录不存在，INSERT语句将插入新记录，否则，不执行任何操作。

快照
如果想要对一个表进行快照，即复制一份当前表的数据到一个新表，可以结合CREATE TABLE和SELECT：

-- 对class_id=1的记录进行快照，并存储为新表students_of_class1:
CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id=1;
新创建的表结构和SELECT使用的表结构完全一致。

写入查询结果集
如果查询结果集需要写入到表中，可以结合INSERT和SELECT，将SELECT语句的结果集直接插入到指定表中。

例如，创建一个统计成绩的表statistics，记录各班的平均成绩：

CREATE TABLE statistics (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    average DOUBLE NOT NULL,
    PRIMARY KEY (id)
);
然后，我们就可以用一条语句写入各班的平均成绩：

INSERT INTO statistics (class_id, average) SELECT class_id, AVG(score) FROM students GROUP BY class_id;
确保INSERT语句的列和SELECT语句的列能一一对应，就可以在statistics表中直接保存查询的结果：

> select * from statistics;
+----+----------+--------------+
| id | class_id | average      |
+----+----------+--------------+
|  1 |        1 |         86.5 |
|  2 |        2 | 73.666666666 |
|  3 |        3 | 88.333333333 |
+----+----------+--------------+
3 rows in set (0.00 sec)
