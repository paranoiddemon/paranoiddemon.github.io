---
title: Java-面向对象
date: 2020-06-17 00:24:46
tags: Java
urlname: java-oop
category: 笔记
description:  Java基础：面向对象  没整理完
toc: false
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
![JVM内存结构](https://i.loli.net/2020/06/17/MyqA1rlhC8VWzBt.jpg)

Heap 堆： 存放对象实例、数组 （new的）

Stack 栈：虚拟机栈，存放局部变量（方法中的变量都是局部变量），各种基本数据类型，对象引用（reference类型，不同于对象本身，村的是对象在堆中的首地址），方法执行完自动释放

Method Area 方法区：存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码

### 4.3 类中属性的使用

```java
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
	局部变量：加载到栈
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

**练习1：设计类**

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

**练习2：对象数组**

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

**练习2改进：将功能封装到方法**

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

万事万物皆对象

匿名对象

### 4.5 方法的拓展

#### 4.5.1 重载

练习

```java
/*
1.编写程序，定义三个重载方法并调用。方法名为mOL。 
三个方法分别接收一个int参数、两个int参数、一个字符串参数。
分别 执行平方运算并输出结果，相乘并输出结果，输出字符串信息。 
在主类的main ()方法中分别用参数区别调用三个方法。


2.定义三个重载方法max()，
第一个方法求两个int值中的最大值，
第二个方 法求两个double值中的最大值，
第三个方法求三个double值中的最大值， 并分别调用三个方法。

 */
public class OverloadExer {
	public static void main(String[] args) {
	OverloadExer test = new OverloadExer();
		test.mOL(5);
		test.mOL(5,6);
		test.mOL("OK");
		
		System.out.println(test.max(0.2, 0.8));
		System.out.println(test.max(2, 8));
		System.out.println(test.max(0.2, 0.8,9.3));
	}
	//1.三个方法构成的重载
	public void mOL(int i) {
		System.out.println(i*i);
	}
	public void mOL(int a,int b) {
		System.out.println(a*b);
	}
	public void mOL(String str) {
		System.out.println(str);
	}
	
	//2.三个方法构成重载
	public int max(int a,int b) {
		if(a>b) {
			return a;
		}else {
			return b;
		}
	}
	public double max(double a,double b) {
		if(a>b) {
			return a;
		}else {
			return b;
		}
	}
	
	public double max(double a,double b,double c) {
		if (a>b&a>c) {
			return a;
		}else if(b>a&b>c) {		
		return b;
	}else{
		return c;
	}
	}
}
```



#### 4.5.2 可变个数的形参

```java
二、可变个数形参的方法
 JDK5.0 variable number of arguments,允许直接定义能和多个实参相匹配的形参
 1.jdk5.0新增的内容
 2.具体使用：
 	2.1格式：类型...变量名
 	2.2当调用可变个数形参的方法时，可以传入任意个数的参数
 	2.3可变个数形参方法和本类中方法名相同，形参不同的方法构成重载
	2.4可变个数形参方法和本类中方法名相同，形参类型相同的数组之间不构成重载，不能够共存
 	2.5可变个数形参在方法的形参中，必须声明在末尾；
 	public void show(int a, double b,String...strs) //编译器无法分清传入数据的类型
 	2.6只能声明一个可变个数的形参，因为必须放在最后面
 */
public class VarargsTest {
	public static void main(String[] args) {
		VarargsTest test = new VarargsTest();
		//test.show("aa");
		test.show("aa","bb","cc");
		//test.show();
	}
	
	public void show(int i) {
		
	};
	public void show(String s) {
		System.out.println("sho");
	}
	//可以取0 1 2 ...
	public void show(String... strs) {
		System.out.println("show");
				//实际上是个数组,也可以遍历
		for(int i = 0; i<strs.length;i++) {
			System.out.println(strs[i]);
	}
	//和上面的定义是没有区别的，不能共存，jdk5.0之后
	//public void show(Stirng[] strs) {
		
	}

}
```

#### 4.5.3 值传递机制

```java
三、方法参数的值传递机制
 1.定义： 方法，必须由其所在类或对象调用才有意义，若方法含有函数：
 形参：方法声明时的参数 定义方法时（）内的参数
 实参：方法调用时，实际传给形参的数据参数值 

 2.值传递机制
 参数传递方式：值传递
 即将实际参数值的副本传入方法内，而参数本身不受影响。
 形参是基本数据类型：将实参基本数据类型变量的数据值传递给形参
 形参是引用数据类型：把地址值传递给形参
 
 关于变量的赋值；
  基本数据类型：赋值得到的是保存的数据值
  引用数据类型：赋值得到的是地址值
  
 
 
 */


public class ArgsTest {
 	public static void main(String[] args) {
 		ArgsTest test = new ArgsTest();
 		Data data = new Data();
 		data.m = 10;
 		data.n = 20;
 		data.n = 20;
 		System.out.println("m="+data.m+" n="+data.n);
 		//调用方法交换m n 
 		//复制 了一份地址到方法中，但是实参和形参指向堆中同样的对象，所以能实现交换
 		test.swap(data);
 		System.out.println("m="+data.m+" n="+data.n);

 		//基本数据类型
 		int m = 10;
		int n = m;
		System.out.println(m+" " +n);
		
		
		n = 20 ;
		System.out.println(m+" " +n);
		
		//交换
//		int temp = m;
//		m = n;
//		n = temp;
//		System.out.println(m+" " +n);
			
		//调用swap方法,因为是基本数据类型不能交换
		//test.swap(10,20);
		System.out.println(m+ " " +n); 
		//没有交换，因为基本数据类型传递进入形参，在swap方法中交换后，就结束了，main方法中的数据没变
		
		
		
		//引用数据类型   o2得到的是o1的地址值，指向堆中的同一个对象
		Order o1 = new Order();
		o1.orderId = 1001;
		Order o2 = o1;
		System.out.println(o1.orderId+ " "+o2.orderId);
		o2.orderId = 2002;
		System.out.println(o1.orderId+ " "+o2.orderId);
		
 	}
 	
 	//交换两个变量值的方法
public void swap (Data data) {
	int temp = data.m;
	data.m = data.n;
	data.n = temp;
	}
}

public class ValueTransferTest {
	public static void main(String[] args) {
		String s1 = "hello";
		ValueTransferTest test = new ValueTransferTest();
		test.change(s1);//还是hello
		
		//String比较特别，本质上是一个char[]
		//String 是存在常量池值中的，数组是不能随意更改的，所以 s在常量池中重新
		//new一个char[] hi;然后s得到新的地址值，所以s1的地址值和指向的对象未改变
		System.out.println(s1); //hello
		//System.out.println(test.change(s1));
	}

	public void change(String s) {
		s = "hi";
	}
}
```

练习

```java
public class ArgsExer {
	public static void main(String[] args) {
		ArgsExer test = new ArgsExer();
		int a = 10;
		int b = 10;
		//test.method(a,b);
		//需要在方法被调用之后，仅打印出a= 100，b=20.写出method 不改变原题
		System.out.println("a="+b+" b="+b);
	}
	
	
	//打印流,取代原来的打印
	//system.exit
}


/*
 值传递练习
 */
public class PassTest {
	public static void main(String[] args) {
		PassObject p = new PassObject();
		Circle c = new Circle();
		p.printAreas(c, 5);
		System.out.println("now radius is: "+c.radius);
	}
	

}
//circle类 
class Circle{
	double radius;
	
	//求面积
	public double findAreas() {
		double area = radius*radius*Math.PI;
		return area;
	}
}

class PassObject {
 public void printAreas(Circle c,int time) {
	
		System.out.println("radius\t\tArea");
		int i = 1; //可以这样写，就可以在循环外使用 
		for(;i<=time;i++) {
			//圆的半径
		c.radius = i;
		double area = c.findAreas();
		System.out.println(c.radius+"\t\t"+area);
	}
		c.radius = i ;
}
}
```



#### 4.5.4 递归方法

```java
/*
 四、递归方法 recursion（了解）
 1.递归方法：一个方法体内调用自身
 2.一种隐式的循环，重复调用某段代码，但无需循环控制
 递归一定要向一直方向递归，否则就成为无穷递归，会进入死循环

 */

public class RecursionTest {
	public static void main(String[] args) {
		//计算1-100之间自然数的和
		//方式一
		int sum = 0;
		for(int i= 1;i<101;i++) {
			sum+=i;
		}
		
		System.out.println(getSum(100));
		System.out.println(getFactorial(10));
		System.out.println(func(10));
		int n = 10;
		
		//第n个数
		System.out.println(fibo(n));
		//打印整个斐波那契数列
		for(int i=1;i<=n;i++) {
			System.out.print(fibo(i)+" ");
		}
	}	
		//方式二 递归
		public static int getSum(int n){
			if(n==1) {
				return 1;
			}else {
				return n +getSum(n-1);
			}
		}
		
		//计算阶乘
		public static int getFactorial(int n){
			if(n==1) {
				return 1;   //终止的情况
			}else {
				return n *getSum(n-1);
			}
		}
		//例3：数列f(0)=1,f(1)=4 f(n+2)=2f(n+1)+f(n)
		//n>0整数 求f（10）
		public static int func(int n) {
			if(n==0) {
				return 1;
			}else if(n==1) {
				return 4;
			}else {
				return 2*func(n-1)+func(n-2);
			}
		}
		
		//例4：斐波那契数列 求n个值，打印数列
		// 1 1 2 3 5 8 13 21... 前两项的和
		public static int fibo(int n) {
			if(n==1) {
				//System.out.println("1");
				return 1;
				
			}else if(n==2) {
				//System.out.println("1 1");
				return 1;
			}else{
				//System.out.println(fibo(n-1)+fibo(n-2));
				return fibo(n-1)+fibo(n-2);
			}
			
		}
		//例5  汉诺塔问题
		//例6  快排
}

public class RecursionTest {
	public static void main(String[] args) {
		RecursionTest test = new RecursionTest();
		test.binomial();
	
		}

//@Test
//递归调用的次数 相当于二叉树的前序遍历 从前往后
/*最后几项输出结果
count:282 k:-1
count:283 k:2
count:284 k:1
count:285 k:0
count:286 k:-1
count:287 k:0
*/
public void binomial() {
	recursion(10);
}

private static int count = 0;
public static int recursion(int k) {
	count ++;
	System.out.println("count:"+count+" k:"+k);
	if(k<=0) {
		return 0;		
	}
	return recursion(k-1)+recursion(k-2);
}
}
```

### 构造器

### 代码块

### 三大特征

#### 封装性

#### 继承性

#### 多态性

### 

### 关键字

#### this

#### super

#### final

#### static

### 设计模式

#### 单例模式

#### 工厂模式

#### 模板设计模式

### Object类和包装类

### 抽象类

### 接口

#### BEFORE JDK 7.0

```java
package com.atguigu.java;
/*
 接口的使用
 1.接口使用interface来定义，类是一种功能，一个类可以实现多个接口
 2.java中，接口和类是并列的结构
 3.如何定义接口：定义接口的成员
 	3.1 JDK 7及以前:只能定义全局常量和抽象方法
 			全局常量：public static final的  但是书写时可以省略
 			public abstract
 	3.2 JDK 8:除了全局常量和抽象方法，还可以定义静态方法、默认方法
 	
 4.接口中是不能定义构造器的，意味着接口不可以实例化	
 
 5.在java开发中，接口通过让类去实现（implements)的方式使用
 	如果实现类覆盖了接口中所有的抽象方法，则此实现类就可以实例化
 	如果实现类没有全部覆盖接口中的所有方法，则此实现类就还是抽象类  重写-->实现
 6.java类可以实现多个接口，弥补了类的单继承性的局限	
 
 7.格式 class Bullet extends Object implements Attackable,Flyable{}  先继承后实现
 
 8.接口和接口之间可以多继承。   
 
 9.接口的使用也体现了多态性  抽象类与接口的区别  都不可以实例化，如果接口或者抽象类做形参，就要使用实现类
 10、接口的本质是一种规范   
 
 */
public class InterfaceTest {
	public static void main(String[] args) {
	System.out.println(Flyable.MIN_SPEED);
	Plane p = new Plane();
	p.fly();
	p.stop();
	Bullet b = new Bullet();
	b.attack();
	b.fly();
	b.stop();
}
}

interface Flyable{
	
	//全局常量	
	public static final int MAX_SPEED = 7900;
	int MIN_SPEED = 1; //前面可以省略
	
	public abstract void fly();
	void stop();//省略了public abstract
}

interface Attackable{
	void attack();
}

class Plane implements Flyable{
	
	@Override
	public void fly() {
		System.out.println("could fly");
		
	}
	@Override
	public void stop() {
		System.out.println("could stop");
		
	}
}

abstract class Kite implements Flyable{  //只实现了一个还是抽象类
	@Override
	public void fly() {
		System.out.println("fly");  
	}
}

class Bullet extends Object implements Attackable,Flyable{
	
	 public void attack() {
		System.out.println("kill");
	}

	@Override
	public void fly() {
		System.out.println("fly");
		
	}

	@Override
	public void stop() {
		System.out.println("stop");
	}

}


//-----------------------------------------------

interface AA{
	void method1();
}

interface BB extends AA{
	void method();
}
```

#### AFTER JDK 8.0

```java
package com.atguigu.interfaceAFTER;
/*
 AFTER JDK8.0 除了定义全局常量和抽象方法，还可以定义静态方法、默认方法
 1.静态方法，通过interface.method调用
 2.默认方法，通过实现类的对象调用，
 默认方法还可以在实现类中重写，通过实现类调用的是重写后的方法
 3.如果子类（或实现类）继承的父类和实现的接口中声明了同名同参数的方法，
 在没有重写此方法的情况下，默认调用的是父类中同名同参数的方法  -->类优先原则
 如果重写了就调用子类自己重写后的方法。
 
 4.如果实现类实现的多个接口中有同名同参数的默认方法，在继承的父类中没有该同名同参数的方法，也并且没有重写。
 直接调用。编译不通过，不知道该调用哪个方法   -->接口冲突，因此需要在实现类中重写此方法
 
 5.在实现类中重写了方法后调用 接口中的默认方法使用  接口名.super.默认方法名
 
 6.实现类写进了接口中的默认方法，一般不怎么重写
 
 
 
 */
public interface CompareA {
	 static void method1() {   //静态方法，通过interface.method调用
		System.out.println("compareA:BEIJING");
	}
	
    public default void method2() {  //默认方法 可以省略public
		System.out.println("compareA:SHANGHAI");
	}
    default void method3() {
    	
    }
}

```

### 内部类

```java
/*
 类的成员之五：内部类
 
 1.允许将一个类A声明在另一个类B中，则类A为内部类，类B称为外部类
 
 2.分类：
 成员内部类： 和其他构成平行的声明在类内   静态的和非静态的
 局部内部类：方法内 代码块内 构造器内
 
 3.成员内部类
 	一方面，作为外部类的成员：
 	> 可以调用外部类的结构
 	> 内部类可以被static修饰  分为静态非静态的内部类
 	> private default protected public    类本身只有两种权限修饰符  public 和 default
 	另一方面，作为一个类：
 	> 可以定义属性 方法 构造器 代码块 再定义内部类
 	> 可以被final修饰，表示不能被继承
 	> 可以被abstract修饰，表示不能被实例化
 4.
 	4.1 如何实例化成员您外部类的对象
 	4.2 如何在成员内部类中区分调用外部类的结构
 	4.3 局部内部类如何在开发中使用   见InnerClassTest1.java
 */
public class InnerClassTest {
	public static void main(String[] args) {
		
		//创建非静态成员内部类的实例化，先要把外部类实例化
		Person p = new Person();
		Person.Brain b = p.new Brain(); //在外部的类的实例中去new一个内部类
		
		//静态成员内部类的实例化
		Person.Heart h = new Person.Heart();  //如果是静态的内部类，可以通过这样调用
		
	}

}

class Person {
	
	String name;
	public void eat(){
		
	}
	
		
	
	
	//成员内部类 非静态的
	 class Brain{
		String name;
		public Brain() {
			
		}
		public void method1(String name) { 
			System.out.println("BRAIN");
			Person.this.eat();   //可以在内部类的方法体中   调用外部类的方法,省略了Person.this.
			
			
			System.out.println(name);   //调用形参传入的name
			System.out.println(this.name);   //调用的该方法所在类的name
			System.out.println(Person.this.name);  //调用的是外部类的而name
		}
		
	}
	//成员内部类 静态的
		static class Heart{
			String name;
			public Heart() {
				
			}
			public void method1() {
				System.out.println("Heart");
			}
		
	}
	//局部内部类：方法中
	public void method() {
		class AA{
			
		}
		class BB{
			
		}
	}
	
	//局部内部类：构造区中
	public Person() {
		class CC{
			
		}
	}
	
	//局部内部类：代码块中
	{
		class DD{
			
		}
	}
}

//开发中局部内部类很少会声明在构造器和代码块中
public class InnerClassTest1 {

	//即使是在方法中也很少见局部内部类
	public void method() {
		class AA{
			
		}
		class BB{
			
		}
	}
	//返回一个实现了Comparable接口的类的对象
	public Comparable getComparable() {
		  //创建一个实现了Comparable接口的类：局部内部类  非匿名实现类的匿名对象
//		class MyComparable implements Comparable{
//			@Override  //对接口的抽象方法进行重写
//			public int compareTo(Object o) {
//				
//				return 0;
//			}
//			
//		}
//		return new MyComparable();     //return一个实现类，这个内部类只在该方法内使用
		//方式二  :return一个 匿名实现类的匿名对象
		return new Comparable() {

			@Override
			public int compareTo(Object o) {
				// TODO Auto-generated method stub
				return 0;
			}
			
		};
	}
	
	
}

```

