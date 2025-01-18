---
title: MySQL语法
tags: [MySQL]
copyright: false
typora-root-url: MySQL语法
date: 2023-02-14 10:12:43
categories: 技术连连看
description: 又一次连删除语句都要百度了，下定决心自己总结一下方便查询。(好吧其实就是把网上的复制一遍而已。。)
cover: cover.jpg
---

## MySQL数据库的基本操作

### MySQL 连接数据库

~~~mysql
mysql -u root -p
Enter password:******
~~~

### MySQL 退出数据库

```mysql
exit
```

### MySQL查看数据库

~~~mysql
SHOW DATABASES [LIKE '数据库名'];
~~~

语法说明如下：

- LIKE 从句是可选项，用于匹配指定的数据库名称。LIKE 从句可以部分匹配，也可以完全匹配。
- 数据库名由单引号`' '`包围。

### MySQL 创建数据库

~~~mysql
CREATE DATABASE [IF NOT EXISTS] <数据库名>
[[DEFAULT] CHARACTER SET <字符集名>] 
[[DEFAULT] COLLATE <校对规则名>];
~~~

`[ ]`中的内容是可选的。语法说明如下：

- <数据库名>：创建数据库的名称。MySQL 的数据存储区将以目录方式表示 MySQL 数据库，因此数据库名称必须符合操作系统的文件夹命名规则，不能以数字开头，尽量要有实际意义。注意在 MySQL 中不区分大小写。
- IF NOT EXISTS：在创建数据库之前进行判断，只有该数据库目前尚不存在时才能执行操作。此选项可以用来避免数据库已经存在而重复创建的错误。
- [DEFAULT] CHARACTER SET：指定数据库的字符集。指定字符集的目的是为了避免在数据库中存储的数据出现乱码的情况。如果在创建数据库时不指定字符集，那么就使用系统的默认字符集。
- [DEFAULT] COLLATE：指定字符集的默认校对规则。

### MySQL 修改数据库

~~~mysql
ALTER DATABASE [数据库名] { 
[ DEFAULT ] CHARACTER SET <字符集名> |
[ DEFAULT ] COLLATE <校对规则名>}
~~~

语法说明如下：

- ALTER DATABASE 用于更改数据库的全局特性。
- 使用 ALTER DATABASE 需要获得数据库 ALTER 权限。
- 数据库名称可以忽略，此时语句对应于默认数据库。
- CHARACTER SET 子句用于更改默认的数据库字符集。

### MySQL 删除数据库

```mysql
DROP DATABASE [ IF EXISTS ] <数据库名>
```

语法说明如下：

- <数据库名>：指定要删除的数据库名。
- IF EXISTS：用于防止当数据库不存在时发生错误。
- DROP DATABASE：删除数据库中的所有表格并同时删除数据库。使用此语句时要非常小心，以免错误删除。如果要使用 DROP DATABASE，需要获得数据库 DROP 权限。

### MySQL 选择数据库

```mysql
USE <数据库名>
```

该语句可以通知 MySQL 把`<数据库名>`所指示的数据库作为当前数据库。该数据库保持为默认数据库，直到语段的结尾，或者直到遇见一个不同的 USE 语句。 只有使用 USE 语句来指定某个数据库作为当前数据库之后，才能对该数据库及其存储的数据对象执行操作。

### MySQL注释

#### MySQL 单行注释

单行注释可以使用`#`注释符，`#`注释符后直接加注释内容。格式如下：

~~~mysql
#注释内容
~~~

#### MySQL 多行注释

多行注释使用`/* */`注释符。`/*`用于注释内容的开头，`*/`用于注释内容的结尾。多行注释格式如下：

~~~mysql
/*
  第一行注释内容
  第二行注释内容
*/
~~~

## MySQL 数据类型和存储引擎

### MySQL 整数类型

| 类型名称      | 说明                                      | 存储需求 |
| ------------- | ----------------------------------------- | -------- |
| TINYINT       | -128〜127                                 | 1个字节  |
| SMALLINT      | -32768〜32767                             | 2个宇节  |
| MEDIUMINT     | -8388608〜8388607                         | 3个字节  |
| INT (INTEGHR) | -2147483648〜2147483647                   | 4个字节  |
| BIGINT        | -9223372036854775808〜9223372036854775807 | 8个字节  |

### MySQL 小数类型

| 类型名称            | 说明               | 存储需求   |
| ------------------- | ------------------ | ---------- |
| FLOAT               | 单精度浮点数       | 4 个字节   |
| DOUBLE              | 双精度浮点数       | 8 个字节   |
| DECIMAL (M, D)，DEC | 压缩的“严格”定点数 | M+2 个字节 |

### MySQL 日期和时间类型

| 类型名称  | 日期格式            | 日期范围                                          | 存储需求 |
| --------- | ------------------- | ------------------------------------------------- | -------- |
| YEAR      | YYYY                | 1901 ~ 2155                                       | 1 个字节 |
| TIME      | HH:MM:SS            | -838:59:59 ~ 838:59:59                            | 3 个字节 |
| DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-3                            | 3 个字节 |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59         | 8 个字节 |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1980-01-01 00:00:01 UTC ~ 2040-01-19 03:14:07 UTC | 4 个字节 |

### MySQL 字符串类型

| 类型名称   | 说明                                         | 存储需求                                                   |
| ---------- | -------------------------------------------- | ---------------------------------------------------------- |
| CHAR(M)    | 固定长度非二进制字符串                       | M 字节，1<=M<=255                                          |
| VARCHAR(M) | 变长非二进制字符串                           | L+1字节，在此，L< = M和 1<=M<=255                          |
| TINYTEXT   | 非常小的非二进制字符串                       | L+1字节，在此，L<2^8                                       |
| TEXT       | 小的非二进制字符串                           | L+2字节，在此，L<2^16                                      |
| MEDIUMTEXT | 中等大小的非二进制字符串                     | L+3字节，在此，L<2^24                                      |
| LONGTEXT   | 大的非二进制字符串                           | L+4字节，在此，L<2^32                                      |
| ENUM       | 枚举类型，只能有一个枚举字符串值             | 1或2个字节，取决于枚举值的数目 (最大值为65535)             |
| SET        | 一个设置，字符串对象可以有零个或 多个SET成员 | 1、2、3、4或8个字节，取决于集合 成员的数量（最多64个成员） |

### MySQL 二进制类型

| 类型名称       | 说明                 | 存储需求               |
| -------------- | -------------------- | ---------------------- |
| BIT(M)         | 位字段类型           | 大约 (M+7)/8 字节      |
| BINARY(M)      | 固定长度二进制字符串 | M 字节                 |
| VARBINARY (M)  | 可变长度二进制字符串 | M+1 字节               |
| TINYBLOB (M)   | 非常小的BLOB         | L+1 字节，在此，L<2^8  |
| BLOB (M)       | 小 BLOB              | L+2 字节，在此，L<2^16 |
| MEDIUMBLOB (M) | 中等大小的BLOB       | L+3 字节，在此，L<2^24 |
| LONGBLOB (M)   | 非常大的BLOB         | L+4 字节，在此，L<2^32 |

## MySQL 数据表的基本操作

### MySQL 创建数据表

~~~mysql
CREATE TABLE <表名> ([表定义选项])[表选项][分区选项];
~~~

其中，`[表定义选项]`的格式为：

~~~mysql
<列名1> <类型1> [,…] <列名n> <类型n>
~~~

CREATE TABLE 语句的主要语法及使用说明如下：

- CREATE TABLE：用于创建给定名称的表，必须拥有表CREATE的权限。
- <表名>：指定要创建表的名称，在 CREATE TABLE 之后给出，必须符合标识符命名规则。表名称被指定为 db_name.tbl_name，以便在特定的数据库中创建表。无论是否有当前数据库，都可以通过这种方式创建。在当前数据库中创建表时，可以省略 db-name。如果使用加引号的识别名，则应对数据库和表名称分别加引号。例如，'mydb'.'mytbl' 是合法的，但 'mydb.mytbl' 不合法。
- <表定义选项>：表创建定义，由列名（col_name）、列的定义（column_definition）以及可能的空值说明、完整性约束或表索引组成。
- 默认的情况是，表被创建到当前的数据库中。若表已存在、没有当前数据库或者数据库不存在，则会出现错误。

### MySQL 修改数据表

~~~mysql
ALTER TABLE <表名> [修改选项]
~~~

修改选项的语法格式如下：

~~~mysql
{ ADD COLUMN <列名> <类型>
| CHANGE COLUMN <旧列名> <新列名> <新列类型>
| ALTER COLUMN <列名> { SET DEFAULT <默认值> | DROP DEFAULT }
| MODIFY COLUMN <列名> <类型>
| DROP COLUMN <列名>
| RENAME TO <新表名>
| CHARACTER SET <字符集名>
| COLLATE <校对规则名> }
~~~

#### 修改表名

~~~mysql
ALTER TABLE <旧表名> RENAME [TO] <新表名>；
~~~

#### 修改表字符集

~~~mysql
ALTER TABLE 表名 [DEFAULT] CHARACTER SET <字符集名> [DEFAULT] COLLATE <校对规则名>;
~~~

### MySQL 修改/删除字段

#### 修改字段名称

~~~mysql
ALTER TABLE <表名> CHANGE <旧字段名> <新字段名> <新数据类型>；
~~~

其中：

- 旧字段名：指修改前的字段名；
- 新字段名：指修改后的字段名；
- 新数据类型：指修改后的数据类型，如果不需要修改字段的数据类型，可以将新数据类型设置成与原来一样，但数据类型不能为空。

#### 修改字段数据类型

~~~mysql
ALTER TABLE <表名> MODIFY <字段名> <数据类型>
~~~

其中：

- 表名：指要修改数据类型的字段所在表的名称；
- 字段名：指需要修改的字段；
- 数据类型：指修改后字段的新数据类型。

#### 删除字段

~~~mysql
ALTER TABLE <表名> DROP <字段名>；
~~~

### MySQL 删除数据表

~~~mysql
DROP TABLE [IF EXISTS] 表名1 [ ,表名2, 表名3 ...]
~~~

对语法格式的说明如下：

- `表名1, 表名2, 表名3 ...`表示要被删除的数据表的名称。DROP TABLE 可以同时删除多个表，只要将表名依次写在后面，相互之间用逗号隔开即可。
- IF EXISTS 用于在删除数据表之前判断该表是否存在。如果不加 IF EXISTS，当数据表不存在时 MySQL 将提示错误，中断 SQL 语句的执行；加上 IF EXISTS 后，当数据表不存在时 SQL 语句可以顺利执行，但是会发出警告（warning）。

### MySQL 查看表结构命令

#### DESCRIBE：以表格的形式展示表结构

~~~mysql
DESCRIBE <表名>; 
或简写为 
DESC <表名>;
~~~

#### SHOW CREATE TABLE：以SQL语句的形式展示表结构

~~~mysql
SHOW CREATE TABLE <表名>;
~~~

### MySQL 数据表添加字段

#### 在末尾添加字段

~~~mysql
ALTER TABLE <表名> ADD <新字段名><数据类型>[约束条件];
~~~

对语法格式的说明如下：                    

- <表名> 为数据表的名字；
- <新字段名> 为所要添加的字段的名字；
- <数据类型> 为所要添加的字段能存储数据的数据类型；
- [约束条件] 是可选的，用来对添加的字段进行约束。

#### 在开头添加字段

~~~mysql
ALTER TABLE <表名> ADD <新字段名> <数据类型> [约束条件] FIRST;
~~~

#### 在中间位置添加字段

~~~mysql
ALTER TABLE <表名> ADD <新字段名> <数据类型> [约束条件] AFTER <已经存在的字段名>;
~~~

## MySQL约束、函数

### 在创建表时设置主键约束

#### 设置单字段主键

~~~mysql
<字段名> <数据类型> PRIMARY KEY [默认值]
~~~

#### 在创建表时设置联合主键

~~~mysql
PRIMARY KEY [字段1，字段2，…,字段n]
~~~

#### 在修改表时添加主键约束

~~~mysql
ALTER TABLE <数据表名> ADD PRIMARY KEY(<字段名>);
~~~

#### 删除主键约束

~~~mysql
ALTER TABLE <数据表名> DROP PRIMARY KEY;
~~~

### MySQL 主键自增长

~~~mysql
字段名 数据类型 AUTO_INCREMENT
~~~

- 默认情况下，AUTO_INCREMENT 的初始值是 1，每新增一条记录，字段值自动加 1。
- 一个表中只能有一个字段使用 AUTO_INCREMENT 约束，且该字段必须有唯一索引，以避免序号重复（即为主键或主键的一部分）。
- AUTO_INCREMENT 约束的字段必须具备 NOT NULL 属性。
- AUTO_INCREMENT 约束的字段只能是整数类型（TINYINT、SMALLINT、INT、BIGINT 等）。
- AUTO_INCREMENT 约束字段的最大值受该字段的数据类型约束，如果达到上限，AUTO_INCREMENT 就会失效。

### MySQL 默认值

#### 在创建表时设置默认值约束

~~~mysql
<字段名> <数据类型> DEFAULT <默认值>;
~~~

#### 在修改表时添加默认值约束

~~~mysql
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <数据类型> DEFAULT <默认值>;
~~~

#### 删除默认值约束

~~~mysql
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <字段名> <数据类型> DEFAULT NULL;
~~~

### MySQL 非空约束

### 在创建表时设置非空约束

~~~mysql
<字段名> <数据类型> NOT NULL;
~~~

### 在修改表时添加非空约束

~~~mysql
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名>
<字段名> <数据类型> NOT NULL;
~~~

#### 删除非空约束

~~~mysql
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <字段名> <数据类型> NULL;
~~~

### MySQL 查看表中的约束

~~~mysql
SHOW CREATE TABLE <数据表名>;
~~~

### MySQL IN 和 NOT IN用法详解

[MySQL](http://c.biancheng.net/mysql/) 中的 IN 运算符用来判断表达式的值是否位于给出的列表中；如果是，返回值为 1，否则返回值为 0。

NOT IN 的作用和 IN 恰好相反，NOT IN 用来判断表达式的值是否不存在于给出的列表中；如果不是，返回值为 1，否则返回值为 0。

IN 和 NOT IN 的语法格式如下：

~~~mysql
expr IN ( value1, value2, value3 ... valueN )
expr NOT IN ( value1, value2, value3 ... valueN )
~~~

expr 表示要判断的表达式，value1, value2, value3 ... valueN 表示列表中的值。MySQL 会将 expr 的值和列表中的值逐一对比。

## MySQL 操作表中数据

### MySQL 查询数据表

```mysql
SELECT
{* | <字段列名>}
[
FROM <表 1>, <表 2>…
[WHERE <表达式>
[GROUP BY <group by definition>
[HAVING <expression> [{<operator> <expression>}…]]
[ORDER BY <order by definition>]
[LIMIT[<offset>,] <row count>]
]
```

其中，各条子句的含义如下：

- `{*|<字段列名>}`包含星号通配符的字段列表，表示所要查询字段的名称。
- `<表 1>，<表 2>…`，表 1 和表 2 表示查询数据的来源，可以是单个或多个。
- `WHERE <表达式>`是可选项，如果选择该项，将限定查询数据必须满足该查询条件。
- `GROUP BY< 字段 >`，该子句告诉 MySQL 如何显示查询出来的数据，并按照指定的字段分组。
- `[ORDER BY< 字段 >]`，该子句告诉 MySQL 按什么样的顺序显示查询出来的数据，可以进行的排序有升序（ASC）和降序（DESC），默认情况下是升序。
- `[LIMIT[<offset>，]<row count>]`，该子句告诉 MySQL 每次显示查询出来的数据条数。

### MySQL 去重

~~~mysql
SELECT DISTINCT <字段名> FROM <表名>;
~~~

其中，“字段名”为需要消除重复记录的字段名称，多个字段时用逗号隔开。

使用 DISTINCT 关键字时需要注意以下几点：

- DISTINCT 关键字只能在 SELECT 语句中使用。
- 在对一个或多个字段去重时，DISTINCT 关键字必须在所有字段的最前面。
- 如果 DISTINCT 关键字后有多个字段，则会对多个字段进行组合去重，也就是说，只有多个字段组合起来完全是一样的情况下才会被去重。

### MySQL 设置别名

#### 为表指定别名

~~~mysql
<表名> [AS] <别名>
~~~

其中各子句的含义如下：

- `<表名>`：数据库中存储的数据表的名称。
- `<别名>`：查询时指定的表的新名称。
- `AS`关键字可以省略，省略后需要将表名和别名用空格隔开。

#### 为字段指定别名

~~~mysql
<字段名> [AS] <别名>
~~~

其中，各子句的语法含义如下：

- `<字段名>`：为数据表中字段定义的名称。
- `<字段别名>`：字段新的名称。
- `AS`关键字可以省略，省略后需要将字段名和别名用空格隔开。

### MySQL 限制查询结果的条数

#### 指定初始位置

~~~mysql
LIMIT 初始位置，记录数
~~~

#### 不指定初始位置

~~~mysql
LIMIT 记录数
~~~

#### LIMIT和OFFSET组合使用

~~~mysql
LIMIT 记录数 OFFSET 初始位置  （效果与指定初始位置相同）
~~~

### MySQL 对查询结果排序

~~~mysql
ORDER BY <字段名> [ASC|DESC]
~~~

语法说明如下。

- 字段名：表示需要排序的字段名称，多个字段时用逗号隔开。
- ASC|DESC：`ASC`表示字段按升序排序；`DESC`表示字段按降序排序。其中`ASC`为默认值。

使用 ORDER BY 关键字应该注意以下几个方面：

- ORDER BY 关键字后可以跟子查询（关于子查询后面教程会详细讲解，这里了解即可）。
- 当排序的字段中存在空值时，ORDER BY 会将该空值作为最小值来对待。
- ORDER BY 指定多个字段进行排序时，MySQL 会按照字段的顺序从左到右依次进行排序。

### MySQL 条件查询

~~~mysql
WHERE 查询条件
~~~

查询条件可以是：

- 带比较运算符和逻辑运算符的查询条件
- 带 BETWEEN AND 关键字的查询条件
- 带 IS NULL 关键字的查询条件
- 带 IN 关键字的查询条件
- 带 LIKE 关键字的查询条件

### MySQL 模糊查询

~~~mysql
[NOT] LIKE  '字符串'
~~~

其中：

- NOT ：可选参数，字段中的内容与指定的字符串不匹配时满足条件。
- 字符串：指定用来匹配的字符串。“字符串”可以是一个很完整的字符串，也可以包含通配符。

#### 带有“%”通配符的查询

“%”是 MySQL 中最常用的通配符，它能代表任何长度的字符串，字符串的长度可以为 0。例如，`a%b`表示以字母 a 开头，以字母 b 结尾的任意长度的字符串。该字符串可以代表 ab、acb、accb、accrb 等字符串。

#### 带有“_”通配符的查询

“_”只能代表单个字符，字符的长度不能为 0。例如，`a_b`可以代表 acb、adb、aub 等字符串。

#### LIKE 区分大小写

默认情况下，LIKE 关键字匹配字符的时候是不区分大小写的。如果需要区分大小写，可以加入 BINARY 关键字。

~~~mysql
SELECT name FROM tb_students_info WHERE name LIKE BINARY 't%';
~~~

### MySQL 范围查询

~~~mysql
[NOT] BETWEEN 取值1 AND 取值2
~~~

其中：

- NOT：可选参数，表示指定范围之外的值。如果字段值不满足指定范围内的值，则这些记录被返回。
- 取值1：表示范围的起始值。
- 取值2：表示范围的终止值。

### MySQL 空值查询

~~~mysql
IS [NOT] NULL
~~~

其中，“NOT”是可选参数，表示字段值不是空值时满足条件。

### MySQL 分组查询

#### GROUP BY单独使用

~~~mysql
GROUP BY  <字段名>
~~~

#### GROUP BY 与 GROUP_CONCAT() 

GROUP BY 关键字可以和 GROUP_CONCAT() 函数一起使用。GROUP_CONCAT() 函数会把每个分组的字段值都显示出来。

~~~mysql
SELECT `sex`, GROUP_CONCAT(name) FROM tb_students_info 
GROUP BY sex;
~~~

结果如下：

> ```
> +------+----------------------------+
> | sex  | GROUP_CONCAT(name)         |
> +------+----------------------------+
> | 女   | Henry,Jim,John,Thomas,Tom  |
> | 男   | Dany,Green,Jane,Lily,Susan |
> +------+----------------------------+
> ```

#### GROUP BY 与聚合函数

聚合函数包括 COUNT()，SUM()，AVG()，MAX() 和 MIN()。其中，COUNT() 用来统计记录的条数；SUM() 用来计算字段值的总和；AVG() 用来计算字段值的平均值；MAX() 用来查询字段的最大值；MIN() 用来查询字段的最小值。

~~~mysql
SELECT sex,COUNT(sex) FROM tb_students_info 
GROUP BY sex;
~~~

结果如下：

> ```
> +------+------------+
> | sex  | COUNT(sex) |
> +------+------------+
> | 女   |          5 |
> | 男   |          5 |
> +------+------------+
> ```

### MySQL 过滤分组

~~~mysql
HAVING <查询条件>
~~~

HAVING 关键字和 WHERE 关键字都可以用来过滤数据，且 HAVING 支持 WHERE 关键字中所有的操作符和语法。

但是 WHERE 和 HAVING 关键字也存在以下几点差异：

- 一般情况下，WHERE 用于过滤数据行，而 HAVING 用于过滤分组。
- WHERE 查询条件中不可以使用聚合函数，而 HAVING 查询条件中可以使用聚合函数。
- WHERE 在数据分组前进行过滤，而 HAVING 在数据分组后进行过滤 。
- WHERE 针对数据库文件进行过滤，而 HAVING 针对查询结果进行过滤。也就是说，WHERE 根据数据表中的字段直接进行过滤，而 HAVING 是根据前面已经查询出的字段进行过滤。
- WHERE 查询条件中不可以使用字段别名，而 HAVING 查询条件中可以使用字段别名。

### MySQL 交叉连接

交叉连接（CROSS JOIN）一般用来返回连接表的笛卡尔积。

交叉连接的语法格式如下：

~~~mysql
SELECT <字段名> FROM <表1> CROSS JOIN <表2> [WHERE子句]
或
SELECT <字段名> FROM <表1>, <表2> [WHERE子句] 
~~~

语法说明如下：

- 字段名：需要查询的字段名称。
- <表1><表2>：需要交叉连接的表名。
- WHERE 子句：用来设置交叉连接的查询条件。

> ## 笛卡尔积
>
> 笛卡尔积（Cartesian product）是指两个集合 X 和 Y 的乘积。
>
> 例如，有 A 和 B 两个集合，它们的值如下：
>
> A = {1,2}
> B = {3,4,5}
>
> 集合 A×B 和 B×A 的结果集分别表示为：
>
> A×B={(1,3), (1,4), (1,5), (2,3), (2,4), (2,5) };
> B×A={(3,1), (3,2), (4,1), (4,2), (5,1), (5,2) };
>
> 以上 A×B 和 B×A 的结果就叫做两个集合的笛卡尔积。
>
> 并且，从以上结果我们可以看出：
>
> - 两个集合相乘，不满足交换率，即 A×B≠B×A。
> - A 集合和 B 集合的笛卡尔积是 A 集合的元素个数 × B 集合的元素个数。
>
>
> 多表查询遵循的算法就是以上提到的笛卡尔积，表与表之间的连接可以看成是在做乘法运算。在实际应用中，应避免使用笛卡尔积，因为笛卡尔积中容易存在大量的不合理数据，简单来说就是容易导致查询结果重复、混乱。

### MySQL 内连接

内连接使用 **INNER JOIN** 关键字连接两张表，并使用 ON 子句来设置连接条件。如果没有连接条件，INNER JOIN 和 CROSS JOIN 在语法上是等同的，两者可以互换。

~~~mysql
SELECT <字段名> FROM <表1> INNER JOIN <表2> [ON子句]
~~~

语法说明如下。

- 字段名：需要查询的字段名称。
- <表1><表2>：需要内连接的表名。
- INNER JOIN ：内连接中可以省略 INNER 关键字，只用关键字 JOIN。
- ON 子句：用来设置内连接的连接条件。

### MySQL 外连接

#### 左外连接

~~~mysql
SELECT <字段名> FROM <表1> LEFT OUTER JOIN <表2> <ON子句>
~~~

语法说明如下。

- 字段名：需要查询的字段名称。
- <表1><表2>：需要左连接的表名。
- LEFT OUTER JOIN：左连接中可以省略 OUTER 关键字，只使用关键字 LEFT JOIN。
- ON 子句：用来设置左连接的连接条件，不能省略。

上述语法中，“表1”为基表，“表2”为参考表。左连接查询时，可以查询出“表1”中的所有记录和“表2”中匹配连接条件的记录。如果“表1”的某行在“表2”中没有匹配行，那么在返回结果中，“表2”的字段值均为空值（NULL）。

#### 右外连接

~~~mysql
SELECT <字段名> FROM <表1> RIGHT OUTER JOIN <表2> <ON子句>
~~~

语法说明如下。

- 字段名：需要查询的字段名称。
- <表1><表2>：需要右连接的表名。
- RIGHT OUTER JOIN：右连接中可以省略 OUTER 关键字，只使用关键字 RIGHT JOIN。
- ON 子句：用来设置右连接的连接条件，不能省略。

与左连接相反，右连接以“表2”为基表，“表1”为参考表。右连接查询时，可以查询出“表2”中的所有记录和“表1”中匹配连接条件的记录。如果“表2”的某行在“表1”中没有匹配行，那么在返回结果中，“表1”的字段值均为空值（NULL）。

### MySQL 子查询

~~~mysql
WHERE <表达式> <操作符> (子查询)
~~~

其中，操作符可以是比较运算符和 IN、NOT IN、EXISTS、NOT EXISTS 等关键字。

**1）IN | NOT IN**

当表达式与子查询返回的结果集中的某个值相等时，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回值正好相反。

**2）EXISTS | NOT EXISTS**

用于判断子查询的结果集是否为空，若子查询的结果集不为空，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回的值正好相反。

~~~mysql
SELECT name FROM tb_students_info 
WHERE course_id IN (SELECT id FROM tb_course WHERE course_name = 'Java');
~~~

结果如下：

> ```
> +-------+
> | name  |
> +-------+
> | Dany  |
> | Henry |
> +-------+
> ```

### MySQL 插入数据

#### 基本语法

~~~mysql
INSERT INTO <表名> [ <列名1> [ , … <列名n>] ]
VALUES (值1) [… , (值n) ];
~~~

语法说明如下。

- `<表名>`：指定被操作的表名。
- `<列名>`：指定需要插入数据的列名。若向表中的所有列插入数据，则全部的列名均可以省略，直接采用 INSERT<表名>VALUES(…) 即可。
- `VALUES` 或 `VALUE` 子句：该子句包含要插入的数据清单。数据清单中数据的顺序要和列的顺序相对应。

~~~mysql
INSERT INTO <表名>
SET <列名1> = <值1>,
        <列名2> = <值2>,
        …
~~~

#### 向表中的全部字段添加值

~~~mysql
INSERT INTO tb_courses
(course_id,course_name,course_grade,course_info)
VALUES(1,'Network',3,'Computer Network');
~~~

#### 向表中指定字段添加值

~~~mysql
INSERT INTO tb_courses
(course_name,course_grade,course_info)
VALUES('System',3,'Operation System');
~~~

#### 使用 INSERT INTO…FROM 语句复制表数据

~~~mysql
INSERT INTO tb_courses_new
(course_id,course_name,course_grade,course_info)
SELECT course_id,course_name,course_grade,course_info
FROM tb_courses;
~~~

### MySQL 修改数据

#### UPDATE 语句的基本语法

~~~mysql
UPDATE <表名> SET 字段 1=值 1 [,字段 2=值 2… ] [WHERE 子句 ]
[ORDER BY 子句] [LIMIT 子句]
~~~

语法说明如下：

- `<表名>`：用于指定要更新的表名称。
- `SET` 子句：用于指定表中要修改的列名及其列值。其中，每个指定的列值可以是表达式，也可以是该列对应的默认值。如果指定的是默认值，可用关键字 DEFAULT 表示列值。
- `WHERE` 子句：可选项。用于限定表中要修改的行。若不指定，则修改表中所有的行。
- `ORDER BY` 子句：可选项。用于限定表中的行被修改的次序。
- `LIMIT` 子句：可选项。用于限定被修改的行数。

~~~mysql
UPDATE tb_courses_new
SET course_name='DB',course_grade=3.5
WHERE course_id=2;
~~~

### MySQL 删除数据

#### 删除单个表中的数据

~~~mysql
DELETE FROM <表名> [WHERE 子句] [ORDER BY 子句] [LIMIT 子句]
~~~

语法说明如下：

- `<表名>`：指定要删除数据的表名。
- `ORDER BY` 子句：可选项。表示删除时，表中各行将按照子句中指定的顺序进行删除。
- `WHERE` 子句：可选项。表示为删除操作限定删除条件，若省略该子句，则代表删除该表中的所有行。
- `LIMIT` 子句：可选项。用于告知服务器在控制命令被返回到客户端前被删除行的最大值。


**注意：在不使用 WHERE 条件的时候，将删除所有数据。**

### MySQL 清空表数据

**TRUNCATE** 关键字用于完全清空一个表

~~~msyql
TRUNCATE [TABLE] 表名
~~~

其中，TABLE 关键字可省略。

#### TRUNCATE 和 DELETE 的区别

从逻辑上说，TRUNCATE 语句与 DELETE 语句作用相同，但是在某些情况下，两者在使用上有所区别。

- DELETE 是 DML 类型的语句；TRUNCATE 是 DDL 类型的语句。它们都用来清空表中的数据。
- DELETE 是逐行一条一条删除记录的；TRUNCATE 则是直接删除原来的表，再重新创建一个一模一样的新表，而不是逐行删除表中的数据，执行数据比 DELETE 快。因此需要删除表中全部的数据行时，尽量使用 TRUNCATE 语句， 可以缩短执行时间。
- DELETE 删除数据后，配合事件回滚可以找回数据；TRUNCATE 不支持事务的回滚，数据删除后无法找回。
- DELETE 删除数据后，系统不会重新设置自增字段的计数器；TRUNCATE 清空表记录后，系统会重新设置自增字段的计数器。
- DELETE 的使用范围更广，因为它可以通过 WHERE 子句指定条件来删除部分数据；而 TRUNCATE 不支持 WHERE 子句，只能删除整体。
- DELETE 会返回删除数据的行数，但是 TRUNCATE 只会返回 0，没有任何意义。

***

**THE END**
