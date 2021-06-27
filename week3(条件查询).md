# sql-learning in week3(using MySQL)
<h2>DQL语言学习</h2>
<h3>进阶2 条件查询</h3>
语法：
select 查询列表 from 表名 where 筛选条件;<br>
查询列表可以为：表中的列（字段），函数，常量，表达式<br>
执行顺序：
<ol>
  <li>先进入对应的表中</li>
  <li>判断筛选条件</li>
  <li>查询</li>
</ol>
根据筛选条件可以分类为：
<ol>
  <li>按条件表达式筛选（包括>表示大于, =表示等于, <表示小于, <>或!=表示不等, >=表示大于等于, <=表示小于等于）</li> 
  <li>按逻辑表达式查询（包括 &&或and表示与, ||或or表示或, !或not表示非）</li>
  <li>模糊查询（本质上是条件运算符，包括like, between and, is null,in）</li>
</ol>
实例：
<ol>
  <li>按照条件表达式查询<br>
  SELECT * FROM employees WHERE salary>12000;（查询工资大于12000的员工信息）<br>
  SELECT * FROM employees WHERE salary<>12000;（查询工资不等于12000的员工信息）</li>
  <li>按照逻辑表达式查询<br>
  SELECT last_name,commission_pct,salary FROM employees WHERE salary>=10000 AND salary<=20000;（查询工资大于10000且小于20000的员工的姓、奖金与工资）<br>
  SELECT * FROM employees WHERE department_id<90 OR department_id>110;（查询部门id大于110或小于90的员工信息）</li>
  <li>模糊查询
    <ol>
      <li>like关键字<br>
      SELECT * FROM employees WHERE last_name LIKE '%a%';（查询姓中带字符a的员工，注意这里的%为通配符）<br>
      通配符分类：
        <ol>
          <li>% 代指任意多个（包括0）个字符</li>
          <li>_ 代指单个字符</li>
        </ol>
      SELECT * FROM employees WHERE last_name LIKE '__n_l%';（查询姓中第三个字符为n第五个为l的员工）<br>
      <strong>注意特殊情况，如当需要查询的字符中有下划线_时，我们要使用转义字符\来转义（或者说代指）下划线_，当然也可以自定义转义字符，使用关键字escape '转义字符'<br>
      如用美元符号$转义下划线_ SELECT * FROM employees WHERE last_name LIKE '_$_%' ESCAPE '$'; （查询姓中第二个为下划线的员工信息）</strong>
      </li>
      <li>between and<br>
      SELECT * FROM employees WHERE department_id BETWEEN 90 AND 110;（查询部门id在90到110之间的员工信息）<br>
      等价于 SELECT * FROM employees WHERE department_id >=90 AND department_id <= 110;<br>
      <strong>注意，between and关键字的区间是包含两个区间端点的，并且一定要有上下限之分，不可颠倒顺序（不会报错，但是结果不是预期）</strong></li>
      <li>in（判断某字段的值是否属于列表的某一项）<br>
      SELECT job_id,last_name FROM employees WHERE job_id IN ('AD_VP', 'IT_PROT', 'AD_PRES');（查询员工工种编号为AD_VP IT_PROT AD_PRES中的一个员工的工种编号与姓）<br>
      等价于 SELECT job_id,last_name FROM employees WHERE job_id = 'AD_VP' OR job_id = 'IT_PROT' OR job_id = 'AD_PRES';<br>
      <strong>注意，要求值与列表的类型一致,且值不支持通配符</strong>
      </li>
      <li>is null<br>
      注意一个有趣的现象，我们查询奖金为NULL的员工信息时，很自然地想到语句<br>
      SELECT * FROM employees WHERE commission_pct = NULL;但是却没有结果，核心原因在于=不能用于判断NULL值<br>
      正确语句为SELECT * FROM employees WHERE commission_pct IS NULL;  （查询奖金为NULL的员工信息）<br>
      该语句可以在IS跟NULL之间加入NOT关键字来查询非NULL值<br>
      如SELECT * FROM employees WHERE commission_pct IS NOT NULL;（查询非NULL）
      </li>
      <li>安全等于<=>（补充内容，可以用于判断是否等于NULL或普通类型的值）<br>
      如SELECT * FROM employees WHERE commission_pct <=> NULL;  <br>
      但是会降低可读性
      </li>
    </ol>
  </li>
</ol>




