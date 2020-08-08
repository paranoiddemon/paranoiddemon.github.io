---
title: Java-IO流
date: 2020-06-29 20:24:46
tags: Java
urlname: java-io-stream
category:  笔记
description: Java基础：IO流
toc: true
---

![IO流体系](https://i.loli.net/2020/06/30/PDpnxHKwYrtWINu.png)

# 一、File类

```java

/*
一、File类的使用

1.File类的对象代表一个文件或者一个文件目录
2.声明在java.io包下
3.涉及文件或者文件目录的创建、删除、重命名、修改时间、文件大小等方法
并未涉及到写入或者读取文件目录，要实现该功能就需要IO流来完成
4.file类的对象常作为参数传到流的构造器中，作为流的写入或者读写的终点

二、如何创建File实例
三个构造器都有可能使用：

相对路径：相较于某个路径下，指明的路径
绝对路径：包含盘符在内的文件或文件目录的路径

注意
在windows和dos 路径分隔符使用“\”表示 转义要写两个
unix和url使用“/”表示

三、常用方法
 public String getAbsolutePath()：获取绝对路径
 public String getPath() ：获取路径
 public String getName() ：获取名称
 public String getParent()：获取上层文件目录路径。若无，返回null
 public long length() ：获取文件长度（即：字节数）。不能获取目录的长度。
 public long lastModified() ：获取最后一次的修改时间，毫秒值

 public String[] list() ：获取指定目录下的所有文件或者文件目录的名称数组
 public File[] listFiles() ：获取指定目录下的所有文件或者文件目录的File数组

 public boolean renameTo(File dest):把文件重命名为指定的文件路径

  public boolean isDirectory()：判断是否是文件目录
  public boolean isFile() ：判断是否是文件
  public boolean exists() ：判断是否存在
  public boolean canRead() ：判断是否可读
  public boolean canWrite() ：判断是否可写
  public boolean isHidden() ：判断是否隐藏

  public boolean createNewFile() ：创建文件。若文件存在，则不创建，返回false
  public boolean mkdir() ：创建文件目录。如果此文件目录存在，就不创建了。 如果此文件目录的上层目录不存在，也不创建。
  public boolean mkdirs() ：创建文件目录。如果上层文件目录不存在，一并创建
注意事项：如果你创建文件或者文件目录没有写盘符路径，那么，默认在项目 路径下。

File类的删除功能
 public boolean delete()：删除文件或者文件夹 删除注意事项：
Java中的删除不走回收站。 要删除一个文件目录，请注意该文件目录内不能包含文件或者文件目录



 */
public class FileTest {
    @Test
    public void test1(){
        //构造器1：直接写路径
        File file1 = new File("hello.txt");  //相对于当前Module所在的文件路径
        File file2  = new File("D:\\IDEA_workspace\\JavaSenior\\day09\\hello.txt"); //两个\是为了转译
        System.out.println(file1);
        System.out.println(file2);  //现在还只是内存层面的一个对象，未涉及到删改，所以没有文件也不会报错

        //构造器2: 上一级路径 + 当前的文件目录或文件
        File file3 = new File("D:\\IDEA_workspace\\JavaSenior\\day09","hello");
        System.out.println(file3);

        //构造器3：对象名+文件名 File对象可以是文件目录
        File file4 = new File(file3, "hello.txt");

    }
    @Test
    public void test2(){
        File file1 = new File("hello.txt");
        File file2 = new File ("d:\\io\\hi.txt");

// public String getAbsolutePath()：获取绝对路径
// public String getPath() ：获取路径
// public String getName() ：获取名称
// public String getParent()：获取上层文件目录路径。若无，返回null
// public long length() ：获取文件长度（即：字节数）。不能获取目录的长度。
// public long lastModified() ：获取最后一次的修改时间，毫秒值
        System.out.println(file1.getPath());  //hello.txt
        System.out.println(file1.getAbsolutePath());  //D:\IDEA_workspace\JavaSenior\day09\hello.txt
        System.out.println(file1.getName());   //hello.txt
        System.out.println(file1.getParent());  //null  此时文件还不存在
        System.out.println(file1.length());    //0  --> 11
        System.out.println(file1.lastModified());   //0  -->  1593391633017  毫秒数
        Date date = new Date(file1.lastModified());
        System.out.println(date);    //Mon Jun 29 08:47:13 CST 2020

        System.out.println("--------------");

        System.out.println(file2.getPath());  //d:\io\hi.txt
        System.out.println(file2.getAbsolutePath());  //d:\io\hi.txt
        System.out.println(file2.getName());   //hi.txt
        System.out.println(file2.getParent());  //d:\io
        System.out.println(file2.length());    //0
        System.out.println(file2.lastModified());   //0

//适用于文件目录
// public String[] list() ：获取指定目录下的所有文件或者文件目录的名称数组
// public File[] listFiles() ：获取指定目录下的所有文件或者文件目录的File数组
        File file3 = new File("D:\\IDEA_workspace\\JavaSenior"); //打印该目录所有文件和目录，要求目录必须存在
        String[] list = file3.list();
        for(String t: list){
            System.out.println(t);     //只列出名字
        }
        File[] files = file3.listFiles();
        for(File f:files){
            System.out.println(f);   //D:\IDEA_workspace\JavaSenior\day01,以绝对路径的方式输出的
        }

    }
    @Test
    public void test3(){
// public boolean renameTo(File dest):把文件重命名为指定的文件路径
// file1 renameTo(file2)，要返回true，需要file1在硬盘中是存在的，file2不存在，不能进行覆盖操作，而且一旦true，file1在硬盘中不再存在；
        File file1 = new File("hello.txt");    //首先要file要存在
        File file2 = new File("d:\\io\\hi.txt");
        boolean renameTo = file2.renameTo(file1);
        System.out.println(renameTo);
    }

    @Test
    public void test4(){
        File file1 = new File("hello.txt");
        File file2 = new File("d:\\io");
// public boolean isDirectory()：判断是不是文件目录
// public boolean isFile() ：判断是不是文件
// public boolean exists() ：判断是否存在
// public boolean canRead() ：判断是否可读
// public boolean canWrite() ：判断是否可写
// public boolean isHidden() ：判断是否隐藏

        System.out.println(file1.isDirectory());
        System.out.println(file1.isFile());
        System.out.println(file1.exists());
        System.out.println(file1.canRead());
        System.out.println(file1.canWrite());
        System.out.println(file1.isHidden());
        System.out.println();
        System.out.println(file2.isDirectory());
        System.out.println(file2.isFile());
        System.out.println(file2.exists());//一般先判断是否存在
        System.out.println(file2.canRead());
        System.out.println(file2.canWrite());
        System.out.println(file2.isHidden());
    }
    @Test
    public void test5() throws IOException {

        File file1 = new File("hi.txt");
//public boolean createNewFile() ：创建文件。若文件存在，则不创建，返回false
//public boolean mkdir() ：创建文件目录。如果此文件目录存在，就不创建了。 如果此文件目录的上层目录不存在，也不创建。
//public boolean mkdirs() ：创建文件目录。如果上层文件目录不存在，一并创建

// public boolean delete()：删除文件或者文件夹 删除注意事项：
// Java中的删除不走回收站。 要删除一个文件目录，请注意该文件目录内不能包含文件或者文件目录

        //文件的创建
        if (!file1.exists()){
            file1.createNewFile();
            System.out.println("创建成功");

        }else{
            file1.delete();
            System.out.println("删除成功");
        }


        }



    @Test
    public void test6(){
        //文件目录的创建
        File file = new File("d:\\io\\io1\\io3");  //目录存在，上层目录不存在。就都不创建
        boolean mkdir = file.mkdir();
        if (mkdir == true){
            System.out.println("创建成功");
        }else {
            System.out.println("创建失败");
        }

        File file2 = new File("d:\\io\\io1\\io4");  //如果上层目录不存在，就可以一并创建
            boolean mkdirs = file2.mkdirs();
        if (mkdirs == true){
            System.out.println("创建成功1");
        }else {
            System.out.println("创建失败1");
        }


    }
}
```

# 二、节点流

```java
package com.landfill.java;

import org.junit.Test;

import java.io.*;

/*
IO流
1.概念
数据的输入输出的方式以流stream的方式进行
Java中定义了各种stream类，可以获取不同的种类并通过标准的方式输入输出

2.分类：
按照数据单位不同
字节流 8bit 图片 视频
字符流 16bit 文本  char 2byte

流向不同
输入流
输出流

按角色不同
节点流：直接作用于文件上的
处理流：在已有的流的基础上在加一层  比如：加快流的传输速度

3.体系结构
抽象基类
字节流：InputStream OutputStream
字符流：Reader Writer
涉及40多个流都是基于以上四个抽象基类
                    字节流                 字节流           字符流           字符流
抽象基类：       InputStream          OutputStream          Reader           Writer
节点流（文件流）: FileInputStream      FileOutputStream      FileReader      FileWriter
缓冲流（处理流）：BufferedInputStream  BufferedOutputStream  BufferedReader  BufferedWriter


 */
public class IOTest {
    /*
    将文件读到程序，并输出到控制台
    1.reader()方法
    2.异常的处理，为了保证流资源一定可以关闭，要使用try-catch-finally处理
    3读入的文件一定要存在，否则会报FileNotFoundException
     */
    @Test
    public void test1()  {   //在单元测试的相对路径是相较于当前module的，main方法默认是在Project文件夹下
        //1.先实例化File对象 指明要操作的文件
        FileReader fr = null;
        try {
            File file = new File("hello.txt");
            //2.提供具体的流
            fr = new FileReader(file);
            //3.数据的读入
            //返回读入的一个字符，如果到达文件末尾，返回-1
//        int data = fr.read();    //以int存的char相当于是ASCII码
//        while (data!=-1){
//            System.out.print((char)data);   //读入的是int，然后在转为char
//            data = fr.read();                  //每次只读一个，不是-1就输出
//        }
            //语法上的修改
            int data;
            while((data = fr.read())!=-1){
                System.out.print((char)data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(fr!=null)   //可能在实例化之前就出现异常，因此就要判断是不是为null，再执行finally内部的语句
                fr.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        //4.流的关闭操作,涉及物理上的连接，JVM不能自动垃圾回收，需要手动关闭， 不然可能会导致内存泄漏

    }

    @Test
    //对read()操作升级，使用read的重载方法
    public void test2() {
        //1.File类实例化
        FileReader fr = null;
        try {
            File file = new File("hello.txt");

            //2.FileReader流的实例化
            fr = new FileReader(file);

            //3.读入的操作
            char[] cbuffer= new char[5];    //其实是用同一个数组反复去装剩下的元素
            int len;     //读进去的个数
            while ((len=fr.read(cbuffer))!=-1) {    //int read(char[])返回值是每次读入的字符的个数,到文件末尾返回-1
                //方法一：遍历char[]
//                for (int i = 0; i <len ; i++) {
//                    System.out.print(cbuffer[i]);
//                }
                //错误写法：还是会把char[]里的没有被覆盖部分输出
//                String str  = new String(cbuffer);
//                System.out.print(str);
                //正确写法
                String str = new String(cbuffer,0,len);   //string里的方法，把char[]的一部分转为String
                System.out.print(str); //不要用换行打印

            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
        //4.资源的关闭
            if(fr!=null)
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }

    }

    @Test
    /*写出文件
    1.输出操作:如果File不存在，就会自动创建此文件
    2.File对应的文件已经存在了
        构造器：FileWriter(file,false) /FileWriter(file) ,覆盖源文件
               FileWriter(file,true)  不会覆盖源文件，而是在源文件append
    */
    public void test3() throws IOException {
        //1.提供File类的对象，指明写出的文件
        File file = new File("hello1.txt");
        //2.提供FileWriter的对象，用于数据写出
        FileWriter fw = new FileWriter(file);

        //3.写出的操作
        fw.write("i have a dream\n");  //覆盖文件
        fw.write("i have an apple");
        //4.关闭流
        fw.close();
    }

    @Test
    //使用字符流复制文本
    public void test4()  {

        FileReader fr = null;
        FileWriter fw = null;
        try {
            //1.创建File对象，指明读入和写出的文件
            //不能使用字符流来处理图像、视频等字节流的文件
            File file1 = new File("hello.txt");
            File file2 = new File("hello2.txt");
            //2.创建流的对象
            fr = new FileReader(file1);
            fw = new FileWriter(file2);
            //3.数据的读写操作
            char[] cbuf  = new char[5];
            int len;
            while ((len = fr.read(cbuf))!=-1){

                    fw.write(cbuf,0,len);  //每次写出len个字符

            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭流
            try {
                if (fr != null)
                fr.close();

            } catch (IOException e) {
                e.printStackTrace();
            }
            if(fw != null)
                try {
                    fw.close();      //如果写在里面，就可能出现fr.close异常的时候，fw.close执行不了，因此要拿出来并列写
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }


    }

    @Test
    //使用字节流复制图片
    /*使用字节流，且以数组来读取，也可以复制文档中的英文和数字，但是汉字就不行了，因为中文用三个字符去表示
    因为ASCII码和UTF-8底层的英文字母和数字还是用一个byte存储的，但是如果一个个byte去读中文也是可以的

    对于文本文件：.txt .java .c .cpp  使用字符流处理
    对于非文本文件： .jpg .mp4 .doc .ppt  是有字节流处理

     */
    public void test5()  {

        FileInputStream fr = null;
        FileOutputStream fw = null;
        try {
            //1.创建File对象，指明读入和写出的文件
            //不能使用字符流来处理图像、视频等字节流的文件
            File file1 = new File("70pomn.jpg");
            File file2 = new File("70pomn1.jpg");
            //2.创建流的对象
            fr = new FileInputStream(file1);
            fw = new FileOutputStream(file2);
            //3.数据的读写操作
            byte[] bbuf  = new byte[5];
            int len;
            while ((len = fr.read(bbuf))!=-1){

                fw.write(bbuf,0,len);  //每次写出len个字符

            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭流
            try {
                if (fr != null)
                    fr.close();

            } catch (IOException e) {
                e.printStackTrace();
            }
            if(fw != null)
                try {
                    fw.close();      //如果写在里面，就可能出现fr.close异常的时候，fw.close执行不了，因此要拿出来并列写
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }


    }

    @Test
    //一个个byte读.复制用字节流也可以，但是在内存中读会乱码
    public void test6()  {

        FileInputStream fr = null;
        FileOutputStream fw = null;
        try {
            //1.创建File对象，指明读入和写出的文件
            //不能使用字符流来处理图像、视频等字节流的文件
            File file1 = new File("hello.txt");
            File file2 = new File("hello2.txt");
            //2.创建流的对象
            fr = new FileInputStream(file1);
            fw = new FileOutputStream(file2);
            //3.数据的读写操作
//            byte[] bbuf  = new byte[5];
//            int len;
//            while ((len = fr.read(bbuf))!=-1){
//
//                fw.write(bbuf,0,len);  //每次写出len个字符
//
//            }
            int data;
            while ((data = fr.read())!=-1){
                fw.write(data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭流
            try {
                if (fr != null)
                    fr.close();

            } catch (IOException e) {
                e.printStackTrace();
            }
            if(fw != null)
                try {
                    fw.close();      //如果写在里面，就可能出现fr.close异常的时候，fw.close执行不了，因此要拿出来并列写
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }

        System.out.println("duplicate success");
    }
    //指定路径下文件的复制
    public void copyFile(String srcPath,String destPath){


        FileInputStream fr = null;
        FileOutputStream fw = null;
        try {
            //1.创建File对象，指明读入和写出的文件
            //不能使用字符流来处理图像、视频等字节流的文件
            File srcfile = new File(srcPath);
            File destfile = new File(destPath);
            //2.创建流的对象
            fr = new FileInputStream(srcfile);
            fw = new FileOutputStream(destfile);
            //3.数据的读写操作
            byte[] bbuf  = new byte[1024];
            int len;
            while ((len = fr.read(bbuf))!=-1){

                fw.write(bbuf,0,len);  //每次写出len个字符

            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭流
            try {
                if (fr != null)
                    fr.close();

            } catch (IOException e) {
                e.printStackTrace();
            }
            if(fw != null)
                try {
                    fw.close();      //如果写在里面，就可能出现fr.close异常的时候，fw.close执行不了，因此要拿出来并列写
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }
    @Test
    //复制视频所需要的时间 70MB
    public void test(){
        long s = System.currentTimeMillis();
        String srcPath = "C:\\Users\\demon\\Desktop\\01.avi";
        String destPath = "C:\\Users\\demon\\Desktop\\02.avi";
        copyFile(srcPath,destPath);
        long e = System.currentTimeMillis();
        System.out.println("duplicate cost mills:"+(e-s)); //769ms
    }
}
```

# 三、缓冲流

```java
package com.landfill.java;

import org.junit.Test;

import java.io.*;

/*
1.缓冲流的使用
                    字节流                 字节流           字符流             字符流
抽象基类：        InputStream          OutputStream          Reader             Writer
节点流（文件流）: FileInputStream      FileOutputStream      FileReader        FileWriter
缓冲流（处理流）：BufferedInputStream  BufferedOutputStream  BufferedReader     BufferedWriter
对应的方法参数：  read(byte[] buffer)  writer(byte[] buffer) read(char[] cbuf)  writer(char[] cbuf)



2.缓冲流的作用：提高读写的速度，开发中缓冲流使用得比较多

为什么可以提高读写的速度：缓冲区
提供了8192byte（8kB)的缓存空间，读满之后一次性的写出  BufferedRead 是8192个char，16kB
通过flush()去刷新缓冲区

3.处理流：套接在已有流的基础上
 */
public class BufferedTest {
    /*
    实现非文本文件的复制
     */
    @Test
    public void test1(){
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        try {
            //1.实例化文件
            File srcFile = new File("70pomn.jpg");
            File destFile = new File("70pomn1.jpg");

            //2.1 节点流
            FileInputStream fileInputStream = new FileInputStream(srcFile);
            FileOutputStream fileOutputStream = new FileOutputStream(destFile);
            //2.2 缓冲流 处理流
            bis = new BufferedInputStream(fileInputStream);
            bos = new BufferedOutputStream(fileOutputStream);

            //3.复制：内容的读写
            byte[] buffer = new byte[10];
            int len;
            while ((len = bis.read(buffer))!=-1){  //read里的数组不要溜掉了
                bos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {

            //4.流资源关闭
            //要求：先关闭外层的流，再关闭内层的流

            //说明：在关闭外层流的时候会自动关闭内层流，因此可以省略
//        fileInputStream.close();
//        fileOutputStream.close();
            try {
                if(bis!=null)
                    bis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if(bos!=null)
                    bos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }



    }
    //实现文件复制的方法
    public void copyFileWithBuffered(String src,String dest){
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        try {
            //1.实例化文件
            File srcFile = new File(src);
            File destFile = new File(dest);

            //2.1 节点流
            FileInputStream fileInputStream = new FileInputStream(srcFile);
            FileOutputStream fileOutputStream = new FileOutputStream(destFile);
            //2.2 缓冲流 处理流
            bis = new BufferedInputStream(fileInputStream);
            bos = new BufferedOutputStream(fileOutputStream);

            //3.复制：内容的读写
            byte[] buffer = new byte[1024];
            int len;
            while ((len = bis.read(buffer))!=-1){  //read里的数组不要溜掉了
                bos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {

            //4.流资源关闭
            //要求：先关闭外层的流，再关闭内层的流

            //说明：在关闭外层流的时候会自动关闭内层流，因此可以省略
//        fileInputStream.close();
//        fileOutputStream.close();
            try {
                if(bis!=null)
                    bis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if(bos!=null)
                    bos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    @Test
    public void test2(){
        long s = System.currentTimeMillis();
        String src = "C:\\Users\\demon\\Desktop\\01.avi";
        String desc = "C:\\Users\\demon\\Desktop\\02.avi";
        copyFileWithBuffered(src,desc);
        long e = System.currentTimeMillis();
        System.out.println(e-s);  //192
    }

    @Test
    public void test3(){
        BufferedReader br = null;
        BufferedWriter bw = null;
        try {
            br = new BufferedReader(new FileReader(new File("hello.txt")));
            bw = new BufferedWriter(new FileWriter(new File("hello3.txt")));//默认是false，就会覆盖源文件

            //方式一
//            char[] cbuf = new char[1024];
//            int len;
//            while((len = br.read(cbuf))!=-1){   //不要漏了判断不等于-1
//                bw.write(cbuf,0,len);
//            }
            //方式二：使用String和readline
            String data;
            while ((data=br.readLine())!= null) {
                bw.write(data+"\n");  //不包含换行符
                //bw.newLine();  提供换行操作
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (br != null)
                br.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (bw != null)
            bw.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

# 四、转换流

```java
package com.landfill.java;
/*
处理流之二:转换流的使用
1.属于字符流
InputStreamReader：将一个字节的输入流 转换为字符的输入流    解码
OutputStreamWriter： 将一个字符的输出流 转换为字节的输出流  编码


2.作用：提供字节流和字符流之间的转换

3.字符集
常见的编码表
 ASCII：美国标准信息交换码。  用一个字节的7位可以表示。被其他字符集兼容
 ISO8859-1：拉丁码表。欧洲码表  用一个字节的8位表示。
 GB2312：中国的中文编码表。最多两个字节编码所有字符
 GBK：中国的中文编码表升级，融合了更多的中文文字符号。最多两个字节编码
    如果是两个byte表示的字符，最高位是1，表示的是一个byte的字符就是0
 Unicode：国际标准码，融合了目前人类使用的所有字符。为每个字符分配唯一的 字符码。所有的文字都用两个字节来表示。
    首位要用来表示是1byte还是2byte，所以只有2的15次方的空间 32768 不够世界上的所有字符使用，所以没有推广
 UTF-8：变长的编码方式，可用1-4个字节来表示一个字符。
    随着互联网的出现，没8bit传输数据就是utf-8 16bit就是utf-16
        1byte 0xxxxxxx
        2byte 110xxxxx 10xxxxxx
        3byte 1110xxxx 10xxxxxx 10xxxxxx
        4byte 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
    1-4byte的分别对应原unicode中的一部分符号范围
    中文在utf-8中是三个byte
ANSI编码，通常指的是平台的默认编码，英文操作系统是ISO-8859-1,中文系统是GBK

 */

import org.junit.Test;

import java.io.*;

public class ConvertStreamTest {
    @Test
    //实现字节的输入到字符的转换
    public void test1() throws IOException {
        FileInputStream fis = new FileInputStream("hello1.txt");
       // InputStreamReader isr = new InputStreamReader(fis); //使用系统默认的字符集UTF-8
        //直接在参数中指定字符集，根据hello.txt文件当初保存时使用的字符集
        InputStreamReader isr = new InputStreamReader(fis,"GBK");  //如果文件用的是utf-8编码，就会输出乱码
        char[] cbuf = new char[20];
        int len;
        while((len = isr.read(cbuf))!= -1){
            for (int i = 0; i <len ; i++) {
                System.out.print(cbuf[i]);
            }
        }

    }
    @Test
    //InputStreamReader和OutputStreamWriter的综合使用
    public void test2() throws IOException {
        File file1 = new File("hello1.txt");
        File file2 = new File("hello3.txt");

        FileInputStream fis = new FileInputStream(file1);
        FileOutputStream fos = new FileOutputStream(file2);

        InputStreamReader isr = new InputStreamReader(fis,"utf-8");   //默认是utf-8
        OutputStreamWriter osw = new OutputStreamWriter(fos,"gbk");

        char[] cbuf = new char[20];
        int len;
        while((len = isr.read(cbuf))!=-1){
            osw.write(cbuf,0,len);
        }
        if(osw!= null)
            osw.close();
        if(isr!= null)
            isr.close();


    }
}
```

# 五、对象流

```java
package com.landfill.java2;

import org.junit.Test;

import java.io.*;

/*
处理流之六：对象流
ObjectInputStream
ObjectOutputStream

序列化：把内存中的Java对象保存到磁盘中或通过网络传输出去 通过ObjectOutputStream实现
反序列化

二、对象的序列化机制
1.把Java对象转换成平台无关的二进制流，从而允许把这种二进制流永久地保存在磁盘上，或者通过网络将
这种二进制流传输到另一个网络节点。
当其他程序获取了这种二进制流，可以从中恢复为对象。

Java对象的可序列化

Person需要满足可序列化的:
1.实现Serializable:没有需要实现的抽象方法，称为标识接口

2.提供一个全局常量 serialVersionUID
2.1 serialVersionUID用来表明类的不同版本间的兼容性。
简言之，其目的是以序列化对象 进行版本控制，有关各版本反序列化时是否兼容。
2.2 如果类没有显示定义这个静态常量，它的值是Java运行时环境根据类的内部细节自 动生成的。
 若类的实例变量（非静态变量）做了修改，serialVersionUID 可能发生变化。故建议， 显式声明。
2.3 Java的序列化机制是通过在运行时判断类的serialVersionUID来验 证版本一致性的。在进行反序列化时，
JVM会把传来的字节流中的 serialVersionUID与本地相应实体类的serialVersionUID进行比较，
如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常。(InvalidCastException)

3.除了当前person类需要实现Serializable,所有属性的也必须是可序列化的。基本数据类型默认是可序列化的
String也实现了Serializable，自定义类要实现Serializable,并且提供一个serialVersionUID

4.不能序列化static和transient修饰的成员变量,属性在序列化的时候就没有值，都是默认初始化值
Person{name='null', age=0, id=0}

把内存的对象转换成一种特殊格式的字符串 json   字符串自身是可以序列化的

 */
public class ObjectStream {
    @Test
    //序列化
    public void test1(){
        ObjectOutputStream oos = null;
        try {
         oos = new ObjectOutputStream(new FileOutputStream("object.dat"));


            oos.writeObject(new String("what is the problem with you?"));
            oos.flush();  //刷新操作
            oos.writeObject(new Person("tom",20));
            oos.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }

        finally {
            try {
                if (oos!= null){
                    oos.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    @Test
    //反序列化，把磁盘文件或者网络传输的对象还原为内存中的一个对象
    public void test2(){
        ObjectInputStream ois = null;
        try {
            ois = new ObjectInputStream(new FileInputStream(new File("object.dat")));
            Object o = ois.readObject();
            String str = (String)o;
            System.out.println(str);
            Object person = ois.readObject();
            System.out.println(person);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            try {
                if(ois!=null)
                ois.close();

            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}


package com.landfill.java2;

import java.io.Serializable;
/*
Person需要满足可序列化的:
1.实现Serializable:没有需要实现的抽象方法，称为标识接口

2.提供一个全局常量 serialVersionUID
2.1 serialVersionUID用来表明类的不同版本间的兼容性。
简言之，其目的是以序列化对象 进行版本控制，有关各版本反序列化时是否兼容。
2.2 如果类没有显示定义这个静态常量，它的值是Java运行时环境根据类的内部细节自 动生成的。
 若类的实例变量（非静态变量）做了修改，serialVersionUID 可能发生变化。故建议， 显式声明。
2.3 Java的序列化机制是通过在运行时判断类的serialVersionUID来验 证版本一致性的。在进行反序列化时，
JVM会把传来的字节流中的 serialVersionUID与本地相应实体类的serialVersionUID进行比较，
如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常。(InvalidCastException)

3.除了当前person类需要实现Serializable,所有属性的也必须是可序列化的。基本数据类型默认是可序列化的
String也实现了Serializable，自定义类要实现Serializable,并且提供一个serialVersionUID

4.不能序列化static和transient修饰的成员变量,属性在序列化的时候就没有值，都是默认初始化值
Person{name='null', age=0, id=0}
 */

public class Person implements Serializable {
    public static final long serialVersionUID = 1231231313L; //用来识别所在类的

    static private String name;
    transient private int age;
    private int id;


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

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", id=" + id +'}';
    }
}
```

# 六、其他流

```java
package com.landfill.java;
/*
其他流的使用
1.标准的输入输出流
system.in
system.out

2.打印流(都是输出流）
PrintStream    system.out返回的就是PrintStream的实例 println()就是该类的方法
可以通过setOut把数据从控制台打印到指定的文件中
System.setOut(PrintStream)  PrintSteam(FileOutputStream) 从而打印到文件中
PrintWriter

提供了一系列重载的print()和println()

3.数据流
操作基本数据类型和String的
DataInputStream
DataOutputStream

将数据持久化，用于读取和写入基本数据类型和字符串
有一系列方法。每次写入就要flush() 刷新把数据存在文件中
可以永久化和读取，读取数据得按写入的顺序读


 */

import org.junit.Test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class SystemInOut {
    /*
    标准的输入输出流
    System.in   默认从键盘输入
    System.out  默认从控制台输出

    System类的 setIn(InputStream in) setOut(PrintStream out)

    练习：把读取到的整行字符串转换成大写输出，e exit才退出
    方法一：使用Scanner实现
    方法二：使用System.in实现  -->转换流--> bufferReader的readline()
     */
    public static void main(String[] args) {
        BufferedReader br = null;
        try {
            InputStreamReader isr = new InputStreamReader(System.in); //除了可以读取file 也可以直接读取键盘输入
            br = new BufferedReader(isr);
            while(true){
                String data = br.readLine();
                if("e".equalsIgnoreCase(data)||"exit".equalsIgnoreCase(data)){ //data写在参数里可以更好地避免空指针
                    break;
                }
                String upper = data.toUpperCase();
                System.out.println(upper);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(br!=null)
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }

    }
    @Test
    public void test1(){



    }




}
```

# 七、RandomAccessFile

```java
package com.landfill.java2;

import org.junit.Test;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.RandomAccessFile;
/*
   RandomAccessFile的使用
   在java.io包下，但直接继承于Object，并且实现了DataInput DataOutput这两个接口，意味着这个类既可以读也可以写
   既是输入流又是输出流
   构造器 RandomAccessFile(File mode）RandomAccessFile(String mode）
   mode
   r:只读
   rw：读写
   rwd 同步文件内容
   rws 同步文件内容和元数据更新

   //作为输出流时，写出到的文件如果不存在，就自动创建；如果文件已经存在，就会默认从头对原有文件内容进行覆盖

   可以一定操作实现插入数据的操作

   seek() 断点续传 开多个线程，每个线程执行，在下载的时候创建一个临时文件记录不同的线程的指针的位置，就可以实现断点续传
    */
public class RandomTest {//千万不要把类名写成和已经有的类一样的

    @Test
    public void test1() throws FileNotFoundException {
        RandomAccessFile raf1 = null;
        RandomAccessFile raf2 = null;
        try {
            raf1 = new RandomAccessFile(new File("70pomn4.jpg"),"rw");
            raf2 = new RandomAccessFile(new File("70pomn.jpg"),"r");
            byte[] buffer = new byte[1024];
            int len;
            while ((len = raf2.read(buffer))!=-1){
                raf1.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(raf1!=null)
            raf1.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if(raf2!=null)
            raf2.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
    @Test
    public void test2() throws IOException {
        //作为输出流时，写出到的文件如果不存在，就自动创建；如果文件已经存在，就会默认从头对原有文件内容进行覆盖
        RandomAccessFile raf = new RandomAccessFile("helloworld.txt", "rw");
        raf.seek(3);//把指针调到角标为3的位置，即从第四位开始覆盖，也可以实现append
        raf.write("xyz".getBytes());
       // raf.writeChars("xyz");
        raf.close();


    }

    //使用RandomAccessFile实现插入的效果
    @Test
    public void test3() throws IOException {
        RandomAccessFile raf = new RandomAccessFile(new File("helloworld.txt"), "rw");
        raf.seek(3);
        byte[] buffer = new byte[20];
        int len;
        StringBuilder sb = new StringBuilder((int)new File("helloworld.txt").length());
        while((len = raf.read(buffer))!=-1){

            sb.append(new String(buffer,0,len)); //把byte数组放进一个StringBuilder
            }

        raf.seek(3);
        raf.write("xyz".getBytes());

        //把StringBuild再写入
        raf.write(sb.toString().getBytes());
        raf.close();

    }

    //思考：将StringBuilder替换为ByteArrayInputStream

}
```

# 练习

```java
package com.landfill.exer;

import org.junit.Test;

import java.io.*;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;


/**
 * 练习3:获取文本上字符出现的次数,把数据写入文件
 *
 * 思路：
 * 1.遍历文本每一个字符
 * 2.字符出现的次数存在Map中
 *
 * Map<Character,Integer> map = new HashMap<Character,Integer>();
 * map.put('a',18);
 * map.put('你',2);
 *
 * 3.把map中的数据写入文件
 *
 * @author shkstart
 * @create 2019 下午 3:47
 */
public class WordCount {
    /*
    说明：如果使用单元测试，文件相对路径为当前module
          如果使用main()测试，文件相对路径为当前工程
     */
    @Test
    public void testWordCount() {
        FileReader fr = null;
        BufferedWriter bw = null;
        try {
            //1.创建Map集合
            Map<Character, Integer> map = new HashMap<Character, Integer>();

            //2.遍历每一个字符,每一个字符出现的次数放到map中
            fr = new FileReader("hello.txt");
            int c = 0;
            while ((c = fr.read()) != -1) {
                //int 还原 char
                char ch = (char) c;
                // 判断char是否在map中第一次出现
                if (map.get(ch) == null) {
                    map.put(ch, 1);
                } else {
                    map.put(ch, map.get(ch) + 1);
                }
            }

            //3.把map中数据存在文件count.txt
            //3.1 创建Writer
            bw = new BufferedWriter(new FileWriter("wordcount.txt"));

            //3.2 遍历map,再写入数据
            Set<Map.Entry<Character, Integer>> entrySet = map.entrySet();
            for (Map.Entry<Character, Integer> entry : entrySet) {
                switch (entry.getKey()) {
                    case ' ':
                        bw.write("空格=" + entry.getValue());
                        break;
                    case '\t'://\t表示tab 键字符
                        bw.write("tab键=" + entry.getValue());
                        break;
                    case '\r'://
                        bw.write("回车=" + entry.getValue());
                        break;
                    case '\n'://
                        bw.write("换行=" + entry.getValue());
                        break;
                    default:
                        bw.write(entry.getKey() + "=" + entry.getValue());
                        break;
                }
                bw.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关流
            if (fr != null) {
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            if (bw != null) {
                try {
                    bw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }

    }
}
```



```java
package com.landfill.exer;

import org.junit.Test;

import java.io.File;
import java.io.IOException;
//遍历目录中的文件 ，计算空间、删除指定文件夹
public class FileExer {
    @Test
    public void test() throws IOException {
        //创建同目录的文件
        File file1 = new File("d:\\io\\hello.txt");
        String parent = file1.getParent();
        File file2 = new File(file1.getParent(),"haha.txt");
        file2.createNewFile();
    }

    @Test
    public void test2(){
        File file1 = new File("D:\\io");
        String[] list = file1.list();
        for(String fileName: list){
            if (fileName.length()>4) {
                if (fileName.substring(fileName.length() - 4, fileName.length()).equals(".jpg")) {
                    System.out.println(fileName);
                    //可以调用String的endsWith()
                }
            }
        }


    }
    @Test
    public void test3(){
        File file = new File("d:\\io");
        FileExer fileExer = new FileExer();
        long space = fileExer.getDir(file);
        System.out.println("space usage:"+space+"byte");
        File file1 = new File("d:\\io\\io1","io4");
//        file1.listFiles();
        fileExer.deleteDir(file1);


    }
    public long getDir(File file){
        long space = 0;
        File[] files = file.listFiles();
        for(File f:files){
            if(f.isDirectory()){
                getDir(f);
            }else{
                long length = f.length();
                space += length;
                System.out.println(f);

            }
        }
        return space;

    }
    public void  deleteDir(File file){
        if(file.isDirectory()){  //先删子目录
            File[] files = file.listFiles();
            for (File f:files){
                deleteDir(f);
            }
        }
        file.delete();  //然后删自己 ，即可能是文件也可能是文件夹
    }


}
```

```java
package com.landfill.exer;

import org.junit.Test;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class PicTest {

    //图片的加密

    @Test
    public void test(){
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream("70pomn.jpg");
            fos = new FileOutputStream("70pomneEncrypt.jpg");

            byte[] buffer = new byte[1024];
            int len;
            while((len = fis.read(buffer))!=-1){
                //对字节进行修改
                for (int i = 0; i <len ; i++) {
                    buffer[i] =(byte) (buffer[i] ^ 5);
                }
                fos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
        }
        try {
            if(fos!= null)
            fos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            if(fis!=null)
            fis.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //图片的加密
    @Test
    public void test2(){
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream("70pomneEncrypt.jpg");
            fos = new FileOutputStream("70pomneDecrypt.jpg");

            byte[] buffer = new byte[1024];
            int len;
            while((len = fis.read(buffer))!=-1){
                //对字节进行修改
                for (int i = 0; i <len ; i++) {
                    buffer[i] =(byte) (buffer[i] ^ 5);
                }
                fos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
        }
        try {
            if(fos!= null)
                fos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            if(fis!=null)
                fis.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}
```