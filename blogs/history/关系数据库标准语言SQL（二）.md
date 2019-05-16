---
title: 关系数据库标准语言SQL（二）
date: 2016-11-23 16:45:47
tags: [数据库,SQL]
categories: [读书笔记,数据库系统概论]
---
SQL集数据查询、数据操纵、数据定义和数据控制功能于一体，而在这四种功能中，数据查询是最核心的功能。
<!--more-->
数据查询是数据库的核心操作。SQL提供了SELECT语句进行数据查询：
```sql
SELECT [ALL|DISTINCT]<目标列表达式>[,<目标列表达式>]...
FROM <表名或视图名>[,<表名或试图名>...]|(<SELECT语句>)[AS]<别名>
[WHERE <条件表达式>]
[GROUP BY <列名1>[HAVING<条件表达式>]]
[ORDER BY <列名2>[ASC|DESC]];
```
整个SELECT语句的含义是，根据WHERE子句的条件表达式从FROM子句指定的基本表、视图或派生表中找出满足条件的元组，再按SELECT子句中的目标列表达式选出元组中的属性值形成结果表。如果有GROUP BY子句，则将结果按<列名1>的值进行分组，该属性列值相等的元组为一个组。通常会在每组中作用聚散函数。如果有GROUP BY子句带HAVING短语，则只有满足指定条件的组才予以输出。如果有ORDER BY子句，则结果表还要按<列名2>的值的升序或降序排序。
## 单表查询
单表查询是指仅涉及一个表的查询。
### 选择表中的若干列
（1）查询指定列
例如：
```sql
SELECT Sno,Sname
FROM Student;
```
<目标表达式>中各个列的先后顺序可以与表中的顺序不一致。
（2）查询全部列
有两种方法：一是列出所有名，二是简单地将<目标列表达式>指定为*。例如：
```sql
SELECT *
FROM Student;
```
（3）查询经过计算的值
SELECT子句的<目标表达式>不仅可以是表中的属性列，也可以是表达式。例如：
```sql
SELECT Sname,2016-Sage         /*查询结果的第2列是一个算术表达式*/
FROM Student;
```
<目标列表达式>不仅可以是算术表达式，还可以是字符串常量、函数等：
```sql
SELECT Sname,'Year of Birth:',2014-Sage,LOWER(Sdept)  /*LOWER()将字符串变成小写*/
FROM Student;
```
用户可以通过指定别名来改变查询结果的列标题，这对于含算术表达式、常量、函数名的目标列表达式尤为重要。
```sql
SELECT Sname NAME,'Year of Birth' BIRTH,2014-Sage BIRTHDAY,LOWER(Sdept) DEPARTMENT
FROM Student;
```
### 选择表中的若干元组
（1）消除取值重复的行
两个本来并不完全相同的元组在投影到指定的某些列上后，可能会变成相同的行，可以用DISTINCT消除他们。例如
```sql
SELECT DISTINCT Sno
FROM SC;
```
如果没有指定DISTINCT关键词，则默认为ALL，即保留结果表中取值重复的行。
（2）查询满足条件的元组
查询满足指定条件的元组可以通过WHERE子句实现。
①比较大小
用于比较的运算符一般包括=,>,<,>=,<=,!=或<>,!>,!<。例如
```sql
SELECT Sname
FROM Student
WHERE Sdept='CS';
```
②确定范围
谓词BETWEEN...AND...和NOT BETWEEN ...AND...可以用来查找属性值在（或不在）指定范围内的元组，其中BETWEEN后是范围的下限（即低值），AND后是范围的上限（即高值）。例如
```sql
SELECT Sname,Sdept,Sage
FROM Student
WHERE Sage BETWEEN 20 AND 23;
```
③确定集合
谓词IN可以用来查找属性值属于指定集合的元组，NOT IN用于查找属性值不属于指定集合的元组。例如
```sql
SELECT Sname,Ssex
FROM Student
WHERE Sdept IN ('CS','MA','IS');
```
④字符匹配
谓词LIKE可以用来进行字符串的匹配。
```sql
[NOT] LIKE '<匹配串>' [ESCAPE '<换码字符>']
```
其含义是查找指定的属性列值与<匹配串>相匹配的元组。<匹配串>可以是一个完整的字符串，也可以含有通配符%和_。其中：
- % 代表任意长度（可以为0）的字符串。例如a%b表示以a开头b结尾的任意长度的字符串。
- _ 代表任意单个字符。

例如：
```sql
SELECT Sname
FROM Student
WHERE Sname LIKE '欧阳_';
```
注意：数据库字符集为ASCII时一个汉字需要两个_；当字符集为GBK时只需要一个_。
如果用户要查询的字符串本身就含有通配符%和_，这时就要使用ESCAPE '<换码字符>'短语对通配符进行转义了。例如：
```sql
SELECT Cno,Ccredit
FROM Course
WHERE Cname LIKE 'DB\_Design' ESCAPE'\';
```
⑤涉及空值的查询
```sql
SELECT Sno,Cno
FROM SC
WHERE Grade IS (NOT) NULL;
```
⑥多重条件查询
逻辑运算符AND和OR可用来连接多个查询条件。AND的优先级高于OR，但用户可以用括号改变优先级。
```sql
SELECT Sname
FROM Student
WHERE Sdept='CS' AND Sage<20;
```
IN谓词实际上是多个OR运算符的缩写。
### ORDER BY子句
用户可以用ORDER BY子句对查询结果按照一个或多个属性列的升序（ASC）或降序（DESC）排列，默认值为升序。例如：
```sql
SELECT Sno,Grade
FROM SC
WHERE Cno='3'
ORDER BY Grade DESC;   /*查询结果按分数的降序排列*/
```
对于空值，排序时显示的次序由具体系统实现来决定。例如按升序排，含空值的元组最后显示；按降序排，空值的元组最先显示。各个系统的实现可以不同，只要保持一致就行。
```sql
SELECT *
FROM Student
ORDER BY Sdept,Sage DESC;   /*查询结果按所在系的系号升序排列，同一系中的学生按年龄降序排列*/
```
### 聚集函数
为了进一步方便用户，增强检索功能，SQL提供了很多聚集函数，主要有：
- COUNT(*)：统计元组个数
- COUNT([DISTINCT|ALL]<列名>)：统计一列中值的个数
- SUM([DISTINCT|ALL]<列名>)：计算一列值的总和（必须是数值型）
- AVG([DISTINCT|ALL]<列名>)：计算一列值的平均值（必须是数值型）
- MAX([DISTINCT|ALL]<列名>)：求一列值中的最大值
- MIN([DISTINCT|ALL]<列名>)：求一列值中的最小值

如果指定DISTINCT短语，则表示在计算时要取消指定列中的重复值。如果不指定DISTINCT短语或指定ALL短语（ALL为默认值），则表示不取消重复值。例如：
```sql
SELECT COUNT(DISTINCT Sno)
FROM SC;    /*查询选修的课程的学生人数*/
```
当聚集函数遇到空值时，除COUNT(*)外，都跳过空值而只处理非空值。COUNT(*)是对元组进行计数，某个元组的一个或部分列取空值不影响COUNT的统计结果。
注意：WHERE子句中是不能用聚集函数作为条件表达式的。聚集函数只能用于SELECT子句和GROUP BY中的HAVING子句。
### GROUP BY子句
GROUP BY子句将查询结果按某一列或多列的值分组，值相等的为一组。对查询结果分组的目的是为了细化聚集函数的作用对象。如果未对查询结果分组，聚集函数将作用于整个查询结果。分组后聚集函数将作用于每一个组，即每一个组都有一个函数值。例如：
```sql
SELECT Cno,COUNT(Sno)
FROM SC
GROUP BY Cno;   /*各个课程号及相应的选课人数*/
```
该语句对查询结果按Cno的值分组，所有具有相同Cno值的元组为一组，然后对每一组作用聚集函数COUNT进行计算，以求得改组的学生人数。查询结果可能为：

Cno|COUNT(Sno)
---|----------
1|22
2|34
3|43

如果分组后还要求按一定的条件对这些组进行筛选，最终只输出满足指定条件的组，则可以使用HAVING短语指定筛选条件。例如：
```sql
SELECT Sno
FROM SC
GROUP BY Sno
HAVING COUNT(*)>3;   /*查询选修了三门以上课程的学生学号*/
```
这里先用GROUP BY子句按Sno进行分组，再用聚集函数COUNT对每一组计数；HAVING短语给出了选择组的条件，只有满足条件（即元组个数>3，表示此学生选修的课超过3门）的组才会被选出来。
WHERE子句与HAVING短语的区别在于作用对象不同。WHERE子句作用于基本表或视图，从中选择满足条件的元组。HAVING短语作用于组，从中选择满足条件的组。
## 连接查询
若一个查询同时涉及两个以上的表，则称之为连接查询。连接查询是关系数据库中最主要的查询，包括等值连接查询、自然连接查询、非等值连接查询、自身连接查询、外连接查询和复合条件连接查询等。
### 等值与非等值连接查询
连接查询的WHERE子句用来连接两个表的条件称为连接条件或连接谓词，其一般格式为
```sql
[<表名1>.]<列名1><比较运算符>[<表名2>.]<列名2>
```
其中比较运算符主要有：=、>、<、>=、<=、!=（或<>）等。
此外连接谓词还可以使用下面形式：
```sql
[<表名1>.]<列名1> BETWEEN [<表名2>.]<列名2> AND [<表名3>.]<列名3>
```
当连接运算符为=时，称为**等值连接**。使用其他运算符称为**非等值连接**。连接谓词中的列名称为**连接字段**。连接条件中的各连接字段类型必须是可比的，但名称不必相同。例如：
```sql
SELECT Student.*,SC.*
FROM Student,SC
WHERE Student.Sno=SC.Sno;   /*将Student与SC中同一学生的元组连接起来*/
```
查询结果可能如图：

Student.Sno|Sname|Ssex|Sage|Sdept|SC.Sno|Cno|Grade
-----------|-----|----|----|-----|------|---|-----
201215121|李勇|男|20|CS|201215121|1|92
201215121|李勇|男|20|CS|201215121|2|85
201215121|李勇|男|20|CS|201215121|3|88
201215122|刘晨|女|19|CS|201215122|2|90
201215122|刘晨|女|19|CS|201215122|3|80
如果属性名在参加连接的各表中是唯一的，则可以省略表名前缀。
若在**等值连接中把目标列中重复的属性列去掉则为自然连接**。
一条SQL语句可以同时完成选择和连接查询，这时WHERE子句是由连接谓词和选择谓词组成的复合条件。例如
```sql
SELECT Student.Sno,Sname
FROM Student,SC
WHERE Student.Sno=SC.Sno AND          /*连接谓词*/
       SC.Cno='2' AND SC.Grade>90;    /*其他限定条件*/
```
### 自身连接
连接操作不仅可以在两个表之间进行，**也可以是一个表与其自己进行连接，称为表的自身连接**。例如：
查找每一门课的间接先修课（即先修课的先修课），在Course表中只有每门课的直接先修课信息，而没有先修课的先修课。为此，**要为Course表取两个别名，一个是FIRST，一个是SECOND。**
```sql
SELECT FIRST.Cno,SECOND.Cpno
FROM Course FIRST,Course SECOND
WHERE FIRST.Cpno=SECOND.Cno
```
### 外连接
外连接：保留悬浮数组到结果关系中。左外连接列出左边关系中所有的元组，右外连接列出右边关系中所有的元组。例如：
```sql
SELECT Student.Sno,Sname,Ssex,Sage,Sdept,Cno,Grade
FROM Student LEFT OUTER JOIN SC ON (Student.Sno=SC.Sno);
```
### 多表连接
连接操作除了可以是两表连接、一个表与其自身连接外，还可以是两个以上的表进行连接，后者通常称为**多表连接**。例如：
```sql
SELECT Student.Sno,Sname,Cname,Grade
FROM Student,SC,Course
WHERE Student.Sno=SC.Sno AND SC.Cno=Course.Cno;
```
## 嵌套查询
在SQL语言中，一个SELECT-FROM-WHERE语句称为一个**查询块**。将一个查询块嵌套在另一个查询块的WHERE子句或HAVING短语的条件中的查询称为**嵌套查询**。例如：
```sql
SELECT Sname
FROM Student         /*外层查询或父查询*/
WHERE Sno IN
         (SELECT Sno   /*内层查询或子查询*/
		 FROM SC
		 WHERE Cno='2');
```
SQL语言允许多层嵌套查询，即一个子查询中还可以嵌套其他子查询。需要特别指出的是，子查询的SELECT语句中不能使用ORDER BY子句，ORDER BY子句只能对最终查询结果排序。嵌套查询使用户可以用多个简单查询构成复杂的查询，从而增强SQL的查询能力。以层层嵌套的方式来构造程序正式SQL中“结构化”的含义所在。
### 带有IN谓词的子查询
在嵌套查询中，子查询的结果往往是一个集合，所以谓词IN是嵌套查询中最经常使用的谓词。例如查询某位同学在同一个系学习的学生：
分布查询：
①确定“刘晨”所在的系名
```sql
SELECT Sdept
FROM Student
WHERE Sname='刘晨';
```
②查找所有在该系学习的学生
```sql
SELECT Sno,Sname,Sdept
FROM Student
WHERE Sdept='CS';   /*假设第一步查询结果为CS*/
```
将第一步查询嵌入到第二步查询的条件中：
```sql
SELECT Sno,Sname,Sdept
FROM Student
WHERE Sdept IN
       (SELECT Sdept
	   FROM Student
	   WHERE Sname='刘晨');
```
本例中，子查询的查询条件不依赖于父查询，称为**不相关子查询**。如果子查询的查询条件依赖于父查询，这类子查询称为**相关子查询**，整个查询语句称为**相关嵌套查询**语句。
### 带有比较运算符的子查询
带有比较运算符的子查询是指父查询与子查询之间用比较运算符进行连接。当用户能确切知道内层查询返回的是单个值时，可以用>、<、=、>=、<=、!=或<>等比较运算符。例如
```sql
SELECT Sno,Cno         /*找出每个学生超过他自己选修课程平均成绩的课程号*/
FROM SC x
WHERE Grade>=(SELECT AVG(Grade)
			FROM SC y
			WHERE y.Sno=x.Sno);
```
x是表SC的别名，又称为元组变量，可以用来表示SC的一个元组。
### 带有ANY(SOME)或ALL谓词的子查询
子查询返回单值时可以用比较运算符，但返回多值时要用ANY（有点系统用SOME）或ALL谓词修饰符。而使用ANY或ALL谓词时则必须同时使用比较运算符。其语义如下所示：
- \>ANY：大于子查询结果中的某个值
- \>ALL：大于子查询结果中的所有值
- <ANY：小于子查询结果中的某个值
- <ALL：小于子查询结果中的所有值
- \>=ANY：大于等于子查询结果中的某个值
- \>=ALL：大于等于子查询结果中的所有值
- <=ANY：小于等于子查询结果中的某个值
- <=ALL：小于等于子查询结果中的所有值
- =ANY：等于子查询结果中的某个值
- =ALL：等于子查询结果中的所有值（通常没有实际意义）
- !=（或<>）ANY：不等于子查询结果中的某个值
- !=（或<>）ALL：不等于子查询结果中的任何一个值

例如查询非计算机科学系中比计算机科学系任意一个学生年龄小的学生姓名和年龄：
```sql
SELECT Sname,Sage
FROM Student
WHERE Sage<ANY(SELECT Sage
			FROM Student
			WHERE Sdept='CS')
AND Sdept<>'CS';     /*注意这是父查询块中的条件*/
```
也可以用聚集函数MAX先找出CS系中最大年龄，然后查找非CS系年龄小于20岁的学生。事实上，用聚集函数实现子查询通常比直接用ANY或ALL查询效率要高。
### 带有EXISTS谓词的子查询
带有EXISTS谓词的子查询不返回任何数据，只产生逻辑真值“true”或逻辑假值“false”。例如：
```sql
SELECT Sname        /*查询所有选修了1号课程的学生姓名*/
FROM Student
WHERE EXISTS(SELECT *
			FROM SC
			WHERE Sno=Student.Sno AND Cno='1');
```
使用存在量词EXISTS后，若内层查询结果非空，则外层的WHERE子句返回真值，否则返回假值。由于EXISTS引出的子查询，其目标列表达式通常都用“*”，因为带有EXISTS的子查询只返回真值或假值，给出列名无实际意义。
与EXISTS谓词相对应的是NOT EXISTS谓词。使用存在量词NOT EXISTS后，若内层查询结果为空，则外层的WHERE子句返回真值，否则返回假值。
一些带EXISTS或NOT EXISTS谓词的子查询不能被其他形式的子查询等价替换，但所有带IN谓词、比较运算符、ANY和ALL谓词的子查询都能用带EXISTS谓词的子查询等价替换。
## 集合查询
SELECT语句的查询结果是元组的集合，所以多个SELECT语句的结果可进行集合操作。集合操作主要包括并操作UNION、交操作INTERSECT和差操作EXCEPT。例如：
```sql
SELECT *
FROM Student
WHERE Sdept='CS'
UNION
SELECT *
FROM Student
WHERE Sage<=19;
```
注意：参加集合操作的各查询结果的列数必须相同；对应项的数据类型也必须相同。
## 基于派生表的查询
子查询不仅可以出现在WHERE子句中，还可以出现在FROM子句中，这时子查询生成的临时派生表成为主查询的查询对象。例如：
```sql
SELECT Sno,Cno
FROM SC,(SELECT Sno,Avg(Grade) FROM SC GROUP BY Sno)
		AS Avg_sc(avg_sno,avg_grade)
WHERE SC.Sno=Avg_sc.avg_sno AND SC.Grade>=Avg_sc.avg_grade;
```
这里FROM子句中的子查询将生成一个派生表Avg_sc。该表由avg_sno和avg_grade两个属性组成，记录了每个学生的学号及平均成绩。主查询将SC表与Avg_sc按学号相等进行连接，选出选修课成绩大于其平均成绩的课程号。
如果子查询中没有聚集函数，派生表可以不指定属性列，子查询SELECT子句后面的列名为其默认属性。例如：
```sql
SELECT Sname
FROM Student,(SELECT Sno FROM SC WHERE Cno='1') AS SC1
WHERE Student.Sno=SC1.Sno;
```
需要说明的是，通过FROM子句生成派生表时，AS关键字可以省略，但必须为派生表关系指定一个别名。而对于基本表，别名是可选择项。
## SELECT语句的一般格式
```sql
SELECT [ALL|DISTINCT] <目标表达式>[别名],<目标表达式>[别名],...
FROM <表名或视图名>[别名],<表名或视图名>[别名],...| (<SELECT语句>)[AS]<别名>
[WHERE <条件表达式>]
[GROUP BY <列名1> [HAVING <条件表达式>]]
[ORDER BY <列名2> [ASC|DESC]];
```

> 《数据库系统概论（第5版）》王珊 萨师煊 著
