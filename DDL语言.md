# sql-learning in week10(using MySQL)
<h2>DDL（Data Defination Language）</h2>
库与表的操作<br>
库、表操作:
<ol>
	<li>创建create</li>
	<li>修改alter</li>
	<li>删除drop</li>
</ol>
<h3>库的管理</h3>
<h4>库的创建</h4>
语法：

```mysql
create database 库名;
```
<br>
例：创建books库

```mysql
CREATE DATABASE books；
```
<br>
例：创建books库（若存在则不创建）

```mysql
CREATE DATABASE IF NOT EXISTS books；
```
<br>
<h4>库的修改</h4>
直接用sql语句更改库名是不安全的行为<br>
库的修改体现在修改库的字符集<br>
例：修改字符集为gbk

```mysql
ALTER DATABASE books CHARACTER SET gbk;
```
<br>

<h4>库的删除</h4>
例：删除books库

```mysql
DROP DATABASE IF EXISTS books; #为防止重复执行该语句报错，加上IF EXISTS
```
<br>
<h3>表的管理</h3>
<h4>表的创建</h4>
语法：

```mysql
create table 表名(
		列名 列的类型[(长度) 约束],
		列名 列的类型[(长度) 约束],
		列名 列的类型[(长度) 约束],
		...
		列名 列的类型[(长度) 约束],
); #列名、列的类型必须写，长度、约束可不写
```
<br>
例：创建表book

```mysql
CREATE TABLE IF NOT EXISTS book(
	b_name VARCHAR(20),
	id INT,
	price DOUBLE,
	author_id INT,
	publish_date DATETIME
); #创建book表
CREATE TABLE IF NOT EXISTS author(
	id INT,
	nation VARCHAR(20),
	au_name VARCHAR(20)
) #创建author表
```
<br>
<h4>表的修改</h4>
语法：

```mysql
alter table 表名 add|drop|modify|change|rename to column 列名 [列类型 约束];
```
<br>
<ul>
	<li>修改列名

```mysql
ALTER TABLE book CHANGE COLUMN publish_date publishDate DATETIME; 
#修改book表下publish_date列列名为publishDate
#column可以省略
```
<br>
	</li>
	<li>修改列的类型或约束

```mysql
ALTER TABLE book MODIFY COLUMN publishDate TIMESTAMP;
#修改publishDate列的类型为TIMESTAMP
```
<br>
	</li>
	<li>添加列

```mysql
ALTER TABLE author ADD COLUMN annual DOUBLE;
#在author表下添加annual列
```
<br>
	</li>
	<li>删除列

```mysql
ALTER TABLE author DROP COLUMN annual;
#删除author下的annual列
```
<br>
	</li>
	<li>修改表名
	
```mysql
ALTER TABLE author RENAME TO book_author; 
#修改author表的名字为book_author
```
<br>
	</li>
</ul>
<h4>表的删除</h4>

```mysql
DROP TABLE IF EXISTS book_author;
#删除book_author表
```
<br>
<h2>建库建表的通用写法</h2>
<strong>
通用的写法（先删除，后新建）：<br>
库：

```mysql
derp database if exists 旧库;
create database if not exists 新库;
```
<br>
表：

```mysql
derp table if exists 旧表;
create table if not exists 新表(
	...
	...
	...
);
```
<br>
</strong>
<h4>库的复制</h4>
<h5>复制表的结构</h5>

```mysql
CREATE TABLE copy_author_1 LIKE author;
#复制author的结构给copy_author_1
```
<h5>复制表的数据+结构</h5>

```mysql
CREATE TABLE copy_author_2
SELECT * FROM author; 
#复制author的所有数据与结构给copy_author_2
CREATE TABLE copy_author_3
SELECT id,au_name
FROM author
WHERE nation = 'CN';
#复制author中nation = CN的数据中的id和au_name给copy_author_3
CREATE TABLE copy_author_4
SELECT id,au_name
FROM author
WHERE NULL;
#复制author表中的id和au_name的结构给copy_author_4
```
<br>


