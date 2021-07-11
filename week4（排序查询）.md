# sql-learning in week4(using MySQL)
<h2>DQL语言学习</h2>
<h3>进阶3 排序查询</h3>
引入:
select * from employees;输出员工信息时，顺序为默认排序<br/>
语法:<br/>
select 查询列表 from 表名 （可写条件查询条件） order by 查询列表 排序列表(asc低到高/desc高到低);<br/>
特点:
<ol>
	<li>不写默认升序</li>
	<li>asc升序，desc降序</li>
	<li>支持单个字段，多个字段，表达式，函数，别名</li>
	<li>一般放在查询语句的最后面</li>
</ol>
案例:
<ol>
	<li>按升降序排序<br/>
	select * from employees order by hiredate asc;<br/>输出入职日期由小到大的员工的员工信息)</li>
	<li>按表达式排序<br/>
	SELECT *,salary*12*(1+IFNULL(`commission_pct`,0)) FROM employees ORDER BY salary*12*(1+IFNULL(`commission_pct`,0)) ASC;<br/>
	输出年薪由小到大排列的员工信息及其年薪</li>
	<li>按别名排序<br/>
	SELECT *,salary*12*(1+IFNULL(`commission_pct`,0)) 年薪 FROM employees ORDER BY 年薪 DESC;<br/>
	输出年薪由大到小排列的员工信息及其年薪</li>
	<li>按函数排序<br/>
	SELECT LENGTH(last_name) 字节长度,last_name,salary FROM employees ORDER BY 字节长度 DESC;<br/>
	按姓名的长度显示员工的信息与工资</li>
	<li>按多个字段排序<br/>
	SELECT * FROM employees ORDER BY salary ASC, employee_id DESC;<br/>
	查询员工信息，先按照工资排序，在按照员工编号排序</li>
</ol>

