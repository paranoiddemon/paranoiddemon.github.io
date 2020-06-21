---
title: Java学习 03
date: 2020-06-17 00:24:46
tags: Java
category: 笔记
description:  Java基础：面向对象
---

## 4. 面向对象

### 4.1 面向对象编程

```java
Object Oriented Programing vs Procedure Oriented Programing
 1.Java类及类的成员：属性，方法，构造器；代码块，内部类
 2.面向对象的三大特征：封装性、继承性、多态性（抽象性）
 3.其他关键字:this super static final abstract interface package import
 
 POP与OOP
 面向过程：强调功能行为，以函数为最小单位，考虑怎么做
 面向对象：强调具备功能的对象，以类/对象为最小单位，考虑谁来做
 
 人{  
     打开冰箱{
     	冰箱.打开()
     	}
     	抬起(大象){
     		大象.进入(冰箱);
     		}
     	关闭(冰箱){
     	冰箱.闭合();
  }
---------------------------     
 冰箱{
 	打开(){}
 	闭合(){}
 }  
 
---------------------------
 大象{
 	进入(冰箱){
 	}
 }
 类：抽象
 实例化为对象
 
 
4.2 Java语言的基本元素：类和对象
类：对一类实物的描述，抽象的、概念上的定义
对象：实际存在的该类实物的每个个体，实例instance

面向对象程序设计的重点是类的设计，其实就是类的成员的设计
 */

```
### 4.2 类和对象

```java
/*
一、设计类
 
 创建类的对象的类的实例化
 
二、类和对象的的使用（面向对象思想落地的实现）
 1.创建类，设计类的成员；最花时间
 2.创建类的对象
 3.通过“对象.属性” “对象.方法”调用对的结构

三、创建了一个类的多个对象，每个对象都拥有一套类的属性。（非static的）
修改一个对象的属性a，不影响其他对象的属性a

四、对象的内存解析
 */

public class PersonTest {
	public static void main(String[] args) {
		//创建person类的对象
		Person p1 = new Person();
		
		//调用类的结构：属性和方法
		//调用属性：“对象.属性”
		p1.name = "TOM";		
		System.out.println(p1.name);
		
		//调用方法：“对象.方法”
		p1.eat();
		p1.sleep();
		p1.talk("Chinese");
		
		Person p2 = new Person();
		System.out.println(p2.name);//null
		//每一个对象都有一套属性
		System.out.println(p2.isMale);//false
		
		Person p3 = p1;
				System.out.println(p3.name); //TOM
		p3.age = 10;
		System.out.println(p1.age);// 10
		
		Person p4 = new Person();
		p4.isMale=true;		
		p4 = p3;   //把p3的所有属性都给了p4
		System.out.println(p3.isMale);	//false
		System.out.println(p4.isMale);  //false
		System.out.println(p4.name);    //TOM
		System.out.println(p4.age);     //10
	}

}


/* 属性 = 成员变量 = field = 域 、字段
	方法 = 成员方法 = 函数 = method  行为
*/  



class Person{
//属性
String name;
int age = 1;
boolean isMale;

//方法
public void eat() {
	System.out.println("eat");
}
public void sleep() {
	System.out.println("sleep");
}
public void talk( String language) {
	System.out.println("speak "+ language);
}
//构造器
//代码块
//内部类

}
//persontest类 调用person类
public class PersonTest {
	public static void main(String[] args) {
		Person p1 = new Person();
		p1.age = 20;
		p1.sex = 0;
		p1.name = "Alice";
		
		p1.study();
		p1.showAge();
		System.out.println(p1.addAge(2));
		System.out.println(p1.age);//22
	}

}


//person类
public class Person {
		String name;
		int age;
		/**
		 * sex:1 male
		 * sex:0 female
		 * 文档注释要写在上面
		 */
		int sex;
		
		
		public void study() {
			System.out.println("studying");
			
		}
		public void showAge() {
			System.out.println("age:"+age);
		}
		public int addAge(int i) {
			age+=i;
			return age;
		}
		
}
```
![JVM内存结构.jpg](https://i.loli.net/2020/06/17/MyqA1rlhC8VWzBt.jpg)

Heap 堆： 存放对象实例、数组 （new的）

Stack 栈：虚拟机栈，存放局部变量（方法中的变量都是局部变量），各种基本数据类型，对象引用（reference类型，不同于对象本身，村的是对象在堆中的首地址），方法执行完自动释放

Method Area 方法区：存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码

### 4.3 类中属性的使用

```
/*
 类中属性的使用
 
属性（成员变量）vs 局部变量
- 相同点：
1.定义变量的格式：数据类型 变量名=变量值；
2.先声明后使用
3.变量都有其对应的作用域

- 不同点：
1.在类中声明的位置不同
	属性：直接定义在类的一对{}内
	局部变量：声明在方法内、方法形参、代码块内、构造器形参、构造器内部的变量
2.关于权限修饰符的不同
	属性：可以在声明属性是，指明其权限、使用权限修饰符
	常用的权限修饰符：private public protected 却省  --->封装性
	目前，暂时使用缺省
	局部变量：不可以使用
3.默认初始化值的情况
	属性：类的属性，根据其类型，都有默认初始化值
		整形（byte short int long） 0
		浮点型（float double）0.0
		字符型（char）0 'u0000'
		布尔型（boolean）false
		引用数据类型（String、类、数组、接口）null
	局部变量：没有初始化值，需要显式赋值
	特别地：形参在调用时赋值
4.在内存中加载的位置不同
	属性：加载到堆 （非static）
	数据变量：加载到栈
 */
public class UserTest {
	//属性（成员变量）
	private String name;
	public int age;
	boolean isMale;
	
	public void talk(String language) {//language:方法形参
		System.out.println("speak "+language);
	}
	public void eat(){
		String food = "egg"; //局部变量
		System.out.println("eat"+food);
	}
	
}
```

### 4.4 类中方法的使用

```java
	/*
 类中方法的声明和使用
 
  方法：描述类应该具有的功能
  比如：Math类 sqrt() random()
       Scanner nextXxx()
       Arrays sort() binarySearch() toString() equals();
 
  1.例子
  public void eat() {} //没有形参
  public void sleep(int hour) {}  //void 没有返回值
  public String getName() {}	 //返回值类型
  public String getNation(String nation) {} //有形参
  
  2.方法的声明
  权限修饰符  返回值类型 方法名 形参列表(){
  			方法体
  }
  注意：static final abstrct 来修饰方法
  
  3.说明
  3.1 权限修饰符 private public 缺省 protected  -->封装性
  3.2 返回值类型 有返回值vs没有返回值
  	- 如果方法有返回值，必须在方法声明时指定返回值的类型,方法中使用return关键字，返回指定类型常量或者变量
  	- 没有返回值，使用void来表示，通常没有返回值的方法不适用return，但是要使用的话，只能return;表示结束
  	  return后不可声明执行语句	
  	- 该不该有返回值：
  	    题目要求、凭经验
  3.3 方法名：标识符，遵循规范 xxxYyy 见名知意
  3.4 形参列表 可以声明0 1 多个
           格式：数据类型1  形参1，数据类型2  形参2...	
           该不该定义形参
  3.5 方法体
  
  4.return关键字的使用：
  4.1.使用在方法体中
  4.2.作用：结束方法；有返回值的方法，使用return 数据 返回方法所要的数据
  4.3.return后不可以声明执行语句
  
  5.方法的使用中，可以调用当前类的属性、方法
  	特殊的：方法A中调用方法A 递归方法
  	随意递归会导致：StackOverFlow 栈溢出
  	方法中不能定义方法
  
  //开发中一般一个源文件只写一个class
  
     */

public class CustomerTest {
	public static void main(String[] args) {
		Customer cust1 = new Customer();
		cust1.eat();
	}

}

//客户类
class Customer{
	//属性
	String name;
	int age;
	boolean isMale;
	
	//方法
	
	public void eat() {
		System.out.println("eat");
	}
	
	public void sleep(int hour) {
		System.out.println("sleep for "+hour+" hours");
		
	}
	public String getName() {
		return name;
	}
	public String getNation(String nation) {
		String info = "come from " +nation;
		return info;
	}
	}

//学生类
class students{
	String name;
	int age;
	String major;
	String interests;
	public String say() {
		String info = name + age + major + interests;
		return info;
		
	}
}

//利用面向对象的方法；设计类circle计算圆的面积

//测试类
public class CircleTest {
	public static void main(String[] args) {
		Circle areaCal = new Circle();
		areaCal.radius = 2.1;
		//方式一：
		//double c1 = areaCal.circleArea();
		//System.out.println(c1);
		
		//方式二：
		areaCal.circleArea();
		
	}
	

}

//圆
class Circle{
	//属性
	//方式一：
//	double radius;
//	public double circleArea() {
//		double area = radius*radius*Math.PI ;
//		return area;
//			
//	}
	//方式二
	double radius;
		public void circleArea() {
			double area = radius*radius*Math.PI ;
			System.out.println("area is "+ area);
		}
		
	//错误情况:半径作为属性定义更好，而不是放在形参里
		public double circleArea(double r) {
			double area = r*r*Math.PI;
			return area;
		}
}
```

### 练习1：设计类

```java
/*
3.1 编写程序，声明一个method方法，在方法中打印一个10*8 的*型矩形， 在main方法中调用该方法。
3.2 修改上一个程序，在method方法中，除打印一个10*8的*型矩形外，再 计算该矩形的面积，并将其作为方法返回值。在main方法中调用该方法， 接收返回的面积值并打印。
3.3 修改上一个程序，在method方法提供m和n两个参数，方法中打印一个 m*n的*型矩形，并计算该矩形的面积， 将其作为方法返回值。在main方法 中调用该方法，接收返回的面积值并打印。

 */

public class Exer3Test {
	public static void main(String[] args) {
		Print star = new Print();
		star.length = 10;
		star.width = 8;
		star.recPrt();
		
		int area = star.recArea();
		System.out.println("面积为："+area);
		
		star.recPrtArea(5,8);
		int area1=star.recPrtArea(5, 8);
		System.out.println(area1);
		System.out.println(star.recArea());
		
		
	}

}

class Print{
	int length;
	int width;
	public void recPrt(){
		for(int i = 0;i<width;i++) {
			for(int j = 0;j<length;j++) {
				System.out.print("* ");
			}
			System.out.println();
		}
	}
	public int recArea(){
		int area = length*width;
		return area;
	}
	public int recPrtArea(int m,int n) {
		for(int i = 0;i<m;i++) {
			for(int j = 0;j<n;j++) {
				System.out.print("* ");
			}
			System.out.println();
		}
		int area = m*n;
		return area;
	}
}
```

### 练习2：对象数组

```java
/*
 4. 对象数组题目： 定义类Student，包含三个属性：学号number(int)，年级state(int)，成绩 score(int)。 
 创建20个学生对象，学号为1到20，年级和成绩都由随机数确定。 
 问题一：打印出3年级(state值为3）的学生信息。 
 问题二：使用冒泡排序按学生成绩排序，并遍历所有学生信息
 
提示： 1) 生成随机数：Math.random()，返回值类型double;  
2) 四舍五入取整：Math.round(double d)，返回值类型long。

5.声明一个日期类型MyDate：有属性：年year,月month，日day。创建2个日期 对象，分别赋值为：你的出生日期，你对象的出生日期，并显示信息。

 */

//对象数组

public class ExerTest4 {
	
	public static void main(String[] args) {
//		Arrays a = new Arrays();
		
		Students[] stus = new Students[20];
		
		for(int i=0;i<20;i++) {
			//给数组的元素赋值
			stus[i] = new Students();
			//给student对象的属性赋值
			stus[i].number = i+1;
			stus[i].state = (int)Math.round(Math.random()*5+1);
			stus[i].score = (int)Math.round(Math.random()*100);
		}   //遍历学生信息
			for(int i = 0; i<stus.length;i++) {
			System.out.println(stus[i].info());
		}
			System.out.println("-------------------");
			for(int i = 0; i<20;i++) {
			
			//输出state为3的学生信息
			if(stus[i].state == 3) {
				System.out.print(stus[i].number+" ");
				System.out.print(stus[i].state+" ");
				System.out.print(stus[i].score+" "+'\n');
				
			}
			//按成绩冒泡排序，遍历所有学生信息
			
		}	
			for(int i = 0;i<stus.length;i++) {
				for (int j = 0;j<stus.length-i-1;j++) {
					if(stus[j].score>stus[j+1].score) {
					Students temp = stus[j];  
					//交换整个对象，而不是对象中的属性score，这样就能实现按照某个属性排序的效果
					stus[j] = stus[j+1];
					stus[j+1] = temp;  
					}
				}
				
			}
			for(int i = 0; i<stus.length;i++) {
				System.out.println(stus[i].info());
			}
		}
	}
class Students {
	int number;//学号
	int state;//年纪
	int score;//成绩
	
	public String info() {
		return "学号："+number +",年级："+state+"成绩："+score;
	}	
}
	
```

### 练习2改进：将功能封装到方法

```java
//对对象数组问题的改进，将操作数组的功能封装到方法中

public class ExerTest5 {
	
	public static void main(String[] args) {
		
		Students1[] stus = new Students1[20];
		
		for(int i=0;i<20;i++) {
			//给数组的元素赋值
			stus[i] = new Students1();
			//给student对象的属性赋值
			stus[i].number = i+1;
			stus[i].state = (int)Math.round(Math.random()*5+1);
			stus[i].score = (int)Math.round(Math.random()*100);
			
		}  
		ExerTest5 test = new ExerTest5();  
		/*
		在outline中可以看出，ExerTest5这个类中有main方法、print方法、searchState方法、
		sort方法，如果要在main中调用其他方法，要在main中new一个ExerTest5的对象。
		*/
			//遍历所有学生信息
			test.print(stus);
										
			//输出state为3的学生信息
			
			test.searchState(stus, 3);
			
			//按成绩冒泡排序，遍历所有学生信息
			
			test.sort(stus);
			test.print(stus);
	}		
		
	/**
	 * 
	 * @Description  遍历Students1[]数组的操作
	 * @landfill 
	 */
	
	public void print(Students1[] stus) {
	for(int i = 0; i<stus.length;i++) {
		System.out.println(stus[i].info());
	}
	System.out.println("-----------------------");
	}
	
/**
 * 
 * @Description 查找Students1数组中指定年级的学生信息
 * @author landfill 
 * @param stus 要查找的数组
 * @param state 要查找的年级
 */
	public void searchState(Students1[] stus,int state) {
	for(int i = 0; i<stus.length;i++) {	
		if(stus[i].state == state) {
			System.out.println(stus[i].info());							
		}
	}
	System.out.println("----------------------");
	}
	
	
	/**
	 * 
	 * @Description:给Students1数组排序
	 * @landfill 
	 * 
	 */
	public void sort(Students1[] stus) {
	for(int i = 0;i<stus.length;i++) {
		for (int j = 0;j<stus.length-i-1;j++) {
			if(stus[j].score>stus[j+1].score) {
			Students1 temp = stus[j];  
			//交换整个对象，而不是对象中的属性score，这样就能实现按照某个属性排序的效果
			stus[j] = stus[j+1];
			stus[j+1] = temp;  
			}
		}
		
		}
	}
}
class Students1{
	int number;//学号
	int state;//年纪
	int score;//成绩
	
	public String info() {
		return "学号："+number +",年级："+state+"成绩："+score;
	}	
}
```

