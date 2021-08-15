# sql-learning in week10(using MySQL)
<h2>DML（Data Management Language）</h2>
包括：
<ul>
	<li>插入insert</li>
	<li>修改update</li>
	<li>删除delete</li>
</ul>

<h3>插入</h3>
注意要点：
<ol>
	<li>插入的值的类型与列的类型一致</li>
	<li>可以为NULL的列可以直接写NULL或不写该列及其值，不可以为NULL的列必须有插入值</li>
	<li>列的顺序可以调整，但是值必须与列一一对应</li>
	<li>列数与值数必须一致</li>
	<li>可以省略列名，默认对所有列插入，而且列的顺序与表内列的顺序一致</li>
</ol>
两种插入方式对比：
<strong>
<ol>
	<li>方式一可以插入多行，方式二不支持

```mysql
INSERT INTO beauty(id,NAME,phone)
VALUES (15,'王淑仪',NULL),
	(16,'刘晓玲',17584552288),
	(17,'李诗琪',NULL);
```
<br>
	</li>
	<li>方式一支持子查询，方式二不支持子查询

```mysql
INSERT INTO beauty(id,NAME,phone)
SELECT 15,'王淑仪',NULL;
```
<br>
	</li>
</ol>	
</strong>
<h4>方式一</h4>
语法：

```mysql
insert into 表名(列名...)
values(值.....);
```
<br>
例：

```mysql
INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
VALUES(14,'李婷','女','1988-2-14','15482526359',NULL,NULL);
```
<br>
<h4>方式二</h4>
语法：

```mysql
insert into 表名
set 列名=值.....;
```
<br>
例：

```mysql
INSERT INTO boys
SET boyName = '芜湖',id = 100;
```
<br>
<h3>修改</h3>

<h4>修改单表的记录</h4>
语法：

```mysql
update 表名
set 列=值.........
where 筛选条件;
```
<br>
例1：修改beauty表中姓周的女生的电话为13899888899

```mysql
UPDATE beauty
SET phone = '13899888899'
WHERE NAME LIKE '周%';
```
<br>
例1：修改boys表中id=2的名称为芜湖，魅力值为100

```mysql
UPDATE boys
SET boyName = '芜湖',userCP = 100
WHERE id = 2;
```
<br>
<h4>修改多表的记录</h4>
语法（92标准）：

```mysql
UPDATE 表1 别名,表2 别名
set 列=值...
where 连接条件
and 筛选条件;
```
<br>
语法（99标准）：

```mysql
UPDATE 表1 别名
inner(left/right) join 表2 别名
on 连接条件
set 列=值...
where 筛选条件;
```
<br>
例1：修改张无忌女朋友的手机号为14562857496

```mysql
UPDATE boys bo
INNER JOIN beauty b
ON bo.id = b.boyfriend_id
SET b.phone = 14562857496
WHERE bo.boyName = '张无忌'
```
<br>
例2：修改没有男朋友的女生的男朋友编号都为2

```mysql
UPDATE beauty b
SET b.boyfriend_id = 2
WHERE b.boyfriend_id IS NULL
```
<br>
<h3>删除</h3>
<h4>方式一（delete）</h4>
语法（单表或多表的删除）：

```mysql
delete from 表名 where 筛选条件;
```
<br>
<strong>删除为删除整行</strong><br>
单表的删除：<br>
例：删除手机编号最后一位为9的女生信息

```mysql
DELETE FROM beauty WHERE phone LIKE '%9';
```
<br>
多表的删除：<br>
例：删除张无忌的女朋友的信息

sql92语法:

```mysql
delete 需要被删除的信息所在的表的别名
from 表1 别名,表2 别名
where 连接条件
and 筛选条件;
```
<br>
sql99语法:

```mysql
delete 需要被删除的信息所在的表的别名
from 表1 别名
inner(lelt/right) join 表2 别名 on 连接条件
where 筛选条件;
```
<br>
实例：

```mysql
DELETE b
FROM beauty b
INNER JOIN boys bo
ON bo.id = b.`boyfriend_id`
WHERE bo.boyName = '张无忌';
```
<br>
<h4>方式二（truncate）</h4>
语法：

```mysql
truncate table 表名;
```
<br>
<strong>删除为删除整表</strong><br>
<strong>不能加where</strong><br>
<h3>delete与truncate对比</h3>
<img src="https://github.com/SaltyFishy/sql-language/blob/week10/delete%E4%B8%8Etruncate%E5%AF%B9%E6%AF%94.jpg" alt="delete与truncate对比">









