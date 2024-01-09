# SQL 和 NoSQL
## SQL（Structured Query Language）
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1672667277010-72b0f9a5-80ba-43e1-8b23-87af61380895.png#averageHue=%23fcfbfb&clientId=u06c26f77-5d02-4&from=paste&height=516&id=tjFwo&originHeight=1031&originWidth=1647&originalType=binary&ratio=1&rotation=0&showTitle=false&size=143712&status=done&style=none&taskId=ua82dfc20-713a-449f-8932-4a43bc705e1&title=&width=823.5)
结构型查询语言
关系型数据库指的是使用关系模型（二维表格模型 — 由行和列组成）来组织数据的数据库，常用于存储结构化的数据。
关系模型可以简单理解为二维表格模型，而一个关系型数据库就是由二维表及其之间的关系组成的一个数据组织。
### 常见关系型数据库管理系统(ORDBMS)：
> Object Relational Database Management System：对象关系数据库管理系统

1. Oracle
2. MySql
3. Microsoft SQL Server
4. SQLite
5. PostgreSQL
6. IBM DB2
### 关系型数据库的优势

1. 采用二维表结构非常贴近正常开发逻辑（关系型数据模型相对层次型数据模型和网状型数据模型等其他模型来说更容易理解）；
2. 支持通用的SQL（结构化查询语言）语句；
3. 丰富的完整性大大减少了数据冗余和数据不一致的问题。并且全部由表结构组成，文件格式一致；
4. 可以用SQL句子多个表之间做非常繁杂的查询；
5. 关系型数据库提供对事务的支持，能保证系统中事务的正确执行，同时提供事务的恢复、回滚、并发控制和死锁问题的解决。
6. 数据存储在磁盘中，安全可靠。
### 关系型数据库存在的不足
**随着互联网企业的不断发展，数据日益增多，因此关系型数据库面对海量的数据会存在很多的不足。**

1. 高并发读写能力差：网站类用户的并发性访问非常高，而一台数据库的最大连接数有限，且硬盘 I/O 有限，不能满足很多人同时连接。
2. 海量数据情况下读写效率低：对大数据量的表进行读写操作时，需要等待较长的时间等待响应。
3. 可扩展性不足：不像web server和app server那样简单的添加硬件和服务节点来拓展性能和负荷工作能力。
4. 数据模型灵活度低：关系型数据库的数据模型定义严格，无法快速容纳新的数据类型（需要提前知道需要存储什么样类型的数据）。



## NoSQL（Not Only SQL）
非关系型数据库又被称为 NoSQL（Not Only SQL )，意为不仅仅是 SQL。非关系型数据库中的数据通常以对象的形式存储在，而对象之间的关系通过每个对象自身的属性来决定，常用于存储非结构化的数据。
非关系型数据库是分布式的、非关系型的。
### 常见的NOSQL数据库

1. 键值数据库：Redis、Memcached、Riak
2. 列族数据库：Bigtable、HBase、Cassandra
3. 文档数据库：MongoDB、CouchDB、MarkLogic
4. 图形数据库：Neo4j、InfoGrid
### 非关系型数据库的优势：

1. 非关系型数据库存储数据的格式可以是 key-value 形式、文档形式、图片形式等。使用灵活，应用场景广泛，而关系型数据库则只支持基础类型。
2. 速度快，效率高。 NoSQL 可以使用硬盘或者随机存储器作为载体，而关系型数据库只能使用硬盘。
3. 海量数据的维护和处理非常轻松，成本低。
4. 非关系型数据库具有扩展简单、高并发、高稳定性、成本低廉的优势。
5. 可以实现数据的分布式处理。
### 非关系型数据库存在的不足:

1. 非关系型数据库暂时不提供 SQL 支持，学习和使用成本较高。
2. 非关系数据库没有事务处理，无法保证数据的完整性和安全性。适合处理海量数据，但是不一定安全。
3. 功能没有关系型数据库完善。
4. 复杂表关联查询不容易实现。

### 术语
| SQL | MongoDB | 说明 |
| --- | --- | --- |
| database | database | 数据库 |
| table | collection | 数据库表/集合 |
| schema | schema | 模式，关于数据库和表的布局及特性的信息。模式定义了数据在表中如何存储，包含存储什么样的数据，数据如何分解，各部分信息如何命名等信息。数据库和表都有模式。 |
| row | document | 数据记录行/文档 |
| column | field | 数据字段/域 |
| index | index | 索引 |
| table joins | 
 | 表连接，MongoDB不支持 |
| primary key | primary key | 主键，MongoDB自动将_id字段设为主键 |


#### ORM
Object Relational Mapping
#### ODM
Object Document Mapping

#### 笛卡尔积
在数学中，两个集合 X 和 Y 的**笛卡儿积**（英语：**Cartesian product**），又称[直积](https://zh.wikipedia.org/wiki/%E7%9B%B4%E7%A7%AF)，在集合论中表示为，是所有可能的[有序对](https://zh.wikipedia.org/wiki/%E6%9C%89%E5%BA%8F%E5%AF%B9)组成的集合，其中有序对的第一个对象是 X 的成员，第二个对象是 Y 的成员。
A×B={(x, y) | x ∈ A ∧ y ∈ B}
例如，A={a,b}, B={0,1,2}，则
A×B={(a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b, 2)}
B×A={(0, a), (0, b), (1, a), (1, b), (2, a), (2, b)}

![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1673315817000-ee2f6cc4-7c15-4414-a90f-e4ec20ae3db0.png#averageHue=%23008000&clientId=u3b8b6b63-da2d-4&from=paste&height=330&id=uac3ba8cd&originHeight=2190&originWidth=2880&originalType=binary&ratio=1&rotation=0&showTitle=false&size=356098&status=done&style=none&taskId=uf0194a48-eb09-4078-bb32-a5aea530108&title=&width=434)

## SQL 分类
### 数据定义语言（DDL）
Data Definition Language，是 SQL 语言集中负责数据结构定义与数据库对象定义的语言。
DDL 的主要功能是**定义数据库对象**。
DDL 的核心指令是 CREATE、ALTER、DROP。
### 数据操作语言（DML）
Data Manipulation Language，是用于数据库操作，对数据库其中的对象和数据运行访问工作的编程语句。
DML 的核心指令是 INSERT、UPDATE、DELETE、SELECT。这四个指令合称 CRUD(Create, Read, Update, Delete)，即增删改查。

### 数据查询语言（DQL）
Data Query Language，用于查询数据记录。

### 数据控制语言（DCL）
Data Control Language，是一种可对数据访问权进行控制的指令，它可以控制特定用户账户对数据表、查看表、预存程序、用户自定义函数等数据库对象的控制权。
DCL 的核心指令是 GRANT、REVOKE。
DCL 以**控制用户的访问权限**为主，因此其指令作法并不复杂，可利用 DCL 控制的权限有：CONNECT、SELECT、INSERT、UPDATE、DELETE、EXECUTE、USAGE、REFERENCES。
根据不同的 DBMS 以及不同的安全性实体，其支持的权限控制也有所不同。

### 事务控制语言（TCL）
Transaction Control Language，用于**管理数据库中的事务**。这些用于管理由 DML 语句所做的更改。它还允许将语句分组为逻辑事务。
TCL 的核心指令是 COMMIT、ROLLBACK。

## SQL 语法

1. SQL 语句可以单行或多行书写，以分号结尾。 
2. 可使用空格和缩进来增强语句的可读性。 处理 SQL 语句时，多余的空格（超过一个的空格）都被忽略**。**
3. **MySQL 数据库的 SQL 语句和字符串不区分大小写**，关键字建议使用大写。
4. 注释
```
## 注释1
-- 注释2
/* 注释3 */
```

5. 在关系型数据库中，双引号（"）通常用于将数据库对象的名称（如表名、字段名等）括起来。这可以使得名称中包含空格、标点符号、保留关键字等特殊字符时仍能够被正确解析和识别。

具体来说，使用双引号将表名或字段名括起来，可以实现以下效果：
(1) 区分大小写：在某些关系型数据库中，名称是大小写敏感的。使用双引号可以确保名称的大小写被正确识别和使用。
(2) 使用保留关键字或特殊字符：在某些情况下，表名或字段名中可能包含SQL保留关键字或特殊字符，例如空格、逗号、点号等。使用双引号可以使这些名称能够被正确解析和识别。
(3) 维护兼容性：在将数据迁移到其他数据库或平台时，如果表名或字段名中包含特殊字符或保留关键字，使用双引号可以确保名称的兼容性和正确性。

### SELECT 子句
SELECT 是**列/字段**选择语句，可以选择列字段，列间数学表达式，特定值或文本，可用AS关键字设置列别名（AS可省略，别名可使用单/双引号），DISTINCT 关键字可以去除重
> 列表达式中不能写别名，但可以通过 SELECT 子句使用，e.g. 
> SELECT 
> 	invoice_average a
> 	invoice_total - (SELECT a) AS difference
> 
> SELECT 
> 	invoice_average a
> 	invoice_total t
>  (SELECT t - a) AS difference

```sql
SELECT 
    1, first_name, points, (points + 10 * 100) AS discount_factor, DISTINCT state
FROM
    customers

-- 假设 orders 中并无 Active 字段
-- SELECT 'Active' (注意引号)会在查询结果增加 Active 列，且值都是 'Active'
SELECT 
	order_id,
  order_date,
  'Active'
FROM orders

SELECT 
	order_id,
  order_date,
  'Active' AS status
FROM orders
```

### WHERE 子句
WHERE 是行筛选条件，不能使用别名
```sql
SELECT * 
FROM customers
WHERE birth_date > '1990-01-01' OR points > 1000 AND state = 'VA'

SELECT * 
FROM customers
WHERE 
	state IN ('VA', 'FL', 'GA') OR
  (points BETWEEN 1000 AND 3000) OR
  last_name LIKE 'y%' -- % 代表任意数量字符，_ 代表单个字符

```

### ORDER BY 子句

1. 可多列
2. **可以是列间的数学表达式**
3. **可包括任何列，包括没 SELECT 的列（MySQL特性，其它DBMS可能报错），**
4. **可以是之前定义好的别名列（MySQL特性，甚至可以是用一个常数设置的列别名）**
5. 任何一个排序依据列后面都可选加 DESC

**注意**
最好别用 ORDER BY 1, 2（表示以 SELECT …… 选中列中的第1、2列为排序依据） 这种隐性依据，因为SELECT选择的列一变就容易出错，因此大多数情况下显性地写出列名作为排序依据比较好。
> 注：workbench 中扳手图标可打开表格的设计模式，查看或修改表中各列（属性），可以看到谁是主键。省略排序语句的话会默认按主键排序


```sql
SELECT first_name, 10 + 10 AS points
FROM customers
ORDER BY points, first_name

SELECT *, quantity * unit_price AS total_price
FROM order_items
WHERE order_id = 2
ORDER BY total_price DESC
```

### LIMIT 子句
语法：
SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数;
例子：
```sql
-- 查询第一页数据，展示10条
SELECT * FROM employee LIMIT 0, 10;
-- 查询第二页
SELECT * FROM employee LIMIT 10, 10;

SELECT * 
FROM customers
LIMIT 3  -- 选择前3个

SELECT * 
FROM customers
LIMIT 6, 3  -- 6, 3 表示跳过前6个，取第7~9个，6是偏移量
```
#### 注意事项

- 起始索引从0开始，起始索引 = （查询页码 - 1） * 每页显示记录数
- 分页查询是数据库的方言，不同数据库有不同实现，MySQL是LIMIT
- 如果查询的是第一页数据，起始索引可以省略，直接简写 LIMIT 10
- LIMIT 永远放在最后

### USING 子句
当作为合并条件（join condition）的列在两个表中有相同的列名时，可用 USING (……, ……) 取代 ON …… AND …… 予以简化，内/外链接均可如此简化。
```sql
SELECT 
	o.order_id,
  c.first_name,
  sh.name AS shipper
FROM orders o
JOIN customers c
	-- ON o.customer_id = c.customer_id
  USING (customer_id)
LEFT JOIN shippers sh
	USING (shipper_id)


SELECT *
FROM order_items oi
JOIN order_item_notes oin
	-- ON oi.order_id = oin.order_id AND oi.product_id = oin.product_id
	USING (order_id, product_id)
```

### GROUP BY 子句
按一列或多列分组
```sql
-- 在发票记录表中按不同顾客分组统计下半年总销售额并降序排列
SELECT 
	-- 只有聚合函数是按 client_id 分组时，这里选择 client_id 列才有意义，否则会报错
	client_id,
	SUM(invoice_total) AS total_sales
FROM invoices
WHERE invoice_date >= '2019-01-01'
GROUP BY client_id
ORDER BY total_sales DESC -- 若省略排序语句就会默认按分组依据排序

-- 各州各城市的总销售额
SELECT 
    state,
    city,
    SUM(invoice_total) AS total_sales
FROM invoices
JOIN clients USING (client_id) 
GROUP BY state, city  
ORDER BY state

```

### HAVING 子句
HAVING 和 WHERE 都是是条件筛选语句，条件的写法相通，数学、比较（包括特殊比较）、逻辑运算都可以用（如 AND、REGEXP 等等）
两者本质区别:

- 执行顺序
- WHERE：WHERE可以对没选择的列进行筛选，但不能使用别名
- HAVING：不能对 SELECT 里未选择的字段（非聚合函数）进行筛选。如果 HAVING 子句中包含聚合函数，在SELECT 子句中可以不用显式声明。
```sql
SELECT 
	client_id,
  SUM(invoice_total) AS total_sales,
  COUNT(*) AS number_of_invoices
FROM invoices
GROUP BY client_id
HAVING total_sales > 500
```

**GROUP BY 、HAVING 子句先于 SELECT 执行，为什么可以使用 SELECT 的别名?**
是因为mysql对此做了拓展引起的，这个取决于mysql中的一个参数 ONLY_FULL_GROUP_BY ，可以参考官方文档： https://dev.mysql.com/doc/refman/8.0/en/sql-mode.html#sqlmode_only_full_group_by 以及 https://dev.mysql.com/doc/refman/8.0/en/group-by-handling.html

### SQL 子句书写顺序和执行顺序
```sql
(8) SELECT (9)DISTINCT<Select_list>
(1) FROM <left_table> (3) <join_type>JOIN<right_table>
(2) ON<join_condition>
(4) WHERE<where_condition>
(5) GROUP BY<group_by_list>
(6) WITH {CUBE|ROLLUP}
(7) HAVING<having_condtion>
(10) ORDER BY<order_by_list>
(11) LIMIT<limit_number>
```
| **SYNTAX ORDER** | **EXECUTION ORDER** |
| --- | --- |
| SELECT | FROM |
| DISTINCT | ON |
| FROM | JOIN |
| JOIN | WHERE |
| ON | GROUP BY |
| WHERE | WITH |
| GROUP BY | HAVING |
| WITH | SELECT |
| HAVING | DISTINCT |
| ORDER BY | ORDER BY |
| LIMIT | LIMIT |

第一步：首先对from子句中的前两个表执行一个笛卡尔乘积，此时生成虚拟表 vt1（选择相对小的表做基础表）。 
第二步：接下来便是应用on筛选器，on 中的逻辑表达式将应用到 vt1 中的各个行，筛选出满足on逻辑表达式的行，生成虚拟表 vt2 。
第三步：如果是outer join 那么这一步就将添加外部行，left outer jion 就把左表在第二步中过滤的添加进来，如果是right outer join 那么就将右表在第二步中过滤掉的行添加进来，这样生成虚拟表 vt3 。
第四步：如果 from 子句中的表数目多余两个表，那么就将vt3和第三个表连接从而计算笛卡尔乘积，生成虚拟表，该过程就是一个重复1-3的步骤，最终得到一个新的虚拟表 vt3。 
第五步：应用where筛选器，对上一步生产的虚拟表引用where筛选器，生成虚拟表vt4。
> 注意where与on的区别：先执行on，后执行where；on是建立关联关系在生成临时表时候执行，where是在临时表生成后对数据进行筛选的。


第六步：group by 子句将中的唯一的值组合成为一组，得到虚拟表vt5。如果应用了group by，那么后面的所有步骤都只能得到的vt5的列或者是聚合函数（count、sum、avg等）。原因在于最终的结果集中只为每个组包含一行。这一点请牢记。 
第七步：应用avg或者sum选项，为vt5生成超组，生成vt6. 
第八步：应用having筛选器，生成vt7。having筛选器是第一个也是为唯一一个应用到已分组数据的筛选器。 
第九步：处理select子句。将vt7中的在select中出现的列筛选出来。生成vt8. 
第十步：应用distinct子句，对vt8进行去重，生成vt9。
第十一步：应用order by子句。按照order_by_condition排序vt9，此时返回的一个游标，而不是虚拟表。
第十二步：应用limit选项。生成vt10返回结果给请求者即用户。

SQL执行每一步骤，都可以看成是生成了一张虚拟表，每一次都是在操作虚拟表，理解了SQL的执行原理
```sql
FROM：对FROM的左边的表和右边的表计算笛卡尔积。产生虚表VT1
ON：对虚表VT1进行ON筛选，只有那些符合<join-condition>的行才会被记录在虚表VT2中。
JOIN：如果指定了OUTER JOIN（比如left join、 right join），那么保留表中未匹配的行就会作为外部行添加到虚拟表VT2中，产生虚拟表VT3, rug from子句中包含两个以上的表的话，那么就会对上一个join连接产生的结果VT3和下一个表重复执行步骤1~3这三个步骤，一直到处理完所有的表为止。
WHERE：对虚拟表VT3进行WHERE条件过滤。  只有符合<where-condition>的记录才会被插入到虚  拟表VT4中。
GROUP BY：根据groupby子句中的列，对VT4中的记录进行分组操作，产生VT5。
HAVING：对虚拟表VT5应用having过滤，只有符合<having-condition>的记录才会被插入到虚拟表VT6中。
SELECT：执行select操作，选择指定的列，插入到虚拟表VT7中。
DISTINCT：对VT7中的记录进行去重。产生虚拟表VT8。
ORDER BY:将虚拟表VT8中的记录按照<order_by_list>进行排序操作，产生虚拟表VT9。
LIMIT：取出指定行的记录，产生虚拟表VT10,并将结果返回。
```

## 在多张表格中检索数据
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1684391561760-14e84578-e1e5-4bc5-8b54-74896bf7cca4.png#averageHue=%23e5c1c1&clientId=u92089c01-8152-4&from=paste&height=588&id=u279f8f43&originHeight=472&originWidth=600&originalType=binary&ratio=2&rotation=0&showTitle=false&size=171286&status=done&style=none&taskId=u3c80e4e2-6c1f-473b-b23c-605b303ea0a&title=&width=747)
### Inner Joins
各表分开存放是为了减少重复信息和方便修改，需要时可以根据相互之间的关系连接成相应的合并详情表以满足相应的查询。FROM JOIN ON 语句就是告诉 SQL： 将哪几张表以什么条件连接/合并起来。
INNER 关键词可省略
如果没有 ON 条件，则是交叉合并，即**笛卡尔积**
```sql
-- orders 和 customers 中都含有 customer_id
-- orders.customer_id 确定选择 orders 表中的 customer_id
SELECT order_id, orders.customer_id, first_name, last_name
FROM orders
JOIN customers -- INNER JOIN 可省略 INNER
	ON orders.customer_id = customers.customer_id -- on 条件

-- 使用别名
SELECT order_id, o.customer_id, first_name, last_name
FROM orders o
JOIN customers c 
	ON o.customer_id = c.customer_id 

-- 交叉合并，结果：orders（5条记录） * customers（10条记录），得到50条记录
SELECT order_id, o.customer_id, first_name, last_name
FROM orders o
JOIN customers c

```

### Joining across Databases（跨数据库连接）
```sql
-- USE sql_store;

SELECT * 
FROM order_items oi -- 无需给当前数据库的表加数据库前缀
JOIN sql_inventory.products p
    ON oi.product_id = p.product_id
```

### Self Joins （自连接）
一个表和它自己合并。
例如，员工的上级也是员工，所以也在员工表里，所以想得到的有员工和他的上级信息的合并表，就要员工表自己和自己合并，**用两个不同的表别名即可实现**。这个例子中只有两级，但也可用类似的方法构建多层级的组织结构。
```sql
USE sql_hr;

-- e 和 m 是同一张表，选择列需要加表前缀区分
SELECT 
	e.employee_id,
  e.first_name,
  m.first_name AS manager
FROM employees e
JOIN employees m -- managers
	ON e.reports_to = m.employee_id
```
### 
Joining Multiple Tables（多表连接）
FROM 一个核心表A，用多个 JOIN …… ON …… 分别通过不同的链接关系链接不同的表B、C、D……，通常是让表B、C、D……为表A提供更详细的信息从而合并为一张详情合并版A表
```sql
USE sql_store;

SELECT 
	o.order_id,
  o.order_date,
  c.first_name,
  c.last_name,
  os.name AS status
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
JOIN order_statuses os
	ON o.status = os.order_status_id
```

### Compound Join Conditions（复合连接条件）
有时会存在无法用单一列来准确识别某张表的记录，例如存在重复值
像订单项目（order_items）这种表，**订单id和产品id合在一起才能唯一表示一条记录**，这叫**复合主键**，设计模式下也可以看到两个字段都有PK标识，订单项目备注表（order_item_notes）也是这两个复合主键，因此他们两合并时要用复合条件：FROM 表1 JOIN 表2 ON 条件1 【AND】 条件2
```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin
	ON oi.order_id = oin.order_id
  AND oi.product_id = oin.product_id
```

### Implicit Join Syntax（隐式连接语法）
用 FROM WHERE 取代 FROM JOIN ON
> 尽量不使用，因为若忘记 WHERE 条件筛选语句，不会报错但会得到交叉合并（cross join）结果

```sql
SELECT *
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
    
-- Implicit Join Syntax
SELECT *
FROM orders o, customers c
WHERE o.customer_id = c.customer_id -- 注释此行会导致交叉合并
```

### Outer Joins（外部连接）

- (INNER) JOIN 结果只包含两表的交集，**另外注意“广播（broadcast）”效应**
- LEFT/RIGHT (OUTER) JOIN 结果里除了交集，还包含只出现在左/右表中的记录，OUTER 关键词可省略
```sql
SELECT 
		c.customer_id,
    c.first_name,
    o.order_id
FROM customers c
LEFT JOIN orders o -- 左表 customers 的记录都会返回，不管条件是否满足
	ON c.customer_id = o.customer_id
ORDER BY c.customer_id

SELECT 
		c.customer_id,
    c.first_name,
    o.order_id
FROM customers c
RIGHT JOIN orders o -- 右表 orders 的记录都会返回，不管条件是否满足
	ON c.customer_id = o.customer_id
ORDER BY c.customer_id
```

### Outer Join Between Multiple Tables（多表外连接）
与内连接类似，我们可以对多个表（3个及以上）进行外连接
最好同时只用 JOIN 和 LEFT JOIN。因为当你连接多表，并且同时使用左、右连接和内连接，其他人在看你的代码时，会很难想象你到底想怎么连接这些表。

```sql
SELECT 
	c.customer_id,
  c.first_name,
  o.order_id,
  sh.name AS shipper
FROM customers c
LEFT JOIN orders o
	ON c.customer_id = o.customer_id
LEFT JOIN shippers sh
	ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id
```
### Self Outer Joins（自外连接）
上面内连接的例子，用LEFT JOIN让得到的 员工-上级 合并表也包括老板本人（老板没有上级，即 reports_to 字段为空，如果用 JOIN 会被筛掉，用 LEFT JOIN 才能保留）

```sql
SELECT 
	e.employee_id,
  e.first_name,
  m.first_name AS manager
FROM employees e
LEFT JOIN employees m -- managers
	ON e.reports_to = m.employee_id
```

### Natural Joins（自然连接）
NATURAL JOIN 就是让 MySQL 自动检索同名列作为合并条件。
尽量别用，因为不确定合并条件是否找对了，有时会造成无法预料的问题。
```sql
SELECT 
	o.order_id,
  c.first_name
FROM orders o
NATURAL JOIN customers c
```

### Cross Joins（交叉连接）
得到名字和产品的**所有组合**，因此**不需要合并条件**。 实际运用如：要得到尺寸和颜色的全部组合
```sql
SELECT 
	c.first_name AS customer,
  p.name AS product
FROM customers c
CROSS JOIN products p
ORDER BY c.first_name

-- 下面是隐式写法
SELECT 
	c.first_name AS customer,
  p.name AS product
FROM customers c
JOIN products p
ORDER BY c.first_name

SELECT 
	c.first_name AS customer,
  p.name AS product
FROM customers c, products p
ORDER BY c.first_name
```

### Unions（联合）
FROM …… JOIN …… 可对多张表进行横向列合并，而 …… UNION …… 可用来按行纵向合并多个查询结果，这些查询结果可能来自相同或不同的表

**注意**

- SELECT 必须列数量、顺序、类型相等，否则会报错
- 合并表里的列名由排在 UNION 前面的决定
- UNION 是自动去重的，如果要保留重复记录就要用 UNION ALL
```sql
-- 同一张表
-- 假设 orders 并无 'Active' 和 'Archived' 列，以下可以通过两个查询新增并作为 status 列的值
SELECT 
	order_id,
  order_date,
  'Active' AS status
FROM orders
WHERE order_date >= '2019-01-01'
UNION
SELECT 
	order_id,
  order_date,
  'Archived' AS status
FROM orders
WHERE order_date < '2019-01-01'

-- 不同表
SELECT first_name
FROM customers
UNION
SELECT name
FROM shippers

-- 报错：列数不一致
SELECT first_name, last_name
FROM customers
UNION
SELECT name
FROM shippers

```

## 插入、更新和删除数据
### 添加数据
指定字段：
INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...);
全部字段：
INSERT INTO 表名 VALUES (值1, 值2, ...);
批量添加数据：
INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);
INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);
**注意事项**

- 字符串和日期类型数据应该包含在引号中
- 插入的数据大小应该在字段的规定范围内
- 列如果设置了有 default 值，则值可以是 DEFAULT

#### 把一个表的数据插入到另一个表中
1.如果2张表的字段一致，并且希望插入全部数据，可以用这种方法：
```sql
 INSERT INTO 目标表 SELECT * FROM 来源表;

 例如：insert into insertTest1 select * from insertTest2;
```
2.如果只希望导入指定字段，可以用这种方法：
```sql
 INSERT INTO 目标表 (字段1, 字段2, ...) SELECT 字段1, 字段2,... FROM 来源表;(字段必须保持一致)

 例如：insert into insertTest2(id，name) select id,name from insertTest1;
```
注意：如果目标表与来源表主键值相同则会出现添加错误，主键值不同才能插入
3.如果您需要只导入目标表中不存在的记录，可以使用这种方法：
```sql
INSERT INTO 目标表 (字段1, 字段2, ...) SELECT 字段1, 字段2, ... FROM 来源表 

WHERE not exists (select * from 目标表 where 目标表.比较字段 = 来源表.比较字段);

 1>.插入多条记录：

insert into  insertTest2(id,name) select  id,name from insertTest

 where not exists (select * from insertTest2 where insertTest2.id=insertTest.id);

 2>.插入一条记录：

insert into insertTest (id, name) SELECT 100,'liudehua'  FROM dual 

WHERE not exists (select * from insertTest where insertTest.id = 100);

```
4、如果需要导入的目标表字段比来源表的字段多，将来源表的数据导入再加上几个字段组成目标表的数据
```sql
-- 目标字段1，目标字段2：这是目标表比来源表多出的字段
INSERT INTO 目标表 （目标字段1，目标字段2，字段1, 字段2,…）select 目标字段1，目标字段2, 字段1, 字段2,... FROM 来源表

insert into insertTest2(目标字段1，目标字段2，id,name) select 目标字段1, 目标字段2, id, name from insertTest where insertTest2.id=insertTest.id;
```


### 更新和删除数据
修改数据：
UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2, ... [ WHERE 条件 ];
例：
UPDATE emp SET name = 'Jack' WHERE id = 1;
删除数据：
DELETE FROM 表名 [ WHERE 条件 ];

### Inserting a  row
```sql
-- INSERT INTO 目标表 (若不指明列名，则插入的值必须按所有字段的顺序完整插入)
-- VALUES (所有列的值，按顺序逗号隔开，DEFAULT 表示默认值)

INSERT INTO customers 
VALUES (
	DEFAULT,
  'John',
  'Smith',
  '1990-01-01',
  DEFAULT,
  'address',
  'city',
  'CA',
  DEFAULT
)

-- INSERT INTO 目标表 （目标列，可选，逗号隔开）
-- VALUES (目标值，逗号隔开)

INSERT INTO customers (
	first_name,
  last_name,
  birth_date,
  address,
  city,
  state
)
VALUES (
    'John',
    'Smith',
    '1990-01-01',
    'address',
    'city',
    'CA'
)
```

### Inserting multiple rows
```sql
INSERT INTO table_name (column1, column2, column3)
VALUES
  (value1, value2, value3),
  (value4, value5, value6),
  (value7, value8, value9);
```

### Inserting Hierarchical Rows
订单表（orders表）里的一条记录对应订单项目表（order_items表）里的多条记录，一对多，是相互关联的父子表。通过添加一条订单记录和对应的多条订单项目记录，学习如何向父子表插入分级（层）/耦合数据（insert hierarchical data）：

- 关键：在插入子表记录时，需要用内建函数 LAST_INSERT_ID() 获取相关父表记录的自增ID（这个例子中就是 order_id)
- 内建函数：MySQL里有很多可用的内置函数，也就是可复用的代码块，各有不同的功能，注意函数名的单词之间用下划线连接
- LAST_INSERT_ID()：获取最新的成功的 INSERT 语句 中的自增id。
> todo：google LAST_INSERT_ID 问题
> [https://www.cnblogs.com/fnlingnzb-learner/p/15387090.html](https://www.cnblogs.com/fnlingnzb-learner/p/15387090.html)
> 第一、查询和插入所使用的Connection对象必须是同一个才可以，否则返回值是不可预料的。
> 第二、LAST_INSERT_ID是与表无关的，如果向表a插入数据后再向表b插入数据，LAST_INSERT_ID返回表b的Id值。
> 第三、假如你使用一条[INSERT](https://so.csdn.net/so/search?q=INSERT&spm=1001.2101.3001.7020)语句插入多个行，  LAST_INSERT_ID() 只返回插入的第一行数据时产生的值。
> 第四、假如你使用 INSERT IGNORE而记录被忽略，则AUTO_INCREMENT 计数器不会增量，而 LAST_INSERT_ID() 返回0, 这反映出没有插入任何记录。


```sql
INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-02', 1);

INSERT INTO order_items
VALUES 
	(LAST_INSERT_ID(), 1, 1, 2.95),
  (LAST_INSERT_ID(), 2, 1, 3.95)
```

### Creating a copy of a table
DROP TABLE [IF EXISTS] 表名
CREATE TABLE [IF EXISTS] 新表名 AS 子查询
> Note that the new table will not have any primary key, indexes, constraints, or other properties of the original table. If you need to recreate these properties in the new table, you will need to do so manually after creating the new table.

TRUNCATE 表名
INSERT INTO 表名 子查询
子查询： 任何一个充当另一个SQL语句的一部分的 SELECT…… 查询语句都是子查询
创建已有的表或删除不存在的表的话都会报错，所以建表和删表语句都最好加上条件语句
```sql
DROP TABLE IF EXISTS orders_archived;  -- 也可右键该表点击 drop
-- 复制表时不会复制主键的设置
-- Note that the new table will not have any indexes, constraints, or other properties of the original table. If you need to recreate these properties in the new table, you will need to do so manually after creating the new table.
CREATE TABLE orders_archived AS 
SELECT * FROM orders

TRUNCATE TABLE orders_archived; -- 也可右键该表点击 truncate  

INSERT INTO orders_archived  
-- 不用指明列名，会直接用子查询表里的列名
    SELECT * FROM orders  
    -- 子查询，替代原先插入语句中VALUES(……,……),(……,……),…… 的部分
    WHERE order_date < '2019-01-01'
```


### Updating a single row
```sql
UPDATE 表名
SET 要修改的字段 = 具体值/NULL/DEFAULT/列间数学表达式 （修改多个字段用逗号分隔）
WHERE 行筛选


UPDATE invoices
SET 
    payment_total = 100 / 0 / DEFAULT / NULL / 0.5 * invoice_total, 
    payment_date = '2019-01-01' / DEFAULT / NULL / due_date
WHERE invoice_id = 3
```

### Updating multiple rows
语法一样的，就是让 WHERE…… 的条件包含更多记录，就会同时更改多条记录了
Workbench默认开启了Safe Updates功能，不允许同时更改多条记录，要先关闭该功能（在 Preferences - SQL Editor - Safe Updates，重新连接数据库）
```sql
UPDATE invoices
SET 
	payment_total = invoice_total * 0.5, 
  payment_date = due_date
WHERE client_id = 3
```

### Using subqueries in updates
本质上是将子查询用在 WHERE…… 行筛选条件中
Update 前，最好先验证看一看子查询以及WHERE行筛选条件是不是准确的，筛选出的是不是我们的修改目标， 确保不会改错记录，再套入 UPDATE SET 语句更新
```sql
UPDATE invoices
SET payment_total = 567, payment_date = due_date

WHERE client_id = 
    (SELECT client_id 
    FROM clients
    WHERE name = 'Yadel');
    -- 放入括号，确保先执行

-- 若子查询返回多个数据（一列多条数据）时就不能用等号而要用 IN 了：
WHERE client_id IN 
    (SELECT client_id 
    FROM clients
    WHERE state IN ('CA', 'NY'))
```

### Deleting rows
```sql
DELETE FROM 表名 
WHERE 行筛选条件
（当然也可用子查询）
（若省略 WHERE 条件语句会删除表中所有记录（和 TRUNCATE 等效））
```


## 复杂查询
### 相关子查询
```sql
SELECT *
FROM employees e
WHERE salary > (
	SELECT AVG(salary)
  FROM employees
  -- 相关子查询，这段查询会在主查询每一行都会执行，注意执行顺序 父WHERE -> 子FROM -> 子WHERE
  -- 相关子查询很慢
  -- office_id 相同时，会再算一次平均值，导致了重复的计算
  WHERE office_id = e.office_id
)

-- 如果用 JOIN 实现
SELECT *
FROM employees e
-- 各个 office 只计算了一次平均值
JOIN (
	SELECT 
	office_id,
	AVG(salary) AS office_salary
  FROM employees
  GROUP BY office_id
) AS office_avg
-- 这里的 USING 是 office_avg.office_id = e.office_id
-- 显然 avg 的行数更少，比上面的 WHERE 效率更高
USING (office_id)
WHERE e.salary > office_avg.office_salary

-- 性能方面，还需要考虑 WHERE 和 JOIN 的结果集谁更大，在大量数据的情况下，JOIN 会形成非常大的列表
-- 占用内存，严重影响内存
```


### SELECT 子句中的子查询
```sql
SELECT
		invoice_id,
    invoice_total,
    (SELECT AVG(invoice_total) FROM invoices) AS invoice_average,
    invoice_total - (SELECT invoice_average) AS difference
FROM invoices
```

### FROM 子句中的子查询
```sql
SELECT *
FROM (
	SELECT
		client_id,
	FROM clients c
) AS summary -- FROM 子查询必须给子查询起别名
```

## 运算符
### ALL 
> (MAX (……)) 和 > ALL(……) 可互换
以下的例子 MAX 是先算出最大值，然后每行对比最大值，性能更好
ALL 是先得到一列的值，然后每行与这一列值进行比较。
```sql
SELECT *
FROM invoices
WHERE invoice_total > (
	SELECT MAX(invoice_total)
  FROM invoices
  WHERE client_id = 3
)

-- 上下两种方式相等

SELECT *
FROM invoices
WHERE invoice_total > ALL(
	SELECT invoice_total
  FROM invoices
  WHERE client_id = 3
)
```

### ANY
> ANY/SOME (……) 与 > (MIN (……)) 可互换
= ANY/SOME (……) 与 IN (……) 可互换
```sql
SELECT *
FROM clients
WHERE client_id IN (
	SELECT client_id
  FROM invoices
  GROUP BY client_id
  HAVING count(*) >= 2
)

-- 上下两种方式相等

SELECT *
FROM clients
WHERE client_id = ANY (
	SELECT client_id
  FROM invoices
  GROUP BY client_id
  HAVING count(*) >= 2
)
```

### EXISTS
```sql
-- 但是 方式一 IN后面跟有大量数据（例如上亿条）时，会形成非常大的列表，占用内存，严重影响性能
-- 此时采用第二种方式，EXISTS 后不会返回结果集，而是返回 boolean，性能反而会更高

-- 方式一
SELECT *
FROM clients
WHERE client_id IN (
	SELECT DISTINCT client_id
  FROM invoices
)

-- 方式二
SELECT *
FROM clients c
WHERE EXISTS (
	SELECT client_id
  FROM invoices
  WHERE client_id = c.client_id
)
```

### CASE
```sql
SELECT
	order_id,
  CASE
  	WHEN YEAR(order_date) = YEAR(NOW()) THEN 'Active'
    WHEN YEAR(order_date) = YEAR(NOW()) - 1 THEN 'Last Year'
    WHEN YEAR(order_date) < YEAR(NOW()) - 1 THEN 'Archived'
    ELSE 'Future' -- ELSE 子句可选
	END AS category
FROM orders

-- 两种写法
SELECT
		CONCAT(first_name, ' ', last_name) AS customer,
    points,
    CASE 
			WHEN points >= 3000 THEN 'Gold'
      WHEN points >= 2000 THEN 'Silver'
      ELSE 'Bronze'
		END AS category
FROM customers
ORDER BY points DESC

SELECT
    CONCAT(first_name, ' ', last_name) AS customer,
    points,
    IF(points < 2000, 'Bronze', 
        IF(points < 3000, 'Silver', 
            IF(points >= 3000, 'Gold', null))) AS category
FROM customers
ORDER BY points DESC
```


# MySQL
## 命名规范
[https://www.biaodianfu.com/mysql-best-practices.html](https://www.biaodianfu.com/mysql-best-practices.html)

**基本命名原则**

- 使用有意义的英文词汇，词汇中间以下划线分隔。（不要用拼音）
- 只能使用英文字母，数字，下划线，并以英文字母开头。
- 库、表、字段全部采用小写，不要使用驼峰式命名，多个单词以下划线 _ 分隔。
- 避免用ORACLE、MySQL的保留字，如desc，关键字如index。
- 命名禁止超过32个字符，须见名之意，建议使用名词不是动词
- 数据库，数据表一律使用前缀，
   - 临时库、表名以 tmp 为前缀，并以日期为后缀
   - 备份库、表名以 bak 为前缀，并以日期为后缀

**为什么库、表、字段全部采用小写？**
[https://dev.mysql.com/doc/refman/8.0/en/identifier-case-sensitivity.html](https://dev.mysql.com/doc/refman/8.0/en/identifier-case-sensitivity.html)
在 MySQL 中，数据库和表对就于那些目录下的目录和文件。因而，操作系统的敏感性决定数据库和表命名的大小写敏感。

- Windows 和 macOS 下是不区分大小写的。
- Linux下大小写规则：
   - 数据库名与表名是严格区分大小写的；
   - 表的别名是严格区分大小写的；
   - 列名与列的别名在所有的情况下均是忽略大小写的；
   - 变量名也是严格区分大小写的；

MySQL 中的标识符（如表名、列名等）可以通过使用反引号（`）来进行引用，从而使其变得大小写敏感。例如，如果使用 `SELECT * FROM `MyTable`` 这样的查询语句，MySQL 将会区分大小写，并只查找名为 `MyTable` 的表，而不是名为 `mytable` 的表。

[https://dev.mysql.com/doc/refman/8.0/en/case-sensitivity.html](https://dev.mysql.com/doc/refman/8.0/en/case-sensitivity.html)
查询进行字符串比较时，是区分大小写的。

**数据库命名**

- 项目名称_名称

**表命名**

- 使用单数，同一个模块的表尽可能使用相同的前缀，表名称尽可能表达含义。所有日志表均以 log_ 开头

**字段命名**

- 表达其实际含义的英文单词或简写。布尔意义的字段以“is_”作为前缀，后接动词过去分词。
- 表主键：xxx_id
- 外键： fk_从表名_主表名

**索引命名**

- 非唯一索引：idx_字段名称_字段名称[_字段名]
- 唯一索引：uniq_字段名称_字段名称[_字段名]

**约束命名**

- 主键约束：pk_表名称
- 唯一约束：uk_表名称_字段名

**触发器命名**

- trig_表名_操作_后缀（_i,_u,_d）,表示触发条件的触发方式（insert,update或delete）

**视图命名**

-  v_表名[_表名]

**函数过程命名**

-  func_xxx 

**存储过程命名**

- 存储过程名以sp开头，表示存储过程（storage procedure），sp_xxx
- 存储过程中的输入参数以 i_ 开头，输出参数以o_开头

**序列命名**

- seq_表名

## 字符集
### UTF8 和 UTF8MB4
在 mysql 中，utf8 是 utf8mb3 的别名
国际上的 utf8 对标的是 mysql 的 utf8mb4
如无特殊要求，必须使用utf8或utf8mb4。
在国内，选择对中文和各语言支持都非常完善的utf8格式是好的方式，MySQL在5.5之后增加utf8mb4编码，utf8mb4是utf8的超集，mb4是 most bytes 4，专门用来兼容四字节的 unicode 字符，但是会占用更多的存储空间和内存空间

#### UTF8的定义
[https://zh.wikipedia.org/zh-hans/UTF-8](https://zh.wikipedia.org/zh-hans/UTF-8)
[https://www.unicode.org/faq/utf_bom.html#UTF8](https://www.unicode.org/faq/utf_bom.html#UTF8)
utf8 是可变长编码，2003年11月UTF-8被RFC 3629重新规范，只能使用原来Unicode定义的区域，U+0000到U+10FFFF，也就是说**最多使用四个字节**来表示一个 unicode 字符。

## Data Types

- 一个晶体管可开和关，表示0或1两个值，代表最小储存单位，叫一位（bit）
- 一字节（Byte）有8位，可表示2^8个值，即256个值
- 字节（B）、千字节（KB）、兆字节（MB）、吉字节（GB）、太字节（TB）相邻两者之间的换算比率是2^10，即1024

**设置数据类型时，要尽可能节省空间**
### String/Blob Data Types
> 超出部分会被截断

| 类型 | 大小 | 用途 |
| --- | --- | --- |
| CHAR(size) | 0-255 bytes（1b） | 定长字符串 |
| VARCHAR(size) | 0-65 535 bytes（64k） | 变长字符串 |
| BINARY(size) | 0-255 bytes | Equal to CHAR(), but stores binary byte strings. The _size_ parameter specifies the column length in bytes. Default is 1 |
| VARBINARY(size) | 0-65535 bytes | Equal to VARCHAR(), but stores binary byte strings. The _size_ parameter specifies the maximum column length in bytes. |
| TINYBLOB | 0-255 bytes | 不超过 255 个字节的二进制字符串 |
| TINYTEXT | 0-255 bytes | 短文本字符串 |
| BLOB(size) | 0-65 535 bytes | 二进制形式的长文本数据 |
| TEXT(size) | 0-65 535 bytes | 长文本数据 |
| MEDIUMBLOB | 0-16 777 215 bytes | 二进制形式的中等长度文本数据 |
| MEDIUMTEXT | 0-16 777 215 bytes（16M） | 中等长度文本数据 |
| LONGBLOB | 0-4 294 967 295 bytes | 二进制形式的极大文本数据 |
| LONGTEXT | 0-4 294 967 295 bytes（4G） | 极大文本数据 |
| ENUM(val1, val2, val3, ...) | 

 | A string object that can have only one value, chosen from a list of possible values. You can list up to 65535 values in an ENUM list. If a value is inserted that is not in the ENUM list, a blank value will be inserted. The values are sorted in the order you enter them |
| SET(val1, val2, val3, ...) | 
 | A string object that can have 0 or more values, chosen from a list of possible values. You can list up to 64 values in a SET list |

**国际字符**

- 英文字符占1个字节
- 欧洲和中东语言字符占2个字节
- 像中日这样的亚洲语言的字符**最多**占3个字节

所以，如果一列数据的类型为 CHAR(10)，MySQL会预留30字节给那一列的值

**varchar(191)**
在 MySQL 中，utf8 字符集指的是 utf8m，使用 3 个字节来表示大部分 unicode 字符，为了兼容更多的字符，例如 emoji，需要使用 utf8mb4，使用 4 个字节表示 unicode 字符。
在MySQL 5.7.7及更高版本中，将varchar类型的最大长度限制为191个字符是为了支持特定字符集的索引。
[https://dev.mysql.com/doc/refman/5.7/en/charset-unicode-conversion.html](https://dev.mysql.com/doc/refman/5.7/en/charset-unicode-conversion.html)
InnoDB has a maximum index length of 767 bytes for tables that use [COMPACT](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_compact_row_format) or [REDUNDANT](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_redundant_row_format) row format, so for utf8mb3 or utf8mb4 columns, you can index a maximum of 255（767 /3 ） or 191（767 / 4）characters, respectively. If you currently have utf8mb3 columns with indexes longer than 191 characters, you must index a smaller number of characters.

**SET**
SET和ENUM类似，区别是，SET是从固定一系列值中取多个值而非一个值

**ENUM 和 SET 有以下缺点**
1. 新增或修改 ENUM 和 SET 的列表值会重建整个表，如果表中有大量数据会相当耗费时间
2. 想查询可选值的列表或者想用可选值当作一个下拉列表都会比较麻烦
3. 难以在其它表里复用，其它地方要用只有重建相同的列，之后想修改就要多处修改
像这种某个字段是从固定的一系列值中取值的情况，不应该使用 ENUM 和 SET 而应该用这一系列的可选值另外建一个 “查询表” (lookup table)

**Blob**
一般来说，最好不要把文件存在数据库中
关系型数据库是设计来专门处理结构化关系型数据而非二进制文件的
如果将文件储存在数据库内，会有如下问题：

1. Increased database size: 数据库的大小将迅速增长
2. Slower backups: 备份会很慢
3. Performance problems: 性能问题，因为从数据库中读取图片永远比直接从文件系统读取慢
4. More code to read / write images: 需要额外的读写图像的代码

### Numeric Data Types
列属性 UNSIGNED 则只储存非负数
列属性 ZEROFILL 补零
括号表示显示位数，如 INT(4) => 0001，注意这只影响MySQL如何显示数字而不影响如何保存数字
> size 精度
> p - precision 精度
> d - decimal 小数位
> DECIMAL = DEC = NUMERIC = FIXED

| 类型 | 大小 | 范围（有符号） | 范围（无符号） | 用途 |
| --- | --- | --- | --- | --- |
| TINYINT(size) | 1 Bytes | (-128，127) | (0，255) | 小整数值 |
| SMALLINT(size) | 2 Bytes | (-32 768，32 767) | (0，65 535) | 大整数值 |
| MEDIUMINT(size) | 3 Bytes | (-8 388 608，8 388 607) | (0，16 777 215) | 大整数值 |
| INT或INTEGER(size) | 4 Bytes | (-2 147 483 648，2 147 483 647) | (0，4 294 967 295) | 大整数值 |
| BIGINT(size) | 8 Bytes | (-9,223,372,036,854,775,808，9 223 372 036 854 775 807) | (0，18 446 744 073 709 551 615) | 极大整数值 |
| FLOAT(p) | 4 Bytes | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38) | 单精度
浮点数值
不存储准确值，取近似值
A floating point number. MySQL uses the _p_ value to determine whether to use FLOAT or DOUBLE for the resulting data type. If _p_ is from 0 to 24, the data type becomes FLOAT(). If _p_ is from 25 to 53, the data type becomes DOUBLE()
 |
| DOUBLE(size,d) | 8 Bytes | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度
浮点数值
不存储准确值，取近似值
A normal-size floating point number. The total number of digits is specified in _size_. The number of digits after the decimal point is specified in the _d_ parameter |
| DECIMAL(size,d) | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 | size: [1, 65]
d: [0, 30] | size: [1, 65]
d: [0, 30] | 小数值
An exact fixed-point number. The total number of digits is specified in _size_. The number of digits after the decimal point is specified in the _d_ parameter. The maximum number for _size_ is 65. The maximum number for _d_ is 30. The default value for _size_ is 10. The default value for _d_ is 0. |
| DEC(size,d) | Equal to DECIMAL(size,d) |  |  |  |


### Date and Time Data Types
fsp是秒精度，fsp的值，如果给定，必须在0 ~ 6之间，最大值为6，表示精确到微妙

| 类型 | 大小

 | 范围 | 格式 | 用途 |
| --- | --- | --- | --- | --- |
| DATE | 3 | 1000-01-01 to 9999-12-31 | YYYY-MM-DD | 日期值 |
| TIME(fsp) | 3 | '-838:59:59' to '838:59:59' | hh:mm:ss | 时间值或持续时间 |
| YEAR | 1 | 1901/2155 | YYYY | 年份值 |
| DATETIME(fsp) | 8 | 1000-01-01 00:00:00/9999-12-31 23:59:59 | YYYY-MM-DD hh:mm:ss | 混合日期和时间值 |
| TIMESTAMP(fsp) | 4 |  '1970-01-01 00:00:01' UTC to '2038-01-09 03:14:07' UTC.  | YYYY-MM-DD hh:mm:ss | 混合日期和时间值、时间戳
只能存储到2038年以前的问题（2038问题） |


### Spatial Data Types
它是一种特殊的数据类型，用于保存各种几何和地理值。它对应于 OpenGIS 类。下表显示了 MySQL 中支持的所有空间类型：

| 数据类型 | 说明 |
| --- | --- |
| GEOMETRY | 它是一个点或点的集合，可以保存具有位置信息的任何类型的空间值。 |
| POINT | 几何中的一个点代表一个位置。它存储 X、Y 坐标的值。 |
| POLYGON | 它是一个表示多边几何的平面。它可以由零个或多个内部边界和一个外部边界定义。 |
| LINESTRING | 它是具有一个或多个点的曲线。如果它只包含两个点，它代表直线。 |
| GEOMETRYCOLLECTION | 它是一种具有零个或多个几何值的集合。 |
| MULTILINESTRING | 它是一个多曲线几何体，具有一组线字符串值。 |
| MULTIPOINT | 它是多个点元素的集合。在这里，这些点不能以任何方式连接或排序。 |
| MULTIPLYGON | 它是一个多表面对象，表示多个多边形元素的集合。它是一种二维几何。 |


### JSON Data Type
MySQL 从 v5.7.8 版本开始提供对原生 JSON 数据类型的支持。这种数据类型允许我们快速有效地存储和访问 JSON 文档。
```sql
-- 新增 JSON
UPDATE products
SET properties = '
{
		"dimensions": [1, 2, 3],
    "weight": 10,
    "manufacturer": {
			"name": "sony"
    }
}
'
WHERE product_id = 1;

-- 上下两种写法均可

UPDATE products
SET properties = JSON_OBJECT(
		'weight', 10, 
		'dimensions', JSON_ARRAY(1, 2, 3),
    'manufacturer', JSON_OBJECT('name', 'sony')
    )
WHERE product_id = 1;


-- 查询
SELECT 
-- $ 表示当前 JSON
product_id, 
JSON_EXTRACT(properties, '$.weight') AS weight
FROM products
WHERE product_id = 1;

-- 上下两种写法均可

SELECT 
-- $ 表示当前 JSON
product_id, 
-- 获取嵌套JSON的属性
-- 使用 ->> ，查出来的值显示不带引号，不影响值
properties -> '$.manufacturer.name'
FROM products
WHERE product_id = 1;


-- 更新 JSON
UPDATE products
SET properties = JSON_SET(
		properties,
    '$.weight', 20,
  	'$.sizes', JSON_ARRAY(1, 2, 3),
    -- 数组索引
  	'$.dimensions[1]', 8,
    '$.manufacturer.name', 'sony'
)
WHERE product_id = 1;

UPDATE products
SET properties = JSON_REMOVE(
	properties,
	'$.age'
)
WHERE product_id = 1;
```


### 约束
```sql
PK：primary key 主键

NN：not null 非空

UQ：unique 唯一索引，创建UNIQUE约束时，MySQL会UNIQUE在幕后创建索引。

BIN：binary 二进制数据(比text更大的二进制数据)

UN：unsigned 无符号整数（非负数）

ZF：zero fill 填充0 例如字段内容是1 int(4), 则内容显示为0001 

AI：auto increment 自增

G：generated column 生成列

Default/Expression: 默认值/ SQL 表达式

CHECK: 检查约束（8.0.1版本后）	保证字段值满足某一个条件	CHECK

在mysql查询语句中加\g、\G的意思：

\g  的作用是分号和在sql语句中写“;”是等效的
\G  的作用是将查到的结构旋转90度变成纵向（换行打印）

```

例子：
```sql
create table user(
  id int primary key auto_increment,
  name varchar(10) not null unique,
  age int check(age > 0 and age < 120),
  status char(1) default '1',
  gender char(1)
);
```

#### 为什么字段建议设置为 NOT NULL

1. 开发友好性，提高查询性能，不需要额外处理空值情况
2. 使用 NULL，聚合函数会忽略 NULL 值，会造成统计结果不准确
3. 等于号 `=` 失效，必须使用 **IS NULL** 和 **IS NOT NULL** 判断
4. NULL 和其他任何值进行运算都是 NULL，包括表达式的值也是 NULL
5. 对于 distinct 和 group by 来说，所有的 NULL 值都会被视为相等，对于order by来说升序 NULL 会排在最前

![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1686642202650-816d66c9-8dc8-4dc4-927d-6d8c1022fa7f.png#averageHue=%2309262e&clientId=u71c0759a-9226-4&from=paste&height=737&id=u7aea8b2d&originHeight=933&originWidth=666&originalType=binary&ratio=1&rotation=0&showTitle=false&size=271669&status=done&style=none&taskId=ua70df331-0ad6-45c4-b2ec-520bc2e0059&title=&width=526)


## MySQL 运算符
MySQL 支持 4 种运算符，分别是：
**1) 算术运算符**
执行算术运算，例如：加、减、乘、除等。
**2) 比较运算符**
**>  <=  >=  <=  !=  <>**  （ != 和 <> 都是不等于）
**3) 逻辑运算符**
与、或、非等。
**4) 位运算符**
包括按位与、按位或、按位取反、按位异或、按位左移和按位右移等位运算符。位运算必须先将数据转换为补码，然后在根据数据的补码进行操作。运算完成后，将得到的值转换为原来的类型（十进制数），返回给用户。

### 优先级
MySQL 中的各类运算符及其优先级，由高到低：
```sql
INTERVAL
BINARY, COLLATE
!
- (unary minus), ~ (unary bit inversion)
^
*, /, DIV, %, MOD
-, +
<<, >>
&
|
= (comparison), <=>, >=, >, <=, <, <>, !=, IS, LIKE, REGEXP, IN, MEMBER OF
BETWEEN, CASE, WHEN, THEN, ELSE
NOT
AND, &&
XOR
OR, ||
= (assignment), :=
```

### BETWEEN
建议：由于各个数据库的 BETWEEN 边界取值不同，尽量使用 > = < 等运算符

- between 的范围是包含两边的边界值

eg：id between 3 and 7 等价于 id >=3 and id<=7

- not between 的范围是不包含边界值

eg：id not between 3 and 7 等价于 id < 3 or id>7

- DATE 格式，查询 2008-08-08 到 2009-01-01 零点之前的数据
```sql
SELECT * 
FROM 
table 
WHERE column_time 
BETWEEN '2008-08-08' AND '2009-01-01'
```

- DATETIME 格式，查询 2008-08-08 20:00:00 到 2009-01-01 零点之前的数据
```sql
SELECT * 
FROM table 
WHERE column_time 
BETWEEN '2008-08-08 20:00:00' AND '2008-12-31 23:59:59'
```

### REGEXP
**区分大小写**：使用BINARY关键字，如 where post_name REGEXP BINARY 'Hello'

| **模式** | **匹配模式的什么** | **例子** | **含义** |
| --- | --- | --- | --- |
| ^ | 匹配字符串开头 | select name from 表名 where name regexp '^王' | 匹配姓为王的名字 |
| $ | 匹配字符串结尾 | select name from 表名 where name regexp '明$' | 匹配名字最后一个字为明的名字 |
| . | 匹配任意字符 | select name from 表名 where name regexp '.明.' | 匹配带有明的名字 |
| […] | 匹配方括号间列出的任意字符 | select name from 表名 where name regexp '^[wzs]'; | 匹配括号里任意字符的名字 |
| [^…] | 匹配方括号间未列出的任意字符 | select name from 表名 where name regexp '^[^wzs]'; | 匹配未在括号里任意字符的名字 |
| p1&#124;p2&#124;p2 | 交替：匹配任意p1或p2或p3 | select performance from 表名 where performance regexp 'A-&#124;A&#124;A+'; | 匹配p1,p2,p3 |
| * | 匹配前面的字符零次或者多次 | str*' | 可以匹配st/str/strr/strrr…… |
| ? | 匹配前面的字符零次或者1次 | str?' | 可以匹配st/str |
| + | 匹配前面的字符一次或者多次 | str+' | 可以匹配str/strr/strrr/strrrr…… |
| {n} | 匹配前面的字符n次 |  |  |
| {m,n} | 匹配前面的字符m至n次 |  |  |


### WITH ROLLUP
GROUP BY …… WITH ROLL UP 汇总分组的列的值
只适用于聚合值的列，对于没法聚合的列，汇总返回 null
当使用 WITH ROLL UP，GROUP BY 子句中不能列别名
```sql
SELECT
	state,
  city,
  SUM(invoice_total) AS total_sales
FROM invoices
JOIN clients c USING (client_id)
GROUP BY state, city WITH ROLLUP
```


## 基本函数
### String Functions
**谷歌 mysql string functions**
常用函数：

| **函数** | **功能** |
| --- | --- |
| CONCAT(s1, s2, …, sn) | 字符串拼接，将s1, s2, …, sn拼接成一个字符串 |
| LOWER(str) | 将字符串全部转为小写 |
| UPPER(str) | 将字符串全部转为大写 |
| LPAD(str, n, pad) | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度 |
| RPAD(str, n, pad) | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度 |
| TRIM(str) | 去掉字符串头部和尾部的空格 |
| SUBSTRING(str, start, len) | 返回从字符串str从start位置起的len个长度的字符串 |
| REPLACE(column, source, replace) | 替换字符串 |

使用示例：
```sql
-- 拼接
SELECT CONCAT('Hello', 'World');
-- 小写
SELECT LOWER('Hello');
-- 大写
SELECT UPPER('Hello');
-- 左填充
SELECT LPAD('01', 5, '-');
-- 右填充
SELECT RPAD('01', 5, '-');
-- 去除空格
SELECT TRIM(' Hello World ');
-- 切片（起始索引为1）
SELECT SUBSTRING('Hello World', 1, 5);
```
### Numeric Functions
**谷歌 mysql numeric functions**
常见函数：

| **函数** | **功能** |
| --- | --- |
| CEIL(x) | 向上取整 |
| FLOOR(x) | 向下取整 |
| MOD(x, y) | 返回x/y的模 |
| RAND() | 返回0~1内的随机数 |
| ROUND(x, y) | 求参数x的四舍五入值，保留y位小数 |

### 日期函数
标准SQL函数 EXTRACT()，若需要在不同DBMS中录入代码，最好用 
EXTRACT(单位 FROM 日期时间对象/'2023-01-01'/'2023-01-02 11:22'/'2023-01-02 11:22:33')：
```sql
SELECT EXTRACT(YEAR FROM NOW())
```
**Formatting Dates and Times**
DATE_FORMAT(date, format) 将 date 根据 format 字符串进行格式化。
TIME_FORMAT(time, format) 类似于 DATE_FORMAT 函数，但这里 format 字符串只能包含用于小时，分钟，秒和微秒的格式说明符。
**常用函数：**

| **函数** | **功能** |
| --- | --- |
| CURDATE() | 返回当前日期 |
| CURTIME() | 返回当前时间 |
| NOW() | 返回当前日期和时间 |
| YEAR(date) | 获取指定date的年份 |
| MONTH(date) | 获取指定date的月份 |
| DAY(date) | 获取指定date的日期 |
| DATE_ADD(date, INTERVAL expr type) | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
| DATEDIFF(date1, date2) | 返回起始时间date1和结束时间date2之间的天数 |

例子：
```sql
-- DATE_ADD
SELECT DATE_ADD(NOW(), INTERVAL 70 YEAR);
```
### 流程函数
常用函数：

| **函数** | **功能** |
| --- | --- |
| IF(value, t, f) | 如果value为true，则返回t，否则返回f |
| IFNULL(value1, value2) | 如果value1不为空，返回value1，否则返回value2 |
| CASE WHEN [ val1 ] THEN [ res1 ] … ELSE [ default ] END | 如果val1为true，返回res1，… 否则返回default默认值 |
| CASE [ expr ] WHEN [ val1 ] THEN [ res1 ] … ELSE [ default ] END | 如果expr的值等于val1，返回res1，… 否则返回default默认值 |

例子：
```sql
select
	name,
	(case when age > 30 then '中年' else '青年' end)
from employee;
select
	name,
	(case workaddress 
   	when '北京市' then '一线城市' 
   	when '上海市' then '一线城市' 
    else '二线城市' 
   end) as '工作地址'
from employee;
```


#### IFNULL和COALESCE函数
```sql
SELECT 
	order_id,
	-- shipper_id 为 null 时，返回 'Not assigned'
  IFNULL(shipper_id, 'Not assigned') AS shipper
FROM orders

SELECT 
	order_id,
	-- COALESCE(shipper_id, comments,'Not assigned', ...)，可以跟很多值
	-- shipper_id 为 null 时，返回第一个非 null 值
	-- 若 comments 还是为 null，返回 'Not assigned'，以此类推
  COALESCE(shipper_id, comments, 'Not assigned') AS shipper
FROM orders
```

#### IF Function
IF(条件表达式, 返回值1, 返回值2)
```sql
SELECT
	order_id,
  order_date,
  IF(YEAR(order_date) = YEAR(NOW()), 'Active', 'Archived') AS category
FROM orders
```

#### 条件类语句

1. 替换空值 IFNULL(值1，值2) 和 COALESC 函数
2. 条件函数 IF(条件表达式, 返回值1, 返回值2)
3. CASE WHEN 
4. 条件语句块
```sql
IF search_condition THEN statement_list
[ELSEIF search_condition THEN]
    statement_list ...  
[ELSE statement_list]  
END IF;
```


## Aggregate Functions
This section describes aggregate functions that operate on sets of values. They are often used with a GROUP BY clause to group values into subsets.
**输入一系列值并聚合为一个结果**的函数
```sql
-- SELECT选择的不仅可以是列，也可以是数字、列间表达式、列的聚合函数
SELECT 
    MAX(invoice_date) AS latest_date,  
    MIN(invoice_total) lowest,
    AVG(invoice_total) average,
    SUM(invoice_total * 1.1) total,
    -- 复合筛选条件的所有记录数，无论是否含有空值
    COUNT(*) total_records,
    COUNT(invoice_total) number_of_invoices, 
  	-- 【聚合函数会忽略空值】，得到的支付数少于发票数
    COUNT(payment_date) number_of_payments,  
    -- DISTINCT client_id 筛掉了该列的重复值
    COUNT(DISTINCT client_id) number_of_distinct_clients
FROM invoices
WHERE invoice_date > '2019-07-01'  -- 想只统计下半年的结果
```


## (nonaggregate) Window Functions
[https://www.cnblogs.com/shoshana-kong/p/16621254.html](https://www.cnblogs.com/shoshana-kong/p/16621254.html)
[https://dbaplus.cn/news-11-2258-1.html](https://dbaplus.cn/news-11-2258-1.html)
What is window function?
> In the SQL database query language, window functions allow access to data in the records right before and after the current record. A window function defines a frame or window of rows with a given length around the current row, and performs a calculation across the set of data in the window.

Basically this means — A query where a window is created and from that window a value is taken. A window function performs a calculation across a set of table rows that are somehow related to the current row.  Also, Window functions may only appear in the result set and in the ORDER BY clause of a SELECT statement. 

Types of Window functions :

- Analytical
- Aggregate

**Aggregate functions** : An aggregate function computes the aggregate values of a particular row and returns the value. For example : sum() returns sum of a row. This returns a single result.
They are : sum, avg, min, max etc. This is same as we studied in python and in school.
**Analytical functions : **An analytic function computes values over a group of rows and returns a single result for _**each**_ row. This is different from an aggregate function, which returns a single result for _an entire group_ of rows.
They are : Cumulative distribution, row number, dense rank, rank, ntile, lag, lead, firstvalue, last value etc. We will see each one with a example and we will get more understanding.

A window function in MySQL used to do a calculation across a set of rows that are related to the **current row**. The current row is that row for which function evaluation occurs. Window functions perform a calculation similar to a calculation done by using the aggregate functions. But, unlike aggregate functions that perform operations on an entire table, window functions do not produce a result to be grouped into one row. It means window functions perform operations on a set of rows and **_produces an aggregated value for each row_**. Therefore each row maintains the unique identities.

窗口的概念非常重要，它可以理解为记录集合，窗口函数也就是在满足某种条件的记录集合上执行的特殊函数。对于每条记录都要在此窗口内执行函数，有的函数随着记录不同，窗口大小都是固定的，这种属于静态窗口；有的函数则相反，不同的记录对应着不同的窗口，这种动态变化的窗口叫滑动窗口。

窗口函数和普通聚合函数也很容易混淆，二者区别如下：

- 聚合函数是将多条记录聚合为一条；而窗口函数是每条记录都会执行，有几条记录执行完还是几条。
- 聚合函数也可以用于窗口函数中，这个后面会举例说明。


## Keywords and Reserved Words
[https://dev.mysql.com/doc/refman/8.0/en/keywords.html](https://dev.mysql.com/doc/refman/8.0/en/keywords.html)
当表名、字段名与 MySQL 保留关键字冲突或者不确定是否为关键字时，需要用反引号标注。
### EXPLAIN
 explain 查看一条SQL的执行计划
下面我们使用 explain 做一个查询，如下：
![](https://cdn.nlark.com/yuque/0/2023/jpeg/26428688/1674547433518-31671f9b-a0e7-41ef-aa26-a829adeb3209.jpeg#averageHue=%23e2ebf3&clientId=ue4843645-62ef-4&from=paste&id=u4e3439d4&originHeight=122&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u08ff3851-ca69-4e26-97e0-936bc20f1df&title=)

- id: 选择标识符
- select_type: 查询类型
- table: 输出结果集的表
- partitions: 匹配的分区
- type: 表的连接类型
- possible_keys: 查询时可能使用的索引
- key: 实际使用的索引
- key_len: 索引字段的长度
- ref: 列与索引的比较
- rows: 扫描出的行数
- filtered: 按表条件过滤的行百分比
- extra: 执行情况描述和说明

**1. id**
select标识符, 可以理解为SQL执行的顺序, 从大到小执行.
**id相同时执行顺序从上到下, 在所有组中, id值越大, 优先级越高, 越先执行**

**2. select_type**
查询中每个select子句的类型.

1. simple: 简单的select, 不适用union或子查询等. 例如: SELECT * from t_member where member_id = 1;
2. primary: 子查询中最外层查询, 查询中若包含任何复杂的子部分, 最外层的select被标记为primary. 例如: SELECT member_id from t_member where member_id = 3 UNION all SELECT member_id from t_member
3. union: union中的第二个或后面的select语句. 例如: SELECT member_id from t_member where member_id = 3 UNION all SELECT member_id from t_member
4. dependent union: union中第二个或后面的select, 取决于外层的查询. 例如SELECT tm.* from t_member as tm where member_id in (SELECT member_id from t_member where member_id = 3 UNION all SELECT member_id from t_member)
5. union result: union的结果集
6. subquery: 子查询中的第一个select. 例如: SELECT * from t_member where member_id = (SELECT member_id from t_member where member_id = 5)
7. dependent subquery: 子查询中的第一个select, 取决于外面的select. 例如: SELECT tm.* from t_member as tm where member_id in (SELECT member_id from t_member where member_id = 3 UNION all SELECT member_id from t_member)
8. derived: 派生表的select(from子句的子查询). 例如: SELECT * from (SELECT * from t_member where member_id = 1) table



**3. table**
显示这一步所访问数据库中表名称. 有时候不是真实的表明, 可能是简称.

**4. partitions**
官方定义为The matching partitions(匹配的分区),该字段看table所在的分区, 值为NULL表示表未被分区.

**5. type**
对表访问方式, 表示MySQL在表中找到所需行的方式, 又称“访问类型”. 常用的类型有： all、index、range、 ref、eq_ref、const、system(从左到右, 性能从差到好), 一般来说, 需要保证查询至少达到range级别, 最好能达到ref级别.

- all: Full Table Scan, 全表扫描.
- index: Full Index Scan, 全索引扫描. index与all区别为index只遍历索引树, 通常比all快, 因为索引文件通常比数据文件小.
- range: 只检索给定范围的行, 使用一个索引来检索行, 可以在key列中查看使用的索引, 一般出现在where条件中, 比如使用between, <, >, in等查询. 这种索引的范围扫描比全表扫描要好, 因为索引的开始点和结束点都固定, 不用扫描全索引.
- ref: 非唯一性索引扫描, 返回匹配某个单独值的所有行. 本质上也是一种索引访问, 返回匹配某值(某条件)的多行数据, 属于查找和扫描的混合体.
- eq_ref: 类似ref, 区别在于使用的索引是唯一索引, 对于每个索引键值, 表中只有一条记录匹配, 常见于主键或唯一索引扫描.
- const、system: 表示通过一次索引就找到了结果, 常见于primary key或unique索引, 因为只匹配一行数据, 所以查询非常快. 如将主键置于where列表中, MySQL就能将该查询转换为一个常量. system是const类型的特例, 当查询的表只有一行的情况下, 使用system.

**全表扫描是查询速度慢的一个主要原因。使用 EXPLAIN 语句并查找类型为 "ALL" 的查询，这些是全表扫描，使用索引优化这些查询。**

**6. possible_keys**
显示可能应用在表中的索引, 可能一个或多个. 查询涉及到的字段若存在索引, 则该索引将被列出, 但不一定被查询实际使用.

**7. key**
实际中使用的索引, 如为NULL, 则表示未使用索引. 要想强制MySQL使用或忽视possible_keys列中的索引, 在查询中使用FORCE INDEX、USE INDEX或者IGNORE INDEX.
例如，使用索引：
explain select * from tb_user use index(idx_user_pro) where profession="软件工程";
不使用哪个索引：
explain select * from tb_user ignore index(idx_user_pro) where profession="软件工程";
必须使用哪个索引：
explain select * from tb_user force index(idx_user_pro) where profession="软件工程";
use 是建议，实际使用哪个索引 MySQL 还会自己权衡运行速度去更改，force就是无论如何都强制使用该索引。

**8. key_len**
表示索引中所使用的字节数, 可通过该列计算查询中使用的索引长度. 在不损失精确性的情况下, 长度越短越好. key_len显示的值为索引字段的最大可能长度, 并非实际使用长度, 即key_len是根据表定义计算而得, 并不是通过表内检索出的.

**9. ref**
显示关联的字段. 如果使用常数等值查询, 则显示const, 如果是连接查询, 则会显示关联的字段.

**10. rows**
根据表统计信息及索引选用情况大致估算出找到所需记录所要读取的行数. 当然该值越小越好.

**11. filtered**
百分比值, 表示存储引擎返回的数据经过滤后, 剩下多少满足查询条件记录数量的比例.

**12. extra**
显示十分重要的额外信息. 其取值有以下几个：

- using filesort: 表明mysql会对数据使用一个外部的索引排序, 而不是按照表内的索引顺序进行读取. 在mysql中, 无法利用索引完成的排序操作称为"文件排序". 当出现using filesort时就非常危险了, 在数据量非常大的时候几乎"九死一生". 出现using filesort尽快优化sql语句.
- using temporary: 使用了临时表保存中间结果, 常见于排序order by和分组查询group by. 非常危险, “十死无生”, 急需优化.
- using index: 表明相应的select操作中使用了覆盖索引, 避免访问表的额外数据行, 效率不错. 如果同时出现了using where, 表明索引被用来执行索引键值的查找. 如果没有同时出现using where, 表明索引用来读取数据而非执行查找动作.
- using where: 不用读取表中所有信息, 仅通过索引就可以获取所需数据, 这发生在对表的全部的请求列都是同一个索引的部分的时候, 表示mysql服务器将在存储引擎检索行后再进行过滤.
- backward index scan: 反向扫描索引，例如

复合索引均为降序时 
CREATE INDEX idx_state_points ON customers (state desc, points desc);

using index condition：查找使用了索引，但是需要回表查询数据
using where; using index;：查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询
### 

DESCRIBE/DESC
describe 命令用于查看详细设计信息

## Views
### Creating Views
视图（View）是一种虚拟存在的表，**并没有真正储存数据**，数据仍然储存在原始表中，视图的数据在使用视图时动态生成的。
通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果。创建视图虚拟表，可以抽取一些重复性的查询模块，简化各种复杂操作（包括复杂的子查询和连接等）。
> **为什么不保存查询结果？**
> 1. 原表数据可能已经更新了
> 2. 如果视图所基于的表发生了变化，视图的查询结果也会相应地发生变化。例如，如果视图查询语句中包含了某个表的列，而该表的列发生了变化，那么查询视图时，视图的列也会相应地发生变化。

```sql
CREATE [ OR REPLACE ] VIEW 视图名称[（列1,列2...）] AS SELECT 语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]

CREATE VIEW sales_by_client AS  -- 如果已存在 VIEW，则报错
SELECT 
	c.client_id,
  c.name,
  SUM(invoice_total) AS total_sales
FROM clients c
JOIN invoices i USING (client_id)
GROUP BY client_id, name
```

### 查看视图
DESCRIBE/DESC 视图名；

### Altering or Dropping views
视图的查询语句可以在编辑模式下查看和修改，但最好是保存为 sql 文件并使用 git 控制

```sql
-- 方式一：
CREATE [OR REPLACE] VIEW 视图名称[（列名列表)）] AS SELECT 语句 [ WITH[ CASCADED | LOCAL ] CHECK OPTION ]

-- 方式二：
ALTER VIEW 视图名称 [（列名列表)] AS SELECT 语句 [WITH [CASCADED | LOCAL] CHECK OPTION]


DROP VIEW IF EXISTS sales_by_client;

CREATE OR REPLACE VIEW sales_by_client AS  -- 如果已存在 VIEW，则替换，否则创建 VIEW
SELECT 
	c.client_id,
  c.name,
  SUM(invoice_total) AS total_sales
FROM clients c
JOIN invoices i USING (client_id)
GROUP BY client_id, name
```

### Updatable Views
视图作为虚拟表/衍生表，除了可用在查询语句SELECT中，也可以用在增删改（INSERT DELETE UPDATE）语句中，但后者有一定的前提条件。
视图的更新操作可能会影响到原表的数据，但这取决于视图的定义和数据操作的条件。
如果一个视图的原始查询语句中没有如下元素：
1. DISTINCT 去重
2. GROUP BY/HAVING/聚合函数/窗口函数 (后三个通常是伴随着 GROUP BY 分组出现的)
3. UNION 或 UNION ALL 纵向连接

则该视图是可更新视图（Updatable Views），可以增删改，否则只能查。
另外，增（INSERT）还要满足附加条件：视图必须包含底层原表的所有**必须（必填且没有设置默认值）**字段
总之，一般通过原表修改数据，但当出于安全考虑或其他原因没有某表的直接权限时，可以**通过视图来修改底层数据**，前提是视图是可更新的。
```sql
CREATE OR REPLACE VIEW invoices_with_balance AS
SELECT 
		invoice_id,
    number,
    client_id,
    invoice_total,
    payment_total,
    invoice_total - payment_total AS balance,
    invoice_date,
    due_date,
    payment_date
FROM invoices
WHERE (invoice_total - payment_total) > 0 -- WHERE 先执行，无法使用 SELECT 中的别名

DELETE FROM invoices_with_balance
WHERE invoice_id = 1

UPDATE invoices_with_balance
SET due_date = DATE_ADD(due_date, INTERVAL 2 DAY)
WHERE invoice_id = 2
```

### WITH CHECK OPTION 子句
在视图的原始查询语句最后加上 WITH CHECK OPTION 可以防止执行会让视图中行（记录）消失的语句，例如更新或删除语句都可能会让视图中的行被删除
```sql
CREATE OR REPLACE VIEW invoices_with_balance AS
SELECT 
		invoice_id,
    number,
    client_id,
    invoice_total,
    payment_total,
    invoice_total - payment_total AS balance,
    invoice_date,
    due_date,
    payment_date
FROM invoices
WHERE (invoice_total - payment_total) > 0 -- WHERE 先执行，无法使用 SELECT 中的别名
-- 可以防止执行会让视图中行（记录）消失的语句
-- WITH CHECK OPTION

-- 以下语句可能会删除视图中 invoice_id = 2 的行，不会删除原表的行，但 UPDATE 依旧会改变原表数据
-- 因为上面的条件是 WHERE (invoice_total - payment_total) > 0 
-- 更新后可能不符合条件，会被删除
UPDATE invoices_with_balance
SET invoice_total = payment_total
WHERE invoice_id = 2
```

### 视图检查选项
当使用 WITH CHECK QPTION 子句创建视图时，MySQL会通过视图检查正在更改的每个行，例如插入，更新，删除，以使其符合视图的定义。MySQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性。为了确定检查的范围，mysql提供了两个选项：CASCADED 和 LOCAL ，**默认值为 CASCADED**。
#### CASCADED
级联，一旦设置了这个选项，除了会检查创建视图时候的条件，还会检查所依赖视图的条件。
```sql
-- 只要子句没有设置 with check option，则不会向上检查

create or replace view v1 as select id,name from student where id <= 20;
-- with cascaded check option 创建 v2 视图
-- with check option 会检查 v2 的条件
-- cascaded 会检查 v1 的条件，即使 v1 没有设置 with check option
create or replace view v2 as select id,name from v1 where id >= 10 with cascaded check option;
-- 创建 stu_v_3 视图，因为没有设置 with check option，不会检查 v3 的条件: id <= 15
-- 然后向上查找依赖的 v2
-- 因为 v2 设置了 cascaed ，会检查 v2 的条件，而且也会检查 v 2所依赖的 v1 的条件
-- 如果 v2 没有设置 with cascaed check option，则不会检查 v2 的条件，因此也不会检查 v1 的条件
create or replace view v3 as select id,name from v2 where id <= 15;
```

#### LOCAL
本地的条件也会检查，还会向上检查。在向上找的时候，就要看依赖视图是否开了检查选项，如果没开就不检查。和 CASCADED 的区别就是 CASCADED 不管上面开没开检查选项都会进行检查，即 CASCADED 会为上一级增加条件检查约束。
```sql
create or replace view v1 as select id,name from student where id <= 20;
-- with local check option 创建 v2 视图
-- with check option 会检查 v2 的条件
-- local 向上查找 v1 是否设置了 with check option，如果有，则检查 v1 的条件，否则不检查
create or replace view v2 as select id,name from v1 where id >= 10 with local check option;
-- 创建 stu_v_3 视图，因为没有设置 with check option，不会检查 v3 的条件: id <= 15
-- 然后向上查找依赖的 v2
-- 因为 v2 设置了 local ，会检查 v2 的条件
-- 然后再向上查找 v1 是否设置了 with check option，如果有，则检查 v1 的条件，否则不检查
-- 如果 v2 没有设置 with local check option，则不会检查 v2 的条件，因此也不会检查 v1 的条件
create or replace view v3 as select id,name from v2 where id <= 15;
```


### Other Benefits of Views
三大优点：
简化查询、增加抽象层和减少数据库设计改动的影响、数据安全性

1. （首要优点）简化查询 simplify queries

2. 增加抽象层，减少改动的影响 Reduce the impact of changes：视图给表增加了一个抽象层（模块化），这样如果数据库设计改变了（如字段改名、一个字段从一个表转移到了另一个表），只需修改视图的查询语句使其能保持原有查询结果即可，不需要修改使用这个视图的那几十个查询。相反，如果没有视图这一层的话，所有查询将直接使用指向原表的原始查询语句，这样一旦更改原表设计，就要相应地更改所有的这些查询。

3. 数据库可以授权，但不能授权到数据库特定行和特定的列上，限制对原数据的访问权限 Restrict access to the data：在视图中可以对原表的行和列进行筛选，如果你禁止了对原始表的访问权限，用户只能通过视图来修改数据，他们就无法查看和修改视图中未返回的那些字段和记录。

总而言之 类似于给表加上了一个外壳，通过这个外壳访问表的时候，只能按照所设计的方式进行访问与更新。
## 存储过程
### 什么是存储过程
存储过程是事先经过编译并存储在数据库中的一段SQL 语句的集合，调用存储过程可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。 存储过程思想上很简单，就是数据库SQL 语言层面的代码封装与重用。

存储过程三大作用：

1. 储存和管理SQL代码 Store and organize SQL
如果将SQL语句内嵌在应用程序的代码里，将使其混乱且难以维护，所以应该将SQL代码和应用程序代码分开，将SQL代码储存在所属的数据库中，具体来说，是放在储存过程（stored procedure）和函数中。

2. 性能优化 Faster execution 

大部分DBMS会对储存过程中的代码进行一些优化，因此有时储存过中的SQL代码执行起来会更快。

3. 数据安全 Data security 

就像视图一样，储存过程能加强数据安全。比如，我们可以移除对所有原始表的访问权限，让各种增删改的操作都通过储存过程来完成，然后就可以决定谁可以执行何种储存过程，用以限制用户对我们数据的操作范围，例如，防止特定的用户删除数据。

### Creating a Stored Procedure
BEGIN 和 END 之间包裹的是此过程（PROCEDURE）的内容（body），内容里可以有多个SQL语句，但每个语句都要以 ; 结束。
为了将过程内容内部的语句分隔符与SQL本身执行层面的语句分隔符 ; 区别开，要先用 DELIMITER(分隔符) 关键字暂时将SQL语句的默认分隔符改为其他符号，一般是改成双美元符号 $$ ，创建过程结束后再改回来。注意创建过程本身也是一个完整SQL语句，所以别忘了在END后要加一个暂时语句分隔符 $$

```sql
DELIMITER $$

CREATE PROCEDURE 存储过程名称 ([ 参数列表 ])
BEGIN
	-- SQL语句
END$$

DELIMITER ;


-- 创建视图、存储过程、函数、触发器、事件等之前一般会删除原有的，以保证团队数据和代码一致
DROP PROCEDURE IF EXISTS get_clients;

DELIMITER $$

CREATE PROCEDURE get_clients()
BEGIN 
	SELECT * FROM clients;
END$$

DELIMITER ;


CALL get_clients([参数])

-- or

CALL sql_invoicing.get_clients([参数])

```


**使用MySQL工作台创建存储过程**
也可以用点击的方式创造过程，右键选择 Create Stored Procedure，填空，Apply。这种方式 Workbench 会帮你处理暂时修改分隔符的问题
这种方式一样可以储存SQL文件。

### Inspecting Stored Procedure
查询指定数据库的存储过程及状态信息
```sql
SELECT* FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA = 'database_name';
```

存储过程名称；--查询某个存储过程的定义
```sql
SHOW CREATE PROCEDURE procedure_name;
```

### Dropping Stored Procedure
同视图一样，最好把删除和创建每一个过程的代码也储存在不同的SQL文件中，并把这样的文件放在 Git 这样的源码控制下，这样就能与其它团队成员共享 Git 储存库。他们就能在自己的机器上重建数据库以及该数据库下的所有的视图和储存过程
```sql
DROP PROCEDURE IF EXISTS get_clients;
```

### Parameters
参数的类型，主要分为以下三种：IN、OUT、INOUT。 具体的含义如下：

| 类型 | 含义 | 备注 |
| --- | --- | --- |
| IN | 该类参数作为输入，也就是需要调用时传入值 | 默认 |
| OUT | 该类参数作为输出，也就是该参数可以作为返回值 |  |
| INOUT | 既可以作为输入参数，也可以作为输出参数 |  |


```sql
CREATE PROCEDURE 存储过程名称 ([ IN/OUT/INOUT 参数名 参数类型 ])
BEGIN
	-- SQL语句
END ;


DELIMITER $$
-- 多个参数可用逗号隔开
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	SELECT * FROM clients c
    WHERE c.state = state;
END $$
DELIMITER $$

CALL get_clients_by_state('CA');
```

根据传入参数score，判定当前分数对应的分数等级，并返回。

- score >= 85分，等级为优秀。
- score >= 60分 且 score < 85分，等级为及格。
- score < 60分，等级为不及格。
```sql
create procedure p4(in score int,out result varchar(10))
begin 
		if score < 60 then
				set result := '不及格';
		elseif score < 85 then
				set result := '及格';
		else
				set result := '优秀';
		end if;
end;

-- 定义用户变量 @result来接收返回的数据, 用户变量可以不用声明
call p4(76, @result);

select @result;
```

将**传入**的200分制的分数，进行换算，换算成百分制，然后**返回**。
```sql
create procedure p5(inout score double)
begin 
	set score := score*0.5;
end;

set @score := 198;
call p5(@score);

select @score;
```


### Parameters with Default Value
```sql
DELIMITER $$
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	IF state IS NULL THEN
		SET state = 'CA';
	END IF;
	
	SELECT * FROM clients c
  WHERE c.state = state;
END $$
DELIMITER ;

-- 必须传参，即使参数为空
CALL get_clients_by_state(NULL);


DELIMITER $$
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	SELECT * FROM clients c
  WHERE c.state = IFNULL(state, c.state);
END $$
DELIMITER ;

```

### Parameter Validation
过犹不及（"Too much of a good thing is a bad thing"），加入过多的参数验证会让代码过于复杂难以维护。
参数验证工作更多的应该在应用程序端接受用户输入数据时就检测和报告，那样更快也更有效。
储存过程里的参数验证只是在有人越过应用程序直接访问储存过程时作为最后的防线。这里只应该写那些最关键和必要的参数验证。
```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `make_payment`(
		invoice_id INT,
    payment_amount DECIMAL(9, 2),
    payment_date DATE
)
BEGIN
	IF payment_amount <= 0 THEN
				SIGNAL SQLSTATE '22003'
        SET MESSAGE_TEXT = 'Invalid payment amount'; -- 抛出异常
	END IF;
        
	UPDATE invoices i
    SET 
				i.payment_total = payment_amount,
        i.payment_date = payment_date
		WHERE i.invoice_id = invoice_id;
END
```

### Output parameter
在参数的前面加上 OUT 关键字，然后再 SELECT 后加上 INTO
```sql
CREATE PROCEDURE `get_unpaid_invoices_for_client`(
	client_id INT,
    OUT invoices_count INT,
    OUT invoices_total DECIMAL(9, 2)
)
BEGIN
		SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices i
    WHERE i.client_id = client_id 
		AND payment_total = 0;
END;

set @invoices_count = 0;
set @invoices_total = 0;
call sql_invoicing.get_unpaid_invoices_for_client(3, @invoices_count, @invoices_total);
select @invoices_count, @invoices_total;
```

### Variables
在MySQL中变量分为三种类型: 系统变量、用户自定义变量、局部变量。
#### 系统变量
系统变量 是MySQL服务器提供，不能由用户定义的，属于服务器层面。分为全局变量（GLOBAL）、会话变量（SESSION）。

1. 查看系统变量
```sql
SHOW [ SESSION | GLOBAL ] VARIABLES ;               -- 查看所有系统变量
SHOW [ SESSION | GLOBAL ] VARIABLES LIKE '......';  -- 可以通过LIKE模糊匹配方式查找变量
SELECT @@[SESSION | GLOBAL].系统变量名;               -- 查看指定变量的值 
```

2. 设置系统变量
```sql
SET [ SESSION | GLOBAL ] 系统变量名 = 值 ;
```

> 如果没有指定SESSION/GLOBAL，默认是SESSION，会话变量。
> mysql服务重新启动之后，所设置的全局参数会失效，要想不失效，可以在 /etc/my.cnf中配置。
> - 全局变量(GLOBAL): 全局变量针对于所有的会话。
> - 会话变量(SESSION): 会话变量针对于单个会话，在另外一个会话窗口就不生效了。



#### 用户自定义变量
与 SESSION 变量一样，用户自定义变量将在整个用户会话期间存续，在会话结束断开MySQL链接时才被清空，这种变量主要在调用带输出的储存过程时，作为输出参数来获取结果值。
```sql
-- 赋值时，可以使用= ，也可以使用:= ,推荐使用:=
SET @var_name = expr [, @var_name = expr] ... ;
SET @var_name := expr [, @var_name := expr] ... ;

-- 可通过 SELECT 复制
SELECT @var_name := expr [, @var_name := expr] ... ;
SELECT 聚合函数 INTO @var_name FROM 表名;

SELECT @var_name;
```
> 可以直接使用未声明和初始化的用户自定义变量，但是值为 NULL


#### 局部变量（Local variable）
在储存过程或函数中通过 DECLARE 声明并使用，在函数或储存过程执行结束时就被清空，常用来执行过程（或函数）中的计算
```sql
-- 声明
DECLARE 变量名 变量类型 [DEFAULT 默认值 ] ;

-- 赋值
SET 变量名 = 值 ;
SET 变量名 := 值 ;
SELECT 聚合函数 INTO 变量名 FROM 表名 ... ;

CREATE DEFINER=`root`@`localhost` PROCEDURE `get_risk_factor`()
BEGIN
		DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
    DECLARE invoices_total DECIMAL(9, 2);
    DECLARE invoices_count INT;
    
    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices;
	-- risk_factor = invoices_total / invoices_count * 5
    SET risk_factor = invoices_total / invoices_count * 5;
    
    SELECT risk_factor;
END
```

### IF
```sql
IF 条件1 THEN
	.....
ELSEIF 条件2 THEN     -- 可选
	.....
ELSE 				 -- 可选
	.....
END IF;

```

根据定义的分数score变量，判定当前分数对应的分数等级。

- score >= 85分，等级为优秀。
- score >= 60分 且 score < 85分，等级为及格。
- score < 60分，等级为不及格。
```sql
CREATE PROCEDURE p3()
begin 
		declare score int default 68;
		declare result varchar(10);
		
		if score < 60 then
				set result := '不及格';
		elseif score < 85 then
				set result := '及格';
		else
				set result := '优秀';
		end if;
		
		select result;
end;

call p3();

```

### CASE
语法1：
```sql
-- 含义： 当case_value的值为 when_value1时，执行statement_list1，当值为 when_value2时，执行statement_list2， 否则就执行 statement_list
CASE case_value
	WHEN when_value1 THEN statement_list1
	[ WHEN when_value2 THEN statement_list2] ...
	[ ELSE statement_list ]
END;
```
语法2：
```sql
-- 含义： 当条件search_condition1成立时，执行statement_list1，当条件search_condition2成立时，执行statement_list2， 否则就执行 statement_list
CASE
	WHEN search_condition1 THEN statement_list1
	[WHEN search_condition2 THEN statement_list2] ...
	[ELSE statement_list]
END;
```

```sql
SELECT
	order_id,
  CASE
  	WHEN YEAR(order_date) = YEAR(NOW()) THEN 'Active'
    WHEN YEAR(order_date) = YEAR(NOW()) - 1 THEN 'Last Year'
    WHEN YEAR(order_date) < YEAR(NOW()) - 1 THEN 'Archived'
    ELSE 'Future' -- ELSE 子句可选
	END AS category
FROM orders
```

### WHILE
while 循环是有条件的循环控制语句。满足条件后，再执行循环体中的SQL语句。具体语法为：
```sql
-- 先判定条件，如果条件为true，则执行逻辑，否则，不执行逻辑
WHILE 条件 DO
	SQL逻辑...
END WHILE;
```

案例
计算从1累加到n的值，n为传入的参数值。
```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行减1 , 如果n减到0, 则退出循环
DELIMITER $$
create procedure p7(in n int)
begin
		declare total int default 0;
		
		while n>0 do
			set total := total + n;
			set n := n-1;
		end while;
		
		select total;
end$$
DELIMITER ;

call p7(100);

```

### REPEAT
repeat是有条件的循环控制语句, 当满足until声明的条件的时候，则退出循环 
```sql
-- 先执行一次逻辑，然后判定UNTIL条件是否满足，如果满足，则退出。如果不满足，则继续下一次循环
REPEAT
	SQL逻辑...
	UNTIL 条件
END REPEAT;
```
案例
计算从1累加到n的值，n为传入的参数值
```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行-1 , 如果n减到0, 则退出循环
DELIMITER $$
create procedure p8(in n int)
begin
	declare total int default 0;
	
	repeat
			set total := total + n;
			set n := n - 1;
	until n <=0
	end repeat;
	
	select total;
end$$
DELIMITER ;

call p8(100);		

```

### LOOP
LOOP 实现简单的循环，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环。LOOP可以配合一下两个语句使用：

- LEAVE ：配合循环使用，退出循环。
- ITERATE：必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环。
```sql
[begin_label:] LOOP
	SQL逻辑...
END LOOP [end_label];

LEAVE label; 		-- 退出指定标记的循环体
ITERATE label; 		-- 直接进入下一次循环
```

案例一
计算从1累加到n的值，n为传入的参数值。
```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行-1 , 如果n减到0, 则退出循环 ----> leave x
DELIMITER $$
create procedure p9(in n int)
begin 
		declare total int default 0;
		
		sum:loop
				if n<=0 then
						leave sum;
				end if;
				
				set total := total + n;
				set n := n - 1;
		end loop sum;
		
		select total;
end$$
DELIMITER ;

call p9(10);

```

案例2
计算从1到n之间的偶数累加的值，n为传入的参数值。
```sql
-- A. 定义局部变量, 记录累加之后的值;
-- B. 每循环一次, 就会对n进行-1 , 如果n减到0, 则退出循环 ----> leave xx
-- C. 如果当次累加的数据是奇数, 则直接进入下一次循环. --------> iterate xx
DELIMITER $$
create procedure p10(in n int)
begin
		declare total int default 0;
		sum:loop
				if n<=0 then
					leave sum;
				end if;						
				if n%2=1 then
					set n := n - 1;
					iterate sum;
				end if;	
				set total := total + n;
				set n := n - 1;
		end loop sum;
		select total;
end$$
DELIMITER ;

call p10(10);

```

### CURSOR 游标
**游标（CURSOR）是用来存储查询结果集的数据类型** , 在存储过程和函数中可以使用游标对结果集进行循环的处理。游标的使用包括游标的声明、OPEN、FETCH 和 CLOSE，其语法分别如下。声
```sql
-- 声明游标
DECLARE 游标名称 CURSOR FOR 查询语句 ;

-- 打开游标
OPEN 游标名称 ;

-- 获取游标记录
FETCH 游标名称 INTO 变量 [, 变量 ] ;

-- CLOSE 游标名称 ;
CLOSE 游标名称 ;
```

案例
根据传入的参数uage，来查询用户表tb_use中年龄小于等于uage的用户姓名（name）和专业（profession），并将用户的姓名和专业插入到所创建的一张新表(id,name,profession)中。
> 注意，声明自定义变量要写在声明游标前面

```sql
-- 逻辑:
-- A. 声明游标, 存储查询结果集
-- B. 准备: 创建表结构
-- C. 开启游标
-- D. 获取游标中的记录
-- E. 插入数据到新表中
-- F. 关闭游标
DELIMITER $$
create procedure  p11(in uage int)
begin 
		declare uname varchar(100);
		declare upro varchar(100);
		declare u_cursor cursor for select name,profession from tb_user where age <= uage;
		
		drop table if exists tb_user_pro;
		create table if not exists tb_user_pro(
				id int primary key auto_increment,
				name varchar(100),
				profession varchar(100)
		);
		
		open u_cursor;
		while true do
				fetch u_cursor into uname,upro;
				insert into tb_user values (null,uname,upro);
		end while;
		
		close u_cursor;
end$$
DELIMITER ;

call p11(40);
```

上述的存储过程，最终我们在调用的过程中，会报错，之所以报错是因为上面的while循环中，并没有退出条件。当游标的数据集获取完毕之后，没有可获取了，就会报错，从而终止了程序的执行。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675487630645-4ce1ef68-8bf4-4288-a31e-e9105f2f0553.webp#averageHue=%23faf5f3&clientId=ucd54c1fc-0c0a-4&from=paste&id=udaa09840&originHeight=144&originWidth=736&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ud26b1ebe-2384-40b4-a5e5-c7a427061b3&title=)
但是此时，tb_user_pro表结构及其数据都已经插入成功了，我们可以直接刷新表结构，检查表结构中的数据。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675487630664-b16e7d3a-4c9c-4adb-84c7-0ba20458e32b.webp#averageHue=%23e7e6de&clientId=ucd54c1fc-0c0a-4&from=paste&id=ue65461b0&originHeight=482&originWidth=316&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uc8fb9498-62c1-4f34-8dbc-e519cfa953d&title=)
上述的功能，虽然我们实现了，但是逻辑并不完善，而且程序执行完毕，获取不到数据，数据库还报错。 接下来，我们就需要来完成这个存储过程，并且解决这个问题。
要想解决这个问题，就需要通过MySQL中提供的 条件处理程序 Handler 来解决。

### HANDLER
条件处理程序（Handler）可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤
```sql
DECLARE handler_action HANDLER FOR condition_value [, condition_value] ... statement ;

handler_action 的取值：
	CONTINUE: 继续执行当前程序
	EXIT: 终止执行当前程序
	UNDO: 不做操作
	
condition_value 的取值：
	mysql_error_code
	condition_name: https://dev.mysql.com/doc/refman/8.0/en/declare-condition.html
	SQLSTATE sqlstate_value: 状态码，如 02000
	SQLWARNING: 所有以01开头的SQLSTATE代码的简写
	NOT FOUND: 所有以02开头的SQLSTATE代码的简写
	SQLEXCEPTION: 所有没有被SQLWARNING 或 NOT FOUND捕获的SQLSTATE代码的简写
```
具体的错误状态码，可以参考官方文档：
[https://dev.mysql.com/doc/refman/8.0/en/declare-handler.html(opens new window)](https://dev.mysql.com/doc/refman/8.0/en/declare-handler.html)
[https://dev.mysql.com/doc/mysql-errors/8.0/en/server-error-reference.html](https://dev.mysql.com/doc/refman/8.0/en/declare-handler.html)

案例
我们继续来完成在上一小节提出的这个需求，并解决其中的问题。
根据传入的参数uage，来查询用户表tb_user中，所有年龄小于等于uage的用户姓名（name）和专业（profession），并将用户的姓名和专业插入到所创建的一张新表(id,name,profession)中。

```sql
create procedure p12(in uage int)
begin 
		declare uname varchar(100);
		declare upro varchar(100);
		declare u_cursor cursor for select name,profession from tb_user where age <= uage;
		-- 声明条件处理程序 ： 当SQL语句执行抛出的状态码为02000时，将关闭游标u_cursor，并退出
		declare exit hander for sqlstate '02000' close u_cursor;
  	-- declare exit hander for sqlstate NOT FOUND close u_cursor;
		
		drop table if exists tb_user_pro;
		create table if not exists tb_user_pro(
				id int primary key auto_increment,
				name varchar(100),
				profession varchar(100)
		);
		
		open u_cursor;
		while true do
				fetch u_cursor into uname,upro;
				insert into tb_user_pro values (null,uname,upro);
		end while;
		
		close u_cursor;
end;

call p12(40);
```


### Functions
函数和储存过程的作用非常相似，唯一区别是函数只能返回单一值而不能返回多行多列的结果集，当你只需要返回一个值时就可以创建函数。
存储函数的参数只能是IN类型的。
存储函数能做的事情，存储过程一般也能做。
和视图、过程一样，也最好存入SQL文件并加入源码控制
在mysql8.0版本中binlog默认是开启的，一旦开启了，mysql就要求在定义存储函数时，需要指定characteristic特性，否则就会报错
```sql
CREATE FUNCTION 存储函数名称 ([ 参数列表 ])
RETURNS type [characteristic ...]
BEGIN
	-- SQL语句
	RETURN ...;
END ;



1. 参数设置和body内容之间，有一段确定返回值类型以及函数属性的语句段
RETURNS INTEGER

characteristic说明：
NO SQL -- 不包含 SQL 语句
DETERMINISTIC -- 相同的输入参数总是产生相同的结果，唯一输入决定唯一输出，和数据的改动更新无关
READS SQL DATA -- 包含读取数据的语句，但不包含写入数据的语句
MODIFIES SQL DATA -- 函数中有插入、更新、删除操作
……

2. 最后是返回（RETURN）值而不是查询（SELECT）值
RETURN IFNULL(risk_factor, 0);
```

```sql
CREATE DEFINER=`root`@`localhost` FUNCTION `get_risk_factor_for_client`(
	client_id INT
) 
RETURNS int
READS SQL DATA
BEGIN
		DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
    DECLARE invoices_total DECIMAL(9, 2);
    DECLARE invoices_count INT;
    
    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices i
    WHERE i.client_id = client_id;
    
    SET risk_factor = invoices_total / invoices_count * 5;
    
		RETURN IFNULL(risk_factor, 0);
END;

SELECT
		client_id,
    name,
    get_risk_factor_for_client(client_id) AS risk_factor
FROM clients;
```

删除
```sql
DROP FUNCTION [IF EXISTS] 函数名
```



## 触发器和事件
### Triggers
触发器是在插入、更新或删除语句前后自动执行的一段SQL代码，通常我们使用触发器来保持数据的一致性
创建触发器的语法要点：命名三要素，触发条件语句和触发频率语句，主体中 OLD/NEW 的使用

| 触发器类型 | NEW和OLD |
| --- | --- |
| INSERT 型触发器 | NEW 表示将要或者已经新增的数据 |
| UPDATE 型触发器 | OLD 表示修改之前的数据 , NEW 表示将要或已经修改后的数据 |
| DELETE 型触发器 | OLD 表示将要或者已经删除的数据 |

语法上，和创建储存过程等类似，要暂时更改分隔符，用 CREATE 关键字，用 BEGIN 和 END 包裹的主体
几个关键点：
1. 命名习惯（三要素）：触发表_before/after(SQL语句执行之前或之后触发)_触发的SQL语句类型
2. 触发条件语句：BEFORE/AFTER INSERT/UPDATE/DELETE ON 触发表
3. 触发频率语句：这里 FOR EACH ROW 表明每一个受影响的行都会启动一次触发器。
MySQL 触发器现在只支持行级触发。其它有的DBMS还支持表级别的触发器，即不管插入一行还是五行都只启动一次触发器。
4. 主体：主体里可以对各种表的数据进行修改以保持数据一致性，但注意唯一不能修改的表是触发表，否则会引发无限循环（“触发器自燃”），主体中最关键的是使用 OLD/NEW 关键字来指代受影响的旧/新行（若INSERT用NEW，若DELETE用OLD，若UPDATE两个都可以用）并可跟 '点+字段' 以引用这些行的相应属性
```sql
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE
ON tbl_name FOR EACH ROW -- 行级触发器
BEGIN
	trigger_stmt ;
END;


DELIMITER $$

CREATE TRIGGER payments_after_insert
    AFTER INSERT ON payments
    FOR EACH ROW
BEGIN
    UPDATE invoices
    SET payment_total = payment_total + NEW.amount
    WHERE invoice_id = NEW.invoice_id;
END$$

DELIMITER ;
```

### 查看触发器
用以下命令来查看已存在的触发器及其各要素
```sql
SHOW TRIGGERS;
```
如果之前创建时遵行了三要素命名习惯，这里也可以用 LIKE 关键字来筛选特定表的触发器
```sql
SHOW TRIGGERS LIKE 'payments%'
```

### 删除触发器
```sql
DROP TRIGGER [IF EXISTS] payments_after_insert
```

### Using Triggers for Auditing
```sql
DROP TRIGGER IF EXISTS payments_after_insert;

DELIMITER $$
CREATE TRIGGER payments_after_insert
    AFTER INSERT ON payments
    FOR EACH ROW
BEGIN
    UPDATE invoices
    SET payment_total = payment_total + NEW.amount
    WHERE invoice_id = NEW.invoice_id;
    
    -- 往 payments_audit 表里添加日志记录的语句
    INSERT INTO payments_audit
    VALUES (NEW.client_id, NEW.date, NEW.amount, 'Insert', NOW());
END$$
DELIMITER ;

```
往 payments_after_insert 的主体里加上这样的语句：
```sql
INSERT INTO payments_audit 
VALUES (NEW.client_id, NEW.date, NEW.amount, 'insert', NOW());
```
往 payments_after_delete 的主体里加上这样的语句：
```sql
INSERT INTO payments_audit 
VALUES (OLD.client_id, OLD.date, OLD.amount, 'delete', NOW());
```


### Events
事件是一段根据计划执行的任务，可以执行一次，或者按某种规律执行，比如每天早上10点或每月一次
通过事件我们可以自动化数据库维护任务，比如删除过期数据、将数据从一张表复制到存档表 或者 汇总数据生成报告，所以事件十分有用
查看MySQL所有系统变量：
```sql
SHOW VARIABLES;
SHOW VARIABLES LIKE 'event%'; -- 使用 LIKE 操作符查找以event开头的系统变量
```
用SET语句开启或关闭,不想用事件时可关闭以节省资源，这样就不会有一个不停寻找需要执行的事件的后台程序
```sql
SET GLOBAL event_scheduler = ON/OFF
```

```sql
DELIMITER $$
CREATE EVENT yearly_delete_stable_audit_rows
ON SCHEDULE
  	-- 若只执行一次就用 AT 关键字
		-- AT '2019-05-01'
  	-- 开始 STARTS 和结束 ENDS 时间都是可选的
    EVERY 1 YEAR STARTS '2019-01-01'  ENDS '2029-01-01'
DO BEGIN
		DELETE FROM payments_aduit
    WHERE action_date < NOW() - INTERVAL 1 YEAR;
END $$
DELIMITER ;
```

### 查看、更改和删除事件
```sql
SHOW EVENTS 
-- 可看到各个数据库的事件
SHOW EVENTS [LIKE 'yearly%'];  

DROP EVENT IF EXISTS yearly_delete_stale_audit_rows;
```

1. 如果要修改事件内容（包括执行计划和主体内容），直接把 ALTER 当 CREATE 用
2. 暂时地启用或停用事件（用 DISABLE 和 ENABLE 关键字）
```sql
DELIMITER $$
ALTER EVENT yearly_delete_stable_audit_rows
ON SCHEDULE
  	-- 若只执行一次就用 AT 关键字
		-- AT '2019-05-01'
  	-- 开始 STARTS 和结束 ENDS 时间都是可选的
    EVERY 1 YEAR STARTS '2019-01-01'  ENDS '2029-01-01'
DO BEGIN
		DELETE FROM payments_aduit
    WHERE action_date < NOW() - INTERVAL 1 YEAR;
END $$
DELIMITER ;

-- 暂时地启用或停用事件
ALTER EVENT yearly_delete_stable_audit_rows DISABLE/ENABLE;
```

## 锁
锁是计算机协调多个进程或线程并发访问某一资源的机制。在数据库中，除传统的计算资源（CPU、RAM、I/O）的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。
MySQL中的锁，按照锁的粒度分，分为以下三类：

- 全局锁：锁定数据库中的所有表。
- 表级锁：每次操作锁住整张表。
- 行级锁：每次操作锁住对应的行数据。

### 全局锁
全局锁就是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML的写语句，DDL语句，已经更新操作的事务提交语句都将被阻塞。
其典型的使用场景是做全库的逻辑备份，对所有的表进行锁定，从而获取一致性视图，保证数据的完整性。
为什么全库逻辑备份，就需要加全就锁呢？
A. 我们一起先来分析一下不加全局锁，可能存在的问题。
假设在数据库中存在这样三张表: tb_stock 库存表，tb_order 订单表，tb_orderlog 订单日志表。

![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675583342832-75035b7e-ef2e-4fee-ad36-324659a8abf8.webp#averageHue=%23faf3d1&clientId=ucd54c1fc-0c0a-4&from=paste&id=u48b633ac&originHeight=353&originWidth=1370&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u0367de8f-5b2e-45dd-8284-a21cd3aad07&title=)

- 在进行数据备份时，先备份了tb_stock库存表。
- 然后接下来，在业务系统中，执行了下单操作，扣减库存，生成订单（更新tb_stock表，插入tb_order表）。
- 然后再执行备份 tb_order表的逻辑。
- 业务中执行插入订单日志操作。
- 最后，又备份了tb_orderlog表。

此时备份出来的数据，是存在问题的。因为备份出来的数据，tb_stock表与tb_order表的数据不一致(有最新操作的订单信息,但是库存数没减)。
那如何来规避这种问题呢? 此时就可以借助于MySQL的全局锁来解决。
B. 再来分析一下加了全局锁后的情况
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675583397636-2777b7e8-c078-44ef-accb-ec3ee3dacd55.webp#averageHue=%23fefdfb&clientId=ucd54c1fc-0c0a-4&from=paste&id=u0c49e443&originHeight=434&originWidth=1457&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uf6e721f0-e1d5-4caa-98ac-b71323e729a&title=)
对数据库进行进行逻辑备份之前，先对整个数据库加上全局锁，一旦加了全局锁之后，其他的DDL、DML全部都处于阻塞状态，但是可以执行DQL语句，也就是处于只读状态，而数据备份就是查询操作。那么数据在进行逻辑备份的过程中，数据库中的数据就是不会发生变化的，这样就保证了数据的一致性和完整性。

```sql
-- 加全局锁
flush tables with read lock;

-- 数据备份 -uUSER_NAME -pPASSWORD  备份 itcast 数据库到 itcast.sql 文件
mysqldump -uroot –p1234 itcast > itcast.sql（文件路径）

-- 释放锁
unlock tables;
```

数据库中加全局锁，是一个比较重的操作，存在以下问题：

- 如果在主库上备份，那么在备份期间都不能执行更新，业务基本上就得停摆。
- 如果在从库上备份，那么在备份期间从库不能执行主库同步过来的二进制日志（binlog），会导致主从延迟。

在InnoDB引擎中，我们可以在备份时加上参数 --single-transaction 参数来完成不加全局锁的一致性数据备份。
```sql
mysqldump --single-transaction -uroot –p123456 itcast > itcast.sql
```



### 表级锁
表级锁，每次操作锁住整张表。锁定粒度大，发生锁冲突的概率最高，并发度最低。应用在MyISAM、InnoDB、BDB等存储引擎中。
对于表级锁，主要分为以下三类：

- 表锁
- 元数据锁（meta data lock，MDL）
- 意向锁

#### 表锁
对于表锁，分为两类：

- 表共享读锁（read lock）
- 表排他写锁（write lock）

语法：

- 加锁：lock tables 表名... read/write。
- 释放锁：unlock tables / 客户端断开连接 。 UNLOCK TABLES 释放被当前线程持有的任何锁。当线程**发出另外一个**LOCK TABLES时，**或当客户端被关闭时**，当前线程锁定的所有表会**自动被解锁**。 

特点:
A. 读锁
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675584407337-386d77e1-f2d7-446a-86e1-f0cd13384e08.webp#averageHue=%23faf8f8&clientId=ucd54c1fc-0c0a-4&from=paste&id=uab999432&originHeight=326&originWidth=1231&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u85a0ed4e-32ac-4cb3-8243-f77d43e93f6&title=)
左侧为客户端一，对指定表加了读锁，不会影响右侧客户端二的读，但是会阻塞右侧客户端的写。
测试:
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675584438370-1ca67038-2808-44c3-ab6b-93d3de7edfe4.webp#averageHue=%2343403b&clientId=ucd54c1fc-0c0a-4&from=paste&id=uede7d1b8&originHeight=544&originWidth=1894&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub4c879ae-c8cf-41c4-bffd-c81aa08e5d0&title=)
B.写锁
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675584436516-10418987-2f5f-478c-a2e3-3326f43a4aeb.webp#averageHue=%23f9f8f8&clientId=ucd54c1fc-0c0a-4&from=paste&id=uf05a35ae&originHeight=317&originWidth=1263&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uc09d525a-80eb-42e1-9042-b3258daa7b6&title=)
左侧为客户端一，对指定表加了写锁，会阻塞右侧客户端的读和写。
测试:
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675584434496-6240706c-0676-400a-b374-35e4597fd6aa.webp#averageHue=%234c463d&clientId=ucd54c1fc-0c0a-4&from=paste&id=u9fd69f55&originHeight=320&originWidth=1546&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u7030030b-e388-4fff-8819-02f89e8aefc&title=)
**结论**
读锁不会阻塞其他客户端的读，但是会阻塞写。写锁既会阻塞其他客户端的读写操作。
共享读，排他写

#### 元数据锁
#### meta data lock , 元数据锁，简写MDL。
MDL加锁过程是系统自动控制，无需显式使用，在访问一张表的时候会自动加上。MDL锁主要作用是维护表元数据的数据一致性，在表上有活动事务的时候，不可以对元数据进行写入操作。**为了避免DML与DDL冲突，保证读写的正确性**。
在MySQL5.5中引入了MDL，当对一张表进行增删改查的时候，加MDL读锁(共享)；当对表结构进行变更操作的时候，加MDL写锁(排他)。
常见的SQL操作时，所添加的元数据锁：

| 对应SQL | 元数据锁类型 | 说明 |
| --- | --- | --- |
| lock tables xxx read/write | SHARED_READ_ONLY / SHARED_NO_READ_WRITE |  |
| select 、select ... lock in share mode | SHARED_READ | 与SHARED_READ、SHARED_WRITE兼容，与EXCLUSIVE互斥 |
| insert 、update、delete、select ... for update | SHARED_WRITE | 与SHARED_READ、SHARED_WRITE兼容，与EXCLUSIVE互斥 |
| alter table ... | EXCLUSIVE | 与其他的MDL都互斥 |

演示，注意下面的事例的事务是执行中的状态，未提交：
当执行SELECT、INSERT、UPDATE、DELETE等语句时，添加的是元数据共享锁（SHARED_READ / SHARED_WRITE），之间是兼容的。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675585971318-3e0b73c6-ce0f-46d2-a146-511eaf4c13dc.webp#averageHue=%23444038&clientId=ucd54c1fc-0c0a-4&from=paste&id=ue40cffd4&originHeight=309&originWidth=1520&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u48aff425-a3c6-438a-b598-68bbe6f2437&title=)
当执行SELECT语句时，添加的是元数据共享锁（SHARED_READ），会阻塞元数据排他锁（EXCLUSIVE），之间是互斥的。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675585971817-9406525f-6383-43d8-bc54-54e5fc88bc04.webp#averageHue=%233b3a35&clientId=ucd54c1fc-0c0a-4&from=paste&id=u214eb141&originHeight=574&originWidth=1908&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u2fcf16e2-aeca-41ce-8df6-ee2533cfee6&title=)
我们可以通过下面的SQL，来查看数据库中的元数据锁的情况：
```sql
select object_type,object_schema,object_name,lock_type,lock_duration from performance_schema.metadata_locks;
```
我们在操作过程中，可以通过上述的SQL语句，来查看元数据锁的加锁情况。
```sql
mysql> select object_type,object_schema,object_name,lock_type,lock_duration from performance_schema.metadata_locks;
+-------------+--------------------+----------------+--------------+---------------+
| object_type | object_schema      | object_name    | lock_type    | lock_duration |
+-------------+--------------------+----------------+--------------+---------------+
| TABLE       | MySQL_Advanced     | tb_user        | SHARED_READ  | TRANSACTION   |
| TABLE       | MySQL_Advanced     | tb_user        | SHARED_READ  | TRANSACTION   |
| TABLE       | MySQL_Advanced     | tb_user        | SHARED_WRITE | TRANSACTION   |
| TABLE       | MySQL_Advanced     | user_logs      | SHARED_WRITE | TRANSACTION   |
| TABLE       | performance_schema | metadata_locks | SHARED_READ  | TRANSACTION   |
+-------------+--------------------+----------------+--------------+---------------+
5 rows in set (0.00 sec)
mysql> alter table tb_user add column java int;
...阻塞

```


#### 意向锁
为了避免DML在执行时，加的行锁与表锁的冲突，在InnoDB中引入了意向锁，使得表锁不用检查每行数据是否加锁。
假如没有意向锁，客户端一对表加了行锁后，客户端二如何给表加表锁呢，来通过示意图简单分析一下：
首先客户端一，开启一个事务，然后执行DML操作，在执行DML语句时，会对涉及到的行加行锁。
当客户端二，想对这张表加表锁时，会逐行检查每行是否有行锁，且根据行锁的类型来判断是否可以添加表锁，效率较低。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675586850647-ca141236-a1de-497b-abf5-82fabf057d42.webp#averageHue=%23fbfaf9&clientId=ucd54c1fc-0c0a-4&from=paste&id=ue222dfb5&originHeight=488&originWidth=1552&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u45fc1029-8164-4c16-988f-65f2200355a&title=)
有了意向锁之后 :
客户端一，在执行DML操作时，会对涉及的行加行锁，同时也会对该表加上意向锁。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675586876867-07979b3c-4da3-45dd-a032-738fd729b67b.webp#averageHue=%23e4e1da&clientId=ucd54c1fc-0c0a-4&from=paste&id=u1aeb385d&originHeight=589&originWidth=1385&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u203f6abf-d613-42ff-ae1a-467b964171e&title=)
而其他客户端，在对这张表加表锁的时候，会根据该表上所加的意向锁来判定是否可以加表锁，而不用逐行判断行锁情况了，如果意向锁与表锁不兼容时，表锁添加会被阻塞，直至事务结束释放意向锁
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675586931684-dfcea0f8-7f56-47ef-9443-9c119a7f7876.webp#averageHue=%23fafaf9&clientId=ucd54c1fc-0c0a-4&from=paste&id=uc7b04eb7&originHeight=430&originWidth=1383&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u9df55ac5-1ffd-41da-99ce-d608ca8ddb0&title=)
分类

- 意向共享锁(IS): 由语句select ... lock in share mode添加 。与表锁共享锁(read)兼容，与表锁排他锁(write)互斥。
- 意向排他锁(IX): 由insert、update、delete、select...for update添加 。与表锁共享锁(read)及排他锁(write)都互斥，意向锁之间不会互斥。

一旦事务提交了，意向共享锁、意向排他锁，都会自动释放。

可以通过以下SQL，查看意向锁及行锁的加锁情况：
```sql
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```
演示：
A. 意向共享锁与表读锁是兼容的
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675587308935-0e09ccc4-fb5d-4a68-81b6-9893ac908bbc.webp#averageHue=%23413f38&clientId=ucd54c1fc-0c0a-4&from=paste&id=u310e1663&originHeight=674&originWidth=1663&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u80b51839-382d-4950-9f80-02c4e581c66&title=)
B. 意向排他锁与表读锁、写锁都是互斥的
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675587329103-ca09f862-b607-4cab-b212-775809ea324c.webp#averageHue=%23544c43&clientId=ucd54c1fc-0c0a-4&from=paste&id=ud5027158&originHeight=228&originWidth=1372&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u653f23d6-acc0-4e62-9d79-92d0fe9d03b&title=)

### 行级锁
行级锁，每次操作锁住对应的行数据。锁定粒度最小，发生锁冲突的概率最低，并发度最高。应用在InnoDB存储引擎中。
InnoDB的数据是基于索引组织的，行锁是通过对索引上的索引项加锁来实现的，而不是对记录加的锁。对于行级锁，主要分为以下三类：

- 行锁（Record Lock）：锁定单个行记录的锁，防止其他事务对此行进行update和delete。在RC、RR隔离级别都支持。

![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675588056588-e9b8390b-c1f3-407f-8c8e-236d370b4b27.webp#averageHue=%23c3efa9&clientId=ucd54c1fc-0c0a-4&from=paste&id=u0a3c5e5c&originHeight=156&originWidth=1375&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub1268890-70ec-444f-a9c6-abef873c4f4&title=)

- 间隙锁（Gap Lock）：锁定索引记录间隙（不含该记录），确保索引记录间隙不变，防止其他事务在这个间隙进行insert，产生幻读。在RR隔离级别都支持。

![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675588052127-5ed09a5b-aa99-4883-8df4-b8e403f9e1fa.webp#averageHue=%23cceeb1&clientId=ucd54c1fc-0c0a-4&from=paste&id=u1c08c78c&originHeight=166&originWidth=1329&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub7823883-4b9d-4e87-902c-b060a697bea&title=)

- 临键锁（Next-Key Lock）：行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙Gap。在RR隔离级别下支持。

![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675588054592-07b6ce7d-4e64-4652-85ef-612bbcd28599.webp#averageHue=%23cbefb0&clientId=ucd54c1fc-0c0a-4&from=paste&id=u8d5c01bc&originHeight=175&originWidth=1352&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u7a331702-3375-4282-867c-e4f1c2d10a1&title=)

#### 行锁
InnoDB实现了以下两种类型的行锁：

- 共享锁（S）：允许一个事务去读一行，共享锁之间是兼容的，阻止其他事务获得相同数据集的排他锁。
- 排他锁（X）：允许获取到排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁（如果在增删改，则加上排他锁，并阻止其他事务读写）。

两种行锁的兼容情况如下:
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675590144309-aa1ef07b-c2c1-4be7-9233-135bf24feea3.webp#averageHue=%23d6a3a0&clientId=ucd54c1fc-0c0a-4&from=paste&id=u34d9faad&originHeight=310&originWidth=1395&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u4f763543-aac6-4b47-a56d-5b62a232fd1&title=)
常见的SQL语句，在执行时，所加的行锁如下：

| SQL | 行锁类型 | 说明 |
| --- | --- | --- |
| INSERT ... | 排他锁 | 自动加锁 |
| UPDATE ... | 排他锁 | 自动加锁 |
| DELETE ... | 排他锁 | 自动加锁 |
| SELECT（正常） | 不加任何锁 |  |
| SELECT ... LOCK IN SHARE MODE | 共享锁 | 需要手动在SELECT之后加LOCK IN SHARE MODE |
| SELECT ... FOR UPDATE | 排他锁 | 需要手动在SELECT之后加FOR UPDATE |

演示
默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key 锁进行搜索和索引扫描，以防止幻读。

- 针对唯一索引进行检索时，对已存在的记录进行等值匹配时，将会自动优化为行锁。
- InnoDB的行锁是针对于索引加的锁，不通过索引检索数据，那么InnoDB将对表中的所有记录加锁，此时 就会升级为表锁。

可以通过以下SQL，查看意向锁及行锁的加锁情况：
```sql
-- lock_mode：GAP是间隙锁，REC_NOT_GAP是行锁，单独的S是临键锁
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```
数据准备
```sql
CREATE TABLE `stu` (
	`id` int NOT NULL PRIMARY KEY AUTO_INCREMENT,
	`name` varchar(255) DEFAULT NULL,
	`age` int NOT NULL
) ENGINE = InnoDB CHARACTER SET = utf8mb4;
INSERT INTO `stu` VALUES (1, 'tom', 1);
INSERT INTO `stu` VALUES (3, 'cat', 3);
INSERT INTO `stu` VALUES (8, 'rose', 8);
INSERT INTO `stu` VALUES (11, 'jetty', 11);
INSERT INTO `stu` VALUES (19, 'lily', 19);
INSERT INTO `stu` VALUES (25, 'luci', 25);
```
A. **普通的select语句，执行时，不会加锁**。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675590732585-561321da-7628-416b-9f8a-2a23eff58e7a.webp#averageHue=%233d3731&clientId=ucd54c1fc-0c0a-4&from=paste&id=uc16d4430&originHeight=343&originWidth=1854&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u9d3eaf47-5c1b-42f3-aa4c-12dae82521d&title=)
B. select...lock in share mode，加共享锁，**共享锁与共享锁之间兼容**。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675590731916-76f132c3-5cbe-44a7-825a-9ff445473417.webp#averageHue=%23262524&clientId=ucd54c1fc-0c0a-4&from=paste&id=u9952d5da&originHeight=798&originWidth=1839&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u85f33332-a7de-4a82-b43c-00d449478ad&title=)
共享锁与排他锁之间互斥。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675590734078-0711c8a6-4e16-45a7-95e2-62d4a8373597.webp#averageHue=%233e3932&clientId=ucd54c1fc-0c0a-4&from=paste&id=u2ec14907&originHeight=405&originWidth=1842&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u9088da46-f112-4d78-adca-22b5c14f8ac&title=)
客户端一获取的是id为1这行的共享锁，客户端二是可以获取id为3这行的排它锁的，因为不是同一行数据。 而如果客户端二想获取id为1这行的排他锁，会处于阻塞状态，以为共享锁与排他锁之间互斥。
C. **排它锁与排他锁之间互斥**
左侧加上行级排他锁和意向排他锁
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675590731220-bbacf3b7-3419-4aee-b435-9badbac07197.webp#averageHue=%234b443a&clientId=ucd54c1fc-0c0a-4&from=paste&id=u64156d26&originHeight=242&originWidth=1842&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u768d00ec-75d5-4534-bc0d-a8935fb402e&title=)
当客户端一，执行update语句，会为id为1的记录加排他锁； 客户端二，如果也执行update语句更新id为1的数据，也要为id为1的数据加排他锁，但是客户端二会处于阻塞状态，因为排他锁之间是互斥的。 直到客户端一，把事务提交了，才会把这一行的行锁释放，此时客户端二，解除阻塞。
D. **无索引行锁升级为表锁**
stu表中数据如下:
```sql
mysql> select * from stu;
+----+-----+-------+
| id | age | name  |
+----+-----+-------+
|  1 |   1 | Java  |
|  3 |   3 | Java  |
|  8 |   8 | rose  |
| 11 |  11 | jetty |
| 19 |  19 | lily  |
| 25 |  25 | luci  |
+----+-----+-------+
6 rows in set (0.00 sec)
```

我们在两个客户端中执行如下操作:
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675591449642-4306a117-49d3-4223-b254-06636eab3623.webp#averageHue=%23413e34&clientId=ucd54c1fc-0c0a-4&from=paste&id=u87ac6cae&originHeight=244&originWidth=1469&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u5618ebf2-7973-424e-8b28-ace1cabe6a5&title=)
在客户端一中，开启事务，并执行update语句，更新name为Lily的数据，也就是id为19的记录 。然后在客户端二中更新id为3的记录，却不能直接执行，会处于阻塞状态，为什么呢？
原因就是因为此时，客户端一，根据name字段进行更新时，name字段是没有索引的，如果没有索引，此时行锁会升级为表锁(因为行锁是对索引项加的锁，而name没有索引)。
接下来，我们再针对name字段建立索引，索引建立之后，再次做一个测试：
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675591451203-2837ec64-434e-410e-88e9-c3a5984f9ea1.webp#averageHue=%23454137&clientId=ucd54c1fc-0c0a-4&from=paste&id=u3a2d640b&originHeight=264&originWidth=1581&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u71d0e357-f49e-4d23-ba0c-009079b3341&title=)
此时我们可以看到，客户端一，开启事务，然后依然是根据name进行更新。而客户端二，在更新id为3的数据时，更新成功，并未进入阻塞状态。 这样就说明，我们根据索引字段进行更新操作，就可以避免行锁升级为表锁的情况。
接下来，我们再针对name字段建立索引，索引建立之后，再次做一个测试：
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675591687250-826015b3-c0bd-4109-8504-b1217a65aaf9.webp#averageHue=%23454137&clientId=ucd54c1fc-0c0a-4&from=paste&id=u4953dd9e&originHeight=264&originWidth=1581&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u2fc40e86-87b1-4d9d-8f2c-8f9981f707e&title=)
此时我们可以看到，客户端一，开启事务，然后依然是根据name进行更新。而客户端二，在更新id为3的数据时，更新成功，并未进入阻塞状态。 这样就说明，我们根据索引字段进行更新操作，就可以避免行锁升级为表锁的情况。

#### 间隙锁&临键锁
默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key 锁进行搜索和索引扫描，以防止幻读。

- 索引上的等值查询(唯一索引)，给不存在的记录加锁时, 优化为间隙锁 。
- 索引上的等值查询(非唯一普通索引)，第一个满足值的行加上间隙锁，然后找到向右遍历时最后一个值不满足值的行，加上间隙锁。
- 索引上的范围查询(唯一索引)--会访问到不满足条件的第一个值为止。

**注意:**
间隙锁唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁。
演示
A. **索引上的等值查询(唯一索引)，给不存在的记录加锁时, 优化为**间隙锁 。但不会锁住3和8
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1675592843429-62bcc1c9-5be6-4df5-aaab-f7fc5bd54d04.png#averageHue=%23272625&clientId=ucd54c1fc-0c0a-4&from=paste&height=311&id=u27f56958&originHeight=622&originWidth=1675&originalType=binary&ratio=1&rotation=0&showTitle=false&size=450555&status=done&style=none&taskId=u63bcb24e-98ff-421c-9b92-90afa45e3f1&title=&width=837.5)
B. 索引上的等值查询(非唯一普通索引)，第一个满足值的行加上间隙锁，然后找到向右遍历时最后一个值不满足值的行，加上间隙锁。
介绍分析一下：
我们知道InnoDB的B+树索引，叶子节点是有序的双向链表。 假如，我们要根据这个二级索引查询值为18的数据，并加上共享锁（lock in share mode），我们是只锁定18这一行就可以了吗？ 并不是，因为是非唯一索引，可能会有多个值为18的行存在，而且在未来可能在 18 行前或行后加上值为 18 的行，所以，先锁住18行，并锁住 18 行前的间隙（16到18之间的间隙），然后继续往后找，找到一个不满足条件的值（当前案例中也就是29），并对29之前的间隙加锁。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1675592863942-467ca23a-9e84-4ddb-85d3-53920fd0e0d5.webp#averageHue=%23bdf1b2&clientId=ucd54c1fc-0c0a-4&from=paste&id=u6879cf7e&originHeight=134&originWidth=1261&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ue81fa4a9-634d-44f6-91b9-a88cad01527&title=)

C. 索引上的范围查询(唯一索引)--会访问到不满足条件的第一个值为止。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1675595775679-883a3a00-13e2-48e8-bf06-22f6a170ccf7.png#averageHue=%23275993&clientId=ucd54c1fc-0c0a-4&from=paste&height=237&id=uc1975f36&originHeight=474&originWidth=462&originalType=binary&ratio=1&rotation=0&showTitle=false&size=198876&status=done&style=none&taskId=uba8ad812-97df-407c-a2cd-3886374f3cc&title=&width=231)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1675593467750-c440f492-bf44-48f9-aed9-13760de075b0.png#averageHue=%2348413a&clientId=ucd54c1fc-0c0a-4&from=paste&height=229&id=u856317d9&originHeight=458&originWidth=1833&originalType=binary&ratio=1&rotation=0&showTitle=false&size=481647&status=done&style=none&taskId=ub3a3c5d0-755c-4dc8-925e-3400826a9f1&title=&width=916.5)
查询的条件为id>=19，并添加共享锁。 此时我们可以根据数据库表中现有的数据，将数据分为三个部分：
[19]
(19,25]
(25,+∞]
所以数据库数据在加锁是，就是将19加了行锁，25的临键锁（锁住25及25之前的间隙），正无穷的临键锁(正无穷及之前的间隙)。

## 事务和并发
### Transactions
事务是一组操作的集合，事务会把所有操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。如果一部分执行成功一部分执行失败那成功的那一部分就会复原（revert）以保持数据的一致性。
事务有四大特性，总结为 ACID（刚好是英文单词“酸的”）：

1. Atomicity 原子性，即整体性，不可拆分行（unbreakable），每个事务都是一个工作单元，所有语句必须都执行成功事务才算完成，否则只要有语句执行失败，已执行的操作也会被复原
2. Consistency 一致性，指的是通过事务我们的数据库将永远保持一致性状态，比如不会出现没有完整订单项目的订单
3. Isolation 隔离性，指事务间是相互隔离互不影响的，尤其是需要访问相同数据时。具体而言，如果多个事务要修改相同数据，该数据会被锁定，每次只能被一个事务有权修改，其它事务必须等这个事务执行结束后才能进行
4. Durability 持久性，指的是一旦事务执行完毕，这种修改就是永久的，任何停电或系统死机都不会影响这种数据修改


```sql
-- 查看事务提交方式
SELECT @@AUTOCOMMIT;
-- 设置事务提交方式，1为自动提交，0为手动提交，该设置只对当前会话有效
SET @@AUTOCOMMIT = 0;

-- 开启事务
-- START TRANSACTION 或 BEGIN TRANSACTION;

-- 提交事务
COMMIT;

-- 回滚事务
ROLLBACK;
```

### Creating Transactions
```sql
START TRANSACTION;

INSERT INTO orders (customer_id, order_date, status)
VALUE (1, '2019-01-01', 1);

INSERT INTO order_items
VALUES (LAST_INSERT_ID(), 1, 1, 1);

COMMIT;
```
手动退回，可以将最后的 COMMIT 换成 ROLLBACK，这会退回事务并撤销所有的更改
**autocommit**
我们执行的每一个语句（可以是增删查改 SELECT、INSERT、UPDATE 或 DELETE 语句），就算没有 START TRANSACTION + COMMIT，也都会被 MySQL 包装（wrap）成事务并在没有错误的前提下自动提交，这个过程由一个叫做 autocommit 的系统变量控制，默认开启
因为有 autocommit 的存在，当事务只有一个语句时，用不用 START TRANSACTION + COMMIT 都一样，但要将多个语句作为一个事务时就必须要加 START TRANSACTION + COMMIT 来手动包装了

### Concurrency and Locking
#### 上锁
所以，可以看到，当一个事务修改一行或多行时，会给这些行上锁，这些锁会阻止其他事务修改这些行，直到前一个事务完成（不管是提交还是退回）为止，由于上述MySQL默认状态下的锁定行为，多数时候不需要担心并发问题，但在一些特殊情况下，默认行为不足以满足你应用里的特定场景，这时你可以修改默认行为

#### 并发问题

1. Lost Updates 丢失更新：两个事务更新同一行，最后提交的事务将覆盖先前所做的更改
2. Dirty Reads 脏读：读取了未提交的数据
3. Non-repeating Reads 不可重复读取 （或 Inconsistent Read 不一致读取）：在事务中读取了相同的数据两次，但得到了不同的结果
4. Phantom Reads 幻读：事务A查询后执行结束前，事务B更新了（可能是增删改）数据，然后多了一些满足条件的数据，成为“幻影行”，但此时事务A查询已完成，未包含这新增的满足条件的数据。

**1. Lost Updates 丢失更新**
例如，当事务A要更新john的所在州而事务B要更新john的积分时，若两个事务都读取了john的记录，在A跟新了州且尚未提交时，B更新了积分，那后执行的B的更新会覆盖先执行的A的更新，州的更新将会丢失。
解决方法就是前面说的锁定机制，锁定会防止多个事务同时更新同一个数据，必须一个完成的再执行另一个

**2. Dirty Reads 脏读**
例如，事务A将某顾客的积分从10分增加为20分，但在提交前就被事务B读取了，事务B按照这个尚未提交的顾客积分确定了折扣数额，可之后事务A被退回了，所以该顾客的积分其实仍然是10分，因此事务B等于是读取了一个数据库中从未提交的数据并以此做决定，这被称作为脏读
解决办法是设定事务的隔离等级，例如让一个事务无法看见其它事务尚未提交的更新数据。标准SQL有四个隔离等级，比如，我们可以把事务B设为 READ COMMITED 等级，它将只能读取提交后的数据
积分提交完之后，B事务依此做决定，如果之后积分再修改，这就不是我们考虑的问题了，我们只需要保证B事务读取的是提交后的数据

**3. Non-repeating Reads 不可重复读取 （或 Inconsistent Read 不一致读取）**
上面的隔离能保证只读取提交过的数据，但有时会发生一个事务读取同一个数据两次但两次结果不一致的情况
例如，事务A的语句里需要读取两次某顾客的积分数据，读取第一次时是10分，此时事务B把该积分更新为0分并提交，然后事务A第二次读取积分为0分，这就发生了不可重复读取 或 不一致读取
一种说法是，我们应该总是依照最新的数据做决定，所以这不是个问题。在商务场景中，我们一般不用担心这个问题
另一种说法是，我们应该保持数据一致性，以事务A在开始执行时的数据初始状态为依据来做决定，如果这是我们想要的话，就要增加事务A的隔离等级，让它在执行过程中看不见其它事务的数据更改（即便是提交过的），SQL有个标准隔离等级叫 Repeatable Read 可重复读取，可以保证读取的数据是可重复和一致的，无论过程中其它事务对数据做了何种更改

**4. Phantom Reads 幻读**
幻读是数据库中一种并发控制的问题，它发生在事务A操作X读取了某个范围的数据时，另一个事务B在该范围内插入了新的数据，导致事务A的操作Y再次读取该范围时发现了新插入的数据，导致操作X和Y读取相同范围的数据不一致，产生了“幻觉”的现象。
为了避免幻读的问题，可以使用更严格的隔离级别，例如串行化（SERIALIZABLE）隔离级别，它会在事务中执行所有操作之前锁定相关的数据，防止其他事务操作相同的数据。此外，也可以使用乐观并发控制或悲观并发控制等技术来解决幻读问题。

### 事务隔离级别
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674219542325-086b0c3a-723d-4368-bbc1-85525bb19c46.png#averageHue=%23b0b5ae&clientId=u2801e517-224e-4&from=paste&height=350&id=u8119c656&originHeight=350&originWidth=720&originalType=binary&ratio=1&rotation=0&showTitle=false&size=96162&status=done&style=none&taskId=u0b7a72c1-2581-40de-97a7-02e098ddbe5&title=&width=720)

隔离级别越高（从上到下），减少更多的并发问题，但对性能和可扩展性的影响也大。

1. Read Uncommitted 读取未提交：无法解决任何一个问题，因为事务间并没有任何隔离，他们甚至可以读取彼此未提交的更改

2. Read Committed 读取已提交：给予事务一定的隔离，这样我们只能读取已提交的数据，这防止了Dirty Reads 脏读，但在这个级别下，事务仍可能读取同个内容两次而得到不同的结果，因为另一个事务可能在两次读取之间更新并提交了数据，也就是它不能防止 Non-repeating Reads 不可重复读取 （或 Inconsistent Read 不一致读取）

3. Repeatable Read 可重复读取：在一个事务范围内相同的查询会返回相同的数据，即便数据在这期间被更改和提交。Repeatable Read 也是存在幻读，在一次事务范围内多次进行相同的查询，如果其他并发事务中途插入了新的记录，那么之后的查询会读取到这些“幻影”行。

默认情况下，InnoDB 在 REPEATABLE READ 事务隔离级别运行。

4. Serializable 序列化：可以防止以上所有问题，这一级别还能防止幻读，如果数据在我们执行过程中改变了，我们的事务会等待以获取最新的数据，但这很明显会给服务器增加负担，因为管理等待的事务需要消耗额外的储存和CPU资源

**并发问题 VS 性能和可扩展性**
更低的隔离级别更容易并发，会有更多用户能在相同时间接触到相同数据，但也因此会有更多的并发问题，另一方面因为用以隔离的锁定更少，性能会更高
相反，更高的隔离等级限制了并发并减少了并发问题，但代价是性能和可扩展性的降低，因为我们需要更多的锁定和资源
MySQL的默认等级是 Repeatable Read 可重复读取，它可以防止除幻读外的所有并发问题并且比序列化更快，多数情况下应该保持这个默认等级。
如果对于某个特定的事务，防止幻读至关重要，可以改为 Serializable 序列化
对于某些对数据一致性要求不高的批量报告或者对于数据很少更新的情况，同时又想获得更好性能时，可考虑前两种等级

#### 设置事务隔离级别
默认设定的是下一次事务的隔离等级，加上 SESSION 就是设置本次会话（链接）之后所有事务的隔离等级，加上 GLOBAL 就是设置之后所有对话的所有事务的隔离等级
```sql
-- 查看事务隔离级别：
SELECT @@TRANSACTION_ISOLATION;

SET [SESSION]/[GLOBAL] TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION;
...
COMMIT;
```
如果你是个应用开发人员，你的应用内有一个功能或函数可以链接数据库来执行某一事务（可能是利用 ORM 或是直接连接MySQL），你就可以连接数据库，用 SESSION 关键词设置本次链接的事务的隔离等级，然后执行事务，最后断开连接，这样数据库的其它事务就不会受影响

### Deadlocks
死锁，指两个或两个以上的事务在执行过程中，因争夺资源而造成的一种互相等待的现象，若无外力作用，他们都将无法推进下去，导致产生死锁。
用户1
```sql
USE sql_store;
START TRANSACTION;
UPDATE customers SET state = 'VA' WHERE customer_id = 1;
UPDATE orders SET status = 1 WHERE order_id = 1;
COMMIT;
```
用户2
```sql
USE sql_store;
START TRANSACTION;
UPDATE orders SET status = 1 WHERE order_id = 1;
UPDATE customers SET state = 'VA' WHERE customer_id = 1;
COMMIT;
```
模拟场景：
用户1和2均执行完各自的第一个 UPDATE
→ 用户2执行第二个 UPDATE，等待第一个 TRANSACTION 释放资源， 出现旋转指针
→ 用户1执行第二个 UPDATE，等待第二个 TRANSACTION 释放资源,出现死锁，报错：Error Code: 1213. Deadlock found ……

**缓解方法**
死锁如果只是偶尔发生一般不是什么问题，代码检测到死锁重新尝试或提醒用户重新尝试即可，死锁不可能完全避免，但有一些方法可以最小化其发生的概率：

1. 注意语句顺序：如果检测到两个事务总是发生死锁，检查它们的代码，这些事务可能是储存过程的一部分，看一下事务里的语句顺序，如果这些事务以相反的顺序更新记录，就很可能出现死锁，为了减少死锁，我们在**更新多条记录时可以遵循相同的顺序**

2. 尽量让你的事务小一些，持续时间短一些，这样就不太容易和其他事务相冲突

3. 如果你的事务要操作非常大的表，运行时间可能会很长，冲突的风险就会很高，看看能不能让这样的事物避开高峰期运行，以避开大量活跃用户


## 设计数据库
### 数据建模
**1. Understand the requirements 理解需求**
第1步是理解和分析商业/业务需求，遗憾是很多程序员跳过了这一步就急着去设计数据库里的表和列了，实际上，这一步是最关键的一步，你对问题理解的越透彻，你才越容易找到最合适的解决方案，设计数据库也一样。所以，在动手创建表和列之前，要先完整了解你的业务需求，包括和产品经理、行业专家、从业人员甚至终端用户深入交流以及收集查阅与该问题领域相关的表、文件、应用程序、数据库，以及其他相关的任何信息或资料

**2. Build a conceptional model 概念建模**
当收集并理解了所有相关信息后，下一步就是为业务创建一个概念性的模型。这一步包括找出/识别/确认（identify）业务中的 实体/事物/概念（entities/things/concepts）以及它们之间的关系。

**3. Build a logical model 逻辑建模**
创建好概念模型后，转而创建数据模型（data model）或数据结构（data structure for storing data），即逻辑建模。这一步创建的是**不依赖于**具体数据库技术的抽象的数据模型，主要是确认所需要的**表和列**以及大体的数据类型

**4. Build a physical model 实体建模**
实体建模指的是将逻辑模型在具体某种DBMS上加以实现的过程，相比于逻辑模型，实体模型会确定更多细节，包括各表主键的设定，各列在某一DBMS下特定的具体的数据类型，是否有默认值，是否可为空，还包括储存过程和触发器等对象的创建。总之，实体模型是在某一特 DBMS(Database management system) 下对数据模型非常具体的实现

### Conceptual Models
我们需要一种将实体及其关系可视化的方法，一种是实体关系图（Entity Relationship, ER），一种是统一建模语言（Unified Modeling Language，UML），这里我们用实体关系图（ER），使用的工具是 [https://draw.io](https://draw.io)

### Logical Models
逻辑模型是在概念模型的基础上，在不依赖特定数据库系统的前提下确定数据结构，包括**细化实体间的关系，调整字段设置，确定大体的数据类型。**总之，逻辑模型会基本确立数据库中的**表、列以及表间关系。**

### Physical Models
file -> new model
实体模型就是逻辑模型在具体DBMS的实现，主要是一些技术上的细化，包括确定字段具体数据类型和性质（能否为空等），设置主键等

### Primary Keys
主键就是能唯一标识表中每条记录的字段。
在大多数关系型数据库中，创建主键会自动创建对应的聚集索引。
### Foreign Keys
外键是在一张表中引用了另一张表主键的列
子表的主键设置有两个选择：

1. 将多个外键作为联合主键
2. 另外设置一个单独的主键 子表_id

两种选择各有优缺点，以联合主键为例：

- 好处是可以避免重复的记录，防止一些不合理的数据输入
- 坏处是如果 子表A 未来有新的子表B，就需要复制 子表A 的外键作为外键，这也不一定是很大的麻烦，要根据数据量以及子表是否还有子表等情况来考虑，在一定情况下可能会造成不必要冗余和麻烦

**在大多数关系型数据库中，创建外键时通常会自动创建索引来提高查询性能**
这是因为外键通常用于连接两个表，查询经常需要在这两个表之间进行联接，而索引可以加快联接操作的速度。

在某些数据库中，还可以使用配置选项或参数来控制是否自动创建索引或使用特定的索引类型。

需要注意的是，自动创建的索引可能不是最优的索引，因此在某些情况下，可能需要手动创建更适合查询需求的索引来提高性能。此外，在创建外键时自动创建的索引可能会占用一定的存储空间，因此需要根据实际情况进行权衡和调整。

**外键不一定必须是唯一的，但它们通常引用的列需要是唯一的或具有唯一约束**
这是因为外键通常用于确保关系表之间的数据完整性和一致性，而这需要在引用表中使用唯一值。

外键是一个关系表中的列或列组合，它们引用另一个关系表中的主键或唯一键。当在外键列中插入或更新数据时，关系数据库系统会检查该值是否在引用表中存在，并拒绝插入或更新操作，如果该值不存在。

因此，如果外键引用的列不是唯一的，那么在引用表中可能会存在重复的值，这可能会导致数据不一致性。为了避免这种情况，通常需要在引用表中定义唯一约束或主键，在外键列中引用这些列，以确保外键引用的列是唯一的。

需要注意的是，在某些情况下，可能需要允许外键引用非唯一列，例如，当某些数据模型需要引用相同值的多个行时。但这种情况需要仔细考虑和处理，以确保数据的完整性和一致性。

### Foreign Keys Contraints
有外键时，需要设置约束以防止数据损坏/污染
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674366302180-3695eec1-95ed-46e9-87f4-9b14767d1adb.png#averageHue=%23eaeaea&clientId=ue4843645-62ef-4&from=paste&height=714&id=uce958fee&originHeight=1428&originWidth=1556&originalType=binary&ratio=1&rotation=0&showTitle=false&size=373919&status=done&style=none&taskId=ueb2789b2-1f45-4472-b1d4-4a7d9b9e141&title=&width=778)
在表设计模式里，打开 Foreign Keys 标签页，可以看到两个外键，以 fk_子表_父表 的方式命名，名称后可能有数字，是MySQL为了防止外键与其他外键重名自动添加的，这里没必要，可去掉。右边 Foreign Key Options 可分别选择当父表里的主键被修改或删除（Update / Delete）时，子表里的外键如何反应，有四种选项：

1. CASCADE
瀑布/串联/级联，表示随着主键改变而改变，如主键某学生的 student_id 从1变成2，则该学生的所有注册课程记录的 student_id 也会全部变为2 

2. RESTRICT / NO ACTION
> [https://stackoverflow.com/questions/39338474/whats-the-different-between-restrict-and-no-action](https://stackoverflow.com/questions/39338474/whats-the-different-between-restrict-and-no-action)

RESTRICT 和 NO ACTION 都是在定义外键时可以使用的引用完整性约束选项。它们的作用是限制自身进行删除或更新操作时，被引用的表中的数据是否可以被删除或更新。
当使用 RESTRICT 约束时，如果被引用的表中的数据已经被其他表引用，那么在尝试删除或更新该数据时，将会返回一个错误，阻止该操作的执行。
而 NO ACTION 是来源标准的 sql，在某些数据库中，会延迟检查，如果被引用的表中的数据已经被其他表引用，那么在尝试删除或更新该数据时，将会返回一个错误，阻止该操作的执行，但是在MySQL中，外键约束都会立即检查，所以两者等价。

- RESTRICT constraint rules are checked **before** any other operation,
- NO ACTION constraint rules are checked **after** the statement and all other operations (such as triggers) are completed.

In most cases, there is no difference between the two options. The difference is visible when the delete operation is triggered by some other operation, such as delete cascade from a different table, delete via a view with a UNION, a trigger, etc.

The **NoAction** action is similar to **Restrict**, the difference between the two is dependent on the database being used:

- **PostgreSQL**: **NoAction** allows the check (if a referenced row on the table exists) to be deferred until later in the transaction. See [the PostgreSQL docs](https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-FK) for more information.
- **MySQL**: **NoAction** behaves exactly the same as **Restrict**. See [the MySQL docs](https://dev.mysql.com/doc/refman/8.0/en/create-table-foreign-keys.html#foreign-key-referential-actions) for more information.
- **SQLite**: When a related primary key is modified or deleted, no action is taken. See [the SQLite docs](https://www.sqlite.org/foreignkeys.html#fk_actions) for more information.
- **SQL Server**: When a referenced record is deleted or modified, an error is raised. See [the SQL Server docs](https://docs.microsoft.com/en-us/sql/relational-databases/tables/graph-edge-constraints?view=sql-server-ver15#on-delete-referential-actions-on-edge-constraints) for more information.
- **MongoDB** (in preview from version 3.6.0): When a record is modified or deleted, nothing is done to any related records.



3. SET NULL
相应的外键约束需要设置为非必填。

当主键更改或删除时，使得相应的外键变为空，这样的子表记录就没有对应的主键和对应的父表记录了（no parent），被称为孤儿记录（orphan record），这是垃圾数据，让我们不知道是谁注册的课程或不知道注册的是什么课程，一般不用，只在极其特殊的情况可能有用。

| **行为** | **说明** |
| --- | --- |
| NO ACTION | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与RESTRICT一致） |
| RESTRICT | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与NO ACTION一致） |
| CASCADE | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录 |
| SET NULL | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（要求该外键允许为null） |
| SET DEFAULT | 父表有变更时，子表将外键设为一个默认值（Innodb不支持） |



### Normalization
为了防止重复冗余，需要遵循数据库设计的7大规则或者说7大范式，每一条都是建立在你已经遵循了前一条的基础上。实际上，99%的数据库之需要遵循**前三大范式**就够了，其他几个并没有那么重要。
**作用：减少数据重复和冗余，增强数据的一致性和完整性（data integrity）**
感觉三大范式可以用三个关键词总结：**单一值、单一功能、独立**
#### 1NF 
第一范式（First Normal Form）要求一行中的每个单元格都应该有单一值，且不能出现重复列。

#### 链接表
MySQL里只有一对一和一对多，没有多对多关系。
尝试建立 courses 和 tags 之间的联系，发现两者是多对多关系，这说明两者的关系需要进一步细化，我们添加一个 course_tags 表来专门描述两者间的关系，记录每一对课程和标签的组合，这个中间表或者说**链接表（link table）**同时是 courses 和 tags 的子表，与这两个父表均为一对多的关系，建立两条一对多连线后 MySql 自动给 course_tags 表增加了两个外键 course_id 和 tag_id（注意去掉自动添加的表前缀），两者构成了 course_tags 表 的联合主键。
**创建联合主键**
```sql
CREATE TABLE my_table (
  column1 INT,
  column2 VARCHAR(50),
  column3 DATE,
  PRIMARY KEY (column1, column2)
);
```


**How to Create PostgreSQL Composite Primary Keys in many to many**
在上面的示例中，我们创建了一个名为 "student_courses" 的中间表，它记录了每个学生所选的所有课程。
```sql
CREATE TABLE student_courses (
  student_id INT,
  course_id INT,
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES students(id),
  FOREIGN KEY (course_id) REFERENCES courses(id)
);
```
在多对多的中间表，如果只有 student_id 和 course_id 可以不用定义主键，但需要手动创建一个唯一复合索引。

#### 2NF
每个表都应该是单一功能的/应该表示一个实体类型，这个表的所有字段都是用来描述这个实体的

#### 3NF
一个表中的字段不应该是由表中其他字段推导而来
例如，如果表里已经有 first_name 和 last_name 就不该有 full_name，因为第三者总是可以由前两者合并得到

### 不要对全宇宙都建模
设计数据库时总是考虑当前的业务需求，不要试图包罗万象。
总之，尽可能保持简洁，**简洁才是终极哲学（Simplicity is the ultimate sophistication）**，无论你对未来的预测有多好，总会有意料之外的需求出现。
例如，airports 里的 city 和 state 比较特殊，因为考虑到 city 和 state 与 airport 经常一起查询，合并在一张表上能提高查询速度，而且机场数量并不多，重复性的问题并不严重，反而如果另外单独再建cities表和states表会使得数据**过于碎片化**，所以这里进行**“去标准化”（denormalize）**，在 airports 表中保留 city 和 state 的原始字段，**用一定的重复性来换取查询便利和效率**

### 
Forward Engineering a Model
通过模型正向搭建数据库：workbench 菜单的 Database 选项 → Forward Engineer 正向搭建数据库

### Synchronizing a Model with a Database
之后可能会修改数据库结构，比如更改某些表中字段的数据类型或增加字段之类，如果只是自己一个人用的一个本地数据库，可以直接打开对应表的设计模式并点击更改即可，但如果是在团队中工作通常不是这样。
在中大型团队中，我们通常有**多个服务器来模拟各种环境。**
不能是在设计模式中直接点击修改，相反，是在之前的实体模型（EER Diagram）中修改并使用菜单中的 Database → Synchronize Model，其中有一步可以选择链接，这里我们选择本地连接 local_instance，但如果是在团队中可能需要选择测试环境、模拟环境甚至开发环境的链接以对相应环境中的数据库执行更改。
可以把这些代码保存成 sql 脚本并上传到仓库，就可以在不同环境执行相同修改以保持一致性。

### Reverse Engineering a Database
如果要修改没有实体模型的数据库，第一次可以先逆向工程（Reverse Engineering）建立模型
在反向搭建出的模型中，可以更好的看清和理解数据库的结构设计，可以修改表结构，还可以发现问题

### 数据库操作
查询所有数据库：
SHOW DATABASES;
查询当前数据库：
SELECT DATABASE();
创建数据库：
CREATE DATABASE [ IF NOT EXISTS ] 数据库名 [ DEFAULT CHARSET 字符集] [COLLATE 排序规则 ];
删除数据库：
DROP DATABASE [ IF EXISTS ] 数据库名;
使用数据库：
USE 数据库名;

**注意事项**

- UTF8字符集长度为3字节，有些符号占4字节，所以推荐用utf8mb4字符集

```sql
CREATE DATABASE IF NOT EXISTS sql_store2;
DROP DATABASE IF EXISTS sql_store2;
```

### 表操作
查询当前数据库所有表：
SHOW TABLES;
查询表结构：
DESC 表名;
查询指定表的建表语句：
SHOW CREATE TABLE 表名;
创建表：
```sql
CREATE TABLE 表名(
  字段1 字段1类型 [COMMENT 字段1注释],
  字段2 字段2类型 [COMMENT 字段2注释],
  字段3 字段3类型 [COMMENT 字段3注释],
  ...
  字段n 字段n类型 [COMMENT 字段n注释]
)[ COMMENT 表注释 ];
```
**最后一个字段后面没有逗号**
添加字段：
ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];
例：ALTER TABLE emp ADD nickname varchar(20) COMMENT '昵称';
修改数据类型：
ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);
修改字段名和字段类型：
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];
例：将emp表的nickname字段修改为username，类型为varchar(30)
ALTER TABLE emp CHANGE nickname username varchar(30) COMMENT '昵称';
删除字段：
ALTER TABLE 表名 DROP 字段名;
修改表名：
ALTER TABLE 表名 RENAME TO 新表名
删除表：
DROP TABLE [IF EXISTS] 表名;
DROP TABLE 删除表，执行 DROP TABLE 操作将完全删除表及其所有相关的约束、索引、触发器和依赖项。
为了避免意外删除表，您也可以在 DROP TABLE 语句中添加 IF EXISTS 子句，以确保只有在表存在时才执行删除操作。

删除表，并重新创建该表：
TRUNCATE TABLE 表名;
```sql
DROP TABLE IF EXISTS customers;
CREATE TABLE IF NOT EXISTS customers
(
		customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    points INT NOT NULL DEFAULT 0,
    eamil VARCHAR(255) NOT NULL UNIQUE
);
```
TRUNCATE TABLE 是一种 SQL 语句，用于清空一个表中的所有数据，但不删除表本身，保留表结构和定义。TRUNCATE TABLE 操作将表中的所有行删除，并释放表占用的存储空间，同时重置存储引擎中的计数器，将自增列重置为初始值。
相比之下，DELETE FROM 语句删除表中的行，但保留表结构和定义，并且可以通过 WHERE 子句带条件地删除行。DELETE FROM 操作也比 TRUNCATE TABLE 操作更加灵活，但可能需要更长的时间来完成，因为它要逐行地删除数据，而 TRUNCATE TABLE 则像一个快速的重置操作。
需要注意的是，TRUNCATE TABLE 操作是不可逆的，并且不能撤销，因此在执行此操作之前，请务必确保您已经备份了需要保留的数据。

### Creating Relationships
```sql
CONSTRAINT 外键名 FOREIGN KEY (外键字段, ...)
        REFERENCES 父表 (主键字段, ...)
        -- 设置外键约束：
        ON UPDATE CASCADE
        ON DELETE NO ACTION
```
```sql
CREATE DATABASE IF NOT EXISTS sql_store2;
USE sql_store2;
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS customers;
CREATE TABLE IF NOT EXISTS customers(
		customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    points INT NOT NULL DEFAULT 0,
    eamil VARCHAR(255) NOT NULL UNIQUE
);

CREATE TABLE orders
(
		order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    CONSTRAINT fk_orders_customers FOREIGN KEY (customer_id)
		REFERENCES customers (customer_id)
        ON UPDATE CASCADE
        ON DELETE NO ACTION
);
```

在 SQL 中，可以为外键添加 UNIQUE 约束来确保它引用的列的值在引用表中是唯一的。要为外键添加 UNIQUE 约束，可以在 CREATE TABLE 语句中使用 UNIQUE 关键字。

以下是一个示例 SQL 语句，用于创建一个带有外键和 UNIQUE 约束的表：
```sql
CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  customer_id INT UNIQUE,
  order_date DATE,
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

在上面的示例中，orders 表包括一个名为 customer_id 的外键，它引用了 customers 表中的 customer_id 列。同时，该列也被定义为 UNIQUE，因此在 customers 表中，每个 customer_id 的值都必须是唯一的。

在使用 FOREIGN KEY 创建外键时，可以在外键列名称后面使用 UNIQUE 关键字来定义 UNIQUE 约束。在上面的示例中，customer_id 列定义为 UNIQUE，因此它引用的任何行都必须具有唯一的 customer_id 值。

需要注意的是，如果外键引用的列已经是主键或具有唯一约束，则不需要再为外键添加 UNIQUE 约束。

### 更改主键和外键约束
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名(主表字段名) ON UPDATE 行为 ON DELETE 行为;
```sql
ALTER TABLE orders 
  	DROP PRIMARY KEY, -- 删除主键时，不用写列名称
		ADD PRIMARY KEY (order_id),
    DROP FOREIGN KEY fk_orders_customers,
    ADD CONSTRAINT fk_orders_customers FOREIGN KEY (customer_id)
        REFERENCES customers (customer_id)
        ON UPDATE CASCADE
        ON DELETE NO ACTION;
```

### 字符集和排序规则
查看MySQL支持的所有字符集，还可以看到字符集描述，默认排序规则，最大长度
```sql
SHOW CHARSET;
```
排序规则（collation n. 校对，整理，排序规则）指的是某语言内字符的排序方式，utf-8 的默认排序规则是 utf8_general_ci，其中 ci 表示 case insensitive 大小写不敏感，即MySQL在排序时不会区分大小写，这在大部分时候都是适用的，比如用户输入名字的时候大小写不固定，我们希望只按照字符顺序而不管大小写来对名字进行排序。总之，99.9% 的情况下都不需要更改默认排序规则。
最大长度指的是对该字符集来说，给每个字符预留的最大字节数，如 latin1 是 1 字节，utf-8 就是 3 Byte，前面说过，在utf-8里，拉丁字符使用 1 字节，欧洲和中东字符使用 2 字节，亚洲语言的字符使用 3 字节，所以 utf-8 给每个字符预留 3 字节。
对于字符集来说，大部分时候用默认的 utf-8 就行了。但有时，我们可以通过更改字符集来减少空间占用，例如，我们某个特定的应用（对应的数据库）/特定表/特定列是只能输入英文字符的，那如果将该列的字符集从 utf-8 改为 latin1，占用空间就会缩小到原来的 1/3，以字段类型为 CHAR(10)（固定预留10个字符）且有 1 百万条记录为例，占用空间就会从约 30MB 减到 10MB。接下来讲如何用菜单和代码方式更改库/表/列的字符集。

**菜单方式更改字符集**
右键 sql_store2 数据库，点击 Schema Inspector，可以查看整个数据库以及各表各列的字符集和排序规则，Schema Inspector 也能查看该数据库的主键外键、视图、触发器、储存程序、事务、函数等各方面情况
要修改库或者表和列的字符集，直接点开库或者表的设计模式（扳手按钮）在里面选择更改即可，一般我们会让表和列的字符集和整个库保持一致，毕竟一个应用要不然是国际化的要不然就不是。
**代码方式更改字符集**
总的来说就是将设置字符集的语句 CHARACTER SET 字符集名 加在之前那些创建/更改数据库/表/列语句的合适位置即可

1. 在创建或修改数据库时设置或修改数据库的字符集
```sql
CREATE/ALTER DATABASE db_name 
    CHARACTER SET latin1
```
2. 在创建或修改表时设置或修改表的字符集
```sql
CREATE/ALTER TABLE table1
(……) 
CHARACTER SET latin1
```
3. 在创建或修改表时设置或修改列的字符集
就是将 CHARACTER SET latin1 加在列设置语句的**字段类型和字段性质之间**
```sql
CREATE TABLE IF NOT EXISTS customers
(
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) CHARACTER SET latin1 NOT NULL, 
    points INT NOT NULL DEFAULT 0,
    email VARCHAR(255) NOT NULL UNIQUE
)

-- or

USE sql_store2;
ALTER TABLE customers
    MODIFY first_name VARCHAR(50) CHARACTER SET latin1 NOT NULL,
    ADD    last_name  VARCHAR(50) CHARACTER SET latin1 NOT NULL AFTER first_name;
```
### 
Storage Engines
在MySQL中我们有若干种储存引擎，储存引擎决定了我们数据的**储存方式以及可用的功能**
展示可用的储存引擎：
```sql
SHOW ENGINES;
```
储存引擎有很多，我们真正需要知道只有两个：MyISAM 和 InnoDB
MyISAM 是曾经很流行的引擎，但自 MySQL5.5 之后，默认引擎就改为 InnoDB了，InnoDB支持更多的功能特性，包括事务、外键等等，所以最好使用 InnoDB
引擎是表层级的设置，每个表都可以设置不同的引擎（虽然这没必要）
外键是十分重要的，它可以增加引用一致性/完整性（referential integrity），如果我们有一个老数据库的引擎是MyISAM，我们想要给它设置外键，就必须要将其引擎升级为InnoDB，可以在表的设计模式里选择更改，也可以用修改表的代码：
```sql
ALTER TABLE customers
ENGINE = InnoDB
```
改变引擎是一个代价极高（expensive）的操作，它会重建整个表，在此期间无法方法访问数据。所以，除非有特殊的理由，不然不要在生产环境中改变储存引擎。

## 索引
**原理和作用**
以寻找所在州（state）为 'CA' 的顾客为例，如果没索引，MySQL 就必须扫描筛选所有记录。索引，就好比书籍最后的那些关键词索引一样，按字母排序，这样就能按字母迅速找到需要的关键词所在的页数，类似的，对 state 字段建立索引时，其实就是把 state 列单独拿出来分类排序并建立与原表顾客记录的对应关系，然后就可以通过该索引迅速找到所在州为 'CA' 的顾客
另一方面，索引会比原表小得多，通常能够储存在内存中，而从内存读取数据总是比从硬盘读取快多了，这也会提升查询速度
如果数据量比较小，几百几千这种，没必要用索引，但如果是上百万的数据量，有无索引对查询效率的影响就很大

**不管有多少索引，MySQL 最多只选最优的那 1 个索引**

优点：

- 提高数据检索效率，降低数据库的IO成本
- 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗

缺点：

- 索引要占用内存
- 索引大大提高了查询效率，但降低了更新的速度，比如 INSERT、UPDATE、DELETE，因为每次修改数据时都会自动重建索引。

**所以不要对整个表建立索引，而只是针对关键的查询建立索引**。使用索引的终极目的就是为了加快运行较慢的查询。

### 索引结构
| **索引结构** | **描述** |
| --- | --- |
| B+Tree | 最常见的索引类型，大部分引擎都支持B+树索引 |
| Hash | 底层数据结构是用哈希表实现，只有精确匹配索引列的查询才有效，不支持范围查询 |
| R-Tree(空间索引) | 空间索引是 MyISAM 引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少 |
| Full-Text(全文索引) | 是一种通过建立倒排索引，快速匹配文档的方式，类似于 Lucene, Solr, ES |

| **索引** | **InnoDB** | **MyISAM** | **Memory** |
| --- | --- | --- | --- |
| B+Tree索引 | 支持 | 支持 | 支持 |
| Hash索引 | 不支持 | 不支持 | 支持 |
| R-Tree索引 | 不支持 | 支持 | 不支持 |
| Full-text | 5.6版本后支持 | 支持 | 不支持 |


#### B-Tree
![](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674807435562-a4b52062-3c21-442c-a73d-91bd413d645e.png#averageHue=%23f8f7f6&clientId=ue4843645-62ef-4&from=paste&id=u080a388d&originHeight=617&originWidth=1216&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u7c3a1b3c-638a-4f01-ba1a-7a4d7ee93a7&title=)
二叉树的缺点可以用红黑树来解决：
![](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674807435433-bcc405d2-cac3-48cf-a5fb-291dbb0a5826.png#averageHue=%23f7f1f0&clientId=ue4843645-62ef-4&from=paste&id=uedf56584&originHeight=402&originWidth=487&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u21515ea9-6b4a-4f08-b9db-d26f679936a&title=)
红黑树也存在大数据量情况下，层级较深，检索速度慢的问题。
为了解决上述问题，可以使用 B-Tree 结构。
B-Tree (多路平衡查找树) 以一棵最大度数（max-degree，指一个节点的子节点最大个数）为5（5阶）的 b-tree 为例（每个节点最多存储4个key，5个指针，5个子节点）
![](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674807435313-42c58ea4-10de-4a25-9228-4fafd865b4be.png#averageHue=%23efefef&clientId=ue4843645-62ef-4&from=paste&id=uc82b5027&originHeight=457&originWidth=1561&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u8a26d0d0-515c-492a-93cf-8b4f717d81b&title=)
_B-Tree 的数据插入过程动画参照：_[https://www.bilibili.com/video/BV1Kr4y1i7ru?p=68](https://www.bilibili.com/video/BV1Kr4y1i7ru?p=68)__演示地址：_[https://www.cs.usfca.edu/~galles/visualization/BTree.html](https://www.cs.usfca.edu/~galles/visualization/BTree.html)
#### B+Tree
结构图：
![](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674807435289-ebc53a3e-c76c-497e-b23c-a76a08ac8758.png#averageHue=%23efeeed&clientId=ue4843645-62ef-4&from=paste&id=u06e875bc&originHeight=444&originWidth=1424&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ud68f031f-af39-4087-ad5d-d41f5b7fd1d&title=)
_演示地址：_[https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)
与 B-Tree 的区别：

- 所有的数据都会出现在叶子节点
- 叶子节点形成一个单向链表

MySQL 索引数据结构对经典的 B+Tree 进行了优化。在原 B+Tree 的基础上，增加一个指向相邻叶子节点的链表指针（双向链表），就形成了带有顺序指针的 B+Tree，提高区间访问的性能。
![](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674807435307-622055e8-5b88-404f-9925-99c258783801.png#averageHue=%23f0eae4&clientId=ue4843645-62ef-4&from=paste&id=uf13cc51d&originHeight=476&originWidth=1513&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub22fc03d-a34f-488e-a76e-9c2c45c7851&title=)
#### Hash
哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中。
如果两个（或多个）键值，映射到一个相同的槽位上，他们就产生了hash冲突（也称为hash碰撞），可以通过链表来解决。
由于Hash索引数据结构的特殊性，其检索效率非常高，索引的检索可以一次定位，不像B+Tree 索引需要从根节点到枝节点，最后才能访问到叶子节点这样多次的IO访问，所以 Hash 索引的查询效率一般要高于 B+Tree 索引。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674816267419-7fe4e48f-2481-4879-8778-100c30d87e68.png#averageHue=%23f4ebdd&clientId=ue4843645-62ef-4&from=paste&height=529&id=u105f178c&originHeight=1058&originWidth=2694&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1021060&status=done&style=none&taskId=u51e022bb-7de9-4749-8db7-dadeadcec14&title=&width=1347)
特点：

- Hash索引只能用于对等比较（=、in），不支持范围查询（betwwn、>、<、…）
- Hash索引存储是无序的，无法利用索引完成排序操作。因为原先是有序的键值，经过哈希算法映射到槽位后，有可能变成不连续的
- 查询效率高，非hash碰撞（键值）情况下只需要一次检索就可以了。如果键值不是唯一的，就需要先找到该键所在位置，然后再根据链表往后扫描，直到找到相应的数据。在有大量重复键值情况下，哈希索引的效率也是极低的，因为会形成一条很长的链表。



存储引擎支持：

- Memory
- InnoDB: 具有自适应hash功能，hash索引是存储引擎根据 B+Tree 索引在指定条件下自动构建的
#### 面试题

1. 为什么 InnoDB 存储引擎选择使用 B+Tree 索引结构？
- 相对于二叉树，层级更少，搜索效率高
- 对于 B-Tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值和指针（最大度数）减少，要同样保存大量数据，只能增加树的层级数，导致性能降低
- 相对于 Hash 索引，B+Tree 支持范围匹配及排序操作
### 索引分类
| **分类** | **含义** | **特点** | **关键字** |
| --- | --- | --- | --- |
| 主键索引 | 针对于表中主键创建的索引 | 默认自动创建，只能有一个 | PRIMARY |
| 唯一索引 | 避免同一个表中某数据列中的值重复 | 可以有多个 | UNIQUE |
| 常规索引 | 快速定位特定数据 | 可以有多个 |  |
| 全文索引 | 全文索引查找的是文本中的关键词，而不是比较索引中的值 | 可以有多个 | FULLTEXT |

在 InnoDB 存储引擎中，根据索引的存储形式，又可以分为以下两种：

| **分类** | **含义** | **特点** |
| --- | --- | --- |
| 聚集索引(Clustered Index) | 将数据存储与索引放一块，索引结构的叶子节点保存了行数据 | 必须有，而且只有一个 |
| 二级索引(Secondary Index) | 将数据与索引分开存储，索引结构的叶子节点关联的是对应的主键 | 可以存在多个 |

演示图：
![](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674807437775-550e5492-df86-436b-a86f-7b201531d11d.png#averageHue=%23f9f6f4&clientId=ue4843645-62ef-4&from=paste&id=uf14d4072&originHeight=765&originWidth=1517&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ucbea5129-8cbb-4055-ab66-78519f789e0&title=)

![](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674807438327-aa598a4e-c402-44f8-b627-d72d9f877261.png#averageHue=%23faf9f6&clientId=ue4843645-62ef-4&from=paste&id=uf0821694&originHeight=778&originWidth=1510&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u876750f7-f543-4f03-88a4-4024de58e0a&title=)
聚集索引选取规则：

- 如果存在主键，主键索引就是聚集索引
- 如果不存在主键，将使用第一个唯一(UNIQUE)索引作为聚集索引
- 如果表没有主键或没有合适的唯一索引，则 InnoDB 会自动生成一个 rowid 作为隐藏的聚集索引
#### 思考题
1. 以下 SQL 语句，哪个执行效率高？为什么？
```sql
select * from user where id = 10;
select * from user where name = 'Arm';
-- 备注：id为主键，name字段创建的有索引
```
答：第一条语句，因为第二条需要回表查询，相当于两个步骤。
2. InnoDB 主键索引的 B+Tree 高度为多少？
答：一页中可以存储 16k 数据，假设叶子节点一行数据 1k，则可以存储 16 行数据。InnoDB 的指针占用6个字节的空间，主键假设为bigint，占用字节数为8.
可得公式：n * 8 + (n + 1) * 6 = 16 * 1024，
其中 8 表示 bigint 占用的字节数，n 表示单个节点（当页）存储的key的数量，(n + 1) 表示指针数量（比key多一个）
算出 n 约为1170，指针数为 1171，一个指针最终指向一个子节点
如果树的高度为2，那么他能存储的数据量大概为：1171 * 16 = 18736；
如果树的高度为3，那么他能存储的数据量大概为：1171 * 1171 * 16 = 21939856。

### 创建索引
CREATE [ UNIQUE | FULLTEXT ] INDEX index_name ON table_name (col [DESC], ...);
```sql
-- CREATE INDEX 索引名（通常是 idx_列名） ON 表名 (列名 , ...);
CREATE INDEX idx_state ON customers (state);
```

### 查看索引、聚集/二级索引
**聚集家索引**
在大多数关系型数据库中，创建主键会自动创建对应的聚集索引。
聚集索引只能在一张表上创建一个。如果在某个表上创建了聚集索引，那么该表的数据行将按照聚集索引列的值进行排序。如果在查询时使用了聚集索引列，那么查询的性能通常会比非聚集索引要高。这是因为查询可以利用聚集索引的物理顺序，通过顺序扫描或二分查找等方式来定位数据行。
> 需要注意的是，在某些数据库中，如果已经为表创建了聚集索引，那么在创建主键时，系统可能会要求使用现有的聚集索引。此时，如果现有的聚集索引不是主键所在的列，那么需要删除现有的聚集索引并重新创建一个新的聚集索引来支持主键。

与聚集索引相反，非聚集索引（Non-Clustered Index）不会改变表中数据的物理存储顺序，而是将索引列的值存储在一个单独的数据结构中，并指向相应的数据行。因此，在查询时需要通过非聚集索引来查找数据行，然后再根据数据行的物理位置来访问数据。
需要注意的是，聚集索引的创建会对表的性能产生一定的影响。由于聚集索引会对表中的数据进行重新组织，并将它们存储在物理上相邻的位置上，因此在插入、删除或更新数据时，需要对表进行重组，这可能会导致性能下降。

查看 customers 表的索引：
```sql
SHOW INDEXES IN customers; -- SHOW INDEXES IN 表名
```
可以看到有三个索引，第一个是 MySQL 为主键 customer_id 创建的索引 PRIMARY，被称作**clustered index 聚集索引**，每张表最多一个聚集索引，每当我们为表创建主键时，MySQL 就会自动为其创建索引，这样就能快速通过主键（通常是某id）找到记录。后两个是我们之前手动为 state 和 points 字段建立的索引 idx_state 和 idx_points，它们是 **secondary index 二级索引**，MySQL 在创建从属索引时会自动为其添加主键列，如每个 idx_points 索引的记录有两个值：客户的积分points 和对应的客户编号 customer_id，这样就可以通过客户积分快速找到对应的客户记录。

**注意**
Cardinality 表示通过索引查询出来的唯一值的数量
```sql
SELECT COUNT(DISTINCT state) FROM customers
```
Cardinality 这里只是近似值而非精确值，要先用以下语句重建顾客表的统计数据：
```sql
ANALYZE TABLE customers;
```
然后再用 SHOW INDEXES IN customers; 得到的才是精确的 Cardinality

当我们建立表间关系时，MySQL会自动为外键添加索引，这样就能快速就行表连接（join tables）了。
还可以通过菜单方式查看某表中的索引，在左侧导航栏里 customers 表的子文件里就有一个 indexes 文件夹，点击里面的索引可以看到该索引的若干属性，其中 visible（可见性） 表示其是否可用

### 删除索引
```sql
DROP INDEX idx_state ON customers;
```


### 前缀索引
当索引的列是字符串时（包括 CHAR、VARCHAR、TEXT、BLOG），尤其是当字符串较长时，我们通常不会使用整个字符串而是只是用字符串的前面几个字符来建立索引，这被称作 **Prefix Indexes 前缀索引**，这样可以减少索引的大小使其更容易在内存中操作，毕竟在内存中操作数据比在硬盘中快很多
为 customers 表的 last_name 建立索引并且只使用其前20个字符：
```sql
CREATE INDEX idx_lastname ON customers (last_name(20));
```
这个字符数的设定对于 CHAR 和 VARCHAR 是可选的，但对于 TEXT 和 BLOG 是必须的

可最佳字符数如何确定呢？太多了会使得索引太大难以在内存中运行，太少又达不到筛选的效果，比如，只用第一个字符建立索引，那如果查找A开头的名字，索引可能会返回10万个结果，然后就必须对这10万个结果逐条筛选。
可以利用 COUNT、DISTINCT、LEFT 关键词和函数来测试不同数目的前缀字符得到的唯一值个数，目标是用尽可能少的前缀字符得到尽可能多的独特值个数：
> 注意还要考虑日后新增数据的字符情况

```sql
SELECT 
    COUNT(DISTINCT LEFT(last_name, 1)),
    COUNT(DISTINCT LEFT(last_name, 5)),
    COUNT(DISTINCT LEFT(last_name, 10))
FROM customers
```


### 全文索引
**Full-text Indexes**
利用全文索引，结合 MATCH 和 AGAINST 进行 google 式的模糊搜索
```sql
CREATE FULLTEXT INDEX idx_title_body ON posts (title, body);

SELECT *, MATCH(title, body) AGAINST('react redux')
FROM posts
WHERE MATCH(title, body) AGAINST('react redux');
```
注意MATCH后的括号里必须包含全文索引 idx_title_body 建立时相关的所有列，不然会报错
还可以把 MATCH(title, body) AGAINST('react redux') 包含在选择语句里作为各结果的 relevance score 相关性得分（一个 0 到 1 的浮点数）
全文检索有两个模式：自然语言模式和布尔模式，自然语言模式是默认模式。
布尔模式可以更明确地选择包含或排除一些词汇（google也有类似功能），如：
1. title 或 body 尽量有 react，不要有 redux，必须有 form
```sql
WHERE MATCH(title, body) AGAINST('react -redux +form' IN BOOLEAN MODE);
```
2. 布尔模式也可以实现精确搜索，就是将需要精确搜索的内容再用双引号包起来
```sql
WHERE MATCH(title, body) AGAINST('"handling a form"' IN BOOLEAN MODE);
```

**布尔模式语法规则：**
[https://dev.mysql.com/doc/refman/8.0/en/fulltext-boolean.html](https://dev.mysql.com/doc/refman/8.0/en/fulltext-boolean.html)

1. +：匹配包含该关键字的行，必须存在该关键字。
2. -：排除包含该关键字的行。
3. *：通配符，匹配任意字符。
4. ~：模糊搜索，匹配与关键字相似的词语。
5. " "：短语搜索，匹配完整的短语。
6. > 和 <：指定搜索的最小和最大词语长度。
7. ( )：Parentheses group words into subexpressions. Parenthesized groups can be nested.

```sql
匹配包含 book 或 books 的行：
book*

A leading tilde acts as a negation operator, causing the word's contribution to the row's relevance to be negative. This is useful for marking “noise” words. A row containing such a word is rated lower than others, but is not excluded altogether, as it would be with the - operator.
~需要放在 pattern 前面：
~color

匹配包含完整短语 New York 的行：
"New York"

提高该条匹配数据的权重值：
>apple

降低该条匹配数据的权重值：
<apple
```

全文索引十分强大，如果你要建一个搜索引擎可以使用它，特别是要搜索的是长文本时，如文章、博客、说明和描述，否则，如果搜索比较短的字符串，比如名字或地址，就使用前置字符串

### 复合索引
**Composite Indexes**
MySQL 中一个索引最多包含 16 列，索引越多，写入操作越慢
```sql
CREATE INDEX idx_state_points ON customers (state, points);

EXPLAIN SELECT customer_id FROM customers
WHERE state = 'CA' AND points > 1000;
```

### 使用索引
例如，使用索引：
explain select * from tb_user use index(idx_user_pro) where profession="软件工程";
不使用哪个索引：
explain select * from tb_user ignore index(idx_user_pro) where profession="软件工程";
必须使用哪个索引：
explain select * from tb_user force index(idx_user_pro) where profession="软件工程";
use 是建议，实际使用哪个索引 MySQL 还会自己权衡运行速度去更改，force就是无论如何都强制使用该索引。

### 唯一索引
单列唯一索引和联合唯一索引
创建UNIQUE约束时，MySQL会在幕后创建 UNIQUE 索引。
```sql
create table t1 (
  id int ,
	col int,
　unique uq1 (col)  -- unique是唯一约束，会自动创建唯一索引，uq1是这个唯一索引的名字，(col)是将字段col设为唯一索引。
)engine=innodb,default charset=utf8;

create table t1 (
  id int .....,
	col_1 int,
	col_2 int,
	unique uq1 (col_1,col_2)  -- 联合唯一索引
)engine=innodb,default charset=utf8;
```

- 唯一索引：唯一索引限制了索引列中的值必须唯一，就像一个唯一性约束一样。这意味着，如果一个表中有一个唯一索引，那么它将确保索引列中的值在表中是唯一的。如果您试图插入一个已经存在的值，将会导致插入操作失败。唯一索引通常用于确保表中的某些列具有唯一性，例如电子邮件地址、用户名等。
- 非唯一索引：非唯一索引没有唯一性限制，这意味着索引列中的值可以重复出现。非唯一索引通常用于加速对表中某些列的查询操作，例如在一个客户表中，可能会有一个非唯一索引来加速按客户姓名进行查询。


### 当索引无效时
有时你有一个可用的索引，但你的查询却未能充分利用它，这里我们看几种常见的情形：
**案例1**
注意这里是**或（OR）**
```sql
USE sql_store;
EXPLAIN SELECT customer_id FROM customers
WHERE state = 'CA' OR points > 1000;
```
发现虽然显示 type 是 index，用的索引是 idx_state_points，进行了 全索引扫描（full index scan），如果没有创建任何索引，则会进行全表扫描
> 全表扫描：扫描整个表的数据
> 全索引扫描：扫描目标索引所有叶子块

**优化**
另建一个 idx_points 并将这个 OR 查询改写为两部分，分别用各自最合适的索引，再用 UNION 融合结果（注意 UNION 是自动去重的，如果要保留重复记录就要用 UNION ALL）
```sql
CREATE INDEX idx_points ON customers (points);

EXPLAIN

        SELECT customer_id FROM customers
        WHERE state = 'CA'

    UNION

        SELECT customer_id FROM customers
        WHERE points > 1000;
```


**案例2**
```sql
EXPLAIN SELECT customer_id FROM customers
WHERE points + 10 > 2010;
-- key: idx_points
-- rows: 1010
```
因为 column expression 列表达式（列运算） 不能最有效地使用索引，要重写运算表达式，独立/分离此列
**优化**
```sql
EXPLAIN SELECT customer_id FROM customers
WHERE points > 2000;
-- key: idx_points
-- rows: 4
```

**案例3**
所有索引均不符合覆盖索引，需要回表查询

### 使用索引排序
**建立什么索引取决于查询和排序需求，而查询和排序也要尽量去迎合索引以尽可能提高效率**
上次查询的消耗值
```sql
SHOW STATUS LIKE 'last_query_cost';
```
非索引列排序常常用的是 filesort 算法，而索引是已经对字段进行分类和排序了
但如之前所说，特定的索引只对特定的查询（WHERE 筛选条件）和排序（ORDER BY 排序条件）有效，这还是要从原理上理解：
> 以 idx_state_points 为例，它等于是先对 state 分类排序，再在同一个 state 内对 points 进行分类排序，再加上 customer_id 映射到相应的原表记录

所以，索引 idx_state_points 对于以下排序有效：
```sql
CREATE INDEX idx_state_points ON table_name (state, points);

ORDER BY state
ORDER BY state, points


ORDER BY state 
ORDER BY state DESC
ORDER BY state, points
ORDER BY state DESC, points DESC -- 必须同向
ORDER BY points WHERE state = 'CA'
```
相反，idx_state_points 对以下索引无效或只是部分有效，这些都是会部分或全部用到 filesort 算法的：
```sql
ORDER BY points
ORDER BY points, state
ORDER BY state, first_name, points
ORDER BY state, points DESC
ORDER BY state DESC, points
```

### 覆盖索引和回表查询
```sql
USE sql_store;

-- 1. 只选择 customer_id:
EXPLAIN SELECT customer_id FROM customers
ORDER BY state;
SHOW STATUS LIKE 'last_query_cost';

-- 2. 选择 customer_id 和 state:
EXPLAIN SELECT customer_id, state FROM customers
ORDER BY state;
SHOW STATUS LIKE 'last_query_cost';

-- 3. 选择所有字段:
EXPLAIN SELECT * FROM customers
ORDER BY state;
SHOW STATUS LIKE 'last_query_cost';
```
验证发现前两次是完全 Using index 而且 cost 均只有两百左右，而第3种是 Using filesort 而且 cost 超过一千，这从 idx_state_points 的原理上也很好理解：
前面提到过，从属索引除了包含相关列还会自动包含主键列（通常是某种id列）来和原表中的记录建立对应关系，所以 组合索引 idx_state_points 中包含三列：state、points 以及 customer_id，所以如果 SELECT 子句里选择的列是这三列中的一列或几列的话，整个查询就可以在只使用索引不碰原表的情况下完成，这叫作**覆盖索引（covering index）**，即索引满足了查询的所有需求所以全程不需要使用原表，这是最快的。

explain 中 extra 字段含义：
using index condition：查找使用了索引，但是需要回表查询数据
using where; using index;：查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要**回表查询**

假设 id 是聚集索引
如果在聚集索引中直接能找到对应的行，则直接返回行数据，只需要一次查询；如果在辅助索引中找聚集索引，如select id, name from xxx where name='xxx';，也只需要通过辅助索引（name）查找到对应的id，返回name和name索引对应的id即可，只需要一次查询；如果是通过辅助索引（name）查找其他字段，则需要回表查询，如select id, name, gender from xxx where name='xxx';
所以尽量不要用select *，容易出现回表查询，降低效率，除非有联合索引包含了所有字段

设计索引时，先看 **WHERE** 子句，看看最常用的筛选字段是什么，把它们包含在索引中，这样就能迅速缩小查找范围，其次查看 **ORDER BY** 子句，看看能不能将这些列包含在索引中，最后，看看 **SELECT** 子句中的列，如果你连这些也包含了，就得到了覆盖索引，MySQL 就能只用索引就完成你的查询，实现最快的查询速度


### 使用原则
1. 最左原则，将最常使用的列放在前面，查询从索引的最左列开始，并且不跳过索引中的列，如果跳跃某一列，索引将部分失效（后面的字段索引失效）。
联合索引顺序 和 查询条件字段 的顺序尽量保持一致。
联合索引中，出现范围查询（<, >），范围查询右侧的列索引失效。业务允许的情况下可以用 >= 或者 <= 来规避索引失效问题。
```sql
-- idx_age_state
EXPLAIN SELECT customer_id
FROM customers
WHERE age > 30 AND state = 'CA'; -- 右侧 state 索引失效

EXPLAIN SELECT customer_id
FROM customers
WHERE age >= 30 AND state = 'CA'; -- 右侧 state 索引有效
```
2. 把基数更高（约束性更高）的列放在前面（Cardinality 表示通过索引查询出来的唯一值的数量），非硬性要求，例如筛选时用了模糊查询，这条规则并没有起到作用，反而降低了查询性能，e.g.
```sql
EXPLAIN SELECT customer_id
FROM customers
WHERE state = 'CA' AND last_name LIKE 'A%';

CREATE INDEX idx_lastname_state ON customers (last_name, state);
```
3. 不要在索引列上进行运算操作，否则索引将失效。如：
explain select * from tb_user where substring(phone, 10, 2) = '15';
4。 模糊查询中，如果仅仅是尾部模糊匹配，索引不会是失效；如果是头部模糊匹配，索引失效。如：
explain select * from tb_user where profession like '%工程';
5. 对于 WHERE OR，可以将这个 OR 查询改写为两部分，分别用各自最合适的索引，再用 UNION 融合结果（注意 UNION 是自动去重的，如果要保留重复记录就要用 UNION ALL）
6. 如果 MySQL 评估使用索引比全表更慢，则不使用索引
7. 结合实际情况

### 设计原则
> 索引管理是个**根据业务查询需求需要不断去权衡成本效益，抓大放小，迭代优化**的过程

1. 针对于数据量较大，且查询比较频繁的表建立索引
2. 针对于常作为查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引
3. 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高
4. 如果是字符串类型的字段，字段长度较长，可以针对于字段的特点，建立前缀索引
5. 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表，提高查询效率
6. 要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价就越大，会影响增删改的效率
7. 如果索引列不能存储NULL值，请在创建表时使用NOT NULL约束它。当优化器知道每列是否包含NULL值时，它可以更好地确定哪个索引最有效地用于查询
8. 重复索引（duplicate index）：

MySQL 不会阻止你建立重复的索引，所以记得在建立新索引前前检查一下已有索引。验证后发现，具体而言：
同名索引是不被允许的：
```sql
CREATE INDEX idx_state_points ON customers (state, points);
-- Error Code: 1061. Duplicate key name 'idx_state_points'
```
对相同列的相同顺序建立不同名的索引，5.7 版本暂时允许，但 8.0 版本不被允许：
```sql
CREATE INDEX idx_state_points2 ON customers (state, points);
/* warning(s): 1831 Duplicate index 'idx_state_points2' 
defined on the table 'sql_store.customers'. 
This is deprecated (不赞成；弃用；不宜用) 
and will be disallowed in a future release. */
```

9. 冗余索引（redundant index）：

比如，已有 idx_state_points，那 idx_state 就是冗余的了，因为所有 idx_state 能满足的筛选和排序需求 idx_state_points 都能满足
但当已有 idx_state_points 时，idx_points 和 idx_points_state 并不是冗余的，因为它们可以满足不同的筛选和排序需求

10. 无用索引（unused index）:

常用查询、排序用不到的索引没必要建立，毕竟索引是会占用空间和拖慢数据更新速度的

## 保护数据库
### 创建用户
设置一个新用户，用户名为 john，可以选择用 @ 来限制他可以从哪些地方访问数据库
```sql
CREATE USER john  
-- 无限制，可从任何位置访问 

CREATE USER john@127.0.0.1;  
-- 限制ip地址，可以是特定电脑，也可以是特定网络服务器（web server）

CREATE USER john@localhost;  
-- 限制主机名，特定电脑

CREATE USER john@'codewithmosh.com';  
-- 限制域名（注意加引号），可以是该域名内的任意电脑，但子域名则不行 

CREATE USER john@'%.codewithmosh.com'; 
-- 加上了通配符 % ，可以是该域名及其子域名下的任意电脑
```
可以用 IDENTIFIED BY 来设置密码
```sql
CREATE USER john IDENTIFIED BY '1234' 
-- 可从任何地方访问，但密码为 '1234'
-- 该密码只是为了简化，请总是用很长的强密码
```

### 查看用户
**方法一**
可以看到罗列出的所有用户，有几个 MySQL 内部自动建立和使用的帐户（用户名均为 mysql.*）
```sql
-- root 才能查看
SELECT * FROM mysql.user;
```

**方法二**
直接点击左侧导航栏的 Administration 标签页里的 Users and Privileges，同样可以查看服务器上的用户列表和信息

### 删除用户
假设之前创建了 bob 的帐户，允许在 codewithmosh.com 域名内访问数据库，密码是 '1234'：
```sql
CREATE USER bob@codewithmosh.com IDENTIFIED BY '1234';
```
之后 bob 离开了组织，就应该删除它的账户，注意依然要在用户名后跟上 @主机名（host）
```sql
DROP USER bob@codewithmosh.com;
```


### 修改密码
**方法1**
用 SET 语句
```sql
SET PASSWORD FOR john = '1234'; -- 修改john的密码  
SET PASSWORD = '1234';   -- 修改当前登录账户的密码
```
**方法2**
用导航面板：还是在 Administration 标签页 Users and Privileges 里，点击用户 john，可修改其密码，最后记得点 Apply 应用。另外还可以点击 Expire Password 强制其密码过期，下次用户登录必须修改密码。

### 授予权限
对于网页或桌面应用程序的使用用户，给予其读写数据的权限，但禁止其增删表或修改表结构
```sql
CREATE USER moon_app IDENTIFIED BY '1234';

GRANT SELECT, INSERT, UPDATE, DELETE, EXECUTE
-- GRANT子句表明授予哪些权限
-- EXECUTE，执行存储过程
ON sql_store.* 
-- ON子句表明可访问哪些数据库和表
-- ON sql_store.* 代表可访问某数据库所有表，常见设置
-- 只允许访问特定表则是 ON sql_store.customers，不常见
TO moon_app;
-- 表明授权给哪个用户
-- 如果该用户有访问地址限制，也要加上，如：@ip地址/域名/主机名
```
对于管理员，给予其一个或多个数据库乃至整个服务器的管理权限，这不仅包括表中数据的读写，还包括增删表、修改表结构以及创建事务和触发器等
可以谷歌 **MySQL privileges**，第一个结果就是官方文档里罗列的所有可用的权限及含义，其中的 ALL 是最高权限，通常我们给予管理员 ALL 权限
```sql
GRANT ALL
ON sql_store.*
-- 如果是 *.*，则代表所有数据库的所有表或整个服务器
TO john;
```

### 查看权限
**方法1**
查看 john 的权限
```sql
SHOW GRANTS FOR john;
```
查看当前登录帐户的权限
```sql
SHOW GRANTS;
```


**方法2**
依然可以通过导航栏 Administration 标签页里的 Users and Privileges 来查看各用户的权限，其中 Administrative Roles 展示了该用户的**角色和全局权限（Global Privileges）**, 而 Schema Privileges 则显示该用户在**特定数据库的权限**。

### 撤销权限
假设我们错误的给予 moon_app 创建视图的权限：
```sql
GRANT CREATE VIEW  
ON sql_store.* 
TO moon_app;
```
若要收回此权限
```sql
REVOKE CREATE VIEW 
ON sql_store.*
FROM moon_app;
```

## MySQL 管理
### 系统数据库
Mysql数据库安装完成后，自带了一下四个数据库，具体作用如下：

| 数据库 | 含义 |
| --- | --- |
| mysql | 存储MySQL服务器正常运行所需要的各种信息 （时区、主从、用户、权限等） |
| information_schema | 提供了访问数据库元数据的各种表和视图，包含数据库、表、字段类型及访问权限等 |
| performance_schema | 为MySQL服务器运行时状态提供了一个底层监控功能，主要用于收集数据库服务器性能参数 |
| sys | 包含了一系列方便 DBA 和开发人员利用 performance_schema性能数据库进行性能调优和诊断的视图 |

### 常用工具
#### Mysql
该mysql不是指mysql服务，而是指mysql的客户端工具。
```sql
语法 ：
	mysql [options] [database]
选项 ：
	-u, --user=name #指定用户名
	-p, --password[=name] #指定密码
	-h, --host=name #指定服务器IP或域名
	-P, --port=port #指定连接端口
	-e, --execute=name #执行SQL语句并退出
```
-e选项可以在Mysql客户端执行SQL语句，而不用连接到MySQL数据库再执行，对于一些批处理脚本，这种方式尤其方便。
示例：
```sql
 mysql -u root -p MySQL_Advanced -e "select * from stu";
```

#### mysqladmin
mysqladmin 是一个执行管理操作的客户端程序。可以用它来检查服务器的配置和当前状态、创建并删除数据库等。
```sql
语法:
	mysqladmin [options] command ...
选项:
	-u, --user=name       #指定用户名
	-p, --password[=name] #指定密码
	-h, --host=name       #指定服务器IP或域名
	-P, --port=port       #指定连接端口


-- 通过帮助文档查看选项：
mysqladmin --help

Where command is a one or more of: (Commands may be shortened)
  create databasename   Create a new database
  debug                 Instruct server to write debug information to log
  drop databasename     Delete a database and all its tables
  extended-status       Gives an extended status message from the server
  flush-hosts           Flush all cached hosts
  flush-logs            Flush all logs
  flush-status          Clear status variables
  flush-tables          Flush all tables
  flush-threads         Flush the thread cache
  flush-privileges      Reload grant tables (same as reload)
  kill id,id,...        Kill mysql threads
  password [new-password] Change old password to new-password in current format
  ping                  Check if mysqld is alive
  processlist           Show list of active threads in server
  reload                Reload grant tables
  refresh               Flush all tables and close and open logfiles
  shutdown              Take server down
  status                Gives a short status message from the server
  start-replica         Start replication
  start-slave           Deprecated: use start-replica instead
  stop-replica          Stop replication
  stop-slave            Deprecated: use stop-replica instead
  variables             Prints variables available
  version               Get version info from server
```


#### mysqlbinlog
由于服务器生成的二进制日志文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使用到mysqlbinlog 日志管理工具。
```sql
语法 ：
	mysqlbinlog [options] log-files1 log-files2 ...
选项 ：
	-d, --database=name 指定数据库名称，只列出指定的数据库相关操作。
	-o, --offset=# 忽略掉日志中的前n行命令。
	-r,--result-file=name 将输出的文本格式日志输出到指定文件。
	-s, --short-form 显示简单格式， 省略掉一些信息。
	--start-datatime=date1 --stop-datetime=date2 指定日期间隔内的所有日志。
	--start-position=pos1 --stop-position=pos2 指定位置间隔内的所有日志。
```

示例:
A. 查看 binlog.000008这个二进制文件中的数据信息
```sql
[root@frx01 ~]# mysqlbinlog binlog.000008
# The proper term is pseudo_replica_mode, but we use this compatibility alias
# to make the statement usable on server versions 8.0.24 and older.
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
mysqlbinlog: File 'binlog.000008' not found (OS errno 2 - No such file or directory)
ERROR: Could not open log file
SET @@SESSION.GTID_NEXT= 'AUTOMATIC' /* added by mysqlbinlog */ /*!*/;
DELIMITER ;
# End of log file
/*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;
```

上述查看到的二进制日志文件数据信息量太多了，不方便查询。 我们可以加上一个参数 -s 来显示简单格式。
```sql
[root@frx01 ~]# mysqlbinlog -s binlog.000008
WARNING: --short-form is deprecated and will be removed in a future version

# The proper term is pseudo_replica_mode, but we use this compatibility alias
# to make the statement usable on server versions 8.0.24 and older.
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
mysqlbinlog: File 'binlog.000008' not found (OS errno 2 - No such file or directory)
ERROR: Could not open log file
SET @@SESSION.GTID_NEXT= 'AUTOMATIC' /* added by mysqlbinlog */ /*!*/;
DELIMITER ;
# End of log file
/*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;
```

#### mysqlshow
mysqlshow 客户端对象查找工具，用来很快地查找存在哪些数据库、数据库中的表、表中的列或者索引。
```sql
语法 ：
	mysqlshow [options] [db_name [table_name [col_name]]]
选项 ：
	--count 显示数据库及表的统计信息（数据库，表 均可以不指定）
	-i 显示指定数据库或者指定表的状态信息
示例：
	#查询test库中每个表中的字段书，及行数
	mysqlshow -uroot -p2143 test --count
	#查询test库中book表的详细情况
	mysqlshow -uroot -p2143 test book --count
```

示例：
A. 查询每个数据库的表的数量及表中记录的数量
mysqlshow -uroot -p123456 --count
```sql
[root@frx01 ~]# mysqlshow -uroot -p123456 --count
mysqlshow: [Warning] Using a password on the command line interface can be insecure.
+--------------------+--------+--------------+
|     Databases      | Tables |  Total Rows  |
+--------------------+--------+--------------+
| MySQL_Advanced     |      9 |        13582 |
| frx01              |      1 |      1000000 |
| information_schema |     79 |        31153 |
| mysql              |     37 |         3904 |
| performance_schema |    110 |       242999 |
| sys                |    101 |         5021 |
+--------------------+--------+--------------+
6 rows in set.
```
B. 查看数据库MySQL_Advanced的统计信息
mysqlshow -uroot -p123456 MySQL_Advanced --count
```sql
[root@frx01 ~]# mysqlshow -uroot -p123456 MySQL_Advanced --count
mysqlshow: [Warning] Using a password on the command line interface can be insecure.
Database: MySQL_Advanced
+-------------+----------+------------+
|   Tables    | Columns  | Total Rows |
+-------------+----------+------------+
| a           |        2 |          0 |
| employee    |        2 |          0 |
| stu         |        3 |          7 |
| stu_v_1     |        2 |         12 |
| student     |        6 |         18 |
| tb_user     |        9 |         24 |
| tb_user_pro |        3 |      13472 |
| user_logs   |        5 |         39 |
| user_v_1    |        2 |         10 |
+-------------+----------+------------+
9 rows in set.
```
C. 查看数据库db01中的course表的信息
mysqlshow -uroot -p123456 MySQL_Advanced stu --count
```sql
[root@frx01 ~]# mysqlshow -uroot -p123456 MySQL_Advanced stu --count
mysqlshow: [Warning] Using a password on the command line interface can be insecure.
Database: MySQL_Advanced  Table: stu  Rows: 7
+-------+--------------+--------------------+------+-----+---------+----------------+---------------------------------+---------+
| Field | Type         | Collation          | Null | Key | Default | Extra          | Privileges                      | Comment |
+-------+--------------+--------------------+------+-----+---------+----------------+---------------------------------+---------+
| id    | int          |                    | NO   | PRI |         | auto_increment | select,insert,update,references |         |
| age   | int          |                    | NO   | MUL |         |                | select,insert,update,references |         |
| name  | varchar(255) | utf8mb4_general_ci | YES  | MUL |         |                | select,insert,update,references |         |
+-------+--------------+--------------------+------+-----+---------+----------------+---------------------------------+---------+
```
D. 查看数据库db01中的course表的id字段的信息
mysqlshowMySQL show -uroot -p123456 MySQL_Advanced stu id --count
```sql
[root@frx01 ~]# mysqlshow -uroot -p123456 MySQL_Advanced stu id --count
mysqlshow: [Warning] Using a password on the command line interface can be insecure.
Database: MySQL_Advanced  Table: stu  Rows: 7  Wildcard: id
+-------+------+-----------+------+-----+---------+----------------+---------------------------------+---------+
| Field | Type | Collation | Null | Key | Default | Extra          | Privileges                      | Comment |
+-------+------+-----------+------+-----+---------+----------------+---------------------------------+---------+
| id    | int  |           | NO   | PRI |         | auto_increment | select,insert,update,references |         |
+-------+------+-----------+------+-----+---------+----------------+---------------------------------+---------+
```

#### mysqldump
mysqldump 客户端工具用来备份数据库或在不同数据库之间进行数据迁移。备份内容包含创建表，及插入表的SQL语句。
```sql
语法 ：
	mysqldump [options] db_name [tables]
	mysqldump [options] --database/-B db1 [db2 db3...]
	mysqldump [options] --all-databases/-A
连接选项 ：
	-u, --user=name        指定用户名
	-p, --password[=name]  指定密码
	-h, --host=name        指定服务器ip或域名
	-P, --port=#           指定连接端口
输出选项：
	--add-drop-database    在每个数据库创建语句前加上 drop database 语句
	--add-drop-table       在每个表创建语句前加上 drop table 语句 , 默认开启 ; 不
开启 (--skip-add-drop-table)
	-n, --no-create-db     不包含数据库的创建语句
	-t, --no-create-info   不包含数据表的创建语句
	-d --no-data           不包含数据
	-T, --tab=name         自动生成两个文件：一个.sql文件，创建表结构的语句；一个.txt文件，数据文件
```

示例:
A. 备份frx01数据库
mysqldump -uroot -p123456 frx01 > frx01.sql
```sql
[root@frx01 ~]# mysqldump -uroot -p123456 frx01 > frx01.sql
mysqldump: [Warning] Using a password on the command line interface can be insecure.
[root@frx01 ~]# ll
总用量 597048
-rw-r--r--. 1 root root 482420224 9月   9 10:23 ABCD.tar
-rw-------. 1 root root      1697 9月   9 09:45 anaconda-ks.cfg
-rw-r--r--  1 root root  70781900 10月  4 16:31 frx01.sql
-rw-r--r--. 1 root root      1745 9月   9 09:49 initial-setup-ks.cfg
-rw-r--r--  1 root root    508543 10月  1 23:47 itcast.sql
-rw-r--r--  1 root root  57650380 9月  25 09:40 load_user_100w_sort.sql
drwxr-xr-x. 2 root root         6 9月   9 09:55 公共
drwxr-xr-x. 2 root root         6 9月   9 09:55 模板
drwxr-xr-x. 2 root root         6 9月   9 09:55 视频
drwxr-xr-x. 2 root root         6 9月   9 09:55 图片
drwxr-xr-x. 2 root root         6 9月   9 09:55 文档
drwxr-xr-x. 2 root root         6 9月   9 09:55 下载
drwxr-xr-x. 2 root root         6 9月   9 09:55 音乐
drwxr-xr-x. 2 root root         6 9月   9 09:55 桌面
```
可以直接打开frx01.sql，来查看备份出来的数据到底什么样。
```sql
--
-- Table structure for table `user_logs`
--

DROP TABLE IF EXISTS `user_logs`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!50503 SET character_set_client = utf8mb4 */;
CREATE TABLE `user_logs` (
  `id` int NOT NULL AUTO_INCREMENT,
  `operation` varchar(20) NOT NULL COMMENT '操作类型, insert/update/delete',
  `operate_time` datetime NOT NULL COMMENT '操作时间',
  `operate_id` int NOT NULL COMMENT '操作的ID',
  `operate_params` varchar(500) DEFAULT NULL COMMENT '操作参数',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=39 DEFAULT CHARSET=utf8mb3;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `user_logs`
--
```
备份出来的数据包含：

- 删除表的语句
- 创建表的语句
- 数据插入语句

如果我们在数据备份时，不需要创建表，或者不需要备份数据，只需要备份表结构，都可以通过对应的参数来实现。
B. 备份frx01数据库中的表数据，不备份表结构(-t) mysqldump -uroot -p123456 -t frx01 > frx02.sql
打开 frx02.sql ，来查看备份的数据，只有insert语句，没有备份表结构。
```sql
LOCK TABLES `tb_user` WRITE;
/*!40000 ALTER TABLE `tb_user` DISABLE KEYS */;
INSERT INTO `tb_user` VALUES (1,'jdTmmKQlwu1','jdTmmKQlwu','jdTmmKQlwu','2020-10-13','1'),(2,'BTJOeWjRiw2','BTJOeWjRiw','BTJOeWjRiw','202
0-06-12','2'),(3,'waQTJIIlHI3','waQTJIIlHI','waQTJIIlHI','2020-06-02','0'),(4,'XmeFHwozIo4','XmeFHwozIo','XmeFHwozIo','2020-01-11','1'),(
5,'xRrvQSHcZn5','xRrvQSHcZn','xRrvQSHcZn','2020-10-18','2'),(6,'gTDfGFNLEj6','gTDfGFNLEj','gTDfGFNLEj','2020-01-13','0'),(7,'nBETIlVCle7'
,'nBETIlVCle','nBETIlVCle','2020-09-27','1'),(8,'vmePKKZjJU8','vmePKKZjJU','vmePKKZjJU','2020-10-20','2'),(9,'pWjaLhJVaB9','pWjaLhJVaB','
pWjaLhJVaB','2020-05-07','0'),(10,'zimgGFPEQe10','zimgGFPEQe','zimgGFPEQe','2020-08-01','1'),(11,'uHpIsEALNp11','uHpIsEALNp','uHpIsEALNp'
,'2020-12-17','2'),(12,'kCfPeCgMjn12','kCfPeCgMjn','kCfPeCgMjn','2020-07-13','0'),(13,'QRkLwosIdM13','QRkLwosIdM','QRkLwosIdM','2020-08-2
4','1'),(14,'ipsKyeFeJy14','ipsKyeFeJy','ipsKyeFeJy','2020-02-22','2'),(15,'ZRrcPvYDtF15','ZRrcPvYDtF','ZRrcPvYDtF','2020-12-07','0'),(16
,'BbLUYWHeTg16','BbLUYWHeTg','BbLUYWHeTg','2020-04-17','1'),(17,'DalMVUpMnk17','DalMVUpMnk','DalMVUpMnk','2020-10-16','2'),(18,'HFtmFtnZP
i18','HFtmFtnZPi','HFtmFtnZPi','2020-11-25','0'),(19,'maHoYocalj19','maHoYocalj','maHoYocalj','2020-03-03','1'),(20,'nHKmkDGSeH20','nHKmk
DGSeH','nHKmkDGSeH','2020-01-02','2'),(21,'MZZziMtXLH21','MZZziMtXLH','MZZziMtXLH','2020-04-01','0'),(22,'bjUXRogYgF22','bjUXRogYgF','bjU
XRogYgF','2020-07-12','1'),(23,'CVgMEIwyGf23','CVgMEIwyGf','CVgMEIwyGf','2020-12-05','2'),(24,'yXVrVLgSmR24','yXVrVLgSmR','yXVrVLgSmR','2
020-04-11','0'),(25,'oaFXNzAigC25','oaFXNzAigC','oaFXNzAigC','2020-11-09','1'),(26,'IJrJiutZtD26','IJrJiutZtD','IJrJiutZtD','2020-07-17',
'2'),(27,'WGwcrfrFOB27','WGwcrfrFOB','WGwcrfrFOB','2020-09-22','0'),(28,'RbCMhegoiU28','RbCMhegoiU','RbCMhegoiU','2020-06-01','1'),(29,'R
zRzNPEsQm29','RzRzNPEsQm','RzRzNPEsQm','2020-02-24','2'),(30,'SYzgGoVRwv30','SYzgGoVRwv','SYzgGoVRwv','2020-07-03','0'),(31,'hLuUHxjJhk31
','hLuUHxjJhk','hLuUHxjJhk','2020-01-09','1'),(32,'jhUhcVaQkV32','jhUhcVaQkV','jhUhcVaQkV','2020-04-27','2'),(33,'MmbKbOrEpK33','MmbKbOrE
pK','MmbKbOrEpK','2020-05-02','0');
```
C. 将frx01数据库的表的表结构与数据分开备份(-T)
mysqldump -uroot -p123456 -T /root frx01 score
> /root 是路径，frx01 是数据库，score 是表

执行上述指令，会出错，数据不能完成备份，原因是因为我们所指定的数据存放目录/root，MySQL认为是不安全的，需要存储在MySQL信任的目录下。那么，哪个目录才是MySQL信任的目录呢，可以查看一下系统变量 secure_file_priv 。执行结果如下：
```sql
mysql> show variables like '%secure_file_priv%';
+------------------+-----------------------+
| Variable_name    | Value                 |
+------------------+-----------------------+
| secure_file_priv | /var/lib/mysql-files/ |
+------------------+-----------------------+
1 row in set (0.01 sec)
```
```sql
mysqldump -uroot -p123456 -T /var/lib/mysql-files/ frx01 score
```

#### mysqlimport/source

1. mysqlimport

mysqlimport 是客户端数据导入工具，用来导入mysqldump 加 -T 参数后导出的文本文件。
```sql
语法 ：
	mysqlimport [options] db_name textfile1 [textfile2...]
示例 ：
	mysqlimport -uroot -p2143 test /tmp/city.txt
```

2. source

如果需要导入sql文件,可以使用mysql中的source 指令 :
```sql
语法 ：
	source /root/xxxxx.sql
```


## MySQL 日志
### 错误日志
错误日志是 MySQL 中最重要的日志之一，它记录了当 mysqld 启动和停止时，以及服务器在运行过程中发生任何严重错误时的相关信息。当数据库出现任何故障导致无法正常使用时，建议首先查看此日志。
该日志是默认开启的，默认存放目录 /var/log/，默认的日志文件名为 mysqld.log 。查看日志位置：
```sql
show variables like '%log_error%';
```

### 二进制日志
二进制日志（BINLOG）记录了所有的 DDL（数据定义语言）语句和 DML（数据操纵语言）语句，但不包括数据查询（SELECT、SHOW）语句。
作用：①. 灾难时的数据恢复；②. MySQL的主从复制。在MySQL8版本中，默认二进制日志是开启着的，涉及到的参数如下：
```sql
show variables like '%log_bin%';

-rw-r-----  1 mysql mysql       523 10月  2 00:02 binlog.000008
-rw-r-----  1 mysql mysql      9316 10月  2 14:50 binlog.000009
-rw-r-----  1 mysql mysql       398 10月  2 16:45 binlog.000010
-rw-r-----  1 mysql mysql       695 10月  3 00:25 binlog.000011
-rw-r-----  1 mysql mysql      1416 10月  3 15:36 binlog.000012
-rw-r-----  1 mysql mysql      1600 10月  4 22:30 binlog.000013
-rw-r-----  1 mysql mysql       156 10月  5 11:41 binlog.000014
-rw-r-----  1 mysql mysql       224 10月  5 11:41 binlog.index    #二进制索引文件
```
参数说明：

- log_bin_basename：当前数据库服务器的binlog日志的基础名称(前缀)，具体的binlog文件名需要在该basename的基础上加上编号(编号从000001开始)。
- log_bin_index：binlog的索引文件，里面记录了当前服务器关联的binlog文件有哪些。

#### 日志格式
MySQL服务器中提供了多种格式来记录二进制日志，具体格式及特点如下：

| 日志格式 | 含义 |
| --- | --- |
| STATEMENT | 基于SQL语句的日志记录，记录的是SQL语句，对数据进行修改的SQL都会记录在日志文件中。 |
| ROW | 基于行的日志记录，记录的是每一行的数据变更。（默认） |
| MIXED | 混合了STATEMENT和ROW两种格式，默认采用STATEMENT，在某些特殊情况下会自动切换为ROW进行记录。 |

```sql
show variables like '%binlog_format';
```
如果我们需要配置二进制日志的格式，只需要在 /etc/my.cnf 中配置 binlog_format 参数即可。

**查看**
由于日志是以二进制方式存储的，不能直接读取，需要通过二进制日志查询工具 mysqlbinlog 来查看，具体语法：
```sql
mysqlbinlog [ 参数选项 ] logfilename

参数选项：
	-d 指定数据库名称，只列出指定的数据库相关操作。
	-o 忽略掉日志中的前n行命令。
	-v 将行事件(数据变更)重构为SQL语句
	-vv 将行事件(数据变更)重构为SQL语句，并输出注释信息
```

**删除**
对于比较繁忙的业务系统，每天生成的binlog数据巨大，如果长时间不清除，将会占用大量磁盘空间。可以通过以下几种方式清理日志：

| 指令 | 含义 |
| --- | --- |
| reset master | 删除全部 binlog 日志，删除之后，日志编号，将从 binlog.000001重新开始 |
| purge master logs to 'binlog.*' | 删除 * 编号之前的所有日志 |
| purge master logs before 'yyyy-mm-dd hh:mm:ss' | m删除日志为 "yyyy-mm-dd hh:m:ss" 之前产生的所有日志 |

也可以在mysql的配置文件中配置二进制日志的过期时间，单位是秒，默认30天，设置了之后，二进制日志过期会自动删除。
```sql
show variables like '%binlog_expire_logs_seconds%';
```


#### 查询日志
查询日志中记录了客户端的所有操作语句，而二进制日志不包含查询数据的SQL语句。默认情况下，查询日志是未开启的。
```sql
show variables like '%general%';
```
如果需要开启查询日志，可以修改MySQL的配置文件 /etc/my.cnf 文件，添加如下内容：
```sql
#该选项用来开启查询日志 ， 可选值 ： 0 或者 1 ； 0 代表关闭， 1 代表开启

general_log=1

#设置日志的文件名 ， 如果没有指定， 默认的文件名为 host_name.log

general_log_file=mysql_query.log
```
开启了查询日志之后，在MySQL的数据存放目录，也就是 /var/lib/mysql/ 目录下就会出现mysql_query.log 文件。之后所有的客户端的增删改查操作都会记录在该日志文件之中，长时间运行后，该日志文件将会非常大。
#### 慢查询日志
慢查询日志记录了所有执行时间超过参数 long_query_time 设置值并且扫描记录数不小于 min_examined_row_limit 的所有的SQL语句的日志，默认未开启。long_query_time 默认为10 秒，最小为 0， 精度可以到微秒。
如果需要开启慢查询日志，需要在MySQL的配置文件 /etc/my.cnf 中配置如下参数：
```sql
#慢查询日志

slow_query_log=1

#执行时间参数

long_query_time=2

```
默认情况下，不会记录管理语句，也不会记录不使用索引进行查找的查询。可以使用 log_slow_admin_statements和更改此行为 log_queries_not_using_indexes，如下所述。
```sql
#记录执行较慢的管理语句

log_slow_admin_statements =1

#记录执行较慢的未使用索引的语句

log_queries_not_using_indexes = 1
```
上述所有的参数配置完成之后，都需要重新启动MySQL服务器才可以生效。
```sql
[root@frx01 mysql]# tail -f frx01-slow.log
# Query_time: 4.687803  Lock_time: 0.000077 Rows_sent: 1  Rows_examined: 0
use frx01;
SET timestamp=1664871559;
SELECT COUNT(*) FROM `tb_user`;
/usr/sbin/mysqld, Version: 8.0.26 (MySQL Community Server - GPL). started with:
Tcp port: 3306  Unix socket: /var/lib/mysql/mysql.sock
Time                 Id Command    Argument
/usr/sbin/mysqld, Version: 8.0.26 (MySQL Community Server - GPL). started with:
Tcp port: 3306  Unix socket: /var/lib/mysql/mysql.sock
Time                 Id Command    Argument
# Time: 2022-10-05T13:40:50.099040Z
# User@Host: root[root] @ localhost []  Id:     8
# Query_time: 3.980600  Lock_time: 0.000070 Rows_sent: 0  Rows_examined: 1000000
use frx01;
SET timestamp=1664977246;
select * from tb_user limit 1000000,10;
```


## MySQL 主从复制
主从复制是指将主数据库的 DDL 和 DML 操作通过二进制日志传到从库服务器中，然后在从库上对这些日志重新执行（也叫重做），从而使得从库和主库的数据保持同步。
MySQL支持一台主库同时向多台从库进行复制， 从库同时也可以作为其他从服务器的主库，实现链状复制。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676189704481-2398a7b9-5b30-42f8-aa2a-69dc84685fdd.webp#averageHue=%23fefefc&clientId=u48ee23d5-f3ff-4&from=paste&id=u006d0f55&originHeight=267&originWidth=686&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u87db77b7-1be9-4716-8855-9531ea2bb9b&title=)
MySQL 复制的优点主要包含以下三个方面：

- 主库出现问题，可以快速切换到从库提供服务。
- 实现读写分离，降低主库的访问压力。
- 可以在从库中执行备份，以避免备份期间影响主库服务。

### 原理
MySQL主从复制的核心就是 二进制日志，具体的过程如下：
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676189749199-c6b0ad9d-786a-4c85-b178-981a009d6c2e.webp#averageHue=%23f9f9f8&clientId=u48ee23d5-f3ff-4&from=paste&id=uaeb79ef9&originHeight=586&originWidth=1311&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u32cc924d-a829-489c-8b6a-c628ebc84b8&title=)
从上图来看，复制分成三步：

1. Master 主库在事务提交时，会把数据变更记录在二进制日志文件 Binlog 中。
2. 从库读取主库的二进制日志文件 Binlog ，写入到从库的中继日志 Relay Log 。
3. slave重做中继日志中的事件，将中继日志中的数据变更再反映到自身的数据变化。

### 搭建
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676189962066-2dc132a9-b372-4b7e-a340-26b04c7ff0fd.webp#averageHue=%23fbfaed&clientId=u48ee23d5-f3ff-4&from=paste&id=ud42260d2&originHeight=310&originWidth=1249&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u7367208a-2ba4-406b-9f8b-701adc44ee3&title=)
准备好两台服务器之后，在上述的两台服务器中分别安装好MySQL，并完成基础的初始化准备(安装、密码配置等操作)工作。 其中：

- 192.168.91.166 作为主服务器master
- 192.168.91.167 作为从服务器slave

#### 关闭防火墙
生产环境不要关闭整个防火墙，而是开放主机的指定端口号
```sql
firewall-cmd --zone=public --add-port=3306 -permanent
fierwall-cmd -reload

systemctl stop firewalld
# 关闭开机自启
systemctl disable firewalld 
```

#### 主库配置

1. 修改配置文件 /etc/my.cnf
```sql
#mysql 服务ID，保证整个集群环境中唯一，取值范围：1 – 232-1，默认为1，和从库不一样即可

server-id=1

#是否只读,1 代表只读, 0 代表读写

read-only=0

#忽略的数据, 指不需要同步的数据库
#binlog-ignore-db=mysql
#指定同步的数据库
#binlog-do-db=db01
```

2. 重启MySQL服务器



3. 登录mysql，创建远程连接的账号，并授予主从复制权限
```sql
#创建itcast用户，并设置密码，该用户可在任意主机连接该MySQL服务
CREATE USER 'itcast'@'%' IDENTIFIED WITH mysql_native_password BY 'Root@123456';

#为 'itcast'@'%' 用户分配主从复制权限
GRANT REPLICATION SLAVE ON *.* TO 'itcast'@'%';
```

4. 通过指令，查看二进制日志坐标
```sql
 show master status;

mysql>  show master status;
+---------------+----------+--------------+------------------+-------------------+
| File          | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+---------------+----------+--------------+------------------+-------------------+
| binlog.000019 |      663 |              |                  |                   |
+---------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)
```
字段含义说明：

- file : 从哪个日志文件开始推送日志文件
- position ： 从哪个位置开始推送日志
- binlog_ignore_db : 指定不需要同步的数据库

#### 从库配置

1. 修改配置文件 /etc/my.cnf
```sql
#mysql 服务ID，保证整个集群环境中唯一，取值范围：1 – 2^32-1，和主库不一样即可

server-id=2

#是否只读,1 代表只读, 0 代表读写

read-only=1
```

2. 重新启动MySQL服务
```sql
systemctl restart mysqld
```

3. 登录mysql，设置主库配置
> 这里的binlog.000004，663一定要与master二进制日志坐标保持一致。

```sql
CHANGE REPLICATION SOURCE TO SOURCE_HOST='192.168.91.166', SOURCE_USER='itcast', SOURCE_PASSWORD='Root@123456', SOURCE_LOG_FILE='binlog.000004', SOURCE_LOG_POS=663;
```
上述是8.0.23中的语法。如果mysql是 8.0.23 之前的版本，执行如下SQL：
```sql
CHANGE MASTER TO MASTER_HOST='192.168.91.166', MASTER_USER='itcast', MASTER_PASSWORD='Root@123456', MASTER_LOG_FILE='binlog.000004', MASTER_LOG_POS=663;
```

| 参数名 | 含义 | 8.0.23之前 |
| --- | --- | --- |
| SOURCE_HOST | 主库IP地址 | MASTER_HOST |
| SOURCE_USER | 连接主库的用户名 | MASTER_USER |
| SOURCE_PASSWORD | 连接主库的密码 | MASTER_PASSWORD |
| SOURCE_LOG_FILE | binlog日志文件名 | MASTER_LOG_FILE |
| SOURCE_LOG_POS | binlog日志文件位置 | MASTER_LOG_POS |

4. 开启同步操作
```sql
start replica ; #8.0.22之后
start slave ; #8.0.22之前
```

5. 查看主从同步状态
```sql
show replica status ; #8.0.22之后
show slave status ; #8.0.22之前

mysql> show replica status\G
*************************** 1. row ***************************
             Replica_IO_State: Waiting for source to send event
                  Source_Host: 192.168.91.166
                  Source_User: itcast
                  Source_Port: 3306
                Connect_Retry: 60
              Source_Log_File: binlog.000001
          Read_Source_Log_Pos: 156
               Relay_Log_File: MySQL-Slave-relay-bin.000003
                Relay_Log_Pos: 365
        Relay_Source_Log_File: binlog.000001
           Replica_IO_Running: Yes
          Replica_SQL_Running: Yes
```

测试

1. 在主库 192.168.91.166 上创建数据库、表，并插入数据
```sql
create database db01;
use db01;
create table tb_user(
	id int(11) primary key not null auto_increment,
	name varchar(50) not null,
	sex varchar(1)
)engine=innodb default charset=utf8mb4;
insert into tb_user(id,name,sex) values(null,'Tom', '1'),(null,'Trigger','0'),(null,'Dawn','1');
```

2. 在从库 192.168.91.167 中查询数据，验证主从是否同步

![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676191375953-daa7a942-8261-4a44-b651-651f0425c835.webp#averageHue=%23444036&clientId=u48ee23d5-f3ff-4&from=paste&id=ue51d3ca1&originHeight=347&originWidth=1446&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=ud2aec7e4-4449-4c8e-9cca-7ca166d8199&title=)

## MySQL 分库分表
### 介绍
#### 问题分析
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676192347850-9a889fc5-689a-4f6b-9467-740c13b0a3fc.webp#averageHue=%23f9e6e2&clientId=u48ee23d5-f3ff-4&from=paste&id=uf65e15ed&originHeight=212&originWidth=808&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=ub6d119ab-4b7b-4169-94f6-2229ccc8286&title=)
随着互联网及移动互联网的发展，应用系统的数据量也是成指数式增长，若采用单数据库进行数据存储，存在以下性能瓶颈：

1. IO瓶颈：热点数据太多，数据库缓存不足，产生大量磁盘IO，效率较低。 请求数据太多，带宽不够，网络IO瓶颈。
2. CPU瓶颈：排序、分组、连接查询、聚合统计等SQL会耗费大量的CPU资源，请求数太多，CPU出现瓶颈。

为了解决上述问题，我们需要对数据库进行分库分表处理。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676192367318-9deec2be-585a-41f1-9cf8-4bb0855810c3.webp#averageHue=%23fbfafa&clientId=u48ee23d5-f3ff-4&from=paste&id=u499f9980&originHeight=415&originWidth=1280&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=ue947f7fc-ca49-4746-92db-6cd9ed367c3&title=)
分库分表的中心思想都是将数据分散存储，使得单一数据库/表的数据量变小来缓解单一数据库的性能问题，从而达到提升数据库性能的目的。

#### 拆分策略
分库分表的形式，主要是两种：垂直拆分和水平拆分。而拆分的粒度，一般又分为分库和分表，所以组成的拆分策略最终如下：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1676192412606-c59a9d3f-376e-4e7e-9568-b46361280a22.png#averageHue=%23fcfcfb&clientId=u48ee23d5-f3ff-4&from=paste&height=264&id=ua43cebe6&originHeight=527&originWidth=1130&originalType=binary&ratio=2&rotation=0&showTitle=false&size=122294&status=done&style=none&taskId=uc83eb250-3d31-46fa-9890-71775b69b7b&title=&width=565)
#### 垂直拆分
##### 垂直分库
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1676192445799-36d0d44b-a70f-4c60-9078-6729afc9e520.png#averageHue=%23cec7b6&clientId=u48ee23d5-f3ff-4&from=paste&height=286&id=ucba3b624&originHeight=572&originWidth=1087&originalType=binary&ratio=2&rotation=0&showTitle=false&size=247805&status=done&style=none&taskId=u164f3fe3-2b03-4db5-88be-eb60db8eb6c&title=&width=543.5)

垂直分库：以表为依据，根据业务将不同表拆分到不同库中。
特点：

- 每个库的表结构都不一样。
- 每个库的数据也不一样。
- 所有库的并集是全量数据。

##### 垂直分表
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676192848017-ee66a2c8-4526-44fd-9a11-a61d158e3f50.webp#averageHue=%23f5f4f2&clientId=u48ee23d5-f3ff-4&from=paste&id=ub64d3db4&originHeight=630&originWidth=1084&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u6d0226ec-d8bb-4a0b-b2eb-4fe9216323c&title=)
垂直分表：以字段为依据，根据字段属性将不同字段拆分到不同表中。
特点：

- 每个表的结构都不一样。
- 每个表的数据也不一样，一般通过一列（主键/外键）关联。
- 所有表的并集是全量数据。



#### 水平拆分
##### 水平分库
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676192995079-8fdd9fcb-fcce-4652-9ed2-3f894f06f794.webp#averageHue=%23cfc7b5&clientId=u48ee23d5-f3ff-4&from=paste&id=ucafd3ba9&originHeight=613&originWidth=1086&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u053910a1-cca5-4f8a-a9ba-c2fb1f05d4c&title=)
水平分库：以字段为依据，按照一定策略，将一个库的数据拆分到多个库中。
特点：

- 每个库的表结构都一样。
- 每个库的数据都不一样。
- 所有库的并集是全量数据。

##### 水平分表
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676192994578-65d6dd72-6b34-402f-939c-ec772bd3cf1b.webp#averageHue=%23cfc8b7&clientId=u48ee23d5-f3ff-4&from=paste&id=udcbfa400&originHeight=583&originWidth=1074&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u66daad85-dd3f-46fa-a9b5-39fa107b698&title=)
水平分表：以字段为依据，按照一定策略，将一个表的数据拆分到多个表中。
特点：

- 每个表的表结构都一样。
- 每个表的数据都不一样。
- 所有表的并集是全量数据。

在业务系统中，为了缓解磁盘IO及CPU的性能瓶颈，到底是垂直拆分，还是水平拆分；具体是分库，还是分表，都需要根据具体的业务需求具体分析。

#### 实现技术

- shardingJDBC：基于AOP原理，在应用程序中对本地执行的SQL进行拦截，解析、改写、路由处理。需要自行编码配置实现，只支持java语言，性能较高。
- MyCat：数据库分库分表中间件，不用调整代码即可实现分库分表，支持多种语言，性能不及前者。

![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676193262399-3802e7fe-1acf-40f1-8e6a-7957f65ebde9.webp#averageHue=%23f9f8f7&clientId=u48ee23d5-f3ff-4&from=paste&id=udb1abf6e&originHeight=285&originWidth=1135&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u2aa1ad55-743f-403e-b69e-d7c1539f1d8&title=)
本次课程，我们选择了是MyCat数据库中间件，通过MyCat中间件来完成分库分表操作。

### MyCat概述
#### 介绍
Mycat是开源的、活跃的、基于Java语言编写的MySQL数据库中间件。可以像使用mysql一样来使用mycat，对于开发人员来说根本感觉不到mycat的存在。
开发人员只需要连接MyCat即可，而具体底层用到几台数据库，每一台数据库服务器里面存储了什么数据，都无需关心。 具体的分库分表的策略，只需要在MyCat中配置即可。

优势：

- 性能可靠稳定
- 强大的技术团队
- 体系完善
- 社区活跃

#### 安装
Mycat是采用java语言开发的开源的数据库中间件，支持Windows和Linux运行环境，下面介绍MyCat的Linux中的环境搭建。我们需要在准备好的服务器中安装如下软件。

- MySQL
- JDK
- Mycat
| 服务器 | 安装软件 | 说明 |
| --- | --- | --- |
| 192.168.91.166 | JDK、Mycat | MyCat中间件服务器 |
| 192.168.91.166 | MySQL | 分片服务器 |
| 192.168.91.167 | MySQL | 分片服务器 |
| 192.168.91.168 | MySQL | 分片服务器 |

- [jdk安装步骤](https://frxcat.fun/pages/600247/#%E5%AE%89%E8%A3%85%E6%96%B0%E7%9A%84jdk)
- 安装Mycat
- 使用XFTP工具将下载好的文件上传到Linux系统上。
- 使用解压命令
```sql
tar -zxvf Mycat-server-1.6.7.3-release-20190828135747-linux.tar.gz -C /usr/local
```
#### 目录介绍
```sql
[root@MySQL-Master mycat]# ll
总用量 12
drwxr-xr-x 2 root root  190 10月  6 11:36 bin
drwxrwxrwx 2 root root    6 7月  18 2019 catlet
drwxrwxrwx 4 root root 4096 10月  6 11:36 conf
drwxr-xr-x 2 root root 4096 10月  6 11:36 lib
drwxrwxrwx 2 root root    6 8月  28 2019 logs
-rwxrwxrwx 1 root root  227 8月  28 2019 version.txt
```
bin : 存放可执行文件，用于启动停止mycat
conf：存放mycat的配置文件
lib：存放mycat的项目依赖包（jar）
logs：存放mycat的日志文件

- 由于mycat中的mysql的JDBC驱动包版本比较低，所以我们将它删去，下载8.0版本的
```sql
cd /usr/local/mycat/lib/
rm -rf mysql-connector-java-5.1.35.jar
```

- [mysql驱动包下载地址(opens new window)](https://downloads.mysql.com/archives/c-j/)
- 将下载好的驱动包通过XFTP工具上传到Linux系统的/usr/local/mycat/lib/目录。

#### 概念介绍
在MyCat的整体结构中，分为两个部分：上面的逻辑结构、下面的物理结构。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676193720082-39800c4a-9147-4385-aa8c-f0bb50fed518.webp#averageHue=%23edebd8&clientId=u48ee23d5-f3ff-4&from=paste&id=u40726e80&originHeight=671&originWidth=1432&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u7e51e6f9-c323-40ae-8b26-8a4755aa7e7&title=)
在MyCat的逻辑结构主要负责逻辑库、逻辑表、分片规则、分片节点等逻辑结构的处理，而具体的数据存储还是在物理结构，也就是数据库服务器中存储的。
在后面讲解MyCat入门以及MyCat分片时，还会讲到上面所提到的概念。

### MyCat入门
#### 需求
由于 tb_order 表中数据量很大，磁盘IO及容量都到达了瓶颈，现在需要对 tb_order 表进行数据分片，分为三个数据节点，每一个节点主机位于不同的服务器上, 具体的结构，参考下图：
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676194224346-b014fc15-7cb4-49a7-9399-aec05a787c80.webp#averageHue=%23fcfcfc&clientId=u48ee23d5-f3ff-4&from=paste&id=u6d11b168&originHeight=297&originWidth=394&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u5c06a4e1-7ef9-4fa4-9158-4b1c44fed77&title=)
#### 环境准备
准备3台服务器：

- 192.168.91.166：MyCat中间件服务器，同时也是第一个分片服务器。
- 192.168.91.167：第二个分片服务器。
- 192.168.91.168：第三个分片服务器。

![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676194221557-d44de426-8249-467d-a968-f93f820cbc32.webp#averageHue=%23f8f8f6&clientId=u48ee23d5-f3ff-4&from=paste&id=u864ae7ce&originHeight=400&originWidth=519&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u4e8a0a1a-2191-45ad-80d7-4fe50df6c8e&title=)
并且在上述3台数据库中创建数据库 db01 。
#### 配置

1. schema.xml

在schema.xml中配置逻辑库、逻辑表、数据节点、节点主机等相关信息。具体的配置如下：
```sql
<?xml version="1.0"?>
<!DOCTYPE mycat:schema SYSTEM "schema.dtd">
<mycat:schema xmlns:mycat="http://io.mycat/">
    <schema name="DB01" checkSQLschema="true" sqlMaxLimit="100">
        <table name="TB_ORDER" dataNode="dn1,dn2,dn3" rule="auto-sharding-long"/>
    </schema>
    <dataNode name="dn1" dataHost="dhost1" database="db01"/>
    <dataNode name="dn2" dataHost="dhost2" database="db01"/>
    <dataNode name="dn3" dataHost="dhost3" database="db01"/>
    <dataHost name="dhost1" maxCon="1000" minCon="10" balance="0" writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1" slaveThreshold="100">
        <heartbeat>select user()</heartbeat>
        <writeHost host="master" url="jdbc:mysql://192.168.91.166:3306?useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" user="root" password="123456"/>
    </dataHost>
    <dataHost name="dhost2" maxCon="1000" minCon="10" balance="0" writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1" slaveThreshold="100">
        <heartbeat>select user()</heartbeat>
        <writeHost host="master" url="jdbc:mysql://192.168.91.167:3306?useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" user="root" password="123456"/>
    </dataHost>
    <dataHost name="dhost3" maxCon="1000" minCon="10" balance="0" writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1" slaveThreshold="100">
        <heartbeat>select user()</heartbeat>
        <writeHost host="master" url="jdbc:mysql://192.168.91.168:3306?useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" user="root" password="123456"/>
    </dataHost>
</mycat:schema>
```

2. server.xml

需要在server.xml中配置用户名、密码，以及用户的访问权限信息，具体的配置如下：
```sql
<user name="root" defaultAccount="true">
	<property name="password">123456</property>
	<property name="schemas">DB01</property>
	<!-- 表级 DML 权限设置 -->
	<!--
	<privileges check="true">
		<schema name="DB01" dml="0110" >
			<table name="TB_ORDER" dml="1110"></table>
		</schema>
	</privileges>
	-->
</user>
<user name="user">
	<property name="password">123456</property>
	<property name="schemas">DB01</property>
	<property name="readOnly">true</property>
</user>
```
上述的配置表示，定义了两个用户 root 和 user ，这两个用户都可以访问 DB01 这个逻辑库，访问密码都是123456，但是root用户访问DB01逻辑库，既可以读，又可以写，但是 user用户访问DB01逻辑库是只读的。
```sql
<?xml version="1.0" encoding="UTF8"?>
<!DOCTYPE mycat:server SYSTEM "server.dtd">
<mycat:server xmlns:mycat="http://io.mycat/">
	<system>
	<property name="nonePasswordLogin">0</property> 
	<property name="useHandshakeV10">1</property>
	<property name="useSqlStat">0</property>  
	<property name="useGlobleTableCheck">0</property>  
		<property name="sqlExecuteTimeout">300</property>  
		<property name="sequnceHandlerType">2</property>
		<property name="sequnceHandlerPattern">(?:(\s*next\s+value\s+for\s*MYCATSEQ_(\w+))(,|\)|\s)*)+</property>
	<property name="subqueryRelationshipCheck">false</property> 
    
		<property name="processorBufferPoolType">0</property>
		<property name="handleDistributedTransactions">0</property>

		<property name="useOffHeapForMerge">0</property>

        <property name="memoryPageSize">64k</property>

		<property name="spillsFileBufferSize">1k</property>

		<property name="useStreamOutput">0</property>

		<property name="systemReserveMemorySize">384m</property>


		<property name="useZKSwitch">false</property>
		<property name="strictTxIsolation">false</property>
		
		<property name="useZKSwitch">true</property>
		
	</system>
	
	<user name="root" defaultAccount="true">
		<property name="password">123456</property>
		<property name="schemas">DB01</property>
	</user>

	<user name="user">
		<property name="password">123456</property>
		<property name="schemas">DB01</property>
		<property name="readOnly">true</property>
	</user>

</mycat:server>
```


#### 测试
##### 启动
配置完毕后，先启动涉及到的3台分片服务器，然后启动MyCat服务器。切换到Mycat的安装目录，执行如下指令，启动Mycat：
```sql
#启动
bin/mycat start
#停止
bin/mycat stop
```
Mycat启动之后，占用端口号 8066。
启动完毕之后，可以查看logs目录下的启动日志，查看Mycat是否启动完成。
```sql
[root@MySQL-Master mycat]# tail -10 logs/wrapper.log
STATUS | wrapper  | 2022/10/06 23:08:01 | TERM trapped.  Shutting down.
STATUS | wrapper  | 2022/10/06 23:08:03 | <-- Wrapper Stopped
STATUS | wrapper  | 2022/10/06 23:08:08 | --> Wrapper Started as Daemon
STATUS | wrapper  | 2022/10/06 23:08:08 | Launching a JVM...
INFO   | jvm 1    | 2022/10/06 23:08:08 | Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=64M; support was removed in 8.0
INFO   | jvm 1    | 2022/10/06 23:08:08 | Wrapper (Version 3.2.3) http://wrapper.tanukisoftware.org
INFO   | jvm 1    | 2022/10/06 23:08:08 |   Copyright 1999-2006 Tanuki Software, Inc.  All Rights Reserved.
INFO   | jvm 1    | 2022/10/06 23:08:08 |
INFO   | jvm 1    | 2022/10/06 23:08:09 | Loading class `com.mysql.jdbc.Driver'. This is deprecated. The new driver class is `com.mysql.cj.jdbc.Driver'. The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
INFO   | jvm 1    | 2022/10/06 23:08:11 | MyCAT Server startup successfully. see logs in logs/mycat.log
```

##### 连接MyCat
通过如下指令，就可以连接并登陆MyCat。
```sql
[root@MySQL-Master ~]# mysql -h 192.168.91.166 -P 8066 -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.6.29-mycat-1.6.7.3-release-20190828215749 MyCat Server (OpenCloudDB)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```
我们看到我们是通过MySQL的指令来连接的MyCat，因为MyCat在底层实际上是模拟了MySQL的协议。
##### 数据测试
然后就可以在MyCat中来创建表，并往表结构中插入数据，查看数据在MySQL中的分布情况。
```sql
CREATE TABLE TB_ORDER (
	id BIGINT(20) NOT NULL,
    title VARCHAR(100) NOT NULL ,
	PRIMARY KEY (id)
) ENGINE=INNODB DEFAULT CHARSET=utf8 ;
INSERT INTO TB_ORDER(id,title) VALUES(1,'goods1');
INSERT INTO TB_ORDER(id,title) VALUES(2,'goods2');
INSERT INTO TB_ORDER(id,title) VALUES(3,'goods3');

INSERT INTO TB_ORDER(id,title) VALUES(1,'goods1');
INSERT INTO TB_ORDER(id,title) VALUES(2,'goods2');
INSERT INTO TB_ORDER(id,title) VALUES(3,'goods3');
INSERT INTO TB_ORDER(id,title) VALUES(5000000,'goods5000000');
INSERT INTO TB_ORDER(id,title) VALUES(10000000,'goods10000000');
INSERT INTO TB_ORDER(id,title) VALUES(10000001,'goods10000001');
INSERT INTO TB_ORDER(id,title) VALUES(15000000,'goods15000000');
INSERT INTO TB_ORDER(id,title) VALUES(15000001,'goods15000001');
```
```sql
mysql> INSERT INTO TB_ORDER(id,title) VALUES(5000000,'goods5000000');
Query OK, 1 row affected (0.00 sec)
 OK!

mysql> INSERT INTO TB_ORDER(id,title) VALUES(10000000,'goods10000000');
Query OK, 1 row affected (0.03 sec)
 OK!

mysql> INSERT INTO TB_ORDER(id,title) VALUES(10000001,'goods10000001');
Query OK, 1 row affected (0.00 sec)
 OK!

mysql> INSERT INTO TB_ORDER(id,title) VALUES(15000000,'goods15000000');
Query OK, 1 row affected (0.00 sec)
 OK!

mysql> INSERT INTO TB_ORDER(id,title) VALUES(15000001,'goods15000001');
ERROR 1064 (HY000): can't find any valid datanode :TB_ORDER -> ID -> 15000001
```
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676194557954-96f8aa2a-44a5-4192-921d-ac44816d9d08.webp#averageHue=%23e6e4dd&clientId=u48ee23d5-f3ff-4&from=paste&id=u49180e4a&originHeight=226&originWidth=1162&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=ufee83cf5-54c3-43bc-a902-16dbb7b32ab&title=)
经过测试，我们发现，在往 TB_ORDER 表中插入数据时：

- 如果id的值在1-500w之间，数据将会存储在第一个分片数据库中。
- 如果id的值在500w-1000w之间，数据将会存储在第二个分片数据库中。
- 如果id的值在1000w-1500w之间，数据将会存储在第三个分片数据库中。
- 如果id的值超出1500w，在插入数据时，将会报错。

为什么会出现这种现象，数据到底落在哪一个分片服务器到底是如何决定的呢？ 这是由逻辑表配置时的一个参数 rule 决定的，而这个参数配置的就是分片规则，关于分片规则的配置，在后面会详细讲解。

### MyCat 配置
[https://frxcat.fun/database/MySQL/MySQL_Mycat/#mycat-%E9%85%8D%E7%BD%AE](https://frxcat.fun/database/MySQL/MySQL_Mycat/#mycat-%E9%85%8D%E7%BD%AE)

### MyCat 分片
[https://frxcat.fun/database/MySQL/MySQL_Mycat/#mycat-%E5%88%86%E7%89%87](https://frxcat.fun/database/MySQL/MySQL_Mycat/#mycat-%E5%88%86%E7%89%87)

### 分片规则
[https://frxcat.fun/database/MySQL/MySQL_Mycat/#%E5%88%86%E7%89%87%E8%A7%84%E5%88%99](https://frxcat.fun/database/MySQL/MySQL_Mycat/#%E5%88%86%E7%89%87%E8%A7%84%E5%88%99)

### MyCat 管理及监控
[https://frxcat.fun/database/MySQL/MySQL_Mycat/#mycat-%E7%AE%A1%E7%90%86%E5%8F%8A%E7%9B%91%E6%8E%A7](https://frxcat.fun/database/MySQL/MySQL_Mycat/#mycat-%E7%AE%A1%E7%90%86%E5%8F%8A%E7%9B%91%E6%8E%A7)

## 
MySQL 读写分离
读写分离,简单地说是把对数据库的读和写操作分开,以对应不同的数据库服务器。主数据库提供写操作，从数据库提供读操作，这样能有效地减轻单台数据库的压力。
通过MyCat即可轻易实现上述功能，不仅可以支持MySQL，也可以支持Oracle和SQL Server。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1676194979592-5de91986-3994-42d5-962b-9a0fb0839175.png#averageHue=%23faf9f8&clientId=u48ee23d5-f3ff-4&from=paste&height=333&id=ud935c161&originHeight=665&originWidth=1667&originalType=binary&ratio=2&rotation=0&showTitle=false&size=250673&status=done&style=none&taskId=u86632d2f-03de-4919-b1f1-9fb3db14a1d&title=&width=833.5)

### 一主一从
#### 原理
MySQL的主从复制，是基于二进制日志（binlog）实现的。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676194960807-defe5e52-24a8-4fa8-a970-62e26f429d2a.webp#averageHue=%23f9f9f8&clientId=u48ee23d5-f3ff-4&from=paste&id=u2c43dab3&originHeight=748&originWidth=1578&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=ue16c9f39-b7d1-4be4-91ab-05fd3f2a903&title=)
在 [主从复制](https://frxcat.fun/database/MySQL/MySQL_Master_slave_replication/) 章节有详细说明。
#### 准备
| 主机 | 角色 | 用户名 | 密码 |
| --- | --- | --- | --- |
| 192.168.91.166 | master | root | 123456 |
| 192.168.91.167 | slave | root | 123456 |

备注：主从复制的搭建，可以参考前面文章中 [主从复制](https://frxcat.fun/database/MySQL/MySQL_Master_slave_replication/) 章节讲解的步骤操作。

- 结果验证
```sql
mysql> show replica status\G
*************************** 1. row ***************************
             Replica_IO_State: Waiting for source to send event
                  Source_Host: 192.168.91.166
                  Source_User: itcast01
                  Source_Port: 3306
                Connect_Retry: 60
              Source_Log_File: binlog.000001
          Read_Source_Log_Pos: 156
               Relay_Log_File: MySQL-Slave-relay-bin.000002
                Relay_Log_Pos: 321
        Relay_Source_Log_File: binlog.000001
           Replica_IO_Running: Yes
          Replica_SQL_Running: Yes
```

### 一主一从读写分离
MyCat控制后台数据库的读写分离和负载均衡由schema.xml文件datahost标签的balance属性控制。
#### schema.xml 配置
```sql
<!-- 配置逻辑库 -->
<schema name="ITCAST_RW" checkSQLschema="true" sqlMaxLimit="100" dataNode="dn7">
</schema>
<dataNode name="dn7" dataHost="dhost7" database="itcast01" />

<dataHost name="dhost7" maxCon="1000" minCon="10" balance="1" writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1" slaveThreshold="100">
	<heartbeat>select user()</heartbeat>
	<writeHost host="master1" url="jdbc:mysql://192.168.91.166:3306?useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" user="root" password="123456" >
	<readHost host="slave1" url="jdbc:mysql://192.168.91.167:3306?useSSL=false&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=utf8" user="root" password="123456" />
	</writeHost>
</dataHost>
```
上述配置的具体关联对应情况如下：
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676195116902-59870131-1624-449b-a3ae-14e88d54f69a.webp#averageHue=%23f9f7f0&clientId=u48ee23d5-f3ff-4&from=paste&id=uf8b3d8dd&originHeight=404&originWidth=1644&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u734fd0d3-f84b-4765-b96d-dce5a08afc7&title=)
writeHost代表的是写操作对应的数据库，readHost代表的是读操作对应的数据库。 所以我们要想实现读写分离，就得配置writeHost关联的是主库，readHost关联的是从库。
而仅仅配置好了writeHost以及readHost还不能完成读写分离，还需要配置一个非常重要的负责均衡的参数 balance，取值有4种，具体含义如下：

| 参数值 | 含义 |
| --- | --- |
| 0 | 不开启读写分离机制 , 所有读操作都发送到当前可用的writeHost上 |
| 1 | 全部的readHost 与 备用的writeHost 都参与select 语句的负载均衡（主要针对于双主双从模式） |
| 2 | 所有的读写操作都随机在writeHost , readHost上分发 |
| 3 | 所有的读请求随机分发到writeHost对应的readHost上执行, writeHost不负担读压力 |

所以，在一主一从模式的读写分离中，balance配置1或3都是可以完成读写分离的。

#### server.xml配置
配置root用户可以访问SHOPPING、ITCAST 以及 ITCAST_RW逻辑库。
```sql
<user name="root" defaultAccount="true">
	<property name="password">123456</property>
	<property name="schemas">SHOPPING,ITCAST,ITCAST_RW</property>
    
    <!-- 表级 DML 权限设置 -->
    <!--
    <privileges check="true">
		<schema name="DB01" dml="0110" >
			<table name="TB_ORDER" dml="1110"></table>
		</schema>
	</privileges>
-->
</user>
```
#### 测试
配置完毕MyCat后，重新启动MyCat。
```sql
bin/mycat stop
bin/mycat start
```
然后观察，在执行增删改操作时，对应的主库及从库的数据变化。 在执行查询操作时，检查主库及从库对应的数据变化。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676195192363-7b555ddf-72cb-4cbb-a6fe-63b4a0434161.webp#averageHue=%234d4740&clientId=u48ee23d5-f3ff-4&from=paste&id=ue021122b&originHeight=313&originWidth=1508&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u3edfe71f-d15b-4126-82f7-3d06784022b&title=)
在数据库写入一条数据，发现主从节点都增加一条数据，其实这条数据是从主节点写入的，因为数据是由主机点同步到从节点。
在数据库修改一条数据，发现主节点没有改变，从节点改变了，还是因为数据是由主机点同步到从节点。
在测试中，我们可以发现当主节点Master宕机之后，业务系统就只能够读，而不能写入数据了。
```sql
mysql> select * from tb_user;
+------+---------+-----+
| id   | name    | sex |
+------+---------+-----+
|    1 | Tom     | 1   |
|    2 | Trigger | 0   |
|    3 | Dawn    | 1   |
|    8 | It5     | 0   |
+------+---------+-----+
4 rows in set (0.01 sec)

mysql> insert into tb_user(id,name,sex) values(10,'It5',0);
ERROR:
No operations allowed after connection closed.
mysql>
```
那如何解决这个问题呢？这个时候我们就得通过另外一种主从复制结构来解决了，也就是我们接下来演示的双主双从。

### docker 搭建MySQL一主一从
[docker的安装](https://frxcat.fun/project-management/Docker/Docker_install/#%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95)
#### 搭建

1. 镜像下载
```sql
docker pull mysql:5.7
```

2. 新建主服务器容器实例:3307
```sql
docker run -p 3307:3306 --name mysql-master  -v /mydata/mysql-master/log:/var/log/mysql -v /mydata/mysql-master/data:/var/lib/mysql -v /mydata/mysql-master/conf:/etc/mysql -e MYSQL_ROOT_PASSWORD=root  -d mysql:5.7

[root@MySQL-Master ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
48c39e5628e8        mysql:5.7           "docker-entrypoint.s…"   10 minutes ago      Up 10 minutes       33060/tcp, 0.0.0.0:3307->3306/tcp   mysql-master
```

3. 进入/mydata/mysql-master/conf目录下新建my.cnf
```sql
[root@MySQL-Master mycat]# cd /mydata/mysql-master/conf
[root@MySQL-Master conf]# vim my.cnf
```
```sql
[mysqld]

## 设置server_id，同一局域网中需要唯一

server_id=101 

## 指定不需要同步的数据库名称

binlog-ignore-db=mysql  

## 开启二进制日志功能

log-bin=mall-mysql-bin  

## 设置二进制日志使用内存大小（事务）

binlog_cache_size=1M  

## 设置使用的二进制日志格式（mixed,statement,row）

binlog_format=mixed  

## 二进制日志过期清理时间。默认值为0，表示不自动清理。

expire_logs_days=7  

## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。

## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致

slave_skip_errors=1062
```

4. 修改完配置后重启master实例
```sql
docker restart mysql-master
```

5. 进入mysql-master容器
```sql
docker exec -it mysql-master /bin/bash
mysql -u root -p #登录
```

6. master容器实例内创建数据同步用户
```sql
CREATE USER 'slave'@'%' IDENTIFIED BY '123456';
GRANT REPLICATION SLAVE,REPLICATION CLIENT ON *.* TO 'slave'@'%';
```

7. 新建从服务器容器实例3308
```sql
docker run -p 3308:3306 --name mysql-slave -v /mydata/mysql-slave/log:/var/log/mysql -v /mydata/mysql-slave/data:/var/lib/mysql -v /mydata/mysql-slave/conf:/etc/mysql -e MYSQL_ROOT_PASSWORD=root  -d mysql:5.7
```
```sql
[root@MySQL-Master ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
4dba1c095b94        mysql:5.7           "docker-entrypoint.s…"   17 seconds ago      Up 15 seconds       33060/tcp, 0.0.0.0:3308->3306/tcp   mysql-slave
48c39e5628e8        mysql:5.7           "docker-entrypoint.s…"   10 minutes ago      Up 10 minutes       33060/tcp, 0.0.0.0:3307->3306/tcp   mysql-master
```

8. 进入/mydata/mysql-slave/conf目录下新建my.cnf
```sql
cd /mydata/mysql-slave/conf
vim my.cnf
```
```sql
[mysqld]

## 设置server_id，同一局域网中需要唯一

server_id=102

## 指定不需要同步的数据库名称

binlog-ignore-db=mysql  

## 开启二进制日志功能，以备Slave作为其它数据库实例的Master时使用

log-bin=mall-mysql-slave1-bin  

## 设置二进制日志使用内存大小（事务）

binlog_cache_size=1M  

## 设置使用的二进制日志格式（mixed,statement,row）

binlog_format=mixed  

## 二进制日志过期清理时间。默认值为0，表示不自动清理。

expire_logs_days=7  

## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。

## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致

slave_skip_errors=1062  

## relay_log配置中继日志

relay_log=mall-mysql-relay-bin  

## log_slave_updates表示slave将复制事件写进自己的二进制日志

log_slave_updates=1  

## slave设置为只读（具有super权限的用户除外）

read_only=1
```

9. 修改完配置后重启slave实例
```sql
docker restart mysql-slave
```

10. 进入mysql-slave容器
```sql
docker exec -it mysql-slave /bin/bash

mysql -u root -p
```

11. 在从数据库中配置主从复制

这条命令中的mall-mysql-bin.000001和617,要与master节点二进制日志坐标保持一致。
```sql
change master to master_host='192.168.91.166', master_user='slave', master_password='123456', master_port=3307, master_log_file='mall-mysql-bin.000001', master_log_pos=617, master_connect_retry=30;
```
| 参数 | 说明 |
| --- | --- |
| master_host | 主数据库的IP地址 |
| master_port | 主数据库的运行端口 |
| master_password | 在主数据库创建的用于同步数据的用户密码 |
| master_log_file | 指定从数据库要复制数据的日志文件，通过查看主数据的状态，获取File参数 |
| master_log_pos | 指定从数据库从哪个位置开始复制数据，通过查看主数据的状态，获取Position参数 |
| master_connect_retry | 连接失败重试的时间间隔，单位为秒 |

12. 在从数据库中查看主从同步状态
```sql
show slave status\G
```
```sql
mysql> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State:
                  Master_Host: 192.168.91.166
                  Master_User: slave
                  Master_Port: 3307
                Connect_Retry: 30
              Master_Log_File: mall-mysql-bin.000002
          Read_Master_Log_Pos: 371
               Relay_Log_File: mall-mysql-relay-bin.000002
                Relay_Log_Pos: 325
        Relay_Master_Log_File: mall-mysql-bin.000002
             Slave_IO_Running: No
            Slave_SQL_Running: No
```

13. 在从数据库中开启主从同步
```sql
start slave;
```

14. 再次从数据库中查看主从同步状态
```sql
mysql> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.91.166
                  Master_User: slave
                  Master_Port: 3307
                Connect_Retry: 30
              Master_Log_File: mall-mysql-bin.000002
          Read_Master_Log_Pos: 371
               Relay_Log_File: mall-mysql-relay-bin.000002
                Relay_Log_Pos: 325
        Relay_Master_Log_File: mall-mysql-bin.000002
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
```

#### 测试

1. 主机新建库-使用库-新建表-插入数据,ok
```sql
mysql> create database db01;
Query OK, 1 row affected (0.00 sec)

mysql> use db01;
Database changed
mysql> create table tb01(id int,name varchar(10));
Query OK, 0 rows affected (0.01 sec)

mysql> insert into tb01 values (1,'frx');
Query OK, 1 row affected (0.08 sec)
```

2. 从机使用库-查看记录,ok
```sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| db01               |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> use db01;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+----------------+
| Tables_in_db01 |
+----------------+
| tb01           |
+----------------+
1 row in set (0.00 sec)

mysql> select * from tb01 where id = 1;
+------+------+
| id   | name |
+------+------+
|    1 | frx  |
+------+------+
1 row in set (0.00 sec)
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1676195849585-91ce3efa-d624-4567-b0ff-341e39d849d7.png#averageHue=%23272625&clientId=u48ee23d5-f3ff-4&from=paste&height=217&id=ue39afde5&originHeight=433&originWidth=1507&originalType=binary&ratio=2&rotation=0&showTitle=false&size=346983&status=done&style=none&taskId=u83a05650-43f2-4544-9928-39c963f975a&title=&width=753.5)

## 存储引擎
### InnoDB
InnoDB 是一种兼顾高可靠性和高性能的通用存储引擎，在 MySQL 5.5 之后，InnoDB 是默认的 MySQL 引擎。
特点：

- DML 操作遵循 ACID 模型，支持**事务**
- **行级锁**，提高并发访问性能
- 支持**外键**约束，保证数据的完整性和正确性

文件：

- xxx.ibd: xxx代表表名，InnoDB 引擎的每张表都会对应这样一个表空间文件，存储该表的表结构（frm、sdi）、数据和索引。

系统变量：innodb_file_per_table，决定多张表共享一个表空间还是每张表对应一个表空间
知识点：
查看 Mysql 变量：
show variables like 'innodb_file_per_table';
从idb文件提取表结构数据：
（在cmd运行）
ibd2sdi xxx.ibd

#### InnoDB 逻辑存储结构
在 InnoDB 中，page 是磁盘操作的最小单元
![](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674805047089-ec7165dc-741b-4b1e-a670-98b2dd116ace.png#averageHue=%23a6cb7c&clientId=ue4843645-62ef-4&from=paste&id=u3868e126&originHeight=651&originWidth=1583&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ueca5f777-052b-4532-9b2f-cc786bb20f2&title=)

1. 表空间

表空间是InnoDB存储引擎逻辑结构的最高层， 如果用户启用了参数 innodb_file_per_table(在8.0版本中默认开启) ，则每张表都会有一个表空间（xxx.ibd），一个mysql实例可以对应多个表空间，用于存储记录、索引等数据。

1. 段

段，分为数据段（Leaf node segment）、索引段（Non-leaf node segment）、回滚段（Rollback segment），InnoDB是索引组织表，数据段就是B+树的叶子节点， 索引段即为B+树的非叶子节点。段用来管理多个Extent（区）。

1. 区

区，表空间的单元结构，每个区的大小为1M。 默认情况下， InnoDB存储引擎页大小为16K， 即一个区中一共有64个连续的页。

1. 页

页，是InnoDB 存储引擎磁盘管理的最小单元，每个页的大小默认为 16KB。为了保证页的连续性，InnoDB 存储引擎每次从磁盘申请 4-5 个区。

1. 行

行，InnoDB 存储引擎数据是按行进行存放的。
在行中，默认有两个隐藏字段：

- Trx_id：每次对某条记录进行改动时，都会把对应的事务id赋值给trx_id隐藏列。
- Roll_pointer：每次对某条引记录进行改动时，都会把旧的版本写入到undo日志中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息。
- 


**MySQL5.5 版本开始，默认使用InnoDB存储引擎，它擅长事务处理，具有崩溃恢复特性，在日常开发中使用非常广泛。下面是InnoDB架构图，左侧为内存结构，右侧为磁盘结构。**
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676105752898-5f683ec5-36c8-4a28-aca1-2043a5451673.webp#averageHue=%23efefed&clientId=u48ee23d5-f3ff-4&from=paste&id=u668528e4&originHeight=808&originWidth=1128&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u10b976b3-e307-4349-8b9c-6a4428a8f44&title=)
#### 内存架构
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676105753992-9f30760c-2fed-4a67-9620-58ff04da2045.webp#averageHue=%23ebeae7&clientId=u48ee23d5-f3ff-4&from=paste&id=u2091cd1e&originHeight=632&originWidth=290&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u3d47b9ac-9068-4dd8-a302-67b3bea2f63&title=)
在左侧的内存结构中，主要分为这么四大块儿： Buffer Pool、Change Buffer、Adaptive Hash Index、Log Buffer。 下来介绍一下这四个部分。
##### Buffer Pool
InnoDB存储引擎基于磁盘文件存储，访问物理硬盘和在内存中进行访问，速度相差很大，为了尽可能弥补这两者之间的I/O效率的差值，就需要把经常使用的数据加载到缓冲池中，避免每次访问都进行磁盘I/O。
在InnoDB的缓冲池中不仅缓存了索引页和数据页，还包含了undo页、插入缓存、自适应哈希索引以及InnoDB的锁信息等等。
缓冲池 Buffer Pool，是主内存中的一个区域，里面可以缓存磁盘上经常操作的真实数据，在执行增 删改查操作时，先操作缓冲池中的数据（若缓冲池没有数据，则从磁盘加载并缓存），然后再以一定频 率刷新到磁盘，从而减少磁盘IO，加快处理速度。
缓冲池以Page页为单位，底层采用链表数据结构管理Page。根据状态，将Page分为三种类型：

- free page：空闲page，未被使用。
- clean page：被使用page，数据没有被修改过。
- dirty page：脏页，被使用page，数据被修改过，也中数据与磁盘的数据产生了不一致。

在专用服务器上，通常将多达80％的物理内存分配给缓冲池 。参数设置： show variables like 'innodb_buffer_pool_size';
```sql
mysql> show variables like 'innodb_buffer_pool_size';
+-------------------------+-----------+
| Variable_name           | Value     |
+-------------------------+-----------+
| innodb_buffer_pool_size | 134217728 |
+-------------------------+-----------+
1 row in set (0.00 sec)
```

##### Change Buffer
Change Buffer，更改缓冲区（针对于非唯一二级索引页），在执行DML语句时，如果这些数据Page没有在Buffer Pool中，不会直接操作磁盘，而会将数据变更存在更改缓冲区 Change Buffer中，在未来数据被读取时，再将数据合并恢复到Buffer Pool中，再将合并后的数据刷新到磁盘中。
Change Buffer的意义是什么呢?
先来看一幅图，这个是二级索引的结构图：
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676106048668-3ac02453-ce3d-4511-ac5f-6053c53a2d06.webp#averageHue=%23f7f5ef&clientId=u48ee23d5-f3ff-4&from=paste&id=u9b06cf30&originHeight=529&originWidth=1319&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u9250c670-7203-4c4b-91ee-b5000807db9&title=)
与聚集索引不同，二级索引通常是非唯一的，并且以相对随机的顺序插入二级索引。同样，删除和更新可能会影响索引树中不相邻的二级索引页，如果每一次都操作磁盘，会造成大量的磁盘IO。有了ChangeBuffer之后，我们可以在缓冲池中进行合并处理，减少磁盘IO。
##### Adaptive Hash Index
自适应hash索引，用于优化对Buffer Pool数据的查询。MySQL的innoDB引擎中虽然没有直接支持hash索引，但是给我们提供了一个功能就是这个自适应hash索引。因为前面我们讲到过，hash索引在进行等值匹配时，一般性能是要高于B+树的，因为hash索引一般只需要一次IO即可，而B+树，可能需要几次匹配，所以hash索引的效率要高，但是hash索引又不适合做范围查询、模糊匹配等。
InnoDB存储引擎会监控对表上各索引页的查询，如果观察到在特定的条件下hash索引可以提升速度，则建立hash索引，称之为自适应hash索引。
**自适应哈希索引，无需人工干预，是系统根据情况自动完成**。
参数： adaptive_hash_index
##### Log Buffer
Log Buffer：日志缓冲区，用来保存要写入到磁盘中的log日志数据（redo log 、undo log），默认大小为 16MB，日志缓冲区的日志会定期刷新到磁盘中。如果需要更新、插入或删除许多行的事务，增加日志缓冲区的大小可以节省磁盘 I/O。
参数:
innodb_log_buffer_size：缓冲区大小
innodb_flush_log_at_trx_commit：日志刷新到磁盘时机，取值主要包含以下三个：
0:每秒将日志写入并刷新到磁盘一次。
1:日志在每次事务提交时写入并刷新到磁盘，默认值。
2:日志在每次事务提交后写入，并每秒刷新到磁盘一次。
```sql
mysql> show variables like 'innodb_flush_log_at_trx_commit';
+--------------------------------+-------+
| Variable_name                  | Value |
+--------------------------------+-------+
| innodb_flush_log_at_trx_commit | 1     |
+--------------------------------+-------+
1 row in set (0.00 sec)
```

#### 磁盘架构
接下来，再来看看InnoDB体系结构的右边部分，也就是磁盘结构：
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676107297298-596bb9dd-ecf2-4833-91df-d373c2566d68.webp#averageHue=%23f0f0ef&clientId=u48ee23d5-f3ff-4&from=paste&id=ue1ef9a3b&originHeight=744&originWidth=600&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u59392301-34a7-4d42-ba7e-9635fedc6f0&title=)
##### System Tablespace
系统表空间是更改缓冲区的存储区域。如果表是在系统表空间而不是每个表文件或通用表空间中创建的，它也可能包含表和索引数据。(在MySQL5.x版本中还包含InnoDB数据字典、undolog等)
系统表空间，默认的文件名叫 ibdata1
参数：innodb_data_file_path
```sql
mysql> show variables like 'innodb_data_file_path';
+-----------------------+------------------------+
| Variable_name         | Value                  |
+-----------------------+------------------------+
| innodb_data_file_path | ibdata1:12M:autoextend |
+-----------------------+------------------------+
1 row in set (0.00 sec)
```

##### File-Per-Table Tablespaces
如果开启了innodb_file_per_table开关 ，则每个表的文件表空间包含单个InnoDB表的数据和索引 ，并存储在文件系统上的单个数据文件中。
开关参数：innodb_file_per_table，该参数默认开启。
```sql
mysql> show variables like 'innodb_file_per_table';
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_file_per_table | ON    |
+-----------------------+-------+
1 row in set (0.00 sec)
```

那也就是说，我们每创建一个表，都会产生一个表空间文件，如图：
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676107415036-6dc263a5-87f7-40b6-93de-b81e11b0e78c.webp#averageHue=%23403e35&clientId=u48ee23d5-f3ff-4&from=paste&id=ubad00938&originHeight=98&originWidth=1182&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=uc51f4303-ab30-44e9-8f85-7f7c572bb12&title=)

##### General Tablespaces
通用表空间，需要通过 CREATE TABLESPACE 语法创建通用表空间，在创建表时，可以指定该表空间。
A. 创建表空间
```sql
CREATE TABLESPACE ts_name ADD DATAFILE 'file_name' ENGINE = engine_name;

mysql> CREATE TABLESPACE ts_itheima ADD DATAFILE 'myitheima.ibd' ENGINE = innodb;
Query OK, 0 rows affected (0.00 sec)
```
 B. 创建表时指定表空间
```sql
CREATE TABLE xxx ... TABLESPACE ts_name;

mysql> create table a(id int primary key auto_increment,name varchar(10)) engine=innodb tablespace ts_itheima;
Query OK, 0 rows affected (0.01 sec)
```
##### Undo Tablespaces
撤销表空间，MySQL实例在初始化时会自动创建两个默认的undo表空间（初始大小16M），用于存储 undo log日志。
##### Temporary Tablespaces
InnoDB 使用会话临时表空间和全局临时表空间。存储用户创建的临时表等数据。
##### Doublewrite Buffer Files
双写缓冲区，innoDB引擎将数据页从Buffer Pool刷新到磁盘前，先将数据页写入双写缓冲区文件中，便于系统异常时恢复数据。
##### Redo Log
重做日志，是用来实现事务的持久性。该日志文件由两部分组成：重做日志缓冲（redo log buffer）以及重做日志文件（redo log）,前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都会存到该日志中, 用于在刷新脏页到磁盘时,发生错误时, 进行数据恢复使用。
以循环方式写入重做日志文件，涉及两个文件：

前面我们介绍了InnoDB的内存结构，以及磁盘结构，那么内存中我们所更新的数据，又是如何到磁盘中的呢？ 此时，就涉及到一组后台线程，接下来，就来介绍一些InnoDB中涉及到的后台线程。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676107980481-60206498-e0ad-4df5-a235-d7a8f3ae1f98.webp#averageHue=%23ebe9e7&clientId=u48ee23d5-f3ff-4&from=paste&id=u28575fbb&originHeight=752&originWidth=1055&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u68a4bc05-65a2-495b-977c-442cf4470eb&title=)
#### 后台线程
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1676108054582-35da2692-b618-48e2-b27e-c6341d534e09.png#averageHue=%23f9f9f9&clientId=u48ee23d5-f3ff-4&from=paste&height=329&id=uf3f362b0&originHeight=657&originWidth=704&originalType=binary&ratio=2&rotation=0&showTitle=false&size=145058&status=done&style=none&taskId=ud254ca0c-f8ba-4954-b689-0ff4addb474&title=&width=352)
在InnoDB的后台线程中，分为4类，分别是：Master Thread 、IO Thread、Purge Thread、Page Cleaner Thread。

脏页－linux内核中的概念，因为硬盘的读写速度远赶不上内存的速度，系统就把读写比较频繁的数据事先放到内存中，以提高读写速度，这就叫高速缓存，linux是以页作为高速缓存的单位，当进程修改了高速缓存里的数据时，该页就被内核标记为脏页，内核将会在合适的时间把脏页的数据写到磁盘中去，以保持高速缓存中的数据和磁盘中的数据是一致的。

##### Master Thread
核心后台线程，负责调度其他线程，还负责将缓冲池中的数据异步刷新到磁盘中, 保持数据的一致性，还包括脏页的刷新、合并插入缓存、undo页的回收 。
##### IO Thread
在InnoDB存储引擎中大量使用了AIO来处理IO请求, 这样可以极大地提高数据库的性能，而IOThread主要负责这些IO请求的回调。

| 线程类型 | 默认个数 | 职责 |
| --- | --- | --- |
| Read thread | 4 | 负责读操作 |
| Write thread | 4 | 负责写操作 |
| Log thread | 1 | 负责将日志缓冲区刷新到磁盘 |
| Insert buffer thread | 1 | 负责将写缓冲区内容刷新到磁盘 |

我们可以通过以下的这条指令，查看到InnoDB的状态信息，其中就包含IO Thread信息。
```sql
show engine innodb status;
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1676108462421-92dda70d-645a-47db-b40b-a6c4677eac2f.png#averageHue=%23222121&clientId=u48ee23d5-f3ff-4&from=paste&height=427&id=ubfda183a&originHeight=853&originWidth=1518&originalType=binary&ratio=2&rotation=0&showTitle=false&size=663392&status=done&style=none&taskId=u6f13b6b4-ebcc-451b-a7d9-6eb4c8f532f&title=&width=759)
##### Purge Thread
主要用于回收事务已经提交了的undo log，在事务提交之后，undo log可能不用了，就用它来回收。
##### Page Cleaner Thread
协助 Master Thread 刷新脏页到磁盘的线程，它可以减轻 Master Thread 的工作压力，减少阻塞。

#### 事务原理
##### 事务基础

1. 事务

事务 是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

1. 特性
- 原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败。
- 一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态。
- 隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。
- 持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

而对于这四大特性，实际上分为两个部分。 其中的原子性、一致性、持久化，实际上是由InnoDB中的两份日志来保证的，一份是redo log日志，一份是undo log日志。 而持久性是通过数据库的锁，加上MVCC来保证的。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1676110726303-e6bf4828-cfa4-41a3-88fb-9c501ae95e66.png#averageHue=%23fcfbfa&clientId=u48ee23d5-f3ff-4&from=paste&height=238&id=u1a2946ae&originHeight=475&originWidth=1281&originalType=binary&ratio=2&rotation=0&showTitle=false&size=224282&status=done&style=none&taskId=u96748ee3-f6e9-4104-9979-f7371332312&title=&width=640.5)
我们在讲解事务原理的时候，主要就是来研究一下redolog，undolog以及MVCC。

##### Redo Log
重做日志，记录的是事务提交时数据页的物理修改，是用来实现事务的持久性。
该日志文件由两部分组成：重做日志缓冲（redo log buffer）以及重做日志文件（redo log file）,前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都存到该日志文件中, 用于在刷新脏页到磁盘,发生错误时, 进行数据恢复使用。
如果没有redolog，可能会存在什么问题的？ 我们一起来分析一下。
我们知道，在InnoDB引擎中的内存结构中，主要的内存区域就是缓冲池，在缓冲池中缓存了很多的数据页。 当我们在一个事务中，执行多个增删改的操作时，InnoDB引擎会先操作缓冲池中的数据，如果缓冲区没有对应的数据，会通过后台线程将磁盘中的数据加载出来，存放在缓冲区中，然后将缓冲池中的数据修改，修改后的数据页我们称为脏页。 而脏页则会在一定的时机，通过后台线程刷新到磁盘中，从而保证缓冲区与磁盘的数据一致。 而缓冲区的脏页数据并不是实时刷新的，而是一段时间之后将缓冲区的数据刷新到磁盘中，假如刷新到磁盘的过程出错了，而提示给用户事务提交成功，而数据却没有持久化下来，这就出现问题了，没有保证事务的持久性。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1676110773420-c0490c4c-8430-4c3a-b17f-8c78709da669.png#averageHue=%23faf0e7&clientId=u48ee23d5-f3ff-4&from=paste&height=253&id=uc0fb5640&originHeight=505&originWidth=1319&originalType=binary&ratio=2&rotation=0&showTitle=false&size=134359&status=done&style=none&taskId=uac0c38b4-918d-4e7e-b2d0-638068d9502&title=&width=659.5)
那么，如何解决上述的问题呢？ 在InnoDB中提供了一份日志 redo log，接下来我们再来分析一下，通过redolog如何解决这个问题。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676110793203-427cd2c2-7223-4a7c-9cba-8b84552c50b9.webp#averageHue=%23faefe6&clientId=u48ee23d5-f3ff-4&from=paste&id=u351ca9a7&originHeight=496&originWidth=1299&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u7b8a37c9-896b-40e5-8670-c72f44864a2&title=)
有了redolog之后，当对缓冲区的数据进行增删改之后，会首先将操作的数据页的变化，记录在redo log buffer中。在事务提交时，会将redo log buffer中的数据刷新到redo log磁盘文件中。过一段时间之后，如果刷新缓冲区的脏页到磁盘时，**发生错误，此时就可以借助于redo log进行数据恢复，这样就保证了事务的持久性。** 而如果脏页成功刷新到磁盘 或 或者涉及到的数据已经落盘，此时redolog就没有作用了，就可以删除了，所以存在的两个redolog文件是循环写的。
那为什么每一次提交事务，要刷新 redo log 到磁盘中呢，而不是直接将buffer pool中的脏页刷新到磁盘呢 ?
因为在业务操作中，**我们操作数据一般都是随机读写磁盘的，而不是顺序读写磁盘。先把脏页记录在redo log buffer，然后刷新到redo log磁盘文件中，当发生错误，此时就可以借助于redo log进行数据恢复，这样就保证了事务的持久性。而且 redo log是日志文件，是追加的，因此日志文件是顺序写入磁盘的。顺序IO的效率，要远大于随机IO。** 这种先写日志的方式，称之为 WAL（Write-Ahead Logging）。

##### undo log
回滚日志，用于记录数据被修改前的信息 , 作用包含两个 : 提供回滚(保证事务的原子性) 和MVCC(多版本并发控制) 。
undo log和redo log记录物理日志不一样，它是逻辑日志。可以认为当delete一条记录时，undolog中会记录一条对应的insert记录，反之亦然，当update一条记录时，它记录一条对应相反的update记录。当执行rollback时，就可以从undo log中的逻辑记录读取到相应的内容并进行回滚。
Undo log销毁：undo log在事务执行时产生，事务提交时，并不会立即删除undo log，因为这些日志可能还用于MVCC。
Undo log存储：undo log采用段的方式进行管理和记录，存放在前面介绍的 rollback segment回滚段中，内部包含1024个undo log segment。
#### 
MVCC
##### 当前读
读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。对于我们日常的操作，如：select ... lock in share mode(共享锁)，select ...for update、update、insert、delete(排他锁)都是一种当前读。
测试:
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676125162718-b1f31e63-8577-485d-b3a1-2f4c60ee8f48.webp#averageHue=%23262524&clientId=u48ee23d5-f3ff-4&from=paste&id=ua527f83d&originHeight=650&originWidth=1510&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u7dcf3f6d-6943-4850-81b0-34db2803d1b&title=)
在测试中我们可以看到，即使是在默认的RR隔离级别下，事务A中依然可以读取到事务B最新提交的内容，因为在查询语句后面加上了 lock in share mode 共享锁，此时是当前读操作。当然，当我们加排他锁的时候，也是当前读操作。
##### 快照读
简单的select（不加锁）就是快照读，快照读，读取的是记录数据的可见版本，有可能是历史数据，不加锁，是非阻塞读。

- Read Committed：每次select，都生成一个快照读。
- Repeatable Read：开启事务后第一个select语句才是快照读的地方。
- Serializable：快照读会退化为当前读。

测试:
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676125223424-9ddd43b2-2983-455a-b2fd-a9b7d937fdf8.webp#averageHue=%23252524&clientId=u48ee23d5-f3ff-4&from=paste&id=u3fb62fd8&originHeight=643&originWidth=1508&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u488a2dba-27d2-4bcb-b57c-4eb727f78f0&title=)
在测试中,我们看到即使事务B提交了数据,事务A中也查询不到。 原因就是因为普通的select是快照读，而在当前默认的RR隔离级别下，开启事务后第一个select语句才是快照读的地方，后面执行相同的select语句都是从快照中获取数据，可能不是当前的最新数据，这样也就保证了可重复读。

##### MVCC
全称 Multi-Version Concurrency Control，多版本并发控制。指维护一个数据的多个版本，使得读写操作没有冲突，快照读为MySQL实现MVCC提供了一个非阻塞读功能。MVCC的具体实现，还需要依赖于数据库记录中的三个隐式字段、undo log日志、readView。
接下来，我们再来介绍一下InnoDB引擎的表中涉及到的隐藏字段 、undolog 以及 readview，从而来介绍一下MVCC的原理。

###### 隐藏字段
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676125548129-9767eb51-83ca-4919-990f-96e58163b841.webp#averageHue=%23e2e0d9&clientId=u48ee23d5-f3ff-4&from=paste&id=u8f47bd8b&originHeight=166&originWidth=328&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=ue828bab0-14f9-4b99-832a-e0ae40bffbf&title=)
当我们创建了上面的这张表，我们在查看表结构的时候，就可以显式的看到这三个字段。 实际上除了这三个字段以外，InnoDB还会自动的给我们添加三个隐藏字段及其含义分别是：

| 隐藏字段 | 含义 |
| --- | --- |
| DB_TRX_ID | 最近修改事务ID，记录插入这条记录或最后一次修改该记录的事务ID。 |
| DB_ROLL_PTR | 回滚指针，指向这条记录的上一个版本，用于配合undo log，指向上一个版本。 |
| DB_ROW_ID | 隐藏主键，如果表结构没有指定主键，将会生成该隐藏字段。 |

而上述的前两个字段是肯定会添加的， 是否添加最后一个字段DB_ROW_ID，得看当前表有没有主键，如果有主键，则不会添加该隐藏字段。

**查看有主键的表 stu**
进入服务器中的 /var/lib/mysql/MySQL_Advanced/ , 查看stu的表结构信息, 通过如下指令:
```sql
ibd2sdi stu.ibd
```
查看到的表结构信息中，有一栏 columns，在其中我们会看到处理我们建表时指定的字段以外，还有额外的两个字段 分别是：DB_TRX_ID 、 DB_ROLL_PTR ，因为该表有主键，所以没有DB_ROW_ID隐藏字段。

**查看没有主键的表 employee**
建表语句：
```sql
create table employee (id int , name varchar(10));
```
此时，我们再通过以下指令来查看表结构及其其中的字段信息：
```sql
ibd2sdi employee.ibd
```
查看到的表结构信息中，有一栏 columns，在其中我们会看到处理我们建表时指定的字段以外，还有额外的三个字段 分别是：DB_TRX_ID 、 DB_ROLL_PTR 、DB_ROW_ID，因为employee表是没有指定主键的。
```sql
            {
                "name": "DB_ROW_ID",
                "type": 10,
                "is_nullable": false,
                "is_zerofill": false,
                "is_unsigned": false,
                "is_auto_increment": false,
                "is_virtual": false,
                "hidden": 2,
                "ordinal_position": 3,
                "char_length": 6,
                "numeric_precision": 0,
                "numeric_scale": 0,
                "numeric_scale_null": true,
                "datetime_precision": 0,
                "datetime_precision_null": 1,
                "has_no_default": false,
                "default_value_null": true,
                "srs_id_null": true,
                "srs_id": 0,
                "default_value": "",
                "default_value_utf8_null": true,
                "default_value_utf8": "",
                "default_option": "",
                "update_option": "",
                "comment": "",
                "generation_expression": "",
                "generation_expression_utf8": "",
                "options": "",
                "se_private_data": "table_id=1076;",
                "engine_attribute": "",
                "secondary_engine_attribute": "",
                "column_key": 1,
                "column_type_utf8": "",
                "elements": [],
                "collation_id": 63,
                "is_explicit_collation": false
            },
            {
                "name": "DB_TRX_ID",
                "type": 10,
                "is_nullable": false,
                "is_zerofill": false,
                "is_unsigned": false,
                "is_auto_increment": false,
                "is_virtual": false,
                "hidden": 2,
                "ordinal_position": 4,
                "char_length": 6,
                "numeric_precision": 0,
                "numeric_scale": 0,
                "numeric_scale_null": true,
                "datetime_precision": 0,
                "datetime_precision_null": 1,
                "has_no_default": false,
                "default_value_null": true,
                "srs_id_null": true,
                "srs_id": 0,
                "default_value": "",
                "default_value_utf8_null": true,
                "default_value_utf8": "",
                "default_option": "",
                "update_option": "",
                "comment": "",
                "generation_expression": "",
                "generation_expression_utf8": "",
                "options": "",
                "se_private_data": "table_id=1076;",
                "engine_attribute": "",
                "secondary_engine_attribute": "",
                "column_key": 1,
                "column_type_utf8": "",
                "elements": [],
                "collation_id": 63,
                "is_explicit_collation": false
            },
            {
                "name": "DB_ROLL_PTR",
                "type": 9,
                "is_nullable": false,
                "is_zerofill": false,
                "is_unsigned": false,
                "is_auto_increment": false,
                "is_virtual": false,
                "hidden": 2,
                "ordinal_position": 5,
                "char_length": 7,
                "numeric_precision": 0,
                "numeric_scale": 0,
                "numeric_scale_null": true,
                "datetime_precision": 0,
                "datetime_precision_null": 1,
                "has_no_default": false,
                "default_value_null": true,
                "srs_id_null": true,
                "srs_id": 0,
                "default_value": "",
                "default_value_utf8_null": true,
                "default_value_utf8": "",
                "default_option": "",
                "update_option": "",
                "comment": "",
                "generation_expression": "",
                "generation_expression_utf8": "",
                "options": "",
                "se_private_data": "table_id=1076;",
                "engine_attribute": "",
                "secondary_engine_attribute": "",
                "column_key": 1,
                "column_type_utf8": "",
                "elements": [],
                "collation_id": 63,
                "is_explicit_collation": false
            }
        ],
```

###### undo log
回滚日志，在insert、update、delete的时候产生的便于数据回滚的日志。
当insert的时候，产生的undo log日志只在回滚时需要，在事务提交后，可被立即删除。
而update、delete的时候，产生的undo log日志不仅在回滚时需要，在快照读时也需要，不会立即被删除

###### 版本链
有一张表原始数据为：
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676126183824-a0a99b11-70f1-4f46-8b0b-335a7ddd41f8.webp#averageHue=%23efefeb&clientId=u48ee23d5-f3ff-4&from=paste&id=ua1742366&originHeight=84&originWidth=522&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=uef8810f6-750a-4568-8e3e-51f35a2d790&title=)
DB_TRX_ID : 代表最近修改事务ID，记录插入这条记录或最后一次修改该记录的事务ID，是自增的。
DB_ROLL_PTR： 由于这条数据是才插入的，没有被更新过，所以该字段值为null。
然后，有四个并发事务同时在访问这张表。
A. 第一步
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676126185878-5bfc5c9e-7315-41a5-b2cf-7938a86f9d3d.webp#averageHue=%23f9f8f8&clientId=u48ee23d5-f3ff-4&from=paste&id=u9b4ddcaa&originHeight=331&originWidth=826&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u52196ca0-ea69-40c9-9dd4-c499c89e949&title=)
当事务2执行第一条修改语句时，会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676126185216-f1a7d817-57ba-439a-9bbf-0434edbb7d45.webp#averageHue=%23f5f0e5&clientId=u48ee23d5-f3ff-4&from=paste&id=u7195c4e6&originHeight=416&originWidth=1339&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=udc07f4f7-eab8-4500-9ffa-88a9842b782&title=)
B.第二步
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676126187980-063ab22e-802a-4d56-a528-f47c5da58b75.webp#averageHue=%23f9f6f6&clientId=u48ee23d5-f3ff-4&from=paste&id=u1f0996fe&originHeight=303&originWidth=731&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=ua2eaa083-a6db-4bde-9b18-c2fcc2c260e&title=)
当事务3执行第一条修改语句时，也会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676126185032-c7899f5f-e896-4a1b-8eea-2a51675572ca.webp#averageHue=%23eff4e2&clientId=u48ee23d5-f3ff-4&from=paste&id=uaf70b046&originHeight=380&originWidth=1141&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=udd7235fc-d952-491f-8632-425d9dd7733&title=)
C. 第三步
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676126184941-8a6a10ff-b240-4897-ae03-bb826a0a3fda.webp#averageHue=%23f9f5f4&clientId=u48ee23d5-f3ff-4&from=paste&id=uf19a37f0&originHeight=326&originWidth=806&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u5ddbbdea-7dbb-4441-bc60-fc3f4d2e720&title=)
当事务4执行第一条修改语句时，也会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676126186874-846986c3-6e11-4eb9-b09c-a0d8ca72bf76.webp#averageHue=%23f6e9d9&clientId=u48ee23d5-f3ff-4&from=paste&id=ue00b0d6c&originHeight=406&originWidth=1279&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=uf05df359-6e62-44e6-b4d4-7c46672fac4&title=)
最终我们发现，不同事务或相同事务对同一条记录进行修改，会导致该记录的undolog生成一条记录版本链表，链表的头部是最新的旧记录，链表尾部是最早的旧记录。
###### 
readview
ReadView（读视图）是 快照读 SQL执行时MVCC提取数据的依据，记录并维护系统当前活跃的事务（未提交的）id。
ReadView中包含了四个核心字段：

| 字段 | 含义 |
| --- | --- |
| m_ids | 当前活跃的事务ID集合 |
| min_trx_id | 最小活跃事务ID |
| max_trx_id | 预分配事务ID，当前最大事务ID+1（因为事务ID是自增的） |
| creator_trx_id | ReadView创建者的事务ID |

而在readview中就规定了版本链数据的访问规则：
trx_id 代表当前undolog版本链对应事务ID。

| 条件 | 是否可以访问 | 说明 |
| --- | --- | --- |
| trx_id == creator_trx_id | 可以访问该版本 | 成立，说明数据是当前这个事务更改的。 |
| trx_id < min_trx_id | 可以访问该版本 | 成立，说明数据已经提交了。 |
| trx_id > max_trx_id | 不可以访问该版本 | 成立，说明该事务是在ReadView生成后才开启。 |
| min_trx_id <= trx_id <= max_trx_id | 如果trx_id不在m_ids中，是可以访问该版本的 | 成立，说明数据已经提交。 |

不同的隔离级别，生成ReadView的时机不同：

- READ COMMITTED ：在事务中每一次执行快照读时生成ReadView。
- REPEATABLE READ：仅在事务中第一次执行快照读时生成ReadView，后续复用该ReadView。

**RC隔离级别**
RC隔离级别下，在事务中**每一次**执行快照读时生成ReadView。
我们就来分析事务5中，两次快照读读取数据，是如何获取数据的?
在事务5中，查询了两次id为30的记录，由于隔离级别为Read Committed，所以每一次进行快照读都会生成一个ReadView，那么两次生成的ReadView如下。
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676127167211-9f6c0839-6f6e-4e5f-8e74-97ef16b61f8e.webp#averageHue=%23f1f1f1&clientId=u48ee23d5-f3ff-4&from=paste&id=uff663d61&originHeight=409&originWidth=1426&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u0b4dae82-9d2c-400b-aa23-acfdd626e4f&title=)
那么这两次快照读在获取数据时，就需要根据所生成的ReadView以及ReadView的版本链访问规则，到undolog版本链中匹配数据，最终决定此次快照读返回的数据。
A. 先来看第一次快照读具体的读取过程：
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676127441893-dd84beb4-ada5-4e0a-a930-4353bb972327.webp#averageHue=%23f2f1ee&clientId=u48ee23d5-f3ff-4&from=paste&id=u54a89d63&originHeight=751&originWidth=1483&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u8a0b3a4e-e1c0-421f-a3e5-d838ceb4569&title=)
在进行匹配时，会从undo log的版本链，从上到下进行挨个匹配：

- 先匹配![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676127442316-7038ca43-f252-4a2c-9016-3cc09adc3a33.webp#averageHue=%23f6f3ef&clientId=u48ee23d5-f3ff-4&from=paste&id=u5eaf5296&originHeight=167&originWidth=895&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u2ec56926-f20b-44ab-ba22-e144d1c732c&title=)这条记录，这条记录对应的trx_id为4，也就是将4带入右侧的匹配规则中。 ①不满足 ②不满足 ③不满足 ④也不满足 ，都不满足，则继续匹配undo log版本链的下一条。
- 再匹配第二条![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676127441058-1c98f049-659d-4070-85c3-e3fee2668079.webp#averageHue=%23f3d4b5&clientId=u48ee23d5-f3ff-4&from=paste&id=ucf5550ad&originHeight=84&originWidth=1055&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u99bfe2ff-3b37-4cfb-aea6-efab91b44b4&title=)，这条记录对应的trx_id为3，也就是将3带入右侧的匹配规则中。①不满足 ②不满足 ③不满足 ④也不满足 ，都不满足，则继续匹配undo log版本链的下一条。
- 再匹配第三条![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676127442856-5111caef-8d94-4f79-8eb1-65cb2bb20b81.webp#averageHue=%23f3d3b6&clientId=u48ee23d5-f3ff-4&from=paste&id=u8ed26927&originHeight=76&originWidth=1034&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u0de7c966-ee04-4b82-9b1d-8fde54ece40&title=)，这条记录对应的trx_id为2，也就是将2带入右侧的匹配规则中。①不满足 ②满足 终止匹配，此次快照读，返回的数据就是版本链中记录的这条数据。

B. 再来看第二次快照读具体的读取过程:
![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676127441642-ae40e7e5-77dd-4092-84e3-f555d54d40ff.webp#averageHue=%23f3f1ef&clientId=u48ee23d5-f3ff-4&from=paste&id=ue9e8b664&originHeight=793&originWidth=1510&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u3bec9dd9-f561-4e93-8b04-b90cc982ceb&title=)
在进行匹配时，会从undo log的版本链，从上到下进行挨个匹配：

- 先匹配![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676127443236-38454886-9cae-4079-ada5-15668fea5fb0.webp#averageHue=%23f4f1ed&clientId=u48ee23d5-f3ff-4&from=paste&id=u836119c5&originHeight=157&originWidth=887&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=ubc82cfa9-3fb7-46f5-89ab-eb64f333610&title=)这条记录，这条记录对应的trx_id为4，也就是将4带入右侧的匹配规则中。 ①不满足 ②不满足 ③不满足 ④也不满足 ，都不满足，则继续匹配undo log版本链的下一条。
- 再匹配第二条![](https://cdn.nlark.com/yuque/0/2023/webp/26428688/1676127445321-9264b92e-71cd-40aa-8ebc-a3bbc2f6945b.webp#averageHue=%23f4d3b4&clientId=u48ee23d5-f3ff-4&from=paste&id=ud36733b2&originHeight=77&originWidth=1054&originalType=url&ratio=2&rotation=0&showTitle=false&status=done&style=none&taskId=u4ffdf58d-1aab-4c7c-8ef6-d7f2c1c384a&title=)，这条记录对应的trx_id为3，也就是将3带入右侧的匹配规则中。①不满足 ②满足 。终止匹配，此次快照读，返回的数据就是版本链中记录的这条数据。

**RR隔离级别**
RR隔离级别下，仅在事务中第一次执行快照读时生成ReadView，后续复用该ReadView。 而RR 是可重复读，在一个事务中，执行两次相同的select语句，查询到的结果是一样的。
那MySQL是如何做到可重复读的呢? 我们简单分析一下就知道了
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1676127685065-ac0e190b-529c-4795-83c0-c660b89d6b17.png#averageHue=%23f1f1f1&clientId=u48ee23d5-f3ff-4&from=paste&height=223&id=uf3d44ba2&originHeight=445&originWidth=1402&originalType=binary&ratio=2&rotation=0&showTitle=false&size=230738&status=done&style=none&taskId=u54cf9819-fc9e-4ad6-a747-052ef362b52&title=&width=701)
我们看到，在RR隔离级别下，只是在事务中**第一次**快照读时生成ReadView，后续都是复用该ReadView，那么既然ReadView都一样， ReadView的版本链匹配规则也一样， 那么最终快照读返回的结果也是一样的。
所以呢，MVCC的实现原理就是通过 InnoDB表的隐藏字段、UndoLog 版本链、ReadView来实现的。而MVCC + 锁，则实现了事务的隔离性。 而一致性则是由redolog 与 undolog保证。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1676127697055-83de4423-5664-4a88-aedd-10e53438dc73.png#averageHue=%23fbf4f3&clientId=u48ee23d5-f3ff-4&from=paste&height=274&id=u202b288b&originHeight=547&originWidth=1330&originalType=binary&ratio=2&rotation=0&showTitle=false&size=234922&status=done&style=none&taskId=u1f019edc-b7fc-4944-94c7-33832a21d22&title=&width=665)

### MyISAM
MyISAM 是 MySQL 早期的默认存储引擎。
特点：

- 不支持事务，不支持外键
- 支持表锁，不支持行锁
- 访问速度快

文件：

- xxx.sdi: 存储表结构信息
- xxx.MYD: 存储数据
- xxx.MYI: 存储索引
### Memory
Memory 引擎的表数据是存储在内存中的，受硬件问题、断电问题的影响，只能将这些表作为临时表或缓存使用。
特点：

- 存放在内存中，速度快
- hash索引（默认）

文件：

- xxx.sdi: 存储表结构信息
### 存储引擎特点
| **特点** | **InnoDB** | **MyISAM** | **Memory** |
| --- | --- | --- | --- |
| 存储限制 | 64TB | 有 | 有 |
| 事务安全 | 支持 | - | - |
| 锁机制 | 行锁 | 表锁 | 表锁 |
| B+tree索引 | 支持 | 支持 | 支持 |
| Hash索引 | - | - | 支持 |
| 全文索引 | 支持（5.6版本之后） | 支持 | - |
| 空间使用 | 高 | 低 | N/A |
| 内存使用 | 高 | 低 | 中等 |
| 批量插入速度 | 低 | 高 | 高 |
| 支持外键 | 支持 | - | - |


### 存储引擎的选择
在选择存储引擎时，应该根据应用系统的特点选择合适的存储引擎。对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合。

- InnoDB: 如果应用对事物的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，还包含很多的更新、删除操作，则 InnoDB 是比较合适的选择
- MyISAM: 如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不高，那这个存储引擎是非常合适的。
- Memory: 将所有数据保存在内存中，访问速度快，通常用于临时表及缓存。Memory 的缺陷是对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性

电商中的足迹和评论适合使用 MyISAM 引擎，缓存适合使用 Memory 引擎。


## 性能分析
### 查看执行频次
查看当前数据库的 INSERT, UPDATE, DELETE, SELECT 访问频次：
SHOW GLOBAL STATUS LIKE 'Com_______'; 或者 SHOW SESSION STATUS LIKE 'Com_______';
例：show global status like 'Com_______'
### 慢查询日志
慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有SQL语句的日志。
MySQL的慢查询日志默认没有开启，需要在MySQL的配置文件（/etc/my.cnf）中配置如下信息：
```
# 开启慢查询日志开关
slow_query_log=1
# 设置慢查询日志的时间为2秒，SQL语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
long_query_time=2
```
更改后记得重启MySQL服务，日志文件位置：/var/lib/mysql/localhost-slow.log
查看慢查询日志开关状态：
show variables like 'slow_query_log';
### profile
show profile 能在做SQL优化时帮我们了解时间都耗费在哪里。通过 have_profiling 参数，能看到当前 MySQL 是否支持 profile 操作：
SELECT @@have_profiling;
profiling 默认关闭，可以通过set语句在session/global级别开启 profiling：
SELECT @@profiling;
SET profiling = 1;
查看所有语句的耗时：
show profiles;
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674812097098-ba2cf7f5-53ae-46be-99cd-01d1b9aa9235.png#averageHue=%232e4b5e&clientId=ue4843645-62ef-4&from=paste&height=237&id=tD7x4&originHeight=474&originWidth=732&originalType=binary&ratio=1&rotation=0&showTitle=false&size=357442&status=done&style=none&taskId=uebf04577-a310-4d8b-bf7a-76af05dd678&title=&width=366)
查看指定query_id的SQL语句各个阶段的耗时：
show profile for query query_id;
查看指定query_id的SQL语句CPU的使用情况
show profile cpu for query query_id;
### explain
EXPLAIN 或者 DESC 命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中表如何连接和连接的顺序。
语法：
```sql
-- 直接在select语句之前加上关键字 explain / desc
EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件;
```


## SQL 优化
### 插入数据
普通插入：

1. 采用批量插入（一次插入的数据不建议超过1000条）
2. 主键顺序插入

大批量插入：
如果一次性需要插入大批量数据，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的load指令插入。
```sql
# 客户端连接服务端时，加上参数 --local-infile（这一行在bash/cmd界面输入）
mysql --local-infile -u root -p
# 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
set global local_infile = 1;
select @@local_infile;
# 执行load指令将准备好的数据，加载到表结构中
load data local infile '/root/data.sql' into table 'tb_user' fields terminated by ',' lines terminated by '\n';
```

### 主键优化
数据组织方式：在InnoDB存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表（Index organized table, IOT）
![](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674807435307-622055e8-5b88-404f-9925-99c258783801.png#averageHue=%23f0eae4&clientId=ue4843645-62ef-4&from=paste&id=hjBfQ&originHeight=476&originWidth=1513&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub22fc03d-a34f-488e-a76e-9c2c45c7851&title=)

**页分裂演示**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674913155196-9073c429-1822-4360-8f3b-f14c61aab987.png#averageHue=%23fdf7f1&clientId=u11d005e6-6de2-4&from=paste&height=137&id=u459a8483&originHeight=274&originWidth=1398&originalType=binary&ratio=1&rotation=0&showTitle=false&size=138597&status=done&style=none&taskId=u874e2315-76ec-4fc4-a023-62b4428d7a2&title=&width=699)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674913983983-ab134506-b5c1-4192-8dd4-590c6e167377.png#averageHue=%23fcf8f4&clientId=u11d005e6-6de2-4&from=paste&height=228&id=u7367e41b&originHeight=468&originWidth=1434&originalType=binary&ratio=1&rotation=0&showTitle=false&size=155300&status=done&style=none&taskId=uebed48a7-b78d-4305-af6a-daec6f3697f&title=&width=699)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674913614975-1d73b8d3-d567-4aa9-8a3f-93ff22b8bd0d.png#averageHue=%23fdf5ed&clientId=u11d005e6-6de2-4&from=paste&height=117&id=u5aca4dc7&originHeight=234&originWidth=1366&originalType=binary&ratio=1&rotation=0&showTitle=false&size=120818&status=done&style=none&taskId=ucd07998f-28cd-4e96-aa19-2657d8ed838&title=&width=683)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674914098825-e03cd221-47b9-4810-bce6-bb24c90b1d1b.png#averageHue=%23fdf8f3&clientId=u11d005e6-6de2-4&from=paste&height=167&id=u101dd088&originHeight=334&originWidth=1348&originalType=binary&ratio=1&rotation=0&showTitle=false&size=137729&status=done&style=none&taskId=u7b54f779-1420-47c8-ba39-93cbeac4e3c&title=&width=674)
页分裂：页可以为空，也可以填充一半，也可以填充100%，每页至少包含 2 行数据（如果一行数据过大，会行溢出），根据主键排列。
> 如果每页只包含一行数据，则会形成链表，而不是 B+Tree


**页合并演示**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674914459897-b6556a32-834e-4e9e-90be-5157afb8748f.png#averageHue=%23f8f4f1&clientId=u11d005e6-6de2-4&from=paste&height=193&id=u7d4b6581&originHeight=386&originWidth=1420&originalType=binary&ratio=1&rotation=0&showTitle=false&size=225687&status=done&style=none&taskId=u34df6c13-7c58-41e3-9a3e-9afb5a50f46&title=&width=710)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674914492034-92bff4e6-17ac-4f61-bc37-74677fd70429.png#averageHue=%23fdf5ee&clientId=u11d005e6-6de2-4&from=paste&height=121&id=ub0935aa6&originHeight=242&originWidth=1388&originalType=binary&ratio=1&rotation=0&showTitle=false&size=121948&status=done&style=none&taskId=u20877883-4528-4a4d-b605-c8dfbe5b8b9&title=&width=694)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674914506731-2c76f5e2-dc9d-4443-9703-bad818ceacbd.png#averageHue=%23fcf2e7&clientId=u11d005e6-6de2-4&from=paste&height=93&id=u171bbdd1&originHeight=186&originWidth=1368&originalType=binary&ratio=1&rotation=0&showTitle=false&size=117211&status=done&style=none&taskId=uf720a883-82f2-4194-8841-f5ab09814e4&title=&width=684)
页合并：当删除一行记录时，实际上记录并没有被物理删除，只是记录被标记（flaged）为删除并且它的空间变得允许被其他记录声明使用。当页中删除的记录到达 MERGE_THRESHOLD（默认为页的50%），InnoDB会开始寻找最靠近的页（前后）看看是否可以将这两个页合并以优化空间使用。
MERGE_THRESHOLD：合并页的阈值，可以自己设置，在创建表或创建索引时指定
_文字说明不够清晰明了，具体可以看视频里的PPT演示过程：_[https://www.bilibili.com/video/BV1Kr4y1i7ru?p=90](https://www.bilibili.com/video/BV1Kr4y1i7ru?p=90)

**主键设计原则**

- 满足业务需求的情况下，尽量降低主键的长度
- 插入数据时，尽量选择顺序插入，选择使用 AUTO_INCREMENT 自增主键
- 尽量不要使用 UUID 做主键或者是其他的无序的主键，如身份证号
- 业务操作时，避免对主键的修改

### order by优化

1. Using filesort：通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区 sort buffer 中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序
2. Using index：通过有序索引顺序扫描直接返回有序数据，这种情况即为 using index，不需要额外排序，操作效率高

如果order by字段全部使用升序排序或者降序排序，则都会走索引，但是如果一个字段升序排序，另一个字段降序排序，则不会走索引，explain的extra信息显示的是Using index, Using filesort，如果要优化掉Using filesort，则需要另外再创建一个索引，如：create index idx_age_phone_ad on tb_user(age asc, phone desc);，此时使用select id, age, phone from tb_user order by age asc, phone desc;会全部走索引

![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1674998744615-0ddba819-8616-4899-ad49-5f81c7335f30.png#averageHue=%23faf5e5&clientId=ucd54c1fc-0c0a-4&from=paste&height=300&id=ua4c7f120&originHeight=600&originWidth=1446&originalType=binary&ratio=1&rotation=0&showTitle=false&size=338394&status=done&style=none&taskId=ubf419c34-f054-4398-89ef-2b73fb5c351&title=&width=723)

**总结：**

- 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则
- 尽量使用覆盖索引
- 多字段排序，一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）
- 如果不可避免出现filesort，大数据量排序时，可以适当增大排序缓冲区大小 sort_buffer_size（默认256k）

### group by优化

- 在分组操作时，可以通过索引来提高效率
- 分组操作时，索引的使用也是满足最左前缀法则的

如索引为idx_pro_age_status，则句式可以是select ... where profession = 'xxx' order by age，这样也符合最左前缀法则

### limit优化
常见的问题如limit 2000000, 10，此时需要 MySQL 排序前2000000条记录，但仅仅返回2000000 - 2000010的记录，其他记录丢弃，查询排序的代价非常大。
优化方案：一般分页查询时，通过创建覆盖索引能够比较好地提高性能，可以通过覆盖索引加子查询形式进行优化
例如：
```sql
-- 此语句耗时很长
select * from tb_sku limit 9000000, 10;
-- 通过覆盖索引加快速度，直接通过主键索引进行排序及查询
select id from tb_sku order by id limit 9000000, 10;
-- 下面的语句是错误的，因为 MySQL 不支持 in 里面使用 limit
-- select * from tb_sku where id in (select id from tb_sku order by id limit 9000000, 10);
-- 通过覆盖索引 +（隐式）连表查询实现，把 select 子查询的结果集作为表 a
select s.* from tb_sku as s, (select id from tb_sku order by id limit 9000000, 10) as a
where s.id = a.id;
```

### count优化
MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 count(*) 的时候会直接返回这个数，效率很高（前提是没有其他条件），例如
> select count(*) from table;

InnoDB 在执行 count(*) 时，需要把数据一行一行地从引擎里面读出来，然后累计计数。
优化方案：自己计数，如创建key-value表存储在内存或硬盘，或者是用redis
count的几种用法：
count(*)、count(主键)、count(字段)、count(1)
各种用法的性能：

- count(主键)：InnoDB引擎会遍历整张表，把每行的主键id值都取出来，返回给服务层，服务层拿到主键后，直接按行进行累加（主键不可能为空）
- count(字段)：

没有not null约束的话，InnoDB引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，服务层判断是否为	       null，不为null，计数累加；
有not null约束的话，InnoDB引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加

- count(1)：InnoDB 引擎遍历整张表，但不取值。服务层对于返回的每一层，放一个数字 1 进去，直接按行进行累加，也可以设置其他数字，但结果都是跟 count(*) 一样
- count(*)：InnoDB 引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加

按效率排序：count(字段) < count(主键) < count(1) < count(*)，所以尽量使用 count(*)

### update优化（避免行锁升级为表锁）
**尽量根据主键或索引字段进行数据更新**
InnoDB 的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行锁升级为表锁。
如以下两条语句：
update student set no = '123' where id = 1;，这句由于id有主键索引，所以只会锁这一行；
update student set no = '123' where name = 'test';，这句由于name没有索引，所以会把整张表都锁住进行数据更新，解决方法是给name字段添加索引

## 性能最佳实践

1. 较小的表性能更好。不要存储不需要的数据。解决今天的问题，而不是明天可能永远不会发生的问题。

2. 使用尽可能小的数据类型。如果你需要存储人们的年龄，一个TINYINT就足够了，无需使用INT。对于一个小的表来说，节省几个字节没什么大不了的，但在包含数百万条记录的表中却具有显著的影响。

3. 每个表都必须有一个主键。

4. 主键应短。如果您只需要存储一百条记录，最好选择 TINYINT 而不是 INT。

5. 首选数字类型而不是字符串作为主键。这使得通过主键查找记录更快。

6. 避免BLOB。它们会增加数据库的大小，并会对性能产生负面影响。如果可以，请将文件存储在磁盘上。

7. 如果表的列太多，请考虑使用一对一关系将其拆分为两个相关表。这称为垂直分区（vertical partitioning）。例如，您可能有一个包含地址列的客户表。**如果这些地址不经常被读取**，请将表拆分为两个表（users 和 user_addresses）。

8. **相反，如果由于数据过于碎片化而总是需要在查询中多次使用表联接**，则可能需要考虑对数据反归一化。反归一化与归一化相反。它涉及把一个表中的列合并到另一个表（以减少联接数）。

9. **请考虑为昂贵的查询创建摘要/缓存表。**例如，如果获取论坛列表和每个论坛中的帖子数量的查询非常昂贵，请创建一个名为 forums_summary 的表，其中包含论坛列表及其中的帖子数量。您**可以使用事件定期刷新此表中的数据。您还可以使用触发器在每次有新帖子时更新计数。**

10. **全表扫描是查询速度慢的一个主要原因。使用 EXPLAIN 语句并查找类型为 "ALL" 的查询。这些是全表扫描。使用索引优化这些查询。**

11. 在**设计索引时**，请先查看 WHERE 子句中的列。这些是第一批候选人，因为它们有助于缩小搜索范围。接下来，查看 ORDER BY 子句中使用的列。如果它们存在于索引中，MySQL 可以扫描索引以返回有序的数据，而无需执行排序操作（filesort）。最后，考虑将 SELECT 子句中的列添加到索引中。这为您提供了覆盖索引，能覆盖你查询的完整需求。MySQL 不再需要从原表中检索任何内容。

12. **选择组合索引，而不是多个单列索引**。

13. 索引中的列**顺序**很重要。将最常用的列和基数较高的列放在第一位，但始终考虑您的查询。

14. 删除重复、冗余和未使用的索引。重复索引是同一组具有相同顺序的列上的索引。冗余索引是不必要的索引，可以替换为现有索引。例如，如果在列（A、 B）上有索引，并在列 （A）上创建另一个索引，则后者是冗余的，因为前一个索引可以满足相同的需求。

15. 在分析现有索引之前，不要创建新索引。

16. 在查询中隔离你的列，以便 MySQL 可以使用你的索引。

17. 避免 SELECT *。**大多数时候，选择所有列会忽略索引并返回您可能不需要的不必要的列。这会给数据库服务器带来额外负载。**

18. 只返回你需要的行。使用 LIMIT 子句限制返回的行数。

19. **避免使用前导通配符的LIKE 表达式（eg."%name"） 。**

20. 如果您有一个使用 OR 运算符的速度较慢的查询，请考虑将查询分解为两个使用单独索引的查询，并使用 UNION 运算符组合它们。
