# MYSQL数据库学习笔记

[TOC]



## 基本数据类型：

1. 整型：
   1. TINYINT        1 Byte
   2. SMALLINT     2 Byte
   3. MEDIUMINT  3 Byte
   4. INT                  4 Byte
   5. BIGINT            8 Byte
2. 浮点型：
   1. FLOAT
   2. DOUBLE
3. 日期时间类型：
4. 字符型：
   1. CHAR(M)                    M Byte（M在[0,255]区间）
   2. VARCHAR(M)             L+1 Byte（其中L<=M,且M在[0,65535]）
   3. TINYTEXT                   L+1 Byte（L<2^8^）
   4. TEXT                           L+2 Byte（L<2^16^）
   5. MEDIUMTEXT           L+3 Byte（L<2^24^)
   6. LONGTEXT                L+4 Byte（L<2^32^)
   7. ENUM('VALUE1','VALUE2',……)         1~2 Byte ，取决于枚举值的个数（最多65535个）
   8. SET('VALUE1','VALUE2',……)              1,2,3,4或8 Byte ， 取决于集合成员个数（最多64个）

## 最基本的SQL语句

1. 创建（`CREATE`）

   * 数据库

     `CREATE DATABASE data_name`

   * 新表

     1. `CREATE TABLE table_name(col1 type1 [not null][primary key],col2 type2 [not null],……)`
     2. `CRETAE TABLE table_new LIKE table_old`

2. 删除（`DROP`）

   * 数据库

     `DROP DATABASE data_name`

   * 新表

     `DROP TABLE table_name`

3. 修改（`ALTER`）

   * 增加一列

     `ALTER TABLE table_name ADD COLUMN col type`

4. 插入（`INSERT`）一条数据

   `INSERT INTO table_name VALUES(data1,data2,data3,……)`

5. 选取（`SELECT`）数据

   `SELECT col_you_wanna_know FROM table_name WHERE condition`

6. 更新（`UPDATAE）某条数据

   `UPDATE table_name SET col_expression WHERE condition`

   **`eg.`**	`UPDATE student SET age=(YEAR(CURDATE())-YEAR(birth)) WHERE age=0;`



## 过滤数据

1. `WHERE`子句操作符

   |           操作符           |    说明     |
   | :---------------------: | :-------: |
   |            =            |    等于     |
   |          <>或！=          |    不等于    |
   |            <            |    小于     |
   |          <=或!>          | 小于等于（不大于） |
   |            >            |    大于     |
   |          >=或!<          | 大于等于（不小于） |
   | `BETWEEN `  a  `AND`  b |  在a和b之间   |
   |        `IS NULL`        |  为NULL值   |
   |                         |           |

## MYSQL语句积累

1. `USE`语句

   `USE database_name;`		选择数据库

2. SHOW`语句

   `SHOW DATABASES;` 显示所有数据库

   `SHOW TABLES;` 显示该数据库中所有表

3. `DESCRIBE`语句

   `DESCRIBE table_name;` 显示表所有的字段

4. `LOAD DATA LOCAL INFILE 'file_path' INTO TABLE table_name;`

   上述是从本地导入数据到表中，

   文件数据格式：

   ​	一条数据为一行，每个记录用定位符（Tab键）分隔；

   Linux默认语句如上；

   Windows在上述语句加`LINES TERMINATED BY '\r\n';`

   Mac OS加`LINES TERMINATED BY '\r';`

5. `AND OR `逻辑操作符

   `AND`优先级高于`OR`,常用于`WHERE`条件语句中

6. `DISTINCT`关键字

   **`eg.`** `SELECT DISTINCT name FROM employee;`

   将`name`重复条目去掉

7. `ORDER BY`排序输出

   **`eg.`** `SELECT * FROM employee ORDER BY [BINARY] age [DESC/ASC];`

   * 使用`BINARY`强制区分大小写；
   * `DESC`只对该关键字前面的一条起作用，对前面的前面无效，对后面也无效；
   * `ORDER BY`默认升序（`ASC`）排列；

   ```mysql
   SELECT prod_id , prod_price , prod_name FROM products ORDER BY prod_price , prod_name;
   SELECT prod_id , prod_price , prod_name FROM products ORDER BY 2,3;
   #上述两句结果等价
   ```

8. `AS`关键字

   **`eg.`** ` SELECT id,name,(YEAR(CURDATE())-YEAR(birth)) AS age FROM student;`

9. `LIMIT`关键字

   ```mysql
   SELECT prod_name FROM Products LIMIT 5;      --列出前五条prod_name数据

   #带偏移的LIMIT
   SELECT prod_name FROM Products LIMIT 5 OFFSET 4;    --从第4行起的5条数据
   SELECT prod_name FROM Products LIMIT 4,5;           --与上一行语句等价
   ```

