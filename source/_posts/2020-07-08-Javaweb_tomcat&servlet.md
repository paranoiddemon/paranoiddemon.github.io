---
title: Tomcat、Servlet与HTTP协议
date: 2020-07-08 00:24:46
tags:  
- Java
-  Web
urlname:  tomcat-servlet-http-protocol
category:  笔记
description: Tomcat部署、Servlet原理，HTTP协议的请求响应
doc: true
---


本文内容：

1. web相关概念
2. web服务器软件：Tomcat
3. Servlet
4. HTTP协议
5. Request
6. Response



## web相关概念

1. 软件架构
   C/S 客户端
   B/S 浏览器
2. 资源分类
   静态资源：所有用户访问后得到的效果是一样的   如：html css js，可以直接被浏览器解析，客户端请求
   动态资源: 每个用户访问相同资源后，得到的结果可能不一样。 动态资源先转换为静态资源，再返回（服务端响应）给浏览器解析 如servlet/jsp php asp
3. 网络通信三要素
   IP：计算机等网络设备在网络中的唯一标识
   端口：应用程序的在计算机中的唯一标识 0-65536
   协议：规定了数据通信的规则  	如TCP/UDP
	


## web服务器软件
服务器：安装了服务器软件的计算机  
服务器软件：接收用户的请求，处理请求，做出响应     如：mysql服务器，web服务器
web服务器软件：
	可以部署web项目，让用户通过浏览器来访问这些项目
	web容器，动态资源需要通过容器来使用

常用的java相关的web服务器软件
	WebLogic oracle公司 大型的JavaEE服务器 支持所有的JavaEE规范  收费
	WebSphere：IBM 收费
	JBOSS:JBOSS公司 收费
	Tomcat：Apache基金组织，中小型的JavaEE服务器，仅支持少量的JavaEE规范servlet/jsp，开源
	
注：JavaEE： Java语言在企业级开发中使用的技术规范的总和，一共规定了13项大的规范

## Tomcat

```
1.下载 官方网站
2.安装 解压压缩包 安装的目录不要有中文和空格
3.卸载 删除目录就行了
4.启动	bin/startup.bat   localhost:8080 一般会把默认端口号改为80 http协议的默认端口号访问不用再输入端口号
5.关闭 
   强制关闭  关闭窗口
   正常关闭  调用shutdown.bat/ ctrl c 推荐使用

6.配置 

部署项目的方式：
1. 直接将项目放在webapps目录下 `http://localhost/hello/hello.html`
/hello 项目的访问路径，虚拟目录 一般会将项目打成war包放到webapps目录下，war包会自动解压缩

2. 在conf/server.xml配置
<Context docBase="D:\hello" path="/hi"/> docBase是资源所在路径，path是访问时的虚拟路径。server.xml是全局配置文件不建议直接配置

3.conf\Catalina\localhost创建任意名称的xml，xml的文件名称即为虚拟目录
<Context docBase="D:\hello"/>
```



​		

![Tomcat目录结构](https://i.loli.net/2020/07/07/jNUxvsRF36Z1p8V.png)

静态项目和动态项目
	目录结构：	
		java动态项目的目录的结构：
			|--根目录
				|--WEB-INFO （动态项目）
					|--web.xml 	   web项目的核心配置文件
					|--classes目录 放置字节码文件
					|--lib目录     	放置依赖的jar包

将Tomcat集成到IDEA，创建JavaEE项目 run -> edit configuration->tomcat
热部署 update resources


## Servlet
### 概念

概念：server applet  运行在服务器上

动态资源通过逻辑性的Java代码（java类）来执行，依赖于服务器tomcat执行它
需要遵守一定的接口，才能被tomcat所识别

Servlet就是一个接口，定义了java类被浏览器访问到（tomcat识别到）的规则
自定义Servlet接口的实现类，重写方法。
浏览器访问相应的路径就会运行mapping的类中重写的方法

### 步骤

快速入门：
	1. 创建JavaEE项目
	2. 定义Servlet的实现类
	3. 重写所有方法
	4. 配置servlet

```xml
在web.xml中配置  把实现了Servlet的类映射到一个虚拟路径，浏览器访问该路径就会去调用该类
<!--    配置servlet-->
    <servlet>
        <servlet-name>test1</servlet-name>  <!--名称自定义-->
        <servlet-class>cc.landfill.web.servlet.ServletTest</servlet-class> <!-- 全类名-->
    </servlet>

    <servlet-mapping>
        <servlet-name>test1</servlet-name>
        <url-pattern>/test2</url-pattern>   <!--虚拟路径-->
    </servlet-mapping>
```
在run configuration->deployment->application context，把虚拟目录修改为项目名称

### 执行原理：

1.当服务器接收到客户端浏览器的请求后，会解析url路径，获取Servlet的资源路径
2.查找web.xml文件，是否有对应的url—pattern标签体内容
3.如果有则找到对应的<servlet-class>全类名
4.tomcat通过反射把字节码加载进入内存，并且创建Servlet对象
5.调用service()方法

![servlet原理](https://i.loli.net/2020/07/07/P4LFV2aZ1toGn6f.png)

### Servlet的生命周期

1.被创建    init() 执行一次方法
	默认情况下，第一次被访问时，Servlet被创建

```xml 
在<servlet></servlet>  标签中配置
<!--        指定servlet的创建时机
            1.在第一次被访问时，创建   默认值为-1
            2.在服务器启动时，创建     0或正整数

-->
        <load-on-startup>5</load-on-startup>
Servlet 的init()执行一次，说明内存中只有一个Servlet对象，是单例的
多个用户同时访问该对象时，存在线程安全问题，如果加锁会严重影响性能
解决：尽可能不要定义成员变量，用局部变量。即使了定义了成员变量，也不要对其进行赋值，			
只去获取值。
    
```
2.提供服务	service()  执行多次
	每次访问都会被调用
3.被销毁	service() 执行一次
	服务器正常关闭的时候执行，Servlet对象销毁，destroy()在被销毁之前执行，用于释放资源

### 注解配置

Servlet3.0:
支持注解配置，不再需要web.xml

步骤：
1. 选择3.0以上版本，可以不创建web.xml文件
2. 定义Servlet接口的实现类
3. 重写方法
4. 在类上使用webservlet注解配置 @WebServlet("/test4") Servlet资源路径
	

IDEA与Tomcat部署
1. IDEA会为每一个tomcat部署的项目单路建立一份配置文件
2. 项目路径

```
工作空间项目: C:\Users\demon\IdeaProjects\day13_tomcat
tomcat部署的项目C:\Users\demon\IdeaProjects\out\artifacts\day13_tomcat_Web_exploded
tomcat真正访问的是tomcat部署的web项目，对应工作空间项目的web目录下的所有资源
```
3. 项目的web-info文件夹不能直接被浏览器访问，静态资源
4. 断点调试。debug 
5. 不同项目需要设置不同的虚拟目录 application context 再下一层目录才是实际的资源的位置

### 体系结构

Servlet 							    接口
	|--GenericServlet	  抽象类
		|--HttpServlet		抽象类：继承GenericServlet

**GenericServlet**
在GenericServlet类中把其他方法做了空实现，只剩下一个抽象方法void Service(),只要实现一个方法

**HttpServlet**
对http协议的封装，简化操作

1. 定义类继承**HttpServlet**
2. 重写doGet() 和doPost()   根据请求的方式

### 相关配置

urlpattern

1. 一个Servlet可以定义多个访问路径  `@WebServlet({"/demo4-1","/demo4-2","/demo4-3"})`
2. 路径定义规则：
   `/xxx`   
   `/xxx/xxx`   多层路径  目录结构 可以写成/xxx/*  可以使用通配符，通配符的优先级较低
    `*.do`        配合demo4.do来配置 do是自定义的，可以任意
   
    
### ServletContext

概念：代表整个web应用，可以和程序的容器（Server）通信

获取：

```java
//通过从Servlet继承的方法获取
ServletContext context1 = request.getServletContext();
//通过HttpServlet获取
ServletContext context2 = this.getServletContext();
//获取的对象是指向同一个引用的 ==
```

功能：

```java
1. 获取MIME类型
	MIME类型：在互联网通信过程中定义的一种文件数据类型
	格式：大类型/小类型 text/html image/jpeg
	响应的时候需要设置content-type
	获取：String getMimeType(String file)

2. 域对象 共享数据
	范围：所有用户所有请求的数据
	在一个 Servlet存数据
	  ServletContext context = this.getServletContext();
      context.setAttribute("msg","hello");
	在另一个 Servlet读数据
	  ServletContext context = this.getServletContext();
      Object msg = context.getAttribute("msg");
      System.out.println(msg);
	因为ServletContext的生命周期很长，会一直驻留在内存，存的数据太多会占用内存，谨慎使用
        
3. 获取文件的真实（服务器）路径
	将项目部署在远程的服务器上，需要其在服务器中的真实路径
        
//项目部署在服务器，访问的不是工作空间，而是项目路径
//默认是找在web目录下的？
String realPath = context.getRealPath("a.txt");
System.out.println(realPath);  //C:\Users\demon\IdeaProjects\out\artifacts\day15_response_war_exploded\a.txt

//web目录下
String realPath1 = context.getRealPath("/a.txt");
System.out.println(realPath1); //C:\Users\demon\IdeaProjects\out\artifacts\day15_response_war_exploded\a.txt

//web的WEB-INF目录下
String realPath2 = context.getRealPath("/WEB-INF/a.txt");
System.out.println(realPath2); //C:\Users\demon\IdeaProjects\out\artifacts\day15_response_war_exploded\WEB-INF\a.txt

//src目录下
String realPath3 = context.getRealPath("/WEB-INF/classes/a.txt");
System.out.println(realPath3); //C:\Users\demon\IdeaProjects\out\artifacts\day15_response_war_exploded\WEB-INF\classes\a.txt

```



## HTTP协议

### 概念

概念：Hyper Text transfer Protocol    定义了客户端和服务器端通信时，传输的数据的格式  

请求消息/相应消息
特点：

1. 基于TCP/IP的高级协议
2. 默认端口号为80
3. 基于请求响应模型：一次请求对应一次相应
4. 无状态的：每次请求之间相互独立，不能交互数据

版本：

ver1.0 每次请求响应都会建立新的连接

ver1.1 复用连接

### Request/Response工作原理

1. tomcatt服务器会根据url中的资源路径，创建ServletDemo1的对象（Sevelet实现类的对象）
2. tomcat创建request和response对象，request对象封装了请求消息数据
3. 把两个对象作为参数传给ServletDemo1实例的Service()
4. 通过request对象来获取请求消息数据，通过response对象来设置响应消息数据
5. 服务器在响应浏览器之前，会从封装了响应消息的Response对象中获取响应消息数据，再返回给浏览器

### Request

#### 请求消息数据格式
```
1. 请求行
请求方式 请求url 协议/版本
GET/login.html HTTP/1.1

请求方式：有7种，常用的有两种
	GET: 
	请求参数在请求行中，直接跟在url后面
	请求的url长度有限制
	不安全
		
	POST：
	请求参数在请求体中
	没有url长度限制
	更安全

2. 请求头
请求头名称：请求头值   以键值对的方式出现

常见请求头：
Host
User-Agent：浏览器告诉服务器，访问使用的浏览器及其版本，可以解决浏览器的兼容问题（因为浏览器的解析引擎不同），服务器根据
Accept：接收响应的形式
Referer：告诉服务器请求从哪里来
	作用：
		防盗链
		统计：判断流量的来源
Connection； keep-alive


eg：
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7,zh-HK;q=0.6
Cache-Control: max-age=0
Connection: keep-alive
Content-Length: 11
Content-Type: application/x-www-form-urlencoded
Cookie: Idea-41d450f1=229dd7a7-ea44-4bf7-8c79-ca88718e85a4; JSESSIONID=7E11EE42B5820055DDF4EA1FA317A334
Host: localhost
Origin: http://localhost
Referer: http://localhost/login.html
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36


3. 请求空行
空行  用于分隔 POST的请求头和请求体
4. 请求体
正文  封装POST请求信息的请求参数，GET方式就没有请求体
字符串格式  username=jack
```

5. 

#### Request的继承关系

ServletRequest 接口
		|--HttpServletRequest 接口 继承
					|--org.apache.catalina.connector.RequestFacade  Tomcat写的实现类

#### 功能

1. ##### 获取请求消息数据：

```
   获取请求行  GET  /day14/demo1?name=jack HTTP/1.1
   	方法：
   	String getMethod()   GET 
       String getContextPath()  /day14  (*)重点掌握
   	String getServletPath() /demo1
   	String getQueryString  name=jack
   	String getRequestURI()  /day14/demo1   (*)
   	StringBuffer getRequestURL()  http://localhost/day14/demo1
   	String getProtocol HTTP/1.1
   	String getRemoteAddr()  获取客户机的ip地址
   	
   	URL:统一资源定位符  是URL的子集
   	URI:统一资源标识符  
   	
   获取请求头
   	String getHeader(String name) 通过请求头的名称获取请求头的值  键值对(*)
   	Enumeration<String> getHeaderNames() 获取所有请求头名称 
   	
   获取请求体
   	只有POST方式才有，封装了请求参数
   	步骤：
   	1.获取流对象
   		BufferedRead getReader() 字符流
   		ServletInputStream get InputStream() 字节流  在文件上传时在讲
   	2.再从流对象中获取数据
   	
```

2. ##### 其他功能

```java
   获取请求参数通用方式  兼容GET和POST
   	String getParameter(String name) 根据参数名称返回参数值  如getParameter("name") 
   	String[] getParameterValues(String name)  返回参数名的多个value，返回数组
   	Enumeration<String> getParameterNames() 获取所有参数名称
   	Map<String,String[]> getParameterMap() 获取所有参数的Map集合
   	
   	TOMCAT8 在GET模式下已经将中文乱码问题解决
   	POST模式 在获取参数前，设置请求request的变暗 request.setCharacterEncoding("utf-8")
   
   请求转发:一种在服务器内部进行资源跳转的方式 
   	步骤：
   	1.通过request对象获取请求转发器对象RequestDispatcher getRequestDispatcher(String path)
   	2.使用RequestDispatcherd对象进行转发 forward（ServletRequest request,ServletResponseresponse) 
       特点：
       1.浏览器地址栏不会发生变化
       2.服务器内部的资源的转发，转发的路径不能是外部的资源
       3.转发是一次请求
   
   共享数据
       域对象：一个有作用范围的对象，在范围内共享数据
       request域：代表一次请求的范围，用于请求转发的多个资源中共享数据
       方法：
       setAttribute(String name,Object obj) 在第一个servlet里设置值
       Object getAttribute(name)  在得到转发的servlet中去接收值
       removeAttribute(name)
       
       
   获取SevrletContext
       ServletContext getServletContext()
       
```

   

#### 案例：用户登录

用户登录案例需求：
	1.编写login.html登录页面
		username & password 两个输入框
	2.使用Druid数据库连接池技术,操作mysql，day14数据库中user表
	3.使用JdbcTemplate技术封装JDBC
	4.登录成功跳转到SuccessServlet展示：登录成功！用户名,欢迎您
	5.登录失败跳转到FailServlet展示：登录失败，用户名或密码错误
步骤：

1. 创建项目，导入html页面，数据库配置文件，jar包

2. 获取数据库连接

3. 创建Javabean，封装user信息

4. 创建UserDAO 操作user表的类，写增删改查方法

5. 写login suc fail Servlet，进行请求转发

6. BeanUtils工具类
   JavaBean标准
   类用public修饰
   提供空参的构造器
   成员变量必须使用private修饰
   提供public的getter和setter

   setProperty() getProperty()  populate(Object obj,Map map)封装







### Response

#### 响应消息数据格式

```
响应行
	组成：协议/版本  响应状态码 状态码描述  HTTP/1.1 200 OK
	
	响应状态码：服务器告诉浏览器本次请求和响应的状态，都是3位数，分为5类
		分类：
		1xx  服务器接收客户端消息，但没有接收完成，等待一段时间后，发送1xx，询问是否还有请求
		2xx  成功。200
		3xx  重定向。302（重定向），资源跳转的方式； 304（访问缓存）
		4xx  客户端错误  404请求路径没有对应的资源 405（请求没有对应的doXxx方法，和请求方式不一致）
		5xx	 服务器错误 500（服务器内部错误）
响应头
	格式 头名称：值
	常见响应头
		Content-Type:type/html;charset=UTF-8
		Content-disposition 服务器告诉客户端以什么格式打开响应体数据
			默认值：in-line 在当前页面打开
			atttachment；filename=xxx 以附件形式打开响应体。用于文件下载
响应空行
响应体
	真实的传输的数据 html页面 ，图片等资源
```




#### 功能：设置响应消息

1. 设置响应行

```
HTTP/1.1 200 OK
设置状态码：setStatus(int sc)
```

2. 设置响应头

```
setHeader(String name,String value)
```

3. 设置响应体

```
步骤
(1)获取输出流
	字符流	PrintWriter getWriter()
	字节流 ServletOutputStream getOutputStream()

(2)使用输出流将数据输出到客户端浏览器
```

   

#### 案例

1. ##### 重定向

```
(1)实现：
/* //方式一：访问/reresponseDemo1 会自动跳转到responseDemo2
//1.设置响应行的状态码为302
response.setStatus(302);
//2.设置响应头location
response.setHeader("location","/day15/responseDemo2");  //虚拟路径+资源路径*/

//方式二：
response.sendRedirect("/day15/responseDemo2"); //可以是任意的url

(2)特点：转发vs重定向
转发：forward
1.转发地址栏路径不变
2.只能访问当前服务器路径下的资源
3.转发是一次请求，可以使用request对象共享数据

重定向：redirect
1.地址栏发生变化
2.可以访问其他服务器的资源
3.重定向是两次请求，不再能使用request对象共享数据，两次req/resp是不同的

(3)路径写法
1.相对路径：以.开头 ./index.html
	确定当前资源和目标资源之间的位置关系
	例1 ./ 当前目录
	当前：http://localhost/day15/location.html
	目标：http://localhost/day15/responseDemo1
	路径为 ./responseDemo1  可以省略为responseDemo1
	例2 ../ 后退一级目录
	当前：http://localhost/day15/htmls/location.html
	目标：http://localhost/day15/responseDemo1
	路径为 ../responseDemo1
	
2.绝对路径： 以/开头
	通过绝对路径可以确定唯一资源 如http://localhost/day15/responseDemo1
			可以省略协议，ip，端口/day15/responseDemo1
			
3.使用规则：根据使用的对象决定是否加虚拟目录
		给客户端浏览器使用：需要加虚拟目录，如重定向，<a> <form>
		response.sendRedirect("/day15/responseDemo2");
		给服务器使用：不需要加虚拟目录，如转发时,就不要写虚拟目录
		request.getRequestDispatcher("/xxxServlet").forward(request,response);

4.动态获取虚拟目录
String request.getContextPath()
response.sendRedirect(contextPath+"资源名称"),更改虚拟目录不需要大量调整代码
客户端的虚拟目录也可以动态获取：jsp

```

   

2. ##### 服务器输出字符数据到浏览器

```
/*设置编码格式 防止乱码问题
方式一：
//在获取流之前，设置流的默认编码（ISO-8859-1）为需要的编码格式
response.setCharacterEncoding("utf-8");
//告诉浏览器，服务器发送的消息编码格式，建议浏览器使用该编码解码
response.setHeader("content-type","text/html;charset=utf-8" );  //text是html的根本的格式*/
//方式二
response.setContentType("text/html;charset=utf-8");


//1.获取字符输出流
PrintWriter pw = response.getWriter();  //不需要关流，response一次响应结束后自动回销毁，自己做了关闭流的操作
//2.输出数据
// pw.write("hello response");
// pw.write("<h1>hello response</h1>");
pw.write("你好啊 响应");
```

   

3. ##### 服务器输出字节数据到浏览器

```
response.setContentType("text/html;charset=utf-8");
//1.获取字节输出流
ServletOutputStream sos = response.getOutputStream();

//2.输出数据
sos.write("hello 你好".getBytes("utf-8"));  //Chrome默认的字符集是随系统的GBK
```

   

4. ##### 生成验证码

```
本质是图片 防止恶意的表单注册

Servlet代码
		int width = 100;
        int height = 50;

        //1.创建一个对象，验证码图片的对象
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        //2.生成验证码图片
        //2.1 填充背景色
        Graphics graphics = image.getGraphics(); //画笔对象
        graphics.setColor(Color.pink);  //设置颜色
        graphics.fillRect(0,0,width,height);

        //2.2 画边框
        graphics.setColor(Color.BLUE);
        graphics.drawRect(0,0,width-1,height-1);   //0,0是左上角的坐标,边框有一个px
        String str ="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
        Random random = new Random();

        //2.3 写验证码
        for (int i = 1; i <5 ; i++) {
            int index = random.nextInt(str.length());  //随机角标
            char ch = str.charAt(index);
            graphics.drawString(ch+"",width/5*i,height/2);
        }
        //2.4 画干扰线
        graphics.setColor(Color.green);
        for (int i = 0; i <5 ; i++) {
            int x1 = random.nextInt(width);
            int x2 = random.nextInt(width);
            int y1 = random.nextInt(height);
            int y2 = random.nextInt(width);
            graphics.drawLine(x1,y1,x2,y2);
        }
        
        //3.输出到页面
        ImageIO.write(image,"jpg",response.getOutputStream());
        //从内存中输出的图片，那这里还有response吗？
    }
 
 
HTML页面 
<body>

    <img id="checkCode" src="/day15/checkCodeServlet"/>
    <a id="change" href="">看不清，换一张</a>

</body>

<script>
       /* 1.给超链接和图片绑定单击事件
        2.重新设置图片的src属性*/
        window.onload = function(){
           var img = document.getElementById("checkCode");
           img.onclick = function(){
               //加时间戳,解决缓存问题，每次都传一个不重复的参数
               var date = new Date().getTime();
               img.src="/day15/checkCodeServlet?"+date;
            }

        }
 </script>
```

  

#### 案例：文件下载

需求：

1. 页面显示超链接
2. 点击超链接弹出下载提示框
3. 完成图片文件下载

超链接指向的资源，如果可以被浏览器解析，则直接展示，不能解析则下载。使用响应头的content-dispostion:attachment;filename=xxx

步骤：

1. 定义页面，超链接指向一个servlet，传递资源的名称filename

2. 定义servlet，获取filename，使用字节输入流加载文件进内存

3. 设置响应头 content-dispostion:attachment;filename=xxx

4. 将数据写出到response输出流

```
HTML页面
<body>
    <a href="/day15/downloadServlet?filename=img1.jpg">image1</a>  加虚拟路径图片格式
</body>

@WebServlet("/downloadServlet")
public class DownloadServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String filename = request.getParameter("filename");
        ServletContext servletContext = this.getServletContext();
        //获取内容类型，设置返回内容类型
        String mimeType = servletContext.getMimeType(filename);
        response.setHeader("content-type",mimeType);
        //获取真实路径
        String realPath = servletContext.getRealPath("img"+filename);  //这里不要加/web /指的就是web，WEB-INF特殊
        FileInputStream fis = new FileInputStream(new File(realPath)); //不要加引号
        response.setHeader("content-disposition", "attachment;filename="+filename);
        ServletOutputStream os = response.getOutputStream();
        byte[] buffer = new byte[1024*8];
        int len;
        while ((len=fis.read(buffer))!=-1){
           os.write(buffer,0,len);
        }

        fis.close();

    }
```

下载文件名中文乱码

获取客户端的浏览器版本信息

根据不同版本信息，去设置编码方式

```
//解决中文文件名问题：在设置响应头之前设置编码方式
        //获取请求头的ua
        String agent = request.getHeader("user-agent");
        //使用工具类方法编码文件名
        filename = DownLoadUtils.getFileName(agent, filename);
         response.setHeader("content-disposition", "attachment;filename="+filename);
```

可以实现局域网内文件传输

```
1.把需要传输的文件放入img文件夹
2.在download.html 修改filename
3.启动服务器
4.从手机端，或者另一台电脑访问192.168.2.116/day15/download.html
```























