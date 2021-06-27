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
  <li>DQL（Data Query Language）语言，主要用于查询。</li>
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
<strong>查询时的一些细节注意:注意使用select指令的时候，需要在对应的库中执行，所以每当使用select命令时记得先USE 库名;</strong><br>
实例:<ol>
<li>1.查询表中的（单个）字段<br>
  如select email from employees;</li>
<li>2.查询表中的多个字段（注意：列的顺序可任意，只需要名字正确即可）<br>
  如select email,salary from employees;</li>
<li>3.查询表中的所有字段（使用了*则虚拟表的顺序与原表中一致）<br>
  如select * from employees;<br>
  tips:还可以直接双击对应的字段，会自动添加，此处添加的表名会带''，仅为了提升可读性（避免出现字段名与关键字同名时的二义性），可按F12进行格式规范化。</li>
<li>查询常量：select 常量;<br>
  如select 'join';</li>
<li>查询表达式：select 表达式;<br>
  如select 100+90;</li>
<li>查询函数：select 函数();<br>
  如select version()（调用该函数，并取其返回值）</li>
<li>起别名（对常量与表达式）：select 表达式、常量 as(可省略) 别名;<br>
   如select 100%98 as number;<br>
   起别名（对字段）：select 字段名 as(可省略) 别名 from 表名;<br>
   如select name as 名 from employees;<br>
  <strong>起别名很重要，能帮助理解，也能在查询字段出现重名时区分。</strong><br>
  <strong>当别名出现关键字时，可以用双引号""或''将别名括起。</strong></li>
 <li>去重：select distinct 字段名 from 表名;<br>
   如select distinct name from employees;（注意加没加distinct的不同在于，对于同一个name，没加distinct会多次输出，加了就只输出一次）</li>
  <li>+号的作用：<br>
  <strong>mysql中的+仅作为加法运算符，返回值为数字。<br>
    实例：
      <ol>
    <li>select 100+90;得到的结果为数字190;</li>
    <li>select '123'+90;会试图将字符转换为数字，再进行加法运算，若转换成功则得到结果：数字213。<br>
        转换失败如select 'join'+90;会将字符串转换为0，得到结果数字90。<br>
        若+前后至少有一个为null，得到结果必为null。</li>
      </ol></li></strong>
  <li>使用concat函数实现连接：select concat(字段名1,字段名2·······);（可以填入字符，用''括起）<br>
  如select concat(last_name,first_name) as 姓名 from employees;（将姓与名组合输出姓名）<br>
  <strong>null与任何字段拼接得到的结果必为null。为了避免此类错误，可以使用ifnull函数，方法为：ifnull(可能为null,当真的为null时返回何值)</strong>
  </li>
</ol>



<strong>备注：本人按照bilibili视频(https://www.bilibili.com/video/BV1xW411u7ax?p=18) 学习，此文档仅用作个人笔记&记录之用。</strong>
