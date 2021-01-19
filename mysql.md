# 1. mysql简介

- MySQL是一个**关系型数据库管理系统**
- 由瑞典MySQL AB 公司开发，属于 [Oracle](https://baike.baidu.com/item/Oracle) 旗下产品
- Mysql是一个**开源免费的**关系型数据库管理系统
- mysql分为社区办和企业版

- 我们学习的是社区版

---

# 2. 关系型数据库

建立在关系模型基础上的数据库,借助数学中的集合代数等一些数学概念和方法,处理关系型数据库中的数据

---

# 3. Mysql的安装

- windows的安装
- Linux的安装

---

# 4. Mysql默认的字符集的配置(了解)

以windows中的mysql的配置为例:

- 找到mysql的存放配置文件的地方 `C:\ProgramData\MySQL\MySQL Server 5.6/my.ini`

- 修改

  ~~~shell
  [mysqld]
  character-set-server=utf8
  ~~~

> 但是: **我们完全不建议这样修改**,因为我们每个项目系统使用的数据库编码完全有可能不一样;

---

# 5. 启动和停止mysql的服务

~~~shell
net start mysql
net stop mysql
~~~

注意: 如果命令显式不可用代表操作系统的版本不支持(家庭版)

---

# 6. mysql的登录命令

~~~sql
mysql -u用户名  -p密码 
	-h: 默认为localhost
	-P: 默认3306
~~~

---

# 7. mysql常用的系统命令

~~~shell
#查看mysql的版本
SELECT VERSION();
#查看mysql当前登录的用户
select user();
#查看mysql的当前日期
select now();
#查看当前所在的数据库
select database();
~~~

---

# 8. mysql的语句规范

- 关键字和函数名建议大写
- 数据库的名称,表的名称,字段的名称建议小写
- 数据库 表名 字段名建议加上``
- sql语句的**定界符**默认以 ; 结尾

---

# 9. 数据库的操作SQL类型(了解)

- DDL(数据定义语言)

  ~~~shell
  CREATE TABLE/VIEW/INDEX
  ~~~

- DML(数据操纵语言)

  ~~~sql
  1) 插入：INSERT
  2) 更新：UPDATE
  3) 删除：DELETE
  ~~~

- DQL(数据查询语言)

  ~~~sql
  DQL基本结构是由SELECT子句，FROM子句，WHERE
  子句组成的查询块：
  SELECT <字段名表>
  	FROM <表或视图名>
  WHERE <查询条件>
  ~~~

  ---

# 10. mysql中的概念

- 数据库管理系统: 管理数据库的系统
- 数据库: 数组库是用来存放和组织表的
- 表:是存储数据的**容器**
- 记录: 一行的数据
- 属性: 一列属性值

----

# 11. 数据库的操作(必会)

- 创建数据库

~~~sql
CREATE DATABASE `db2`; #最简单的方式创建一个数据库
~~~

注意: 数据库的名称可以加 `` ,也可以不加,默认mysql会给你自动加上

~~~sql
CREATE DATABASE db2;  #可以省略``
~~~

- 带判断的创建数据库

~~~SQL
CREATE DATABASE IF NOT EXISTS `db1`; #如果不存在db1这个数据库则创建db1数据库,如果存在则不会创建但是也不会报错
~~~

- 创建数据库并且指定字符集

~~~sql
#mysql默认的字符集是latin1,latin1不支持中文
CREATE DATABASE IF NOT EXISTS `db3` DEFAULT CHARSET=UTF8;
~~~

- 删除数据库

~~~sql
#删除数据库,如果数据库不存在则会报错
DROP DATABASE `db1`;
#带判断的删除数据库
DROP DATABASE IF EXISTS `db1`
~~~

- 查询数据库

~~~sql
#查询数据库的创建信息
SHOW CREATE DATABASE `db3`;
#查询当前RDBMS中有哪些数据库
SHOW DATABASES;
#查询当前所在的数据库
SELECT DATABASE();
~~~

- 进入数据库

~~~sql
USE 'db1';
~~~

---

# 12. mysql中的数据类型

## 12.1 整形

|      类型       |                             范围                             |            备注            |
| :-------------: | :----------------------------------------------------------: | :------------------------: |
|   **tinyint**   |         **1个字节 范围(-128~127)\|\|0~255(无符号)**          | **与java中的byte类型对应** |
|  **smallint**   |     **2个字节 范围(-32768~32767\|\| 0 ~ 65535(无符号))**     |  **与java中的short对应**   |
|    mediumint    |    3 个字节, -8388608 to 8388607\|\|0 to 16777215(无符号)    |                            |
| **int:Integer** | 4 个字节, -2147483648 to 2147483647\|\|0 to 4294967295(无符号) |                            |
|   **bigint**    | 8 个字节, -9223372036854775808 to 922337203685477580\|\|0 to 18446744073709551615(无符号) |   **与java中的long对应**   |

---

## 12.2 浮点型

|    类型     |                       范围                       |        备注        |
| :---------: | :----------------------------------------------: | :----------------: |
| float(m,d)  | 单精度浮点型  8位精度(4字节)   m总个数，d小数位  | 与java的float对应  |
| double(m,d) | 双精度浮点型  16位精度(8字节)   m总个数，d小数位 | 与java的double对应 |

注意: 在实际开发过程中设计数据库时==一定一定一定==,**涉及到小数的不要使用FLOAT和DOUBLE类型**

---

## 12.3 定点

浮点型在数据库中存放的是近似值，而定点类型在数据库中存放的是精确值

|     类型     |                  范围                  |       备注       |
| :----------: | :------------------------------------: | :--------------: |
| decimal(m,d) | 参数m<65 是总个数，d<30且 d<m 是小数位 | 表示金额等精确值 |

注意: Decimal这个类型如果Insert的数据比我们预设的d的长度大,也会进行四舍五入;  一般存储小数都会使用DECIMAL类型,**不会丢失精度**

---

## 12.4 字符串

|      类型      |              范围              |                             备注                             |
| :------------: | :----------------------------: | :----------------------------------------------------------: |
|  **char(n)**   |    **固定长度，最多255个**     | 定长字符串,n 范围(0,255)， 如果不是定长的数据，n<=4 时才使用 |
| **varchar(n)** |        最多65532个字符         |    变长字符串，65532>n>4, 注意，n 是字符数，而不是字节数     |
|    tinytext    | 存储 L+1 个字节，其中 L < 2^8  |                                                              |
|    **text**    | 存储 L+2 个字节，其中 L < 2^16 |                          存储文本的                          |
|   mediumtext   | 存储 L+3 个字节，其中 L < 2^24 |                                                              |
|    longtext    | 存储 L+4 个字节，其中 L < 2^32 |                                                              |

## 12.5 Blob二进制类型

- BLOB和text存储方式不同，TEXT以文本方式存储，英文存储区分大小写，而Blob是以二进制方式存储，不分大小写。

- BLOB存储的数据只能整体读出。 

- TEXT可以指定字符集，BLOB不用指定字符集。

---

## 12.6 日期时间类型

|   类型    |             备注              |
| :-------: | :---------------------------: |
|   date    |       日期 '2008-12-2'        |
|   time    |        时间 '12:25:36'        |
| datetime  | 日期时间 '2008-12-2 22:06:44' |
| timestamp |     自动存储记录修改时间      |

---

# 13. mysql中数据类型属性(约束)

|  MySQL关键字   |           含义           |                     备注                      |
| :------------: | :----------------------: | :-------------------------------------------: |
|      NULL      |    数据列可包含NULL值    | mysql默认不指定约束,字段也不添加值,默认为NULL |
|    NOT NULL    |  数据列不允许包含NULL值  |                   非空约束                    |
|    DEFAULT     |          默认值          |                  默认值约束                   |
|  PRIMARY KEY   |           主键           |         主键约束 = 非空约束+唯一约束          |
| AUTO_INCREMENT | 自动递增，适用于整数类型 |    自增(一般和**数值类型的主键**联合使用)     |
|    UNSIGNED    |          无符号          |                 保留正数部分                  |
|     UNIQUE     |         唯一约束         |              此字段的值不能重复               |

---

# 14. 表的操作

数据表是数据库的最重要的组成部分之一,是其他对象的基础;

## 14.1 查看数据库中的所有的表

~~~sql
SHOW TABLES;
~~~

---

## 14.2 创建表

~~~sql
CREATE TABLE  `user`(id int,name char(32),age tinyint);
~~~

~~~sql
-- 带条件的创建表
CREATE TABLE  IF NOT EXISTS `user`(id int,name char(32),age tinyint);
~~~

~~~SQL
-- 创建表时指定字符集,如果不指定默认使用的是数据库的字符集
CREATE TABLE  IF NOT EXISTS `user`(id int,name char(32),age tinyint) DEFAULT CHARSET=UTF8;
~~~

~~~sql
-- 查看表结构
DESCRIBE `user`;
DESC `user`;  #简写
~~~

---

## 14.3 删除表

~~~sql
-- 删除表
DROP TABLE `user`;
~~~

~~~sql
-- 带判断的删除
DROP TABLE IF EXISTS `user`;
~~~

---

## 14.4 修改表结构

~~~sql
-- 添加列
ALTER TABLE `member` ADD COLUMN sex CHAR(1);
-- COLUMN关键字可以省略
ALTER TABLE `member` ADD  sex CHAR(1);  
~~~

~~~sql
-- 删除列	
ALTER TABLE member DROP COLUMN  city;
ALTER TABLE member DROP  city;
~~~

~~~sql
-- 查询列
DESC member;
~~~

~~~sql
-- MODIFY只能修改列的属性 不能修改名字
ALTER TABLE member MODIFY age SMALLINT;
-- CHANGE 既能修改列的名称也能修改列的属性
ALTER TABLE member CHANGE sex sex1 VARCHAR(10)
~~~

---

# 15. 记录(数据)的操作

## 15.1 添加数据

~~~sql
-- 给表插入一条数据
INSERT INTO member(id,nick_name,age,sex) VALUES(1,'小明',12,'男');
~~~

~~~sql
-- 批量插入数据
INSERT INTO member(id,nick_name,age,sex) VALUES(4,"小短腿",10,'男'),(5,'大胖',30,'男');
~~~

~~~sql
-- 骚操作(不指定字段全量插入)---->一般不建议使用全量插入数据
INSERT INTO member VALUES(NULL,'小明',20);
或者
INSERT INTO member VALUES(DEFAULT,'小明',20);
~~~

---

## 15.2 查询数据(简单查询)

~~~sql
-- 最简单的查询语句
SELECT * FROM member;
~~~

~~~sql
-- 查询指定的字段
SELECT nick_name FROM member;
~~~

---

## 15.3 修改数据

~~~sql
-- 修改数据
UPDATE member SET nick_name='小胖子' WHERE id=2;
~~~

---

## 15.4 删除数据

~~~sql
-- 删除数据
DELETE FROM member WHERE id=2;                               
~~~

---

# 16. 表和表之间关系

## 16.1 一对一的关系

例如: 一个会员表中的条记录只对应我们身份证表中的一条记录

- 我们如果设计数据库时出现了一对一的表应该尽量避免;我们只需要给字段多的一方的表**添加额外的字段即可**

- 如果我们设计的时候**无法避免(考虑的优化为题)**一对一的设计,我们需要让两个表的主键进行对应
- 如果一个表中的字段的个数超过16个,**强烈建议**使用一对一的表的设计

---

## 16.2 一对多的关系

例如: 一个会员对应多个订单,而一个订单只对应一个会员

- 在多的一方的表加一个字段对应一的一方的表中的主键,数据类型要保持一致
- 而且我们根据墨菲定律,还可以为其加一个约束条件(**外键(FOREIGN KEY)**)

~~~sql
-- 在表创建之后添加外键

-- 在多的一方的表中添加了一个字段
ALTER TABLE orders ADD COLUMN m_id INT ;

-- 添加外键约束
ALTER TABLE orders ADD FOREIGN KEY orders(m_id)  REFERENCES member(id);
ALTER TABLE orders ADD FOREIGN KEY(m_id)  REFERENCES member(id);

-- 可以指定外键的名称
ALTER TABLE orders ADD CONSTRAINT ifbk_orders_mid_merber FOREIGN KEY(m_id)  REFERENCES member(id);
~~~

~~~sql
-- 创建表的时候直接添加外键
CREATE TABLE IF NOT EXISTS orders(
	id INT  AUTO_INCREMENT COMMENT '订单ID',
	number VARCHAR(64) NOT NULL COMMENT '订单编号',
	address VARCHAR(255) NOT NULL COMMENT '订单的发货地址',
	m_id INT,	
	PRIMARY KEY(id),
	FOREIGN KEY(m_id) REFERENCES member(id)
) COMMENT '订单表';
~~~

**注意:**

- 创建一对多的表的时候,首先要创建一方对应的那个表
- 我们为了数据的安全性,我们会把多方中的参照的字段设置为外键,而且类型要和一方中的主键保持一致

- **一般我们在商业项目中,尽量不要使用外键(等会解释)**

---

## 16.3 多对多的关系

![image-20201122113930434](upload/image-20201122113930434.png)

> 思想: 借助一个额外的表,实现多对多关系

~~~sql
-- 创建商品表
CREATE TABLE IF NOT EXISTS shop(
	id INT AUTO_INCREMENT COMMENT '主键',
	shop_name VARCHAR(255) COMMENT '商品名称',
	stock INT UNSIGNED COMMENT '商品库存',
	PRIMARY KEY(id)
) COMMENT '商品表';


DROP TABLE IF EXISTS member_shop;
-- 创建第三方的表(member_shop)
CREATE TABLE IF NOT EXISTS  member_shop(
	m_id INT COMMENT '参照member中的id',
	s_id INT COMMENT '参照shop中的id',
	FOREIGN KEY(m_id) REFERENCES member(id),
	FOREIGN KEY(s_id) REFERENCES shop(id)
) ENGINE=INNODB DEFAULT CHARSET=utf8 COMMENT '第三方的表';
~~~

---

# 17. mysql的多字段查询

> select * 这种方式不建议使用,我们只查询需要的字段,select * 这种方式对性能有影响

~~~sql
-- 查询多个字段
SELECT nick_name,age FROM member;
~~~

~~~sql
-- 查询字段并且指定字段的别名
SELECT nick_name AS nname,age FROM member;
SELECT nick_name  nname,age FROM member;
~~~

~~~sql
-- 这才是一条完整的sql语句,我们在实际的开发中不会写这个多的东西,会省略一些东西(库名,字段的别名,表的别名...),sql的执行引擎会帮我们进行词法和语法的补全
SELECT db3.member.nick_name AS nick_name,db3.member.age AS age FROM db3.member AS member;
~~~

---

# 18. mysql中的排序

> ORDER BY col_name ASC|DESC

~~~sql
-- 不指定任何的排序字段的情况下,默认是按主键的升序排列的
SELECT id,nick_name,age FROM member;
~~~

~~~SQL
-- 指定按age来排序(默认是升序)
SELECT id,nick_name,age FROM member ORDER BY age ;
~~~

~~~sql
-- 指定按age的降序进行排序
SELECT id,nick_name,age FROM member ORDER BY age DESC ;
~~~

1)desc就是用于查询出结果时候对结果进行排序,是降序排序,而asc就是升序。要用与order by一起用。
		2）例如select * from student order by id desc; 就是把选出的结果经过“按id从大到小排序”后，把资源返回。
		3）还可以select * from student order by age desc,id desc;用“,”号隔开多个排序条件，这样，先按age 再按 id，就是说，先按age从大到小排序，如果有相同年龄的，那么相同年龄的学生再按他们的id从大到小排序。

group by是分组，是把group by后面带的列元素名内容相等的进行分组

---

# 19. mysql中的分组查询

~~~sql
-- 创建学生表
CREATE TABLE student(
    id INT AUTO_INCREMENT,
    sname VARCHAR(64),
    age TINYINT UNSIGNED,
    grade VARCHAR(32),
    PRIMARY KEY(id)
);
~~~

~~~sql
-- 查询grade字段不为NULL的总记录数
SELECT COUNT(grade) FROM student ;

-- 查询每个班级及其班级的总人数
SELECT grade, COUNT(grade) FROM student GROUP BY(grade);

-- HAING 是对分组后的数据进行条件过滤
SELECT grade, COUNT(grade) FROM student GROUP BY(grade) HAVING COUNT(grade)>1;
~~~

~~~sql
-- 查询年龄age>25的,班级及其班级里面的人数  WHERE 是对分组之前的数据进行筛选
SELECT grade,COUNT(grade) FROM student WHERE age>25 GROUP BY(grade) ;
~~~

~~~sql
-- 查询年龄age>25的,班级人数>1的,班级及其班级里面的人数
SELECT grade,COUNT(grade) FROM student WHERE age>25 GROUP BY(grade) HAVING COUNT(grade)>1 ;
~~~

---

# 20. AND和OR

~~~sql
-- and代表两个添加都成立
SELECT * FROM member  WHERE  age>25 AND sex='女';
~~~

~~~sql
-- or其中只要有一个成立则查出来
SELECT * FROM member  WHERE  age>25 OR sex='女' 
~~~

---

# 21. IN和NOT IN

~~~SQL
-- 多个OR连接并不是很方便
SELECT * FROM member WHERE age = 30 OR age=40 OR age=50 OR age=45 ;
-- 使用IN进行匹配,达到上面OR的效果
SELECT * FROM member WHERE age IN(30,40,50,45) ;
~~~

~~~SQL
-- 多个AND连接不方便
SELECT * FROM member WHERE age != 30 AND age!=40 AND age!=50 AND age!=45 ;
SELECT * FROM member WHERE age NOT IN(30,40,50,45) ;
~~~

---

# 22. BETWEEN...AND

~~~SQL
-- 查询指定区域内的数据
SELECT * FROM member WHERE age>=30 AND age<=40
-- 也可以使用BETWEEN...and来代替
SELECT * FROM member WHERE age BETWEEN 30 AND 40;
~~~

---

# 23. NOT BETWEEN...AND

~~~SQL
SELECT * FROM member WHERE age<30 OR age>40;
SELECT * FROM member WHERE age NOT BETWEEN 30 AND 40;
~~~

---

# 24. mysql的子查询

把一个查询的结果当成另一个查询的条件进行使用

~~~sql
-- 查询小花购买过的全部的商品
SELECT 
  * 
FROM
  member_shop 
WHERE m_id = 
  (SELECT 
    id
  FROM
    member 
  WHERE nick_name = '小花')
~~~

---

# 25. 多表连接查询

- 内连接(显式内连接)

  ~~~sql
  -- 查询小花下过的订单(使用多表连接进行查询)  笛卡尔积
  SELECT 
    member.id m_id,
    member.nick_name,
    orders.`address`,
    orders.`create_time` 
  FROM
    member INNER JOIN orders 
  ON member.id = orders.m_id 
  WHERE member.nick_name = '小花' ;
  ~~~

  注意:我们使用内连接的时候可以省略`INNER JOIN`,使`逗号`在多个表之间进行连接(隐式连接),连接条件也要使用`WHERE`关键系代替`ON`关键字

  隐式连接的**语法不太友好**,简单的多个表之间的查询可以使用,但是复杂的sql就不建议使用`隐式连接`

  ~~~sql
  -- 查询小花下过的订单(使用多表连接进行查询)  笛卡尔积
  SELECT 
    m.id  m_id,
    m.nick_name,
    o.`address`,
    o.`create_time`
  FROM
    member m,
    orders o
  WHERE m.id = o.m_id 
    AND m.nick_name = '小花' ;
  ~~~

  

- 外链接

  - 左外连接

    ~~~sql
    SELECT 
      member.id m_id,
      member.nick_name,
      orders.`address`,
      orders.`create_time` 
    FROM
      member LEFT JOIN orders 
    ON member.id = orders.m_id 
    AND member.nick_name = '小花' ;
    ~~~

  - 右外连接(不建议使用,因为可以直接转换为左外链接)

多表的联结又分为以下几种类型：

1）左联结（left join），联结结果保留左表的全部数据

2）右联结（right join），联结结果保留右表的全部数据

3）内联结（inner join），取两表的公共数据

from子句中on条件主要用来连接表，其他不属于连接表的条件可以使用where子句来指定

数据库在通过连接两张或多张表来返回记录时，都会生成一张中间的临时表，然后再将这张临时表返回给用户。 在使用left jion时，on和where条件的区别如下：

1、on条件是在生成临时表时使用的条件，它不管on中的条件是否为真，都会返回左边表中的记录。

2、where条件是在临时表生成好后，再对临时表进行过滤的条件。这时已经没有left join的含义（必须返回左边表的记录）了，条件不为真的就全部过滤掉。

SQL语句中连接多个表时, 请使用表的别名并把别名前缀于每个Column上。这样一来,就可以减少解析的时间并减少那些由Column歧义引起的语法错误。

---

# 26. 多表的连接的商业项目使用建议

- 多表连接时尽量使用**显式连接**,因为显式连接的sql的语义明确

- 生产环境建议表的联查个数不要超过3张表(可以是3张)
- 如果3张表的联查还不能解决你的问题,那么你就需要在**业务层面解决**或者是**数据库设计缺陷**

- **在项目中不建议使用外键**,我们完全可以在业务层保证数据的安全性;

---

# 27. 级联删除与级联更新

~~~~sql
-- ON DELETE CASCADE  级联删除
ALTER TABLE orders ADD FOREIGN KEY(`m_id`) REFERENCES member(`id`) ON DELETE CASCADE ;
~~~~

~~~sql
-- ON UPDATE CASCADE   级联更新
ALTER TABLE orders  ADD FOREIGN KEY (`m_id`) REFERENCES member (`id`)  ON UPDATE CASCADE;
~~~

当然级联更新和级联删除时可以同时设置的

~~~sql
ALTER TABLE orders  ADD FOREIGN KEY (`m_id`) REFERENCES member (`id`) 
ON DELETE CASCADE  ON UPDATE CASCADE;
~~~

> 也可以借助可视化工具进行修改级联删除和级联更新

---

# 28. mysql中的分隔符(定界符)

mysql中默认的分割符是 `;`  也就是说遇到 `;` 就会立即执行sql

在**函数 存储过程**这些特性中需要写多个sql组成一个整体,当成整体来执行,而这些特性中,一条一条的语句之间语法规定必须用`;`来分开

所以我们要创建函数 存储过程 必须先要把默认的分隔符 `;`替换成其他的符号;

**在会话中替换默认的分隔符,使用下面语句**

~~~sql
DELIMITER $$  
~~~

---

# 29. mysql中的函数

![image-20201210161925782](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201210161925782.png)

函数:在编程中函数其实就是一段业务的封装

mysql中的函数: 对多个sql业务的封装,**避免反复的进行词法和语法分析**

## 29.1 系统函数

mysql系统帮我们定义的函数

~~~sql
-- 查询mysql系统当前时间
SELECT NOW();
-- 查询当前mysql的版本
SELECT VERSION();
-- 查询当前所在的数据库
SELECT DATABASE();
-- 查询当前登录mysql的用户和主机
SELECT USER();
-- 获取一个字符串对应的md5值
SELECT MD5("HELLO");


--当distinct应用到多个字段的时候，其应用的范围是其后面的所有字段，而不只是紧挨着它的一个字段，而且distinct只能放到所有字段的前面
-- distinct对NULL是不进行过滤的，即返回的结果中是包含NULL值的。
-- distinct一般是用来去除查询结果中的重复记录的，而且这个语句只可以在select中使用

~~~

---

## 29.2 聚合函数

~~~sql
-- 统计某个字段的记录数
SELECT COUNT(`age`) FROM student;
-- 查询某个字段的最大值
SELECT MAX(age) FROM student;
-- 查询某个字段的最小值
SELECT MIN(age) FROM student;
-- 查询某个字段的平均值
SELECT AVG(age) FROM student;
-- 用来返回前几条或者中间某几行数据
SELECT * FROM tableName limit i,n
-- tableName：表名
-- i：为查询结果的索引值(默认从0开始)，当i=0时可省略i
-- n：为查询结果返回的数量
-- i与n之间使用英文逗号","隔开
-- LIMIT 子句可以被用于强制 SELECT 语句返回指定的记录数。LIMIT 接受一个或两个数字参数。参数必须是一个整数常量。如果给定两个参数，第一个参数指定第一个返回记录行的偏移量，第二个参数指定返回记录行的最大数目。初始记录行的偏移量是 0(而不是 1)

-- IFNULL(,) 函数用于判断第一个表达式是否为 NULL，如果为 NULL 则返回第二个参数的值，如果不为 NULL 则返回第一个参数的值。

IFNULL(,) 函数语法格式为：

IFNULL(expression, alt_value)
如果第一个参数的表达式 expression 为 NULL，则返回第二个参数的备用值。
~~~

---

## 29.3 自定义函数(了解)

我们自己编写的函数

- 函数的参数
- 返回值

~~~sehll
- 函数的可以返回任意类型的值,也可以接受任意类型的值为参数
- 函数的返回值和参数没有必然联系的
~~~

~~~sql
CREATE FUNCTION 函数名称(参数名称 参数类型) RETURNS 返回值类型
BEGIN
	函数语句 #函数语句只有一条时,可以省略BEGIN和END
END
~~~

~~~sql
-- 首先创建函数之前一定要先修改其定界符,要不然遇到函数中的第一个;就会执行报错
DELIMITER $$
CREATE FUNCTION MYADD (a INT,b INT) RETURNS INT 
BEGIN
  DECLARE i INT ; -- 变量的定义
  SET i = a+b ; -- 给变量设置值
  RETURN i ;  -- 返回数据
END $$

SELECT MYADD(12,22); -- 函数的调用
~~~

~~~sql
-- 创建自定义函数
DELIMITER $$
CREATE FUNCTION STUAGEGTCOUNT (a INT) RETURNS BIGINT 
BEGIN
  DECLARE c BIGINT;
   SET c=(SELECT COUNT(*) FROM student WHERE age>a);
   RETURN c;
END $$
-- 调用自定义函数
SELECT STUAGEGTCOUNT(30);
~~~

**函数的调用**

~~~sql
SELECT 函数名称(实参列表);
~~~

**删除函数**

~~~sql
DROP FUNCTION [IF EXISTS]  `MYADD`
~~~

**自定义函数可能出现错误:**

~~~sql
错误代码： 1418
This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)
~~~

==原因:==mysql在新版本中添加**函数保护器**,默认如果不进行配置,则无法创建自定义函数,我们需要在当前会话中把函数保护器关掉,尽量不要在全局(系统的配置文件)关闭函数保护器;

~~~sql
-- 查看函数保护器的状态
SHOW VARIABLES LIKE "%log_bin_trust_function_creators%"
	-- OFF:不能创建自定义函数(保护器处于开启状态)
	-- ON:能创建自定义函数(保护器关闭)
~~~

~~~sql
-- 允许创建自定义函数
SET GLOBAL log_bin_trust_function_creators=1;
~~~

经过上面的设置我们就可以正常的创建自定义函数了;

---

# 30. mysql存储过程(了解)

## 30.1 sql语句的执行流程

~~~sql
sql语句--->sql执行优化器(编译)--->词法分析,语法分析--->sql优化--->运行sql(读取数据)-->结果
~~~

---

## 30.2 什么是存储过程?

 sql语句的编译的集合,以名称来存储,合并为一个单元处理;

---

## 30.3 存储过程的特点

- 实现较快的执行速度(避免重复的编译,词法分析,语法分析等操作)
- 减少网络流量

---

## 30.4 存储过程的语法

~~~sql
-- 创建存储过程
DELIMITER $$
CREATE PROCEDURE proc2 (a INT) 
BEGIN
  SELECT * FROM student WHERE age>a;
  SELECT * FROM member;
END $$

-- 调用存储过程
CALL proc2(20)
~~~

## 30.5 删除存储过程

~~~sql
DROP PROCEDURE proc1;
~~~

---

# 31. mysql函数和存储过程对比

- 存储过程和函数都是为了提高**程序的运行效率**和**减少网络带宽**而存在的

- 存储过程可以实现相对复杂的功能,而函数针对性比较强
- 存储过程可以返回多个值(无需`return`关键字),函数只能有一个返回值

- **在实际商业项目中尽量不要使用存储过程和自定义函数**

---

# 32. mysql中的视图(了解)

视图其实就是一个虚拟表,这虚拟表可以存储我们查询的结果,方便我们进行二次查询,提升查询效率;查询的时候就可以像使用表一样用视图;

~~~sql
-- 创建视图(View)
CREATE VIEW myStudentView AS (SELECT * FROM student WHERE age>20);
~~~

~~~sql
-- 查询视图的数据(可以把视图当成表来使用)
SELECT * FROM myStudentView;
~~~

~~~sql
-- 删除视图
DROP VIEW myStudentView;
~~~

**注意: 视图是依赖表而存在的,如果表被删除了,视图就无效了;**

---

# 33. mysql中的触发器(了解)

触发器(trigger):监听事件,并触发某操作

触发器的四要素:

- 监视地点(table)
- 监视事件(insert/update/delete)
- 触发时机(after/before)
- 触发事件(insert/update/delete)

~~~sql
-- 创建班级表
CREATE TABLE grade (
  id INT AUTO_INCREMENT,
  gname VARCHAR (64),
  PRIMARY KEY(id)
);
-- 创建学生表
CREATE TABLE student (
  id INT AUTO_INCREMENT,
  nickname VARCHAR (64),
  age TINYINT UNSIGNED,
  g_id TINYINT UNSIGNED,
  PRIMARY KEY(id)
);
~~~

需求: 我们删除班级,实现自动删除班级对应的学生 

~~~sql
-- 创建触发器
DELIMITER $$
-- 触发地点:grade  监听事件: delete  触发时机: before  触发事件:delete
CREATE TRIGGER `tigger1` BEFORE DELETE ON `grade` 
    FOR EACH ROW 
BEGIN
 DELETE FROM student; 
END;
$$
DELIMITER ;
~~~

---

# 34. mysql中的存储引擎

mysql可以将数据以不同的技术存储在文件或者(内存)中,这种技术叫存储引擎;

每种存储引擎都有不同的存储机制,索引技术,表锁定技术,最终应用的场景各不相同,但是现在最主流的mysql的存储引擎用的对多的还是`INNODB`

|   存储引擎   | MYISAM | INNODB  | MEMORY |
| :----------: | :----: | :-----: | :----: |
|   存储限制   | 256TB  |  64TB   | 有限制 |
|  事务安全性  |   NO   | ==YES== |   NO   |
|   支持索引   |  YES   | ==YES== |  YES   |
|   数据压缩   |  YES   |   NO    |   NO   |
| 是否支持外键 |   NO   | ==YES== |   NO   |

> 如果没有特殊要求,在互联网项目中,INNODB存储引擎是我们首选;
>

---

# 35.  执行mysql的脚本

- 登录mysql之后执行sql脚本

  ~~~sql
  source sqlpath
  ~~~

- 登录mysql时候直接执行sql脚本

  ~~~sql
  mysql -uroot -p <sqlpath
  ~~~

- **使用可视化工具备份和还原(推荐做法)**

---

# 36. Mysql管理工具

- Workbench(mysql官方推荐使用的)  免费的 跨平台的 
- Sequel Pro 只在mac端有
- HeidiSQL(免费  开源)
- phpMyAdmin(web应用)
- mysqlfont(免费  轻量级) 只有windows端有,对高分辨率屏支持不好
- **Navicat(商业 收费) 跨平台  功能最强大  UI最漂亮**  
- **SQLyog(收费  不跨平台)** 对高分辨率屏支持不好

---

# 37. mysql的版本升级

升级数据库版本之前一定要先备份数据,再执行升级;

- 备份数据
- 卸载老版本的mysql
  - 停止系统的mysql服务
  - 检查系统服务是否存在如果存在先删除 `sc delete mysql`,这个命令必须使用管理员用户权限
  - 删除数据目录 
- 安装新版本的`mysql-8.0.22.0`
- 恢复数据(执行前面备份的sql脚本)

---

# 38. mysql中的模糊查询

`%`: 匹配0个或者多个任意字符

`_`: 匹配任意一个字符

~~~sql
-- 查询昵称中以 '小' 开头的数据
SELECT * FROM member WHERE nickname LIKE '小%';
-- 查询昵称中包含 '小' 的数据
SELECT * FROM member WHERE nickname LIKE '%小%';
~~~

- 模糊查询的前缀查询效率极低,要慎重使用;

- 一般生产环境会直接禁用**like**模糊查询功能;

---

# 39. mysql中的事务

## 39.1 mysql事务介绍

把做完一个业务分成好多单元,整个过程每个单元**全部**处理成功,才算整个的业务处理成功,只要有有任何一个单元处理失败,则认为业务处理失败;

**作用: 保证了数据的完整性**

---

## 39.2 事务控制

整个过程的每一个单元全部处理成功那么事务才会**提交(commit)**,只要其中任何一个单元出现异常,我们则让事务**回滚(rollback)**

---

## 39.3 事务的特性

ACID

atomicity(原子性): 事务中所有的操作要么全部成功,要么全部失败;

consistency(一致性):事务执行前后的状态(数据)保持一致

isolation(隔离性): 多个事务在执行过程中互相不受影响

durability(持久性):事务一旦被提交,那么对数据库中的数据的改变时永久性的,即使在数据库系统遇到故障的时候,排除故障之后这些数据也不会丢失;

注意: 事务这个特性其实我们一直在使用,只是我们没有特别的在意这个事,因为mysql默认的事务的提交方式是自动提交的

~~~sql
SHOW VARIABLES LIKE "%autocommit%"
~~~

---

## 39.4 手动控制事务的提交与回滚

mysql默认事务的提交方式是自动提交的,但是我们一般使用到事务的时候都会进行手动的控制,也就是要关闭mysql的事务自动提交;

~~~sql
-- 开启事务(临时关闭mysql的"事务自动提交")
START TRANSACTION;-- 或者使用 BEGIN;
INSERT INTO member(nickname,sex,age)VALUES("小胖1",'男',30);
UPDATE member SET age=10 WHERE id=3;
-- 提交事务
COMMIT;
~~~

~~~SQL
-- 开启事务(临时关闭mysql的"事务自动提交")
START TRANSACTION;  -- 或者使用BEGIN;
INSERT INTO member(nickname,sex,age)VALUES("小胖1",'男',30);
UPDATE member SET age=10 WHERE id=3;
-- 回滚事务
ROLLBACK;
~~~

## 39.5 事务的隔离性

隔离性保证了,多个事务在执行过程中互相不受影响;

mysql中的事务隔离级别有以下几种:

- Read UNCOMMITTED(读未提交)
- READ COMMITTED(读已提交) ---->oracle中事务默认的隔离级别
- REPEATABLE READ(重复读) --->mysql中的事务的隔离级别
- SERIALIZABLE(串行化)

~~~sql
-- 查询默认的事务隔离级别
SELECT @@tx_isolation  --老版本中的查询方式
SELECT @@transaction_isolation  -- 新版本中查询方式
~~~

~~~sql
-- 更改当前会话的隔离级别
set session transaction isolation level read uncommitted; -- 设置当前会话隔离级别为读未提交
~~~

---

## 39.6 事务隔离级别引发的问题

> 脏读问题

- 脏读:  设置事务的隔离级别为 `READ UNCOMMITTED`,会读到其他事务没有提交的数据;

- 解决脏读: 设置事务的隔离级别为 `READ COMMITTED`,既可以解决数据脏读问题

> 不可重复读问题

- 不可重复读: 在一个事务中,前后两次读到的数据不一致
- 解决不可重复读: 使用的`REPEATABLE READ`隔离级别可以解决;

> 虚读|幻读问题

- 虚读: 设置为`READ UNCOMMITTED ,READ COMMITTED,REPEATABLE READ`的事务,有时候可以读取到其他事务新插入的行,这种情况就称为虚读;

- 解决虚读: 把事务的隔离级别设置成 `SERIALIZABLE`之后就可以解决虚读问题;

> 解决方案: 我们在实际开发过程中一般不会使用两个极端的隔离级别(读未提交,  串行化),我们会使用中间的两个;

---

# 40. innodb锁机制

## 40.1 innodb锁机制介绍

从锁的颗粒来说锁分为行锁和表锁;

在innodb中提供了两种锁机制:

- 乐观锁:  并不是硬编码的实现,而是通过version版本号来进行实现==(innodb中并没有实现乐观锁)==

- 悲观锁: 这是innodb存储引擎默认实现的锁机制,这种锁是**表锁**,而悲观锁的实现又分为两种实现:

  - 共享锁(S锁),读锁
    - 在读取的行设置一个共享模式的锁,这个共享锁允许其他的会话读取数据,但是不允许修改,如果其他的会话也需要修改数据,则要等待持有共享锁的会话结束锁的释放,才能修改数据;
    - 可以在多个会话中加多个共享锁
    - **添加多个共享锁容易出现互相等待释放的情况,造成死锁问题,所以使用多个共享锁一定要慎重;**
  - 排它锁(X锁),写锁
    - 排它锁是不允许重复添加的
    - 排它锁没有死锁问题

  ---

## 40.2 显式加锁

- 共享锁的添加: `lock in share mode`
- 排它锁的添加: `for update`

---

## 40.3 mvcc并发系统快照读与当前读

**快照读:** 不加锁的select操作就属于快照读

当前读: 加锁的操作属于当前读

当前读读到的是最新的数据,而且在读取的过程中是==不允许==其他的事务修改数据;

---

# 41. mysql中的执行计划

mysql执行流程:

客户端(sql语句)--->mysql-rdbms-->innodb存储引擎-->sql查询优化器(sql语句的优化)--->....

sql查询优化器会把优化的东西形成一个产物,这个产物就是执行计划;

我们在实际的开发过程中,一般设计到mysql的优化都会先查看其执行计划;

~~~sql
EXPLAIN SELECT * FROM shop;
~~~

- 执行计划看的时候先看执行计划的id,**id越大的先执行**;

- 如果id相同从上往下看

---

# 42. mysql中的索引技术

## 42.1 索引技术的介绍

- mysql中的索引技术可以帮助我们快速检索数据
- innodb底层索引技术就是通过B+tree实现的
- 索引其实就是我们平常用到的 '`目录`'
- 索引在mysql启动时就会加到内存中,形成B+Tree,在mysql停止的时候会持久化到硬盘;

---

## 42.2 索引的分类

- 普通索引
- 主键索引
- 唯一索引
- 全文索引

---

## 42.3 普通索引

普通索引如果不指定名称,则索引的名称和字段的名称相同;

~~~sql
-- 创建索引的第一种方式
CREATE INDEX idx_nickname ON account(nickname); 

-- 创建索引的第二种方式(不常用)
ALTER TABLE account ADD INDEX idx_nickname1(nickname);

-- 创建索引的第三种方式
CREATE TABLE u1(nickname VARCHAR(64),age TINYINT UNSIGNED,KEY idx_nickname(nickname));
~~~

- 建立普通索引的列的数据是可以重复的

---

## 42.4 主键索引

主键索引如果不指定索引的名称,则主键索引的名称为 `PRIMARY`

~~~sql
-- 声明为主键的列就是自动添加主键索引
CREATE TABLE u1(id INT ,nickname VARCHAR(64),age TINYINT UNSIGNED,PRIMARY KEY(id));
~~~

- 主键索引的列的数据非空,唯一的

- 一个表中建议只有一个主键列

---

## 42.5 唯一索引

唯一索引如果不指定名称,则索引的名称和字段的名称相同;

~~~sql
-- 创建索引的第一种方式
CREATE UNIQUE INDEX uk_nickname ON u1(nickname); 

-- 创建表时创建唯一索引
CREATE TABLE u1 (
  id INT,
  nickname VARCHAR (64) ,
  age TINYINT UNSIGNED,
  PRIMARY KEY (id),
  UNIQUE KEY uk_nickname(nickname) 
) ;
~~~

- 如果一列被约束为`UNIQUE`,则在这一列默认添加唯一索引
- 添加唯一索引的列可以为`NULL`值,这也是和主键索引,不同的地方

---

## 42.6 全文索引

> 全文索引,通过建立`倒排索引`,可以提高数据的检索效率,解决判断字段中 `是否包含` 的问题;

我们如果使用like关键字会出现很多问题:

~~~sql
-- 我们已经给nickname字段添加了普通索引
-- 会使用索引
SELECT * FROM account WHERE nickname='小明';
-- 会使用索引
SELECT * FROM account WHERE nickname LIKE '小明%';
-- 不会使用索引
SELECT * FROM account WHERE nickname LIKE '%小明%';
~~~

不会使用到索引我们如果进行大规模数据检索时,效率会大大的降低,所以前面我们说过 `我们只在简单业务或者数据量小的时候才考虑使用like关键字`;

> 全文索引注意的地方:

- mysql5.6以前,只有`MYISAM`存储引擎支持全文索引
- 在5.6中`INNODB`存储引擎加入了对全文索引的支持,**但是只支持英文的全文索引,不支持中文的全文索引**

- ==**在5.7.6中,mysql内置了`ngram`分词器,用来支持中文;**==

> 配置ngram分词的最小长度:

默认长度为2,当然我们也可以设置成1,但是设置成1的话就会浪费大量的空间,不是很好,`mysql建议我们配置为2`;

~~~shell
#ngram分词器对分词最小长度(也就是说分词器,分词的时候最小也是两个词一分)
[mysqld]
ft_min_word_len=2
~~~

> 创建全文索引

~~~sql
-- 创建索引的第一种方式
CREATE FULLTEXT INDEX ft_nickname ON account(nickname) WITH PARSER ngram ; 

-- 创建索引的第二种方式(不常用)
ALTER TABLE account ADD FULLTEXT INDEX ft_nickname(nickname) WITH PARSER ngram;

-- 创建索引的第三种方式
CREATE TABLE u1(nickname VARCHAR(64),age TINYINT UNSIGNED,FULLTEXT KEY ft_nickname(nickname) WITH PARSER ngram );
~~~

> 使用全文索引

~~~SQL
SELECT * FROM account WHERE MATCH(nickname) AGAINST("你觉得华为笔记本合小米手机哪个好");
~~~

- `match`中的字段和创建全文建索引时的字段**必须一致**;

> 全文索引的检索流程

用户输入词--->sql执行引擎--->`ngram`分词器对用户输入的词进行分词(配置了最小的分词个数)--->把分词器分的词依次的去倒排索引中去查找,找出相应的记录返回;

---

## 42.7 组合索引

**包含多个字段的索引**称为组合索引;

> 组合索引包含

- 组合普通索引
- 组合主键索引
- 组合唯一索引
- 组合全文索引

~~~sql
-- 创建复合索引时必须指定索引的名称,不能省略
CREATE INDEX mu_title_content ON article(title,content,publish_time);
~~~

~~~sql
SELECT * FROM article WHERE title LIKE '小米%'; -- 使用到索引
SELECT * FROM article WHERE title LIKE '小米%' AND content LIKE '小米%'; -- 使用了索引
SELECT * FROM article WHERE  content LIKE '小米%'; -- 没有索引
~~~

**建议多列索引的列不要超过2个列**

- 以上这个复合索引相当于建立了这3个索引

  ~~~sql
  (title),(title,content),(title,content,publish_time)
  ~~~

- 多列索引遵循**最左前缀**的原则
- 多列索引在创建的时候,如果其中有字段时`TEXT`或`BLOB`类型,就必须指定索引的长度;

---

## 42.8 使用索引的优点

- 使用主键索引或者唯一索引,可以保证数据库中的表的数据是唯一
- **通过建立索引可以大大的提高数据检索的效率,减少表扫描的行数(避免进行全表扫描)**
- 我们在进行多表连接的时候,可以使用索引加速表之间的连接

---

## 42.9 使用索引的缺点

- 在创建索引和维护索引时都需要耗费时间;
- 索引文件会占用物理存储空间,除了表的数据占用一部分空间,索引文件也会占用一部分空间;
- **设置为`text`和`blob`类型的字段强烈不建议添加索引;**

---

# 43. mysql中的记录截取

~~~sql
SELECT * FROM account LIMIT start,count;
start:开始位置,从0开始
count:截取的记录数量
~~~

---

# 44. mysql数据库设计(了解)

- **第一设计范式 ：表中的每一列都不能再分(不要出现二维表)**
- 第二设计范式：满足第一设计范式，除主键外每一列都必须依靠主键

- 第三设计范式：满足第二设计范式，除主键列外，每一列都不能相互依靠

数据库范式的提出是很早以前的事了,在很早以前硬盘是非常昂贵的,一般都会遵循1,2,3范式,但是随着互联网的发展,硬盘非常便宜,所以我们在现在的商业项目中一般不会遵循2,3范式(用时间换空间),**第一范式会遵循**;

---

# 45. 国内大厂的数据库开发规范(参照阿里的开发规范)

1. 库名与应用名称尽量一致
2. 表名、字段名必须使用小写字母或数字，禁止出现数字开头,如果一个单词不能表达那就使用下划线分隔;
3. 表名不使用复数名词;
4. 表的命名最好是加上“业务名称_表的作用”。如，edu_teacher 
5. 表必备三字段：id, gmt_create, gmt_modified
   说明：其中 id 必为主键，类型为 bigint unsigned、单表时自增、步长为 1。（如果使用分库分表集群部署，则id类型为varchar，非自增，业务中使用分布式id生成器）
   gmt_create, gmt_modified 的类型均为 datetime 类型，前者现在时表示主动创建，后者过去分词表示被动更新。
6. 单表行数超过 500 万行或者单表容量超过 2GB，才推荐进行分库分表。 说明：如果预计三年后的数据量根本达不到这个级别，请不要在创建表时就分库分表。
7. 表达是与否概念的字段，必须使用 is_xxx 的方式命名，数据类型是 unsigned tinyint （1 表示是，0 表示否）。
   说明：任何字段如果为非负数，必须是 unsigned。
   注意：POJO 类中的任何布尔类型的变量，都不要加 is 前缀。数据库表示是与否的值，使用 tinyint 类型，坚持 is_xxx 的 命名方式是为了明确其取值含义与取值范围。
   正例：表达逻辑删除的字段名 is_deleted，1 表示删除，0 表示未删除。
8. 小数类型为 decimal，禁止使用 float 和 double。 说明：float 和 double 在存储的时候，存在精度损失的问题，很可能在值的比较时，得到不 正确的结果。如果存储的数据范围超过 decimal 的范围，建议将数据拆成整数和小数分开存储。
9. 如果存储的字符串长度几乎相等，使用 char 定长字符串类型。
10. varchar 是可变长字符串，不预先分配存储空间，长度不要超过 5000，如果存储长度大于此值，定义字段类型为 text，独立出来一张表，用主键来对应，避免影响其它字段索 引效率。
11. 唯一索引名为 uk_字段名；普通索引名则为 idx_字段名。
    说明：uk_ 即 unique key；idx_ 即 index 的简称
12. **不得使用外键与级联，一切外键概念必须在应用层解决**。外键与级联更新适用于单机低并发，不适合分布式、高并发集群；级联更新是强阻塞，存在数据库更新风暴的风险；外键影响数据库的插入速度
13. 慎重使用`like`进行模糊查询, 通配符如果在前面则不会使用到索引,影响检索效率;
14. 如果是简单的搜索业务建议使用mysql5.7中新增的全文索引,不建议直接上solor,elasticsearch这样的检索系统,因为会使得维护成本增加;
