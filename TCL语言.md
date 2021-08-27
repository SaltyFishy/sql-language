# sql-learning in week11(using MySQL)
<h2>TCL(Transaction Control Language,事务控制语言)</h2>
事物的概念：一个或一组sql语言组成一个执行单元，这个执行单元要么全部执行，要么全部都不执行,如果某一处出现了错误，整个单元都将回滚，所有数据回到初始状态。<br>
存储引擎：在mysql中的各种数据用各种不同的技术储存在文件（或内存）中，且不是所有的存储引擎都支持事务。<br>
<img src="https://github.com/SaltyFishy/sql-language/blob/week11/%E4%BA%8B%E7%89%A9%E7%9A%84acid%E5%B1%9E%E6%80%A7.png" alt="事物的acid属性">
<h3>事务的创建</h3>
隐式事务：事务没有明显的开启与结束的标记。<br>
如：insert、update、delete语句<br>
显式事务：事务有明显的开始与结束的标记。<br>
前提：设置自动提交功能为禁止。<br>

```mysql
set autocommit = 0;#关闭自动提交功能
start transaction;#开启事务（默认已开启）
```
<br>
开启事务：

```mysql
#步骤1：
set autocommit = 0;
start transaction;#可选

#步骤2：
#编写事务中的sql语句

#步骤3：
commit;#提交事务（使数据保存到磁盘，保存后就无法进行回滚了）
rollback;#回滚事务（注意，回滚成功的前提是数据保存到了内存，还没有保存到磁盘）
```
<br>
<h3>delete与truncate在事务中的区别</h3>

```mysql
#演示delete
SET autocommit = 0;
START TRANSACTION;
USE books;
DELETE FROM author;#先执行上面的代码块
ROLLBACK;#再执行此处的代码（回滚成功）

#演示truncate
SET autocommit = 0;
START TRANSACTION;
USE books;
TRUNCATE TABLE author;#先执行上面的代码块
ROLLBACK;#再执行此处的代码（回滚失败）
```
<br>
<h3>并发问题</h3>
对于同时运行的多个事务，当这些事务访问**数据库中相同的数据时**，如果没有采取必要的隔离措施，就会导致并发问题。<br>
常见并发问题：
<ul>
	<li>脏读：对于两个事务a、b，a读取了已经被b更新却还没有被提交的字段，之后，若b回滚，则a读取的数据就是临时且无效的（脏读只在读未提交隔离级别才会出现）<br>
	<img src="https://github.com/SaltyFishy/sql-language/blob/week11/%E8%84%8F%E8%AF%BB%E7%A4%BA%E6%84%8F%E5%9B%BE.png" alt="脏读示意图"></li>
	<li>不可重复读：对于两个事务a、b，a读取了一个字段，然后b更新了该字段，之后a重新读取了该字段，值不同（不可重复读在读未提交和读已提交隔离级别都可能会出现）
	<img src="https://github.com/SaltyFishy/sql-language/blob/week11/%E4%B8%8D%E5%8F%AF%E9%87%8D%E5%A4%8D%E8%AF%BB%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg" alt="不可重复读示意图"><br>
	（注意sessionB的事务为隐式事务，此时的autocommit为1，每条SQL语句执行完自动提交）</li>
	<li>幻读：对于两个事务a、b，a从表中读取了一个字段，然后b在该表中插入了一些新的行，之后若a再次读取同一个表，就会多出几行（幻读在读未提交、读已提交、可重复读隔离级别都可能会出现）
	<img src="https://github.com/SaltyFishy/sql-language/blob/week11/%E5%B9%BB%E8%AF%BB%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg" alt="幻读示意图"><br>
	（注意sessionB的事务为隐式事务，此时的autocommit为1，每条SQL语句执行完自动提交</li>
</ul>
查询隔离级别与修改：

```mysql
SELECT @@transaction_isolation;#查询隔离级别
SET SESSION TRANSACTION ISOLATION LEVEL 隔离级别;#修改当前没mysql连接的隔离级别
```
<br>
事务隔离级别（mysql有四种）：
<ul>
	<li>读未提交（READ UNCOMMITTED）<br>
	在读未提交隔离级别下，事务A可以读取到事务B修改过但未提交的数据。</li>
	<li>读已提交（READ COMMITTED）<br>
	在读已提交隔离级别下，事务B只能在事务A修改过并且已提交后才能读取到事务B修改的数据。</li>
	<li>可重复读（REPEATABLE READ）,也是mysql的默认隔离级别<br>
	在可重复读隔离级别下，事务B只能在事务A修改过数据并提交后，自己也提交事务后，才能读取到事务B修改的数据。</li>
	<li>可串行化（SERIALIZABLE）<br>
	<img src="https://github.com/SaltyFishy/sql-language/blob/week11/%E8%AF%BB%E5%86%99%E9%98%BB%E5%A1%9E.jpg" alt="读写阻塞">
	</li>
</ul>
隔离级别比较：可串行化>可重复读>读已提交>读未提交<br>
隔离级别对性能的影响比较：可串行化>可重复读>读已提交>读未提交<br>
<img src="https://github.com/SaltyFishy/sql-language/blob/week11/%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB.jpg" alt="隔离级别"><br>
<h3>回滚点</h3>
关键词：savepoint<br>

```mysql
SET autocommit = 0;
START TRANSACTION;
USE books;
DELETE FROM author WHERE id = 1;
SAVEPOINT a;#设置回滚点
DELETE FROM author;
ROLLBACK TO a;#返回到回滚点
```
<br>
<h3>视图</h3>
含义：虚拟的表<br>
mysql5.1版本的新特性，是通过表动态生成的数据，只保存了sql逻辑，不保存查询结果<br>
应用场景：多个地方用到同样的查询结果or该查询结果使用的sql语句较为复杂<br>
举例：

```mysql
#正常的查询
SELECT employee_id,department_name
FROM employees e
INNER JOIN departments d ON e.`department_id` = d.`department_id`
WHERE first_name LIKE "Ste%";

#视图写法
#创建视图
CREATE VIEW v2
AS 
SELECT first_name,department_name
FROM employees e
INNER JOIN departments d ON e.`department_id` = d.`department_id`
#使用视图进行查询
SELECT * FROM v2 WHERE first_name LIKE "Ste%";
```
<br>
基本语法：

```mysql
#创建视图
CREATE VIEW IF NOT EXISTS 别名
AS 
查询语句;

#创建视图或修改，方式一：
CREATE OR REPLACE VIEW IF NOT EXISTS 别名
AS 
查询语句;
#创建视图或修改，方式二：
ALTER VIEW 别名
AS
查询语句;

#删除视图
DROP VIEW IF EXISTS 别名;

#查看视图的结构，方式一：
DESC 别名;
#查看视图的结构，方式二：
SHOW CREATE VIEW 别名;

#视图的更新（不要试图更新视图）
#更新视图的数据
#插入
CREATE OR REPLACE VIEW v1
AS
SELECT last_name,email FROM employees;
INSERT INTO v1 VALUES('芜湖','wuhu@qq.com');
#注意插入之后原表也会被插入数据。。。。

#更新
UPDATE v1 SET last_name = "嘿嘿嘿" WHERE last_name = "芜湖";
#注意更新之后原表也会更新。。。。

#删除
DELETE FROM v1 WHERE last_name = "嘿嘿嘿";
#注意删除之后原表也会删除。。。。

#可以看到视图更新会更新原表的数据，这不是我们所希望的，因此会采用DCL语言（这里不展开）来进行控制。
#并且能够更新的视图是很小的一部分，包含了分组函数、distinct、having、union、union all、group by、常量视图、select中有子查询、join、from了一个不能更新的视图、where子句的子查询引用了from子句中的表这些情况之一都不能更新视图。
```
<br>
视图与表的对比：
<table>
	<tr>
		<td></td>
		<td>创建语法</td>
		<td>是否实际占用物理空间</td>
		<td>使用</td>
	</tr>
	<tr>
		<td>视图</td>
		<td>create view</td>
		<td>否（只保存了sql逻辑）</td>
		<td>增删改查（一般只查）</td>
	</tr>
	<tr>
		<td>表</td>
		<td>create table</td>
		<td>是</td>
		<td>增删改查</td>>
	</tr>
</table>











