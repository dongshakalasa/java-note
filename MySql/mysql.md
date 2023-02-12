#  一、SQL通用语法

1. SQL语句可以单行或多行书写，以分号结尾。
2. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写。
3. 注释
   - 单行注释：--注释内容或#注释内容(MySQL特有)
   - 多行注释：/\*注释*/

# 二、SQL分类

## 1、DDL - 操作数据库

### 1.1、查询

```sql
show DATABASES;
```

### 1.2、创建

- 创建数据库

```sql
CREATE DATABASE 数据库名称 
```

- 创建数据库(判断，如果不存在则创建)

```sql
CRATE DATABASE IF NOT EXISIS 数据库名称；
```

### 1.3、删除

- 删除数据库

```sql
DROP DATABASE 数据库名称
```

- 删除数据库(判断，如果存在则删除)

```sql
DROP DATABASE IF EXISTS 数据库名称
```

### 1.4、使用数据库

- 查看当前使用的数据库

```sql
SELECT DATABASE();
```

- 使用数据库

```sql
USE 数据库名称；
```

## 2、DDL - 操作表

### 2.1、查询表		

- 查询当前数据库中的所有表名称

```mysql
SHOW TABLES;
```

- 查询表结构

```mysql
DESC 表名称
```



### 2.2、创建表

注意：最后一行末尾，不能加逗号

```sql
CREATE TABLE 表名（
			字段1	数据类型1，
			字段2 数据类型2,
			...
			字段n 数据类型n
); 
```



### 2.3、删除表

```sql
1、直接删除表
DROP TABLE 表名;

2、删除表时判断表是否存在
DROP TABLE IF EXISTS 表名;
```

### 2.4、修改表

```sql
1、修改表名
ALTAER TABLE 表名 RENAME TO 新的表名;

2、添加一列
ALTER TABLE 表名 ADD 列名 数据类型;

3、修改数据类型
ALTER TABLE 表名 MODIFY 列名 新数据类型;

4、修改列名和数据类型
ALTER TABLE 表名 CHANGE 列名 新的列名 新的数据类型;

5、删除列
ALTER TABLE 表名 DROP 列名;
```



## 3、DML - 操作数据 

### 3.1、添加数据

```sql
1、给定列的添加数据
INSERT INTO 表名（列名1,列名2,...）VALUES(值1,值2,...);

2、给全部的列添加数据
INSERT INTO 表名 VALUES(值1，值2，...);

3、批量添加数据
INSERT INTO 表名(列名1,列名2,...) VALUES(值1,值2,...),(值1,值2,...),(值1,值2,...)...;
INSERT INTO 表名 VALUES(值1,值2,...),(值1,值2,...),(值1,值2,...)...;
```

### 3.2、修改数据

​	注意：修改语句中如果不加条件，则将所有数据都修改

```sql
UPDATE 表名 SET 列名1=值1,列名2=值2,...[WHERE 条件];
```

### 3.4、删除数据

​	注意：删除语句中如果不加条件，则将所有数据都删除

```sql
DELETE FROM 表名 [WHRER 条件];
```

## 4、DQL - 基础查询

### 4.1、查询语法

```sql
SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段
HAVING
	分组后条件
ORDER BY
	排序字段
LIMIT
	分页限定
```

### 4.2、基础查询

```sql
1、查询多个字段
SELECT 字段列表 FROM 表名;
SELECT * FROM; --查询所有数据

2、去除重复记录
SELECT DISTINCT 字段列表 FROM 表名

3、起别名
AS: AS 也可以省略
```

### 4.2、条件查询

```sql
1、条件查询语法
SELECT 字段列表 FROM 表名 WHERE 条件列表

2、条件查询
	>					大于
	<					小于
	>=					大于等于
	<=					小于等于
	=					等于
	<>或!=	   		         不等于
	BETWEEN...AND		在某个范围之内(都包含)
	IN(...)				     多选一
	LINK占位符			   模糊查询_单个任意字符 %多个任意字符
	IS NULL				是NULL
	IS NOT NULL			不是NULL
	AND或&&				并且
	OR或||				或者
	NOT或				非，不是
```

### 4.3、排序查询

```sql
1、排序查询语法
注意：如果有多个排序条件，当前边的条件值一样时，才会根据条件二进行排序

SELECT 字段列表 FORM 表名 ORDER BY 排序字段名1 [排序方式1]，排序字段2 [排序方法2],...;

排序方式：
ASC:升序排列（默认）
DESC：降序排序
```

### 4.4、分组查询

#### 4.4.1、聚合函数

1、 概念：将一列数据作为一个整体，进行纵向计算

2、聚合函数分类

- count(列名)	 统计数量(一般选用不为null的列)
- max(列名)       最大值
- min(列名)       最小值
- sum(列名)       求和
- avg(列名)       平均

3、聚合函数语法

```sql
SELECT 聚合函数名(列名) FROM 表名

注意：null值不参与所有聚合函数运算
```

#### 4.4.2、分组查询

```sql
1、分组查询语法
SELECT 字段列表 FROM 表名 [WHERE 分组前条件限定] GROUP BY 分组字段名 [HAVING 分组后条件过滤]；

注意：分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义

where和having区别：
执行时机不一样：where是分组之前进行限定，不满足where条件，则不参与分组，而having是分组之后对结果进行过滤
可判断的条件不一样：where不能对聚合函数进行判断，having可以

执行顺序：where > 聚合函数 > having
```

### 4.5、分页查询

1、分页查询语法

```sql
SELECT 字段列表 FROM 表名 LIMIT 其实索引，查询条目数；
```

- 起始索引：从0开始
- 计算公式：起始索引 = (当前页码 - 1) * 每页显示的条数
- tips:
  - 分页查询limit是MySQL数据库的方言
  - Oracle分页查询使用rownumber
  - SQL Server分页查询使用top

## 5、DCL-数据控制语言

用来管理数据库 用户、控制数据库的访问权限

### 5.1、DCL管理用户

1、查询用户

```mysql
USE mysql;
SELECT * FROM user
```

2、创建用户

```mysql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码'；
```

3、修改用户密码

```mysql
ALERT USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码'；
```

4、删除用户

```mysql
DROP USER '用户名'@'主机名'
```

注意：

- 主机名可以使用 % 通配符
- 这类SQL开发人员操作的比较少，主要是DBA（数据库管理员）使用

### 5.2、DCL权限控制

MySQL中定义了很多种权限，常用的如下

![image-20221007193835840](截图\image-20221007193835840.png)

1、查询权限

```mysql
SHOW GRANTS FOR '用户名'@'主机名'
```

2、授予权限

```mysql
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名'
```

3、撤销权限

```mysql
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名'
```

# 三、函数

## 1、字符串函数

MySQL中内置了很多字符串函数，常用的如下

![image-20221007194901522](截图\image-20221007194901522.png)

## 2、数值函数

常见的数值函数：

![image-20221007210938359](截图\image-20221007210938359.png)

## 3、日期函数

常见的日期函数如下：

![image-20221007212402593](截图\image-20221007212402593.png)

## 4、流程函数

可以在SQL语句中实现条件筛选，从而提高语句的效率。

![image-20221007215019595](截图\image-20221007215019595.png)

# 四、约束

## 1、约束的概念和分类

### 1.1、约束的概念

- 约束是作用于表中列上的规则，用于限制加入表的数据
- 约束的存在保证了数据库中数据的正确性、有效性和完整性

### 1.2、约束的分类

| 约束名称 | 描述                                                         | 关键字      |
| :------- | ------------------------------------------------------------ | ----------- |
| 非空约束 | 保证列中所有数据不能有null值                                 | NOT NULL    |
| 唯一约束 | 保证列中数据各不相同                                         | UNIQUE      |
| 主键约束 | 主键是一行数据的唯一标识，要求非空且唯一                     | PRIMARY KEY |
| 检查约束 | 保证列中的值满足某一条件                                     | CHECK       |
| 默认约束 | 保存数据时，未指定值则采用默认值                             | DEFAULT     |
| 外键约束 | 外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性 | FOREIGN KEY |

### 1.3、自动增长

auto_increment:当列是数据类型并且唯一约束

## 2、基本语法

①、添加约束

```sql
-- 创建表时添加约束
CRATE TABLE 表名（
	列名 数据类型 NOT NULL，-- 非空约束
	列名 数据类型 unique  [Auto_increment]， -- 唯一约束 AUTO_INCREMENT：当不指定值时，自动增长
	列名 数据类型 primary KEY  [Auto_increment]，-- 主键约束
	列名 数据类型 default 默认值，-- 默认约束
	）
```

```sql
-- 建完表后添加约束
ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
ALTER TABLE 表名 ADD PRIMARY KEY(字段名);
ALTER TABLE 表名 ALTER 列名 SET DEFAULT 默认值;
```

②、删除约束

```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型； -- 非空
ALTER TABLE 表名 DROP INDEX 字段名; -- 唯一
ALTER TABLE 表名 DROP PRIMARY KEY; -- 主键
ALTER TABLE 表名 ALTER 列名 DROP DEFAULT; -- 默认值

```

## 3、外键约束

(1)、创建表时

```mysql
constraint 外键名称 foreign KEY(外键列名)references 主表(主表列名)
```

(2)、添加外键

```mysql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称) ;
```

(3)、删除外键

```mysql
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称; -- 外键
```

(4)、删除和更新行为

![image-20221008084952185](截图\image-20221008084952185.png)

```mysql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY （外键字段）REFERENCES 主表名（主表字段）ON UPDATE CASCADE ON DELETE CASCADE
```

# 五、多表查询

## 1、多表查询简介

- 多表查询：从多张表查询数据
  - 连接查询
    - 内连接：相当于查询`A,B`交集数据
    - 外连接：
      - 左外连接：相当于查询`A`表所有数据和交集部分数据
      - 右外连接：相当于查询`B`表所有数据和交集部分数据
  - 子查询

## 2、内连接

内连接查询语法

```sql
-- 隐式内连接
SELECT 字段列表 FROM 表1，表2...WHERE 条件

-- 显示内连接
SELECT 字段列表 FROM 表1，[INNER] JOIN 表2 ON 条件
```

- 内连接相当于查询 `A B` 交集数据

## 3、外连接

外连接查询语法

```sql
-- 左外连接
SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件；

-- 右外连接
SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件；
```

- 左外连接：相当于查询A表所有数据和交集部分数据
- 右外连接：相当于查询B表所有数据和交集部分数据

## 4、联合查询

对于union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集

```MYSQL
SELECT 字段列表 FROM 表A ...
UNION [ALL]
SELECT 字段列表 FROM 表B ...
```

对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致。

## 5、子查询

1、子查询的概念：

- 查询中嵌套查询，称嵌套查询为子查询

2、子查询根据查询结果不同，作用不同：

- 单行单列
- 单行多列
- 多行单列
- 多行多列 

3、子查询根据查询结果不同，作用不同：

```sql
-- 单行单列：作为条件值，使用= != > <等进行判断
SELECT 字段列表 FROM 表 WHERE  字段名 = (子查询)；

-- 单行多列：作为条件值，使用= != > <等进行判断
SELECT 字段列表 FROM 表 WHERE  （字段名1，字段名2） = (子查询)；

-- 多行单列：作为条件值，使用in，any,all等关键字进行条件判断
SELECT 字段列表 FROM 表 WHERE 字段名 in (子查询)；

-- 多行多列
SELECT 字段列表 FROM (子查询)  WHERE 条件；
SELECT 字段列表 FROM 表 WHERE  （字段名1，字段名2） in (子查询)；
```



#  六、事务

## 1、事务的简介

- 数据的事务（transaction)是一中机制、一个操作序列，包含了`一组数据库操作命令`
- 事务把所有的命令作为一个整体一起向系统提价或撤销操作请求，即这一组数据库命令`要么同时成功，要么同时失败`
- 事务是一个不可分割的工作逻辑单元

```sql
-- 开启事务
START TRANSACTION;
或者 BEGIN;

--提交事务
COMMIT;

-- 回滚事务
ROLLBACK;
```

## 2、事务的四大特性

- 原子性（`A`tomicity)：事务是不可分割的最小操作单位，要么同时成功，要么同时失败
- 一致性（`C`onsistency)：事务完成时，必须使所有数据都保持一直状态
- 隔离性（`I`solation)：多个事务之间，操作的可见性
- 持久性（`D`urability)：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的

## 3、相关问题

MySQL事务默认自动提交

```
-- 查看事务的默认提交方式
SELECT @@autocommit;

--1 自动提交  0 手动提交
--修改事务提交方式
set @@autocommit = 0;
```

# 七、MySQL数据类型

## 1、概述

### 1.1、MySQL中的数据类型

![img](截图\35181d3e526046938b0febfce0346ea0.png)

### 1.2、常见数据类型的属性

![img](截图\8bf8b262d8984002892544c5047161fc.png)

## 2、数据类型

### 2.1、整数类型

整数类型一共有5中，包括TINYINT、SMALLINT、MEDIUMINT、INT(INTGENR)和BIGINT。区别如下

![img](截图\641cba90fbb146a383d8ccc273b25791.png)

**应用常见**

- TINYINT（tinyint）：一般用于枚举数据，比如系统设定取值范围很小且固定的场景。
- SMALLINT（smallint） ：可以用于较小范围的统计数据，比如统计工厂的固定资产库存数量等。
- MEDIUMINT （mediumint）：用于较大整数的计算，比如车站每日的客流量等。
- INT、INTEGER（int、integer） ：取值范围足够大，一般情况下不用考虑超限问题，用得最多。比如商品编号。
- BIGINT（bigint） ：只有当你处理特别巨大的整数时才会用到。比如双十一的交易量、大型门户网站点击量、证券公司衍生产品持仓等。

**举例**

```mysql
# 创建数据库时指明字符集
CREATE DATABASE IF NOT EXISTS dbtest1 CHARACTER SET 'utf8';

SHOW  CREATE DATABASE dbtest1;

USE dbtest1;
# 创建表时指明表的字符集
CREATE TABLE test1(
f1 TINYINT,
f2 SMALLINT,
f3 MEDIUMINT,
f4 INT,
f5 BIGINT
)CHARACTER SET 'utf8';


CREATE TABLE test2(
f1 INT,
f2 INT(5),  # f1和f2在最终的显示效果是一样的，括号中的数字不影响该数据类型本身的显示位数
f3 INT(5) ZEROFILL  # 显示宽度为5。当insert的值不足5位时，使用0填充；当使用ZEROFILL时，自动会添加UNSIGNED
);


CREATE TABLE test3(
f1 INT UNSIGNED
);
```

```
USE dbtest1;

# SHOW可以用来查看创建语句(如下)和查看数据库和表(SHOW DATABAS 数据库名;/SHOW TABLE 表名;)
SHOW CREATE TABLE test1;
# DESC只能用来查看表结构
DESC test1;

INSERT INTO test1(f1)
VALUES(1),(-1),(-128),(127);

SELECT * FROM test1;

# 报错：Out of range value for column 'f1' at row 1(TINYINT的范围为-128到127，其他类型同理)
INSERT INTO test1(f1)
VALUES(128);
 
NSERT INTO test2(f1,f2)
VALUES (123,123),(123456,123456); 
 SELECT * FROM test2;

INSERT INTO test2(f3)
VALUES (123),(123456);
SELECT * FROM test2;  # 注意Navicat中不显示自动补零的效果


INSERT INTO test3
VALUES(1234567);
SELECT * FROM test3;
```



### 2.2、浮点类型

浮点类型的类型一共是三种FLOAT、DOUBLE、REAL

![img](截图\12fe69b2fa9f4d388a516c047cba7592.png)

- FLOAT：表示单精度浮点数
- DOUBLE：表示双精度浮点数
- REAL：默认就是DOUBLE

**精度说明**

- 对于浮点类型，在MySQL中单精度值使用4个字节，双精度值使用8个字节
- MySQL允许使用非标准语法：FLOAT(M,D)或DOUBLE(M,D)。M称为精度，D称为标度。(M,D)中 M=整数位+小数位，D=小数位。

**举例**

```mysql
USE dbtest1;

CREATE TABLE test1(
f1 FLOAT,
f2 FLOAT(5,2),
f3 DOUBLE,
f4 DOUBLE(5,2)
);
  
DESC TABLE test1;

CREATE TABLE test2(
f1 DOUBLE
);
```

```mysql
INSERT INTO test1(f1,f2)
VALUES (123.45,123.45);
SELECT * FROM test1;


INSERT INTO test1(f3,f4)
VALUES (123.45,123.456);  # 存在四舍五入，都显示123.45
SELECT * FROM test1;

# 报错：Out of range value for column 'f4' at row 1
INSERT INTO test1(f3,f4)
VALUES(123.45,1234.456);


#报错：Out of range value for column 'f4' at row 1
INSERT INTO test_double1(f3,f4)
VALUES(123.45,999.995); # 存在四舍五入后变成1000.00越界


#测试float和double精度问题
INSERT INTO test2
VALUES (0.47),(0.44),(0.19);

SELECT SUM(f1)   # 和为1.0999999999999999
FROM test2;

SELECT SUM(f1)=1.1  # 结果为0，即不相等
FROM test2;
```

### 2.3、日期与时间类型

> 1. YEAR 类型通常用来表示年
> 2. DATE 类型通常用来表示年、月、日
> 3. TIME 类型通常用来表示时、分、秒
> 4. DATETIME 类型通常用来表示年、月、日、时、分、秒
> 5. TIMESTAMP 类型通常用来表示带时区的年、月、日、时、分、秒

![img](截图\788cf073c2db495fa11e6a6374cb68b0.png)

#### 2.3.1、YEAR类型

- 用来表示年份，其格式是YYYY，最小值是1901，最大值是2155
- 以2位字符串格式表示YEAR类型，最小值为00，最大值为99。
  - （1）当取值为01到69时，表示2001到2069；
  - （2）当取值为70到99时，表示1970到1999；
  - （3）当取值整数的0或00添加的话，那么是0000年；
  - （4）当取值是日期/字符串的’0’添加的话，是2000年。

**举例**

```mysql
CREATE TABLE test1(
f1 YEAR,
F2 YEAR(4)
);
```

```
INSERT INTO test1(f1)
VALUES ('2021'),(2021);
SELECT *FROM test1;  # 显示f1为 2021 2021

INSERT into test1(f1)
VALUES (1905),(2155);
SELECT * FROM test1; #  #显示f1为1905 2155(YEAR类型的最小值为1905，最大值为2155)

#报错：Out of range value for column 'f1' at row 1
INSERT into test1(f1)
VALUES (2156);


#以2位字符串格式表示YEAR类型
#当取值为01到69时，表示2001到2069；
#当取值为70到99时，表示1970到1999
INSERT into test1(f1)
VALUES ('69'),('70');
SELECT * FROM test1; # 显示f1 为 2069  1970


INSERT into test1(f1)
VALUES (0),(00),('00');
SELECT * FROM test1; # 显示f1 为 0 0  2000
```



#### 2.3.2、DATE类型

- 用来表示日期，没有时间类型，格式为YYYY-MM-DD，其中，YYYY表示年份，MM表示月份，DD表示日期
  - （1）以 YYYY-MM-DD 格式或者 YYYYMMDD 格式``表示的字符串日期，其最小取值为1000-01-01，最大取值为9999-12-03。YYYYMMDD格式会被转化为YYYY-MM-DD格式。
  - （2）以 YY-MM-DD 格式或者 YYMMDD 格式表示的字符串日期，此格式中，年份为两位数值或字符串满足YEAR类型的格式条件为：当年份取值为00到69时，会被转化为2000到2069；当年份取值为70到99
    时，会被转化为1970到1999。
  - （3）使用 **CURRENT_DATE()** 或者 **NOW()** 函数，会插入当前系统的日期。

**举例**

```mysql
CREATE TABLE test1(
f1 DATE
);
```

```mysql
INSERT INTO test1
VALUES ('2020-10-01'), ('20201001'),(20201001);  # 推荐格式为 YYYY-MM-DD

INSERT INTO test1
VALUES ('00-01-01'), ('000101'), ('69-10-01'), ('691001'), ('70-01-01'), ('700101'), ('99-01-01'), ('990101');

INSERT INTO test1
VALUES (000101), (690101), (700101), (990101); #存在隐式转换

INSERT INTO test1
VALUES (CURDATE()),(CURRENT_DATE()),(NOW());

SELECT *
FROM test1;
```

#### 2.3.3、TIME类型

- 用来表示时间，不包含日期部分。格式HH:MM:SS格式表示TIME类型，HH表示小时，MM表示分钟，SS表示秒

  > （1）可以使用带有冒号的字符串，比如’ D HH:MM:SS’ 、’ HH:MM:SS ‘、’ HH:MM ‘、’ D HH:MM ‘、’ D HH ‘或’ SS ‘格式，都能被正确地插入TIME类型的字段中。其中D表示天，其最小值为0，最大值为34。如果使用带有D格式的字符串插入TIME类型的字段时，D会被转化为小时，计算格式为D*24+HH。当使用带有冒号并且不带D的字符串表示时间时，表示当天的时间，比如12:10表示12:10:00，而不是00:12:10。
  >
  > （2）可以使用不带有冒号的字符串或者数字，格式为’ HHMMSS '或者 HHMMSS 。如果插入一个不合法的字符串或者数字，MySQL在存储数据时，会将其自动转化为00:00:00进行存储。
  > 比如1210，MySQL会将最右边的两位解析成秒，表示00:12:10，而不是12:10:00。
  >
  > （3）使用 CURRENT_TIME() 或者 NOW() ，会插入当前系统的时间。

**举例**

```mysql
CREATE TABLE test1(
f1 TIME
);
```

```mysql
# 第一个表示两天后12点30分29秒，即需要加上48个小时
# 第二个推荐表示时间格式
# 第三个（带冒号）从左往右表示时分秒
# 第四个同理第一、三个
# 第五个（不带冒号）从右往左表示秒时分
# 第六个同理第五个
INSERT INTO test1
VALUES('2 12:30:29'), ('12:35:29'), ('12:40'), ('2 12:40'),('1 05'), ('45');

INSERT INTO test1
VALUES ('123520'), (124011),(1210);

INSERT INTO test1
VALUES (NOW()), (CURRENT_TIME()),(CURTIME());

SELECT *
FROM test1;
```

#### 2.3.4、DATETIME类型

- 格式是DATE和TIME格式的组合，可以表示为YYYY-MM-DD HH:MM:SS

- 在向DATETIME类型的字段插入数据时，同样需要满足一定的格式条件

  > （1）以 YYYY-MM-DD HH:MM:SS 格式或者 YYYYMMDDHHMMSS 格式的字符串插入DATETIME类型的字段时，最小值为1000-01-01 00:00:00，最大值为9999-12-03 23:59:59。以YYYYMMDDHHMMSS格式的数字插入DATETIME类型的字段时，会被转化为YYYY-MM-DD HH:MM:SS格式。
  >
  > （2）以YY-MM-DD HH:MM:SS格式或者YMMDDHHMMSS格式的字符串插入DATETIME类型的字段时,两位数的年份规则符合YEAR类型的规则，00到69表示2000到2069; 70到99表示1970到1999。
  >
  > （3）使用函数 **CURRENT_TIMESTAMP() 和 NOW()** ，可以向DATETIME类型的字段插入系统的当前日期和时间

**举例**

```
CREATE TABLE test1(
f1 DATETIME
);
```

```
INSERT INTO test1
VALUES ('2021-01-01 06:50:30'), ('20210101065030');

INSERT INTO test1
VALUES ('99-01-01 00:00:00'), ('990101000000'), ('20-01-01 00:00:00'), ('200101000000');

INSERT INTO test1
VALUES (20200101000000), (200101000000), (19990101000000), (990101000000);# 存在隐式转换
 
INSERT INTO test1
VALUES (CURRENT_TIMESTAMP()), (NOW()),(SYSDATE());

SELECT *
FROM test1;
```

#### 2.3.5、TIMESTAMP类型

- TIMESTAMP类型也可以表示日期时间，其显示格式与DATETIME类型相同，都是 YYYY-MM-DD HH:MM:SS，但是TIMESTAMP存储的时间范围比DATETIME要小很多， **只能存储“1970-01-01 00:00:01 UTC”到“2038-01-19 03:14:07 UTC”之间的时间**。其中，UTC表示世界统一时间，也叫作世界标准时间。
- 存储数据的时候需要对当前时间所在的时区进行转换，查询数据的时候再将时间转换回当前的时区。因此，**使用TIMESTAMP存储的同一个时间值，在不同的时区查询时会显示不同的时间**。
- 向TIMESTAMP类型的字段插入数据时，当插入的数据格式满足YY-MM-DD HH:MM:SSYYMMDDHHMMSS时，两位数值的年份同样符合YEAR类型的规则条件，只不过表示的时间范围要小很多。
- 如果向TIMESTAMP类型的字段插入的时间超出了TIMESTAMP类型的范围，则MySQL会抛出错误信息

**举例**

```mysql
CREATE TABLE test1(
f1 TIMESTAMP
);

CREATE TABLE test2(
d1 DATETIME,
d2 TIMESTAMP
);
```

```mysql
INSERT INTO test1
VALUES ('1999-01-01 03:04:50'), ('19990101030405'), ('99-01-01 03:04:05'), ('990101030405');

INSERT INTO test1
VALUES ('2020@01@01@00@00@00'), ('20@01@01@00@00@00'); #使用@ 分隔年月日时分秒

INSERT INTO test1
VALUES (CURRENT_TIMESTAMP()), (NOW());

#Incorrect datetime value(范围：“1970-01-01 00:00:01 UTC”到“2038-01-19 03:14:07 UTC”)
#INSERT INTO test1
#VALUES ('2038-01-20 03:14:07');

SELECT *
FROM test1;

# 对比DATETIME 和 TIMESTAMP
INSERT INTO test2 VALUES('2022-5-8 12:33:00','2022-5-8 12:33:00');

INSERT INTO test2 VALUES(NOW(),NOW());

SELECT * FROM test2;

#修改当前的时区
SET time_zone = '+12:00';

SELECT * FROM test2;
```

TIMESTAMP和DATETIME的区别：

> （1）TIMESTAMP存储空间比较小，表示的日期时间范围也比较小。
> （2）底层存储方式不同，TIMESTAMP底层存储的是毫秒值，距离1970-1-1 0:0:0 0毫秒的毫秒值。
> （3）两个日期比较大小或日期计算时，TIMESTAMP更方便、更快。
> （4）TIMESTAMP和时区有关。TIMESTAMP会根据用户的时区不同，显示不同的结果。而DATETIME则只能反映出插入时当地的时区，其他时区的人查看数据必然会有误差的。

#### 2.3.6、总结

> 用得最多的日期时间类型，就是 DATETIME 。虽然 MySQL 也支持 YEAR（年）、 TIME（时间）、DATE（日期），以及 TIMESTAMP 类型，但是在实际项目中，尽量用 DATETIME 类型。因为这个数据类型包括了完整的日期和时间信息，取值范围也最大，使用起来比较方便。毕竟，如果日期时间信息分散在好几个字段，很不容易记，而且查询的时候，SQL 语句也会更加复杂。
> 此外，一般存注册时间、商品发布时间等，不建议使用DATETIME存储，而是使用时间戳 ，因为DATETIME虽然直观，但不便于计算。

# 八、数据库设计

### 2.1、数据库设计简介

#### 2.1.1、数据库设计设计什么？

- 有哪些表
- 表里有哪些字段
- 表和表之间是什么关系

#### 2.1.2、表关系有哪几种？

- 一对一
- 一对多(多对一)
- 多对多

### 2.2、表关系实现

- 一对多
  - 实现方式：在`多`的一方建立`外键`，指向一的一方的主键

- 多对多
  - 实现方式：建立第三张`中间表`，中间表至少包含`两个外键`，分别`关联两方主键`

- 一对一
  - 实现方式：在任意一方加入外键，关联另一方主键，并且设置外键为`唯一（UNIQUE）`
