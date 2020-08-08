---
title: SpringMVC框架初步与SSM整合
date: 2020-07-21 00:30:46
tags:
- SpringMVC
- Java
urlname: springmvc-notes
category: 笔记
description: SpringMVC框架处理请求响应、文件上传、异常处理、SSM整合
---



## 一、概述
### 特点
展示层框架。集成在Spring。
通过注解，让POJO成为处理请求的控制器，无须实现任何接口
支持REST风格的URL请求 Restful
采用了松散耦合可插拔组件结构，让其他MVC框架更具扩展性和灵活性 
偏前端，不是业务逻辑层



### 功能
集成了IoC AOP，不需要像SSH那样把对象交给Spring管理，支持Restful风格
支持灵活的URL到页面控制器的映射
非常容易与其他视图技术集成：如Velocity FreeMarker(静态页面化) 
​		页面静态化：jsp需要编译，需要服务器环境，加载速度慢，所以都是要静态化的
模型数据不存在特定的API，而是放在Model（Map数据结构），很容易被其他框架使用



### 组件
DispatchServlet 前端控制器 本质是Servlet
Controller  处理器/页面控制器，用于对请求进行处理
HandlerMapping 请求映射处理器，找谁来处理。返回一个HandlerExecutionChain对象，包含一个Handler处理器（页面控制器）对象、多个HandlerInterceptor 拦截器对象
ViewResolver 视图解析器
LocalResolver  本地化、国际化
MultipartResolver 文件上传解析器
HandlerExceptionResolver 异常处理器



## 二、Hello SpringMVC

### 1、实现步骤
1、从archetype webapp创建maven项目，设置archetypeCatalog=internal，导入pom依赖

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <spring.version>5.0.2.RELEASE</spring.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
        <scope>provided</scope>
      </dependency>
      <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.0</version>
        <scope>provided</scope>
      </dependency>

  </dependencies>
```

2、web.xml
```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>


  <!--前端控制器-->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>  <!--前端控制器随着服务启动而加载，加载而时候就在指定位置加载配置文件，就能扫描到控制器及其映射-->
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>

  </servlet>

  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>   <!--把所有请求都发送给DispatcherServlet-->
    <!--最开始就是要这么配置servlet的，之后就用注解配置了 @WebServlet("/addUserServlet")-->
  </servlet-mapping>

</web-app>
```

3、配置springmvc.xml
加载配置文件后，开启组件扫描，就可以找到控制层
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd 
                    http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--扫描组件，将加了@Controller注解的类作为SpringMVC的控制层-->
    <context:component-scan base-package="cc.landfill"/>
    
    <!--配置视图解析器-->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    
    <!--开启SpringMVC框架注解的支持-->
    <mvc:annotation-driven/>    自动加载RequestMappingHandlerMapping 和 RequestMappingHandlerAdapter

</beans>
```

4、创建POJO，加上@Controller SpringMVC就会将其作为控制层加载，让其处理请求响应

5、在控制层中，在方法上设置@RequestMapping(value = "hell"),Spring就是通过此注解将访问路径与控层中的方法进行匹配
index.jsp 的<a href="hell"> 超链接 ->根据注解，访问控制器对应名称的方法 ->  控制器将这一请求映射到实际的资源路径
```java
@Controller
public class TestController {

    //如果访问 localhost:80/springmvc01/hell


    @RequestMapping("hell")   //value = "hell" 只有一个value的时候可以省略 会跳转到http://localhost/springmvc01/hell
    //实际访问的资源时  localhost/WEB-INF/view/success.jsp
    public String hello(){  //和方法名无关，只和注解的value有关
        System.out.println("success");  
        return "success";  //视图名称  返回的是资源的路径prefix+视图名称+suffix，把一个url请求的路径 转发到项目的页面
    }
    //可以在Controller里配置多个映射的页面

}
```
6、处理请求的方法会返回一个字符串 即视图名称。根据springMVC-servlet.xml配置的视图解析器实现页面的跳转
prefix+视图名称+suffix，为最终跳转的页面路径

之前是在每一个Servlet上配置路径的。Servlet再处理每一个请求的doPost doGet

工作流程：
服务器启动-> 加载web.xml-> 创建了DispatcherServlet对象-> 加载了springmvc.xml -扫描开启-> controller对象创建->
ViewResolver对象创建->index.jsp <a href="hello"> hello是相对路径 ->经过DispatcherServlet -> 
通过注解找到@RequestMapping("/hello)的方法 -> 方法执行完 return 一个String，转发请求到指定页面 -> 
由核心控制器把返回的String发送到ViewResolver,访问资源路径prefix+String+suffix的页面,返回给控制器，控制器响应请求
![SpringMVC执行流程](https://i.loli.net/2020/07/19/xR7BowMqkLa1SVI.jpg)

### 2、@RequestMapping
#### （1）该注解可以加在类上

就可以形成多级路径，每一个Controller可以是一个功能模块

如：访问/user/hello 来访问Hello()方法
```java

@Controller
@RequestMapping(path = "/user")
public class HelloController {

    @RequestMapping(path = "/hello")
    public String Hello(){
        System.out.println("hello springmvc");
        return "hello";
    }
}
```
```jsp
<a href="user/testrm">RequestMapping</a>  <%--user前加/访问的是根目录，localhost/user/testrm
                                          不加就是访问jsp当前所在路径 localhost/springmvc01/user/testrm --%>
```

#### （2）常用属性
```java
value 和 path 都是指映射的路径，可以相互替换
如果只有value值只有一个，可以把value省略
可以写作：@RequestMapping("/user") 
    
method 请求方式是GET 还是 POST 值是枚举类 几种请求方式
@RequestMapping("/user",method={RequestMethod.POST})  超链接的GET请求就不会处理

params 指定请求参数的key和value要满足params的限制
@RequestMapping("/user",method={RequestMethod.POST},params = {"username"}) 
必须要传入一个key为username的请求参数，否则就会返回400 bad request 
@RequestMapping(params = {"username=tom"}) 
则必须携带username=tom的请求参数

headers: 发送的请求中必须包含的请求头
@RequestMapping(headers = {"Accept"}) 
```


## 三、请求参数的绑定
### 1、获取请求参数

和之前的 request.getParameter()一样;

（1）普通类型的请求参数

```java
<a href="param/testParam">RequestMapping</a>  <%--user前加/访问的是根目录，localhost/param/testParam
                                            不加就是访问jsp当前所在路径 localhost/springmvc01/param/testParam --%>
@Controller
@RequestMapping("/param")
public class ParamController { 
//传入一般类型的请求参数
    @RequestMapping("/testParam")
    public String testParam(String username){

        System.out.println("请求参数绑定");
        System.out.println(username);
        return "hello";
    }
}
```
（2）对象类型的请求参数

```java
@Controller
@RequestMapping("/param")
public class ParamController { 
//传入一般类型的请求参数
    @RequestMapping("/testParam")
    public String testParam(User user){  //提交的参数name和Bean的属性名一致，会自动注入，封装成bean
        System.out.println("请求参数绑定");
        System.out.println(username);
        return "hello";
    }
}
```



（3）对象类型的属性中有对象

```java
 //实体类
public class Account implements Serializable {
    private String username;
    private String password;
    private int balance;
    private User user;
}
//映射的方法
@RequestMapping("/saveAccount")
public String saveAccount(Account account){

    System.out.println("请求参数绑定");
    System.out.println(account);
    return "hello";
}

//jsp
<form action="param/saveAccount" method="post">
    姓名：<input type="text" name="username"/><br/>  <%--name和bean的属性名要相同--%>
    密码：<input type="text" name="password"/><br/>
    金额：<input type="text" name="balance"/><br/>

    <%--封装对象--%>
    用户名：<input type="text" name="user.name"/><br/>
    用户年龄：<input type="text" name="user.age"/><br/>
    <input type="submit" value="提交"/>

</form>
```
### 2、请求参数中文乱码问题，在web.xml中添加过滤器

```xml
<!--配置过滤器，解决中文乱码，所有经过过滤器的请求都会设置为utf-8-->
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

```

### 3、自定义类型转换器

```java
类型转换器的自定义的实现类，SpringMVC中已经有大量的实现可以直接调用
public class StringToDateConverter implements Converter<String,Date> {
    @Override
    public Date convert(String s) {
        if(s == null){
            throw new RuntimeException("请输入数据");
        }
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        try {
            return sdf.parse(s);
        } catch (Exception e) {
            throw new RuntimeException("类型转换错误");
        }
    }
}
```

```xml
在springmvc.xml中注入自定义的类型转换器，并且开启。
<!--配置自定义类型转换器-->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <!--把自定义的转换器注入到conversionService,查看该类的属性-->
            <set>
                <bean class="cc.landfill.util.StringToDateConverter"/>
            </set>
        </property>
    </bean>

    <!--开启SpringMVC框架注解的支持（开启HandlerMapping和HandlerAdapter),开启类型转换器-->
    <mvc:annotation-driven conversion-service="conversionService"/>
```

### 4、获取原生的Servlet API

```java
直接作为参数穿进来就可以了
//获取原生Servlet API
     @RequestMapping("/testServlet")
        public String testServlet(HttpServletRequest request, HttpServletResponse response){
         System.out.println(request);
         System.out.println(response);

         HttpSession session = request.getSession();  //仍然可以和原来一样调用Request的方法
         ServletContext servletContext = session.getServletContext();
         System.out.println(servletContext);
         System.out.println(session);
         request.getParameter("name");
         System.out.println("name");

         return "hello";
     }
```

## 四、常用注解

### 1、@RequestParam

```html
<body>
<%--常用的注解--%>
<a href="anno/testReqParam?name=tom">RequestParam</a>
</body>
```

```java
@Controller
@RequestMapping("/anno")
public class AnnotationController {

    @RequestMapping("/testReqParam")
    public String testReqParam (@RequestParam(value = "name") String username){ 
        //注解参数列表,value是页面提交的请求参数的key名称
        System.out.println("testReqParam");
        System.out.println(username);

        return "hello";
    }
}
```

属性：

name/value都是指定请求参数的key名称

required 默认是true。指定该属性是必须传入的，否则返回400 BadRequest

### 2、@RequestBody

用于获取请求体的key=value&key=value...结构的数据（异步Ajax json 会使用到。只有POST才有请求体

```java
@Controller
@RequestMapping("/anno")
public class AnnotationController {

    @RequestMapping("/testReqParam")
    public String testReqParam (@RequestParam(value = "name") String username){
        System.out.println("testReqParam");
        System.out.println(username);

        return "hello";
    }

    @RequestMapping("/testReqBody")
    public String testReqBody (@RequestBody String body){
        System.out.println(body);  //username=%E5%BC%A0%E4%B8%89&age=20 中文被编码了
        return "hello";
    }
}
```

### 3、@PathVariable

获取URL路径中的占位符

（1）[理解RESTful架构](https://www.ruanyifeng.com/blog/2011/09/restful.html)

RESTful风格  Representational State Transfer

它结构清晰、符合标准、易于理解、扩展方便

```java
<a href="anno/testPathVariable/10">testPathVariable</a>

    
    @RequestMapping("/testPathVariable/{sid}")
    public String testPathVariable (@PathVariable(name="sid") String id){
        System.out.println(id);
        return "hello";
    }
```

（2）HiddenHttpMethodFilter

在Spring中配置过滤器，可以将浏览器请求改为指定的请求方式，支持GET、 POST、PUT、Delete请求

暂作了解，配置过滤器，在页面中增加隐藏域

WebService /WebClient API也可以实现

### 4、@RequestHeader

获取指定key的请求头的值

```java
    @RequestMapping(value = "/testReqHeader")
    public String testReqHeader (@RequestHeader(value="Accept") String header){
        System.out.println(header);
        return "hello";
    }
```

### 5、@CookieValue

```java
    @RequestMapping(value = "/testCookieValue")
    public String testCookieValue (@CookieValue(value="JSESSIONID") String cookieValue){
        System.out.println(cookieValue);
        return "hello";
    }
```

### 6、@ModelAttribute

在方法上，表示当前方法会在控制器的方法执行之间执行

```java
 @RequestMapping(value = "/testModelAttribute")

    public String testModelAttribute (User user){
        System.out.println("hello");
        System.out.println(user);
        return "hello";
    }
    
    @ModelAttribute
    public User showUser(String username){
        System.out.println("showUser()...");  
        //优先执行，但表单提交的不是完整的实体类数据，未提交的数据，使用数据库原有的值
        //使用username去查询数据库
        User user = new User();
        user.setAge(20);
        user.setName("jack");     //提交了就封装到bean
        user.setDate(new Date()); //表单没有提交就从数据库查
        return user;    //在传给方法之前，得到一个封装完整的对象，防止没有提交的字段变成null
    }
```



在参数上，获取指定的数据给参数赋值,方法没有返回值，放在一个Map里，从map里取出对象

```java
 @RequestMapping(value = "/testModelAttribute")

    public String testModelAttribute (@ModelAttribute("user")User user){
        System.out.println("hello");
        System.out.println(user);
        return "hello";
    }
    
    @ModelAttribute
    public void showUser(String username,Map<String,User> map){
        System.out.println("showUser()...");  
        //优先执行，但表单提交的不是完整的实体类数据，未提交的数据，使用数据库原有的值
        //使用username去查询数据库
        User user = new User();
        user.setAge(20);
        user.setName("jack");     //提交了就封装到bean
        user.setDate(new Date()); //表单没有提交就从数据库查
        map.put("user",user)
    }
```

### 7、@SessionAttributes

用于多次执行控制器方法间的参数共享

在类上注解

```java
@Controller
@RequestMapping("/anno")
@SessionAttributes(value={"msg"}) //把request域中的msg存到了session域
public class AnnotationController {
	@RequestMapping(value = "/testSessionAttributes")
    public String testSessionAttributes (Model model){
        model.addAttribute("msg","msg here"); //model本质是一个Map,把值存到了Request域
        System.out.println("testSessionAttributes");
        return "hello";
    }
    @RequestMapping(value = "/getSessionAttributes")
    public String getSessionAttributes (ModelMap modelMap){
        String msg = (String)modelMap.get("msg");//ModelMap实现类中get方法获取session域中的值,要先存才能取到
        System.out.println(msg);
        System.out.println("getSessionAttributes");
        return "hello";
    }
    @RequestMapping(value = "/delSessionAttributes")
    public String delSessionAttributes (SessionStatus sessionStatus){
        System.out.println("delSessionAttributes");
        sessionStatus.setComplete(); //该方法表示清除session
        return "hello";
    }
}

jsp
    <a href="anno/testSessionAttributes">SessionAttributes</a>
	<a href="anno/getSessionAttributes">getAttributes</a>
	<a href="anno/delSessionAttributes">delAttributes</a>
    
展示jsp ${requestScope.msg}  可以使用el表达式取域中的值
		${sessionScope.msg}
```



## 五、响应数据和结果视图

### 1、请求方法的返回值

String/void/ModelAndView

```java
@Controller
@RequestMapping("/user")
public class UserController {
    //（1）返回值为String
    @RequestMapping("/testString")
    public String testString(Model model){
        System.out.println("/testString");
        //模拟从从数据库查询User，返回给页面
        User user = new User("tom", "123", 23);
        model.addAttribute("user",user);
        return "hello";
    }
    
    
    
    //返回值为空 默认请求路径为/user/testVoid.jsp
//    @RequestMapping("/testVoid")
//    public void testVoid(Model model){
//        System.out.println("testVoid");
//    }    
    //（2）返回值为空 默认请求路径为/user/testVoid.jsp
    @RequestMapping("/testVoid")
    public void testVoid(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("testVoid");
        //请求转发，地址栏不会改变
        //request.getRequestDispatcher("/WEB-INF/pages/hello.jsp").forward(request,response);
        //手动转发不会执行viewResolver

        //重定向，地址栏会改变。需要加虚拟路径
        //response.sendRedirect(request.getContextPath()+"/index.jsp"); //不能直接访问WEB-INF目录

        //直接响应给浏览器
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=utf-8");
        response.getWriter().print("hello 世界");  //直接响应，输出在浏览器页面上
        return;
    } 
    
    
   //（3）返回值为ModelAndView对象   
    @RequestMapping("/testReturnModelAndView")
    public ModelAndView testReturnModelAndView() {
        System.out.println("testVoid");
        ModelAndView mv = new ModelAndView();
        mv.addObject("username", "jack");  			//被存在request域里
        mv.setViewName("hello");			//设置视图名称，功能和return String 相同
        return mv;
    }           
}
```

### 2、转发和重定向关键字

```java
//使用关键字进行转发和重定向
    @RequestMapping("/testForwardAndRedirect")
    public String testForwardAndRedirect() {
        System.out.println("testForwardAndRedirect");

        //转发和重定向关键字，手动增加prefix和suffix
        //return "forward:/WEB-INF/pages/hello.jsp";  //自己写请求转发
        return "redirect:/index.jsp";  //底层加了虚拟路径，不用手动加 不加 / 就跳转到当前目录 localhost/springmvc02/user/index.jsp
    }
```

### 3、@ResponseBody响应Json数据

在springmvc.xml里配置不拦截静态资源。否则访问不到js等静态资源

```x
 <!--配置前端控制器不拦截的静态资源，不然页面引入的js，css,图片 都不能访问-->
    <mvc:resources location="/js/" mapping="/js/**"/>
    <mvc:resources location="/images/" mapping="/images/**"/> <!--文件夹下所有的文件都不拦截-->
    <mvc:resources location="/css/" mapping="/css/**"/>
```

页面ajax

```html
<script>
        $(function () {
            $("#btn").click(function () {
                $.ajax({
                    //json
                    url:"user/testAjax",
                    contentType:"application/json;charset=UTF-8",
                    data:'{"username":"jack","password":"123","age":"23"}',
                    dataType:"json",
                    type:"post",
                    success:function(data){
                        //这个data是指服务器端响应的json数据，解析该数据
                        alert("hello");
                        alert(data.username)
                    }

                });
            });
        });

    </script>

<body>
	<button id="btn">sendAjexRequest</button>
</body>
```

```java
 //异步响应
    @RequestMapping("/testAjax")
    public @ResponseBody User testAjax(@RequestBody User user) {  
        //使用注解把User对象自动转换为Json，不再需要使用手动去转换
        System.out.println("testAjax");
        System.out.println(user);//请求体是json
        //把json封装到bean中，SpringMVC已经集成了，需要导入jackson解析器的依赖
        //模拟查询数据库
        user.setUsername("jerry");
        return user;
        }
```



## 六、文件上传

文件上传

前提：

- form表单的enctype(表单请求正文的类型)设置为multipart/form-data 。
   默认值是application/x-www-form-urlencoder 是键值对

- 提交方法必须是POST

- 提供文件选择域`<input type="file"/>`

原理：会把表单的请求分成几部分，有部分请求中确定MIME类型

### 1、传统方式上传文件

使用第三方组件实现文件上传

导入pom依赖 commons-fileupload 、commons-io

```xml
  <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.1</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.4</version>
    </dependency>
```

jsp

```html
<body>
<form action="user/fileupload" method="post" enctype="multipart/form-data">
    选择文件<input type="file" name="upload"/><br>
    <input type="submit" value="upload">
</form>
</body>
```

controller

```java
@Controller
@RequestMapping("/user")
public class FileUploadController {
    @RequestMapping("/fileupload")
    public String UserController(HttpServletRequest request) throws Exception {
        System.out.println("uploading file");

        //使用fileupload组件，使用request上传
        String path = request.getSession().getServletContext().getRealPath("/uploads/");//上传到服务器的真实路径
        File file = new File(path);
        if(!file.exists()){
            //不存在就创建
            file.mkdirs();
        }
        //解析request对象，获取上传文件项 commons-fileupload的类
        DiskFileItemFactory factory = new DiskFileItemFactory();
        ServletFileUpload upload = new ServletFileUpload(factory);
        //解析request
        List<FileItem> items = upload.parseRequest(request);
        //遍历
        for(FileItem item :items){
            //判断item对象是不是上传文件项
            if(item.isFormField()){
            //普通表单项

            }else {
                //写出到路径
                String filename = item.getName();
                String uuid = UUID.randomUUID().toString().replace("-", "");
                filename = uuid + filename;//生成唯一文件名
                item.write(new File(path,filename));
                //文件路径在D:\IDEA_workspace\SpringMVC_itcast\springmvc-03\target\springmvc-03\uploads
                //删除临时文件
                item.delete();
            }
        }
        return "hello";
    }
}
```

### 2、SpringMVC实现文件上传

（1）在springmvc.xml配置文件解析器

```xml
 <!--配置文件解析器-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!--id必须是multipartResolver，不能变-->
        <property name="maxUploadSize" value="10485760"/> <!--以byte为单位 10Mb-->
    </bean>
```

（2）表单页

```jsp
<form action="user/fileupload2" method="post" enctype="multipart/form-data">
    选择文件<input type="file" name="upload"/><br> <%--name和user/fileupload2映射的方法的MultipartFile参数名一致--%>
    <input type="submit" value="upload">
</form>
```

（3）Controller

```java
@Controller
@RequestMapping("/user")
public class FileUploadController {
    //SpringMVC方式上传
    @RequestMapping("/fileupload2")
    public String ileUpload2(HttpServletRequest request, MultipartFile upload) throws IOException { //必须和表单文件上传域的name属性一致
        System.out.println("springmvc upload");
        //获取文件上传的真实路径，如果不存在路径就mkdirs
        String path = request.getSession().getServletContext().getRealPath("/uploads/");
        File file = new File(path);
        if(!file.exists()){
            file.mkdirs();
        }
        //获取文件唯一名称
        String filename = upload.getOriginalFilename();
        String uuid = UUID.randomUUID().toString().replace("-", "");
        filename = uuid + filename;//生成唯一文件名
        //上传文件
        upload.transferTo(new File(path,filename));


        return "hello";
    }
```

### 3、跨服务器的文件上传

分服务器的目的：

应用服务器 webapp
数据库服务器：运行数据库
缓存和消息服务器：处理高并发访问的缓存和消息
文件服务器：存储用户上传的文件

性能更高。解耦的一种

再配置一个tomcat，改一下端口，新建一个uploads文件夹。

403错误 在tomcat/conf/web.xml 配置 readonly 为false

```xml
<servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>

        <init-param>
        <param-name>readonly</param-name>
        <param-value>false</param-value>
        </init-param>
    
        <init-param>
            <param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
 </servlet>
```



jsp

```jsp
<hr>
<h1>跨服务器上传</h1>
<form action="user/fileupload3" method="post" enctype="multipart/form-data">
    选择文件<input type="file" name="upload"/><br> 
    <input type="submit" value="upload">
</form>

```

controller

```java
@Controller
@RequestMapping("/user")
public class FileUploadController {
    //跨服务器的文件上传
    @RequestMapping("/fileupload3")
    public String fileUpload3(MultipartFile upload) throws IOException { //必须和表单文件上传域的name属性一致
        System.out.println("inter-server springmvc upload");
        //定义上传文件服务器的路径
        String path = "http://localhost:9091/FileuploadServer_war_exploded/uploads/";
        //获取文件唯一名称
        String filename = upload.getOriginalFilename();
        String uuid = UUID.randomUUID().toString().replace("-", "");
        filename = uuid + filename;//生成唯一文件名

        //创建客户端对象
        Client client = Client.create();
        //和图片服务器连接
        WebResource resource = client.resource(path + filename);//在path的末尾加了/

        //上传文件
        resource.put(upload.getBytes());  //通过字节传输
        return "hello";
    }
```

## 七、异常处理

异常处理器

拦截器Interceptor 类似Servlet里的Filter,对处理器Controller进行预处理和后处理，进出都要放行。可以配置为Controller chain

过滤器配置了/* 对所有访问都要拦截。拦截器只拦截访问控制器的方法。访问jsp、html、css、js、images不会拦截

### 1、自定义异常处理器

异常类

```java
//自定义异常类
public class SysException extends Exception {

    private String msg;


    public SysException(String msg) {
        this.msg = msg;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }
}
```

异常处理器

```java
//自定义异常处理器
public class SysExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception ex) {
        SysException e = null;
        if(e instanceof SysException){
            e = (SysException) ex;
        }else {
            e = new SysException("系统正在维护");
        }
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("errorMsg",e.getMsg()); //从e对象获取，存入域中,在jsp中通过el表达式获取
        modelAndView.setViewName("error");  //跳转到自定义的异常页面
        return modelAndView;
    }
}
```

配置异常处理器

```xml
 <!--配置异常处理器-->
    <bean id="sysExceptionResolver" class="cc.landfill.exeception.SysExceptionResolver"/>
```

在Controller抛出异常，前端控制器把异常交给异常处理器处理，异常处理器返回error.jsp给前端控制器。响应给浏览器

```java
@Controller
@RequestMapping("/user")
public class ExceptionController {

    @RequestMapping("/testException")
    public String testException() throws SysException {
        //模拟异常，如果在Controller不处理，会直接抛给前端控制器，会直接显示到页面上
        try {
            int i = 10/0;
        } catch (Exception e) {
            e.printStackTrace();
            //抛出自定义的异常信息
            throw new SysException("出现错误");
        }

        return "hello";
    }
}
```

### 2、拦截器

自定义拦截器，重写方法

```java
//自定义拦截器
public class MyInterceptor implements HandlerInterceptor {

    //预处理，在controller之前执行。return true 放行，执行下一个拦截器。如果没有，执行Controller方法
    //如登录验证等
    //return false就不放行，跳转到某些页面
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("MyInterceptor");
        //request.getRequestDispatcher("/WEB-INF/pages/error.jsp").forward(request,response);
        return true;  //false就不放行，直接转发到错误页面去了
    }

    //后处理 Controller执行后，转发到jsp页面前
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("后处理");
        request.getRequestDispatcher("/WEB-INF/pages/error.jsp").forward(request,response);
        //如果后处理指定转发的页面，不再跳转到controller指定的页面了，但是原来jsp的页面的信息还是输出了。为什么还能输出？
    }

    //在jsp页面之后执行
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("jsp页面后afterCompletion()"); //servlet已经执行结束了，无法再跳转页面，但是可以做一些其他的处理，比如释放资源
        
   //执行顺序 :preHandle  ->  controller内方法 ->postHandler-> controller转发到的jsp-> afterCompletion()
        
    }
}
```

配置拦截器

```xml
<!--配置拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/user/*"/>      <!--要拦截的方法,拦截user目录下的所有方法， /**拦截所有方法-->
           <!-- <mvc:exclude-mapping path=""/>-->  <!--不拦截的方法-->
            <bean class="cc.landfill.interceptor.MyInterceptor"/> <!--把自定义的拦截器传入配置，拦截器里写具体的逻辑-->
        </mvc:interceptor>
        <mvc:interceptor>
            <mvc:mapping path="/user/*"/>            
            <bean class="cc.landfill.interceptor.MyInterceptor2"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

多个拦截器按照配置的顺序执行

![Interceptor Chain 执行流程](https://i.loli.net/2020/07/20/yZDUCkHspYlOXA1.png)

## 八、SSM框架整合

### 1、导入SSM的各种依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>cc.landfill</groupId>
  <artifactId>SSM</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>SSM Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <spring.version>5.0.2.RELEASE</spring.version>
    <slf4j.version>1.6.6</slf4j.version>
    <log4j.version>1.2.12</log4j.version>
    <mysql.version>8.0.20</mysql.version>
    <mybatis.version>3.4.5</mybatis.version>

  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>compile</scope>
    </dependency>
    <!-- spring -->
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.6.8</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>${mysql.version}</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.0</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>
    <!-- log start -->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>${log4j.version}</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>${slf4j.version}</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>${slf4j.version}</version>
    </dependency>
    <!-- log end -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>${mybatis.version}</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.0</version>
    </dependency>
    <dependency>
      <groupId>c3p0</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.1.2</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.10</version>
    </dependency>

  </dependencies>

</project>

```

### 2、创建dao、service、bean、controller的包及其接口、实现类、实体类

### 3、配置Spring

并且测试：创建对象、调用方法 。log4j的配置文件也可以先导入。

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                            http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context
                            http://www.springframework.org/schema/context/spring-context.xsd
                            http://www.springframework.org/schema/aop
                            http://www.springframework.org/schema/aop/spring-aop.xsd
                            http://www.springframework.org/schema/tx
                            http://www.springframework.org/schema/tx/spring-tx.xsd">
    <!--导入名称空间 aop context tx -->

    <!--开启注解扫描,只把Service和Dao层交给Spring的IoC容器管理,Controller由SpringMVC管理-->
    <context:component-scan base-package="cc.landfill.service">
        <!--排序Controller注解-->
        <context:exclude-filter type="annotation" 		expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
</beans>
```

### 4、配置SpringMVC

（1）在web.xml配置上前端控制器

```xml
<!--前端控制器-->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

  <!--中文乱码过滤器-->
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>  <!--过滤所有资源-->
  </filter-mapping>
```

（2）创建springmvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--开启注解扫描，只扫描Controller-->
    <context:component-scan base-package="cc.landfill">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <!--视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <!--过滤静态资源-->
    <mvc:resources mapping="/js/**" location="/js/"/>
    <mvc:resources mapping="/css/**" location="/css/"/>
    <mvc:resources mapping="/images/**" location="/images/"/>

    <!--开启SpringMVC注解支持-->
    <mvc:annotation-driven/>
</beans>
```

（3）写个index.jsp 和 跳转页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>
<a href="account/testSpringMVC">test</a>
</h1>
</body>
</html>



<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>跳转成功</h1>
</body>
</html>

```

（4）在Controller上配置注解

```java
@Controller
@RequestMapping("/account")
public class AccountController {

    @RequestMapping("testSpringMVC")
    public String testSpringMVC(){
        System.out.println("testSpringMVC");
        return "success";
    }
}
```

（5）部署项目到服务器，访问页面成功跳转

### 5、Spring整合SpringMVC

SpringMVC的前端控制器已经在web.xml里配置过了，但是Spring的配置文件自始至终都没有被加载过。因此需要
利用监听器（ContextLoaderListener,在spring-web包)在ServletContext（生命周期同Tomcat服务器）创建的时候，去加载Spring的配置文件，创建Web版的工厂，存储ServletContext对象

在web.xml配置监听器

```xml
 <!--配置Spring的监听器，默认只加载WEB-INF目录下的applicationContext.xml配置文件-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value> <!--加载类路径下的配置文件-->
  </context-param>
```

在Controller注入Service对象。调用方法。整合成功

```java
@Controller
@RequestMapping("/account")
public class AccountController {
    @Autowired
    private AccountService accountService;
    @RequestMapping("testSpringMVC")
    public String testSpringMVC(){
        System.out.println("testSpringMVC");

        //调用注入对象的方法
        accountService.findAll();
        return "success";
    }
}
```

### 6、配置Mybatis

核心配置文件SqlMapConifg.xml, 配置数据库的environment，使用mysql8.0.20和druid。将mapper注册

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配置环境-->
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///ssm?useSSL=true&amp;useUnicode=true&amp;CharacterEncoding=UTF-8&amp;serverTimezone=Asia/Shanghai"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 使用的是注解 -->
    <!--引入映射文件，注册mapper-->
    <mappers>
       <!-- <mapper resource="cc.landfill.dao.AccountDao.xml"/>-->  <!--使用配置文件的方式注册-->
        <!-- <mapper class="cc.landfill.dao.AccountDao"/> -->   <!--使用注解的方式注册-->
        <package name="cc.landfill"/> <!-- 该包下所有的dao接口都可以使用 -->
    </mappers>
</configuration>

```

直接使用注解配置sql语句

```java
//AccountDao接口

public interface AccountDao {

    //查询所有
    @Select("select * from account")
    List<Account> findAll();

    //保存账户信息
    @Insert("insert into account (name,money) values (#{name},#{money})")
    void saveAccount(Account account);
}

```

测试

```java
public class TestMybatis {

    @Test
    public void test1() throws Exception {
        //加载配置文件
        InputStream is = Resources.getResourceAsStream("SqlMapConifg.xml");
        //创建SqlSessionFactory对象
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
        //创建sqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //获取mapper代理对象
        AccountDao mapper = sqlSession.getMapper(AccountDao.class);
        //调用方法
        List<Account> list = mapper.findAll();
        list.forEach(System.out::println);  //输出Account{id=1, name='tom', money=2300.0}
        //释放资源
        sqlSession.close();
    }
    
    @Test
    public void testInsert() throws Exception {
        //加载配置文件
        InputStream is = Resources.getResourceAsStream("SqlMapConifg.xml");
        //创建SqlSessionFactory对象
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
        //创建sqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //获取mapper代理对象
        AccountDao mapper = sqlSession.getMapper(AccountDao.class);
        //调用方法
        Account account = new Account();
        account.setName("jack");
        account.setMoney(2300);
        mapper.saveAccount(account);
        //提交事务
        sqlSession.commit();  //记得提交事务
        //释放资源
        sqlSession.close();
    }
}
```

### 7、Spring整合Mybatis

把Mybatis生成的代理对象交由Spring的IoC容器管理

在Spring的配置文件applicationContext.xml去配置数据库连接池，创建SqlSessionFactory，扫描dao包的注解，配置这些就不再需要Mybatis的核心配置文件了。

```xml
<!--整合Mybatis，做的事情和使用JdbcTemplate的时候差不多-->
    <!--配置连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>  <!--这里注入属性要和DruidDataSource定义的一样-->
        <property name="url" value="jdbc:mysql:///ssm?useSSL=true&amp;useUnicode=true&amp;CharacterEncoding=UTF-8&amp;serverTimezone=Asia/Shanghai"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>
    <!--配置工厂对象 SqlSessionFactory，通过连接池来创建工厂-->
    <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>  <!--引入dataSource-->
    </bean>
    
    <!--配置接口所在的包-->
    <bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="cc.landfill.dao"/>
    </bean>
```

可以在Service中注入dao的代理对象，调用crud方法

在Controller中可以获取查询的结果，并且存到Model，转发给页面展示

### 8、Spring中配置声明式的事务管理

在Spring的配置文件applicationContext.xml配置事务管理器

```xml
 <!--Spring声明式事务管理-->
    <!--配置事务管理器-->
    <bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="dataSourceTransactionManager">
        <tx:attributes>
            <tx:method name="find*" read-only="true"/>
            <tx:method name="*" isolation="REPEATABLE_READ"/> <!--设置事务的一些属性-->
        </tx:attributes>
    </tx:advice>
    <!--配置AOP增强-->
    <aop:config>  <!--切入点-->
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* cc.landfill.service.impl.*ServiceImpl.*(..))"/>
    </aop:config>
```

