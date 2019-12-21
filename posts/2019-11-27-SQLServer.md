---
layout: post
title:  "SQL Server 基本语法"
---

* content
{:toc}

## SQL Server 简介

由于最近所做项目的需求，又重温了一下 SQL Server 数据库的使用，以及 SQL Server 数据库与 MFC 连接的方法。

SQL Server 是 Microsoft 公司推出的关系型数据库管理系统。具有使用方便可伸缩性好与相关软件集成程度高等优点。Microsoft SQL Server 是一个全面的数据库平台，使用集成的商业智能 (BI) 工具提供了企业级的数据管理。Microsoft SQL Server 数据库引擎为关系型数据和结构化数据提供了更安全可靠的存储功能，可以方便用户构建和管理用于业务的高可用和高性能的数据应用程序。

---

## SQL Server 基本语法

1、创建数据库

> CREATE DATABASE 数据库名

``` c++
CREATE DATABASE DBXXX  //创建一个名为DBXXX的数据库
```

2、删除数据库

> DROP DATABASE 数据库名

``` c++
DROP DATABASE DBXXX  //删除一个名为DBXXX的数据库
```

3、创建表

> CREATE TABLE 表格名 (栏位1 数据类型1, 栏位2 数据类型2)

``` c++
CREATE TABLE TABXXX (XX1 varchar(MAX), XX2 varchar(MAX))  //创建一个名为TABXXX的表格，其中有两列，一列名为XX1（数据类型为varchar(MAX)），一列名为XX2（数据类型为varchar(MAX)）
```

4、删除表

> DROP TABLE 表格名

``` c++
DROP TABLE TABXXX  //删除一个名为TABXXX的表格
```

5、新增表记录

> INSERT INTO 表格名 (栏位1, 栏位2) VALUES (值1, 值2)

``` c++
INSERT INTO TABXXX (XX1, XX2) VALUES ('xx1_data', 'xx2_data')  //往TABXXX表格中插入一组数据（一行数据），列名为XX1处的位置插入数据值为xx1_data,列名为XX2处的位置插入数据值为xx2_data
```

6、删除表记录

> DELETE FROM 表格名 WHERE 栏位X = 值X

``` c++
DELETE FROM TABXXX WHERE XX1 = 'xx1_data'  //删除TABXXX表格中的一组数据（一行数据），此组数据的XX1列的数据值为xx1_data
```

7、修改表记录

> UPDATE 表格名 SET 栏位X = 值X WHERE 栏位Y = 值Y

``` c++
UPDATE TABXXX SET XX1 = 'xx1_data_new' WHERE XX1 = 'xx1_data_old'  //修改TABXXX表格中的一个数据，此数据在XX1列数据值为xx1_data_old的那行数据中，将这行数据中的对应XX1列的数据改为xx1_data_new

UPDATE TABXXX SET XX1 = 'xx1_data' WHERE XX2 = 'xx2_data'  //修改TABXXX表格中的一个数据，此数据在XX2列数据值为xx2_data的那行数据中，将这行数据中的对应XX1列的那个数据改为xx1_data
```

8、查询表记录

（1）
> SELECT 栏位X FROM 表格名 WHERE 栏位Y = 值Y

``` c++
SELECT XX1 FROM TABXXX WHERE XX2 = 'xx2_data'  //查询TABXXX表格中的一个数据，此数据在XX2列数据值为xx2_data的那行数据中，查询这行数据中的对应XX1列的那个数据
```

（2）
> SELECT * FROM 表格名 WHERE 栏位Y = 值Y

``` c++
SELECT * FROM TABXXX WHERE XX1 = 'xx1_data'  //查询TABXXX表格中的一组数据（一行数据），此行数据即为XX1列数据值为xx1_data的那行数据
```

---

有关 SQL Server 的内容实在太多了，一篇根本写不完。所以就先写这些基础的吧，我要去吃饭了。
