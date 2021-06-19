# sql-language in week1(using MySQL)
## Basic concept：<br>
 DB（Database）  
 数据库，储存数据的“仓库”,他保存了一系列有组织的数据。  
 DBMS（Datebase Management System）  
 数据库管理系统，又称为数据库管理软件，DB是通过DBMS创建和操作的容器，可用于管理DB中的数据。  
 SQL（Structure Query Language）  
 结构化查询语言，专门与数据库通信的语言。  
 SQL的优点：  
 <ol>  
 <li>不是某种特定数据库供应商专有的语言，几乎所有的DBMS都支持SQL。</li>  
 <li>是一种强有力的语言，可以借由其他语言元素，进行复杂和高级的数据库操作。</li>  
 </ol>
 
## How it store data
  <ol>
  <li>将数据放入表中，再由表放入库中。</li>
  <li>一个数据库可以有多个表，每个表有一个名字用于标识自己，表名具有唯一性</li>
  <li>表名具有一些特性，如定义数据在表中如何存储，类似于java“类”的设计。</li>
  <li>表由列组成，称为字段。所有表都由一个或者多个列组成，每一列类似java中的“属性”。</li>
  <li>表中的数据按行存储，每一行类似于java中的“对象”。</li>
  </ol>
    
## Download mysql  
  DBMS分为两类：
  <ol>
  <li>基于共享文件系统的DBMS（如：Access）</li>
  <li>基于客户机——服务器的DBMS（如：MySQL、Oracle、SqlServer）</li>
  </ol>
  此处我们选择mysql学习，因为mysql具有如下几个优点：
  <ol>
  <li>开源，成本低。可免费使用。</li>
  <li>性能高，执行快。</li>
  <li>简单，容易安装及使用。</li>
  </ol>
  附：Oracle上的mysql下载链接(https://dev.mysql.com/downloads/mysql/)<br>
  下载选择：
  <img src="C:\Users\Rose\Desktop\每周blog\2021.6.19\1624077416(1).jpg" alt="a picture" title="choose this">
    
    
