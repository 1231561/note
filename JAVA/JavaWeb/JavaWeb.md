# JavaWeb

## （一）什么是JavaWeb

JavaWeb是所有通过Java语言编写可以通过浏览器访问的程序的总称。

JavaWeb是基于请求和响应来开发的。

JavaWeb的三大组件：Servlet程序、Filter过滤器、Listener监听器

## （二）什么是请求

请求(Request)是指客户端给服务器发生数据。

## （三）什么是响应

响应(Response)是指服务器给客户端回传数据

![image-20220210174806403](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220210174806403.png)

## Web资源的分类

web资源按照实现的技术和呈现不同的效果的不同，又分为静态资源和动态资源两种

静态资源：html、css、js、txt、mp4视频、jpg图片

动态资源：jsp页面、Servlet程序。

## Tomcat

Tomcat是由Aphache组织提供的一种Web服务器，提供对jsp和Servlet的支持。它是一种轻量级的javaWeb容器（服务器），也是当前应用最广的javaweb服务器（免费）。

#### Tomcat目录文件

bin			专门用来存放Tomcat服务器的可执行文件

conf			专门存放Tomcat服务器的配置文件

lib			专门存放Tomcat服务器的jar包

logs			专门存放Tomcat服务器运行时输出的日记信息

temp		专门存放Tomcat运行时产生的临时文件

webapps		专门存放部署的Web工程，web是Tomcat工作时的目录，用来存放Tom运行时jsp翻译为Servlet的源码

## 如何启动Tomcat服务器

1、找到bin目录下的startu.bat文件，打开

2、测试Tomcat是否成功打开

打开浏览器，输入测试地址

1、http://localhost:8080

2、http://127.0.0.1.8080

3、http://真实 ip:8080

## Tomcat的停止

点服务窗口的×

ctrl C

点击bin目录的shutdown.bat

## 将web工程部署到Tomcat中

第一种方法：

在Tomcat的webapps目录下创建工程目录，然后将工程文件拷贝进来。

8080代表webapps目录，在8080后加上项目文件地址即可访问项目文件。

第二种方式：

在Tomcat的配置文件的Catalina的localhost添加xml文件,内容有path(目录别名),docBase(项目地址)。就可以访问项目文件。

## 通过协议请求服务器访问页面

直接将项目拖到浏览器，地址栏显示file协议，该协议是解析本身硬盘目录的项目文件

而通过地址栏输入格式如下：http：//ip:port/工程名/资源名，即可通过请求服务器访问项目。访问步骤如下图：

![image-20220211152406151](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220211152406151.png)

## 默认访问的工程和默认访问的资源

当我们输入http://ip:port/		后面没有工程名(工程目录)时，默认访问ROOT工程

当我们输入http://ip:port/工程名/	后面没有资源名，默认访问index.html页面

## 动态web工程目录的介绍

![image-20220211160419508](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220211160419508.png)

## Tomcat在idea中启动部署web

![image-20220212132114723](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212132114723.png)



![image-20220212133150710](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212133150710.png)

## Servlet

#### 什么是Servlet

1、servlet是javaEE的规范之一。规范就是接口。

2.sevlet是javaWeb的三大组件之一。三大组件分别是Servlet程序、Fliter过滤器、Listener监听器。

3、Servlet就是运行在服务器上的一个java小程序，**它可以接收客户端发送过来的请求，并响应数据给客户端。**

4、所以Servlet就是一个服务器连接器，**是客户端和服务器的连接的桥梁**

## 第一个Servlet程序

1、首先重写HelloServlet内的service方法，使其输入"启动service",当客户访问到HelloServlet服务时就会调用service方法。

2、配置web.xml文件，如下

```xml
<!--servlet标签给Tomcat配置Servlet程序-->
    <servlet>
        <!--servlet-name标签给Servlet起一个别名(类名)-->
        <servlet-name>HelloServlet</servlet-name>
        <!--servlet-class是Servlet程序的全类名-->
        <servlet-class>com.example.hhh.HelloServlet</servlet-class>
    </servlet>
<!--servlet-mapping标签给servlet程序配置访问地址-->
    <servlet-mapping>
        <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
        <servlet-name>HelloServlet</servlet-name>
        <!--url-pattern配置标签访问地址
        斜杠/在服务解析表示地址:http://ip:port/工程路径
        所有/hello表示地址：http://ip:port/工程路径/hello-->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
```

#### 常见错误

1、servlet标签下的servlet-name和servlet-mapping标签下的servlet-name不同。

![image-20220212161232274](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212161232274.png)

2、url-pattern访问地址前面没有斜杠/。

![image-20220212161134658](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212161134658.png)

3、servlet-class全类名书写错误，导致找不到HelloServlet。

#### Servlet的URL地址如何定位到Servlet程序去访问

![image-20220212162507108](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212162507108.png)



![image-20220212162257233](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212162257233.png)

## Servlet的生命周期

1、执行Servlet构造器

2、执行init初始化方法

前面这两种方法只执行一次

3、执行service方法

这一步，每次访问都会调用。

4、执行destroy方法

这一步在web工程停止后调用。

![image-20220212163914580](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212163914580.png)

![image-20220212163929545](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212163929545.png)

#### 提交表单到Servlet服务

```html
<form action="http://localhost:8080/hhh_war_exploded/hello" method="get">
    <input type="submit" value="提交表单" >
</form>
```

```java
public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
    super.service(req, res);
    HttpServletRequest hq=(HttpServletRequest) req;//向下转型，用子对象调用getMethodff
    System.out.println(((HttpServletRequest) req).getMethod());
    System.out.println("启动service");
}
```

提交之后：

![image-20220212171519000](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212171519000.png![image-20220212171616436](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212171616436.png)

## 通过继承HttpServlet实现Servlet程序

```java
public class HelloServlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("HelloServlet2的doGet方法");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("HelloServlet2的doPost方法");
    }
}
```

#### 直接新建一个Servlet文件

![image-20220212174745555](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212174745555.png)

更快捷的新建了一个继承了HttpServlet的类，xml文件也帮写有name和class，只需填写地址。

## Servlet类的继承体系

![image-20220212182151363](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212182151363.png)

GenericServlet类：

抽象出service方法，谁继承他就要实现service方法。

```java
public abstract void service(ServletRequest req, ServletResponse res)
        throws ServletException, IOException;
```

HttpServletRequest类：

**主要是实现了service方法，if—else实现了请求的分发处理。**

**如果客户端发送get请求，则service方法就会匹配调用doGet方法，如果客户端发送post请求，则service方法就会匹配调用doPost方法**

并抛出Get和Post异常

```java
protected void service(HttpServletRequest req, HttpServletResponse resp)
    throws ServletException, IOException {

    String method = req.getMethod();

    if (method.equals(METHOD_GET)) {
        long lastModified = getLastModified(req);
        if (lastModified == -1) {
            // servlet doesn't support if-modified-since, no reason
            // to go through further expensive logic
            doGet(req, resp);
        } else {
            long ifModifiedSince;
            try {
                ifModifiedSince = req.getDateHeader(HEADER_IFMODSINCE);
            } catch (IllegalArgumentException iae) {
                // Invalid date header - proceed as if none was set
                ifModifiedSince = -1;
            }
            if (ifModifiedSince < (lastModified / 1000 * 1000)) {
                // If the servlet mod time is later, call doGet()
                // Round down to the nearest second for a proper compare
                // A ifModifiedSince of -1 will always be less
                maybeSetLastModified(resp, lastModified);
                doGet(req, resp);
            } else {
                resp.setStatus(HttpServletResponse.SC_NOT_MODIFIED);
            }
        }

    } else if (method.equals(METHOD_HEAD)) {
        long lastModified = getLastModified(req);
        maybeSetLastModified(resp, lastModified);
        doHead(req, resp);

    } else if (method.equals(METHOD_POST)) {
        doPost(req, resp);

    } else if (method.equals(METHOD_PUT)) {
        doPut(req, resp);

    } else if (method.equals(METHOD_DELETE)) {
        doDelete(req, resp);

    } else if (method.equals(METHOD_OPTIONS)) {
        doOptions(req,resp);

    } else if (method.equals(METHOD_TRACE)) {
        doTrace(req,resp);

    } else {
        //
        // Note that this means NO servlet supports whatever
        // method was requested, anywhere on this server.
        //

        String errMsg = lStrings.getString("http.method_not_implemented");
        Object[] errArgs = new Object[1];
        errArgs[0] = method;
        errMsg = MessageFormat.format(errMsg, errArgs);

        resp.sendError(HttpServletResponse.SC_NOT_IMPLEMENTED, errMsg);
    }
}
```

自定义的的Servlet类：

重写doGet和doPost方法。

## ServletConfig类

Servlet程序的配置信息类

#### 作用:

1、可以获取Servlet程序的别名Servlet-name的值

2、获取初始化参数init-param

3、获取ServletContext对象

用法一：在init方法里就用到了ServletConfig类的参数

```java
@Override
public void init(ServletConfig config) throws ServletException {
    super.init(config);
    System.out.println("2 启动init初始化方法");
    System.out.println(config.getServletName());
    System.out.println(config.getInitParameter("数学"));
    System.out.println(config.getInitParameter("语文"));
}
```

![image-20220212185125085](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220212185125085.png)

用法二：

创建 ServletConfig对象

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    System.out.println("HelloServlet2的doGet方法");
    ServletConfig sc=getServletConfig();//getServletConfig()是GenericServlet类的方法，这个方法return config。
    System.out.println(sc);
    System.out.println(getServletName());
    System.out.println(sc.getInitParameter("语文2"));
    System.out.println(sc.getInitParameter("数学2"));
}
```

```java
@Override
public void init(ServletConfig config) throws ServletException {
    super.init(config);//必须得有，不然GenericServlet类的init方法不会保存config值，getServletConfig()，就无法return config，就出现空指针异常。
    System.out.println("重写init方法");
}
```

GenericServlet类的init方法

```java
public void init(ServletConfig config) throws ServletException {
    this.config = config;//保存config值
    this.init();
}
```

## ServletContext

1、ServletContext是一个接口，它表示Servlet上下文对象

2、一个web工程，只有一个ServeletContext对象实例。

3、ServeletContext对象是一个域对象。

4、ServletContext是在web工程部署的时候创建。在web工程停止的时候销毁。

#### 什么是域对象

域对象，就是像Map一样存取数据的对象。

域指的是存取数据的操作范围，ServletContext对象的域是整个web工程。

​				存数据			取数据		删除数据			

Map		put()				get()			remove()

域对象	setAttribute	getAttribute	removeAttribute

​	context-param是上下文参数，它属于整个工程

```xml
<context-param>
    <param-name>username</param-name>
    <param-value>Tom</param-value>
</context-param>
<context-param>
    <param-name>password</param-name>
    <param-value>123456</param-value>
</context-param>
```

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
ServletContext con=getServletContext();
    System.out.println(con.getInitParameter("username"));
    System.out.println(con.getInitParameter("password"));
    //获取工程路径
    System.out.println(con.getContextPath());
    //获取工程在硬盘上的绝对路径
    System.out.println(con.getRealPath("/"));
}
```

![image-20220213161046625](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220213161046625.png)

## Servlet存取数据

```java
@Override
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    ServletContext con1=getServletContext();
    System.out.println("保存前，key1的值是:"+con1.getAttribute("key1"));//重新部署之后，key1保存的值就会被销毁，然后，这里就会输出null。
    con1.setAttribute("key1",1);
    System.out.println("key1的值是:"+con1.getAttribute("key1"));
}

```

因为一个web工程，只有一个ServeletContext对象实例，所以可以新建一个对象来访问原来对象保存的数据。

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
ServletContext con2=getServletContext();
    System.out.println("通过con2访问key1的值是:"+con2.getAttribute("key1"));
}
```

当con1保存数据后：con2也可以读取数据。

![image-20220213165305702](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220213165305702.png)

## Http协议

#### 什么是协议

协议是指双方或多方，或多方，相互约好，大家都要遵守的规则。

Http协议就是指客户端和服务器之间通信时，发送的数据，需要遵守的规则。

HTTP协议中的数据又叫报文。

## Http协议内容

### GET请求

![image-20220214192435141](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220214192435141.png)

Accept:告诉服务器，客户端可以接收的数据类型。

Accept-Lauguage:告诉服务器，客户端可以接收的语言类型

​	zh_CN			中文中国

​	en_US			英文美国

User-Agent：浏览器的信息

Accept-Encoding：告诉服务器，客户端可以接收的数据编码(压缩)格式

Host：表示请求的服务器ip和端口号。

Connection：告诉服务器请求连接然后处理

​			Keep-Alive		告诉服务器回传数据后不要马上关闭，保持一小段时间的连接

​			Closed				马上关闭

### Post请求

![image-20220213174106710](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220213174106710.png)

Accept:告诉服务器，客户端可以接收的数据类型。

Accept-Lauguage:告诉服务器，客户端可以接收的语言类型

Referer：表示请求发送时，浏览器地址栏的地址（从哪来）

User-Agent：表示浏览器的信息

Content-Type：表示发送的数据类型

​		application/x-www-form-urlencoded

​					表示提交的数据格式是:name=value&name=value,然后对其进行url编码

​					url编码是把非英文内容转化为：%xx%xx

​		multipart/form-data

​						表示以多段的形式提交数据给服务器(以流的形式发送)	

​				Content-Length：表示发送数据的长度

​				Cache-Control 表示如何控制缓存	no-cache不缓存

## 如何区分GET和POST请求

GET请求：

form标签 method=get

a标签

link标签引入css

Script标签引入js文件

img标签引入图片

iframe引入html页面

浏览器地址栏中输入地址后回车

POST请求

form标签 method=post					

## Servlet响应的HTTP协议格式

![image-20220213180856150](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220213180856150.png)

Servlet表示服务器的信息

Context-Type：表示响应体的数据类型

Content-Length：响应体的长度

Date：请求响应的时间

## 常见的响应码

200：	请求成功

302：	请求重定向

404：	表示服务器请求收到了，但是找不到请求的地址（请求地址错误）

500：	表示服务器已经收到请求了，但是服务器内部错误（java代码错误）

##	MIME类型

MIME是HTTP协议中的数据类型。

![image-20220213181920104](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220213181920104.png)

## HttpServletRequest

### 作用：

每次只要有请求进入Tomcat服务器，Tomcat服务器就会把请求过来的HTTP协议封装好的Request对象中。然后传递到service方法(doGet和doPost)中给我们使用。我们可以通过HTTPServletRequest对象，获取所有请求的信息。

## HttpServletRequest的常用方法

1、getRequestURI()			获取请求的资源路径

2、getRequestURL()				获取请求的统一资源定位符

3、getRemoteHost()				获取客户端的ip地址

4、getHeader()						获取请求头

5、getParameter()					获取请求的参数

6、getParameterValues()		获取请求的参数(多个值的时候使用)

7、getMethod()						获取请求的方式GET或POST

8、setAttribute(key,value)	设置域数据

9、getAttribute(key)				获取域数据

10、getRequestDispatcher()		获取请求转发对象

实例：

```java
public class RequestServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取请求的资源路径
        System.out.println(request.getRequestURI());///servlet_07/requestServlet
        //获取请求的绝对路径
        System.out.println(request.getRequestURL());//http://localhost:8080/servlet_07/requestServlet
        //获取请求客户端的ip地址
        System.out.println(request.getRemoteHost());//localhost、127.0.0.1访问结果都是127.0.0.1，用真实ip地址取访问时，得到客户端的ip地址（谁访问得到谁的ip）
        //获取请求头
        System.out.println(request.getHeader("User-Agent"));
    }


}
```

## 获取请求的参数值(获取表单数据)

#### GET请求：

表单使用GET请求提交

```java
public class ParameterServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("用户名:"+request.getParameter("username"));//获取form里用户名的name为username的数据
        System.out.println("密码:"+request.getParameter("password"));
        String[] arr=request.getParameterValues("hobbies");//name为hobbies的数据可能有多个值，要用数组存储。
        System.out.println("爱好:"+Arrays.asList(arr));//将数组转化为集合输出
    }
}
```

结果:

用户名:小明
密码:123456
爱好:[cpp, java, c]

#### POST请求

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //设置请求体的字符集为UTF-8，从而解决post请求的中文乱码问题
    req.setCharacterEncoding("UTF-8");
    System.out.println("用户名:"+req.getParameter("username"));
    System.out.println("密码:"+req.getParameter("password"));
    String[] arr=req.getParameterValues("hobbies");
    System.out.println("爱好:"+Arrays.asList(arr));
}
```

注意： req.setCharacterEncoding("UTF-8")要在所有的获取请求参数前面定义才有效，比如req.getParameter("username")如果在设置UTF-8之前，设置字符集为UTF-8将无效，就会出现下面的乱码：

![image-20220214205815372](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220214205815372.png)

## 请求转发

服务器收到请求之后，从一个资源跳转到另一个资源的操作叫转发。

![image-20220215161801891](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220215161801891.png)

#### Servlet1

```java
public class Servlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取用户数据
        System.out.println("获取资料："+request.getParameter("username"));
        //保存柜台1印的章
        request.setAttribute("key","柜台1的章");
        //获取Servlet2的地址，斜杠/表示：http://ip:port//工程名/，映射到IDEA代码的webapp目录
        RequestDispatcher s2= request.getRequestDispatcher("/servlet2");
        //转到Servlet2(柜台2)
        s2.forward(request,response);
    }
}
```

#### Servlet2

```java
public class Servlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取用户资料
        System.out.println("获取资料:"+request.getParameter("username"));
        //查找服务器1保存的数据，因为HttpServletRequest域是整个web工程，有唯一的对象。
        System.out.println("检查柜台1的章:"+request.getAttribute("key"));
        System.out.println("完成柜台2的办理");
    }
}
```

结果：

![image-20220215005553770](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220215005553770.png)

#### 请求转发的特点：

1、转发后浏览器的地址没变，始终是最初请求的Servlet程序的地址

2、只进行了一次请求

3、共享Request域中的数据(因为只有一次请求，Tomcat只创建了一个reqeust对象)

4、可以转发到WEB-INF目录下，因为是服务器访问，有访问权

5、只可以访问内部资源，不可以访问工程外的资源（比如百度）,因为服务器将请求转发的地址解析为工程目录下的资源地址，工程目录下没有百度这个资源，就访问不到了。

## base标签的作用

实例：

设置一个首页跳转到c页面，c页面也可以跳转到首页，然后设置一个类继承HttpServlet，重写doGet，转发到c页面。问：从doGet请求转发到c页面之后，还可以直接转到首页吗？

答案：不可以。

首页：

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
<a href="a/b/c.html">跳转到c页面</a><br>
<a href="http://localhost:8080/servlet_07/forwardC">ForwardC跳转到c页面</a>
</body>
</html>
```

c页面:

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>c页面</title>
</head>
<body>
<a href="../../index.html">跳转到首页</a>
</body>
</html>
```

ForwardC程序：

```java
public class ForwardC extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("经过了Forward程序");
        req.getRequestDispatcher("/a/b/c.html").forward(req,resp);
    }
}
```

因为转发到c页面时，地址仍是http://localhost:8080/servlet_07/forwardC不变。

但是跳转到首页参考的是相对地址，http://localhost:8080/servlet_07/forwardC这个地址参照相对地址:"../../index.html"会出问题

首页直接转到c页面后的地址为http://localhost:8080/servlet_07/a/b/c.html，参照相对地址就没错

**所以需要加上base标签标记c页面的绝对地址**

如下：

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>c页面</title>
    <base href="http://localhost:8080/servlet_07/a/b/c.html">//加上base标签
</head>
<body>
<a href="../../index.html">跳转到首页</a>
</body>
</html>
```

加上base标签后跳转到首页的地址相当于:<a href="localhost:8080/servlet_07/a/b/c.html/../../index.html">跳转到首页</a>,相当于把相对路径改为绝对路径，这样就不会错了。

#### 相对路径和绝对路径

相对路径：
. 		当前目录

..		上一级目录

资源名	当前目录/资源名



绝对路径：

http://ip:potr//工程路径//资源路径

## web中斜杠/的不同含义

在web在斜杠/是一种绝对路径

斜杠/被如果被浏览器解析，得到的地址是：http://ip:port/

```html
<a href="/">斜杠</a>
```

结果：

![image-20220215133659177](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220215133659177.png)

斜杠/被如果被服务器解析，得到的的地址是：http://ip:port/工程路径，idea映射到工程路径的web目录

1、<url-pattern>/servlet1</url-pattern>

2、context1.getRealPath("/")；

3、request.getRequestDispatcher("/")；



特殊情况：reponse.sendRediect("/")，把/发送给浏览器解析得到地址：http://ip:port/

## HttpServletResponse的作用

HttpServletResponse和HttpServletResquest一样。每次请求，Tomcat服务器都会创建一个Response对象传递给Servlet程序去用。HttpServletResquest表示请求过来的信息，HttpServletResponse表示所有响应的信息。我们可以通过Response对象设置返回给客户端的信息。

## 输出流

字节流		getoutputStream			常用于下载(传递二进制数据)

字符流		getWriter							常用于回传字符串(常用)

注意：两个方法只能用一个。

事例：

```java
public class ResponeIOServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        PrintWriter writer = response.getWriter();//设置回传字符串
        writer.write("hhh");
    }
```

## 解决响应的中文乱码

代码示例：

```java
public class ResponeIOServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
//        System.out.println(response.getCharacterEncoding());//服务器默认的字符格式：ISO-8859-1
        response.setCharacterEncoding("UTF-8");//将服务器的字符格式设置为UTF-8
        //浏览器默认的字体格式是GDK，需要把它也改为UTF-8,不然会出现中文乱码
                               //内容类型           //文本                 
        response.setHeader("Content-Type","text/html;charset=UTF-8");//将浏览器的字体格式也设置为UTF-8
        //方法二:
         response.setContentType("text/html;charset=UTF-8");//同时将服务器和浏览器的字体格式设置为UTF-8，这方法要放在获取流对象之前用才有效
        PrintWriter writer = response.getWriter();
        writer.write("哈哈哈");
    }
}
```

![image-20220215153912417](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220215153912417.png)

## 请求重定向

当客户端要访问的服务器中的某个接口被废弃了，由新的接口取代，则原来的接口会将自己的响应码设置为302，表示本接口已转移，并告诉客户端新的接口地址（将响应头地址转变为新的接口地址)，然后客户端会自动地第二次访问新的地址。

代码实例：

旧网站：

```java
public class ResponeServlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //客户端想要访问的资源
        System.out.println("我要访问的资源");
        //设置响应码
        //保存数据
        request.setAttribute("key","123");//测试能不能共享Request的资源
        response.setStatus(302);
        //设置响应头，换上新的地址
       response.setHeader("Location","http://localhost:8080/servlet_07/responeServlet2");
        //方法2，更简便
        response.sendRedirect("http://localhost:8080/servlet_07/responeServlet2");
    }

}
```

新网站：

```java
public class ResponeServlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");
        response.getWriter().write("访问到了新的网站");
        System.out.println(request.getAttribute("key"));
    }
}
```

结果：

![image-20220215165011022](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220215165011022.png)

![image-20220215165021176](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220215165021176.png)

接受到的请求：

![image-20220215165038196](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220215165038196.png)

#### 请求重定向特点

1、浏览器地址栏地址会发生变化（会跳转到新的地址）

2、客户端发出了两次请求

3、不能共享Request域中的资源（因为两次请求，Tomcat就创建了两个request对象，所以是两个不同的资源）

4、不能访问WEB-INF下的资源(因为两次请求都是客户端发起的(浏览器)，WEB-INF受保护，只能通过接受服务器的请求)

5、可以访问工程外的资源(因为是浏览器访问，当然可以)

## JavaEE的三层结构

![image-20220215171804338](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220215171804338.png)

### JSP（已过时）

JSP的本质就是Servlet。

JSP可以像HTML一样显示页面框架，并可以加上Java语句逻辑，JSP还可以实现Servlet的功能（请求、相应、输出等功能）。

#### EL表达式

作用：代替jsp表达式脚本在jsp页面的数据输出。

#### JSTL

是jsp的标准标签库

### Listener监听器

Listener监听器是JavaWeb的三大组件之一

Listener是JavaEE的规范（接口）

**监听器Listener的作用：**监听某种事务的变化，然后通过回调函数，反馈给客户(程序)去做一些相应的处理。

### ServletContextListenrt监听器的演示

ServletContextListenrt监听器可以监听ServletContext对象的创建和销毁。

ServletContext对象再web启动时创建，再web结束时销毁。

ServletContextListenrt监听器用下面两个方法监听ServletContext对象的创建和销毁。

```java
public class MyServletContextListenerImpl implements ServletContextListener {
    @Override
    //监听到ServletContext对象的创建
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("ServletContext被创建了");
    }

    @Override
    //监听到ServletContext对象的销毁
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("ServletContext被销毁了了");
    }
}
```

![image-20220302163935283](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220302163935283.png)

ServletContextListenrt监听器会在Spring框架中应用。

### 文件的上传和下载(重点)

上传条件：

1、有一个form标签，method=post请求

2、form标签的encTye属性值必须为multipart/form-date值

3、在form标签中使用input type=file添加上传文件

4、编写服务器接收代码，处理上传的数据

**下图为提交的数据类型：**

![image-20220303120811276](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220303120811276.png)

​	**multipart/form-date表示提交的数据，以段的形式进行发送，每个表单项为一段(form里每一个input为一个表单项，除了提交按钮)，然后将多段进行拼接，最后以二进制的形式发送给服务器。**

​	**boundary表示每一段表单项的分隔符，是系统算计产生的。**

相关代码：

Html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <form action="http://localhost:8080/UploadAndDownload/uploadServlet" method="post" enctype="multipart/form-data">
        用户:<input type="text" name="username"><br>
        头像:<input type="file" name="photo"><br>
        <input type="submit" value="上传">
    </form>
</body>
</html>
```

Servlet:

```java
package com.example.uploadanddownload.upload;

import javax.servlet.ServletException;
import javax.servlet.ServletInputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;

public class UploadServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //以流的形式接受二进制数据，并输出
        ServletInputStream inputStream = req.getInputStream();
        byte[] buff = new byte[1024000];
        int read = inputStream.read(buff);
        System.out.println(new String(buff,0,read));

    }
}
```

输出结果：

![image-20220303121723512](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220303121723512.png)

### 解析上传的文件

由于上传的数据我们看不懂，所以我们需要使用API来解析数据。

解析上传的文件需要的jar包：

![image-20220303131531927](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220303131531927.png)

解析上传文件常用的类及方法：
1、ServletFileUpload类：用于解析上传文件

2、FileItem类：表示ServletFileUpload类解析后生成的表单项（数据类型）

3、Boolean ServletFileUpload.isMultipartContent(HttpServletRequest req):判断上传的数据格式是否是多段的格式

4、List<FileItem> list = servletFileUpload.parseRequest(HttpServletRequest req)：将上传的数据解析为表单项存放到List集合中

5、boole FileItem.isFormField()：判断是否为普通表单项

6、String FileItem.getFieldName()：取表单项的name属性值

7、String FileItem.getFieldName()：取表单项的value值

8、void  FileItem.write(file)：将上传的文件保存的磁盘上。

相关代码：

Servlet：

```java
public class UploadServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //将请求的数据格式改为UTF-8，防止上传的中文文件名乱码。
        req.setCharacterEncoding("UTF-8");
        //判断上传的数据是否是多段数据，多段数据代表上传的是文件
       if(ServletFileUpload.isMultipartContent(req)){
           //创建 FileItemFactory工厂实现类
           FileItemFactory fileItemFactory = new DiskFileItemFactory();
           //创建用于解析上传数据的工具实现类 ServletFileUpload
           ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);
           //解析数据，将每一段表单数据,转化成表单项FileItem保存到List集合中
           try {
               List<FileItem> list = servletFileUpload.parseRequest(req);
               for (FileItem it :list){
                   //isFormField()方法判断是否为普通表单项
                   if(it.isFormField()){
                       System.out.println("表单项的name属性值"+it.getFieldName());
                       System.out.println("表单项的value属性值"+it.getString("UTF-8"));
                       //表单项是上传的文件
                   }else {
                       System.out.println("表单项的name属性值"+it.getFieldName());
                       System.out.println("上传的文件名"+it.getName());
                       //将上传的文件保存到指定路径，并取名
                       it.write(new File("D:\\"+it.getName()));
                   }
               }
           } catch (FileUploadException e) {
               e.printStackTrace();
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
    }
}
```

```jsp
<form action="http://localhost:8080/UploadAndDownload/uploadServlet" method="post" enctype="multipart/form-data">
    用户:<input type="text" name="username"><br>
    头像:<input type="file" name="photo"><br>
    <input type="submit" value="上传">
</form>
```

上传文件：

![image-20220303133331539](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220303133331539.png)





结果：

![image-20220303133315037](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220303133315037.png)

### 文件下载

大致步骤：

1、获取要下载的文件名

2、通过ServletContext对象读取要下载的文件内容以及获取下载的文件的类型

3、返回给客户端（浏览器）预下载的数据的类型

4、将客户端收到的数据通过设置响应头来进行下载操作

5、调用URLEncoder类方法将中文文件名转成16进制形式

6、用输入流读取文件，在赋值给输出流

代码实例：

```java
public class DownloadServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取要下载的文件名
        String downFileName="荷花.jpg";
        //通过ServletContext对象读取要下载的文件内容以及获取下载的文件的类型
        ServletContext servletContext = getServletContext();
        //获取文件的类型
        String mimeType = servletContext.getMimeType("/file/" + downFileName);
        //返回给客户端（浏览器）预下载的数据的类型
        resp.setContentType(mimeType);
        //url编码是将汉字转换为%xx%xx的格式
        //将客户端收到的数据通过设置响应头来进行下载操作，Content-Disposition表示接受的数据处理，attachment表示设置为附件，filename为下载的文件名
        resp.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode("荷花.jpg","UTF-8"));
        //以输入流的形式获取ServletContext对象读取的文件内容
        InputStream resourceAsStream = servletContext.getResourceAsStream("/file/" + downFileName);
        //获取响应的输入流
        ServletOutputStream outputStream = resp.getOutputStream();
        //通过调用jar包的IOUtils类的copy方法完成读取输入流的数据，复制给输出流。
        IOUtils.copy(resourceAsStream,outputStream);
    }
}
```

结果：

![image-20220303194729488](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220303194729488.png)

这个因该是跳转时是post方法，但是子类中没有post，就像上找post，然后父类有post，调用父类post方法

###  BeanUtils工具类的使用（书城项目book）

**BeanUtils.populate(Object,Map)方法**：

该方法可以将参数注入给某个对象，相当于new Object(参数1，参数2.....)

因为对象的属性值是成对出现的，所以用Map来给对应属性赋值，key表示属性名，value表示值。

代码实例：

```java
protected void regist(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //1、获取请求参数

    String code=req.getParameter("code");

	//调用.copyParamToBean方法，将请求参数注入对象
    //req.getParameterMap()方法是将参数封装到Map集合里，然后将值注入User对象中。
    User user=WebUtils.copyParamToBean(req.getParameterMap(),new User());
    //2、检查验证码是否正确
    //验证码正确
    if("abcde".equalsIgnoreCase(code)) {
        UserService us = new UserServiceImpl();
        //3、检查用户名是否可用
        //可用
        if (!us.existsUsername(user.getUsername())){
            //调用Service保存到数据库
            us.registerUser(user);
            //跳转到注册成功页面
            req.getRequestDispatcher("/pages/user/regist_success.jsp").forward(req,resp);
        }else {
            //不可用
            //跳回注册页面
            System.out.println("用户名"+user.getUsername()+"已存在！！！");
            req.setAttribute("msg","用户名已存在");
            req.setAttribute("username",user.getUsername());
            req.setAttribute("email",user.getEmail());
            req.getRequestDispatcher("/pages/user/regist.jsp").forward(req,resp);
        }

    }else {
        //验证码不正确
        //跳回注册页面
        req.setAttribute("msg","验证码错误错误");
        req.setAttribute("username",user.getUsername());
        req.setAttribute("email",user.getEmail());
        System.out.println("验证码"+code+"不正确");
        req.getRequestDispatcher("/pages/user/regist.jsp").forward(req,resp);
    }
}
```

注入实现方法：

```java
public class WebUtils {
    public static <T> T  copyParamToBean(Map map,T bean){
        System.out.println("注入之前："+bean);
        try {//根据req.getParameterMap()查找User()对应的属性的set方法，属性没有set方法就不能注入
            BeanUtils.populate(bean,map);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        System.out.println("注入之后："+bean);
        return bean;
    }
}
```

## MVC

MVC全称：Model模型 、View视图、Controller控制器

MVC的作用是降低耦合，使代码分层更明确

![image-20220304165229627](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220304165229627.png)

### 书城项目（增删查改）

### 书城项目（分页）

![image-20220305191701608](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220305191701608.png)

总体思路：

int begin = (Math.max((pageNo - 1), 0)) * pageSize; 

### Cookie

1、Cookie是服务器通知客户端保存键值对的一种技术

2、客户端有了Cookie后，每次请求都发给服务器

3、每个Cookie的大小不能超过4KB

4、Cookie只有一个构造器:Cookie(String name,String value)，需要传入键值对

#### Cookie的创建

![image-20220307193120164](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220307193120164.png)

代码：

```java
public class CookieServlet extends BaseServlet{
    //创建cookie
    protected void createCookie(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        Cookie cookie = new Cookie("key1","value1");
        //这一步必须要有
        resp.addCookie(cookie);
        resp.getWriter().write("cookie已创建");
    }
}
```

 resp.addCookie(cookie)：这一步服务器给客户端发送响应头，然后客户端就会查找响应头里的set-cookie有没有相同的cookie的名字,如果没有，就创建cookie然后把服务器给的键值对赋给cookie，如果有，就修改cookie的键值对。

可以服务器new多个cookie对象，让客户端保存多个键值对。

#### Cookie的获取

服务器怎么向客户端获取Cookie呢?

![image-20220307215002855](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220307215002855.png)

步骤1：根据cookie的名字找到cookie

将找cookie的方法封装到CookieUtils类里：

```java
public class CookieUtils {
    public static Cookie findCookie(String name,Cookie[] cookies){
        if (name==null || cookies==null||cookies.length==0 ){
            return null;
        }
        //根据cookie的名字查找cookie
        for (Cookie cookie:cookies){
            if (name.equals(cookie.getName())){
                return cookie;
            }
        }
        return null;
    }
}
```

步骤2：将找到的cookie响应给客户端:

```java
protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    Cookie wantCookie = CookieUtils.findCookie("key2",req.getCookies());
    if (wantCookie!=null){
    resp.getWriter().write(wantCookie.getName());
    }
}
```

#### Cookie的修改

```java
  protected void updateCookie(HttpServletRequest req, HttpServletResponse resp) throws IOException {
//        //方案一：
//        Cookie cookie = new Cookie("key1","newValue");
//        resp.addCookie(cookie);
//        resp.getWriter().write("已修改Cookie");
        //方案二：
        Cookie cookie1 = CookieUtils.findCookie("key2",req.getCookies());
      	if（cookie1!=null）{
           cookie1.setValue("newValue2");
		}
        resp.addCookie(cookie1);
        resp.getWriter().write("已修改Cookie");
    }
```

结果：

![image-20220307224518081](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220307224518081.png)

####	Cookie生命控制

 cookie.setMaxAge()方法

参数为负数：关闭浏览器Cookie消失（默认为-1）

参数为正数：Cookie的生存期为参数的秒

参数为0：Cookie立即消失

```java
//关闭就删除的Cookie
protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    Cookie cookie = new Cookie("defaultLife","defaultLife");
    cookie.setMaxAge(-1);
    resp.addCookie(cookie);
}
//马上删除指定的Cookie
protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    Cookie cookie = CookieUtils.findCookie("key1", req.getCookies());
    if(cookie!=null){
        cookie.setMaxAge(0);
        resp.addCookie(cookie);
        resp.getWriter().write("立即关闭Cookie");
    }
}
//将name为key2的Cookie生存时间设为1小时
protected void life3600(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    Cookie cookie = CookieUtils.findCookie("key2", req.getCookies());
    if(cookie!=null){
        cookie.setMaxAge(60*60);
        resp.addCookie(cookie);
        resp.getWriter().write("存活一个小时的Cookie");
    }
}
```

#### Cookie的有效路径Path

Cookie的有效路径的设置是为了筛选出满足设置地址的请求地址的Cookie，满足要求的Cookie可以被发送到服务器。

例如：浏览器有下面两个Cookie

CookieA	setPath=/工程路径

CookieB	setPath=/工程路径/abc



然后分别有两个请求地址发送请求：
http://ip:port/工程路径/a.html

则CookieA被发送给服务器，Cookie不被发送

http://ip:port/工程路径/abc/a.html

则两个Cookie都被发送

代码实例：

```java
protected void testPath(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    Cookie cookie = new Cookie("Path1","Path1");
    //指定工程名和指定资源
    cookie.setPath(req.getContextPath()+"/abc");
    resp.getWriter().write("创建了一个带有指定工程路径的Cookie");
    resp.addCookie(cookie);
}
```

结果：

访问地址

![image-20220307235403745](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220307235403745.png)

发送的Cookie：

![image-20220307235415329](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220307235415329.png)

**用Cookie实现第一次登录成功后用户名保存（书城项目）**：

服务器：

```java
public class LoginServlet extends HelloServlet {
    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        String username=request.getParameter("username");
        String password=request.getParameter("password");
        if ("admin".equals(username)&&"123456".equals(password)){
            //登录成功
            System.out.println("登录成功");
            //保存用户名，保存七天
            Cookie cookie = new Cookie("username","username");
            cookie.setMaxAge(60*60*24*7);
            response.addCookie(cookie);
        }else {
            System.out.println("登录失败");
        }
    }
}
```

jsp页面：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="http://localhost:8080/cookie_session/loginServlet" method="get">
        用户名:<input type="text" name="username" value="${cookie.username.value}"><br>
        密码:<input type="password" name="password" ><br>
        <input type="submit" value="登录">
    </form>
</body>
</html>
```

### Session（会话）

Session是一个接口，用来维护客户端和服务器之间关联的一种技术。

Session会话在服务器端，用来保存用户登陆后的信息。

#### 创建和获取会话

代码：

创建和获取会话都是使用req.getSession()方法

session.isNew()判断会话是否是刚创建的是返回true，反之返回false

session.getId()方法返回会话id

```java
protected void createOrGetSession(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //创建会话
    HttpSession session = req.getSession();
    //判断会话是否是刚创建的
    boolean aNew = session.isNew();
    String id = session.getId();
    resp.getWriter().write("是否新创建？"+aNew+"  id:"+id);
}
```

结果：

刚调用createOrGetSession方法是是创建session会话，所以session.isNew()返回true，后面再调用就是获取session会话，所以session.isNew()返回false。

![image-20220308005945135](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220308005945135.png)

### Session域对象的存取

```java
//将数据存入Session
protected void setAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    req.getSession().setAttribute("key1","value1");
    resp.getWriter().write("已向Session保存数据");
}
//将数据从Session中取出
protected void getAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    Object key1 = req.getSession().getAttribute("key1");
    resp.getWriter().write("从Session接收到的数据： "+key1);
}
```

### 会话的超时设置

getMaxInactiveInterval()方法：获取会话超时时间

setMaxInactiveInterval()方法：设置会话超时时间，参数为正数时为超时的秒数，负数为永远不超时(不推荐)

invalidate()方法，设置会话现在超时

代码：

```java
//返回会话默认超时时间
protected void iniOverTime(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    int maxInactiveInterval = req.getSession().getMaxInactiveInterval();
    resp.getWriter().write("默认超时时间："+maxInactiveInterval);

}
//会话三秒超时
protected void overTime3(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    HttpSession session = req.getSession();
     session.setMaxInactiveInterval(3);
     resp.getWriter().write("三秒后超时"+req.getSession().getMaxInactiveInterval());

}
//马上超时
protected void overTimeNow(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    HttpSession session = req.getSession();
    session.invalidate();
    resp.getWriter().write("马上超时");
}
```

会话默认超时时间时由服务器（Tomcat）的web.xml的配置设置的默认为30分钟，会话默认超时时间作用在整个服务器下的web工程。

如果在一个web工程的web.xml的配置设置的默认为其他分钟，则会话默认超时时间作用在当前web工程。

超时的概念：

两次请求的间隔时间。

当在超时的时间内请求，超时计时器会重置

![image-20220308014709009](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220308014709009.png)

### Session和浏览器之间的内部关联

Session其实是基于Cookie技术实现的。

Session和Cookie的关联

cookie是保存在客户端(浏览器)，session保存在服务器。session的id对应浏览器的cookie的值

**1、当浏览器没有Cookie时，向服务器发送请求：req.getSession()，服务器会新建一个会话Session1**

**2、然后服务器就会以响应的方式将JSESSIONID放在响应头的set-Cookie督促浏览器创建Cookie，然后浏览器创建一个name为JSESSIONID，value为JSESSIONID的值(如下图)的Cookie。**

请求头：

![image-20220308132032628](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220308132032628.png)

浏览器创建的Cookie：

![image-20220308132208121](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220308132208121.png)

**3、之后浏览器再发起req.getSession()请求，就将Cookie发送给服务器，服务器就根据Cookie的name和value查找到之前创建的Session1，然后把Session的信息发送给浏览器。**



相关图解：

![image-20220607154813764](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220607154813764.png)

![image-20220308131134416](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220308131134416.png)

注意：当删除浏览器Cookie后，即使服务器的对应Session还在(没有超时)，浏览器发送req.getSession()请求，服务器也不能根据发送过来的Cookie信息找到对应的Session。

### Filter过滤器

是一个接口。

JavaWeb的三大组件之一。

作用：拦截请求，过滤响应。

示例：

admin下的两个页面

![image-20220310200550200](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220310200550200.png)



AdminFilter类（继承Filter过滤器），实现对网页请求的筛选：

```java
public class AdminFilter implements Filter{
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    //重写 doFilter方法，实现拦截操作。
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        //向下转型，调用子类的httpServletRequest的方法getSession()创建会话
        HttpServletRequest httpServletRequest= (HttpServletRequest) request;
        HttpSession session = httpServletRequest.getSession();
        //通过会话获取，登录成功时保存的用户
        Object user = session.getAttribute("user");
        if (user==null){
            //如果为空，将重新跳转登录页面
           request.getRequestDispatcher("/login.jsp").forward(request,response);
            return;
        }else {
            //如果不为空，filter将允许请求访问filter管理的所有页面
            chain.doFilter(request,response);
        }
    }

    @Override
    public void destroy() {

    }
}
```

xml配置文件：

```java
//过滤器的配置文件
<filter>//过滤器文件名和完整地址
    <filter-name>AdminFilter</filter-name>
    <filter-class>com.filter.AdminFilter</filter-class>
</filter>
<filter-mapping>//选择哪个过滤器
    <filter-name>AdminFilter</filter-name>
    <url-pattern>/admin/*</url-pattern>//过滤器的作用范围admin下的所有文件
</filter-mapping>
```

### Filter的几个方法的调用顺序

1、构造器方法

2、init初始化方法

​	第1、第2步，在web工程启动的时候执行。

3、doFilter 过滤方法

4、destroy销毁

### 多个过滤器的执行

当多个过滤器一起执行过滤，看看是什么效果.

过滤器Filter1:

```java
public class Filter1 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("Filter1的调用");
        System.out.println("Filter1的线程："+Thread.currentThread().getName());
        System.out.println("Filter1取req的参数："+request.getParameter("username"));
        chain.doFilter(request,response);
        System.out.println("Filter1的线程："+Thread.currentThread().getName());
        System.out.println("Filter2结束的调用");
    }

    @Override
    public void destroy() {

    }
}
```

过滤器Filter2:

```java
public class Filter2 implements Filter{
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("Filter2的调用");
        System.out.println("Filter2的线程："+Thread.currentThread().getName());
        System.out.println("Filter2取req的参数："+request.getParameter("username"));
        chain.doFilter(request,response);
        System.out.println("Filter2的线程："+Thread.currentThread().getName());
        System.out.println("Filter2的调用结束");
    }

    @Override
    public void destroy() {

    }
}
```

```xml
<filter>
    <filter-name>Filter1</filter-name>
    <filter-class>com.filter.Filter1</filter-class>
</filter>
<filter-mapping>
    <filter-name>Filter1</filter-name>
    <url-pattern>/target.jsp</url-pattern>
</filter-mapping>
<filter>
    <filter-name>Filter2</filter-name>
    <filter-class>com.filter.Filter2</filter-class>
</filter>
<filter-mapping>
    <filter-name>Filter2</filter-name>
    <url-pattern>*.jsp</url-pattern>
</filter-mapping>
```

target.jsp页面：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    System.out.println("target页面");
    System.out.println("target的线程："+Thread.currentThread().getName());
%>
</body>
</html>
```

![image-20220311005039967](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220311005039967.png)

从结果可以看出：

![image-20220311005250784](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220311005250784.png)

1、两个过滤器和一个jsp页面属于一个线程。

2、两个Filter接受的是同一个request请求。

3、两个过滤器的先后作用和他们xml的配置循序一样。

4、chain.doFilter的作用：1、如果下一个过滤器有doFilter则调用下一个doFilter

2、如果下一个过滤器没有doFilter允许请求访问目标资源

### Filter的三种拦截方式

1、精确拦截

如：只拦截对target.jsp页面的请求。

```xml
<url-pattern>/target.jsp</url-pattern>
```

2、目录拦截：

如：拦截admin目录下的所有资源

```xml
<url-pattern>/admin/*</url-pattern>
```

3、后缀名拦截：

如：所有以.jsp结尾的请求都会被拦截，不管该请求的资源存不存在，（404的.jsp请求也拦截）

```xml
<url-pattern>*.jsp</url-pattern>
```

就是，刚刚使用了一个线程安全的静态变量的hashtable对象在多线程情况下存取数据，没有发生线程安全问题。使用共享的静态ThreadLocal对象在多线程情况下存取数据也不存在线程安全问题

### ThreadLoad

ThreadLoad的作用是解决多线程的数据安全问题。

ThreadLoad和HashMap一样既可以解决多线程的数据安全问题，也用键值对存储数据，key为当前线程

ThreadLoad实例的一个对象只能为当前线程保存一个数据。(和Map一样，key值唯一)

ThreadLoad实例的一个对象一般用static类型定义。

相关代码：ThreadLoad测试类：

```java
public class ThreadLoadTest {
    public static ThreadLocal<Object> threadLocal=new ThreadLocal<Object>();
    private static Random random=new Random();
    public static class Task implements Runnable{

        @Override
        public void run() {
            int i = random.nextInt(100);
            String name=Thread.currentThread().getName();
            System.out.println("线程["+name+"]生成的随机数是："+i);
            threadLocal.set(i);
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            //其他类的方法调用线程，不影响本线程的数据。
            new OrderServlet().createOrder();
            //线程准备结束时，再取相关数据
            Object o = threadLocal.get();
            System.out.println("线程["+name+"]准备结束后取出的数据是:"+o);
        }

        public static void main(String[] args) {
            for (int i=0;i<3;i++){
                new Thread(new Task()).start();
            }
        }
    }
```

ThreadLoad常用于处理事务，ThreadLoad的对象保存连接对象，后续获取连接可以通过hreadLoad的对象.get()获取连接，达到使连接唯一的效果，解决多线程的数据安全问题。

#### 使用doFilter处理所有异常

​	doFilter可以在我们发出请求的时候间接调用调用特定的Servlet，比如我们要提交订单，doFilter就可以调用OrderServlet，来间接调用OrderService，当OrderService里的创建订单的方法createOrder有异常时，doFilter就可以捕获该异常。同理doFilter可以捕获在它管理下的所有方法的异常。

```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
    try {
        chain.doFilter(request,response);
        JdbcUtils.commitAndClose();
    } catch (Exception e) {
        JdbcUtils.rollbackAndClose();
        e.printStackTrace();
        throw new RemoteException();
    }
}
```

Filter配置文件：

对所有的请求进行过滤：

```xml
<filter-mapping>
    <filter-name>TransactionFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 使用Tomcat接受所有异常处理

虽然我们可以使用doFilter处理所有异常，但是出现异常的时候，页面显示会出现异常（白屏等情况）。所以我们库从doFilter中抛出异常，让Tomcat获取处理。

我们可以加入配置：

```xml
<error-page>
    <error-code>500</error-code>
    <location>/pages/error/error500.jsp</location>
</error-page>
<error-page>
    //错误类型
    <error-code>404</error-code>
    //出现错误后访问的页面
    <location>/pages/error/error404.jsp</location>
</error-page>
```



### JSON

Json是一种轻量级的数据交换格式（相对xml来说）

Json是一个封装着键值对的对象：

它可以这样定义：

```json
var JsonObj={
    //整型键值对
   "key1":5,//5
   "ke2" :[2,1,6],
    //类键值对
   "ke3" : {
      key3_1:31,
      key3_2:32
},
数组类键值对
"key4":[{
   "key4_1":41,
   "key4_2":42
},{
   "key4_3":43,
   "key4_4":44
}]
};
```

json的访问和java差不多

#### json的两种形式

1、对象形式：当我们要操作或者访问json里的键值对，就使用json对象

2、字符串形式：当客户端和服务器进行数据交互时，就使用json字符串

对象转换为字符串：

```json
var JsonObjString=JSON.stringify(JsonObj);
alert(JsonObj)
```

将输出json对象里的所以键值对，类似toString

![image-20220312201829370](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220312201829370.png)

Json字符串又转化为对象类型：

```json
var JsonObjString=JSON.stringify(JsonObj);
var JsonObj2=JSON.parse(JsonObjString)
alert(JsonObj2);
```

![image-20220312202049754](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220312202049754.png)

### Json在Java中的使用

Json可以和JavaBean、List集合、Map集合相互转换

1、和JavaBean转化：

```java
@Test
public void json(){
    Gson gson = new Gson();
    Person person = new Person(1,"张三");
    //将Person转化为Json字符串
    String personString = gson.toJson(person);
    System.out.println(personString);
    //将Json字符串转化为Person类对象
    Person person1 = gson.fromJson(personString, Person.class);
    System.out.println(person1);
}
```

结果：

![image-20220312204647435](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220312204647435.png)

2、和List转化：

首先写一个 PersonListType继承TypeToken类,TypeToken类使用反射原理，支持Json字符串转化为集合

```java
public class PersonListType extends TypeToken<ArrayList<Person>> {
}
```

```java
@Test
public void test2(){
    Gson gson = new Gson();
    List<Person> list=new ArrayList<>();
   list.add(new Person(1,"张三")) ;
    list.add(new Person(2,"李四")) ;
    //将Person转化为Json字符串
    String personListString = gson.toJson(list);
    System.out.println(personListString);
  //将Json字符串转化为List
    List<Person> personList=gson.fromJson(personListString,new PersonListType().getType());
    System.out.println(personList);
}
```

3、和Map转化

```java
@Test
public void test3(){
    Gson gson = new Gson();
    Map<Integer,Person> map=new HashMap<>();
    map.put(1,new Person(1,"张三"));
    map.put(2,new Person(2,"李四"));
    //将Map转化为Json字符串
    String mapPerson = gson.toJson(map);
    System.out.println(mapPerson);
    //使用匿名内部类调用getType()方法。
    Map map2 = gson.fromJson(mapPerson, new TypeToken<HashMap<Integer,Person>>(){}.getType());
    System.out.println(map2);
}
```

## AJAX请求

浏览器通过js异步发起请求。局部更新页面的技术。

AJAX技术可以发送请求，还可以接受该请求的响应。

#### JS下的Ajax请求

JavaScript：

```html
<html>
	<head>
		<meta http-equiv="pragma" content="no-cache" />
		<meta http-equiv="cache-control" content="no-cache" />
		<meta http-equiv="Expires" content="0" />
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Insert title here</title>
		<script type="text/javascript">
			function ajaxRequest() {
// 				1、我们首先要创建XMLHttpRequest 
				var xmlHttpRequest = new XMLHttpRequest();
// 				2、调用open方法设置请求参数（method，url，是否异步）
				xmlHttpRequest.open("GET","http://localhost:8080/json_ajax_i18n/ajaxServlet?action=javaScriptAjax",true);
// 				3、在send方法前绑定onreadystatechange事件，处理请求完成后的操作。
				xmlHttpRequest.onreadystatechange=function (){
                    //xmlHttpRequest.readyState == 4表示客户端已接受到响应
					if (xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200){
						//将响应字符串转化为对象
                        var person = JSON.parse(xmlHttpRequest.responseText);
						document.getElementById("div01").innerText="编号："+person.id+"姓名："+person.name;
					}
				}
// 				4、调用send方法发送请求
				xmlHttpRequest.send();
			}
		</script>
	</head>
	<body>	
		<button onclick="ajaxRequest()">ajax request</button>
		<div id="div01">
		</div>
	</body>
</html>
```

AjaxServlet:

```java
public class AjaxServlet extends BaseServlet{
    protected void javaScriptAjax(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("阿贾克斯请求发送过来了");
        Person person = new Person(1,"张三");
        Gson gson = new Gson();
        String personString = gson.toJson(person);
        //响应请求：
        resp.getWriter().write(personString);
    }
}
```

结果：

![image-20220313101757976](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220313101757976.png)

### Ajax请求的特点

局部更新：当发起请求时，浏览器地址不会发生变化，页面内容原页面上发生变化，不会直接跳到另一个页面。

如点击a标签的地址请求会发送的另一个页面，地址和内容都发生了变化。

异步请求：当我们发起第一个请求时，不必要等待服务器响应完成才能发起第二个请求，能给用户带来更好的体验。



### JQuery的Ajax请求

可以用JQuery来简化Ajax请求的发送

相关参数：

 url:请求访问服务器绝对路径

type：请求类型

data：在请求访问服务器路径后加上参数,相当于param，之后获取响应的数据。

dataType：将响应数据格式转化。

代码实例：

单击事件实际发送的请求地址是：url+data

即：http://localhost:8080/json_ajax_i18n/ajaxServlet?action=JQueryAjax

```java
<script type="text/javascript">
   $(function(){
      // ajax请求
      $("#ajaxBtn").click(function(){
         $.ajax({
             //url表示发送请求发送到哪个Servlet服务器
            url:"http://localhost:8080/json_ajax_i18n/ajaxServlet",
            //type表示发送请求的类型
             type:"GET",
             // data表示拼接在url后面的参数请求调用Servlet服务器中的哪个方法，然后接受服务器响应的字符串数据
            data:"action=JQueryAjax",
            success :function (data){
               $("#msc").html("名字:"+data.name+"编号："+data.id)
            },
             //data的格式转化为对象
            dataType:"json"
         });


      });

      // ajax--get请求
      $("#getBtn").click(function(){
         $.get("http://localhost:8080/json_ajax_i18n/ajaxServlet","action=getAjax",function (data){
            $("#msc").html("名字:"+data.name+"编号："+data.id)
         },"json")
      });
      
      // ajax--post请求
      $("#postBtn").click(function(){
         // post请求
         $.post("http://localhost:8080/json_ajax_i18n/ajaxServlet","action=postAjax",function (data){
            $("#msc").html("名字:"+data.name+"编号："+data.id)
         },"json")
         
      });

      // ajax--getJson请求
      $("#getJSONBtn").click(function(){
         // 调用
         $.getJSON("http://localhost:8080/json_ajax_i18n/ajaxServlet","action=getJsonAjax",function (data){
            $("#msc").html("通过getJson取得：名字："+data.name+"id："+data.id)
         })

      });

      // ajax--getJson请求2
      $("#submit").click(function(){
         // 通过serialize()方法把表单的所有数据拼成字符串
         alert($("#form01").serialize());
         $.getJSON("http://localhost:8080/json_ajax_i18n/ajaxServlet","action=getJsonAjaxBySerialize&"+$("#form01").serialize(),function (data){
            $("#msc").html("通过getJson取得：名字："+data.name+"id："+data.id)
         })
      });
      
   });
</script>
```

服务器的处理请求的各种对应方法：

```java
 protected void JQueryAjax(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("JQuery的阿贾克斯请求发送过来了");
        Person person = new Person(1,"张三");
//        try {
//            Thread.sleep(3000);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        }
        Gson gson = new Gson();
        String personString = gson.toJson(person);
        resp.getWriter().write(personString);
    }
    protected void getAjax(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("get的阿贾克斯请求发送过来了");
        Person person = new Person(1,"张三");

        Gson gson = new Gson();
        String personString = gson.toJson(person);
        resp.getWriter().write(personString);
    }
    protected void postAjax(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post的阿贾克斯请求发送过来了");
        Person person = new Person(1,"张三");

        Gson gson = new Gson();
        String personString = gson.toJson(person);
        resp.getWriter().write(personString);
    }
    protected void getJsonAjax(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("getJson的阿贾克斯请求发送过来了");
        Person person = new Person(1,"张三");

        Gson gson = new Gson();
        String personString = gson.toJson(person);
        resp.getWriter().write(personString);
    }
    protected void getJsonAjaxBySerialize(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("getJson的阿贾克斯请求发送过来了");
        Person person = new Person(1,"张三");
        String name = req.getParameter("username");
        String password = req.getParameter("password");
        System.out.println("用户名："+name+"密码："+password);
        Gson gson = new Gson();
        String personString = gson.toJson(person);
        resp.getWriter().write(personString);
    }
```

## 书城项目：

### 阶段一：检验用户的注册是否合法

```html
<script type="text/javascript">
   // 页面加载完成之后
   $(function () {
      //注册输入用户名时，判断用户名是否存在
      $("#username").blur(function (){
         var username=this.value;
      //使用JQuery调用Ajax方法
         // $.getJSON自动将Servlet响应字符串数据转化为Json的键值对型型
          $.getJSON("http://localhost:8080/book/userServlet","action=aJaxExistsUsername&username="+username,function (data){
            if(data.existsUsername){
               $(".errorMsg").text("该用户名已存在");
            }else {
               $(".errorMsg").text("用户名可用");
            }

         });
      })

      $("#code_img").click(function (){
         this.src="${basePath}kaptcha.jpg";
      });
      //用户名
      $(":submit").click(function (){
         var textname=/^\w{5,12}$/;
         var username=$("#username").val();
         if(!textname.test(username)){
            $(".errorMsg").text("用户名不合法");
            return false;
         }
         //密码
         var textpw=/^\w{5,12}$/;
         var userpw=$("#password").val();
         if(!textpw.test(userpw)){
            $(".errorMsg").text("密码不合法");
            return false;
         }
         //确认密码
         var repw=$("#repwd").val();
         if(userpw!=repw){
            $(".errorMsg").text("两次密码不一致，请重新输入");
            return false;
         }
         //电子邮箱
         var useemail=$("#email").val();
         var textemail=/^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
         if(!textemail.test(useemail)){
            $(".errorMsg").text("邮箱格式错误");
            return false;
         }
         //验证码
         var usecode=$("#code").val();
         usecode=$.trim(usecode);
         if(usecode==null||usecode==""){
            $(".errorMsg").text("验证码为空");
            return false;
         }
      });



   });

</script>
```

### 阶段二：

**确定JavaEE三层架构：**
1、Servlet层：（Web层）

接受客户端的get或post请求，然后处理请求

Servlet层可以调用Service层的方法来处理请求，比如调用Service层的方法查找用户名和客户端的发送请求的用户名来进行的对比

2、Service层：

Service层定义实现了各种业务的方法，比如添加商品到购物车业务。

Service层调用Dao层的基本的数据增删改查方法来构成自己的业务方法。

Service层是基于Dao层，来进行完善成业务所需的方法库层。

3、Dao层持久层（JDBC）

获取数据库的连接，然后使用Java语言来将sql语句传入数据库，实现各种增删查改方法，达到域数据库交互的目的。

4、JavaBean：创建和数据库表对应的类，数据库表的属性就是类的成员量，数据库的每一行就相对于类的每一个对象实例。

**注册业务的实现**

### 用户的表：

![image-20220313234106182](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220313234106182.png)

### 对应用户的JavaBean：类属性和表属性对象

```java
package com.book.pojo;

public class User {
    private Integer id;
    private String username;
    private String password;
    private String  email;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
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

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", email='" + email + '\'' +
                '}';
    }

    public User(Integer id, String username, String password, String email) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.email = email;
    }

    public User() {
    }
}
```

### JdbcUtils工具类

该工具类实现了数据库连接，数据库关闭，事务回滚、事务提交等静态方法，当需要时，可直接调用其方法。

获取连接可调用Druid（阿里巴巴）连接池，来

```java
import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

public class JdbcUtils {
    //获取Druid连接池
    private static DataSource source=null;
    private static ThreadLocal<Connection> conns=new ThreadLocal<>();
    //连接池只需要一个，通过Properties集合以流的形式读取druid.properties文件，文件保存了数据库连接所需的数据库用户名、密码、url等。
    static {
        try {
            Properties pro = new Properties();
            InputStream is = JdbcUtils.class.getClassLoader().getResourceAsStream("druid.properties");
            pro.load(is);
            source= DruidDataSourceFactory.createDataSource(pro);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    //获取连接的静态方法，一个连接池可提供多个连接。
    public static Connection getConnection(){
        Connection conn = conns.get();
        if (conn==null){
            try {
                conns.set(source.getConnection());
            } catch (SQLException e) {
                e.printStackTrace();
            }
            conn=conns.get();
        }
        try {
            conn.setAutoCommit(false);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return conn;
    }
		//提交事务并关闭连接
    public static void commitAndClose()  {
        Connection conn = conns.get();
        //如果连接被用过
        if (conn!=null){
            try {
            conn.commit();
            } catch (SQLException e) {
                e.printStackTrace();
            }finally {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        conns.remove();
    }
    //回滚事务并关闭连接
    public static void rollbackAndClose()  {
        Connection conn = conns.get();
        //如果连接被用过
        if (conn!=null){
            try {
                conn.rollback();
            } catch (SQLException e) {
                e.printStackTrace();
            }finally {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        conns.remove();
    }

}
```

Druid连接池获取连接资源所需的数据库用户信息，以及连接的管理信息

```properties
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/book?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
username=root
password=123456Hh
//初始连接数
initialSize=5
//最大活跃连接数
maxAction=10
```

### Dao

导入DbUtils的jar包，DbUtils是一个开源工具类库，封装类许多数据库的增删查改方法，简化了JDBC的开发。

我们可以通过使用DbUtils，调用其方法，来编写一个BaseDao类，获取数据库的连接，然后将sql语句、连接和填充占位符的可变数组传进DbUtils里的方法，然后将结果返回给调用dao的Service方法。

BaseDao：抽象类实现各种情况的增删查改方法，供所有JavaBean类继承重写方法。

```java
package com.book.dao.impl;

import com.book.test.utils.JdbcUtils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

public abstract class BaseDao {
    //使用DbUtils包实现各种增删查改方法
    //实现增删改通用
    public int  update(String sql,Object...ags){
        Connection conn = null;
        try {
            conn = JdbcUtils.getConnection();
            QueryRunner runner = new QueryRunner();
            return runner.update(conn,sql,ags);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
    //实现单行查询
    public <T> T queryForOne(Class<T> clazz,String sql,Object...ags){
        Connection conn = null;
        try {
            conn = JdbcUtils.getConnection();
            QueryRunner runner = new QueryRunner();
            return runner.query(conn,sql,new BeanHandler<T>(clazz),ags);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
    //实现多行查询
    public <T> List<T> queryForList(Class<T> clazz, String sql, Object...ags){
        Connection conn = null;
        try {
            conn = JdbcUtils.getConnection();
            QueryRunner runner = new QueryRunner();
            return runner.query(conn,sql,new BeanListHandler<T>(clazz),ags);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
    //实现特殊组函数查询
    public Object queryForSingleValue(String sql, Object...ags){
        Connection conn = null;
        try {
            conn = JdbcUtils.getConnection();
            QueryRunner runner = new QueryRunner();
            return runner.query(conn,sql,new ScalarHandler(),ags);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
}
```

### UserDao

继承BaseDao类，只需修改sql语句就可以实现自己的方法。

```java
package com.book.dao.impl;

import com.book.dao.UserDao;
import com.book.pojo.User;

public class UserDaoImpl extends BaseDao implements UserDao {
    @Override
    public User queryUserByUsername(String username) {
        String sql = "select id,username,password,email from t_user where username=?";
        return queryForOne(User.class, sql, username);
    }

    @Override
    public User queryUserByUsernameAndPassword(String username, String password) {
        String sql = "select id,username,password,email from t_user where username=? and password=?";
        return queryForOne(User.class, sql, username, password);
    }

    @Override
    public int saveUser(User user) {
        String sql = "insert into t_user(username,password,email) values(?,?,?) ";
        return update(sql,user.getUsername(),user.getPassword(),user.getEmail());
    }
}
```

### UserService

调用UserDao的方法，来实现具体业务需要

如检测用户名是否存在，和注册时保存用户名和密码到数据库里。

```java
import com.book.dao.UserDao;
import com.book.dao.impl.BaseDao;
import com.book.dao.impl.UserDaoImpl;
import com.book.pojo.User;
import com.book.service.UserService;

public class UserServiceImpl implements UserService {
    UserDao dao = new UserDaoImpl();
    @Override
    public void registerUser(User user) {
         dao.saveUser(user);
    }
		//通过客户端表单输入的用户名和密码来与数据库比对，数据库如果有这个用户名和密码，就返回User对象，否则返回null
    @Override
    public User login(User user) {
        return dao.queryUserByUsernameAndPassword(user.getUsername(),user.getPassword());
    }
		//注册是检验用户名是否可用，如果用户名存在，返回false,表示该用户名不能用，否则返回true
    @Override
    public boolean existsUsername(String username) {
        return (dao.queryUserByUsername(username)!=null);
    }
}
```

### Servlet层(Web层)

#### BaseServlet

BaseServlet继承了HttpServlet，可以处理get和post请求。

BaseServlet通过反射来获取优化代码。

BaseServlet封装成抽象类，供各种Servlet来继承.

action是BaseServlet的重点，客户端的请求指定了action是什么方法，BaseServlet就通过获取地址栏的action值,来调用对应子类的方法（  method.invoke(this,req,resp)）

```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.Method;

public abstract class BaseServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("UTF-8");
        resp.setContentType("test/html;charset=UTF-8");
        String action=req.getParameter("action");
        try {
            //获取子类的方法，并调用
            Method method = this.getClass().getDeclaredMethod(action,HttpServletRequest.class,HttpServletResponse.class);
            method.invoke(this,req,resp);
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
}
```

### UserServlet：将注册和登录结合起来

UserServlet继承了BaseServlet。

实现了各种业务请求处理方法。

```java
import com.book.pojo.User;
import com.book.service.UserService;
import com.book.service.impl.UserServiceImpl;
import com.book.test.utils.WebUtils;
import com.google.gson.Gson;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

import static com.google.code.kaptcha.Constants.KAPTCHA_SESSION_KEY;

public class UserServlet extends BaseServlet {
    //注销
    protected void loginOut(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //删除会话
        req.getSession().invalidate();
        //重定向到首页
        resp.sendRedirect(req.getContextPath());
    }
    protected void login(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        //检验用户或密码是否正确
        UserService us= new UserServiceImpl();
        User user = new User(null,username,password,null);
        User loginUser=us.login(user);
        if (loginUser!=null){
            //成功登录
            //保存用户登录信息到Session域中
            req.getSession().setAttribute("user",loginUser);
            req.getRequestDispatcher("/pages/user/login_success.jsp").forward(req,resp);
        }else {
            req.setAttribute("msg","用户名密码错误");
            req.setAttribute("username",username);
            req.getRequestDispatcher("/pages/user/login.jsp").forward(req,resp);
        }
    }
    //注册输入用户名时检查该用户名是否存在
    protected void aJaxExistsUsername(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        //调用方法来判断用户名是否存在
        UserService us= new UserServiceImpl();
        boolean existsUsername = us.existsUsername(username);
        Map<String,Boolean> map=new HashMap<>();
        map.put("existsUsername",existsUsername);
        Gson gson = new Gson();
        String s = gson.toJson(map);
        resp.getWriter().write(s);


    }
    protected void regist(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1、获取请求参数

        String code=req.getParameter("code");
        //获取验证码
        String token = (String) req.getSession().getAttribute(KAPTCHA_SESSION_KEY);
        //删除验证码
        req.getSession().removeAttribute(KAPTCHA_SESSION_KEY);

        User user=WebUtils.copyParamToBean(req.getParameterMap(),new User());
        //2、检查验证码是否正确
        //验证码正确
        if(token!=null&& token.equalsIgnoreCase(code)) {
            UserService us = new UserServiceImpl();
            //3、检查用户名是否可用
            //可用
            if (!us.existsUsername(user.getUsername())){
                //调用Service保存到数据库
                us.registerUser(user);
                //跳转到注册成功页面
                req.getRequestDispatcher("/pages/user/regist_success.jsp").forward(req,resp);
            }else {
                //不可用
                //跳回注册页面
                System.out.println("用户名"+user.getUsername()+"已存在！！！");
                req.setAttribute("msg","用户名已存在");
                req.setAttribute("username",user.getUsername());
                req.setAttribute("email",user.getEmail());
                req.getRequestDispatcher("/pages/user/regist.jsp").forward(req,resp);
            }

        }else {
            //验证码不正确
            //跳回注册页面
            req.setAttribute("msg","验证码错误错误");
            req.setAttribute("username",user.getUsername());
            req.setAttribute("email",user.getEmail());
            System.out.println("验证码"+code+"不正确");
            req.getRequestDispatcher("/pages/user/regist.jsp").forward(req,resp);
        }
    }

}
```

###  BeanUtils

将请求的参数一次性注入到JavaBean中，就不用JavaBean每次都要实例化对象了。

#### WebUtils

实现了BeanUtils的思想

例如我们上架新书，添加商品，发送请求给服务器，Servel处理请求，将请求添加的商品的名字、价格等属性封装成Map对象，注入到Book（JavaBean）的对象中，然后把对象传入Service的对应添加方法，保存到数据库中。

```java
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.http.HttpServletRequest;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

public class WebUtils {
    //将请求的参数以键值对的方式封装起来，传入，将bean对象（空的对象，属性没赋值的对象）也传入，将请求的参数里的值赋给Bean对象,并返回对象。
    public static <T> T  copyParamToBean(Map map,T bean){
        System.out.println("注入之前："+bean);
        try {
            BeanUtils.populate(bean,map);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        System.out.println("注入之后："+bean);
        return bean;
    }
    //将请求的ID号转化成int型
    static public Integer parseInt(String str,int value){
        try {

        return Integer.parseInt(str);
        }catch (Exception e){
            e.printStackTrace();
        }
        return value;
    }
}
```

### 第五阶段：图书模块

#### 创建图书的数据库表和对应的JavaBean

图书的JavaBean（Book）

```java
import java.math.BigDecimal;

public class Book {
    private Integer id;
    private String name;
    private String author;
    private BigDecimal price;
    private Integer sales;
    private Integer stock;
    private String imgpath="static/img/logo.gif";

    public Book() {
    }

    public Book(Integer id, String name, String author, BigDecimal price, Integer sales, Integer stock, String imgpath) {
        //图书id
        this.id = id;
        //图书名
        this.name = name;
        //作者
        this.author = author;
        //价格
        this.price = price;
        //销量
        this.sales = sales;
        //库存
        this.stock = stock;
        if(imgpath!=null&&!"".equals(imgpath)){

            this.imgpath = imgpath;
        }
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public BigDecimal getPrice() {
        return price;
    }

    public void setPrice(BigDecimal price) {
        this.price = price;
    }

    public Integer getSales() {
        return sales;
    }

    public void setSales(Integer sales) {
        this.sales = sales;
    }

    public Integer getStock() {
        return stock;
    }

    public void setStock(Integer stock) {
        this.stock = stock;
    }

    public String getImgpath() {
        return imgpath;
    }

    public void setImgpath(String imgpath) {
        if(imgpath!=null&&!"".equals(imgpath)){

        this.imgpath = imgpath;
        }
    }

    @Override
    public String toString() {
        return "Book{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", author='" + author + '\'' +
                ", price=" + price +
                ", sales=" + sales +
                ", stock=" + stock +
                ", imgpath='" + imgpath + '\'' +
                '}';
    }
}
```

### 分页的JavaBean

```java
import java.util.List;

public class Page<T> {
    public static final Integer PAGE_SIZE =4;
    //当前页码
    private Integer pageNo;
    //总页码
    private Integer pageTotal;
    //当前页显示数量
    private Integer pageSize= PAGE_SIZE;
    //总记录数
    private Integer pageTotalCount;
    //当前页数据
    private List<T> items;
    //分页条的请求地址
    private String url;

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public Page() {
    }


    public Page(Integer pageNo, Integer pageTotal, Integer pageSize, Integer pageTotalCount, List<T> items, String url) {
        this.pageNo = pageNo;
        this.pageTotal = pageTotal;
        this.pageSize = pageSize;
        this.pageTotalCount = pageTotalCount;
        this.items = items;
        this.url = url;
    }

    public Integer getPageNo() {
        return pageNo;
    }

    public void setPageNo(Integer pageNo) {
//        数据边界有效检查
         if (pageNo<1){
            pageNo=1;
        }
        if (pageNo>pageTotal) {
            pageNo = pageTotal;
        }
        this.pageNo = pageNo;
    }

    public Integer getPageTotal() {
        return pageTotal;
    }

    public void setPageTotal(Integer pageTotal) {
        this.pageTotal = pageTotal;
    }

    public Integer getPageSize() {
        return pageSize;
    }

    public void setPageSize(Integer pageSize) {
        this.pageSize = pageSize;
    }

    public Integer getPageTotalCount() {
        return pageTotalCount;
    }

    public void setPageTotalCount(Integer pageTotalCount) {
        this.pageTotalCount = pageTotalCount;
    }

    public List<T> getItems() {
        return  items;
    }

    public void setItems(List<T> items) {
        this.items =  items;
    }

    @Override
    public String toString() {
        return "Page{" +
                "pageNo=" + pageNo +
                ", pageTotal=" + pageTotal +
                ", pageSize=" + pageSize +
                ", pageTotalCount=" + pageTotalCount +
                ", items=" + items +
                ", url='" + url + '\'' +
                '}';
    }
}
```

### 图书的Dao（BookDao）

继承BaseDao实现了图书的各种增删改查方法

```java
public class BookImpl extends BaseDao implements BookDao {
    @Override
    public int addBook(Book book) {
        String sql= "insert into t_book(`name`,`author`,`price`,`sales`,`stock`,`img_path`) value(?,?,?,?,?,?)";
        int addCounts = update(sql, book.getName(), book.getAuthor(), book.getPrice(), book.getSales(), book.getStock(), book.getImgpath());
        return addCounts;
    }

    @Override
    public int deleteBookById(Integer id) {
        String sql="delete from t_book where id=?";
        return update(sql,id);
    }

    @Override
    public int updateBook(Book book) {
        String sql="update t_book set `name`=?, `author`=?, `price`=?, `sales`=?, `stock`=?, `img_path`=? where id=?";
        int updateCount = update(sql, book.getName(), book.getAuthor(), book.getPrice(), book.getSales(), book.getStock(),book.getImgpath(), book.getId());
        return updateCount;
    }

    @Override
    public Book queryBookById(Integer id) {
        System.out.println(Thread.currentThread().getName());
        String sql="select `id`,`name`,`author`,`price`,`sales`,`stock`,`img_path` from t_book where id=?";
        return queryForOne(Book.class,sql,id);
    }

    @Override
    public List<Book> queryBooks() {
        String sql="select `id`,`name`,`author`,`price`,`sales`,`stock`,`img_path` from t_book";
        return queryForList(Book.class,sql);
    }

    @Override
    public Integer queryForTotalCount() {
        String sql="select count(*) from t_book";
        Number number = (Number) queryForSingleValue(sql);
        return number.intValue();
    }

    @Override
    public List<Book> queryForPageItem(int begin, Integer pageSize) {
        String sql="select `id`,`name`,`author`,`price`,`sales`,`stock`,`img_path` from t_book limit ? ,?";
        return queryForList(Book.class,sql,begin,pageSize);
    }

    @Override
    public Integer queryForTotalCount(int min, int max) {
        String sql="select count(*) from t_book where price between ? and ?";
        Number number = (Number) queryForSingleValue(sql,min,max);
        return number.intValue();
    }

    @Override
    //查询价格区间内的所有书籍并保存到List集合中
    public List<Book> queryForPageItem(int begin, Integer pageSize, int min, int max) {
        String sql="select `id`,`name`,`author`,`price`,`sales`,`stock`,`img_path` from t_book where price between ? and ? limit ? ,?";
        return queryForList(Book.class,sql,min,max,begin,pageSize);
    }
}
```

### 图书Service（BookService）

具体地实现了读书的增删查改业务，以及图书的页的分页。

```java
import com.book.dao.BookDao;
import com.book.dao.impl.BookImpl;
import com.book.pojo.Book;
import com.book.pojo.Page;
import com.book.service.BookService;

import java.util.List;

public class BookServiceImpl implements BookService {
    private BookDao dao=new BookImpl();

    @Override
    public Page page(Integer pageNo, Integer pageSize) {
        Page<Book> page = new Page<>();
        page.setPageSize(pageSize);
        //求总记录数
        Integer totalCount=dao.queryForTotalCount();
        page.setPageTotalCount(totalCount);
        //求总页数
        int pageTotal = totalCount / pageSize;
        if (totalCount%pageSize!=0){
            pageTotal=pageTotal+1;
        }
        page.setPageTotal(pageTotal);
        page.setPageNo(pageNo);
        //求当前页数据
        //求当前页数据的开始
        int begin=(page.getPageNo()-1)*pageSize;
        List<Book> items=dao.queryForPageItem(begin,pageSize);
        page.setItems(items);
        return page;
    }

    @Override
    //价格区间显示到页面的业务处理方法
    public Page pageByPrice(int pageNo, int pageSize, int min, int max) {
        Page<Book> page = new Page<>();
        page.setPageSize(pageSize);
        //求总记录数
        Integer totalCount=dao.queryForTotalCount(min,max);
        page.setPageTotalCount(totalCount);
        //求总页数
        int pageTotal = totalCount / pageSize;
        if (totalCount%pageSize!=0){
            pageTotal=pageTotal+1;
        }
        page.setPageTotal(pageTotal);
        page.setPageNo(pageNo);
        //求当前页数据
        //求当前页数据的开始
        int begin=(page.getPageNo()-1)*pageSize;
        List<Book> items=dao.queryForPageItem(begin,pageSize,min,max);
        page.setItems(items);
        return page;
    }
    @Override
    public void addBook(Book book) {
         dao.addBook(book);
    }

    @Override
    public void deleteBookById(Integer id) {
         dao.deleteBookById(id);
    }

    @Override
    public void updateBook(Book book) {
         dao.updateBook(book);
    }

    @Override
    public Book queryBookById(Integer id) {
         return dao.queryBookById(id);
    }

    @Override
    public List<Book> queryBooks() {
         return dao.queryBooks();
    }



}
```

### 图书Servlet层（BookServlet）

将处理图书页面的请求，将处理后的信息保存到请求域中，再通过请求转发或者重定向，图书经过发起增删查改请求后，根据请求转发或者重定向地址(jsp页面)，jsp页面获取请求域的数据，动态显示图书改动效果。

```java
public class BookServlet extends BaseServlet {
    BookService bs=new BookServiceImpl();
    //添加图书
    protected void add(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Integer pageNo = WebUtils.parseInt(req.getParameter("pageNo"), 0);
        pageNo+=1;
        Book book= WebUtils.copyParamToBean(req.getParameterMap(),new Book());
            bs.addBook(book);
            //重定向，工程名+具体路径
        	//重定向回到刚刚添加图书的那一页
            resp.sendRedirect(req.getContextPath()+"/manager/bookServlet?action=page&pageNo="+pageNo);
    }
    protected void delete(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String id1=req.getParameter("id");
        Integer id = WebUtils.parseInt(id1, 0);
        bs.deleteBookById(id);
        //重定向回到刚刚添加图书的那一页
        resp.sendRedirect(req.getContextPath()+"/manager/bookServlet?action=page&pageNo="+req.getParameter("pageNo"));

    }
    protected void update(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Book book = WebUtils.copyParamToBean(req.getParameterMap(), new Book());
        bs.updateBook(book);
        resp.sendRedirect(req.getContextPath()+"/manager/bookServlet?action=page&pageNo="+req.getParameter("pageNo"));
    }
    protected void list(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        List<Book> books = bs.queryBooks();
        req.setAttribute("books",books);
        req.getRequestDispatcher("/pages/manager/book_manager.jsp").forward(req,resp);
    }
    protected void getBook(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取要修改的图书id
        Integer id = WebUtils.parseInt(req.getParameter("id"), 0);
        //通过ID查找图书信息
        Book book = bs.queryBookById(id);
        //将图书信息保存到请求域中
        req.setAttribute("b",book);
        //跳转到修改书的页面
        req.getRequestDispatcher("/pages/manager/book_edit.jsp").forward(req,resp);
    }
    //处理分页的请求，点击一次跳转的页面数都会发送一次请求，将想要跳转的页面通过请求发送过来，然后调用Service的page方法，返回当前页面的图书集合List<Book>,然后Servlet的page方法将把图书集合放到请求域，jsp就可以通过请求域获取图书集合，然后遍历图书集合，显示当前页面。
    
    protected void page(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取pageNo和pageSize参数
        Integer pageNo = WebUtils.parseInt(req.getParameter("pageNo"), 1);
        Integer pageSize = WebUtils.parseInt(req.getParameter("pageSize"), Page.PAGE_SIZE);
        Page page=bs.page(pageNo,pageSize);
        //将请求地址封装发到jsp，给jsp获取，以后想改分页条的跳转地址，只需在这里改url就行了
        String url="manager/bookServlet";
        page.setUrl(url);
        req.setAttribute("page",page);
        req.getRequestDispatcher("/pages/manager/book_manager.jsp").forward(req,resp);
    }
}
```

注意：增删改图书时，都要回到刚才操作的页面，而已要用重定向到页面，不然会出现重复操作的现象。

### 阶段四：购物车

将购物车对象放到Session上保存，每次添加商品到购物车可以通过获取Session上的Cart对象调用addItems方法，将商品添加到购物车的对象Cart的Map<Integer,CartItem> items属性里。

购物车将用户添加的图书进行数量，价格，的统计，还可以进行删除购物车读书和清空购物车的操作。

### 购物车商品项JavaBean

### CartItem

```java
import java.math.BigDecimal;

public class CartItem {
    private Integer id;
    private String name;
    private Integer count;
    //商品单价
    private BigDecimal price;
    //商品总价
    private BigDecimal totalPrice;


    public CartItem() {
    }

    public CartItem(Integer id, String name, Integer count, BigDecimal price, BigDecimal totalPrice) {
        this.id = id;
        this.name = name;
        this.count = count;
        this.price = price;
        this.totalPrice = totalPrice;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getCount() {
        return count;
    }

    public void setCount(Integer count) {
        this.count = count;
    }

    public BigDecimal getPrice() {
        return price;
    }

    public void setPrice(BigDecimal price) {
        this.price = price;
    }

    public BigDecimal getTotalPrice() {
        return totalPrice;
    }

    public void setTotalPrice(BigDecimal totalPrice) {
        this.totalPrice = totalPrice;
    }

    @Override
    public String toString() {
        return "CartItem{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", count=" + count +
                ", price=" + price +
                ", totalPrice=" + totalPrice +
                '}';
    }
}
```

### 购物车所有商品操作和统计JavaBean（Cart）

该javabean直接实现了业务方法，代替了Service的功能，

```java
import java.math.BigDecimal;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

public class Cart {
    private Map<Integer,CartItem> items=new LinkedHashMap<>();

    //添加商品
    public void addItem(CartItem cartItem){
        //查看当前商品存不存在
        //如果存在
        if (items.containsKey(cartItem.getId())){
            items.get(cartItem.getId()).setCount(items.get(cartItem.getId()).getCount()+1);
            items.get(cartItem.getId()).setTotalPrice(items.get(cartItem.getId()).getPrice().multiply(new BigDecimal(items.get(cartItem.getId()).getCount())));
            System.out.println(items);
        }else {
            //如果不存在
//            cartItem.setCount(1);
            items.put(cartItem.getId(),cartItem);
        }
    }
    //删除商品
    public void deleteItem(Integer id){
        items.remove(id);
    }
    //清空购物车
    public void clear(){
        items.clear();
    }
    //修改商品数量
    public void updateCount(Integer id,Integer count){
        if (items.containsKey(id)){
            items.get(id).setCount(count);
            items.get(id).setTotalPrice(items.get(id).getPrice().multiply(new BigDecimal(count)));
        }
//        for (Map.Entry<Integer,CartItem> item:items.entrySet() ){
//            if (id.equals(item.getKey())){
//                item.getValue().setCount(count);
//            }
//        }
    }
    public Cart() {
    }



    public Integer getTotalCount() {
        Integer totalCount=0;
        for (Map.Entry<Integer,CartItem> item:items.entrySet() ){
            totalCount+=item.getValue().getCount();
        }
        return totalCount;
    }

    public Cart(Map<Integer, CartItem> items) {
        this.items = items;
    }

    public BigDecimal getTotalPrice() {
        BigDecimal totalPrice=new BigDecimal(0);
        for (Map.Entry<Integer,CartItem> item:items.entrySet() ){
            totalPrice=totalPrice.add(item.getValue().getTotalPrice());
        }
        return totalPrice;
    }



    public Map<Integer, CartItem> getItems() {
        return items;
    }

    public void setItems(Map<Integer, CartItem> items) {
        this.items = items;
    }

    @Override
    public String toString() {
        return "Cart{" +
                "totalCount=" + getTotalCount() +
                ", totalPrice=" + getTotalPrice() +
                ", items=" + items +
                '}';
    }
```

### 购物车的Servlet（CartServlet）

直接调用Cart类里的方法来构建处理请求的方法。

```java
import com.book.pojo.Book;
import com.book.pojo.Cart;
import com.book.pojo.CartItem;
import com.book.service.BookService;
import com.book.service.impl.BookServiceImpl;
import com.book.test.utils.WebUtils;
import com.google.gson.Gson;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

public class CartServlet extends BaseServlet{
    //接受ajax请求的方法，然后通过响应流将最后添加的图书和购物车总商品数发送出去。
    protected void ajaxAddItem(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取请求参数，商品的编号
        Integer id = WebUtils.parseInt(req.getParameter("id"), 0);
        //调用Service里的方法，通过id得到图书信息
        BookService bookService = new BookServiceImpl();
        Book book = bookService.queryBookById(id);
        Cart cart = (Cart) req.getSession().getAttribute("cart");
        CartItem cartItem = new CartItem(id,book.getName(),1,book.getPrice(),book.getPrice());
        //把图书信息转化为CartItem商品项，创建会话保存对象
        //如果购物车为空
        if (cart==null){
            //创建一个name等于cart的会话（存放购物车）
             cart= new Cart();
            req.getSession().setAttribute("cart",cart);
        }
        //调用addItem方法添加图书
        cart.addItem(cartItem);
        //通过会话将最后一次添加的商品name上传
        Map<String,Object> map=new HashMap<>();
        map.put("lastCartName", cartItem.getName());
        map.put("cartTotalCount",cart.getTotalCount());
        Gson gson = new Gson();
        String s = gson.toJson(map);
        resp.getWriter().write(s);

    }
    //删除购物车里的商品
    protected void deleteItem(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       //获取请求删除的商品的id
        Integer id = WebUtils.parseInt(req.getParameter("id"), 0);
        //获取会话里的购物车
        Cart cart= (Cart) req.getSession().getAttribute("cart");
        //判断购物车是否存在
        if(cart!=null){
            //删除
            cart.deleteItem(id);
            //重定向到购物车页
            resp.sendRedirect(req.getHeader("Referer"));
        }
    }
    //清空购物车
    protected void clearCart(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取会话里的购物车
        Cart cart= (Cart) req.getSession().getAttribute("cart");
        //判断购物车是否存在
        if(cart!=null){
            //清空
            cart.clear();
            //重定向到购物车页
            resp.sendRedirect(req.getHeader("Referer"));
        }
    }
    //修改商品数量
    protected void updateCount(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Integer count = WebUtils.parseInt(req.getParameter("count"), 1);
        Integer id = WebUtils.parseInt(req.getParameter("id"), 0);
        //获取会话里的购物车
        Cart cart= (Cart) req.getSession().getAttribute("cart");
        //判断购物车是否存在
        if(cart!=null){
            //修改商品数量
            cart.updateCount(id,count);
            //重定向到购物车页
            resp.sendRedirect(req.getHeader("Referer"));
        }
    }
}
```

### 第七阶段：订单

购物车页面点击提交订单是，会生成一个订单，该订单的里的所有商品会保存到数据库的订单商品表，订单也会保存到一个订单表（订单编号、创建订单时间）

### 订单JavaBean

```java
import java.math.BigDecimal;
import java.util.Date;

public class Order {
    //订单Id
    private String orderId;
    private Date createTime;
    //订单总价
    private BigDecimal price;
    //订单状态，0：未发货 1：已发货 2：已签收
    private Integer status=0;
    private Integer userId;

    public Order() {
    }

    public Order(String orderId, Date createTime, BigDecimal price, Integer status, Integer userId) {
        this.orderId = orderId;
        this.createTime = createTime;
        this.price = price;
        this.status = status;
        this.userId = userId;
    }

    public String getOrderId() {
        return orderId;
    }

    public void setOrderId(String orderId) {
        this.orderId = orderId;
    }

    public Date getCreateTime() {
        return createTime;
    }

    public void setCreateTime(Date createTime) {
        this.createTime = createTime;
    }

    public BigDecimal getPrice() {
        return price;
    }

    public void setPrice(BigDecimal price) {
        this.price = price;
    }

    public Integer getStatus() {
        return status;
    }

    public void setStatus(Integer status) {
        this.status = status;
    }

    public Integer getUserId() {
        return userId;
    }

    public void setUserId(Integer userId) {
        this.userId = userId;
    }

    @Override
    public String toString() {
        return "Order{" +
                "orderId='" + orderId + '\'' +
                ", createTime=" + createTime +
                ", price=" + price +
                ", status=" + status +
                ", userId=" + userId +
                '}';
    }
```

### 订单商品项JavaBen

```java
import java.math.BigDecimal;

public class OrderItem {
    private Integer id;
    private String name;
    private  Integer count;
    private BigDecimal price;
    private BigDecimal totalPrice;
    private String orderId;

    public OrderItem() {
    }

    public OrderItem(Integer id, String name, Integer count, BigDecimal price, BigDecimal totalPrice, String orderId) {
        this.id = id;
        this.name = name;
        this.count = count;
        this.price = price;
        this.totalPrice = totalPrice;
        this.orderId = orderId;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getCount() {
        return count;
    }

    public void setCount(Integer count) {
        this.count = count;
    }

    public BigDecimal getPrice() {
        return price;
    }

    public void setPrice(BigDecimal price) {
        this.price = price;
    }

    public BigDecimal getTotalPrice() {
        return totalPrice;
    }

    public void setTotalPrice(BigDecimal totalPrice) {
        this.totalPrice = totalPrice;
    }

    public String getOrderId() {
        return orderId;
    }

    public void setOrderId(String orderId) {
        this.orderId = orderId;
    }

    @Override
    public String toString() {
        return "OrderItem{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", count=" + count +
                ", price=" + price +
                ", totalPrice=" + totalPrice +
                ", orderId='" + orderId + '\'' +
                '}';
    }
}
```

### 订单Dao

```java
import com.book.dao.OrderDao;
import com.book.pojo.Order;
import com.book.web.BaseServlet;

public class OrderDaoImp extends BaseDao implements OrderDao {
    @Override
    public int saveOrder(Order order) {
        System.out.println(Thread.currentThread().getName());
        String sql="insert into t_order(order_id,create_time,price,status,user_id) values(?,?,?,?,?)";
        return update(sql,order.getOrderId(),order.getCreateTime(),order.getPrice(),order.getStatus(),order.getUserId());
    }
}
```

### 订单商品项Dao

```java
import com.book.dao.OrderItemDao;
import com.book.pojo.OrderItem;

public class OrderItemDaoImp extends BaseDao implements OrderItemDao {
    @Override
    public int saveOrderItem(OrderItem orderItem) {
        System.out.println(Thread.currentThread().getName());
        String sql="insert into t_order_item(name,count,price,total_price,order_id) values(?,?,?,?,?)";
        return update(sql,orderItem.getName(),orderItem.getCount(),orderItem.getPrice(),orderItem.getTotalPrice(),orderItem.getOrderId());
    }
}
```

上面这两个Dao都是将相应的订单对象的内容保存到数据库中。

### 订单Service（OrderService）

保存订单，和将购物车商品信息保存到订单商品项里。

```java
import com.book.dao.BookDao;
import com.book.dao.OrderDao;
import com.book.dao.OrderItemDao;
import com.book.dao.impl.BookImpl;
import com.book.dao.impl.OrderDaoImp;
import com.book.dao.impl.OrderItemDaoImp;
import com.book.pojo.*;
import com.book.service.OrderService;

import java.util.Date;
import java.util.Map;

public class OrderServiceImp implements OrderService {
    @Override
    public String createOrder(Cart cart, Integer userId) {
        System.out.println(Thread.currentThread().getName());
        OrderDao orderDao=new OrderDaoImp();
        OrderItemDao orderItemDao=new OrderItemDaoImp();
        String orderId=System.currentTimeMillis()+""+userId;
        //保存订单
        Order order = new Order(orderId,new Date(),cart.getTotalPrice(),1,userId);
        orderDao.saveOrder(order);
//        int i=12/0;
        //保存订单项，遍历购物车，将购物车商品变成订单项
        for (Map.Entry<Integer, CartItem> entry:cart.getItems().entrySet()){
            CartItem cartItem = entry.getValue();
            OrderItem orderItem = new OrderItem(null,cartItem.getName(),cartItem.getCount(),cartItem.getPrice(),cartItem.getTotalPrice(),orderId);
            orderItemDao.saveOrderItem(orderItem);
            BookDao bd=new BookImpl();
            //每次生成一个订单，修改一个商品的库存和销量
            Book book = bd.queryBookById(cartItem.getId());
            book.setSales(cartItem.getCount()+book.getSales());
            book.setStock(book.getStock()-cartItem.getCount());
            bd.updateBook(book);
        }
//            结算完成删除购物车。
            cart.clear();
        return orderId;
    }
}
```

订单Servlet（OrderServlet）

当请求结账时，OrderServlet就会处理请求，将当前购物车和用户ID参数传到Service的createOrder(cart, id)方法，生成订单，保存订单，和订单商品到数据库，同时将订单号发送出去，并清空购物车。

```java
import com.book.pojo.Cart;
import com.book.pojo.User;
import com.book.service.OrderService;
import com.book.service.impl.OrderServiceImp;
import com.book.test.utils.JdbcUtils;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class OrderServlet extends BaseServlet{
    OrderService orderService=new OrderServiceImp();
    protected void createOrder(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取Session中的Cart和id
        Cart cart = (Cart) req.getSession().getAttribute("cart");
        User loginUser = (User) req.getSession().getAttribute("user");
        //如果用户没有登录，则结算时强制让用户登录
        if (loginUser==null){
            req.getRequestDispatcher("/pages/user/login.jsp").forward(req,resp);
            return;
        }
        Integer id = loginUser.getId();
        String orderId = null;
            orderId = orderService.createOrder(cart, id);
        //将订单号发送上去，给订单页面获取
        req.getSession().setAttribute("orderId",orderId);
        resp.sendRedirect("/book/pages/cart/checkout.jsp");

    }
}
```
