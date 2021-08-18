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
<br># sql-learning in week10(using MySQL)
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
<h5>约束的介绍</h5>
约束指的是一种限制，限制行或者列的数据，为了保证数据的准确性、可靠性。<br>
分类：
<ol>
	<li>not null非空，用于保证该字段的值不能为空</li>
	<li>default默认，用于保证该字段存在默认值</li>
	<li>primary key主键，用于保证该字段的值具有唯一性，并且非空</li>	
	<li>unique唯一，用于保证该字段的值具有唯一性，但是可以为空</li>
	<li>check检查约束</li>
	<li>foreign key外键，用于限制两个表的关系，用于保证该字段的值必须来自于主表的关联列的值（在从表中添加外键约束，引用主表中某列的值）</li>
</ol>
添加约束（约束可以添加多个）：
<ul>
	<li>创建表时</li>
	<li>修改表时</li>
</ul>
约束的添加分类：
<ul>
	<li>列级约束

```mysql
create table employees(
	id int 约束 #这是一个列级约束
	name varchar 约束 #这是一个列级约束
    #所有的约束都可以作为列级约束，但是外键约束作为列级约束没有效果
)
```
<br>
	</li>
	<li>表级约束

```mysql
create table employees(
	id int 
	name varchar 
	约束 #这是一个表级约束
	#除了非空，默认，其他的都支持
)
```
<br>
	</li>
</ul>
创建表时添加约束：
添加列级约束：

```mysql
CREATE TABLE IF NOT EXISTS stuinfo(
	id INT PRIMARY KEY, #主键
	NAME VARCHAR(20) NOT NULL, #非空
	gender CHAR CHECK(gender='男' OR gender='女'), #检查
	seat INT UNIQUE, #唯一
	age INT DEFAULT 18, #默认	
);
```
<br>
添加表级约束：

语法：

```mysql
constraint 约束名 约束条件(字段名)
```
<br>
 
```mysql
CREATE TABLE IF NOT EXISTS stuinfo(
	id INT,
	NAME VARCHAR(20),
	gender CHAR ,
	seat INT ,
	age INT ,
	major_id INT,
	CONSTRAINT s_id(键名) PRIMARY KEY(id),#主键
	CONSTRAINT s_gender CHECK(gender='男' OR gender='女'),#检查
	CONSTRAINT s_seat(键名) UNIQUE(seat),#唯一
	CONSTRAINT s_major_id(键名) FOREIGN KEY(major_id) REFERENCES major(id)#外键 references 主表(关联列)	
);
```
<br>
列级约束对比表级约束：
<table>
	<tr>
		<td></td>
		<td>位置</td>
		<td>支持的约束类型</td>
		<td>是否可以起约束名（Key_name）</td>
	</tr>
	<tr>
		<td>列级约束</td>
		<td>列的后面</td>
		<td>语法都支持，但是外键没有效果</td>
		<td>不可以</td>
	</tr>
	<tr>
		<td>表级约束</td>
		<td>所有列下面</td>
		<td>默认和非空不支持，其他支持</td>
		<td>可以（主键没有效果）</td>>
	</tr>
</table>
<strong>规范：外键写表级约束，其他都写列级约束</strong><br>
主键与唯一的对比：
<table>
	<tr>
		<td></td>
		<td>保证唯一性</td>
		<td>是否允许为空</td>
		<td>一个表中是否可以有多个</td>
		<td>是否允许组合</td>
	</tr>
	<tr>
		<td>主键</td>
		<td>√</td>
		<td>×</td>
		<td>×</td>
		<td>√（指组合主键，表现为多个字段组合为一个主键）</td>
	</tr>
	<tr>
		<td>唯一</td>
		<td>√</td>
		<td>√</td>
		<td>√</td>
		<td>√（指组合唯一，表现为多个字段组合为一个唯一）</td>
	</tr>
</table>
外键的特点：
<ol>
	<li>从表中设置外键关系</li>
	<li>从表的外键列与主表的关联列类型一致，名称无要求</li>
	<li>主表的关联列必须是一个key（主键或唯一键）</li>
	<li>插入数据时先插主表的数据，在插入从表数据</li>
	<li>删除数据时先删除从表，再删除主表</li>
</ol>
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
#添加publishDate列的类型为TIMESTAMP
ALTER TABLE stuinfo MODIFY COLUMN age INT NOT NULL;
#添加age字段约束为NOT NULL
ALTER TABLE stuinfo MODIFY COLUMN gender CHAR UNIQUE;
#添加gender字段约束为unique（列级约束写法）
ALTER TABLE stuinfo ADD UNIQUE(seat);
#添加seat字段约束unique（表级约束写法）

#删除约束：
ALTER TABLE stuinfo MODIFY COLUMN age INT NOT NULL;
#删除空约束（删除非空则为NULL）
ALTER TABLE stuinfo MODIFY COLUMN seat INT;
#删除默认约束
ALTER TABLE stuinfo DROP PRIMARY KEY;
#删除主键约束
ALTER TABLE stuinfo DROP INDEX 约束名;
#删除唯一键约束
ALTER TABLE stuinfo DROP FOREIGN KEY 约束名;
#删除外键约束
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
<h5>约束的介绍</h5>
约束指的是一种限制，限制行或者列的数据，为了保证数据的准确性、可靠性。<br>
分类：
<ol>
	<li>not null非空，用于保证该字段的值不能为空</li>
	<li>default默认，用于保证该字段存在默认值</li>
	<li>primary key主键，用于保证该字段的值具有唯一性，并且非空</li>	
	<li>unique唯一，用于保证该字段的值具有唯一性，但是可以为空</li>
	<li>check检查约束</li>
	<li>foreign key外键，用于限制两个表的关系，用于保证该字段的值必须来自于主表的关联列的值（在从表中添加外键约束，引用主表中某列的值）</li>
</ol>
添加约束（约束可以添加多个）：
<ul>
	<li>创建表时</li>
	<li>修改表时</li>
</ul>
约束的添加分类：
<ul>
	<li>列级约束

```mysql
create table employees(
	id int 约束 #这是一个列级约束
	name varchar 约束 #这是一个列级约束
    #所有的约束都可以作为列级约束，但是外键约束作为列级约束没有效果
)
```
<br>
	</li>
	<li>表级约束

```mysql
create table employees(
	id int 
	name varchar 
	约束 #这是一个表级约束
	#除了非空，默认，其他的都支持
)
```
<br>
	</li>
</ul>
创建表时添加约束：
添加列级约束：

```mysql
CREATE TABLE IF NOT EXISTS stuinfo(
	id INT PRIMARY KEY, #主键
	NAME VARCHAR(20) NOT NULL, #非空
	gender CHAR CHECK(gender='男' OR gender='女'), #检查
	seat INT UNIQUE, #唯一
	age INT DEFAULT 18, #默认	
);
```
<br>
添加表级约束：

语法：

```mysql
constraint 约束名 约束条件(字段名)
```
<br>
 
```mysql
CREATE TABLE IF NOT EXISTS stuinfo(
	id INT,
	NAME VARCHAR(20),
	gender CHAR ,
	seat INT ,
	age INT ,
	major_id INT,
	CONSTRAINT s_id PRIMARY KEY(id),#主键
	CONSTRAINT s_gender CHECK(gender='男' OR gender='女'),#检查
	CONSTRAINT s_seat UNIQUE(seat),#唯一
	CONSTRAINT s_major_id FOREIGN KEY(major_id) REFERENCES major(id)#外键	
);
```
<br>
<strong>规范：外键写表级约束，其他都写列级约束</strong><br>
主键与唯一的对比：
<table>
	<tr>
		<td></td>
		<td>保证唯一性</td>
		<td>是否允许为空</td>
		<td>一个表中是否可以有多个</td>
		<td>是否允许组合</td>
	</tr>
	<tr>
		<td>主键</td>
		<td>√</td>
		<td>×</td>
		<td>×</td>
		<td>√（指组合主键，表现为多个字段组合为一个主键）</td>
	</tr>
	<tr>
		<td>唯一</td>
		<td>√</td>
		<td>√</td>
		<td>√</td>
		<td>√（指组合唯一，表现为多个字段组合为一个唯一）</td>
	</tr>
</table>
外键的特点：
<ol>
	<li>从表中设置外键关系</li>
	<li>从表的外键列与主表的关联列类型一致，名称无要求</li>
	<li>主表的关联列必须是一个key（主键或唯一键）</li>
	<li>插入数据时先插主表的数据，在插入从表数据</li>
	<li>删除数据时先删除从表，再删除主表</li>
</ol>
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
#添加publishDate列的类型为TIMESTAMP
ALTER TABLE stuinfo MODIFY COLUMN age INT NOT NULL;
#添加age字段约束为NOT NULL
ALTER TABLE stuinfo MODIFY COLUMN gender CHAR UNIQUE;
#添加gender字段约束为unique（列级约束写法）
ALTER TABLE stuinfo ADD UNIQUE(seat);
#添加seat字段约束unique（表级约束写法）

#删除约束：
ALTER TABLE stuinfo MODIFY COLUMN age INT NOT NULL;
#删除空约束（删除非空则为NULL）
ALTER TABLE stuinfo MODIFY COLUMN seat INT;
#删除默认约束
ALTER TABLE stuinfo DROP PRIMARY KEY;
#删除主键约束
ALTER TABLE stuinfo DROP INDEX 键名;
#删除唯一键约束
ALTER TABLE stuinfo DROP FOREIGN KEY 键名;
#删除外键约束
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


