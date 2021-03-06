---
title: MySQL基础笔记
date: 2020-06-09 00:22:46
tags: 
- MySQL
urlname: mysql-basic
category: 笔记
description:  MySQL基础：增删改查、库表管理、事务控制
---



B站视频教程：[MySQL 基础+高级篇- 数据库 -sql -尚硅谷](https://www.bilibili.com/video/BV12b411K7Zu?p=1)
MySql8.0：[MySql安装](https://zhuanlan.zhihu.com/p/37152572)
Navicat 15： [Navicat注册机](https://www.ghpym.com/navicatpatchdoc.html)

DB/DBMS

Sql/MySql

| 分类                                | 中文         | 语法                    |
| ----------------------------------- | ------------ | ----------------------- |
| DQL（Data Query Language）          | 数据查询语言 | select                  |
| DML  (Data Manipulate Language)     | 数据操作语言 | insert 、update、delete |
| DDL（Data Define Languge）          | 数据定义语言 | create、drop、alter     |
| TCL（Transaction Control Language） | 事务控制语言 | commit、rollback        |

# DQL 查询

## 基础查询

```mysql

/*
语法：
SELECT 查询列表 FROM 表名；

特点：
1.查询列表可以是：表中的字段、常量值、表达式、函数
2.查询的结果是一个虚拟的表格
*/

USE myemployees;

#1. 查询单个字段
SELECT email FROM employees;

#2. 查询多个字段
SELECT email, last_name FROM employees;

#3. 查询全部字段
SELECT * FROM employees;

#4. 查询常量值
SELECT 100;
SELECT 'john';

#5.查询表达式
SELECT 100*98;

#6.查询函数
SELECT VERSION();

#7.字段起别名
/*
1.便于理解
2.如果要查询的字段有重名，可以区分开
*/
#方式一：使用as
SELECT 100*98 AS 结果;
SELECT last_name AS 姓,first_name AS 名 FROM employees;

#方式二：使用空格
SELECT last_name 姓,first_name 名 FROM employees;

#案例：查询salary，显示结果为out put,含有关键词或者空格
SELECT salary  AS "out put" FROM employees;

#8.去重

#案例：查询员工表中中涉及到的所有的部门编号
SELECT DISTINCT department_id FROM employees;

#9.+号的作用
/*
mysql中的+号：
仅仅只有一个功能：运算符

1.两个操作数都为数值型，则做加法运算
  SELECT 100+90; 

2.其中一方为字符型，试图将字符型数值转换成数值型
- SELECT '123'+90;   如果转换成功，则继续做加法运算
- SELECT 'John'+90;  如果转换失败，则将字符型数值转换成0
- SELECT null +10;   只要其中一方为null，则结果肯定为null；
*/
# 查询员工们和姓连接过程一个字段
SELECT last_name+first_name AS 姓名 FROM employees;
SELECT CONCAT(last_name,' ', first_name) AS 姓名 FROM employees;

#显示表的结构，并查询全部数据
DESC departments;
SELECT * FROM departments;

#显示出employees的全部列，各个列之间用逗号连接，列头为output
SELECT 
IFNULL(commission_pct,0) AS 奖金率,commission_pct
FROM
employees;
#----------------------------------------------------
SELECT 
CONCAT(first_name,' ',last_name,',',IFNULL(commission_pct,0)) AS output
FROM
employees;
```
## 条件查询

```mysql

/*
语法：
SELECT 查询列表
FROM 表名 
WHERE 筛选条件；

分类：
1. 按条件表达式筛选
条件运算符：> < = != or <> >= <=

2.按逻辑表达式筛选
逻辑运算符：用于连接条件表达式
							&& || ！
						and or not
	and: 两个条件都为true
	or: 一个条件为true，则为true
	not：相反
	
3.模糊查询
like
between and
in
is null
*/
#1.按条件表达式筛选
#案例1：查询工资>12000的员工信息
SELECT
*
FROM
employees
WHERE
salary>12000;
#案例2：查询部门编号不等于90的员工名和部门编号
SELECT
last_name, department_id
FROM 
employees
WHERE
department_id!=90 ;

#2.按逻辑表达式筛选

#案例1：查询工资在10000-20000的员工名、工资、及奖金
SELECT 
CONCAT(first_name,' ',last_name) AS 'name',
salary,
IFNULL(commission_pct,0) AS comission
FROM
employees
WHERE
salary>=10000 AND salary<=20000;

#案例2：查询部门编号不在90-110之间，或者工资高于15000的员工信息
SELECT
*
FROM
employees
WHERE
department_id<90 OR department_id>110 OR salary>=15000;

#3.模糊查询
/*
like 
特点：
一般和通配符搭配使用
					通配符：
						% 任意多个字符，包含0个字符
						_ 任意单个字符
						\ 转义字符
between and
in
is null
is not null
*/
#1.like

#案例1：查询员工名中包含字符a的员工信息

SELECT
*
FROM
employees
WHERE
last_name LIKE '%a%';

#案例2：查询员工名中第三个字符为n，第五个字符为l的员工名和工资
SELECT
last_name,salary
FROM
employees
WHERE
last_name like '__n_l%';

#案例3：查询员工名中第二个字符为_的员工信息
SELECT
last_name
FROM
employees
WHERE
last_name LIKE '_a_%' ESCAPE 'a';
# 效果一样last_name LIKE '_\_%';

#2. between and 
#case 1: 查询员工编号在100-120的员工信息
SELECT
*
FROM
employees
WHERE
employee_id BETWEEN 100 AND 120; #包含临界值，不可以颠倒临界值顺序

#3. in
/*
判断某字段的值是否属于in列表中的某一项
-提高简洁度
-in列表的值类型必须一致或兼容
-不支持通配符的使用
*/
# case：查询员工的工种编号是IT_PROG、AD_VP、AD_PRES中的员工名和工种编号
SELECT
last_name,
job_id
FROM
employees
WHERE 
job_id ='IT_PROG' OR job_id='AD_VP' OR job_id='AD_PRES';
#-------------------------------------------------------
SELECT
last_name,
job_id
FROM
employees
WHERE 
job_id IN('IT_PROG','AD_VP' ,'AD_PRES');

#4 is null/ is not null
/*
= <>不能判断null值，is/ is not 才可以判断null值
*/
# case1:查询没有奖金的员工名和奖金率
SELECT
last_name,
commission_pct
FROM
employees
WHERE
commission_pct is NULL;	

#安全等于<=> 判断null值和普通数值
# case1:查询没有奖金的员工名和奖金率
SELECT
last_name,
commission_pct
FROM
employees
WHERE
commission_pct <=> NULL;	

#case2:查询工资为12000的员工信息
SELECT
last_name,
salary
FROM
employees
WHERE
salary <=> 12000;	

#查询员工号为176的姓名和部门号和年薪
SELECT
last_name,
department_id,
salary*12*(1+IFNULL(commission_pct,0)) AS 'annual salary'
FROM
employees
WHERE
employee_id=176;

#测试题
#1.查询没有奖金，且工资小于18000的salary,last_name
SELECT
salary,
last_name
FROM
employees
WHERE
commission_pct is NULL 
AND salary<18000;

#2.查询employees表中，job_id不为‘IT’或者工资为12000的员工信息
SELECT
*
FROM
employees
WHERE
job_id <>'IT' OR salary=12000;

#3.查询departments的结构
DESC departments;

#4.查询departments表中涉及到了哪些位置编号
SELECT DISTINCT
location_id
FROM
departments;

/*5.SELECT * FROM employees 和
SELECT * FROM employees WHERE commission_pct like'%%' AND last name like '%%'
结果是否一样，原因
不一样，值存在null的情况
*/
```
## 排序查询

```mysql
/*
SELECT * from employees;

语法：
SELECT 列表
FROM 表
WHERE 筛选条件
ORDER BY 排序列表 DESC/ASC
 1.ORDER BY子句支持单个字段、多个字段、表达式、函数、别名
 2.ORDER BY子句一般是放在查询语句的最后面，limit子句除外
*/

#case 1：查询员工信息，要求工资从高到低排序
SELECT
*
FROM
employees
ORDER BY
salary
DESC;

#------------------------------升序是默认的，可以不用写出
SELECT
*
FROM
employees
ORDER BY
salary
ASC;

#case 2：查询部门编号>=90的员工信息，按入职时间的先后（筛选条件）
SELECT
*
FROM
employees
WHERE
department_id>=90
ORDER BY
hiredate;

#case 3：按年薪的高低显示员工信息（按表达式排序）
SELECT
*,salary*12*(1+IFNULL(commission_pct,0)) 'annual salary' 
FROM
employees
ORDER BY
salary*12*(1+IFNULL(commission_pct,0)) DESC;

#case 4:按年薪的高低显示员工信息（按别名排序）
SELECT
*,salary*12*(1+IFNULL(commission_pct,0)) 'annual salary' 
FROM
employees
ORDER BY
'annual salary' DESC;

#case 5:按姓名的长度显示员工的姓名和工资（按函数）
SELECT 
LENGTH(last_name),last_name,salary
FROM
employees
ORDER BY LENGTH(last_name) DESC;

#case 6:查询员工信息，先按工资排序，再按员工编号排序(按多个字段排序）
SELECT
*
FROM
employees
ORDER BY salary ASC,employee_id DESC;

#1.查询员工的姓名、部门号、年薪，按年薪降序，按姓名升序
SELECT
last_name,
department_id,
salary*12*(1+IFNULL(commission_pct,0)) 'annual salary'
FROM
employees
ORDER BY
'annual salary' DESC,last_name;

#2.选择工资不在8000-17000的员工的姓名和工资，按工资降序
SELECT
last_name,salary
FROM
employees
WHERE
#salary<8000 OR salary>17000
#NOT (salary BETWEEN 8000 AND 17000) 
salary NOT BETWEEN 8000 AND 17000
ORDER BY
salary DESC;

#3.查询邮箱中包含e的员工信息，按邮箱字节数降序，按部门号升序
SELECT 
*,LENGTH(email)
FROM
employees
WHERE
email like '%e%'
ORDER BY
LENGTH(email) DESC,department_id;
```

## 常见函数

```mysql

/*
概念：一组逻辑语句封装在方法体重，对外暴露方法名
1.隐藏了实现细节
2.提高代码的重用性
调用：SELECT 函数名(实参列表) [from 表]；如果调用了表内的字段
特点：1.函数名 2.函数功能
函数可以自定义
分类：
			1.单行函数：CONCAT、LENGTH，IFNULL等，做处理使用
			2.分组函数：做统计使用，传一组值，返回一个值，又称为统计函数、聚合函数、组函数
*/

#1. 单行函数
/*
	字符函数 concat substr instr length lpad rpad upper lower trim replace
	数学函数  ceil floor truncate mod round
	日期函数 now curdate curtime year month day hour minute second  str_to_date date_format monthname
	其他函数 version datebase user
	流程控制函数 if case的两种用法
*/
			
#一、字符函数

#1.length 获取参数值的字节个数
SELECT LENGTH('john');
SELECT LENGTH('啊'); #utf8字符集，汉字占3个字节

SHOW VARIABLES LIKE '%char%' #展示字符集

#2.concat 拼接字符串
SELECT CONCAT(last_name,'_',first_name) 'name'
FROM employees;

#3.upper、lower 改变大小写
SELECT UPPER('john');
SELECT LOWER('jOHN');
# e.g. 将姓大写，名小写,然后拼接
SELECT
CONCAT(UPPER(last_name),' ',LOWER(first_name)) 'NAME' #函数可以嵌套
FROM employees;

#4.substr、substring 截取字符串
SELECT SUBSTR('annihilation',3);   #索引都是从1开始的， nihilation
SELECT SUBSTR('annihilation',1,3); #ann 索引，长度

#case: 姓名中首字符大写，其他小写后用_拼接
SELECT 
CONCAT(UPPER(SUBSTR(last_name,1,1)),'_',LOWER(SUBSTR(last_name,2)))
FROM
employees;

#5.instr 返回子串在字符串中的第一次出现的起始索引
SELECT INSTR('annihilation','nihilation');

#6.trim  去除两端的字符
SELECT LENGTH(TRIM('  annihilation  '));
SELECT TRIM('a' FROM 'aannihilationa'); #输出nnihilation

#7.lpad 用指定字符左填充指定长度
SELECT LPAD('annihilation',20,'-') AS output; #--------annihilation
SELECT LPAD('annihilation',2,'-') AS output;  #an

#8.rpad 用指定字符右填充指定长度
SELECT RPAD('annihilation',20,'-') AS output; #annihilation--------

#9.replace 替换
SELECT REPLACE('abc','a','z');

#二、数学函数

#round 四舍五入
SELECT ROUND(1.65);  #2
SELECT ROUND(1.45);  #1
SELECT ROUND(-1.65); #-2
SELECT ROUND(1.657,2); #1.66 保留2位

#ceil 向上取整，返回>=该参数的最小整数
SELECT CEIL(1.52);  #2
SELECT CEIL(1.00);  #1 
SELECT CEIL(-1.02); #-1 

#floor 向下取证，返回<=该参数的最大整数
SELECT FLOOR(9.8)   #9
SELECT FLOOR(-9.8)  #-10

#truncate 截断
SELECT TRUNCATE(1.699,1)  #1.6

#mod 取余
/*
mod(a,b): a-a/b*b  (其中a/b是取整数）
*/
SELECT MOD(10,3); #1
SELECT 10%3;
SELECT MOD(-10,3); #-1  根据被除数的正负，取正负


#三、日期函数

#now 返回当前系统日期及时间 2020-06-10 17:10:22
SELECT NOW();

#curdate 返回当前系统日期   2020-06-10
SELECT CURDATE(); 

#curtime 返回当前时间   17:12:15
SELECT CURTIME();

#获取指定的部分：年月日、时分秒
SELECT YEAR(NOW());
SELECT YEAR('1900-1-1');
SELECT YEAR(hiredate) FROM employees;

SELECT MONTH(NOW());     # 6
SELECT MONTHNAME(NOW()); # June

#DAY(date),HOUR(time),MINUTE(time),SECOND(time)

#STR_TO_DATE(str,format) 将字符串转换成指定格式的日期
/*格式符
%Y  20xx
%y  xx
%m  01,02...12
%c  1,2...12
%d  01,02...31
%H  01,02...24
%h  01,02...12
%i  00,01...59
%s  00,01...69
*/
SELECT STR_TO_DATE('1900-01-01','%Y-%m-%d');

#case 查询入职日期1992-4-3的员工信息
SELECT *FROM employees
WHERE hiredate= STR_TO_DATE('4-3 1992','%c-%d %Y');

#DATE_FORMAT(date,format) 将日期转换成字符
SELECT DATE_FORMAT('2008/1/1','%Y年%m月%d日');

#四、其他函数
SELECT VERSION();
SELECT DATABASE();
SELECT USER();

#五、流程控制函数
#1.if函数：if else的效果
SELECT IF(10>5,'大','小') ; #expr1条件表达式，true返回expr2，false返回expr3
SELECT last_name,commission_pct, IF(commission_pct is null,'no','yes') # if前面要加逗号
FROM employees;

#2.case函数的使用一：switch case的效果
/*
java中 
switch(变量或表达式）{
       case 常量1：语句1；break；
			 ...
			 default:语句n;break;
			 
			 }
			 
mysql中   判断等值
  CASE 要判断的变量或表达式						
	WHEN 常量1 THEN 要显示的值1或语句1； 
	WHEN 常量2 THEN 要显示的值2或语句2；
	...

	ELSE 要显示的值n或语句n；
  END 
#如果是在select后面，case作为表达式，只能显示为值，而不能用语句
*/

/*案例：查询员工的工资，要求
部门号=30，显示的工资为1.1倍
部门号=40，显示的工资为1.2倍
部门号=50，显示的工资为1.3倍
其他部门，显示为原工资
*/
SELECT salary,department_id,
  CASE department_id
	WHEN  30 THEN salary*1.1   #是个值，不用加分号
	WHEN  40 THEN salary*1.2 
	WHEN  50 THEN salary*1.3
	ELSE  salary
  END AS 'new salary'
FROM employees;

#3.case函数的使用二：类似于多重if
/*
java中：
if(条件1）｛
				语句1；
｝
else if(条件2）｛
				语句2；
｝
...
else{
				语句n;
}

mysql中：
case   														#后面没有变量或表达式，判断区间
when 条件1 then 显示值1 或语句1；
when 条件2 then 显示值2 或语句2；
...
else 显示值n或语句n；
end

*/
/*
#案例：查询员工的工资情况
如果工资>20000，显示A
如果工资>15000，显示B
如果工资>10000，显示C
否则D
*/
SELECT salary,
CASE
WHEN salary>20000 THEN 'A'   #字符串一定要加单引号
WHEN salary>15000	THEN 'B'
WHEN salary>10000 THEN 'C'
ELSE 'D'
END AS 'rank'
FROM employees;

#练习
#1.显示系统时间（时间+日期）
SELECT NOW();
#2.查询员工号、姓名、工资、以及工资提高20%后的结果
SELECT employee_id,last_name,salary,salary*1.2 as 'new salary'
FROM employees
#3.将员工的姓名按首字母排序，写出姓名长度
SELECT last_name,length(last_name) as length,substr(last_name,1,1) as initial
FROM employees
ORDER BY initial;
#区别于order by last_name 效果不一样，先首字母，后第二个字母。按initial排序，后面是随机的

#4.做一个查询，产生下面结果
/*<last_name> earns <salary> monthly but wants <salary*3>
Dream Salary
K_ing earns 24000.00 monthly but wants 72000.00
*/
SELECT CONCAT(last_name,' earns ',salary,' monthly but wants ', salary*3) as 'Dream Salary'
FROM employees
WHERE last_name='K_ing' and salary=24000;

#5. 使用case when，按照下面的条件
 job     grade
AD_PRES    A
ST_MAN     B
IT_PROG    C

SELECT last_name,job_id job, 
CASE job_id
WHEN 'AD_PRES' THEN 'A'  #字符串一定要加引号
WHEN 'ST_MAN'  THEN 'B'
WHEN 'IT_PROG' THEN 'C'
END AS grade
FROM employees
WHERE job_id='AD_PRES';


#二、分组函数
/*
功能：用作统计使用，又称聚合函数、统计函数、组函数
分类
sum
avg
max
min
count：计算非空的

特点：
1.忽略null
2.和dinstinct搭配使用
3.和分组函数一同查询的字段要求是group by后的字段
*/

#1.简单使用
SELECT SUM(salary) FROM employees;
SELECT AVG(salary) FROM employees;
SELECT min(salary) FROM employees;
SELECT MAX(salary) FROM employees;
SELECT COUNT(salary) FROM employees;

SELECT SUM(salary) 和,ROUND(AVG(salary),2) 平均,min(salary),MAX(salary),COUNT(salary)
FROM employees;

#2.参数支持类型
/*
sum avg：数值型
max min count ：字符型也可以做参数
*/

#3.是否忽略null
/*
null+任何数为null
sum avg:忽略
max min count ：忽略
*/

#4.搭配distinct  去重
SELECT 
SUM(DISTINCT salary),  #中间没有逗号
SUM(salary) 
FROM employees;

SELECT COUNT(DISTINCT salary) FROM employees;

#5.count函数的详细介绍
SELECT COUNT(*) FROM employees; #统计行数
SELECT COUNT(1) FROM employees; #加了1列1，统计行数
SELECT COUNT(2） FROM employees; 
SELECT COUNT('abc') FROM employees; #加了1列常量，统计行数

/*
效率：
MYISAM存储引擎，count（*）效率高
INNODB存储引擎，count（*）和count（1）相似，比count（字段）高
*/

#6.和分组函数一同查询的字段有限制
SELECT AVG(salary),employee_id FROM employees; # employee_id的值无意义

#查询最大入职时间和最小入职时间相差天数
SELECT DATEDIFF(NOW(),'1996-09-12') #前面减后面 8673

# 查询部门编号为90的员工个数
SELECT COUNT(department_id) #不能用count（department_id=90）,结果和count(department_id)一样
FROM employees;

SELECT COUNT(*)
FROM employees
WHERE department_id=90;


```

## 分组查询

```my sql	

#查询每个部门的平均工资
/*
SELECT column(要出现在group by后面, group_function()
FROM table
[WHERE 筛选条件]
GROUP BY  分组的列表
[ORDER BY 字句]

注意：
		查询列表必须特殊，要求是分组函数和group by后出现的字段	
		where一定放在from后面
		
特点：1.分组查询中的筛选条件分为两类
	|	 			|数据源				|位置					|关键字|
	|---------------|----------------------|-------------------|------|
	|分组前筛选       |原始表			 |group by子句前		    | where|
    |分组后筛选		 |分组后的结果集	  |group by子句后		     | having|
				
				分组函数（max min sum avg count）做条件肯定是放在having子句中
				能用分组前筛选的，优先考虑分组前筛选
				
			2.group by子句支持表达式，单个字段，多个字段（用逗号隔开）
			3.也可以添加排序，在整个分组查询的最后

*/

#简单的分组查询
# case 1:查询每个工种的最高工资
SELECT MAX(salary),job_id
FROM employees
GROUP BY job_id;

# case 2：查询每个位置上的部门个数
SELECT COUNT(DISTINCT department_id),location_id
FROM departments
GROUP BY location_id;

#推加筛选条件
# case 1：邮箱中包含a字符的，每个部门的平均工资
SELECT AVG(salary),department_id
FROM employees
WHERE email LIKE '%a%'
GROUP BY department_id;

# case 2:查询每个领导手下有奖金的员工的最高工资
SELECT MAX(salary),manager_id
FROM employees
WHERE commission_pct is not null
GROUP BY manager_id;

#添加复杂的筛选条件
#案例1：查询哪个部门的员工个数>2
#a.查询每个部门的员工数
SELECT COUNT(*), department_id
FROM employees
GROUP BY department_id;
#b.根据前面的结果进行筛选
SELECT COUNT(*), department_id
FROM employees
GROUP BY department_id
HAVING COUNT(*)>2;      #分号视作语句的结束，在最后面用

#case 2:查询每个工种有奖金的员工最高工资>12000的工种编号和最高工资
SELECT MAX(salary),job_id
FROM employees
WHERE commission_pct is not NULL
GROUP BY job_id
HAVING MAX(salary)>12000;

#case 3:查询领导编号>102的每个领导手下的最低工资>5000的领导编号及最低工资
SELECT MIN(salary), manager_id
FROM employees
WHERE manager_id>102
GROUP BY manager_id 
HAVING MIN(salary)>5000;


#按表达式或函数分组
# case：按员工姓名的长度分组，查询每一组的员工个数，筛选员工数>5的
SELECT COUNT(*),LENGTH(last_name)
FROM employees
GROUP BY LENGTH(last_name)
HAVING COUNT(*)>5;

#按多个字段分组
#case：查询每个部门每个工种的平均工资
SELECT AVG(salary),department_id,job_id
FROM employees
GROUP BY department_id,job_id; #可以调换顺序

#添加排序
#case：查询每个部门每个工种的平均工资，并且按照平均工资高低显示
SELECT AVG(salary),department_id,job_id
FROM employees
WHERE department_id is NOT NULL
GROUP BY department_id,job_id
HAVING AVG(salary)>10000     #having和group by支持别名
ORDER BY AVG(salary) DESC;

#1.查询个job_id员工工资的最大值、最小值、平均值、和，按job_id升序
SELECT MAX(salary),MIN(salary),AVG(salary),SUM(salary),job_id
FROM employees
GROUP BY job_id
ORDER BY job_id;

#2.最高最低工资的差距
SELECT MAX(salary)-MIN(salary)
FROM employees;

#3.查询各个管理者手下员工的最低工资，其中最低工资不能低于6000，没有管理者的员工不计入
SELECT MIN(salary),manager_id
FROM employees
WHERE manager_id is not null
GROUP BY manager_id
HAVING MIN(salary)>=6000;

#4.查询所有部门的编号，员工数量和工资平均值，按平均工资降序
SELECT department_id,COUNT(*) quantity,ROUND(AVG(salary),2) avgsal
FROM employees
GROUP BY department_id
ORDER BY AVG(salary) DESC;

#5.选择具有各个job_id的员工人数
SELECT COUNT(*),job_id
FROM employees
GROUP BY job_id;
```

## 连接查询

```mysql
/*
含义：多表查询，当查询的字段来自于多个表时使用
笛卡尔乘积现象：表1 m行，表2 n行，结果m*n行

原因：没有有效的连接条件
避免：添加有效的连接条件

分类：
			1.按年代分类：
			sql92标准：在mysql中仅仅支持内连接，在oracle等中支持部分外连接
			sql99标准（推荐）：支持内连接+外连接（左、右）+交叉连接
			
			2.按功能分类：
			内连接：
							等值连接
							非等值连接
							自连接
			外连接：
							左外连接
							右外连接
							全外连接
			交叉连接

*/
USE myemployees;

SELECT * FROM beauty;
SELECT * FROM boys;

SELECT name, boyName FROM beauty ,boys; 

SELECT name, boyName FROM beauty ,boys
WHERE beauty.boyfriend_id=boys.id;

#一、sql92标准

#1.等值连接
/*
a.多表等值连接为交集部分
b.n表连接，至少要n-1个连接条件
c.顺序无要求
d.一般要起别名
e.可以搭配排序、分组、筛选
*/
#case 1
SELECT name, boyName FROM beauty ,boys
WHERE beauty.boyfriend_id=boys.id;

#case 2:查询员工名和对应的部门名
SELECT last_name,department_name 
FROM employees,departments
WHERE employees.department_id=departments.department_id;

#2.为表起别名
#case 查询员工名、工种号、工种名
SELECT last_name,e.job_id,job_title #不知道job_id的来源。需要用表名限定
FROM employees e,jobs j
WHERE e.job_id=j.job_id;   #如果起别名就全部要用，统一

#3.两个表的顺序可以调换
SELECT last_name,e.job_id,job_title #用一个表一行行去对另一个表
FROM jobs j，employees e
WHERE e.job_id=j.job_id

#4.加筛选
# case 1 :查询有奖金的员工名、部门名
SELECT last_name,department_name
FROM employees e, departments d
WHERE e.department_id=d.department_id
AND e.commission_pct is not null;

# case 2:查询城市名中第二个字符为o的部门名和城市名
SELECT city,department_name
FROM departments d, locations l
WHERE d.location_id=l.location_id
AND city LIKE '_o%';

#5.加分组
#case 1：查询每个城市的部门个数
SELECT COUNT(*) amount ,city
FROM departments d, locations l
WHERE d.location_id=l.location_id
GROUP BY city;

#case 2:查询有奖金的每个部门的部门名和部门领导编号和该部门的最低工资
SELECT d.department_name, d.manager_id,min(salary)
FROM departments d,employees e
WHERE d.department_id=e.department_id
AND commission_pct is NOT NULL
GROUP BY e.department_id,d.manager_id;

#6.加排序
#案例：查询每个工种的工种名和员工的个数，按员工个数降序
SELECT job_title, COUNT(*)
FROM jobs j,employees e
WHERE j.job_id=e.job_id
GROUP BY job_title
ORDER BY COUNT(*) DESC;

#7.三表连接
#case 1： 查询员工名、部门名、所在的城市

SELECT last_name,department_name,city
FROM employees e, departments d, locations l
WHERE e.department_id=d.department_id
AND d.location_id=l.location_id;

#2.非等值连接
#case 1：查询员工的工资和工资级别
SELECT salary,grade_level
FROM employees e, job_grades g
WHERE salary BETWEEN lowest_sal and highest_sal
AND grade_level='A';

#3.自连接
#case：查询员工名和上级的名称
SELECT e.last_name,e.employee_id,m.last_name
FROM employees e,employees m 
WHERE m.employee_id=e.manager_id;

#显示员工表的最大工资，工资平均值
SELECT MAX(salary),AVG(salary)
FROM employees;

#查询 员工表的job_id中包含a和e，a在前
SELECT job_id
FROM employees
WHERE job_id like '%a%e%'

#Exercise

#1. 显示所有员工的姓名，部门号和部门名称。 
SELECT last_name,e.department_id,department_name
FROM employees e,departments d
WHERE e.department_id=d.department_id;

#2. 查询 90 号部门员工的 job_id 和 90 号部门的 location_id 
SELECT job_id,location_id,department_id
FROM employees e, locations l
WHERE department_id=90 ;  #不用连接条件会产生笛卡尔乘积问题
*-----------------------
SELECT job_id,location_id,e.department_id
FROM employees e, departments d
WHERE e.department_id=90 
AND e.department_id=d.department_id;


#3. 选择所有有奖金的员工的 last_name , department_name , location_id , city 
SELECT last_name,department_name,l.location_id,city
FROM employees e,departments d,locations l
WHERE commission_pct is not null
AND e.department_id=d.department_id
AND d.location_id=l.location_id;

#4. 选择city在Toronto工作的员工的 last_name , job_id , department_id , department_name
SELECT last_name,job_id,e.department_id,department_name,city
FROM employees e,departments d,locations l
WHERE city='Toronto'
AND e.department_id=d.department_id
AND d.location_id=l.location_id;   #三表连接，要写两个条件
 
#5.查询每个工种、每个部门的部门名、工种名和最低工资 
SELECT j.job_id,department_name,job_title,MIN(salary)
FROM jobs j, departments d,employees e
WHERE j.job_id=e.job_id
AND e.department_id=d.department_id
GROUP BY job_id;

#6.查询每个国家下的部门个数大于 2 的国家编号 
SELECT country_id, department_id,count(*)
FROM locations l,departments d
WHERE l.location_id=d.location_id
GROUP BY country_id
HAVING count(*)>2;
#7.择指定员工的姓名，员工号，以及他的管理者的姓名和员工号，结果类似于下面的格 式 
/*
employees Emp# manager Mgr# 
kochhar   101  king    100 
*/
SELECT e.last_name employees,e.employee_id 'Emp#',m.last_name manager,m.employee_id 'Mgr#'
FROM employees e, employees m
WHERE e.manager_id=m.employee_id
AND e.employee_id=101;

```



## 子查询

```mysql

#进阶7：子查询
/* 

含义：出现在其他语句内部的Select语句，称为子查询或内查询
查询的嵌套，外部的就是外查询（主查询）


分类
按子查询出现的位置：
			  SELECT :仅支持标量子查询
			  FROM    支持表只查询
（重点）WHERE/HAVING  标量子查询 列子查询 行子查询
				EXISTS （相关子查询（ 表子查询
按结果集的行列数不同：
		标量子查询：结果集只有一行一列
		列子查询  一列多行
		行子查询  一行多列
		表子查询  多行多列
			
*/

#一、WHERE或HAVING后
-- 1、标量子查询 单行
-- 2、列子查询  一列多行
-- 
-- 3、特点：
-- 子查询放在小括号内
-- 子查询一般放在条件的右侧
-- 标量子查询，一般搭配单行操作符使用 ，如 > < = <>
-- 
-- 列子查询 一般搭配多行操作符使用  in any/some all
-- 4、子查询是优先于主查询进行的


-- 案例1：谁的工资比Abel高？
# 查询出子查询
SELECT salary
FROM employees
WHERE last_name = 'Abel'

#作为主查询的条件
SELECT * 
FROM employees
WHERE salary > (
				SELECT salary
				FROM employees
				WHERE last_name = 'Abel'
);

#案例2 返回job_id与141员工相同，salary比143员工多的员工 的姓名 job_id 和工资
SELECT job_id
FROM employees
WHERE employee_id =141;

SELECT salary
FROM employees
WHERE employee_id = 143;

SELECT last_name,job_id,salary
FROM employees
WHERE salary > (
			SELECT salary
			FROM employees
			WHERE employee_id = 143

) AND job_id = 
(
			SELECT job_id
			FROM employees
			WHERE employee_id =141

);

#案例3 返回工资最少的员工的 last_name job_id salary

SELECT last_name,job_id,salary
FROM employees
WHERE salary = (
			SELECT MIN(salary)
			FROM employees
);


#案例4 查询最低工资大于50号部门最低工资的部门id和其最低工资
#选出每个部门的最低工资，大于子查询中 50号部门的最低工资

SELECT MIN(salary),department_id
FROM employees
GROUP BY department_id
HAVING MIN(salary)>(
			SELECT MIN(salary)
			FROM employees
			WHERE department_id = 50
);

   
#非法使用标量子查询
SELECT MIN(salary),department_id
FROM employees
GROUP BY department_id
HAVING MIN(salary)>(
			SELECT salary    #内部是个列子查询  多个值，没法比较
			FROM employees
			WHERE department_id = 50
);



#2、多行子查询(列子查询)  搭配多行比较操作符  一列多行
IN /NOT IN     等于列表中任意一个
ANY / SOME		和子查询返回的某一个值比较    使用较少 会使用MIN MAX等代替
ALL						和子查询返回的所有值比较      也可以替换为MIN MAX

#案例1 返回location_id是1400或1700部门中所有的员工姓名

#只要先查出1400 1700的部门编号就行，不用连接查询
SELECT last_name 
FROM employees
WHERE department_id IN(   #IN 可以替换成 = ANY   /  NOT IN  可以替换成 <>ALL
			SELECT DISTINCT department_id  #最好去重
			FROM departments
			WHERE location_id IN (1400,1700)
);


#案例2 返回其他部门中比  job_id为IT_PROG工种任一工资   低的员工的工号 姓名 jobid 及salary
#都可以是使用min max替代

SELECT employee_id, last_name, job_id,salary
FROM employees
WHERE salary < ANY(
		SELECT DISTINCT salary
		FROM employees
		WHERE job_id = 'IT_PROG'
);

#案例3 返回其他部门中比  job_id为IT_PROG工种所有工资   低的员工的工号 姓名 jobid 及salary

SELECT employee_id, last_name, job_id,salary
FROM employees
WHERE salary < ALL(
		SELECT salary
		FROM employees
		WHERE job_id = 'IT_PROG'
);


#3、行子查询（一行多列或者多行多列）   用的相对列子查询少

#案例 查询员工编号最小且工资最高的员工信息
#不一定存在，也可以使用上面的列子查询的 AND来做，效果是一致的
SELECT * 
FROM employees
WHERE (employee_id,salary) = (   #两个条件都放在小括号里，将多个字段当做一个虚拟的字段
		SELECT MIN(employee_id),MAX(salary)
		FROM employees
);		


#二、SELECT 后面  只能一行一列

# 案例查询每个部门的员工个数 
#和连接的效果一样
SELECT d.*,(
			SELECT COUNT(*)
			FROM employees
			WHERE e.department_id = d.department_id
			)
FROM departments d;

#查询员工号 102的部门名

SELECT (
		SELECT department_name
		FROM departments d
		INNER JOIN employees e
		ON e.department_id = d.department_id
		WHERE employee_id = 102
		) 部门名;


#三、FROM后面
#案例：查询每个部门的平均工资的工资等级

SELECT AVG(salary),department_id
FROM employees
GROUP BY department_id


SELECT ags.* ,grade_level
FROM (
		SELECT AVG(salary) ag,department_id
		FROM employees
		GROUP BY department_id) ags   #必须起别名，因为该表没有名字
INNER JOIN job_grades j
ON ags.ag BETWEEN lowest_sal AND highest_sal;


#四、exsits后面 相关子查询  boolean 子查询是不是有值 ,1为true
/*
exsits(完整的查询语句）
返回1,0

*/

SELECT EXISTS(SELECT employee_id FROM employees);

#查询有员工的部门名
SELECT department_name
FROM departments d
WHERE EXISTS(   #类似一个filter
		SELECT employee_id 
		FROM employees e
		WHERE d.department_id = e.department_id   #如果不加这句，就变成全选了，全都返回true
		);

SELECT department_name
FROM departments d
WHERE d.department_id IN(
		SELECT department_id
		FROM employees  #凡是有部门的员工 都有其对应的部门编号
		#WHERE employee_id IS NOT NULL
		)
		


SELECT * FROM boys   #boys. 前缀要加就都加
WHERE id NOT IN(
		SELECT boyfriend_id
		FROM beauty
)

-- 1. 查询和 Zlotkey 相同部门的员工姓名和工资 
SELECT last_name,salary
FROM employees
WHERE department_id = (
			SELECT department_id
			FROM employees
			WHERE last_name = 'Zlotkey'
			);

-- 2. 查询工资比公司平均工资高的员工的员工号，姓名和工资。 

SELECT employee_id,last_name,salary
FROM employees
WHERE salary>(
			SELECT AVG(salary)
			FROM employees
			);
			
-- 3. 查询各部门中工资比本部门平均工资高的员工的员工号, 姓名和工资 
SELECT employee_id,last_name,salary,e.department_id,avs.ag
FROM employees	e
 JOIN (    #INNER 可以省略 复习一下连接查询
			SELECT AVG(salary) ag,department_id
			FROM employees
			GROUP BY department_id
			) as avs  #这里要加分号的
ON e.department_id = avs.department_id	
WHERE e.salary>avs.ag;  #分组才用having

-- 4. 查询和姓名中包含字母 u 的员工在相同部门的  员工的员工号和姓名 
SELECT employee_id,last_name
FROM employees
WHERE department_id IN(
			SELECT DISTINCT department_id
			FROM employees
			WHERE last_name LIKE '%u%'  #不要写成is like
			);

-- 5.  查询在部门的 location_id 为 1700 的部门工作的员工的员工号
SELECT employee_id
FROM employees
WHERE department_id IN (
			SELECT department_id
			FROM departments
			WHERE location_id =1700
			);


-- 6.  查询管理者是 K_ing 的员工姓名和工资 

SELECT last_name,salary
FROM employees
WHERE manager_id IN (
			SELECT employee_id
			FROM employees
			WHERE last_name = 'K_ing'
			);
			
-- 7.  查询工资最高的员工的姓名，要求 first_name 和 last_name 显示为一列，列名为 姓.名 
SELECT CONCAT(first_name,last_name) '姓名'
FROM employees
WHERE salary = (
		SELECT MAX(salary)
		FROM employees
		);
		
	
#子查询案例	
-- 		 1. 查询工资最低的员工信息: last_name, salary  


-- 		 2. 查询平均工资最低的部门信息 

#和分组函数的一起查询字段的也是分组的后的

SELECT d.* 
FROM departments d  
JOIN (
		SELECT AVG(salary) avgsal,department_id
		FROM employees
		GROUP BY department_id
		) avs
ON d.department_id = avs .department_id
ORDER BY avs.avgsal
LIMIT 1;
		
-- 		 3. 查询平均工资最低的部门信息和该部门的平均工资  
SELECT d.*,avs.avgsal
FROM departments d
JOIN (
		SELECT AVG(salary) avgsal,department_id
		FROM employees
		GROUP BY department_id
		) avs
ON d.department_id = avs .department_id
ORDER BY avs.avgsal
LIMIT 1;

-- 		 4. 查询平均工资最高的 job 信息 
SELECT j.*
FROM jobs j
JOIN (
			SELECT AVG(salary) avs,job_id
			FROM employees
			GROUP BY job_id
			
) maxj
ON maxj.job_id = j.job_id
ORDER BY maxj.avs DESC
LIMIT 1;


-- 		 5. 查询平均工资高于公司平均工资的部门有哪些? 



SELECT department_name,avg.asgs
FROM departments d
INNER JOIN
(SELECT AVG(salary) asgs,department_id
	FROM employees
	GROUP BY department_id
	
	) as avg
ON avg.department_id = d.department_id  #存在department_id为null的员工

WHERE avg.asgs> (
		SELECT AVG(salary) FROM employees
);


	
	
-- 		 6. 查询出公司中所有 manager 的详细信息.  

SELECT *
FROM employees
WHERE employee_id IN (
		SELECT DISTINCT manager_id
		FROM employees
		);
-- 		 7. 各个部门中 最高工资中最低的那个部门的 最低工资是多少 

SELECT MIN(m.maxsal)
FROM(
		SELECT MAX(salary) maxsal
		FROM employees
		GROUP BY department_id
) m;

-- 		 8. 查询平均工资最高的部门的 manager 的详细信息: last_name, department_id, email, salary 
SELECT last_name,e.department_id,email,salary
FROM employees e
JOIN departments d
ON d.manager_id = e.employee_id
WHERE e.department_id = (

		SELECT department_id
		FROM employees
		GROUP BY department_id
		ORDER BY AVG(salary) DESC
		limit 1
) ;



-- 
```

## 分页查询

```mysq
#进阶8 分页查询  重要
/*

应用场景：当要显示的数据一页显示不全，需要分页提交sql请求
语法：SELECT  查询列表
			FROM table1
			【连接类型 JOIN table2
			ON 连接条件
			WHERE  筛选条件
			GROUP BY 分组字段
			HAVING  分组后的筛选
			ORDER BY 排序的字段】
			LIMIT OFFSET,SIZE   分页查询
			
			offset：要显示条目的起始索引，起始索引是0
			size：显示条目个数
			
特点：limit语句在查询语句的最后
			公式 显示的页数page 每页的条目size
			SELECT 
			FROM
			LIMIT (page-1)*size ,size;
*/
#案例1 查询前五条员工信息

SELECT * FROM employees LIMIT 0,5;
SELECT * FROM employees LIMIT 5;#从0开始可以简写

#案例2 查询11-25条员工信息
SELECT * FROM employees LIMIT 10,15;

#案例3 有奖金，工资前十名
SELECT *
FROM employees
WHERE commission_pct IS NOT NULL
ORDER BY salary DESC
LIMIT 10;


```

## 联合查询

```mysql
#进阶9 联合查询
/*
UNION 将多条查询语句的结果合并成一个结果

格式：
SELECT * FROM employees WHERE email LIKE '%a%'
UNION
SELECT * FROM employees WHERE department_id >90
UNION
...;

应用场景：
当涉及查多个表的时候，且没有连接关系，但是查的要素是一样的，
可以拆分查询，然后合成一个查询结果

特点：
1、多个查询语句查询的列数必须是一致的
2、字段名是第一条查询的字段名，所以要求多条查询的每一列类型和顺序是一致的
3、如果多个表中存在完全相同的数据，会去重。如需不去重 使用UNION ALL

*/

#查询部门编号>90 或邮箱含a的员工信息
SELECT * FROM employees WHERE email LIKE '%a%'
UNION
SELECT * FROM employees WHERE department_id >90;

```

# DML 增删改

```mySQL
# DATA MANIPULATE LANGUAGE
/*

INSERT 插
UPDATE 改
DELETE 删

*/

#一、插入语句
/*

方式一  用的多
语法：
INSERT INTO 表名(列名,...) VALUES(值...)


*/

#1.插入值的类型要与列的类型一致或兼容
INSERT INTO beauty(id,`name`,sex，borndate,phone,phone,boyfriend_id)
VALUES(15,'lucy','female','1999-1-1','18988898889',NULL,1)

#2.不可以为null的列必须插入值。可以为null的列可以插入null，或者
#字段和value都不写，会填入默认值，未设置默认值的就会填充null

#3.列的顺序可以随意，但值要一一对应

#4.列数和值得个数一定要一致

#5.可以省略列名，默认为所有列的默认顺序
INSERT INTO beauty
VALUES(16,'jessie','female','1999-1-1','18988898889',NULL,2)


/*

方式二
语法：
INSERT INTO 表名
SET 列名 = 值，列名 = 值，...

*/
INSERT INTO beauty 
SET id = 20,name = 'bope',phone = '3123';

#比较

#1.方式一支持插入多行
INSERT INTO beauty
VALUES(17,'jessie','f','1999-1-1','1898889888',NULL,2),
(18,'jessie2','f','1999-1-2','1898889288',NULL,2);

#2.方式一支持子查询
INSERT INTO beauty(id,name,phone)
SELECT 23,'joe','123433';

#二、修改语句
/*
1.修改单表的记录
语法：
UPDATE 表名
SET 列名 = 新值，列名 = 新值，...
WHERE 筛选条件;


2.修改多表的记录（补充）

sql92语法：
UPDATE 表名1 别名，表2 别名
SET 列名 = 新值，列名 = 新值，...
WHERE 连接条件
AND 筛选条件;

sql99语法：
UPDATE 表名1 别名
xxxJOIN 表2 别名
ON 连接条件
SET 列名 = 新值，列名 = 新值，...
WHERE 筛选条件;


*/
#1.修改单表的记录

#案例1 修改 姓唐的 电话为''
UPDATE beauty
SET phone = '23123213'
WHERE NAME LIKE 'j%';


#案例1 修改 boys id2 的名称和userC
UPDATE boys SET boyName = 'jack',userCP = 100
WHERE id = 1;


#2.修改多表的记录
#案例2 
UPDATE boys bo
INNER JOIN beauty b
ON bo.id = b.boyfriend_id
SET phone = '123132'
WHERE boyName = 'jack'


#
UPDATE boys bo
RIGHT JOIN beauty b 
ON bo.id = b.boyfriend_id
SET b.boyfriend_id = 2
WHERE b.boyfriend_id IS NULL;


#三、删除语句
/*
方式一：DELETE
语法：
1.单表的删除
DELETE FROM 表名 WHERE 筛选条件;

2.多表的删除
sql92语法
DELETE  表1别名，【表2别名】
FROM 表1 别名，表2 别名
WHERE 连接条件
AND  筛选条件

sql99
DELETE  表1别名，【表2别名】
FROM 表1
xxxJOIN 表2  ON 连接条件
WHERE 筛选条件

方式二：TRUNCATE
语法：
TRUNCATE TABLE 表名；



*/

#方式一：DELETE
#单表
DELETE FROM beauty WHERE phone LIKE '%9';

#多表删除，同多表update，按需要删除某个表或多个表的元素


#方式二：TRUNCATE,不能使用where

TRUNCATE TABLE boys;  #清空

#区别
/*
1.delete可以加where
2.truncate 效率略高
3.如果使用delete，再insert，自增长列(id自增长）的值从断点开始
 truncate则从1开始
4.truncate没有返回值 DELETE会返回 affected rows: int

5.truncate删除不可以回滚，DELETE可以回滚
*/

```

# DDL 库表

## 库表操作 

```MYSQL
#DDL DATA DEFINE LANGUAGE

/*
库和表的管理

一、库的管理
创建、修改、删除

二、表的管理
创建、修改、删除

三、关键字
create alter drop

*/

#一、库的管理
#1.创建库
CREATE DATABASE IF NOT EXISTS books;

#2.修改库  一般不修改
#更改字符集
ALTER DATABASE books CHARACTER SET gbk;

#3.删库
DROP DATABASE IF EXISTS books;
CREATE DATABASE IF NOT EXISTS books;

#二、表的管理
#1、表的创建
/*
CREATE TABLE 表名(
		列名 列类型【（长度）约束】,
		列名 列类型【（长度）约束】,
		列名 列类型【（长度）约束】,
		...
		列名 列类型【（长度）约束】
		)
		

*/

CREATE TABLE book(
				id INT,
				bookName VARCHAR(20),
				price DOUBLE,
				authorID INT,
				publishDate INT 
				
);

DESC book;

CREATE TABLE AUTHOR(
			id INT,
			au_name VARCHAR(20),
			nation VARCHAR(20)
);

#2、表的修改

#ALTER TABLE 表名 CHANGE/MODIFY/ADD/DROP COLUMN 列名 列类型 约束;
#1.修改列名
ALTER TABLE book CHANGE COLUMN publishDate pubDate TIMESTAMP;
#需要加上类型

#2.修改列类型或约束
ALTER TABLE book MODIFY COLUMN pubDate TIMESTAMP;

#3.添加新列
ALTER TABLE author ADD COLUMN annual DOUBLE;

#4.删除列
ALTER TABLE author DROP COLUMN annual;
#5.修改表名

ALTER TABLE author RENAME TO bookauthor;


#3、表的删除
/*
*/
DROP TABLE IF EXISTS book_author;
SHOW TABLE;

#4、表的复制

INSERT INTO author VALUES
(1,'a','japan'),
(2,'b','japan'),
(3,'c','japan'),
(4,'d','china');
 
 #1.仅复制表的结构
CREATE TABLE copyOfAuthor LIKE author;  
SELECT * FROM author;
SELECT * FROM copyOfAuthor;

 #2.复制表的 结构+数据
CREATE TABLE copyOfAuthor1
SELECT * FROM author;
SELECT * FROM copyOfAuthor1;

#3.复制表的 结构+部分数据
CREATE TABLE copyOfAuthor2
SELECT id,au_name FROM author
WHERE nation = 'japan';

#4.复制表的 部分结构
CREATE TABLE copyOfAuthor3
SELECT id,au_name FROM author
WHERE 0;
```

## 常见的数据类型

```mysql
#常见的数据类型

/*
数值型
			整型
			小数： 定点数  DEC(M,D)
						浮点数 FLOAT(M,D) DOUBLE(M,D)
字符型
			较短的文本：char varchar
			较长的文本：text blob（binary large object）
			
日期型：			

			
			

*/

#一、整型 TINYINT(8bit) SMALLINT(2byte) MEDIUMINT(3byte) INT/INTEGER(4byte) BIGINT(8byte)
/*
#特点：
1.默认是有符号
2.超出范围，默认插入临界值  out of range
3.如果不设置长度，会有默认的长度,长度为显示的宽度，ZEROFILL会用0填充
*/
#1.如何设置无符号和有符号


CREATE TABLE tab_int(
	t1 INT(7) ZEROFILL, #而且默认变成无符号
	t2 INT UNSIGNED #无符号
);

INSERT INTO tab_int 
SET t1 = 123,
t2 = 123;


-- #二、小数
/*
DEC(M,D)
FLOAT(M,D) DOUBLE(M,D)


# 
1.M 整数+小数长度  D保留D位小数
2.浮点的M D可以省略
3.DEC默认为 DEC(10,0) 不保留小数 四舍五入

*/
CREATE TABLE tab_float(
	f1 FLOAT(5,2),
	f2 DOUBLE(5,2),
	f3 DEC  #不要逗号
);

INSERT INTO tab_float VALUES
(100.00,100.00,100.90);

#选择简单类型，选择较小的够保存的


#三、字符型
/*

	较短的文本：
	char(M)    M:字符数 效率略高一些，默认为1可以省略
	varchar(M)  必须声明长度 可变长度，根据存储的数据的长度，开辟存储空间，节省空间
	
	较长的文本：
	text 
	blob（binary large object） JDBC
	
	enum型  规定了插入数据 只能是其枚举的类型,大小写不影响,只能是字符
	
	set型  集合,可以插入几个，都在一对''内
	
	BINARY
	
	
*/

CREATE TABLE enum(

		e1 enum('a','b','c')

		);
		
INSERT INTO enum VALUES
('A')

CREATE TABLE set1(

		e1 SET('a','b','c')

		);

INSERT INTO set1 VALUES
('a,b'),
('a'),
('a,b,c');


#四、日期型
/*
DATE  3BYTE
TIME  3
YEAR  1

DATETIME  8byte   1000-9999
TIMESTAMP 4byte  32位，只能表示2的32次方ms 当前时区
*/

CREATE TABLE chronicle(
			c1 DATETIME,
			c2 TIMESTAMP

);

INSERT INTO chronicle VALUES
(now(),now());

SELECT * FROM chronicle;

SHOW VARIABLES LIKE 'time_zone';

SET time_zone = '+9:00';
```

## 常见约束

```MYSQL
#常见约束

/*

含义： 一种限制，用于限制表中的数据，为了保证表中的数据的准确性和可靠性

分类：六种
			NOT NULL 		保证该字段的值不能为空
			DEFAULT 		保证字段有默认值
			PRIMARY KEY 主键 保证该字段的唯一性且非空
			UNIQUE 			保证字段的值唯一，可以为空
			CHECK  			MYSQL不支持 检查性别，检查年龄的范围
			FOREIGN KEY 限制两个表的关系，用于保证该字段的值必须来自主表的关联列的值
			            在从表添加外键约束，引用主表的值
添加约束的时机：
1.创建表时
2.修改表时

添加分类
		列级约束  语法上都支持，但外键约束没有效果
		表级约束  除了NOT NULL 和DEFAULT

添加列级约束：
直接在字段类型后面追加 NOT NULL、 DEFAULT、 PRIMARY KEY、 UNIQUE
			
添加表级约束：在字段最后  【CONSTRAINT 约束名】 约束类型(约束字段)


PRIMARY KEY 对比  UNIQUE
1.主键为非空，一个表中至多只有一个
2.UNIQUE只能有一个null，一个表可以有多个
3.都可以组合主键 组合唯一  就是组合中只要有一项不同就是不同的，但是不推荐使用

外键：
1.要求在从表设置外键关系
2.从表的外键列的类型必须和主表的关联列一致或兼容
3.要求主表的关联列必须是一个key （一般是主键或者唯一）
4.插入数据时，先插入主表，再插入从表；删除时先删除从表，后删除主表，因为有从表的引用

一个字段可以添加多个约束
*/

#一、创建表时添加约束
#1.添加列级约束

CREATE TABLE info(
			id INT PRIMARY KEY,
			s_name VARCHAR(20) NOT NULL,
			gender CHAR(1) CHECK(gender = '男'or gender = '女'),
			seat INT UNIQUE,
			age INT DEFAULT 18,
			majorId INT REFERENCES major(id)  #引用major表的列,列级约束无效

);

CREATE TABLE major(
			id INT PRIMARY KEY,
			majorName VARCHAR(20)
);

#查看所有的索引 
SHOW INDEX FROM info;


#2.添加表级约束
#在字段最后  【CONSTRAINT 约束名】 约束类型(约束字段)

CREATE TABLE info1(
			id INT ,
			s_name VARCHAR(20) ,
			gender CHAR(1) ,
			seat INT ,
			age INT ,
			majorId INT,
			
			CONSTRAINT pk PRIMARY KEY(id,s_name),
			CONSTRAINT uni UNIQUE(seat),
			CONSTRAINT ck CHECK(gender = '男'or gender = '女'),
			CONSTRAINT fk FOREIGN KEY(majorId) REFERENCES major(id)

);

#一般除了外键使用表级约束，其他都使用列级约束

#二、修改表时添加约束
#表级约束用ADD   列级用MODIFY

CREATE TABLE info2(
			id INT ,
			s_name VARCHAR(20) ,
			gender CHAR(1) ,
			seat INT ,
			age INT ,
			majorId INT
);

ALTER TABLE info2 MODIFY COLUMN s_name VARCHAR(20) NOT NULL;
ALTER TABLE info2 MODIFY COLUMN age INT DEFAULT 19;

#主键
ALTER TABLE info2 MODIFY COLUMN id PRIMARY KEY;
ALTER TABLE info2 ADD PRIMARY KEY(id); #只有添加表级约束才可以设置约束名，省略了【CONSTRAINT 约束名】

#UNIQUE
ALTER TABLE info2 MODIFY COLUMN seat UNIQUE;
ALTER TABLE info2 ADD UNIQUE(seat);

ALTER TABLE info2 ADD FOREIGN KEY(majorId) REFERENCES major(id);


#三、修改表时删除约束
ALTER TABLE info2 MODIFY COLUMN s_name VARCHAR(20) NULL;
ALTER TABLE info2 MODIFY COLUMN age INT;

ALTER TABLE info2 DROP PRIMARY;

#唯一 外键 +约束名
ALTER TABLE info2 DROP INDEX seat;
ALTER TABLE info2 DROP FOREIGN KEY fk;

```

## 标识列 （自增长列）

```MYS
#标识列

/*

自增长列

含义：系统提供的默认的序列值

1.标识列：必须是一个key

2.一个表只能有一个自增长列

3.标识列的类型：只能是数值型  一般是int

4.标识列可以设置步长 set auto_increment_increment = 3;  
可以修改起始值  在插入的时候自己先插入一个数，再插入null
*/

#一、创建表时设置标识列

CREATE TABLE indentity (
			id INT PRIMARY KEY AUTO_INCREMENT,
			NAME VARCHAR(20)
	
);
TRUNCATE TABLE indentity;
INSERT INTO indentity VALUES
(NULL,'john'),
(NULL,'john'),
(NULL,'john'),
(NULL,'john'),
(NULL,'john');

set auto_increment_increment = 3;  #修改步长

#起始值 在插入的时候自己先插入一个数，再插入null

#二、修改表时设置标识列

ALTER TABLE indentity MODIFY COLUMN id 	INT PRIMARY KEY AUTO_IMCREMENT;

#三、修改表时删除标识列
ALTER TABLE indentity MODIFY COLUMN id 	INT;

```

# TCL 事务控制 

```MYSQL
#TCL  TRANSACTION CONTROL LANGUAGE 事务控制语言

/*
事务：一个或一组语句组成一个执行单元，这个执行单元要么全部执行，要么全部不执行

a转账100给b
 

需要二者同时执行


存储引擎 ：	InnoDB存储引擎 支持事务

事务的特性：ACID属性
1.原子性 atomicity ：事务是一个不可分割的工作单位，事务的操作要么发生，要么不发生
2.一致性 consisentecy：事务会从一个一致性的状态进入另一个一致性的状态，比如转账一致
3.隔离性 isolation： 一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用数据
					对其他并发的事务是隔离的，并发执行的各个事务之间不能互相干扰
4.持久性 durability ：一旦提交了就是永久性的改变，接下来的其他操作和数据库故障不应该对其有影响


#事务的创建
隐式事务：事务没有明显的开启和结束的标记
比如 insert update delete

显式事务：事务具有明显的开启和结束的标记
前提：必须先设置自动提交功能为禁用

1.开启事务
SET autocommit = 0; 只针对当前事务
START TRANSACTION; 可选

2.编写sql语句
SELECT;
INSERT;
UPDATE;
DELETE;

3.结束事务
commit;		提交
二选一
rollback; 回滚  

前面的sql语句执行后驻留在内存中，当出现结束语句时，
如果是commit就提交修改，如果是rollback就不写出到磁盘。所以就还是原来的数据


update 表 set 余额 = 0 where name = 'a'
意外中断
update 表 set 余额 = 200 where name = 'b'

数据库的隔离级别
对于同时运行的多个事务，就当这些事务访问数据库中相同的数据时，如果没有
采用必要的隔离机制，就会导致各种并发问题


并发问题：
脏读：对于两个事务T1 T2, T1读取了T2更新但还没有没提交的字段之后，如果T2回滚
T1读取的内容就是临时且无效的。针对update

不可重复读：T1读取了一个字段，T2更新了该字段，T1再次读取同一字段，值就不同了

幻读： T1读取了一个字段，T2在该表中插入了一些新的行，T1再读会多出几行，针对insert

通过设置隔离级别来解决这些问题


*/

SHOW ENGINES;

SHOW VARIABLES LIKE 'autocommit';


```

