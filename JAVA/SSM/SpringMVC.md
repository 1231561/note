# SpringMVC

### 什么是MVC

MVC是一种软件框架思想，将软件按模型、视图、控制器来划分。

**M：Model，模型层，指工程中的JavaBean，作用是处理数据**

JavaBean分为两类：

1、实体Bean：专门存储业务数据的，和数据库表建立联系的类，如Student和User等。

2、业务处理Bean：指Service或Dao对象，专门用于处理业务逻辑和数据访问。

**V：view，视图层，指工程中的html或jsp页面，作用是与用户进行交互，展示数据。**

**C:Controller,控制层，指工程中的Servlet，作用是接受请求，处理请求，和响应浏览器。**

### SpringMVC

SpringMVC基于Servlet，封装了许多处理请求的功能方法，我们可以直接使用对应的请求处理方法来处理请求。

SpringMVC通过强大的**前端控制器DispatcherServlet**，对请求响应响应进行统一处理，如获取请求参数、请求转发，重定定向等操作。极大简化了操作。

### 配置SpringMVC前端控制器

**扩展式配置web.xml**

**因为默认配置web.xml的SpringMVC配置文件默认位于WEB-INF下，而maven环境下的配置文件要求在Resource下，所以我们使用扩展式配置web.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
<!--    配置SpringMVC前端控制器-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<!--        配置SpringMVC配置文件的路径,和名称-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springMVC.xml</param-value>
        </init-param>
<!--        将SpringMVC初始化时间设置在服务器启动时间-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
<!--        设置访问路径为上下文的所有请求路径-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

**配置SpringMVC配置文件**

配置Thymeleaf视图解析器的方式是固定的，不用亲自编写。

Thymeleaf视图解析器的作用就是将请求控制器中的方法返回的视图名称字符串解析，加上视图前缀和视图后缀，得到完成的请求转发地址。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
        <context:component-scan base-package="com.mvc.controller"></context:component-scan>
    <!-- 配置Thymeleaf视图解析器 -->
    <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="order" value="1"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                <property name="templateResolver">
                    <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">

                        <!-- 视图前缀 -->
                        <property name="prefix" value="/WEB-INF/templates/"/>

                        <!-- 视图后缀 -->
                        <property name="suffix" value=".html"/>
                        <property name="templateMode" value="HTML5"/>
                        <property name="characterEncoding" value="UTF-8" />
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
</beans>
```

**创建请求控制器**

前端请求控制器对浏览器发送的请求进行统一的处理。

编一个方法返回首页的视图名称，视图解析根据视图名称加上视图前缀和视图后缀，就能得到完整的工程路径，浏览器就能访问到首页。

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {
    @RequestMapping("/")//请求映射，当浏览器访问到/时，就会调用下面的方法，返回视图名称，加上视图前缀和视图后缀，就能得到完整的工程路径，并跳转
    public String index(){
        return "index";
    }
}
```

### 通过超链接跳转到其他页面

1、**创建目标页面：**

超链接使用thymeleaf语句为自动加上上下文路径，

点击超链接就会访问正确的请求地址![image-20220319151541811](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220319151541811.png)，请求到服务器。

![image-20220319151230268](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220319151230268.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>首页</h1><br>
<!--@{/target}会为浏览器访问的地址加上工程路径即tomcat服务器的上下文路径-->
<a th:href="@{/target}">访问目标页面target.html</a>
</body>
</html>
```

**请求控制器加上返回target视图名称方法**

​	**当超链接的get请求地址访问到/target时，SpringMVC配置文件开启配件扫描，找到 @RequestMapping对应注解里的value是否相同来决定调用哪个方法，就会调用下面的 getTarget方法，返回视图名称，经过Thymeleaf视图解析器解析，加上视图前缀和视图后缀，就能得到完整的target.html资源路径（文件路径），并请求转发到完整的target.html资源路径，就能访问到target.html了。**

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {
    @RequestMapping("/")//请求映射，当浏览器访问到/时，就会调用下面的方法，返回视图名称，加上视图前缀和视图后缀，就能得到完整的工程路径
    public String index(){
        return "index";
    }
    @RequestMapping("/target")//当超链接的get请求地址访问到/target时，就会调用下面的方法，返回视图名称，加上视图前缀和视图后缀，就能得到完整的工程路径
    public String getTarget(){
        return "target";
    }
}
```

超链接请求访问target.html页面成功。

![image-20220319152354026](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220319152354026.png)

###  @RequestMapping注解

**@RequestMapping的value值对应一个请求地址，就是你要通过发送给服务器什么地址来获得对应的页面地址。**

​		就比如，我要通过给服务器请求/这个地址![image-20220319170655860](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220319170655860.png)，得到index.html这个页面，我就得通过@RequestMapping请求映射，找到对应返回"index"字符串的方法。所以我就可以在返回"index"字符串的方法上加上@RequestMapping（value = "/"),来建立发送给服务器的路径和服务器请求的真实文件路径之间的桥梁。

#### 控制器多个方法对应一个请求的情况

如果一个相同的服务器请求，即 @RequestMapping的value值一样时，方法的返回值不同，DispatcherServlet就不知到要请求转发到index页面，还是target页面，就会报错。

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
@Controller
public class ControllerTest {
    @RequestMapping("/")
    public String index(){
        return "index";
    }
    @RequestMapping("/")
    public String target(){
        return "target";
    }
}
```

 ### @RequestMapping注解标识的位置

@RequestMapping注解可以标识一个类或一个方法。

1、标识一个类：表示映射初始的请求路径（有效解决上面的控制器多个方法对应一个请求的问题）

2、标识一个方法：表示映射具体的请求路径

实例：

首页：

有跳转到booklist.html和userlist.html的页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>首页</h1><br>
<!--@{/target}会为浏览器访问的地址加上工程路径-->
<a th:href="@{/target}">访问目标页面target.html</a>
<a th:href="@{/book/list}">访问booklist.html</a>
<a th:href="@{/user/list}">访问userlist.html</a>
</body>
</html>
```

有两个控制器，他们映射的初始的请求路径不同（类上的@RequestMapping），但是映射的具体的请求路径相同（方法上的@RequestMapping）。

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
@Controller
@RequestMapping("/user")
public class ControllerTest1 {
    @RequestMapping("/list")
    public String userList(){
        return "userlist";
    }
}
@Controller
@RequestMapping("/book")
class ControllerTest2 {
    @RequestMapping("/list")
    public String bookList(){
        return "booklist";
    }
}
```

测试：

![image-20220319182627377](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220319182627377.png)

访问booklist.html和userlist.html可以请求到不同的页面。

综上所述：@RequestMapping注解标识在类上（value值不同）,可以有效区分相同@RequestMapping注解的不同方法。

### @RequestMapping注解的参数（标记类或方法）

1、value ：value是个数组类型的参数，可以设置多个请求来映射同一个页面

2、method：method也是一个数组类型的参数，可以设置多个方法，进行请求方法的筛选，默认不进行请求方法筛选。

**实例：**

**控制器访问target页面的方法：**

```java
@RequestMapping(value = {"/target1","/target2"},method = RequestMethod.GET)
public String target(){
    return "target";
}
```

get方法和post方法的测试：

```html
<a th:href="@{/target1}">访问目标页面target.html</a>
<a th:href="@{/target2}">访问目标页面target.html</a>
<form th:action="@{/target1}" method="post">
```

结果：通过/target1和/target2这两个路径都能请求访问到target页面，但是表单提交是post请求，所以访问不到target页面（405）。

**@RequestMappingd的子类**

@GetMapping

在@RequestMappingd的基础上筛选出get请求

@PostMapping

在@RequestMappingd的基础上筛选出post请求

**@RequestMappingd注解参数params**

例如：要求请求要有username和password参数，并且要求username=admin,password=123456。

```java
@RequestMapping(value = {"/target1","/target2"},params = {"username=admin,password=123456"})
public String target(){
    return "target";
}
```

**@RequestMappingd注解参数headers**

在前面的条件都满足的情况下，还要满足请求头的loclhost属性的值为8080

```java
@RequestMapping(value = {"/target1","/target2"},params = {"user=admin","password=123456"},headers = "localhost=8080")
public String target(){
    return "target";
}
```

**@RequestMappingd注解支持ant型风格**

**在value参数里**

？：表示任意的单个字符   如a?a，则可以通过a1a,a2a等任意字符请求访问

*：表示任意的0个或多个字符

\**：表示任意的一层或多层目录   

注意：在使用\**时，b只能使用/**/xxx的方式

### SpringMVC支持路径中的占位符（重点）

SpringMVC支持restful风格的占位符，restful风格的占位符经常被SpringBoot使用。

​		restful风格的占位符的具体使用方式：在@RequestMappingd注解的value路径参数加上占位符{参数名}，该占位符可以从请求中相应位置获取参数值，之后在@RequestMappingd注解标记的方法中加上形参，@PathVariable可以给形参注入值，**方法名（@PathVariable（参数1名）形参1，@PathVariable（参数2名）形参2 ...）**，之后形参就可以直接使用。

restful风格的请求路径：/target/1

对应的@RequestMappingd注解：/target/{id}

对应的方法和参数：public String getTarget(@PathVariable("id") Integer id){}

**实例：**

控制器的某方法：

```java
@RequestMapping("/target/{id}/{username}")
public String target1(@PathVariable("id") Integer id,@PathVariable("username") String username){
    System.out.println("id:"+id+" username:"+username);
    return "target";
}
```

对应的超链接（get请求）：

格式与target1方法的@RequestMapping一致，可以访问target页面

```html
<a th:href="@{/target/1/admin}">访问目标页面target.html</a>
```

测试输入：@PathVariable注解给id和userna都注入了超链接请求的1和admin。

![image-20220320110904488](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320110904488.png)

### 原始ServletAPI获取Param参数

​		DispatcherServlet可以解析请求，然后找到对应的@RequestMapping请求映射，然后将请求参数赋值给对应请求映射的方法形参HttpServletRequest。不过这种获取请求参数的方法不推荐使用，因为	DispatcherServlet已经相应方法，直接获取请求参数。

```java
@Controller
public class ParamController {
    @RequestMapping("/target")
    public String ServletApi(HttpServletRequest request){
        String username = request.getParameter("username");
        String id =request.getParameter("id");
        System.out.println("id"+id+" username"+username);
        return "target";
    }
}
```

对应超链接（get请求）：

```html
<a th:href="@{/target(id=3,username='admin')}">访问目标页面target.html</a>
```

结果：

![image-20220320114936769](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320114936769.png)

### 直接通过控制器方法的形参获取请求参数

​		DispatcherServlet解析请求后，确定对应映射的控制器方法**，如果该控制器方法的形参和请求解析出来参数名一致**，DispatcherServlet就可以给形参直接赋值。

实例：

**控制器方法：**

如果请求参数有username和password，并且都有值，DispatcherServlet可以将对应请求参数的值注入方法形参。

```java
@RequestMapping("/paramTest")
public String getParam(String username,String password,String hobby){
    System.out.println("username:"+username+" password:"+password+" hobby:"+hobby);
    return "paramTest";
}
```

**表单提交（get方法）：**

```html
<form th:action="@{/paramTest}" method="get">
    用户名：<input type="text" name="username">
    密码: <input type="password" name="password">
    爱好:<input type="checkbox" name="hobby" value="a">a
        <input type="checkbox" name="hobby" value="b">b
        <input type="checkbox" name="hobby" value="c">c
        <input type="submit">
</form>
```

**请求地址栏：**

http://localhost:8080/SpringMVC2/paramTest?username=admin&password=123456&hobby=a&hobby=b&hobby=c

结果：出现同名的参数hobby，获取时使用String接收，DispatcherServlet就会将它们拼成字符串。
![image-20220320122911034](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320122911034.png)

请求参数被成功获取。

如果同名的参数hobby使用 String接收：

![image-20220320123430079](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320123430079.png)

### @RequestParam注解（标记变量）

@RequestParam注解的作用就是给请求参数和控制器方法参数建立连接关系

**value属性：**

当请求参数的名字和控制器方法形参名字不同，可以通过@RequestParam注解的value属性给控制器方法型参起别名。

**required属性：**

默认为true，表示该控制器方法型参必须有对应的请求参数，否则报404，除非defaultValue属性有默认值给该控制器方法参数。

如果required属性设置为false，表示如果该控制器方法形参没有对应的请求参数，这个该控制器参数为null，如果有对应的请求参数，就注入请求参数的值。

**defaultValue属性**

该属性就是给没有对应的请求参数的控制器方法形参赋默认值，可以避免required属性为true时的报错。defaultValue属性比较常用。

**实例：**

控制器方法

给控制器方法参数username起别名，当请求参数是username或user_name都可以访问页面

required为true

username默认值为"abc"

```java
@RequestMapping("/paramTest")
public String getParam(@RequestParam(value = "user_name",required = true,defaultValue = "abc") String username, String password, String[] hobby){
    System.out.println("username:"+username+" password:"+password+" hobby:"+ Arrays.toString(hobby));
    return "paramTest";
}
```

get请求：

http://localhost:8080/SpringMVC2/paramTest?password=123456&hobby=a&hobby=b&hobby=c

没有对应username的请求参数，给username默认值“abc

![image-20220320132145736](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320132145736.png)

请求用别名user_name：用可以获取

http://localhost:8080/SpringMVC2/paramTest?user_name=admin&password=123456&hobby=a&hobby=b&hobby=c

![image-20220320132338113](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320132338113.png)

### @RequestHeader和@CookieValue注解

@RequestHeader注解的作用是通过请求头的属性名获取当前服务器请求的请求头的属性名值（请求头和控制器方法参数建立映射关系）。

@CookieValue注解的作用是通过Cookie名字获取当前请求的相关Cookie值（Cookie和控制器方法参数建立映射关系）。

**代码实例**

首先通过请求映射访问target页面，创建会话，此时对应的Cookie JSESSIONID出现在响应头，浏览器将创建这个Cookie。

![image-20220320151910011](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320151910011.png)

然后通过请求映射访问paramTest页面，@RequestHeader获取当前host属性的值，@CookieValue获取Cookie名为JSESSIONID的值。

![image-20220320151958297](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320151958297.png)

```java
@Controller
public class ParamController {
    @RequestMapping("/target")
    public String ServletApi(HttpServletRequest request){
        request.getSession();
        String username = request.getParameter("username");
        String id =request.getParameter("id");
        System.out.println("id"+id+" username"+username);
        return "target";
    }
    @RequestMapping("/paramTest")
    public String getParam(@RequestParam(value = "user_name",required = true,defaultValue = "abc") String username, String password, String[] hobby,
                           @RequestHeader("Host") String host,
                           @CookieValue("JSESSIONID") String jSESSIONID ){
        System.out.println("username:"+username+" password:"+password+" hobby:"+ Arrays.toString(hobby)+"host:"+host+"JSESSIONID"+jSESSIONID );
        return "paramTest";
    }
}
```

结果：成功获取Host和Cookie值

![image-20220320152154386](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320152154386.png)

### 通过实体类获取请求参数

当请求参数的数量很大，种类很多时（如一个表单），显然用上面的方式获取请求参数很不合适，这时我们可以创建一个实体类（Bean），一个成员属性对应一个请求参数类型，就可以直接用一个实体类对象来保存请求参数。

用户表单提交请求：

```html
<form th:action="@{/testBean}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    性别：<input type="radio" name="sex" value="男">男<input type="radio" name="sex" value="女">女<br>
    年龄：<input type="text" name="age"><br>
    邮箱：<input type="text" name="email"><br>
    <input type="submit">
</form>
```

创建一个实体类User：

一个成员属性对应一个请求参数类型

```java
package com.Bean;

public class User {
    private String username;
    private String password;
    private String sex;
    private Integer age;
    private String email;

    public User(String username, String password, String sex, Integer age, String email) {
        this.username = username;
        this.password = password;
        this.sex = sex;
        this.age = age;
        this.email = email;
    }

    public User() {
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", sex='" + sex + '\'' +
                ", age=" + age +
                ", email='" + email + '\'' +
                '}';
    }
}
```

控制器方法：

**DispatcherServlet将请求参数的用户名密码等值注入User对象的每一个同名成员属性**

```java
@RequestMapping("/testBean")
public String getBean(User user){
    System.out.println(user);
    return "target";
}
```

结果:

请求字符不匹配出现乱码

![image-20220320164112046](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320164112046.png)

​		**解决方法：在DispatcherServlet获取请求数据前，将请求字符格式设置为UTF-8，因为filter的启动时间要早于DispatcherServlet所以可以在web.xml配置文件创建一个filter，给每个一个请求都加上UTF-8**

**设置过滤器：**

```xml
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
<!--将请求字符格式设置为UTF-8-->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <!--将响应字符格式设置为UTF-8-->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

![image-20220320163448926](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320163448926.png)

### ModelAndView

​		前面我们向请求域共享数据时，通过HttpServletRequest，使用的是方法request.setAttribute("键值"，"数据")，然后请求转发到html或者jsp页面，然后获取请求域中的数据。

​		现在SpringMVC提供了ModelAndView实现类来完成请求域的共享和返回视图名称。

	#### ModelAndView和HttpServletRequest的对比

控制器方法：

```java
//使用HttpServletRequest向请求域共享数据
@RequestMapping("ServletAPI")
public  String ServletAPI(HttpServletRequest request){
    request.setAttribute("requestTest","Hello ServletRequest !!!");
    //返回target视图名称，相当于请求转发。
    return "target";
}
//使用ModelAndView向请求域共享数据
@RequestMapping("ModelAndViewTest")
public ModelAndView ModelAndViewTest(){
    ModelAndView mav = new ModelAndView();
    //向请求域保存数据（键值对的方式）
    mav.addObject("requestTest","hello ModelAndViewTest!!!");
    //给Thymeleaf视图解析器发送视图名称，请求转发。
    mav.setViewName("target");
    return mav;
}
```

**请求转发目标页面：**

通过Thymeleaf语句获取请求域中的数据。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<p th:text="${requestTest}"></p>
</body>
</html>
```

**测试：**

分别发送两个请求：

![image-20220320192729289](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320192729289.png)

![image-20220320192743132](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320192743132.png)

结果说明两种方法都能向请求域共享数据。

### Model、Map和ModelMap

可以使用Model、ModelMap和Map将数据共享到请求域。

```java
@RequestMapping("Model")
public String ModelTest(Model model){
    model.addAttribute("requestTest","hello Model!!!");
    return "target";
}
@RequestMapping("Map")
public String MapTest(Map<String,Object> map){
    map.put("requestTest","hello Map!!!");
    return "target";
}
     @RequestMapping("ModelMap")
    public String ModelMapTest(ModelMap modelMap){
        modelMap.addAttribute("requestTest","hello ModelMap!!!");
        return "target";
    }
```

### Model、Map和ModelMap的关系

分别输入它们的三个对象model、map、modelMap

发现它们的toString方法是一样的

![image-20220320213728163](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320213728163.png)

再输出三个对象的所属类的名字

发现都是**BindingAwareModelMap**

![image-20220320214143463](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320214143463.png)

**综上所述：BindingAwareModelMap是Model、Map接口的实现类，是ModelMap的子类**

### 总结

无论是哪种方法来将数据共享到请求域中，都得被封装成ModelAndView对象，来供Thymeleaf解析器解析。

通过debug发现，五个控制器方法的方法栈都有doDisparch方法，该方法完成对ModelAndView对象的赋值。

![image-20220320223112352](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220320223112352.png)

### SpringMVC的视图

视图的作用是将数据渲染，将model数据展示给用户。

SpringMVC的视图接口是View

SpringMVC默认的视图有转发视图和重定向视图

#### 1、ThymeleafView

​	当控制器方法返回的视图名称没有前缀时，就会被Thymeleaf视图解析器解析，并创建为其创建一个ThymeleafView，然后将视图名称加上前缀和后缀完成请求转发。

#### 2、InternalResourceView（转发视图）

​		当控制器方法返回的视图名称有forward：前缀时（如forward:/ThymeleafViewTest），Thymeleaf视图解析器不会解析该视图名称，而是将forward前缀去掉，然后将forward后面的路径进行请求转发，该过程创建的视图解析器就是InternalResourceView。

#### 3、RedirectView（重定向视图）

​	当控制器方法返回的视图名称有redirect：前缀时（如redirect:/ForwardTest），Thymeleaf视图解析器不会解析该视图名称，而是将redirect前缀去掉，然后将redirect后面的路径进行重定向跳转，该过程创建的视图解析器就是RedirectView。

实例：

当我们想要重定向到target页面，但是重定向不能直接访问WEB-INF下面的页面，所以只能先重定向到请求转发，通过请求转发到WEB-INF下的target页面。

target页面：

![image-20220321114006840](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220321114006840.png)

控制器方法：

```java
@Controller
public class ViewController {
    //通过Thymeleaf解析器解析后请求转发
    @RequestMapping("/ThymeleafViewTest")
    public String ThymeleafViewTest(){
        return "target";
    }
    //当视图名称不满足Thymeleaf解析器的要求，但又要请求转发到target页面，只能先请求转发到/ThymeleafViewTest的请求
    public String ForwardTest(){
        return "forward:/ThymeleafViewTest";
    }
    //重定向到ThymeleafViewTest请求，再请求转发到target页面
    @RequestMapping("/RedirectViewTest")
    public String RedirectView(){
        return "redirect:/ThymeleafViewTest";
    }
}
```

### 视图控制器

视图控制器在SpringMVC的配置文件里配置。

可以使用视图控制器来代替控制器方法来完成请求转发。

比如我们可以创建一个视图控制器来代替控制器方法访问首页：

在SpringMVC的配置文件加上这两句即可：

 path表示请求地址，view-name表示视图名称

当在SpringMVC的配置文件里配置一个视图控制器时，其他的控制器方法（请求映射）全部失效。

所以还得开启注解驱动：\<mvc:annotation-driven>\</mvc:annotation-driven>

```xml
<mvc:view-controller path="/" view-name="index"></mvc:view-controller>
<mvc:annotation-driven></mvc:annotation-driven>
```

### RESTFul

全称：**Re**presentational **S**tate **T**ransfer ，表现层状态转移

**RESTFul的核心作用：请求地址URL不变，根据请求方式的不同，对操作资源的方式进行区分**

#### RESTFul实例

假如：

1、使用"/user"请求地址，get请求方法来调用查询所有用户的方法。

2、使用"/user/{id}"请求地址，get请求方法来调用通过id查询用户的方法。

3、使用"/user"请求地址，post请求方法来调用添加用户的方法。

4、使用"/user"请求地址，put请求方法来调用查询所有用户的方法。

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class RESTFulTest {
    @RequestMapping(value = "/user",method = RequestMethod.GET)
    public String queryAll(){
        System.out.println("查询所有用户信息");
        return "target";
    }
    @RequestMapping(value = "/user/{id}",method = RequestMethod.GET)
    public String queryById(){
        System.out.println("根据查询用户信息");
        return "target";
    }
    @RequestMapping(value = "/user",method = RequestMethod.POST)
    public String insertUser(String username,String password){
        System.out.println("用户名:"+username);
        System.out.println("密码："+password);
        return "target";
    }

    @RequestMapping(value = "/user",method = RequestMethod.PUT)
    public String updateUser(String username,String password){
        System.out.println("更新用户");
        System.out.println("用户名:"+username);
        System.out.println("密码："+password);
        return "target";
    }
    @RequestMapping("/RESTFulTest")
    public String getRESTFulTest(){
        return "RESTFulTest";
    }
}
```

测试页面：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<a th:href="@{/user}">查询所有用户信息</a><br>
<a th:href="@{/user/1}">通过id查询用户信息</a><br>
<form th:action="@{/user}" method="post">
  用户名:<input type="text" name="username"><br>
  密码:<input type="password" name="password"><br>
      <input type="submit" value="添加用户">
</form>
<form th:action="@{/user}" method="post">
    <input type="hidden" name="_method" value="put">
    用户名:<input type="text" name="username"><br>
    密码:<input type="password" name="password"><br>
    <input type="submit" value="更新用户信息">
</form>
</body>
</html>
```

### HiddenHttpMethodFilter（隐藏域过滤器）

​		由于浏览器只能发送get和post请求，所以要想使用浏览器发送put或delete请求，就得通过HiddenHttpMethodFilter配合表单发送的隐藏域来实现。

实现方法：

首先在web.xml中配置HiddenHttpMethodFilter，让它过滤所以请求，并通过表单请求的隐藏域值来覆盖post请求，

```xml
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

如下列表单：

<form th:action="@{/user}" method="post">
    <input type="hidden" name="_method" value="put">
    用户名:<input type="text" name="username"><br>
    密码:<input type="password" name="password"><br>
    <input type="submit" value="更新用户信息">
</form>

隐藏域：\<input type="hidden" name="_method" value="put">

HiddenHttpMethodFilter通过隐藏域的name="_method"调用方法，将value的值"put"大写，并将POST方法替换,成PUT请求。

**注意：只有表单的提交方式是POST，才符合HiddenHttpMethodFilter中替换方法的要求，才能被隐藏域的方法替换。**

### SpringMVC处理静态资源

由于SpringMVC的核心组件DispatcherServlet处理不了静态资源(static包下的资源)，所以得使用default-servlet（默认servlet）来处理静态资源。

SpringMVC配置文件得使用这两句配置：

```xml
开启mvc注解驱动
<mvc:annotation-driven></mvc:annotation-driven>
开放对静态资源的访问
<mvc:default-servlet-handler></mvc:default-servlet-handler>
```

配置完成后，当DispatcherServlet通过查找控制器方法映射找不到对静态资源访问的方法时，就会通过default-servlet来查找静态资源。

### HttpMessageConverter（报文信息转换器）

HttpMessageConverter可以将请求报文转化为java对象，或将java对象转化为响应报文。

**@RequestBody:获取请求体字符串,给形参赋值，一个很重要的注解，在微服务领域经常用到。**

RequestEntity<String>：该类封装了请求报文，可以通过实例化对象来获取请求头和请求体。

@ResponseBody ：标记控制器方法，该控制器方法的返回值就是响应体（返回值可以是字符串、对象等类型）。

```java
import org.springframework.http.RequestEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Controller
public class controllerTest {
    @RequestMapping("/requestBody")
    public String getRequestBody(@RequestBody String requestBody){
        System.out.println(requestBody);
        return "success";
    }
    @RequestMapping("/requestEntity")
    public String getRequestEntity(RequestEntity<String> request){//获取请求报文
        System.out.println("请求头："+request.getHeaders());
        System.out.println("请求体"+request.getBody());
        return "success";
    }
    @RequestMapping("/testResponse")
    public void getResponse(HttpServletResponse response) throws IOException {//原始响应请求方法
        response.getWriter().write("Response");
    }
    @RequestMapping("/testResponseBody")
    @ResponseBody
    public String getResponseBody(){
        return "haha"，;//加了@ResponseBody，响应体就是"haha"，而不是请求转发到haha页面。
    }
}
```

### SpringMVC处理json

​		当使用@ResponseBody注解标记的控制器方法返回一个对象时（响应体为对象），浏览器不能直接解析对象，因为浏览器只能解析报文，所以必须使用json将实体对象转化为json字符串，让浏览器解析。

​		引入json的依赖

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
```

​	引入json的依赖后，我们不用编写对象转化为json字符串的方法体，SpringMVC的配置文件中的mvc注解驱动可以帮助我们自动的将响应到浏览器的对象转化为json字符串。

```xml
<mvc:annotation-driven></mvc:annotation-driven>
```

控制器方法：

```java
@RequestMapping("/testResponseBody")
@ResponseBody
public User getResponseBody(){
    return new User(1,"jack","male");
}
```

结果：

![image-20220322165758822](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220322165758822.png)

### SpringMVC处理ajax

步骤：

1、添加静态资源：jquery-1.7.2.js

![image-20220322203446576](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220322203446576.png)

2、将jquery-1.7.2.js的目录重新打包，使其保存到已经部署过的Tomcat服务器资源。

![image-20220322203440725](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220322203440725.png)

3、SpringMVC配置文件配置默认Servlet

```xml
<mvc:default-servlet-handler></mvc:default-servlet-handler>
```

4、给测试ajax的超链接绑定单击事件

使用Jquery操作ajax，绑定的单击事件的实际请求地址是url+data

即:http://localhost:8080/SpringMVC4/testAjax?username=admin&password=123456

请求发送后，根据请求映射找到返回响应体的控制器方法testAjax，再由data接收响应数据。

```html
<a id="ajax" th:href="@{/testAjax}">ajax</a>//发送请求的超链接
<script type="text/javascript" th:src="@{/script/jquery-1.7.2.js}"></script>
<script type="text/javascript">
   $(function (){
       $("#ajax").click(function (){
           $.ajax({
               url:"http://localhost:8080/SpringMVC4/testAjax",
               method:"POST",
               data:"username=admin&password=123456",
               success:function (data){
                   alert(data)
               },
           });
           return false;
       });
   });
</script>
```

**testAjax方法**：返回响应体

```java
@RequestMapping("/testAjax")
@ResponseBody
public String testAjax(String username,String password){
    System.out.println(username+""+password);
    return "hello,ajax";
}
```

结果：

![image-20220322205022168](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220322205022168.png)

### @RestController注解（重要）

​		因为在微服务的领域中经常使用标记了@ResponseBody注解的控制器方法来返回响应体，所以springMVC提供了@RestController注解，@RestController注解是springMVC提供的一个复合注解，标识在控制器的类上，就相当于为类添加了@Controller注解，并且为其中的每个方法添加了@ResponseBody注解

### 文件下载

文件的下载需要用到SpringMVC提供的ResponseEntity类来返回相应实体对象（包括响应头，响应体）

代码实例：

设置超链接，请求映射下载的控制器方法：

```html
<a th:href="@{/testDown}">图片下载</a>
```

实现下载的控制器方法：

```java
@RequestMapping("/testDown")
public ResponseEntity<byte[]> testDown(HttpSession session) throws Exception {
    //获取ServletContext对象
    ServletContext servletContext = session.getServletContext();
    //获取服务器中文件的真实路径
    String realPath = servletContext.getRealPath("/static/img/荷花.jpg");
    //创建输入流
    InputStream is = new FileInputStream(realPath);
    //创建字节数组，根据输入流文件的大小来设置byte数
    byte[] bytes = new byte[is.available()];
    //将流读到字节数组中
    is.read(bytes);
    //创建HttpHeaders对象设置响应头信息
    MultiValueMap<String, String> headers = new HttpHeaders();
    //设置要下载方式以及下载文件的名字，键值对,设置为附件形式，文件按名可以该。
    headers.add("Content-Disposition", "attachment;filename=1.jpg");
    //设置响应状态码
    HttpStatus statusCode = HttpStatus.OK;
    //创建ResponseEntity对象
    ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, statusCode);
    //关闭输入流
    is.close();
    return responseEntity;
}
```

### 文件上传

文件上传需要导入以下依赖：

```xml
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>
```

文件上传控制器方法：

MultipartFile是SpirngMVC提供的接口，用来接收多种请求方式来上传的文件，一般用于form表单上传的文件。

为了避免同名文件上传时发生内容覆盖，则给每个上传的文件名改成UUID加后缀。

```java
//文件上传
@RequestMapping("/testUp")
public String testUp(MultipartFile photo,HttpSession session) throws IOException {
    String filename = photo.getOriginalFilename();
    //获取文件名后缀
    String lastName = filename.substring(filename.lastIndexOf("."));
    //创建UUID，随机文件名
    String uuid = UUID.randomUUID().toString();
    //将每个上传的文件名改为UUID加文件后缀
    filename =uuid+lastName;
    ServletContext context = session.getServletContext();
    //获取服务器的photo目录的路径
    String photoPath = context.getRealPath("photo");
    //创建photo目录路径的文件对象
    File file = new File(photoPath);
    if (!file.exists()){//如果该目录在系统中不存在
        //创建目录
        file.mkdir();
    }
    String filePath=photoPath+File.separator+filename;
    photo.transferTo(new File(filePath));
    return "success";
}
```

​		要想将上传的文件成功的转化为MultipartFile形式的实现类对象，则要在SpringMVC配置文件加上MultipartFile解析器，将上传文件转化为MultipartFile实现类对象，而且需要给对象加上固定的id：multipartResolver，供对象装配时能在IOC查找的这个对象。

```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
```

测试：

```html
<form th:action="@{/testUp}" method="post" enctype="multipart/form-data">
    文件上传：<input type="file" name="photo">
            <input type="submit">
</form>
```

结果：

![image-20220323122922900](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220323122922900.png)

### 拦截器

SpringMVC的拦截器有有三种抽象方法。

preHandle：该拦截器方法在控制器方法执行之前执行，preHandle返回true表示放行拦截到的请求，false表示拦截不放行，**当preHandle返回false时，后面的控制器方法和两个拦截器方法将不执行**

postHandle:在控制器方法执行之后执行。

afterCompletion:处理完视图和模型数据，渲染视图完毕之后执行。

#### 自定义拦截器

自定义拦截器需要实现HandlerInterceptor接口，重写它的默认方法，来修改方法体。

```java
@Component
public class FirstInterceptor implements HandlerInterceptor {
     public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
         System.out.println(" preHandle");
         return true;
    }

     public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
         System.out.println("postHandle");
     }

     public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
         System.out.println("afterCompletion");
     }

}
```

SpringMVC配置文件配置拦截器：

```xml
        <mvc:interceptors>
<!--    <bean class="com.mvc.interceptor.FirstInterceptor"></bean>-->
<!--            <ref bean="firstInterceptor"></ref>-->
            <!-- 以上两种配置方式都是对所有的请求进行拦截 -->
        <mvc:interceptor>
            <!-- /**表示对所有请求进行拦截，/*表示的上下文路径后的一层路径进行拦截-->
            <mvc:mapping path="/**"/>
            <!--表示哪些路径不被拦截-->
            <mvc:exclude-mapping path="/"/>
             <!--创建自定义拦截器对象-->
            <ref bean="firstInterceptor"></ref>
        </mvc:interceptor>
    </mvc:interceptors>
```

#### 多个拦截器方法的执行行顺序

多个拦截器的preHandle方法执行顺序和拦截器配置的顺序一致

多个拦截器的postHandle方法和afterComplation方法执行顺序和拦截器配置的顺序相反

例如两个拦截器的配置顺序如下：

```xml
<ref bean="firstInterceptor"></ref>
<ref bean="secondInterceptor"></ref>
```

则它们的拦截器方法的执行顺序如下。

![image-20220323172505716](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220323172505716.png)

当有5个拦截器f1,f2,f3,f4,f5

其中只有f3拦截器的preHandle方法返回false，其他的都返回true,则执行preHandle方法的拦截器有f1,f2,f3，没有一个拦截器执行postHandle方法，执行afterCompletion方法的拦截器有f1,f2。

### 异常处理器

SpringMVC提供了处理控制器执行过程中出现异常的接口：HandlerExceptionResolver。

我们一般使用HandlerExceptionResolver的实现类SimpleMappingExceptionResolver来处理异常。

异常处理的方法有基于配置文件和基于注解处理的方法。

#### 1、基于配置文件

配置异常信息处理器：

```xml
<!--   创建SimpleMappingExceptionResolver对象 -->
    <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
<!--        设置异常映射-->
        <property name="exceptionMappings">
            <props>
<!--                设置出现某种异常时的跳转页面，key表示出现的异常类型，标签内容就是视图名称-->
                <prop key="java.lang.ArithmeticException">mathError</prop>
            </props>
        </property>
<!--        设置异常信息，将异常信息共享到请求域中，value的值就是请求域中要获取的key-->
        <property name="exceptionAttribute" value="ex"></property>
    </bean>
```

出现异常的控制器方法：

```java
@RequestMapping("exTest")
public String exTest(){
    int i=1/0;
    return "success";
}
```

测试：

当出现了数学逻辑的异常，控制器方法会被异常信息处理器发现，然后解析到出现了数学逻辑异常，映射到"java.lang.ArithmeticException"，然后就会返回mathError（新的）视图名称，跳转到mathError页面。

![image-20220323183935568](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220323183935568.png)

### 基于注解处理异常

​	SpringMVC提供了一个专门处理异常的注解@ExceptionHandler，该注解是@componont注解的扩展注解，所以@ExceptionHandler也具有将类标识为组件的功能。

```java
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
@ControllerAdvice//处理异常的注解
public class ExceptionController {
    //该注解映射控制器方法的错误类型，参数设置了错误信息，然后用Model将错误信息共享到请求域
    @ExceptionHandler(value = {ArithmeticException.class,NullPointerException.class})
    public String testException(Exception ex, Model model){
        model.addAttribute("ex",ex);
        return "mathError";
    }
}
```

### 注解配置springMVC

使用配置类和注解来代替web.xml和SpringMVC配置文件的功能。

#### 创建初始化类，代替web.xml

创建初始化类WebInit继承AbstractAnnotationConfigDispatcherServletInitializer，当Tomcat服务器启动时就会自动加载初始化类WebInit，就像web.xml会被服务器自动加载。

```java
import org.springframework.web.filter.CharacterEncodingFilter;
import org.springframework.web.filter.HiddenHttpMethodFilter;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

import javax.servlet.Filter;

public class WebInit extends AbstractAnnotationConfigDispatcherServletInitializer {
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }
    //配置SpringMVC
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMVCConfig.class};//需要返回SpringMVC配置文件类的运行时类
    }
    //配置DispatcherServlet能处理的请求的路径，/表示除了jsp外的所以请求。
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
	//配置文字格式过滤器和隐藏域过滤器
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter();
        encodingFilter.setEncoding("UTF-8");
        encodingFilter.setForceResponseEncoding(true);
        HiddenHttpMethodFilter httpMethodFilter = new HiddenHttpMethodFilter();
        return new Filter[]{encodingFilter,httpMethodFilter};

    }
}
```

### 创建SpringMVCConfig配置类，代替SpringMVC配置文件

```java
@Configuration
//扫描组件
    @ComponentScan("com.mvc.controller")
//开启MVC注解驱动
    @EnableWebMvc
    public class SpringMVCConfig implements WebMvcConfigurer {

        //使用默认的servlet处理静态资源

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }


    //配置文件上传解析器
    @Bean
    public MultipartResolver multipartResolver(){
        CommonsMultipartResolver multipartResolver = new CommonsMultipartResolver();
        return multipartResolver;
    }


        //配置拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new FirstInterceptor()).addPathPatterns("/**");
    }


    //配置视图控制

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/success").setViewName("success");
    }


    //配置异常映射
    @Override
    public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
        SimpleMappingExceptionResolver mappingExceptionResolver = new SimpleMappingExceptionResolver();
        Properties properties = new Properties();
        properties.setProperty("java.lang.ArithmeticException","mathError");
        mappingExceptionResolver.setExceptionMappings(properties);
        mappingExceptionResolver.setExceptionAttribute("ex");
        resolvers.add(mappingExceptionResolver);
    }

    //配置生成模板解析器
    @Bean
    public ITemplateResolver templateResolver() {
        WebApplicationContext webApplicationContext = ContextLoader.getCurrentWebApplicationContext();
        // ServletContextTemplateResolver需要一个ServletContext作为构造参数，可通过WebApplicationContext 的方法获得
        ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver(
                webApplicationContext.getServletContext());
        templateResolver.setPrefix("/WEB-INF/templates/");
        templateResolver.setSuffix(".html");
        templateResolver.setCharacterEncoding("UTF-8");
        templateResolver.setTemplateMode(TemplateMode.HTML);
        return templateResolver;
    }

    //生成模板引擎并为模板引擎注入模板解析器
    @Bean
    public SpringTemplateEngine templateEngine(ITemplateResolver templateResolver) {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(templateResolver);
        return templateEngine;
    }

    //生成视图解析器并未解析器注入模板引擎
    @Bean
    public ViewResolver viewResolver(SpringTemplateEngine templateEngine) {
        ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
        viewResolver.setCharacterEncoding("UTF-8");
        viewResolver.setTemplateEngine(templateEngine);
        return viewResolver;
    }

    }
```

### SpringMVC常用的组件

1、DispatcherServlet：**前端控制器**，由SpringMVC框架提供。

作用：统一处理请求和响应，调用相关组件（下面的组件）处理请求，最后处理服务器响应给客户端。

2、HandlerMapping：**处理器映射器**，根据url找到对应的控制器方法，之后将控制器方法、拦截器方法、Handler对象，拦截器索引等信息封装到HandlerExecutionChain执行链对象中。

3、Handler，**处理器**，DispatcherServlet根据处理器选择合适的HandlerAdapter，并为其创造对象。

4、HandlerAdapter，**处理器适配器**，处理器适配器对象用来调用控制器方法执行，返回一个ModelAndView。

5、ViewResolver：**视图解析器**，有ThymeleafView、InternalResourceView（ThymeleafView解析不了的视图名称就有内部解析器解析），视图解析器处理视图的拼接，根据ModelAndView渲染视图。

### DispatcherServlet 的工作原理

#### 1、DispatcherServlet 的初始化过程

#### 2、DispatcherServlet调用组件处理请求

##### a>processRequest()

FrameworkServlet重写HttpServlet中的service()和doXxx()，这些方法中调用了processRequest(request, response)

所在类：org.springframework.web.servlet.FrameworkServlet

```java
protected final void processRequest(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {

    long startTime = System.currentTimeMillis();
    Throwable failureCause = null;

    LocaleContext previousLocaleContext = LocaleContextHolder.getLocaleContext();
    LocaleContext localeContext = buildLocaleContext(request);

    RequestAttributes previousAttributes = RequestContextHolder.getRequestAttributes();
    ServletRequestAttributes requestAttributes = buildRequestAttributes(request, response, previousAttributes);

    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
    asyncManager.registerCallableInterceptor(FrameworkServlet.class.getName(), new RequestBindingInterceptor());

    initContextHolders(request, localeContext, requestAttributes);

    try {
		// 执行服务，doService()是一个抽象方法，在DispatcherServlet中进行了重写
        doService(request, response);
    }
    catch (ServletException | IOException ex) {
        failureCause = ex;
        throw ex;
    }
    catch (Throwable ex) {
        failureCause = ex;
        throw new NestedServletException("Request processing failed", ex);
    }

    finally {
        resetContextHolders(request, previousLocaleContext, previousAttributes);
        if (requestAttributes != null) {
            requestAttributes.requestCompleted();
        }
        logResult(request, response, failureCause, asyncManager);
        publishRequestHandledEvent(request, response, startTime, failureCause);
    }
}
```

##### b>doService()

所在类：org.springframework.web.servlet.DispatcherServlet

```java
@Override
protected void doService(HttpServletRequest request, HttpServletResponse response) throws Exception {
    logRequest(request);

    // Keep a snapshot of the request attributes in case of an include,
    // to be able to restore the original attributes after the include.
    Map<String, Object> attributesSnapshot = null;
    if (WebUtils.isIncludeRequest(request)) {
        attributesSnapshot = new HashMap<>();
        Enumeration<?> attrNames = request.getAttributeNames();
        while (attrNames.hasMoreElements()) {
            String attrName = (String) attrNames.nextElement();
            if (this.cleanupAfterInclude || attrName.startsWith(DEFAULT_STRATEGIES_PREFIX)) {
                attributesSnapshot.put(attrName, request.getAttribute(attrName));
            }
        }
    }

    // Make framework objects available to handlers and view objects.
    request.setAttribute(WEB_APPLICATION_CONTEXT_ATTRIBUTE, getWebApplicationContext());
    request.setAttribute(LOCALE_RESOLVER_ATTRIBUTE, this.localeResolver);
    request.setAttribute(THEME_RESOLVER_ATTRIBUTE, this.themeResolver);
    request.setAttribute(THEME_SOURCE_ATTRIBUTE, getThemeSource());

    if (this.flashMapManager != null) {
        FlashMap inputFlashMap = this.flashMapManager.retrieveAndUpdate(request, response);
        if (inputFlashMap != null) {
            request.setAttribute(INPUT_FLASH_MAP_ATTRIBUTE, Collections.unmodifiableMap(inputFlashMap));
        }
        request.setAttribute(OUTPUT_FLASH_MAP_ATTRIBUTE, new FlashMap());
        request.setAttribute(FLASH_MAP_MANAGER_ATTRIBUTE, this.flashMapManager);
    }

    RequestPath requestPath = null;
    if (this.parseRequestPath && !ServletRequestPathUtils.hasParsedRequestPath(request)) {
        requestPath = ServletRequestPathUtils.parseAndCache(request);
    }

    try {
        // 处理请求和响应
        doDispatch(request, response);
    }
    finally {
        if (!WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
            // Restore the original attribute snapshot, in case of an include.
            if (attributesSnapshot != null) {
                restoreAttributesAfterInclude(request, attributesSnapshot);
            }
        }
        if (requestPath != null) {
            ServletRequestPathUtils.clearParsedRequestPath(request);
        }
    }
}
```

##### c>doDispatch()

所在类：org.springframework.web.servlet.DispatcherServlet

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    boolean multipartRequestParsed = false;

    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

    try {
        ModelAndView mv = null;
        Exception dispatchException = null;

        try {
            processedRequest = checkMultipart(request);
            multipartRequestParsed = (processedRequest != request);

            // Determine handler for the current request.
            /*
            	mappedHandler：调用链
                包含handler、interceptorList、interceptorIndex
            	handler：浏览器发送的请求所匹配的控制器方法
            	interceptorList：处理控制器方法的所有拦截器集合
            	interceptorIndex：拦截器索引，控制拦截器afterCompletion()的执行
            */
            mappedHandler = getHandler(processedRequest);
            if (mappedHandler == null) {
                noHandlerFound(processedRequest, response);
                return;
            }

            // Determine handler adapter for the current request.
           	// 通过控制器方法创建相应的处理器适配器，调用所对应的控制器方法
            HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

            // Process last-modified header, if supported by the handler.
            String method = request.getMethod();
            boolean isGet = "GET".equals(method);
            if (isGet || "HEAD".equals(method)) {
                long lastModified = ha.getLastModified(request, mappedHandler.getHandler());
                if (new ServletWebRequest(request, response).checkNotModified(lastModified) && isGet) {
                    return;
                }
            }
			
            // 调用拦截器的preHandle()
            if (!mappedHandler.applyPreHandle(processedRequest, response)) {
                return;
            }

            // Actually invoke the handler.
            // 由处理器适配器调用具体的控制器方法，最终获得ModelAndView对象
            mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

            if (asyncManager.isConcurrentHandlingStarted()) {
                return;
            }

            applyDefaultViewName(processedRequest, mv);
            // 调用拦截器的postHandle()
            mappedHandler.applyPostHandle(processedRequest, response, mv);
        }
        catch (Exception ex) {
            dispatchException = ex;
        }
        catch (Throwable err) {
            // As of 4.3, we're processing Errors thrown from handler methods as well,
            // making them available for @ExceptionHandler methods and other scenarios.
            dispatchException = new NestedServletException("Handler dispatch failed", err);
        }
        // 后续处理：处理模型数据和渲染视图
        processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
    }
    catch (Exception ex) {
        triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
    }
    catch (Throwable err) {
        triggerAfterCompletion(processedRequest, response, mappedHandler,
                               new NestedServletException("Handler processing failed", err));
    }
    finally {
        if (asyncManager.isConcurrentHandlingStarted()) {
            // Instead of postHandle and afterCompletion
            if (mappedHandler != null) {
                mappedHandler.applyAfterConcurrentHandlingStarted(processedRequest, response);
            }
        }
        else {
            // Clean up any resources used by a multipart request.
            if (multipartRequestParsed) {
                cleanupMultipart(processedRequest);
            }
        }
    }
}
```

##### d>processDispatchResult()

```java
private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,
                                   @Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv,
                                   @Nullable Exception exception) throws Exception {

    boolean errorView = false;

    if (exception != null) {
        if (exception instanceof ModelAndViewDefiningException) {
            logger.debug("ModelAndViewDefiningException encountered", exception);
            mv = ((ModelAndViewDefiningException) exception).getModelAndView();
        }
        else {
            Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
            mv = processHandlerException(request, response, handler, exception);
            errorView = (mv != null);
        }
    }

    // Did the handler return a view to render?
    if (mv != null && !mv.wasCleared()) {
        // 处理模型数据和渲染视图
        render(mv, request, response);
        if (errorView) {
            WebUtils.clearErrorRequestAttributes(request);
        }
    }
    else {
        if (logger.isTraceEnabled()) {
            logger.trace("No view rendering, null ModelAndView returned.");
        }
    }

    if (WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
        // Concurrent handling started during a forward
        return;
    }

    if (mappedHandler != null) {
        // Exception (if any) is already handled..
        // 调用拦截器的afterCompletion()
        mappedHandler.triggerAfterCompletion(request, response, null);
    }
}
```

### SpingMVC的执行流程

1、发送请求被DispatcherServlet捕获，并解析url，找到对应的控制器方法，获取资源。

2、如果找不到对应的控制器方法，就会找到配置文件，看看是否配置了defaultServlet（默认的Serlvet）

3、如果没有配置默认的Serlvet，但是DispatcherServlet又找不到对应的控制器方法，就会在客户端报404错误，并说明DispatcherServlet找不到资源

4、如果配置了defaultServlet，就会使用defaultServlet访问有没有请求所需要的静态资源，如果还找不到，就会报404，并说明defaultServlet找不到资源。