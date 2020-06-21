---
title: Java学习 01
date: 2020-06-14 00:24:46
tags: Java
category: 笔记
description:  Java基础
---

# 1. Java语言

## 1.1 开发环境

JavaSE 9.04 
IDEA 2020.1.2

- JVM Java virtual machine 
Java的跨平台性
Java程序 运行在 JVM，JVM运行在不同的系统中
- JRE(Java runtime environment)
包含JVM和运行所需的核心类库
- JDK（Java development kit） 安装
程序开发工具包，包含JRE和开发人员使用的工具（编译器等）

JDK 5.0 8.0 升级较大 1.8就是8.0
JavaSE 桌面级（不再用） /JavaEE 企业级 Web开发 /JavaME

## 1.2 基础命令行指令

系统软件(操作系统）
应用软件 

人机交互方式
图形化界面 GUI graphical user interface
命令行 CLI command line interface
algorithms+data structures=programs

MS-DOS (Microsoft Disk operating system)
CMD
启动：win R cmd
切换盘符： 盘符：
进入文件夹 cd 文件夹名
进入多级文件夹：cd 文件夹1\文件夹2
返回上级 cd..
回根路径 cd\
查看文件夹：dir
清屏：cls
退出：exit
删除：del
上下箭头：历史操作命令
删除文件夹 ：rd  (remove dir 目录得是空的
创建目录：md

## 1.3 语言特点

机器语言
汇编语言
高级语言 
- 面向过程 C Pascal
- 面向对象 Java Python Scala
C 开发效率差，执行效率高→  Java  →  Python
严格的语法，丰富的类库 
PHP JS解释型语言

舍弃了C语言中容易引起错误的指针（以引用取代）、运算符重载、多重继承等（以接口取代），增加了垃圾回收期功能

- 面向对象：
  两个基本概念：类、对象
  三大特性：封装、继承、多态
- 健壮性
- 跨平台性  JVM
  编写（.java，在记事本就可以）、编译（.class 字节码文件 javac.exe编译器），运行（JVM运行 java.exe解释器）

垃圾回收：不再使用的内存空间
还是存在内存泄漏和内存溢出

Java web应用开发
后台开发：Java  PHP Python Go Node.js


Android系统结构
内核 linux kernel 和硬件交互
libraries  C
android runtime  C
application framework Java
applications Java
![Android系统结构.jpg](https://i.loli.net/2020/06/13/tgTAWRYlJnyPKvi.jpg)




```java
public class HelloWorld {   //定义一个类的名称，类是Java中所有源代码的基本组织单位
    public static void main(String[] args){ //内容是固定写法，代表main方法，代表程序执行的起点
        System.out.println("hello world!");  //打印输出语句，（）中即为显示的内容
    }
}
编译后的.class不包含注释

```
## 1.4 注释及API文档

单行注释、多行注释
文档注释:可以被JDK提供的javadoc工具解析，生成一套以网页文件形式体现的该程序的说明文档
 /** 
文档注释
@author xxx
@version v1.0 
*/
注意：多行注释不可以嵌套使用

Java API 文档
API application programing interface 类库

注意事项：
1. java程序编写-编译-运行的过程
   - 编写，以.java结尾的源文件
   - 编译，javac file.java 生成字节码文件
   - 运行   java 类名  运行解释字节码文件

2. 在一个java文件中可以声明多个class，但是最多只有一个类声明为public，要求声明为public的类的类名必须与源文件同名。

3. 程序的入口是main（）方法，格式固定
public static void main(String[] args) { }  //args arguments参数

4. 输出语句
System.out.println();  输出数据，然后换行
System.out.print();

5. 每一行执行语句都以分号结束，一行的结束不是分号就是大括号

6. 编译的过程：编译以后，会生成一个或多个字节码文件，与源文件中所声明的类的名称相同

# 2. 基本语法

## 2.1 关键字和保留字

- Keywords
  定义：被java语言赋予了特殊含义，用作专门用途的字符串（单词）
  特点：所有字母都为小写
- reserved word
  现有java版本尚未使用，以后版本可能会作为关键字使用如goto，const

## 2.2 标识符
identifier
1. 自己定义的内容。类名、方法名、变量名、包名、接口名等

2. 命名规则：不遵守，编译不通过
   - 标识符可以包含 英文字母26个(区分大小写) 、 0-9数字 、 $（美元符号） 和 _ （下划线） 
   - 标识符不能以数字开头。 
   - 标识符不能是关键字和保留字，但可以包含。 
   - 严格区分大小写，长度无限制
   - 不能加空格

3. 命名规范： 建议遵守
   - 类名：首字母大写，后面每个单词首字母大写（大驼峰式）。HelloWorld 
   - 方法名、变量名： 首字母小写，后面每个单词首字母大写（小驼峰式）。 helloWorld
   - 常量名：多个单词组成时，字母全部大写，下划线连接 例：INTEGER_CACHE
   - 包名：多单词所有字母小写 xxxyyyzzz

4. 取名：见名知义 提高可读性
5. 用unicode字符集，支持中文但不要使用

## 2.3 变量
1. 概念：
   - 内存中的一个存储区域
   - 该区域的数据可以在同一类型范围内不断变化
   - 变量是程序中的最基本的存储单元。包含类型，变量名，存储的值
   
2. 作用：在内存中保存数据

   注意：
	- 每个变量必须先声明，后使用
	- 使用变量名来访问这块区域的数据
	- 变量的作用域：定义在一对｛｝内
	- 变量只有在其作用域内才有效 
	- 同一个作用域内，不能定义重名的变量

3. 使用
	
	- 格式 数据类型 变量名 = 变量值；
	
4. 变量按数据类型分：
	- 基本数据类型 
		- 整数型 byte（1byte=8bit -128~127） short int（默认）long
		- 浮点型 float double(双精度，默认) 有些小数也无法精确表示
		- 字符型 char
		- 布尔型 boolean
	- 引用数据类型 
		- 数组 [ ] array
		- 类 class   字符串也属于class
		- 接口 interface
```java
class VariableTest{
	public static void main(String[] args){
		int myAge = 20;  //默认使用
		System.out.println(myAge);
		long l1 = 323134L ;//必须以l或L结尾
		short s1 = 1234;
		byte b1 = 127 ; //-127~128 
		float f1 = 1.5F ;//4byte,范围比long还大，以f或F结尾
		double d1 = 123.4 ;
		
		//1.声明一个字符 
		char c1 = 'a'; //2byte,用'',只能有一个字符
		//2.转义字符
		char c2 = '\n' ;  //换行符
		c2 = '\t';
		System.out.print("hello" + c2);
		System.out.println("你好world");
		
		//3.unicode值来表示字符型常量
		char c6 = '\u0043';
		System.out.println(c6);
        
        //4.还可以用ACISS玛
        char c7 = 97  //输出a  开发中非常少年
		
		//布尔型 boolean
		boolean bb1 = true;
		System.out.println(bb1);
		boolean isMarried = true;
		if(isMarried){
			System.out.println("你就不能参加\"单身\"party了，\n很遗憾");
		}else{
			System.out.println("哈哈");
		}
	
	}
}
```
|数据类型|关键字|内存占用|取值范围|
|--|--|-|-|
|字节型|byte|1个字节|-128~127|
|短整型 |short| 2个字节| -32768~32767|
|整型| int（默认）| 4个字节| -231次方~2的31次方-1|
|长整型 |long |8个字节| -2的63次方~2的63次方-1|
|单精度浮点数| ﬂoat |4个字节 |1.4013E-45~3.4028E+38|
|双精度浮点数 |double（默认）| 8个字节 |4.9E-324~1.7977E+308|
|字符型 |char |2个字节| 0-65535|
|布尔类型 |boolean |1个字节| true，false|

5. 按照声明位置 成员变量vs局部变量
6. 基本数据类型之间的运算规则

```java
/*
基本数据类型之间的运算规则：

前提：这里讨论只是7种基本数据类型变量间的运算。不包含boolean类型的。

1. 自动类型提升：
    结论：当容量小的数据类型的变量与容量大的数据类型的变量做运算时，结果自动提升为容量大的数据类型。
	byte 、char 、short --> int --> long --> float --> double 

	特别的：当byte、char、short三种类型的变量做运算时，结果为int型
	
2. 强制类型转换：
	使用强转符
	可能导致精度损失


说明：此时的容量大小指的是，表示数的范围的大和小。比如：float容量要大于long的容量
*/
class VariableTest2 {
	public static void main(String[] args) {
		
		byte b1 = 2;
		int i1 = 129;
		//编译不通过
		//byte b2 = b1 + i1;
		int i2 = b1 + i1;
		long l1 = b1 + i1;
		System.out.println(i2);

		float f = b1 + i1;
		System.out.println(f);

		short s1 = 123;
		double d1 = s1;
		System.out.println(d1);//123.0

		//***************特别地*********************
		char c1 = 'a';//会转换成a的ASCII码97  
		int i3 = 10;
		int i4 = c1 + i3;
		System.out.println(i4);
        
        char cc = (char)(2+'A');  //输出C ASCII码加两位
        System.out.println(cc);

		short s2 = 10;
		//char c2  = c1 + s2;//编译不通过

		byte b2 = 10;
		//char c3 = c1 + b2;//编译不通过

		//short s3 = b2 + s2;//编译不通过

		//short s4 = b1 + b2;//编译不通过
		//****************************************

	}
}
class VariableTest3{
	public static void main(String[] args){
		double d1 = 12.9;
		int i1 = (int)d1;  //12 强转符，截断，损失精度
		System.out.println(i1);
		
		int i2 = 128;
		byte b1 = (byte)i2;
		System.out.println(b1); //-128
		
	}
	
}

class VariableTest4{
	public static void main(String[] args){
		//1.编码情况
		long 1 = 123123;  
		System.out.println(l);//没有报错，实际是个int型
		//long l1 = 121111111111111111111;   超出int范围就编译失败
		long l1 = 121111111111111111111L;
		
		//--------------------
		//float f1 = 12.3;   编译失败，相当于把double转为float，一定要加f
		
		//2.情况2
		//整型常量默认为int 浮点型默认为double
		byte b = 12;
		//byte b1 = b + 1;  编译失败
		
		//float f1 = b + 12.3; 编译失败
		
		
	}
	
}

```

7. String类型的使用

``` java
   /*
   String类型变量的使用
   1.属于引用数据类型
   2.使用"" 字符串
   3.String可以和8种基本数据类型做运算且运算只能是连接运算 +
   */
   class StringTest {
   	public static void main(String[] args) {
   		String s1 = "hello world!";
   		System.out.println(s1);
   		String s2 = "a";
   		String s3 = "";  //长度没有限制，不同于char一定要有一个字符
   		
   		int num = 1001;
   		String numStr = "学号：";
   		String info = numStr + num;  //连接语法，输出的是Spring
   		boolean b1 = true;
   		String info1 = b1 + info;
   		System.out.println(info);
   		System.out.println(info1);
   		
   		//----------------------
   		//练习1
   		char c = 'a';
   		int num2 = 10;
   		String str = "hello"; 
   		System.out.println(c + num2 + str); //107hello  A:65
   		System.out.println(c + str + num2); //ahello10
   		System.out.println(c + (num2 + str));//a10hello
   		System.out.println((c + num2) + str);//107hello
   		System.out.println(str+ num2 + c); //hello10a
   		
   		//练习2
   		//输出*	*   \t 为table
   		System.out.println("* *");
   		System.out.println('*' + '\t' + '*');   //93 两个char相加会转成int
   		System.out.println('*' + "\t" + '*');	//只有前面的+运算是string就会传递
   		System.out.println('*' + '\t' + "*"); 	//51*
   		System.out.println('*' + ('\t' + '*'));  	
           
           //String无法强转为int
   		//int num1 = (int)str1  编译不通过
   		int num1 = Integer.parseInt(str1);		
   	}
	}
```

   8. 进制转换（了解）
   
      原码 反码 补码（计算机底层存储的）
      
      二进制  0b 或0B开头
      十进制
      八进制 以0开头
      十六进制 0x或 0X开头  A-F不区分大小写

## 2.4 运算符

### 2.4.1 算术运算符
\+  正号 加
\-  负号 减
*
/
% mod
\+\+  自增1
\-\-  自减1
\+  连接符


```java
public class AriTest {
	public static void main(String[] args){
		// 除号：/
		int num1 = 12;
        int num2 = 5;
        int result1 = num1 / num2;
        System.out.println(result1); //2
        int result2 = num1/num2*num2;
        System.out.println(result2); //10 
        double result3 = num1/num2; //2.0 相当于把整形2赋值给double
        double result4 = num1/num2 + 0.0; //2.0
        System.out.println(result4);
        double result5 = num1/(num2+0.0);
        System.out.println(result5); //2.4 相当于int/double
        double result6 = (double)num1/num2; //2.4 把num1强转
        System.out.println(result6);
        double result7 = (double)(num1/num2); //2.0 把int型的2强转
        System.out.println(result7);
    
        //%:mod运算
        //结果的负号与被模数的符号相同
        //开发中，判断是否能除尽
        int m1 = 12;
        int n1 = 5;
        System.out.println("m1 % n1 = " + m1 % n1 );
        int m2 = -12;
        int n2 = 5;
        System.out.println("m2 % n2 = " + m2 % n2);
        int m3 = 12;
        int n3 = -5;
        System.out.println("m3 % n3 = " + m3 % n3 );
        int m4 = -12;
        int n4 = -5;
        System.out.println("m4 % n4 = " + m4 % n4 );
    
        //（前）++ ：先自增1，然后再运算   运算可以是赋值之外的运算
        //（后）++ ：先运算，后自增1
        int a1 = 10;
        int b1 = ++a1;  //先a1+1 再赋值给b1
        int a2 = 10;    //先把10赋值给b2，再自增1
        int b2 = a2++;
        System.out.println("a1 = "+ a1 + ",b1 = " + b1 ); //a1 = 11,b1 = 11
        System.out.println("a2 = "+ a2 + ",b2 = " + b2 ); //a1 = 11,b1 = 10
        //注意点：
        short s1 = 10;
        s1++;
        System.out.println(s1);  //11  自增1 不会改变变量自身的数据类型
        byte bb1 = 127;
        bb1++;
        System.out.println(bb1);  //-128  二进制+1
        //（前）-- 先自减1 后运算
        //（后）-- 先运算 后自减1
        int a4 = 10;
        int b4 = --a4;
        System.out.println("a4="+a4+",b4="+b4); //a4=9,b4=9
    
         /*练习：随意给出一个整数，打印显示它的个位数，十位数，百位数的值。 格式如下： 数字xxx的情况如下： 个位数： 十位数： 百位数：
         例如： 数字153的情况如下： 个位数：3 十位数：5 百位数：1 */
        int num = 153;
        int hun = num/100;
        int ten = (num-hun*100)/10;  //  num%100/10
        int one = num - 100*hun-10*ten; // num%10  %1是0
        System.out.println("个位数："+one+'\n'+" 十位数："+ten+" 百位数："+hun);
    	}
    }
```


### 2.4.2 赋值运算符


```java

 支持连续赋值
 = 两边数据类型不一致 可以使用自动类型转换或强制类型转换
 包括：= += -= *= /= %=


public class SetValueTest {
    public static void main(String[] args){
        //赋值符号：=
        int i1 = 10;
        int j1 = 10;
        //连续赋值
        int i2,j2;
        i2 = j2 = 10;
        int i3 = 10,j3 = 20;

        //----------------------------
        int num1 = 10;
        num1 += 2;// 12 相当于 num1 = num1+2
        num1 %= 5;   //不会改变变量本身的数据类型
        System.out.println(num1);
        // 开发中，如果希望变量实现+2的操作，有几种加法（前提：int num=10）
        int num = 10;
        num = num + 2;
        num += 2; //推荐
        //实现 +1
        //前两种 以及 ++运算 （推荐） 经常使用

        //练习1
        int i = 1;
        i *= 0.1;
        System.out.println(i);  // 0  不改变数据类型  0.1截断
        i++;
        System.out.println(i);

        //练习2
        int m = 2;
        int n = 3;
        n *= m++;  //m=3,n=6

        //练习3
        int n1 = 10;
        n1 += (n1++) + (++n1);
        System.out.println(n1); //n = 32  10+10+12  (++n1)中的n1已经是11了
    }
}

```



### 2.4.3 比较运算符

```java
/*
比较运算符
== != > < >= <= instanceof

结论：
1.比较运算符的结果是boolean类型
2.区分== 和  =
3.== 和 != 不仅可以用于数值类型数据之间，还可以用在其他引用类型的变量之间
*/


public class CompareTest {
    public static void main(String[] args){
        int i = 10;
        int j = 20;
        System.out.println(i==j);  //false
        System.out.println(i=j);   //20

        boolean b1 = true;
        boolean b2 = false;
        System.out.println(b1==b2);  //false
        System.out.println(b1=b2);   //false
    }
}

```



### 2.4.4 逻辑运算符

```java
/*
逻辑运算符
&逻辑与 &&短路与 |逻辑或 || 短路或 !逻辑非 ^ 逻辑异或
1.用于boolean型变量之间的运算
 */
public class LogicTest {
    public static void main(String[] args){
        //区分&  和&&
        //运算结果相同；当符号左边是true时，都会执行右边的计算
        //当符号左边是false时,只有&会执行右边的计算
        boolean b1 = true;
        b1 = false;
        int num1 = 10 ;
        if(b1 & (num1++ > 0)){
            System.out.println("china");
        }else{
            System.out.println("japan");
        }
        System.out.println("num1="+num1);

        boolean b2 = true;
        b2 = false;
        int num2 = 10 ;
        if(b2&& (num2++ > 0)){
        //b2已经是false了，短路与后面的语句就不再执行了，前面是true就要执行
            System.out.println("china");
        }else{
            System.out.println("japan");
        }
        System.out.println("num2="+num2);

        //区分：| 与 ||
        //运算结果相同，当符号左边是false时，二者都会执行符号右边的计算
        //当符号左边为true时，只有|继续执行右边语句
        //开发中推荐使用&& ||
    }
}
```

### 2.4.5 位运算符

```java
/*
位运算符 （了解）
结论：
1.操作整型数据
2.<< 每向左移1位 相当于*2   左移 末尾补0
  >> 每向右移1位 相当于/2   右移 根据原先的符号，左边补1或0
  >>> 无符号右移 都用00在前面补  负数会变成正数
  &  二进制各位进行与运算 0位false 1为true
  |  二进制各位进行或运算
  ^  二进制各位进行异或运算
  ~  二进制码按补码各位取反

  最高效方式计算 2*8？   2<<3 或 8<<1 乘法就是两个8相加或者8个2相加，底层运算更复杂

 */
public class BitTest {
    public static void main(String[] args){
        int i = 21;
        System.out.println("i<<2: "+(i<<2));  //84
        System.out.println("i<<3: "+(i<<3));  //168

        System.out.println("i<<27: "+(i<<27)); //符号改变 int 32bit 二进制的第一位是符号位，1是负数，0是正数


        //练习： 交换两个变量的值
        int num1 = 10;
        int num2 = 20;
        System.out.println("num1="+num1+",num2="+num2);

        //方式一:定义临时变量（推荐）
        int temp = num1;
        num1 = num2;
        num2 = temp;
        System.out.println("num1="+num1+",num2="+num2);

        //方式二：好处：不用定义临时变量 节省内存空间
        //弊端：相加可能超出存储范围，只能用于数值型
        num1 = num1 + num2;
        num2 = num1 - num2;
        num1 = num1 - num2;
        System.out.println("num1="+num1+",num2="+num2);

        //方式三：位运算符  m = (m^n)^n 也只能用于数值类型
        num1 = num1 ^ num2;
        num2 = num1 ^ num2;
        num1 = num1 ^ num2;
        System.out.println("num1="+num1+",num2="+num2);
    }
}
```



### 2.4.6 三元运算符

```java
/*
三元运算符 三目运算符
1.格式： (条件表达式)?表达式1 ： 表达式2
2.条件表达式的结果是boolean
    true执行表达式1
    false执行表达式2
3.表达式1和表达式2 能够统一为一个类型，才能用一个新的变量去接收
4.可以嵌套使用
5.凡是三元运算符，都可以改写为if-else；相反则不行
6.如果既可以用三元运算符又可以使用if-else 优先用三元运算符
 */

public class TripleTest {
    public static void main(String[] args){
        //获取两个整数的较大值
        int m = 12;
        int n = 5;
        int max = (m>n)? m : n;
        System.out.println(max);
        double max0 = (m>n)? 2 : 1.0;
        n=12;
        String maxStr = (m>n)? "big" : ((m == n)? "equal":"small");  //三元运算符作为一个表达式 里面输出的是String
        System.out.println(maxStr);

        //获取三个数的最大值
        int n1 = 10;
        int n2 = 30;
        int n3 = -43;
        int max1 = (n1>n2)? n1 : n2;
        int max2 = (max1>n3)? max1 : n3;
        System.out.println(max2);
    }
}
```
### 2.4.7 运算符的优先级
了解

## 2.5 程序流程控制

### 2.5.0 使用Scanner获取数据

```java
/*
如何从键盘获取不同类型的变量：需要Scanner类

具体实现步骤：
1. 导包： import java.util.Scanner; java.util为包名，Scanner为类
2.Scanner的实例化
3.调用Scanner类的相关方法，来获取指定类型的变量
注意：如果输入的数据类型与要求不匹配时，会报异常导致程序终止运行 InputMisMatchException
 */
import java.util.Scanner;
public class ScannerTest {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        //Scanner为类 scan为标识符 new为关键字 System.in为从系统输入 新建了以个对象
        System.out.println("input your name:");  //char以string替代，没有char相关方法
        String name = scan.next();
        System.out.println(name);
        System.out.println("input your age:");
        int age = scan.nextInt(); //nextInt为Scanner类中的方法之一
        System.out.println(age);
        System.out.println("input your weight:");
        double weight = scan.nextDouble();
        System.out.println(weight);
        System.out.println("Are u single?(true/false)");
        boolean status = scan.nextBoolean();
        System.out.println(status);
        System.out.println("input your gender");
        String gender = scan.next();
        char genderChar = gender.charAt(0); //变量名.charAt()  获取索引位置0上的字符
        System.out.println(genderChar);


    }
}
```



### 2.5.1 分支结构
#### if- else if -else 结构
```java
/*
分支结构中的if else（条件判断结构）
三种结构
if (条件表达式){
    执行表达式
}
------------------
if (条件表达式){
    执行表达式1
}else {
    执行表达式2
}
------------------
if (条件表达式1){
    执行表达式1
}else if(条件表达式2) {
    执行表达式2
}else if(条件表达式3) {
    执行表达式3}
...
else {
    执行表达式n
}
 */

public class IfTest {
    public static void main(String[] args){
        //举例1
        int heartBeats = 79;
        if (heartBeats <60 || heartBeats>100){
            System.out.println("further check");
        }
        System.out.println("healthy");


        //举例2
        int age = 23;
        if (age<18){
            System.out.println("anime");
        }else {
            System.out.println("porn");
        }

        //举例3
        if(age < 0){
            System.out.println("false");
        }else if(age<18){
            System.out.println("teen");
        }else{
            System.out.println("grownup");
        }
    }
}
```

练习1
```java

/*
说明：
1.else 结构是可选的,如果没有结果可以不输出
2.针对条件表达式：
>如果多个条件表达式之间是互斥关系（没有交集），判断和执行语句上下位置无所谓
>如果有交集，需要根据实际情况考虑清楚应该讲那个结构声明在上面
>如果有包含关系，通常情况下要将范围小的放在范围大的上面
 */

import java.util.Scanner;
public class IfTest2 {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        System.out.println("input your grade:(0-100)");
        int grade = scan.nextInt();
        if (grade == 100){
            System.out.println("BMW automobile");
        }else if( grade>80 && grade<=99 ){
            //不能写作 80<grade<=99,前面一步结果为boolean，不能和后面的int进行比较
            System.out.println("iPhone Xs Max");
        }else if( grade>60 && grade<=80){
            System.out.println("iPad Pro");
        }else{
            System.out.println("Nothing");
        }
    }
}
```

练习2：将输入的三个数排序输出
```java
/*
说明：
1.if-else的结构是可以嵌套的
2.嵌套结构中的大括号是可以省略的（还是加上好，可能经常要在其中加入其他语句），只执行if下的1句
3.else 就近原则 和最近的if配对
4.if(条件)，判断条件中如果变量是boolean，=号也能编译通过
 */

import java.util.Scanner;
class Iftest3 {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        System.out.println("input three integral:(finish with enter)");
        int a = scan.nextInt();
        int b = scan.nextInt();
        int c = scan.nextInt();
        if (a>b){
            if(c>a){
                System.out.println(c+">"+a+">"+b);
            }else if (c<b){
                System.out.println(a+">"+b+">"+c);
            } else{
                System.out.println(a+">"+c+">"+b);
            }
        }else{
            if(c>b){
                System.out.println(c+">"+b+">"+a);
            }else if (c<a){
                System.out.println(b+">"+a+">"+c);
            } else{
                System.out.println(b+">"+c+">"+a);
            }
        }

    }
}
```

练习：彩票问题

```java
import java.util.Scanner;
public class LotteryGame {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int ranNum = (int)(Math.random()*90 + 10);
        System.out.println("input your lottery number:");
        int lotNum = scan.nextInt();
        System.out.println(ranNum);
        int rTen = ranNum/10;
        int rOne = ranNum%10;
        int lTen = lotNum/10;
        int lOne = lotNum%10;
        if (lTen == rTen && lOne == rOne){
            System.out.println("$10000");
        }else if(lTen == rOne && lOne == rTen){
            System.out.println("$3000");
        }else if(lTen == rTen || lOne == rOne){
            System.out.println("$1000");
        }else if(lTen == rOne || lOne == rTen) {
            System.out.println("$500");
        }else {
            System.out.println("nothing");
        }
    }
}

```

如何获取随机数

```java
//练习：如何获取一个随机数：10-99
double value = Math.random();//区间[0.0,1.0)
int num = (int)(Math.random()*90+10);
System.out.println(num)
//公式  [a,b]: (int)(Math.random()*(b-a+1)+a)
```

练习：

```java
//if(String的变量名.equals());
import java.util.Scanner;
public class IfExer {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        System.out.println("input height(cm) wealth(10Grand) appearance(true/false:");
        double height = scan.nextDouble();
        int wealth = scan.nextInt();
        boolean appearance = scan.nextBoolean();
        if (height>180.0 &&wealth>1000 && appearance== true ){
            System.out.println("marry him");
        }else if(height>180.0 ||wealth>1000 || appearance== true) {
            System.out.println("think twice");
        }else{
            System.out.println("find a better one");
        }
    }
}
```



#### switch - case结构

```java
/*
1.格式
switch(表达式){
case 常量1：
     执行语句1；
     //break;
case 常量2：
     执行语句2；
     //break;
...
default:
      执行语句n;
      //break;
}
2.说明：
> 根据switch表达式的值，依次匹配case中的常量。一旦匹配成功，进入相应case中结构中
调用执行语句，调用完毕后，则仍然继续向下执行其他case中的执行语句，直到遇到break
或者switch结构结束。
> break,在switch case结构中，一旦遇到就跳出，是可选的
> switch 结构中的表达式只能是如下的6中数据类型之一：
byte short int char String（JDK7.0新增） 枚举类型(JDK 5.0新增)
>case后面只能声明常量，不能声明范围
>default 类似于if - else中的 else ，也是可选的，而且位置是灵活的
>能用switch case都能用 if else，反之不行
>写分支结构时，二者都可用时，switch中的表达式的取值不多的情况下，有限选择 switch case，因为switch case 的执行效率稍高，实际开发中用if else较多。

 */


import java.util.Scanner;

public class SwitchTest {
    public static void main(String[] args) {
        int num = 2;
        switch (num){
            case 0:
                System.out.println("zero");
            case 1:
                System.out.println("one");
            case 2:
                System.out.println("two");
            case 3:
                System.out.println("three");
            default:
                System.out.println("others");
        }
    }
}

class ConvertCapital {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        System.out.println("input the character");
        String character = scan.next();
        char cha = character.charAt(0);
        switch (cha) {
            case 'a':
                System.out.println("A");
                break;
            case 'b':
                System.out.println("B");
                break;
            case 'c':
                System.out.println("C");
                break;
            case 'd':
                System.out.println("D");
                break;
            case 'e':
                System.out.println("E");
                break;
            default:
                System.out.println("others");
        }
    }
}
//说明：如果执行语句相同，可以合并
class SwitchCaseTest1{
    public static void main(String[] args) {


    int score = 78;
		switch(score / 10){
        case 0:
        case 1:
        case 2:
        case 3:
        case 4:
        case 5:
            System.out.println("不及格");
            break;
        case 6:
        case 7:
        case 8:
        case 9:
        case 10:
            System.out.println("及格");
            break;
    }
    }
}
//练习 ：一年中的第几天 但是怎么限定每个月的值的取值范围呢
class SwitchCaseTest2 {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        System.out.println("input month(1-12) & date(0-31)");
        int month = scan.nextInt();
        int date = scan.nextInt();
        int days = 0;             //定义变量要初始化值
        switch (month){
            case 1:
            case 2:
            case 4:
            case 5:
                days = (month-1)*30+date;
                break;
            case 3:
                days = (month-1)*30+date-1;
                break;
            case 6:
            case 7:
                days = (month-1)*30+date+1;
                break;
            case 8:
                days = (month-1)*30+date+2;
                break;
            case 9:
            case 10:
                days = (month-1)*30+date+3;
                break;
            case 11:
            case 12:
                days = (month-1)*30+date+4;
                break;
            default:
                System.out.println("wrong");
        }
        System.out.println("this is the "+ days +" days of 2019");
    }
}

//方法二
class SwitchCaseTest3 {
    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);
        System.out.println("请输入2019年的year：");
        int year = scan.nextInt();
        System.out.println("请输入2019年的month：");
        int month = scan.nextInt();
        System.out.println("请输入2019年的day：");
        int day = scan.nextInt();


        //定义一个变量来保存总天数
        int sumDays = 0;
switch(month){
        case 12:
        sumDays += 30;
        case 11:
        sumDays += 31;
        case 10:
        sumDays += 30;
        case 9:
        sumDays += 31;
        case 8:
        sumDays += 31;
        case 7:
        sumDays += 30;
        case 6:
        sumDays += 31;
        case 5:
        sumDays += 30;
        case 4:
        sumDays += 31;
        case 3:
            if (year%4==0 && year%100!= 0 || year % 400 == 0){
                sumDays += 29;
            }else{
                sumDays += 28;
        }
        case 2:
        sumDays += 31;
        case 1:
        sumDays += day;
        }

        System.out.println("2019年" + month + "月" + day + "日是当年的第" + sumDays + "天");
        }
        }
```



### 2.5.3 循环结构

在某些条件满足的情况下，反复执行特定代码

#### for语句

```
/*
For循环的使用
一、循环结构的4个要素
1.初始条件
2.循环条件  boolean 类型
3.循环体
4.迭代条件


二、for 循环的结构
for（1；2；4）{
    3;
}
执行过程1>2>3>4>2>3>4>...>2 退出循环

i在循环外是不可调用的
 */

public class ForTest {
    public static void main(String[] args) {
        for(int i=1;i <= 5;i++){
            System.out.println("hello world");

        }
        //练习：
        int num = 1;  //要在外面初始化值，不然可能后面无法输出 如果是if else则为二选一，一定会有值，可以不用初始化，如果只是if没有else也要初始化。
        for(System.out.print('a');num <= 3;System.out.print('c'),num++){
            System.out.print('b');
        }
        //输出结果：abcbcbc
    }
}



//遍历100内的偶数,输出所有偶数的和，输出偶数的个数
class TraverseEven {
    public static void main(String[] args) {
        int sum = 0;  //sum要在循环外创建，不然每次进循环就又赋值0了
        int count = 0;
        for (int i = 0; i <= 100; i++) {
            if (i % 2 == 0) {
                System.out.println(i);
                sum += i;
                count++;
            }
        }
        System.out.println(sum);  //写在for语句外面
        System.out.println(count);
    }
}

    //练习
    class ForExer{
        public static void main(String[] args) {
            for(int i=1;i<151;i++){
                System.out.print(i);
                if(i%3==0){
                    System.out.print(" foo");
                }
                if(i%5==0){  //这里不用else if 因为既是3也是5的倍数
                    System.out.print(" biz");
                }else if(i%7==0) {
                    System.out.print(" baz");
                }
                System.out.print('\n');
            }
        }
    }
```

break 关键字的使用

```java
//break关键字的使用
import java.util.Scanner;
public class ForTest1 {
    public static void main(String[] args) {
        //输入正整数m,n  求最大公约数和最小公倍数  
    Scanner scan = new Scanner(System.in);
        System.out.println("input 2 positive integral:");
    int m = scan.nextInt();
    int n = scan.nextInt();
    int max = (m>n)?m:n;
    int min = (m<n)?m:n;
    for(int i = max; i>=1;i--) {
        if (m % i == 0 && n % i == 0) {
            System.out.println("greatest common divisor is "+i);
            break;  //一旦执行到break 就跳出循环
        }
    }

        for(int i = min; i<=m*n;i++) {
            if (i % m == 0 && i % n == 0) {
                System.out.println("least common multiple is "+i);
                break;
            }
        }
    }
}
```



#### while

```java
import java.util.Scanner;

/*
while循环的使用
1
while(2){
3;
4;
}
说明：
1.写while循环不能少了迭代条件，可能导致死循环。
2.避免死循环
3.for和while可以相互转换
4.初始条件的作用域不同
5.初始化条件复杂的就用while

算法：有限性

执行过程：
1 2 3 4 2 3 4...2

 */
public class WhileTest {
    public static void main(String[] args) {
        int i = 1;
        while (i < 100){
            if(i%2 == 0){
                System.out.println(i);
            }
            i++;
        }
        System.out.println(i);


    }
}
```



#### do - while

```java
/*
do-while循环的使用
1;
do{
3;
4;
} while(2);

执行过程：
1  3 4  2 3 4  2 3 4 ...2
说明：
1.do while循环至少会执行一次循环体
2.使用 for while 较多，do while 较少
 */

 class WhileTest1 {
    public static void main(String[] args) {
        int sum = 0;
        int count = 0;
        int i = 1;
        do{
            if (i%2==0){
                System.out.println(i);
                sum += i;
                count++;
            }
            i++;
        }while(i<101);
        System.out.println(i);
        System.out.println(sum);
        System.out.println(count);
        System.out.println(sum + (char)count);


    }
}
```

#### while(true) 和 for( ; ; )

```java
//从键盘读入不确定个数的整数，并判断正负数的个数，输入为0时结束程序
/*
不再限制循环次数的结构：for(;;) while(true)
结束循环的方式：
1.循环条件返回 false
2.在循环体中执行break
 */

class WhileTest2{
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int postive = 0;
        int negative =0;
        System.out.println("input");
                while(true){//while的判断条件是一个boolean
            int num = scan.nextInt();
            if(num>0){
                postive++;

            }else if(num<0){
                negative++;

            }else{
                break;
            }
        }System.out.println(postive);
        System.out.println(negative);
    }
}
```

#### 嵌套循环

```java
// 2.5.x嵌套循环
/* 嵌套循环的使用
1.定义：将一个循环结构A声明在另一个循环结构B的结构体中，就构成了嵌套循环
2.
内层循环；
外层循环

3.内层循环结构遍历一遍，相当于外层循环体执行了一次
4.外层m次，内层n次，内层循环体执行了m*n次
5.外层控制行数，内层控制列数
 */
class WhileTest3{
    public static void main(String[] args) {
        for (int i =1;i<7;i++){
            System.out.print("*");
        }
        for (int m=1;m<=4;m++){
            for (int i =1;i<7;i++) {
                System.out.print("*");
            }
            System.out.println();
        }
        for (int m = 1;m<=5;m++){
            for(int n = 1; n<=m;n++){
                System.out.print("*");
            }
            System.out.println();
        }
        for (int m = 1;m<=5;m++){
            for(int n = 1; n<=5-m;n++){
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
/* 打印菱形 lozenge diamond
    *
   * *
  * * *
 * * * *
* * * * *
 * * * *
  * * *
   * *
    *

 */
class WhileExer1{
    public static void main(String[] args) {
        //上半部分
        for (int i = 1; i<=5;i++){
            for(int k=1;k<=5-i;k++){
                System.out.print(" ");
            }
            for(int j = 1; j<=i;j++){
                System.out.print("* ");
            }
            System.out.println();
        }
        //下半部分 略
    }
}

//嵌套循环的应用：九九乘法表
class MultiplicationTable{
    public static void main(String[] args) {
        for(int i = 1;i<=9;i++){
            for(int j = 1;j<=i;j++){
                System.out.print(i+"*"+j+"="+(i*j)+" ");
            }
            System.out.println();
        }

    }
}
```

#### 算法优化：输出质数

  ```java
//100以内所有质数的输出
//自己做的 （没有引入boolean,错误做法）
//流程控制结构的的使用+算法逻辑（难点）
class PrimeNumber {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();
        int count0 = 0;

        for (int i = 3; i <= 100000; i++) {
            if(i==3){
                System.out.println(i-1);
            }
            //大于10的情况
            //if (i%2!=0&&i%3!=0&&i%5!=0&&i%7!=0){
            for (int j = 2; j <= i-1; j++) {
                if (i % j == 0) {
                    break;
                }else{
                    System.out.println(i);
                    count0++;
                    break;  //使用break来结束循环，就能够只打印一次
                }
            }
        }
        long end = System.currentTimeMillis();
        System.out.println("amount of prime number:"+count0);
        System.out.println("time "+(end-start)) ;
    }
}

class PrimeNumberTest {
    public static void main(String[] args) {

        boolean isFlag = true;//标识i是否被j除尽，一旦除尽，修改其值

        for(int i = 2;i <= 100;i++){//遍历100以内的自然数


            for(int j = 2;j < i;j++){//j:被i去除

                if(i % j == 0){ //i被j除尽
                    isFlag = false;
                }

            }
            //
            if(isFlag == true){    //i=2直接没进入内循环，所以是true就输出了
                System.out.println(i);
            }
            //重置isFlag
            isFlag = true;

        }
    }
}


//对质数问题的优化
class PrimeNumberTest2 {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();  //当前时间距离 1970.1.1的毫秒数

        boolean isFlag = true;//标识i是否被j除尽，一旦除尽，修改其值
        int count = 0;
        for(int i = 2;i <= 100000;i++){//遍历
           for(int j = 2;j <=i-1 ;j++){
          //  for(int j = 2;j <=Math.sqrt(i) ;j++){
                //优化2 开根号 使用两个数进行因式分解，就只要考虑小的一段，开根号为因式分解的中间临界点
                //对本身是质数的自然数有效
                if(i % j == 0){ //i被j除尽
                    isFlag = false;
                    //break;   //优化1：只对非质数有效的优化
                }

            }
            //
            if(isFlag == true){
                //System.out.println(i);
                count ++;
            }
            //重置isFlag
            isFlag = true;

        }
        long end = System.currentTimeMillis();
        System.out.println("amount of prime number:"+count);
        System.out.println("time "+(end-start)) ;
        //衡量优化的指标  31511ms 优化1后 3560ms 优化2 后214ms
        //不再输出只计数       40144ms 优化1后 5064ms 优化2 后41ms

    }
}

//输出质数的实现方式二；
class PrimeNumber2 {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();  //当前时间距离 1970.1.1的毫秒数
        int count = 0;
        label: for(int i = 2;i <= 100000;i++){//遍历
                for(int j = 2;j <=Math.sqrt(i) ;j++){
                if(i % j == 0){
                    continue label;   //一旦被除尽，就进入下一个i；
                }
            }
            //   凡是能执行到吃步骤的就都是质数
                System.out.println(i);//是我一开始想尝试的做法
                count ++;
            }

        long end = System.currentTimeMillis();
        System.out.println("amount of prime number:"+count);
        System.out.println("time "+(end-start)) ; //199
        }

}
  ```

#### break continue 的用法【带标签】

```java	
/*
break coutinue 的使用
break： switch case
          循环结构：  结束当前循环
continue: 循环结构    结束当次循环
相同：
break 和continue后不能加语句，无法编译
默认跳出包裹此关键字最近一层的循环
 */


public class BreakContinueTest {
    public static void main(String[] args) {
        for(int i=1;i<=10;i++){
            if(i%4 == 0){
                //break;  //123
                continue;//123567910
            }
            System.out.println(i);
        }
    }
}

class BreakContinueTest1 {
    public static void main(String[] args) {
       l: for(int i=1;i<=4;i++) {
            for (int j = 1; j <= 10; j++) {
                if (j % 4 == 0) {
                    // break l;   //结束指定标识的一层循环结构  123
                    continue l;  //  结束指定标识的一层循环结构的当次循环 123123123123
                }
                System.out.print(j);
            }
            System.out.println();
        }
    }
}
```

衡量一个功能代码的优劣势：

1. 正确性
2. 可读性
3. 健壮性
4. 高效率与低存储：**时间复杂度**、空间复杂度（衡量算法的好坏）








