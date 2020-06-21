---
title: Java学习 02
date: 2020-06-16 11:24:46
tags: Java
category: 笔记
description:  Java基础
---
## 3. 数组

### 3.1 一维数组

#### 3.1.1 概念

/*
数组的概念

1. array，多个相同数据类型的数据按一定顺序排列集合，并使用一个名字命名，
 并通过编号的方式对这些数据进行统一管理

2. 相关概念
数组名
下表、角标、索引
元素
数组的长度 ，元素个数

3. 
1）数据是有序排列的 *
2）数组是引用数据类型变量，数组的元素既可以是基本数据类型，也可以是引用数据类型。
3）创建数据是在内存中开辟了一块连续的空间，数组名引用的是该空间的首地址
4）数组的长度一旦确定不能修改

4. 分类：
维数：一维，二维数组...
按照元素：基本数据类型元素的数组，引用数据类型元素的数组

5. 一维数组的使用
 *一维数组的声明和初始化
 *如何调用数组的制定位置的元素
 *如何获取数组的长度
 *如何遍历数组
 *数组元素的默认初始化值
 *数组的内存解析
*/



#### 3.1.2 一维数组的使用

1. 一维数组的声明和初始化
2. 如何调用数组的制定位置的元素
3. 如何获取数组的长度
4. 如何遍历数组
5. 数组元素的默认初始化值
6. 数组的内存解析


```java
public class ArrayTest {
	public static void main(String[] args) {
		//1.一维数组的声明和初始化
		int num;
		num = 10 ;
		int id = 1001;
		
		int[] ids;
		//1.1静态初始化:数组的初始化和数组元素的赋值操作同时进行 
		ids = new int[] {1001,1002,1003,1004};
		//1.2动态初始化：数组的初始化和数组元素的赋值操作分开进行，只写长度
		String[] names = new String[5];
		
		//总结：数组一旦初始化完成，其长度就确定了。
		
		//2.如何调用数组制定位置的元素：通过索引的方式调用
		//数组的索引从0开始，到数组的长度-1结束
		names[0] = "apple";
		names[1] = "banana";
		names[2] = "cat";
		names[3] = "dog";
		names[4] = "egg"; //charAt(0),和数据库sql交互一般从1开始
		//names[5] = "fish";  超出数组长度无法运行 ArrayIndexOutOfBoundsException
		
		//3.如何获取数组的长度：
		//length
		System.out.println(names.length);//5
		System.out.println(ids.length);//4
		
		//4.如何遍历数组
		for(int i = 0;i<names.length;i++){
			System.out.println(names[i]);
		}
		
		//5.数组元素的默认初始化值
		/*
		 * 数组元素是整型 0
		 * 浮点型 0.0
		 * char型：0 （ASCII码）'\u0000',而非'0'
		 * boolean: false(二进制的0） 
		 * 
		 * 引用数据类型
		 * Array作为元素的默认数据类型也是null
		 * String: null
		 */
		int[] arr = new int[4];
		for(int i = 0; i<arr.length;i++) {
			System.out.println(arr[i]);
		}
		char[] arr1 = new char[4];
		for(int i = 0; i<arr1.length;i++) {
			System.out.println("-----"+arr1[i]+"-----");		
		}
		if(arr1[0]==0) {
			System.out.println("ok");
			
		}
		boolean[] arr2 = new boolean[2];
		System.out.println(arr2[0]);
		String[] arr3 = new String[2];
		System.out.println(arr3[0]);
		if(arr3[0]==null) {
			System.out.println("ok");
		}			
	}
}
```

6.数组的内存解析<sup>1</sup>
		 /*简化结构
		 栈 stack线性关系：局部变量，在方法中定义的变量都是局部变量
		 队 heap：new出来的结构 对象、数组
		 方法区 method area，包括常量池，静态域		 
		 */

注1：[JVM内存模型解析](https://www.youtube.com/watch?v=ZFiYxdWKft8&list=PLTsrYYJ5DyAnRKvHbZ7LslqC_wQ1iOQsF&index=3&t=0s)

#### 练习：学生成绩

```java
import java.util.Scanner;
public class ArrayExer {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("input amounts of students");
		int amounts = scan.nextInt();		
		int score[] = new int[amounts];
		int max = 0;
		System.out.println("input score of students");
		for(int i=0;i<amounts;i++) {
			score[i] = scan.nextInt();
			if(score[i]>=max){
				max = score[i];
			}
		}
		System.out.println("the highest score is "+max);
		for(int i = 0;i<amounts;i++) {
			if(score[i]>=(max-10)){
				System.out.println("student "+i+" level:"+"A"+" "+score[i]);
			}else if(score[i]>=(max-20)) {
				System.out.println("student "+i+" level:"+"B"+" "+score[i]);
			}else if(score[i]>=(max-30)) {
				System.out.println("student "+i+" level:"+"C"+" "+score[i]);
			}else {
				System.out.println("student "+i+" level:"+"D"+" "+score[i]);
			}
		}	
		
	}

}
```



### 3.2 二维数组

#### 3.2.1 概念

多维数组(以二维为主）
理解一维数组array1又作为另一一维数组array2的元素而存在。
其实从数组底层的运行机制来看，没有多维数组。
array2的堆中存放的是array1的地址  栈> 堆中的地址> 堆中另一个数组

#### 3.2.2 二维数组的使用

1. 二维数组的声明和初始化
2. 如何调用数组的制定位置的元素
3. 如何获取数组的长度
4. 如何遍历数组
5. 数组元素的默认初始化值
6. 数组的内存解析

```java
package com.atguigu.java;

/*
多维数组的使用(以二维为主）
1.理解一维数组array1又作为另一一维数组array2的元素而存在。
其实从数组底层的运行机制来看，没有多维数组。
2.array2的堆中存放的是array1的地址  栈> 堆中的地址> 堆中另一个数组

2.二维数组的使用：
1）声明和初始化 静态和动态
2）赋值和调用
3）长度
4）遍历（索引）
5）默认初始化值
6）内存解析

*/

public class ArrayTest2 {
	public static void main(String[] args) {
		//1.二维数组的声明和初始化
		//静态初始化
		int[] arr = new int[] {1,2};
		int[][] arr1 = new int[][]{{1,2},{3,4},{6,7,8}};
		//动态初始化1
		String[][] arr2 = new String[3][2]; // 3个地址 指向3个另外的数组
		//动态初始化2
		String[][] arr3 = new String[3][];
		//还没有赋值 外面的数组值是null，还没有指向的数组，所以也没有在堆中预留内部数组的空间	
		//另外的正确写法（较少用）：
		int arr4[][] = new int[2][2];
		int[] arr5[] = new int[2][2];
		int arr6[] = new int[2];
		int[] arr7 = {1,2,3};//类型推断
		int[][] arr8 = {{1,2},{3,4}};
		String[] arr9 = {"A","B"};
		//但是分行的是不能省略的，只有一行的是可以省略的
		
		//2.如何调用数组制定位置的元素
		System.out.println(arr1[0][1]); //2
		System.out.println(arr2[1][1]); //null
//		System.out.println(arr3[1][0]); //报错NullPointerException 空指针
		
		arr3[1] = new String[4]; 
		//在外面数组的索引1位置处，new一个4元素的string数据的空间 
		//地址（16进制0x）赋值到索引位置1，等于创建了以个指针
		System.out.println(arr3[1][0]);
	
		//3.获取长度
		System.out.println(arr1.length);   //3   length和内部的数组无关
		System.out.println(arr1[0].length);//2   length和内部的数组无关
		System.out.println(arr1[2].length);//3
		
		//4.如何遍历
		//嵌套循环
		//二维数组：两层for循环
		for(int i = 0;i<arr1.length;i++) {
			for(int j = 0; j<arr1[i].length;j++) {
			System.out.println(arr1[i][j]+" ");
			}
			
		}
		
		//5.二维数组的元素的默认初始化值
		/*
		 分为：外层数组的元素，内层数组的元素
		 初始化方式1：
		 int[][] arr = new int[4][3];
		 外层元素：arr[0],arr[1];
		 内层元素：arr[0][1],arr[2][3];	
		 外层元素的初始化值为null		 
		 内层元素的初始化值与一维相同 
		 
		 初始化方式二：
		 int[][] arr = new int[4][];
		 外层元素：null
		 内层元素：无法调用，报错		 
		 
		 */
		int[][] arr10= new int[4][3];
		System.out.println(arr10[0]);  //[I@424c0bc4地址值 在arr[0]位置的数组的地址值
		System.out.println(arr10[0][0]); //0
		System.out.println(arr10);   //[[I@3c679bde "[["表示二维 "I"表示int @后面是16进制的数组
		int[] arr11 = new int[2];
		System.out.println(arr11);//[I@3c679bde
		
		float[][] arr12= new float[4][3];
		System.out.println(arr12[0]);  //[F@8807e25
		System.out.println(arr12[0][0]); //0.0
		System.out.println(arr12); //[[F@63e31ee
		
		String[][] arr13= new String[4][3];
		System.out.println(arr13[0]);  //[Ljava.lang.String;@68fb2c38
		System.out.println(arr13[0][0]); //null
		System.out.println(arr13); //[[Ljava.lang.String;@567d299b
		
		double[][] arr14= new double[4][];
		System.out.println(arr14[0]);  //null
		
	
	}
	}
```

6.内存结构
        //main方法结束后，变量先进后出，出栈，指针就没有了，回收内存，堆中回收外层，之后内层元素失去指针也没回收。

方法中变量都是局部变量，局部变量都是在栈中。

地址是JVM算出来的hash值，不是底层内存真实的地址
引用类型的变量，存的不是null就是地址值

<img src="https://i.loli.net/2020/06/16/N9Xoq3bajlTyvV7.png" alt="二维数组的内存解析" style="zoom:85%;" />

数据结构

1. 数据与数据之间的逻辑关系：集合、一对一、一对多、多对多
2. 数据的存储结构
   - 线性表：一对一关系 顺序表（如：数组）、链表、栈、队列
   - 树形结构：二叉树 数据库中的B+树
   - 图形结构（多对多）

算法：

1. 排序算法
2. 搜索算法

#### 练习：打印杨辉三角

```java
//使用二维数组打印一个10行的杨辉三角
class YangHuiTriangle {
	public static void main(String[] args) {
		int[][] arr = new int[10][];
		for(int i = 0;i<10;i++) {
			arr[i]= new int[i+1];
			arr[i][0] = 1;//首末赋值
			arr[i][i] = 1;
			for(int j=1;j<arr[i].length-1;j++) {//这里用length作为判断条件。
			arr[i][j]=arr[i-1][j-1]+arr[i-1][j]; //某行等于上行的两个数的相加
			}
					
		}
		for(int i=0;i<(arr.length);i++) {
			for(int j = 0; j<arr[i].length;j++) {
				System.out.print(arr[i][j]+" ");
			}
			System.out.println();
		}
	}
}

```

### 3.3 数组涉及常见算法

/*
数组涉及的常见算法：
1.**数组元素的赋值**（杨辉三角、回形数）
2.求数值型数组中元素的最大值、最小值、平均数、总和
3.数组的赋值、翻转、查找（线性查找、**二分法查找**）
4.数组元素的**排序算法**（冒泡算法）
 */

#### 3.3.1 数组元素赋值

杨辉三角/回形树

练习：创建一个int[6]数组，要求元素在1-30之间，且是随机赋值，同时要求元素的值各不相同;

```java
public static void main(String[] args) {		
		int[] arr = new int[6];
//		int temp=(int)(Math.random()*30+1);
		for(int i = 0; i<6;i++) {
			arr[i] = (int)(Math.random()*30+1);			
//			while(temp==arr[i]) {
//				arr[i] = (int)(Math.random()*30+1);
//			}
			//temp = arr[1];
			for(int j = 0;j<i;j++) {
				while(arr[i]==arr[j]) {
					arr[i] = (int)(Math.random()*30+1);
				}
			}
		}
		for(int i = 0;i<6;i++) {
			System.out.println(arr[i]);
		}	
	}
```
#### 3.3.2 最大、最小，和、平均
```java
class AlgorithmStatistic {
	public static void main(String[] args) {
		int[] arr2 =new int[10];
		int max=0;
		int min=100;
		int sum=0;
		double avg=0;
		for(int i = 0;i<10;i++) {
			arr2[i]=(int)(Math.random()*90+10);			
		}
		for(int i = 0;i < 10;i++) {
			if(arr2[i]>max) {
				max =arr2[i];
			}
			if(arr2[i]<min) {
				min =arr2[i];
			}
			sum += arr2[i];
			
		}
		for(int i = 0;i<10;i++) {
			System.out.print(arr2[i]+" ");
		}
		avg = sum/(double)arr2.length;
		System.out.println();		
		System.out.println(max);
		System.out.println(min);
		System.out.println(sum);
		System.out.println(avg);
	
	 }		
}
```



#### 3.3.3 赋值、翻转、查找

```java
class AlgorithmAssignment {
	public static void main(String[] args) {
		int[] array1 = new int[] {2,3,5,7,11,13,17,19};
		int[] array2;
		for(int i = 1;i<array1.length;i++) {
			System.out.print(array1[i]+" ");
		}
		System.out.println();
		array2=array1;   //堆空间中实际上只有一个数组，array2得到array1的赋值是个地址值
		//new一次只有一个数组
		//不能称作数组的复制，未在堆中开辟新的内存空间，类似于快捷方式
		for(int i = 0;i<array1.length;i++) {
			if(i%2==0) {
				array2[i]=i;
			}
		}		
		for(int i = 1;i<array1.length;i++) {
			System.out.print(array1[i]+" ");
		}
		System.out.println();
		System.out.println(array1);//[I@424c0bc4		                          
		System.out.println(array2);//[I@424c0bc4		
	}
}


//实现array2数组的复制
class AlgorithmReplicate{
	public static void main(String[] args) {
		int[] array11 = new int[] {2,3,5,7,11,13,17,19};
		int[] array22 = new int[array11.length];		
		for(int i = 1;i<array11.length;i++) {
			array22[i]=array11[i];
		}
		for(int i = 1;i<array11.length;i++) {
			System.out.print(array11[i]+" ");
		}
		System.out.println();
		for(int i = 1;i<array22.length;i++) {
			System.out.print(array22[i]+" ");
		}
		System.out.println();
		for(int i = 0;i<array22.length;i++) {
			if(i%2==0) {
				array22[i]=i;
			}
		}
			for(int i = 1;i<array22.length;i++) {
				System.out.print(array22[i]+" ");
			}
			System.out.println();
			
		
		System.out.println(array11);//[I@424c0bc4		                          
		System.out.println(array22);//[I@424c0bc4	
	}
}

//实现array2数组的反转
//写法1:多new了一个数组，要两个变量，效率不高
class AlgorithmReverse2{
	public static void main(String[] args) {
		String[] array3 = new String[] {"a","b","dd","jj","uu","haha"};
		String[] array4 = new String[array3.length];
		for(int i = 0;i<array3.length;i++) {
			for(int j = array3.length-i-1;j>=0;j--) {
				array4[j]=array3[i];
			}
		}
		for(int i = 0;i<array4.length;i++) {
			System.out.print(array4[i]+" ");
		}
		
		
	}
	
}
//写法2
class AlgorithmReverse{
	public static void main(String[] args) {
		String[] array5 = new String[] {"a","b","dd","jj","uu","haha"};
	//	String[] array6 = new String[array5.length];
//		for(int i = 0;i<array5.length/2;i++) {
//			String temp = array5[i];
//			array5[i]=array5[array5.length-i-1];
//			array5[array5.length-i-1]=temp;
//		}
		//方法三：
		for(int i=0,j=array5.length-1;i<j;i++,j--){
		String temp1=array5[j];
		array5[j]=array5[i];
		array5[i]=temp1;
	}
		for(int i = 0;i<array5.length;i++) {
			System.out.print(array5[i]+" ");
		}	
		
	}
	
}

//查找（或搜索）
//线性查找：一个个找
import java.util.Scanner;
public class AlgorithmSearch {
public static void main(String[] args) {
	String[] search = new String[]{"A","B","CC","FF","PPP"};
	Scanner scan =new Scanner(System.in);
	System.out.println("please input the object you wanna search:");
	String object = scan.next();
	boolean isflag = true;
	for(int i = 0;i<search.length;i++) {
		while(object.equals(search[i])) {
			System.out.println("the index of object is "+i);
			isflag=false;
			break;
		}
	}
	while(isflag) {
	System.out.println("not found");
	break;
	}


//二分法查找：
	//前提：所有查找的数组必须有序
	int[] arr2 = new int[] {-98,-22,1,23,46,98,210,333,456};
	Scanner scan1 = new Scanner(System.in);
	System.out.println("input:");
	int object1 = scan1.nextInt();
	int head = 0;//初始首索引
	int end = arr2.length-1;
	boolean isflag2= false;
			while(head <= end) {
				int middle = (head+end)/2;
				if(object1==arr2[middle]) {
					System.out.println("index： "+middle);
					isflag = true;
					break;
				}else if(object1 > arr2[middle]) {
					head = middle+1;
				}else {
					end = middle-1;
				}
				if(isflag2) {
					System.out.println("not found");
				}
				
			}
	
	}
}

//差值法 哈希算法
```

#### 3.3.4 排序

```java
package com.atguigu.java;

/*
假设n个记录的序列为{R1 R2 ... Rn},其相应的关键字为{K1,K2...Kn}
重新排序{Ri1,Ri2...Rin},满足{Ki1<=Ki2...<=Kin},通常排序的目的是为了快速查找

衡量优劣： 
时间复杂度 
空间复杂度 
稳定性：关键字相等的A,B，排序后先后书序不变

分类：
内部排序
外部排序：需要借助内存之外的磁盘储存

十大内部排序算法：
选择排序：直接选择排序、**堆排序**
**交换排序**：冒泡、快速排序
插入排序：直接插入、折半插入、Shell排序（希尔排序）
**归并排序**
桶式排序（较少）
基数排序（较少）

算法的五大特征：
输入
输出
有穷性
确定性
可行性
注：非确定性算法：并行算法 概率算法 深度学习
不要求终止的计算描述：过程
 
 */

//数组的冒泡排序的实现
//n个元素，n-1趟，比较相邻的两个元素
public class AlgorithmSort {
	public static void main(String[] args) {
		int[] arr = new int[] {0,23,10,-45,99,78,45,3};
		for(int i = 0; i<arr.length-1;i++) {
			for(int j = 0; j<arr.length-1-i;j++) {
				if(arr[j]>arr[j+1]) {
					int temp = arr[j];
					arr[j] = arr[j+1];
					arr[j+1] = temp;
				
				}
			}
		}
		for(int i=0;i<arr.length;i++) {
		System.out.println(arr[i]);
		}
//快速排序：递归  (思想)
		
	}

}

//排序算法的横向对比
```

### 3.4 Arrays 工具类的使用

```java
package com.atguigu.java;
/*
 java.util.Arrays:操作数组的工具类,定义了很多操作数组的方法
 */
import java.util.Arrays;
public class ArraysTest {
	public static void main(String[] args) {
		//1.boolean equals(int[] a, int[] b):判断两个数组是否相等
		int[] arr1 = new int[] {1,2,3};
		int[] arr2 = new int[] {1,3,2};
		boolean isEquals = Arrays.equals(arr1,arr2);
		System.out.println(isEquals);
		
		//String toString(int[] a):输出数组信息
		System.out.println(Arrays.toString(arr1));
		
		//void fill(int[] a, int val):将指定值填充到数组
		Arrays.fill(arr1, 10);
		System.out.println(Arrays.toString(arr1));
	
		//void sort(int[] a):排序
		Arrays.sort(arr2);
		System.out.println(Arrays.toString(arr2));//[1, 2, 3]				
		System.out.println(arr2.toString());//注意两者的区别[I@16b4a017
		//System.out.println(arr2[0].toString());  //int型不能用该方法
		
		//int binarySearch(int[] a,int key)
		int[] arr3 = new int[] {1,2,3,4};
		int index = Arrays.binarySearch(arr3,5);
		System.out.println(index);//返回负数就是未找到
		
	}	

}
```

### 3.5 数组中的常见异常

```java
package com.atguigu.java;

/*
数组中的常见异常：
1.数组角标越界异常：ArrayIndexOutOfBoundException

2.空指针异常：NullPointerException

*/
public class ArrayException {
	public static void main(String[] args) {
		int[] arr = new int[] {1,2,3};
		//System.out.println(arr[5]);
		//System.out.println(arr[-1]);
		
		//情况1：
//	    int[]arr1 = null;
//	    System.out.println(arr1[0]);
	
		//情况二：
		int[][] arr2 = new int[4][];
		System.out.println(arr2[0]);//null
//		System.out.println(arr2[0][0]);//空指针
	
		//情况三：
		String[] arr3 = new String[] {"a","b","c"};
		arr3[0] = null;
		System.out.println(arr3[0].toString());//空指针
	}
}

```

