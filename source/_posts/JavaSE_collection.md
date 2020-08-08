---
title: Java-集合
date: 2020-06-28 00:24:46
tags: Java
urlname: java-collection
category: 笔记
description:  Java基础：集合
toc: true
---

![集合框架](https://i.loli.net/2020/06/30/sNBg92cYCWdZSua.png)



# 一、Collection

## 常用方法

```java
package com.landfill.java;

import org.junit.Test;

import java.time.LocalDateTime;
import java.util.*;

/*
一、集合框架的概述
1.集合、数组都是对多个数据进行存储操作的结构，简称Java容器
   存储：都是内存层面的，不涉及到持久化的存储（.txt .jpg 数据库中）
2.1 数组在储存多个数据方面的特点：
    - 一旦初始化以后，其长度就确定了。 //不能改
    - 一定定义好数组，其元素的类型也就确定了。只能操作指定类型的数据。  //其实是好处，数据类型严格
       但是可以使用多态性，放子类对象
2.2 数组在存储数据的缺点：
    - 一旦初始化，其长度就不可修改，如果需要扩容就较难处理
    - 数组中提供的方法非常有限，多余添加、删除、插入数据等操作，非常不便且效率不高。
    - 获取数组中实际元素的个数的需求，数组没有现成的属性和方法和使用
    - 数组存储的数据特点： 有序、可重复。对于无序和不可重复的数据的需求，无法满足

二、集合框架
    |---Collection接口  单列集合，用来储存一个一个的对象
        |---List接口   有序的可重复的数据       -->动态数组
            |---ArrayList Linkedlist Vector
        |---Set接口    无序的不可重复的数据
            |---HashSet LinkedHashSet TreeSet
    |---Map接口        双列数据，使用一对一对（键值对）的数据  一个key不能对应多个value，但是一个value可以有多个key指向
            |---HashMap LinkedHashMap TreeMap HashTable Properties

集合分为Collection 和 Map（映射）
Collection ：单列数据  实线是继承，虚线是实现关系
    - list接口：有序可重复
    - set接口：无序  不可重复
Map： 双列数据  保存具有映射关系的 “key-value对” 的集合

三、Collection接口中的方法
抽象方法
向Collection接口的实现类的对象中添加对象obj时，需要重写equals()  contains()，remove()才能使用

 */
public class Collections {
    @Test
    public void test1(){
        Collection coll = new ArrayList();

        //add(Object e)   添加元素到集合中
        coll.add("aa");
        coll.add("bb");
        coll.add(123);  //自动装箱
        coll.add(new Date());

        //size()     获取添加的元素的个数
        System.out.println(coll.size());

        //addAll()   将参数集合中的元素，添加到调用该方法的集合中
        ArrayList coll1 = new ArrayList();
        coll1.add(34);
        coll1.add(67);
        coll1.addAll(coll);

        System.out.println(coll1.size());
        System.out.println(coll1);

        //isEmpty 判断size是否为0
        System.out.println(coll.isEmpty());

        //clear()  清空元素，但不是null
        coll1.clear();
        System.out.println(coll1);

        //contains()  是否包含某个对象
        System.out.println(coll.contains(123));
        coll.add(new String("tom"));
        System.out.println(coll.contains(new String("tom")));//true,这里相当于是调用String类中的equals方法
        coll.add(new Person(20,"tom"));
        System.out.println(coll.contains(new Person(20, "tom")));
        // 如果要判断内容，要在类中重写equals()  false-->true,需要调用多此，把contains参数的对象
        // 调用equals()和集合中的每个对象去比较，集合中的元素作为equals的参数，直到找到为止

        // containsAll(Collection coll1)  判断coll1的元素是否全部在调用该方法的集合中
        coll1.add(123);
        System.out.println(coll.containsAll(coll1));

    }
    @Test
    public void test2(){

        //remove() 删除 有返回值，删除成功返回true ；指定的元素不存在 就返回false
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add(456);
        coll.add(new Person(29, "tom"));
        coll.add(new Date());

        System.out.println(coll.remove(123));   //也需要重写equals()先判断是否有元素才能删除
        System.out.println(coll);  //直接sout对象名 相当于调用ArrayList的toString()


        //removeAll(Collection coll1)  删除和coll1重合的部分的元素  交集部分  得到的是差集
        Collection coll1 = new ArrayList();
        coll1.add(456);
        coll1.add(new Date());
        coll.add(23);
//      coll.removeAll(coll1);
        System.out.println(coll);
        System.out.println(coll1);

        //coll.retainAll(Collection coll1)   求交集部分，然后把交集给了coll
        coll.retainAll(coll1);
        System.out.println(coll);





    }
    @Test
    public void test3(){
        //equals()   判断当前集合和形参集合的元素是不是相同（是否考虑顺序要考虑集合的实例
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add(123);
        coll.add(456);
        coll.add(new Person(29, "tom"));
        coll.add(new Date());
        coll.add(LocalDateTime.now());

        Collection coll1 = new ArrayList();
        coll1.add(123);
        coll1.add(456);
        coll1.add(456);
        coll1.add(new Person(29, "tom"));
        coll1.add(new Date());
        coll1.add(LocalDateTime.now());  //LocalDatetime是瞬时的。所以是两个不同的对象，不同于Date对象
        System.out.println(coll);
        System.out.println(coll1);
        System.out.println(coll.equals(coll1));  //true
        //如果调换add元素的顺序就会变成false 因为Arraylist是有序的 可重复的
        coll.removeAll(coll1);
        System.out.println(coll);   //就把相同的元素全部删完，即使出现了两边也是
    }

    @Test
        //hashCode() 返回当前集合对象的hashcode
    public void testHashCode(){
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add(123);
        coll.add(456);
        System.out.println(coll.hashCode());  //根据元素计算出来的

        //toArray()  把集合转换为数组
        Object[] o = coll.toArray();
        for (int i = 0; i <o.length ; i++) {
            System.out.println(o[i]);
        }

        //拓展： 数组也能转为集合中的list 调用 Arrays.asList();静态方法
        List<String> list = Arrays.asList(new String[]{"aa", "bb"});

        System.out.println(list);
        List<int[]> list1 = Arrays.asList(new int[]{1,2});
        System.out.println(list1);   //[[I@1fc2b765]  把整个int[] 当成一个元素了

        List list2 = Arrays.asList( 3, 5);
        System.out.println(list2);     // [3, 5]
        List<Integer> list3 = Arrays.asList(new Integer[]{1, 2, 3});
        System.out.println(list3);     //[1, 2, 3]   使用int型的时候要注意

        //Iterator():返回Iterator接口的实例，用于遍历集合元素，后面讲
    }

}

 class Person{
    int age;
    String name;

    public Person(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        System.out.println("person equals");
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age &&
                name.equals(person.name);
    }

    @Override
    public String toString() {
        return "Person{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
    //    @Override
//    public int hashCode() {
//        return Objects.hash(age, name);
//    }
}
```

## 新的遍历方式

### 增强for循环

```java
package com.landfill.java;
/*
foreach循环 after jdk5.0  增强for循环
用于遍历，从集合中取第一个元素赋值给obj，再打印obj，其实内部是个迭代器

 */

import org.junit.Test;

import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.Collection;
import java.util.Date;

public class ForeachTest {
    @Test
    public void test(){
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add(123);
        coll.add(456);
        coll.add(new Person(29, "tom"));
        coll.add(new Date());
        coll.add(LocalDateTime.now());

        //for( 集合中元素的类型 局部变量：集合对象）
        for(Object obj:coll){  //其实内部是个迭代器
            System.out.println(obj);

        }

        int[] arr = new int[]{1,2,3,4,5,6};
        for(int i:arr){   //形参的对象类型不同于集合，数组的类型的是确定的
            System.out.println(i);
        }


    }
    @Test
    public void test2(){
        String[] arr = new String[]{"mm","mm","mm"};
        //普通for循环赋值
        for (int i = 0; i < arr.length; i++) {
            arr[i] = "gg";
        }

        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);   //"gg"
        }

        //普通for循环赋值
        for (String str :arr) {
            str = "mm";   //将数组元素的地址值赋值给形参，改变的是形参，除了for循环，形参出栈，不改变原数组的元素
        }

        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);   //"gg"
        }
    }
}
```

### iterator

```java
package com.landfill.java;

import org.junit.Test;

import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.Collection;
import java.util.Date;
import java.util.Iterator;

/*
迭代器Iterator

集合元素的遍历，使用Iterator接口    容器：集合、数组
Collection接口继承了Iterator接口，所以只能用来遍历Collection，不能遍历Map

 - Collection接口继承了java.lang.Iterable接口，该接口有一个iterator()方法，
 那么所有实现了Collection接口的集合类都有一个iterator()方法，用以返回一个实现了 Iterator接口的对象。
 - Iterator 仅用于遍历集合，Iterator 本身并不提供承装对象的能力。如果需要创建 Iterator 对象，则必须有一个被迭代的集合。
 - 集合对象每次调用iterator()方法都得到一个全新的迭代器对象，默认游标都在集合 的第一个元素之前。



 执行原理：
 指针 hasNext() 返回true 调用next()放回元素 指针下移
 如果没有元素了，还调用next()会报异常 NoSuchElementException

 方法
 hasNext()
 next()
 remove()  删除集合中的元素，不同于集合中的remove() 是Iterator的方法
  如果还未调用next()或在上一次调用 next 方法之后已经调用了 remove 方法， 再调用remove都会报IllegalStateException。
 迭代器的remove()一定要跟在next()后面

 */
public class IteratorTest {
    @Test
    public void test2(){
       Collection coll = new ArrayList();
       coll.add(123);
       coll.add(123);
       coll.add(456);
       coll.add(new Person(29, "tom"));
       coll.add(new Date());
       coll.add(LocalDateTime.now());
       Iterator iterator = coll.iterator();

       //删除集合中的指定元素
       while(iterator.hasNext()){
           Object obj = iterator.next();
           if(coll.contains(123)){
               iterator.remove();
           }
       }
       //重新new一个iterator 遍历删除后集合
        Iterator iterator1 = coll.iterator();
        while(iterator1.hasNext()) {
            System.out.println(iterator1.next());
        }
   }

    @Test
    public void test(){
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add(123);
        coll.add(456);
        coll.add(new Person(29, "tom"));
        coll.add(new Date());
        coll.add(LocalDateTime.now());
        Iterator iterator = coll.iterator();
        System.out.println(iterator.hasNext());
//        System.out.println(iterator.next());
//        System.out.println(iterator.next());
//        System.out.println(iterator.next());
//        System.out.println(iterator.next());
//        System.out.println(iterator.next());

//        for (int i = 0; i <coll.size() ; i++) {
//            System.out.println(iterator.next());
//        }
        // 使用迭代器去遍历集合元素  一般写法
        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }

        //错误方式一： 每次调用next() 指针会下移，会跳着输出
//        Iterator iterator1 = coll.iterator();
//        while ((iterator.next())!=null){
//            System.out.println(iterator.next());
//        }

//        错误方式二： 每次调用iterator()都会生成一个新的对象，默认游标都会在第一个元素之前。所以会不停输出第一个元素
//        while (coll.iterator().hasNext()){
//            System.out.println(coll.iterator.next());
//        }


    }

}
```

# 二、List

```java
package com.landfill.java2;
/*
Collection子接口之一： List接口

一、List接口基本情况
通常用来替代数组,不用考虑角标越界
元素有序，且可重复，集合中每个元素都有其对应的顺序的索引，可以根据序号来存取容器中的元素
List接口的实现类常用的有：ArrayList LinkedList Vector
添加元素所在类要重写equals()

二、List接口的实现类
|---Collection接口  单列集合，用来储存一个一个的对象
        |---List接口 since v1.2 有序的可重复的数据       -->动态数组，替换原有的数组
            |---ArrayList   主要实现类   v1.2  线程不安全的，效率高，底层使用Object[]存储，数组插入要后面的要前移一位
            |---Linkedlist              v1.2  底层使用双向链表（通过双向地址连接）存储 对于频繁的插入、删除操作，效率比ArrayList高
            |---Vector      古老实现类   v1.0  线程安全的，效率低   底层使用Object[]存储 vector扩容为2倍，默认创建长度为10的数组
        char[]的StringBuffer（线程安全）和StringBuilder（线程不安全）也是一种容器
比较ArrayList Linkedlist Vector 三者的异同
相同：都实现了List接口 ，存储数据的特点相同（可重复、有序）
不同：上述点

三、ArrayList源码分析
  1. ArrayList源码分析： jdk7.0情况下
    ArrayList list = new ArrayList();   //默认空参构造器，底层创建了长度是10的Object[] elementData = new Object[10];

    当添加的元素使得容量不足时，就自动进行扩容  类似于StringBuilder
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);  //扩容为原来的1.5倍，右移是除以2
    if (newCapacity - minCapacity < 0)                   //扩容还不够，就用这个minCapacity
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)                //超过整型的情况 2的32次方
        newCapacity = hugeCapacity(minCapacity);
   // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);   //把原来的数组的元素copy过来

    结论：在开发中如果确定容量，使用带参数的构造器  ArrayList list = new ArrayList(int capacity)

   2.ArrayList jdk8.0
   在创建的数组的时候，初始化时候是 创建了个空的数组{}  //节省内存空间
   在add()的时候才创建长度为10的Object[],并将数据添加到elementData的位置上，后续的添加和扩容操作和JDK7.0一致
   3.小结：7.0中的ArrayList对象创建  类似单例模式的饿汉式，8.0类似懒汉式，节省了内存空间

四、Linkedlist源码分析
顺序表和链表是数据结构中最基本的单位
顺序表：适合查找 末尾添加。不用维护键值对，可以通过索引查找
Node类型的first和last 属性，默认值为null.
list.add(123)// 将123封装到Node，创建Node对象
内部类：Node  体现了Linkedlist双向链表的特征，不涉及扩容
 private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;

五、List接口中的常用方法（Collection没有）
void add(int index, Object ele):在index位置插入ele元素
boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
Object get(int index):获取指定index位置的元素
int indexOf(Object obj):返回obj在集合中首次出现的位置
int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
Object remove(int index):移除指定index位置的元素，并返回此元素
Object set(int index, Object ele):设置指定index位置的元素为ele
List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex 位置的子集合

总结：常用方法
增 add()
删 Object remove(int index)
改 Object set(int index, Object ele)
查 Object get(int index)
插 add(int index, Object ele)
长度  size()
遍历: Iterator  foreach for

 */



import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;

public class ListTest {
    @Test
    public void test2(){
        ArrayList list = new ArrayList();
        list.add("aa");
        list.add("bb");
        list.add(123);

        //遍历  方式一：iterator
        Iterator iterator = list.iterator();
        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }

        //遍历  方式二：foreach
        for(Object obj:list){
            System.out.println(obj);
        }

        //遍历  方式三：for
        for (int i = 0; i <list.size() ; i++) {
            System.out.println(list.get(i));
        }


    }
    @Test
    public void test1(){
        ArrayList list = new ArrayList();
        list.add("aa");
        list.add("bb");
        list.add(123);
        list.add(456);
        list.add(new Person(20,"tom"));
        System.out.println(list);

        //void add(int index,Object ele):在index位置插入ele元素
        list.add(1,343);
        System.out.println(list);

        //boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
        List integers = Arrays.asList(1, 2, 3);
        list.addAll(1, integers);
        System.out.println(list);
        list.addAll(integers);
        System.out.println(list);   //把integers作为一个元素

        //Object get(int index):获取指定index位置的元素
        System.out.println(list.get(2));

        //indexOf(Object obj):返回obj在集合中首次出现的位置,如果不存在返回-1
        System.out.println(list.indexOf("aa"));
        //lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
        System.out.println(list.lastIndexOf("aa"));

        //remove(int index):移除指定index位置的元素，并返回此元素，对Collection里的remove的重载，这里是按索引删除
        Object obj = list.remove(0);
        System.out.println(obj);
        System.out.println(list);
        //set(int index, Object ele):设置指定index位置的元素为ele
        list.set(0, "gg");
        System.out.println(list);

        //List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex 位置的子集合
        List subList = list.subList(0, 4);   //左闭右开
        System.out.println(subList);
        System.out.println(list);

    }

}
class Person {
    int age;
    String name;

    public Person(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        System.out.println("person equals");
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age &&
                name.equals(person.name);
    }

    @Override
    public String toString() {
        return "Person{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}
```

# 三、Set

```JAVA
package com.landfill.java2;

import org.junit.Test;

import java.util.*;

/*
Collection子接口之二：Set接口
一、框架
|---Collection接口  单列集合，用来储存一个一个的对象   开发中用Map和List比较多，Set偏少
        |---Set接口    无序的，不可重复的数据
            |---HashSet   作为Set接口的主要实现类，线程不安全的，可以存null值，可以add(null)
                |---LinkedHashSet 是HashSet的子类 遍历是按照添加的顺序输出(但还是无序的）
            |---TreeSet 使用红黑树储存，应用了Comparable和Comparator接口。可以按照添加对象的指定属性进行排序
三个实现类：HashSet LinkedHashSet TreeSet
Set接口没有额外定义新的方法，所以使用的都是Collection中定义过的方法
要求：向Set中添加数据，其所在的类一定要重写equals() hashCode() 以实现对象相等规则，
      重写的equals()和 hashCode() 尽可能保持一致性 --> 相等的对象必须具有相等的散列码

二、无序、不可重复
Set存储无序不可重复的数据(以HashSet为例）
1.无序性：不等于随机性，每次遍历都是输出相同的结果，不是按照索引的顺序添加元素，而是根据添加元素的Hash值来加入数组
2.不可重复性:保证添加的元素按照equals判断时，不能返回true。即相同的元素只能添加一个。
可以过滤重复数据

三、添加元素的过程：以HashSet为例
实际上是new了个HashMap
底层是一个数组+链表的结构，初始容量为16，如果使用率超过0.75 即16*0.75=12   16位就会扩容为原来的两倍32 64 128...

如果要添加元素a，调用a所在类的HashCode(),计算元素a hash值，此哈希值通过散列函数，得出a在底层数组中的存放位置（即索引位置），
    判断此位置上是否已经有元素：
    如果此位置没有其他元素，则此元素添加成功   --->情况1
    如果此位置已经有其他元素b，或者以链表形式存储的多个元素（数组的位置一样，但hash值不一定一样）
        比较a,b的hash值：
        如果hash值不一样，则添加成功，         --->情况2
        如果两个元素的hash值一样，就调用元素a所在类的equals(b)，  //equals()和hashCode()方法必须一致
                    返回值是true则添加失败
                    返回值是false则添加成功，仍然以链表存放  --->情况3

       对于添加成功的情况2和情况3，元素a与已经存在的于指定索引位置的数据以链表的形式储存
        JDK7.0： 元素a放到数组中，指向原来的元素
        JDK8.0： 原来的元素放在数组，元素a放在链表的最下面
        7上8下


//为什么如果要添加的元素所在的位置没有元素就添加成功了？
如果是调用Object类的hashcode，生成的是一个随机数，
就会导致两个user对象的hashcode肯定不一样，算出了的索引位置也不同，所以都没有调用equals方法。
我的理解是，通过类重写的hashcode算出来的更能反映是不是内容相同的对象
String重写的hashcode() 能搞保证两个相同的字符串的哈希值是相同的

name.hashCode()*31+age
31:质数、较大、5bits溢出概率小 ，i*31可以用 （i<<5）-1来计算


四、LinkedHashSet: 是HashSet的子类，底层用了双向链表，相当于在原有的HashSet的基础上，每个数据还维护了两个引用，
    // 记录此数据前后的数据，一个双向链表的结构，记录了添加顺序的先后，对于频繁的遍历操作，效率更加高一些

五、TreeSet
    TreeSet是用来排序，需要有相同的属性。所以向TreeSet中添加的对象必须是相同类的
    TreeSet的添加的元素所在类还需要重写compareTo()
    两种排序方式 自然排序和定制排序  比较器
    TreeSet是按照添加元素所在类的compareTo()来做为不可重复的标准的
    自然排序中比较两个对象的是否相同的标准为compareTo()，只要返回0，就认为是相同的，不是equals()

 */
public class SetTest {

    @Test
    public void test1() {
        HashSet set = new HashSet();
        set.add("aaa");
        set.add("bbb");
        set.add(123);
        set.add("BB");
        set.add(new User("tom", 12));
        set.add(new User("tom", 12));
        //两个user，因为在user类中没有重写equals类，不可重复就是调用了equals()实现的
        //没有调用equals()，如果没在类中重写HashCode，只有重写了HashCode才调用equals
        set.add(129);

        Iterator iterator = set.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());  //遍历的时候和添加的顺序不同，
        }

        System.out.println("----------------------------");

    }


    @Test

    //LinkedHashSet: 是HashSet的子类，底层用了双向链表，相当于在原有的HashSet的基础上，每个数据还维护了两个引用，
    // 记录此数据前后的数据，一个双向链表的结构，记录了添加顺序的先后，对于频繁的遍历操作，效率更加高一些
    public void test2() {
        HashSet set1 = new LinkedHashSet();
        set1.add("aaa");
        set1.add("bbb");
        set1.add(123);
        set1.add("BB");
        set1.add(new Person(12, "tom"));
        set1.add(129);

        Iterator iterator1 = set1.iterator();
        while (iterator1.hasNext()) {
            System.out.println(iterator1.next());  //遍历的时候和添加的顺序相同，
        }
    }

    @Test

//    TreeSet是用来排序，需要有相同的属性。所以向TreeSet中添加的对象必须是相同类的
//    两种排序方式 自然排序和定制排序  比较器
//    TreeSet是按照添加元素所在类的CompareTo()来做为不可重复的标准的
//    自然排序中比较两个对象的是否相同的标准为CompareTo()，只要返回0，就认为是相同的，不是equals()

    public void test3() {
//        TreeSet set = new TreeSet();
//        set.add(1312);
//        set.add(432);
//        set.add(4324);
//        set.add(-231);
//        set.add(-31);
//        Iterator iterator = set.iterator();
//        while (iterator.hasNext()){
//            System.out.println(iterator.next());   //遍历时是按照从小到大的顺序排序的
//        }

        TreeSet set = new TreeSet();
        set.add(new User("tom", 20));
        set.add(new User("jack", 23));
        set.add(new User("jack", 33));  //如果只写了一级排序，只能添加一个
        set.add(new User("Jacz", 29));  //大写的排前面
        set.add(new User("uit", 10));
        set.add(new User("oiur", 40));

        Iterator iterator = set.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());   //遍历时是按照从小到大的顺序排序的

        }
    }

    @Test
    public void test4() {
        Comparator comparator = new Comparator() {   //匿名实现类，创建一个对象，传入TreeSet的构造器
            //按照年龄从小到大的排序
            @Override
            public int compare(Object o1, Object o2) {
                if (o1 instanceof User && o2 instanceof User) {
                    User u1 = (User) o1;
                    User u2 = (User) o2;
                    return Integer.compare(u1.age, u2.age);
                } else {
                    throw new RuntimeException("输入数据不一致");
                }
            }
        };
        TreeSet set = new TreeSet(comparator);


        set.add(new User("tom", 20));
        set.add(new User("jack", 23));
        set.add(new User("jack", 33));  //如果只写了一级排序，只能添加一个
        set.add(new User("Jacz", 29));  //大写的排前面
        set.add(new User("uit", 10));
        set.add(new User("oiur", 40));
        set.add(new User("oiur", 40)); //comparator只有一级排序，所以后面的元素就会被舍弃
        //set.add(123);  //"输入数据不一致"
        System.out.println(set);
        Iterator iterator = set.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());


        }
    }
}

package com.landfill.java2;

import java.util.Objects;

public class User implements Comparable {
    String name;
    int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public User() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        System.out.println("equals");
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return age == user.age &&
                Objects.equals(name, user.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    //按照姓名从小到大，年龄从小到大
    @Override
    public int compareTo(Object o) {
        if(o instanceof User){
            User u = (User)o;
            int compare =  this.name.compareTo(((User) o).name); //从大到小就加个负号
            if(compare != 0){
                return compare;
            }else{
                return Integer.compare(this.age, u.age);  //integer的compare() 默认也是从小到大的
            }

        }else {
            throw new RuntimeException("输入类型不匹配");
        }
    }
}
```

# 四、Map

## HashMap & LindedHashMap

```java
package com.landfill.java;
/*
一、Map接口的结构
|---Map接口 since1.2  双列数据，使用一对一对（键值对）的数据  一个key不能对应多个value，但是一个value可以有多个key指向
            |---HashMap            作为主要的实现类 since1.2 线程不安全的，效率高，可以储存的null的key和value
                |---LinkedHashMap   since1.4 保证遍历Map元素时，可以按添加顺序遍历，在原基础上添加了一对引用指向前后元素，
                                    对于频繁的遍历操作，效率要比HashMap更高
            |---HashTable          作为古老的实现类 since1.0 线程安全的，效率低，不能存储null的key和value
                |---Properties      常用来处理配置文件。key和value都是String类型的
            |---SortedMap接口
                |---TreeMap        since1.2 按照添加的key-value对进行排序，实现排序排序，按照key来自然排序或者定制排序
                                    底层使用的是红黑树
            HashMap底层： 数组+链表       jdk7.0
                         数组+链表+红黑树 jdk8.0

问题：
1.HashMap的底层实现原理
2.HashMap和HashTable的区别
3.ConcurrentHashMap 与 HashMap的区别  分段锁

二、Map结构的理解 以HashMap为例
Key是无序、不可重复的（KeySet,使用Set存储）  -->key所在的类要重写equals() hashCode()
Value可以重复，无序的（Collection储存）    -->values所在的类要重写equals()
往Map里放的实际上是一个个的Entry，把key-value对作为两个属性装入Entry（无序不可重复），用set来储存
对比数学中的函数，x类似于key；f(x)类似于value

三、HashMap的底层实现原理  以JDk7.0为例
  HashMap map = new HashMap()
  在实例化后，底层创建了长度为16的一位数组Entry[] table;
  ...可能已经执行过多次put了...
  map.put(key1,value1);
  首先，调用key1所在类的hashCode计算key1哈希值，此哈希值经过某种算法后，得到在table中的存放位置
  如果此位置上的数据为空，此时的key1-value1(entry1)添加成功      ----情况1
  如过此位置上的数据不为空，意味着此位置上存在一个或多个数据（以链表存在），比较key1和已经存在的一个
  或多个的数据的哈希值：
        如果key1的哈希值与已经存在的数据都不相同，此时key1-value1添加成功      ----情况2
        如果key1的哈希值与已经存在的某一个数据（key2-value2）的哈希值相同，继续比较：调用key1所在类的equals(key2)
            如果反正false，此时key1-value1添加成功               ----情况1
            如果返回true,使用value1替换value2，如果key

      情况2和情况3：key1-value1以链表的方式存储
      在不断的添加过程中，会涉及到扩容问题。默认的扩容方式：扩容为原来容量的2倍，并将原有的数据复制过来

      jdk8相较于jdk7在底层实现的不同
      1.new HashMap(): 底层没有创建一个长度为16的数组
      2.jdk8底层的数据是：Node[]  而非entry[]
      3.首次使用put()才会创建
      4.jdk8变成了数组+链表+红黑树，新增了红黑树的结构
        当数组的某一个索引位置上的元素以链表形式存在的数据个数>8,且当前数组的长度>64时，MIN_TREEIFY_CAPACITY=64，如果
        数组长度不大于64，就不改红黑树，而是直接去扩容。
        此索引位置上的所有数据改为使用红黑树存储。 查找的时候遍历链表没有遍历红黑树的效率高

    在创建hashMap的时候去指定长度，会创建一个最接近且大于该数的2的n次方的容量的数组
    loadFactor 负载因子，0.75  兼顾数组的利用率和更少的链表结构
    threshold  = capacity*loadFactor
    DEFAULT_INITIAL_CAPACITY = 16
    DEFAULT_LOAD_FACTOR = 0.75
    TREEIFY_THRESHOLD = 8
    MIN_TREEIFY_CAPACITY=64

    因为存在哈希碰撞，虽然数组的位置只有12位，但是可能元素已经不止12个了，而且可能数组永远也放不满，为了减少链表结构的存在
    就要早一点进行扩容的操作。
    扩容的条件（大于threshold且，要添加元素的索引位置非空）
    如果索引位置一样，先看是否是null，不是null先比hash值，hash值和链表中都不一样，就添加（在数组或者链表里）。
    如果hash值一样先判断key是不是==或者equals，如果hash值且key都相同，就把旧的value给覆盖了

    hashCode()再作为hash()的参数
    单向链表 只有next，如果索引位置原来null，next就指向null

    JDK8.0的源码实现的区别

四、LinkedHashMap的底层实现原理
是HashMap的子类，用了HashMap的put()，但是重写了newNode(),LinkedHashMap的内部类Entry在继承父类的Node类时，
又多了一个before after的属性，去记录添加时的前后元素

源码：
static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after;                        //记录添加的元素的前后的元素
        Entry(int hash, K key, V value, Node<K,V> next) {
            super(hash, key, value, next);
        }
    }

HashSet其实就是通过new HashMap() 实现的，set的元素就相当于map里的key，所有的key都指向一个value是一个Object对象，没有实际意义。

五、Map中定义的方法
添加、删除、修改操作：
 Object put(Object key,Object value)：将指定key-value添加到(或修改)当前map对象中
 void putAll(Map m):将m中的所有key-value对存放到当前map中
 Object remove(Object key)：移除指定key的key-value对，并返回value
 void clear()：清空当前map中的所有数据

元素查询的操作：
 Object get(Object key)：获取指定key对应的value
 boolean containsKey(Object key)：是否包含指定的key
 boolean containsValue(Object value)：是否包含指定的value
 int size()：返回map中key-value对的个数
 boolean isEmpty()：判断当前map是否为空
 boolean equals(Object obj)：判断当前map和参数对象obj是否相等

元视图操作的方法：
 Set keySet()：返回所有key构成的Set集合
 Collection values()：返回所有value构成的Collection集合
 Set entrySet()：返回所有key-value对构成的Set集合

总结：
增   put
删   remove
改  put
查  get
长度  size
遍历  keySet  values entrySet
 */

import org.junit.Test;

import java.util.*;

public class MapTest {
    //  元素的增删改
    @Test
    public void test2(){
        //Object put()  put成功返回null，put重复的key 返回的是key旧的value 相当于map.get(key)
        //添加
        HashMap map = new HashMap();
        map.put("aa",123);  //开发中key和value的类型一般是确定的
        map.put("bb",123);
        map.put("gg",12);
        System.out.println(map.put("gg",123));


        //修改
        map.put("aa",1543);
        System.out.println(map);  //key相同的时候执行的是修改操作

        HashMap map1 = new HashMap();
        map1.put("gg", 234);
        map1.put("bb", 434);
        map1.put("xx", 134);


        //void putAll()
        //没有key就添加，有就修改
        map.putAll(map1);     //也是替换
        System.out.println(map);

        //Object remove(key)   //返回移除的key对应的value,不存在就放回null
        Object gg = map.remove("gg");
        System.out.println(gg);
        System.out.println(map);

        //void clear()
        map.clear();
        System.out.println(map.size());     //0  不是null
        System.out.println(map);            //{}
    }
//  元素的查询
    @Test
    public void test4(){
        HashMap map = new HashMap();
        map.put("aa",123);
        map.put("bb",123);
        map.put("gg",124);
        map.put("dd",553);

        //Object get(Object key)：获取指定key对应的value,不存在返回null
        System.out.println(map.get("aa"));

        //boolean containsKey(Object key)：是否包含指定的key
        System.out.println(map.containsKey("aa"));  //true

        //boolean containsValue(Object value)：是否包含指定的value
        System.out.println(map.containsValue(123)); //只找一个

        //int size()：返回map中key-value对的个数
        map.size();
        // boolean isEmpty()：判断当前map是否为空,判断的是size是不是为0
        map.isEmpty();
        //boolean equals(Object obj)：判断当前map和参数对象obj是否相等
        //得是两个map 且内容都一样 才是true
    }

    //元视图操作的方法： 用来遍历的方法
    @Test
    public void test5(){
        HashMap map = new HashMap();
        map.put("aaqwe",123);
        map.put("zz",4143124);
        map.put("gg",124);
        map.put("dd",553);
        System.out.println(map);
     // Set keySet()：返回所有key构成的Set集合
        Set set = map.keySet();
        Iterator iterator = set.iterator();
        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }
        // Collection values()：返回所有value构成的Collection集合，是和key的遍历的顺序是一样的
        Collection values = map.values();
        Iterator iterator1 = values.iterator();
        while (iterator1.hasNext()){
            System.out.println(iterator1.next());
        }

        // Set entrySet()：返回所有key-value对构成的Set集合
        Set set1 = map.entrySet();

        //方式一：
        Iterator iterator2 = set1.iterator();
        while (iterator2.hasNext()){
            Object obj = iterator2.next();  //都是entry对象
            Map.Entry entry = (Map.Entry)obj;
            System.out.println(entry.getKey()+"---"+entry.getValue());  //可以调用entry的方法

        }
        System.out.println("-------------");

        //方式二：
        Iterator iterator5 = set.iterator();
        while (iterator5.hasNext()){
            Object next = iterator5.next();
            System.out.println(next+"==="+map.get(next));
        }

        //方式三：
        Iterator iterator3 = set.iterator();
        Iterator iterator4 = values.iterator();
        while (iterator3.hasNext()&&iterator4.hasNext()){
            System.out.println(iterator3.next()+"@@@@"+iterator4.next());
        }

    }


    @Test
    public void test1(){
        Map map = new HashMap();
        map.put(null, 123);
        map.put(null, null);

        Map map1 = new Hashtable();
       // map1.put(null, 123);  //java.lang.NullPointerException
    }

}
```

## TreeMap

```java
package com.landfill.java;

// TreeMap
//向TreeMap 添加key-value时，key要有同一个类的对象创建
//因为要根据key来排序  自然排序、定制排序

import org.junit.Test;

import java.util.Comparator;
import java.util.Iterator;
import java.util.Set;
import java.util.TreeMap;

public class TreeMapTest {

    @Test
    public void test1(){
        TreeMap treeMap = new TreeMap();
        User u1 = new User("dat", 34);
        User u2 = new User("ewq", 42);
        User u3 = new User("jerrt", 42);
        User u4 = new User("azure", 88);
        treeMap.put(u1, 100);
        treeMap.put(u2, 56);
        treeMap.put(u3, 89);
        treeMap.put(u4, 30);

        System.out.println(treeMap);

    }

    //定制排序
    //只能根据key的属性去排序，不能用value排序
    @Test
    public void test2(){
        Comparator comparator = new Comparator() {

            @Override
            public int compare(Object o1, Object o2) {
                if(o1 instanceof User && o2 instanceof User){
                    User u1 = (User)o1;
                    User u2 = (User)o2;
                    if(u1.age != u2.age){
                        return Integer.compare(u1.age, u2.age);
                    }else{

                        return -u1.name.compareTo(u2.name);
                    }


                }else{
                    throw new RuntimeException("类型错误");
                }

            }
        };
        TreeMap treeMap = new TreeMap(comparator);
        User u1 = new User("dat", 34);
        User u2 = new User("ewq", 42);
        User u3 = new User("jerrt", 42);
        User u5 = new User("hash", 42);
        User u4 = new User("azure", 88);
        treeMap.put(u1, 100);
        treeMap.put(u2, 56);
        treeMap.put(u3, 89);
        treeMap.put(u4, 30);
        treeMap.put(u5, 30);

        Set set = treeMap.entrySet();
        Iterator iterator = set.iterator();
        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }


    }

}

package com.landfill.java;

import java.util.Objects;

public class User implements Comparable {
    String name;
    int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public User() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        System.out.println("equals");
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return age == user.age &&
                Objects.equals(name, user.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    //按照姓名从小到大，年龄从小到大
    @Override
    public int compareTo(Object o) {
        if(o instanceof User){
            User u = (User)o;
            int compare =  this.name.compareTo(((User) o).name); //从大到小就加个负号
            if(compare != 0){
                return compare;
            }else{
                return Integer.compare(this.age, u.age);  //integer的compare() 默认也是从小到大的
            }

        }else {
            throw new RuntimeException("输入类型不匹配");
        }
    }
}
```

## properties

```java
package com.landfill.java;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;

/*
Properties:HashTable的子类
处理配置文件，key value都是string
 */
public class PropertiesTest {
    public static void main(String[] args) throws IOException {
        Properties pros = new Properties();
        FileInputStream fis = new FileInputStream("jdbc.properties");
        pros.load(fis);  //加载流对应的文件
        String name = pros.getProperty("name");
        String password = pros.getProperty("password");

        System.out.println(name+password);
    }
}
```

# 五、Collections工具类

```java
package com.landfill.java;

import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

/*
Collections工具类
可以操作List Set Map的实现类

问题：Collection和Collections的区别
一个是接口，一个是工具类。

Colletions

Collections 中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作， 还提供了对集合对象设置不可变、对集合对象实现同步控制等方法

排序操作：（均为static方法）
reverse(List)：反转 List 中元素的顺序
shuffle(List)：对 List 集合元素进行随机排序
sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换

查找、替换
Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
Object min(Collection)
Object min(Collection，Comparator)
int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
void copy(List dest,List src)：将src中的内容复制到dest中
boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值

线程安全
synchronizedXxx()

 */
public class CollectionsTest {
    //排序操作
@Test
    public void test1(){
    ArrayList list = new ArrayList();
    list.add(123);
    list.add(47);
    list.add(-13);
    list.add(54);
    list.add(231);

    //void reverse(List)：反转 List 中元素的顺序
    Collections.reverse(list);
    System.out.println(list);

    //void shuffle(List)：对 List 集合元素进行随机排序
    Collections.shuffle(list);
    System.out.println(list);

    //void sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
    Collections.sort(list);   //按照Integer的compare()排序
    System.out.println(list);

    //void sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序

    //void swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换
    Collections.swap(list, 1, 3);

}

@Test
    public void test2(){
    ArrayList list = new ArrayList();
    list.add(123);
    list.add(47);
    list.add(123);
    list.add(54);
    list.add(231);
    list.add(231);
//    查找、替换

//    Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素，排序中最右边的就是最大的 要求是同一个类的元素
    Comparable max = Collections.max(list);
    System.out.println(max);
//    Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
//    Object min(Collection)
//    Object min(Collection，Comparator)

//    int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
    int frequency = Collections.frequency(list, 231);
    System.out.println(frequency);

    //复制
//    void copy(List dest,List src)：将src中的内容复制到dest中  src是source，dest是新的list
    //正确写法
    List dest = Arrays.asList(new Object[list.size()]);  //数组转成list
    Collections.copy(dest, list);
    System.out.println(dest);
//    Collections.copy(list1, list);  //要求dest.size()和source.size()要相同 这种写法会报异常

//    boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值

    List<Object> list1 = Collections.synchronizedList(list);  //返回的list就是线程安全的
}

@Test
    public void test3(){

}
}
```