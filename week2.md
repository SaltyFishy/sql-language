# sql-learning in week2(using MySQL)
<h2>Basic Grammer in MySQL</h2>
<ol>
  <li>mysql的操作指令结尾需要加;或\g（除了exit,\g用于cmd窗口）</li>
  <li>mysql的指令对大小写不敏感，但是建议对表名、列名用小写，对关键字用大写。</li>
  <li>对于命令，如果太长，可以缩进、换行，用分号作为结尾。</li>
  <li>单行注释：
    <ol>
    <li>#注释文字</li>
    <li>-- （注意有一个空格）注释文字</li>
    </ol>
  </li>
  <li>多行注释：/*  注释文字 */</li>
</ol>
<h2>图形化用户界面</h2>
这里我们使用sqlyog
简单的登录界面：
<img src="https://github.com/SaltyFishy/sql-language/blob/week2/sqlyog.jpg" alt="sqlyog">
进入操作界面：
<img src="https://github.com/SaltyFishy/sql-language/blob/week2/sqlyog%E7%95%8C%E9%9D%A2.jpg" alt="sqlyog界面">
左边最上面表示主机，下面有四个数据库，点击“+”可以查看数据库下面的表。<br>
在右边“查询”处下可写代码，在sqlyog中会自动将关键字大写，输入完代码加入分号后可点击此处执行，或使用快捷键F9。
<img src="https://github.com/SaltyFishy/sql-language/blob/week2/sqlyog%E7%AE%80%E5%8D%95%E6%93%8D%E4%BD%9C.jpg" alt="sqlyog简单操作">
保存/生成sql文件以保存代码 快捷键：ctrl+s 文件保存格式为.sql 效果如图：
<img src="https://github.com/SaltyFishy/sql-language/blob/week2/sql%E6%96%87%E4%BB%B6%E4%BF%9D%E5%AD%98.jpg" alt="sql文件保存">
保存后可以通过“文件-打开-选择你要打开的对应的sql文件”方式打开对应的sql文件，还原代码。
调整字体：“工具-首选项-字体编辑器设置” 也可用ctrl+鼠标滚动键调整字体大小，此处不做演示。
<h2>SQL语言学习</h2>
<ol>
  <li>DQL(Data Query Language)语言，主要用于查询。</li>
  <li>DML（Data Manipulation Language）语言，主要用于增删改。</li>
  <li>DDL（Data Defination Language）语言，库与表的操作核心语言。</li>
  <li>TCL（Transaction Control Language）语言，事务与事务的管理。</li>
</ol>
<h3>DQL语言学习</h3>
学前准备:导入一个合适的脚本。
导入脚本操作：<ol>
  <li>ctrl+shift+Q或鼠标右键主机，单击执行SQL脚本。</li>
  <li>选择对应的SQL脚本。</li>
  <li>单击执行或F9。</li>
  <li>F5刷新。</li>
</ol>
<h4>进阶1 基础查询</h4>
<img src="https://github.com/SaltyFishy/sql-language/blob/week2/%E5%9F%BA%E7%A1%80%E6%9F%A5%E8%AF%A2.png" alt="基础查询">
语法：
select 查询列表（可不止一个） from 表名;<br>
此语句类似于java中的System.out.println();<br>
特点:<ol>
  <li>1.查询列表可以为：表中的列（字段），函数，常量，表达式</li>
<li>2.查询的结果为虚拟的表格</li>
</ol>
实例:<ol>
<li>1.查询表中的（单个）字段<br>
  如select email from employees;</li>
<li>2.查询表中的多个字段（注意：列的顺序可任意，只需要名字正确即可）<br>
  如select email,salary from employees;</li>
<li>3.查询表中的所有字段（使用了*则虚拟表的顺序与原表中一致）<br>
  如select * from employees;</li>
</ol>


<strong>备注：本人按照bilibili视频(https://www.bilibili.com/video/BV1xW411u7ax?p=18) 学习，此文档仅用作个人笔记&记录之用。</strong>
