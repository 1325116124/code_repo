# javaweb

## day05

### xml

```xml
<?xml version="1.0" encoding="utf-8" ?>
<books>
    <book sn="12314632165">
        <name>时间简史</name>
        <author>霍金</author>
        <price>75</price>
    </book>
</books>
```

- **所有XML元素都须有关闭标签（也就是闭合）**
- **XML标签对大小写敏感**
- **必须正确地嵌套**
- **文档必须有根元素** 
- **属性值须加引号** 
- **<![CDATA[这里可以把你输入的字符原样显示，不会解析xml]]>**

### tomcat

- **bin 专门用来存放 Tomcat 服务器的可执行程序** 

- **conf 专门用来存放 Tocmat 服务器的配置文件** 

- **lib 专门用来存放 Tomcat 服务器的 jar 包** 

- **logs 专门用来存放 Tomcat 服务器运行时输出的日记信息** 

- **temp 专门用来存放 Tomcdat 运行时产生的临时数据** 

- **webapps 专门用来存放部署的 Web 工程。** 

- **work 是 Tomcat 工作时的目录，用来存放 Tomcat 运行时 jsp 翻译为 Servlet 的源码，和 Session 钝化的目录。**

![image-20211016154723997](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211016154723997.png)

## day06

### servlet

**什么是** **Servlet** 

- **Servlet 是 JavaEE 规范之一。规范就是接口** 

- **Servlet 就 JavaWeb 三大组件之一。三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监听器。** 

- **Servlet 是运行在服务器上的一个 java 小程序，它可以接收客户端发送过来的请求，并响应数据给客户端。**

**第一个servlet程序**

```xml
<servlet>
    <!--servlet-name标签 Servlet程序起一个别名（一般是类名） -->
    <servlet-name>HelloServlet</servlet-name>
    <!--servlet-class是Servlet程序的全类名-->
    <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
</servlet>
<!--servlet-mapping标签给servlet程序配置访问地址-->
<servlet-mapping>
    <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
    <servlet-name>HelloServlet</servlet-name>
    <!--
        url-pattern标签配置访问地址                                     <br/>
           / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径          <br/>
           /hello 表示地址为：http://ip:port/工程路径/hello              <br/>
    -->
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

```java
package com.atguigu.servlet;

import javax.servlet.*;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-16-17:49
 */
public class HelloServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println(1234563);
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}

```

- **1、执行 Servlet 构造器方法** 

- **2、执行 init 初始化方法 (第一、二步，是在第一次访问，的时候创建 Servlet 程序会调用)**

- **3、执行 service 方法 (第三步，每次访问都会调用)**

- **4、执行 destroy 销毁方法 (第四步，在 web 工程停止的时候调用)**

#### servlet的请求处理

```java
package com.atguigu.servlet;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-16-17:49
 */
public class HelloServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        String method = httpServletRequest.getMethod();
        if("post".equalsIgnoreCase(method)) {
            doPost();
        } else if("get".equalsIgnoreCase(method)) {
            doGet();
        }
    }

    public void doPost() {
        System.out.println("post");
    }

    public void doGet() {
        System.out.println("get");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

#### HttpServlet子类

**一般在实际项目开发中，都是使用继承 HttpServlet 类的方式去实现 Servlet 程序。** 

- **1、编写一个类去继承 HttpServlet 类** 

- **2、根据业务需要重写 doGet 或 doPost 方法** 

- **3、到 web.xml 中的配置 Servlet 程序的访问地址**

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-16-20:46
 */
public class HelloServlet2 extends HttpServlet {
    /**
     * get请求的时候调用
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("GEt");
    }

    /**
     * post请求的时候调用
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <!--servlet-name标签 Servlet程序起一个别名（一般是类名） -->
        <servlet-name>HelloServlet</servlet-name>
        <!--servlet-class是Servlet程序的全类名-->
        <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
    </servlet>
    <!--servlet-mapping标签给servlet程序配置访问地址-->
    <servlet-mapping>
        <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
        <servlet-name>HelloServlet</servlet-name>
        <!--
            url-pattern标签配置访问地址                                     <br/>
               / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径          <br/>
               /hello 表示地址为：http://ip:port/工程路径/hello              <br/>
        -->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

    <servlet>
        <!--servlet-name标签 Servlet程序起一个别名（一般是类名） -->
        <servlet-name>HelloServlet2</servlet-name>
        <!--servlet-class是Servlet程序的全类名-->
        <servlet-class>com.atguigu.servlet.HelloServlet2</servlet-class>
    </servlet>
    <!--servlet-mapping标签给servlet程序配置访问地址-->
    <servlet-mapping>
        <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
        <servlet-name>HelloServlet2</servlet-name>
        <!--
            url-pattern标签配置访问地址                                     <br/>
               / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径          <br/>
               /hello 表示地址为：http://ip:port/工程路径/hello              <br/>
        -->
        <url-pattern>/hello2</url-pattern>
    </servlet-mapping>
</web-app>
```

**新版的idea可以自动创建servlet**

https://blog.csdn.net/qq_50253227/article/details/116258368

#### ServletConfig

**ServletConfig 类从类名上来看，就知道是 Servlet 程序的配置信息类。** 

**Servlet 程序和 ServletConfig 对象都是由 Tomcat 负责创建，我们负责使用。** 

**Servlet 程序默认是第一次访问的时候创建，ServletConfig 是每个 Servlet 程序创建时，就创建一个对应的 ServletConfig 对** 

**象。**

**作用：**

**1、可以获取 Servlet 程序的别名 servlet-name 的值** 

**2、获取初始化参数 init-param** 

**3、获取 ServletContext 对象** 

```java
package com.atguigu.servlet;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

public class HelloServlet implements Servlet {

    public HelloServlet() {
        System.out.println("1 构造器方法");
    }

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("2 init初始化方法");

//        1、可以获取Servlet程序的别名servlet-name的值
        System.out.println("HelloServlet程序的别名是:" + servletConfig.getServletName());
//        2、获取初始化参数init-param
        System.out.println("初始化参数username的值是;" + servletConfig.getInitParameter("username"));
        System.out.println("初始化参数url的值是;" + servletConfig.getInitParameter("url"));
//        3、获取ServletContext对象
        System.out.println(servletConfig.getServletContext());
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    /**
     * service方法是专门用来处理请求和响应的
     * @param servletRequest
     * @param servletResponse
     * @throws ServletException
     * @throws IOException
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("3 service === Hello Servlet 被访问了");
        // 类型转换（因为它有getMethod()方法）
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        // 获取请求的方式
        String method = httpServletRequest.getMethod();

        if ("GET".equals(method)) {
            doGet();
        } else if ("POST".equals(method)) {
           doPost();
        }

    }

    /**
     * 做get请求的操作
     */
    public void doGet(){
        System.out.println("get请求");
        System.out.println("get请求");
    }
    /**
     * 做post请求的操作
     */
    public void doPost(){
        System.out.println("post请求");
        System.out.println("post请求");
    }


    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {
        System.out.println("4 . destroy销毁方法");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <!--servlet-name标签 Servlet程序起一个别名（一般是类名） -->
        <servlet-name>HelloServlet</servlet-name>
        <!--servlet-class是Servlet程序的全类名-->
        <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
        <init-param>
            <param-name>username</param-name>
            <param-value>root</param-value>
        </init-param>
        <init-param>
            <param-name>url</param-name>
            <param-value>jdbc:mysql://localhost:3306/test</param-value>
        </init-param>
    </servlet>
    <!--servlet-mapping标签给servlet程序配置访问地址-->
    <servlet-mapping>
        <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
        <servlet-name>HelloServlet</servlet-name>
        <!--
            url-pattern标签配置访问地址                                     <br/>
               / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径          <br/>
               /hello 表示地址为：http://ip:port/工程路径/hello              <br/>
        -->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

    <servlet>
        <!--servlet-name标签 Servlet程序起一个别名（一般是类名） -->
        <servlet-name>HelloServlet2</servlet-name>
        <!--servlet-class是Servlet程序的全类名-->
        <servlet-class>com.atguigu.servlet.HelloServlet2</servlet-class>
    </servlet>
    <servlet>
        <servlet-name>HelloServlet3</servlet-name>
        <servlet-class>com.atguigu.servlet.HelloServlet3</servlet-class>
    </servlet>
    <!--servlet-mapping标签给servlet程序配置访问地址-->
    <servlet-mapping>
        <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
        <servlet-name>HelloServlet2</servlet-name>
        <!--
            url-pattern标签配置访问地址                                     <br/>
               / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径          <br/>
               /hello 表示地址为：http://ip:port/工程路径/hello              <br/>
        -->
        <url-pattern>/hello2</url-pattern>
    </servlet-mapping>
    <servlet-mapping>
        <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
        <servlet-name>HelloServlet3</servlet-name>
        <!--
            url-pattern标签配置访问地址                                     <br/>
               / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径          <br/>
               /hello 表示地址为：http://ip:port/工程路径/hello              <br/>
        -->
        <url-pattern>/hello3</url-pattern>
    </servlet-mapping>
</web-app>
```

#### ServletContext

**1、ServletContext 是一个接口，它表示 Servlet 上下文对象** 

**2、一个 web 工程，只有一个 ServletContext 对象实例。** 

**3、ServletContext 对象是一个域对象。** 

**4、ServletContext 是在 web 工程部署启动的时候创建。在 web 工程停止的时候销毁。** 

**什么是域对象?** 

**域对象，是可以像 Map 一样存取数据的对象，叫域对象。 这里的域指的是存取数据的操作范围，整个 web 工程。** 

**ServletContext的作用**

**1、获取 web.xml 中配置的上下文参数 context-param** 

**2、获取当前的工程路径，格式: /工程路径** 

**3、获取工程部署后在服务器硬盘上的绝对路径** 

**4、像 Map 一样存取数据**

```java
public class ContextServlet1 extends HttpServlet { 
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { 
        // 获取 ServletContext 对象 
        ServletContext context = getServletContext(); System.out.println(context); 
        System.out.println("保存之前: Context1 获取 key1 的值是:"+ context.getAttribute("key1")); 
        context.setAttribute("key1", "value1"); 
        System.out.println("Context1 中获取域数据 key1 的值是:"+ context.getAttribute("key1")); 
    } 
}
```

#### http

**i. GET** **请求** 

**1、请求行** 

**(1) 请求的方式 GET** 

**(2) 请求的资源路径[+?+请求参数]** 

**(3) 请求的协议的版本号 HTTP/1.1** 

**2、请求头** 

**key : value 组成不同的键值对，表示不同的含义。**

![image-20211019013049556](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211019013049556.png)

**ii. POST** **请求** 

**1、请求行**

**(1) 请求的方式 POST** 

**(2) 请求的资源路径[+?+请求参数]** 

**(3) 请求的协议的版本号 HTTP/1.1** 

**2、请求头**

**1) key : value 不同的请求头，有不同的含义** 

**空行** 

**3、请求体 ===>>> 就是发送给服务器的数据**

![image-20211019013612784](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211019013612784.png)

**iii.** **常用请求头的说明** 

**Accept: 表示客户端可以接收的数据类型** 

**Accpet-Languege: 表示客户端可以接收的语言类型** 

**User-Agent: 表示客户端浏览器的信息** 

**Host： 表示请求时的服务器 ip 和端口号** 

**iv.** **哪些是** **GET** **请求，哪些是** **POST** **请求** 

​	**GET 请求有哪些：** 

​	**1、form 标签 method=get** 

​	**2、a 标签** 

​	**3、link 标签引入 css** 

​	**4、Script 标签引入 js 文件** 

​	**5、img 标签引入图片** 

​	**6、iframe 引入 html 页面** 

​	**7、在浏览器地址栏中输入地址后敲回车** 

​	**POST 请求有哪些：** 

​	**8、form 标签 method=post** 

**响应的** **HTTP** **协议格式** 

**1、响应行** 

**(1) 响应的协议和版本号** 

**(2) 响应状态码** 

**(3) 响应状态描述符** 

**2、响应头** 

**(1) key : value 不同的响应头，有其不同含义** 

**空行** 

**3、响应体 ---->>> 就是回传给客户端的数据**

![image-20211019014006536](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211019014006536.png)

**常用的响应码说明** 

**200 表示请求成功** 

**302 表示请求重定向（明天讲）** 

**404 表示请求服务器已经收到了，但是你要的数据不存在（请求地址错误）** 

**500 表示服务器已经收到请求，但是服务器内部错误（代码错误）**

#### MIME类型

**MIME 是 HTTP 协议中数据类型。** 

**MIME 的英文全称是"Multipurpose Internet Mail Extensions" 多功能 Internet 邮件扩充服务。MIME 类型的格式是“大类型/小** 

**类型”，并与某一种文件的扩展名相对应。** 

![image-20211020004852187](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211020004852187.png)

## day07

### servlet

#### HttpServletRequest

**i. getRequestURI() 获取请求的资源路径** 

**ii. getRequestURL() 获取请求的统一资源定位符（绝对路径）** 

**iii. getRemoteHost() 获取客户端的 ip 地址** 

**iv. getHeader() 获取请求头** 

**v. getParameter() 获取请求的参数** 

**vi. getParameterValues() 获取请求的参数（多个值的时候使用）** 

**vii. getMethod() 获取请求的方式 GET 或 POST** 

**viii. setAttribute(key, value); 设置域数据** 

**ix. getAttribute(key); 获取域数据** 

**x. getRequestDispatcher() 获取请求转发对象**

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class RequestAPIServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //        i.getRequestURI()					获取请求的资源路径
        System.out.println("URI => " + req.getRequestURI());
//        ii.getRequestURL()					获取请求的统一资源定位符（绝对路径）
        System.out.println("URL => " + req.getRequestURL());
//        iii.getRemoteHost()				获取客户端的ip地址
        /**
         * 在IDEA中，使用localhost访问时，得到的客户端 ip 地址是 ===>>> 127.0.0.1<br/>
         * 在IDEA中，使用127.0.0.1访问时，得到的客户端 ip 地址是 ===>>> 127.0.0.1<br/>
         * 在IDEA中，使用 真实ip 访问时，得到的客户端 ip 地址是 ===>>> 真实的客户端 ip 地址<br/>
         */
        System.out.println("客户端 ip地址 => " + req.getRemoteHost());
//        iv.getHeader()						获取请求头
        System.out.println("请求头User-Agent ==>> " + req.getHeader("User-Agent"));
//        vii.getMethod()					获取请求的方式GET或POST
        System.out.println( "请求的方式 ==>> " + req.getMethod() );
    }
}
```

**获取请求参数**

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Arrays;

public class ParameterServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("-------------doGet------------");

        // 获取请求参数
        String username = req.getParameter("username");

        //1 先以iso8859-1进行编码
        //2 再以utf-8进行解码
        username = new String(username.getBytes("iso-8859-1"), "UTF-8");

        String password = req.getParameter("password");
        String[] hobby = req.getParameterValues("hobby");

        System.out.println("用户名：" + username);
        System.out.println("密码：" + password);
        System.out.println("兴趣爱好：" + Arrays.asList(hobby));
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置请求体的字符集为UTF-8，从而解决post请求的中文乱码问题
        // 也要在获取请求参数之前调用才有效
        req.setCharacterEncoding("UTF-8");

        System.out.println("-------------doPost------------");
        // 获取请求参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobby = req.getParameterValues("hobby");

        System.out.println("用户名：" + username);
        System.out.println("密码：" + password);
        System.out.println("兴趣爱好：" + Arrays.asList(hobby));
    }
}
```

**请求的转发** 

**什么是请求的转发?** 

**请求转发是指，服务器收到请求后，从一次资源跳转到另一个资源的操作叫请求转发。**

![image-20211020140751244](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211020140751244.png)

**servlet1**

```java
package com.atguigu.servlet;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class Servlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        // 获取请求的参数（办事的材料）查看
        String username = req.getParameter("username");
        System.out.println("在Servlet1（柜台1）中查看参数（材料）：" + username);

        // 给材料 盖一个章，并传递到Servlet2（柜台 2）去查看
        req.setAttribute("key1","柜台1的章");

        // 问路：Servlet2（柜台 2）怎么走
        /**
         * 请求转发必须要以斜杠打头，/ 斜杠表示地址为：http://ip:port/工程名/ , 映射到IDEA代码的web目录<br/>
         *
         */
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("/servlet2");
//        RequestDispatcher requestDispatcher = req.getRequestDispatcher("http://www.baidu.com");

        // 走向Sevlet2（柜台 2）
        requestDispatcher.forward(req,resp);

    }
}
```

**servlet2**

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class Servlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取请求的参数（办事的材料）查看
        String username = req.getParameter("username");
        System.out.println("在Servlet2（柜台2）中查看参数（材料）：" + username);

        // 查看 柜台1 是否有盖章
        Object key1 = req.getAttribute("key1");
        System.out.println("柜台1是否有章：" + key1);

        // 处理自己的业务
        System.out.println("Servlet2 处理自己的业务 ");
    }
}
```

#### base标签的作用

![image-20211020141824728](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211020141824728.png)

```html
<!DOCTYPE html> 
<html lang="zh_CN"> 
    <head> 
        <meta charset="UTF-8"> 
        <title>Title</title> 
        <!--base 标签设置页面相对路径工作时参照的地址 href 属性就是参数的地址值 --> 
        <base href="http://localhost:8080/07_servlet/a/b/"> 
    </head> 
    <body> 这是 a 下的 b 下的 c.html 页面<br/> 
        <a href="../../index.html">跳回首页</a>
        <br/> 
    </body> 
</html>
```

**web** **中** **/** **斜杠的不同意义** 

**在 web 中 / 斜杠 是一种绝对路径。** 

**/ 斜杠** **如果被浏览器解析，得到的地址是：http://ip:port/** 

**/ 斜杠** **如果被服务器解析，得到的地址是：http://ip:port/工程路径** 

**1、<url-pattern>/servlet1</url-pattern>** 

**2、servletContext.getRealPath(“/”);** 

**3、request.getRequestDispatcher(“/”);** 

**特殊情况： response.sendRediect(“/”);** 

**把斜杠发送给浏览器解析。得到 http://ip:port/**



#### HttpServletResponse

**HttpServletResponse 类和 HttpServletRequest 类一样。每次请求进来，Tomcat 服务器都会创建一个 Response 对象传** 

**递给 Servlet 程序去使用。HttpServletRequest 表示请求过来的信息，HttpServletResponse 表示所有响应的信息，** 

**我们如果需要设置返回给客户端的信息，都可以通过 HttpServletResponse 对象来进行设置**

- **字节流和字符流**

**字节流：** **getOutputStream();** **常用于下载（传递二进制数据）** 

**字符流：** **getWriter();** **常用于回传字符串（常用）** 

**两个流同时只能使用一个。** 

**使用了字节流，就不能再使用字符流，反之亦然，否则就会报错**

- **给客户端回传数据**

```java
public class ResponseIOServlet extends HttpServlet 
{ 
    @Override protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException { 
        // 要求 ： 往客户端回传 字符串 数据。 
        PrintWriter writer = resp.getWriter(); 
        writer.write("response's content!!!"); 
    } 
}
```

- **解决中文乱码问题**

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * @author yh
 * @create 2021-10-20-14:29
 */
public class ResponseIOServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 它会同时设置服务器和客户端都使用 UTF-8 字符集，还设置了响应头 
        // 此方法一定要在获取流对象之前调用才有效 
        resp.setContentType("text/html; charset=UTF-8");
        PrintWriter writer = resp.getWriter();
        writer.write("就这");
    }
}
```

- **请求重定向**

**请求重定向，是指客户端给服务器发请求，然后服务器告诉客户端说。我给你一些地址。你去新地址访问。叫请求** 

**重定向（因为之前的地址可能已经被废弃）。**

![image-20211020145432144](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211020145432144.png)

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-20-14:44
 */
public class Response1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.sendRedirect("http://localhost:8080");
    }
}
```



### 项目工程目录

**web 层              com.atguigu.web/servlet/controller** 

**service 层         com.atguigu.service Service 接口包** 

​				           **com.atguigu.service.impl Service 接口实现类** 

**dao 持久层        com.atguigu.dao Dao 接口包** 

​					       **com.atguigu.dao.impl Dao 接口实现类** 

**实体 bean 对象 com.atguigu.pojo/entity/domain/bean JavaBean 类** 

**测试包 com.atguigu.test/junit** 

**工具类 com.atguigu.utils**

![image-20211020185545158](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211020185545158.png)

**javaEEMVC层的步骤**

- **1.创建数据库的表**
- **2.在pojo下创建对应的User类**

```java
package com.atguigu.pojo;

/**
 * @author yh
 * @create 2021-10-20-15:35
 */
public class User {
    private Integer id;
    private String username;
    private String password;
    private String email;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public User() {
    }

    public User(Integer id, String username, String password, String email) {
        this.id = id;
        this.username = username;
        this.password = password;
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
}
```

- **3.在uitls包下进行JdbcUtils工具类的编写(利用工具包)**

```java
package com.atguigu.utils;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.pool.DruidDataSourceFactory;

import java.io.InputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

/**
 * @author yh
 * @create 2021-10-20-15:41
 */
public class JdbcUtils {
    private static DruidDataSource dataSource;
    static {
        try {
            Properties properties = new Properties();
            InputStream inputStream = JdbcUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
            properties.load(inputStream);
            dataSource = (DruidDataSource) DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取数据库连接池
     * @return
     */
    public static Connection getConnection() {
        Connection conn = null;
        try {
            conn = dataSource.getConnection();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        return conn;
    }

    /**
     * 关闭连接
     * @param conn
     */
    public static void close(Connection conn) {
        if(conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

- **4.在dao.impl包下创建BaseDao(利用工具包)**

```java
package com.atguigu.dao.impl;

import com.atguigu.utils.JdbcUtils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;

import java.io.ObjectStreamException;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-20-16:09
 */
public abstract class BaseDao {
    private QueryRunner queryRunner = new QueryRunner();

    public int update(String sql,Object ...args) {
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.update(conn,sql,args);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JdbcUtils.close(conn);
        }
        return -1;
    }

    public <T> T queryForOne(Class<T> type,String sql,Object ...args) {
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.query(conn,sql,new BeanHandler<T>(type),args);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JdbcUtils.close(conn);
        }
        return null;
    }

    public <T> List<T> queryForList(Class<T> type,String sql,Object ...args) {
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.query(conn,sql,new BeanListHandler<T>(type),args);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JdbcUtils.close(conn);
        }
        return null;
    }

    public Object queryForSingleValue(String sql,Object ...args) {
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.query(conn,sql,new ScalarHandler(),args);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JdbcUtils.close(conn);
        }
        return null;
    }
}

```

- **5.在dao.impl下创建UserDao接口和实现类UserDaoImpl**

```java
package com.atguigu.dao.impl;

import com.atguigu.pojo.User;

/**
 * @author yh
 * @create 2021-10-20-16:31
 */
public interface UserDao {
    public User queryUserByUsername(String username);
    public int saveUser(User user);
    public User queryUserByUsernameAndPassword(String username,String password);
}
```

```java
package com.atguigu.dao.impl;

import com.atguigu.pojo.User;

/**
 * @author yh
 * @create 2021-10-20-16:33
 */
public class UserDaoImpl extends BaseDao implements UserDao{
    @Override
    public User queryUserByUsername(String username) {
        String sql = "select id,username,password,email from t_user where username = ?";
        return queryForOne(User.class,sql,username);
    }

    @Override
    public int saveUser(User user) {
        String sql = "insert into t_user(username,password,email) values(?,?,?)";
        return update(sql,user.getUsername(),user.getPassword(),user.getEmail());
    }

    @Override
    public User queryUserByUsernameAndPassword(String username, String password) {
        String sql = "select id,username,password,email from t_user where username = ? and password = ?";
        return queryForOne(User.class,sql,username,password);
    }
}
```

- **6.进行UserDaoImpl的测试**

```java
package com.atguigu.dao.impl;

import com.atguigu.pojo.User;
import org.junit.Test;

import static org.junit.Assert.*;

/**
 * @author yh
 * @create 2021-10-20-16:38
 */
public class UserDaoTest {

    @Test
    public void queryUserByUsername() {
        UserDao userDao = new UserDaoImpl();
        System.out.println(userDao.queryUserByUsername("admin"));
    }

    @Test
    public void saveUser() {
        UserDao userDao = new UserDaoImpl();
        System.out.println(userDao.saveUser(new User(null, "admin2", "123456", "12322@qq.com")));
    }

    @Test
    public void queryUserByUsernameAndPassword() {
        UserDao userDao = new UserDaoImpl();
        System.out.println(userDao.queryUserByUsernameAndPassword("admin","admin"));
    }
}
```

- **7.在service.impl包下创建UserService的接口，并编写实现类UserServiceImpl**

```java
package com.atguigu.service.impl;

import com.atguigu.pojo.User;

/**
 * @author yh
 * @create 2021-10-20-16:52
 */
public interface UserService {

    /**
     * 注册用户
     * @param user
     */
    public void registerUser(User user);

    /**
     * 登录
     * @param user
     * @return
     */
    public User login(User user);

    /**
     * 检查用户名是否可用
     * @param username
     * @return
     */
    public boolean existsUsername(String username);
}
```

```java
package com.atguigu.service.impl;

import com.atguigu.dao.impl.UserDao;
import com.atguigu.dao.impl.UserDaoImpl;
import com.atguigu.pojo.User;

/**
 * @author yh
 * @create 2021-10-20-16:54
 */
public class UserServiceImpl implements UserService{
    private UserDao userDao = new UserDaoImpl();
    @Override
    public void registerUser(User user) {
        userDao.saveUser(user);
    }

    @Override
    public User login(User user) {
        return userDao.queryUserByUsernameAndPassword(user.getUsername(),user.getPassword());
    }

    @Override
    public boolean existsUsername(String username) {
        if(userDao.queryUserByUsername(username) == null) {
            return false;
        }
        return true;
    }
}
```

**8.UserService的测试**

```java
package com.atguigu.service.impl;

import com.atguigu.pojo.User;
import org.junit.Test;

import static org.junit.Assert.*;

/**
 * @author yh
 * @create 2021-10-20-16:57
 */
public class UserServiceTest {

    UserService userService = new UserServiceImpl();
    @Test
    public void registerUser() {
        userService.registerUser(new User(null,"admin3","admin",null));
    }

    @Test
    public void login() {
        System.out.println(userService.login(new User(null,"admin","admin",null)));
    }

    @Test
    public void existsUsername() {
        System.out.println(userService.existsUsername("admin"));
    }
}
```

- **9.在web包下创建servlet服务**

```java
package com.atguigu.web;

import com.atguigu.pojo.User;
import com.atguigu.service.impl.UserService;
import com.atguigu.service.impl.UserServiceImpl;
import com.sun.net.httpserver.HttpServer;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-20-19:18
 */
public class RegistServlet extends HttpServlet {
    private UserService userService = new UserServiceImpl();
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String email = req.getParameter("email");
        String code = req.getParameter("code");

        if("abcde".equalsIgnoreCase(code)) {
            if(userService.existsUsername(username)) {
                System.out.println("用户名已存在！");
                req.getRequestDispatcher("/pages/user/regist.html").forward(req,resp);
            } else {
                userService.registerUser(new User(null,username,password,email));
                req.getRequestDispatcher("/pages/user/regist_success.html").forward(req,resp);
            }
        } else {
            System.out.println("验证码错误");
            req.getRequestDispatcher("/pages/user/regist.html").forward(req,resp);
        }
    }
}
```



### IDEA调试

![image-20211020195032216](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211020195032216.png)

![image-20211020195049862](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211020195049862.png)

![image-20211020195059859](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211020195059859.png)

![image-20211020195713747](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211020195713747.png)

## day08

### Listener监听器

**1、Listener 监听器它是 JavaWeb 的三大组件之一。JavaWeb 的三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监** 

**听器。**

**2、Listener 它是 JavaEE 的规范，就是接口** 

**3、监听器的作用是，监听某种事物的变化。然后通过回调函数，反馈给客户（程序）去做一些相应的处理。** 

**ServletContextListener 它可以监听 ServletContext 对象的创建和销毁。** 

**ServletContext 对象在 web 工程启动的时候创建，在 web 工程停止的时候销毁。** 

**监听到创建和销毁之后都会分别调用 ServletContextListener 监听器的方法反馈。**



**如何使用 ServletContextListener 监听器监听 ServletContext 对象。** 

**使用步骤如下：** 

**1、编写一个类去实现 ServletContextListener** 

**2、实现其两个回调方法** 

**3、到 web.xml 中去配置监听器**

```java
package com.atguigu.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

/**
 * @author yh
 * @create 2021-10-21-20:27
 */
public class myServletContextListenerImpl implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("create");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("delete");
    }
}
```

```xml
<listener>
     <listener-class>com.atguigu.listener.myServletContextListenerImpl</listener-class>
</listener>
```

## day09

### 文件上传

**1、要有一个 form 标签，method=post 请求** 

**2、form 标签的 encType 属性值必须为 multipart/form-data 值** 

**3、在 form 标签中使用 input type=file 添加上传的文件** 

**4、编写服务器代码（Servlet 程序）接收，处理上传的数据。** 

**encType=multipart/form-data 表示提交的数据，以多段（每一个表单项一个数据段）的形式进行拼接，然后以二进制流的形式发送给服务器**

![image-20211022002306867](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211022002306867.png)



**commons-fileupload.jar 常用 API 介绍说明** 

**commons-fileupload.jar 需要依赖 commons-io.jar 这个包，所以两个包我们都要引入。** 

**第一步，就是需要导入两个 jar 包：** 

**commons-fileupload-1.2.1.jar** 

**commons-io-1.4.jar** 

**commons-fileupload.jar 和 commons-io.jar 包中，我们常用的类有哪些？** 

**ServletFileUpload 类，用于解析上传的数据。** 

**FileItem 类，表示每一个表单项。** 

**boolean ServletFileUpload.*isMultipartContent*(HttpServletRequest request);** 

**判断当前上传的数据格式是否是多段的格式。** 

**public List<FileItem> parseRequest(HttpServletRequest request)** 

**解析上传的数据** 

**boolean FileItem.isFormField()** 

**判断当前这个表单项，是否是普通的表单项。还是上传的文件类型。** 

**true 表示普通类型的表单项** 

**false 表示上传的文件类型** 

**String FileItem.getFieldName()** 

**获取表单项的 name 属性值String FileItem.getString()** 

**获取当前表单项的值。** 

**String FileItem.getName();** 

**获取上传的文件名** 

**void FileItem.write( file );** 

**将上传的文件写到 参数 file 所指向抽硬盘位置 。**

```java
package com.atguigu.servlet;


import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileItemFactory;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.util.List;

public class UploadServlet extends HttpServlet {
    /**
     * 用来处理上传的数据
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //1 先判断上传的数据是否多段数据（只有是多段的数据，才是文件上传的）
        if (ServletFileUpload.isMultipartContent(req)) {
//           创建FileItemFactory工厂实现类
            FileItemFactory fileItemFactory = new DiskFileItemFactory();
            // 创建用于解析上传数据的工具类ServletFileUpload类
            ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);
            try {
                // 解析上传的数据，得到每一个表单项FileItem
                List<FileItem> list = servletFileUpload.parseRequest(req);
                // 循环判断，每一个表单项，是普通类型，还是上传的文件
                for (FileItem fileItem : list) {

                    if (fileItem.isFormField()) {
                        // 普通表单项

                        System.out.println("表单项的name属性值：" + fileItem.getFieldName());
                        // 参数UTF-8.解决乱码问题
                        System.out.println("表单项的value属性值：" + fileItem.getString("UTF-8"));

                    } else {
                        // 上传的文件
                        System.out.println("表单项的name属性值：" + fileItem.getFieldName());
                        System.out.println("上传的文件名：" + fileItem.getName());

                        fileItem.write(new File("e:\\" + fileItem.getName()));
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }

}
```

### 文件下载

**下载的常用** **API** **说明：** 

**response.getOutputStream();** 

**servletContext.getResourceAsStream();** 

**servletContext.getMimeType();** 

**response.setContentType();** 

**response.setHeader("Content-Disposition", "attachment; fileName=1.jpg");** 

**这个响应头告诉浏览器。这是需要下载的。而 attachment 表示附件，也就是下载的一个文件。fileName=后面，** **表示下载的文件名。** 

**完成上面的两个步骤，下载文件是没问题了。但是如果我们要下载的文件是中文名的话。你会发现，下载无法正确显示出正确的中文名。** 

**原因是在响应头中，不能包含有中文字符，只能包含 ASCII 码。** 

```java
package com.atguigu.servlet;

import org.apache.commons.io.IOUtils;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextListener;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

/**
 * @author yh
 * @create 2021-10-22-9:57
 */
public class download extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1、获取要下载的文件名
        String downloadFileName = "girl.jpg";
        // 2、读取要下载的文件内容 (通过ServletContext对象可以读取)
        ServletContext servletContext = getServletContext();
        // 3、获取要下载的文件类型
        String mimeType = servletContext.getMimeType("/file/" + downloadFileName);
        System.out.println(mimeType);
        //4、在回传前，通过响应头告诉客户端返回的数据类型
        resp.setContentType(mimeType);
        // 5、还要告诉客户端收到的数据是用于下载使用（还是使用响应头）
        // Content-Disposition响应头，表示收到的数据怎么处理
        // attachment表示附件，表示下载使用
        // filename= 表示指定下载的文件名
        // url编码是把汉字转换成为%xx%xx的格式
        resp.setHeader("Content-Disposition","attachment; filename=" + downloadFileName);
        InputStream resourceAsStream = servletContext.getResourceAsStream("/file/" + downloadFileName);
        // 获取响应的输出流
        OutputStream outputStream = resp.getOutputStream();
        // 6、把下载的文件内容回传给客户端
        // 读取输入流中全部的数据，复制给输出流，输出给客户端
        IOUtils.copy(resourceAsStream,outputStream);
    }
}
```

### base64

```java
package com.atguigu.base64;

import sun.misc.BASE64Decoder;
import sun.misc.BASE64Encoder;

public class Base64Test {
    public static void main(String[] args) throws Exception {
        String content = "这是需要Base64编码的内容";
        // 创建一个Base64编码器
        BASE64Encoder base64Encoder = new BASE64Encoder();
        // 执行Base64编码操作
        String encodedString = base64Encoder.encode(content.getBytes("UTF-8"));

        System.out.println( encodedString );
        // 创建Base64解码器
        BASE64Decoder base64Decoder = new BASE64Decoder();
        // 解码操作
        byte[] bytes = base64Decoder.decodeBuffer(encodedString);

        String str = new String(bytes, "UTF-8");

        System.out.println(str);
    }
}
```

**兼容火狐浏览器**

```java
package com.atguigu.servlet;

import org.apache.commons.io.IOUtils;
import sun.misc.BASE64Encoder;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.URLEncoder;

public class Download extends HttpServlet {


    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        1、获取要下载的文件名
        String downloadFileName = "2.jpg";
//        2、读取要下载的文件内容 (通过ServletContext对象可以读取)
        ServletContext servletContext = getServletContext();
        // 获取要下载的文件类型
        String mimeType = servletContext.getMimeType("/file/" + downloadFileName);
        System.out.println("下载的文件类型：" + mimeType);
//        4、在回传前，通过响应头告诉客户端返回的数据类型
        resp.setContentType(mimeType);
//        5、还要告诉客户端收到的数据是用于下载使用（还是使用响应头）
        // Content-Disposition响应头，表示收到的数据怎么处理
        // attachment表示附件，表示下载使用
        // filename= 表示指定下载的文件名
        // url编码是把汉字转换成为%xx%xx的格式
        if (req.getHeader("User-Agent").contains("Firefox")) {
            // 如果是火狐浏览器使用Base64编码
            resp.setHeader("Content-Disposition", "attachment; filename==?UTF-8?B?" + new BASE64Encoder().encode("中国.jpg".getBytes("UTF-8")) + "?=");
        } else {
            // 如果不是火狐，是IE或谷歌，使用URL编码操作
            resp.setHeader("Content-Disposition", "attachment; filename=" + URLEncoder.encode("中国.jpg", "UTF-8"));
        }
        /**
         * /斜杠被服务器解析表示地址为http://ip:prot/工程名/  映射 到代码的Web目录
         */
        InputStream resourceAsStream = servletContext.getResourceAsStream("/file/" + downloadFileName);
        // 获取响应的输出流
        OutputStream outputStream = resp.getOutputStream();
        //        3、把下载的文件内容回传给客户端
        // 读取输入流中全部的数据，复制给输出流，输出给客户端
        IOUtils.copy(resourceAsStream, outputStream);
    }
}
```

## day10

### 书城项目第三阶段

**页面** **jsp** **动态化** 

**1、在 html 页面顶行添加 page 指令。** 

**2、修改文件后缀名为：.jsp** 

**3、使用 IDEA 搜索替换.html 为.jsp(快捷键：Ctrl+Shift+R)**

![image-20211022104721275](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211022104721275.png)

**代码优化**

**代码优化一：代码优化：合并** **LoginServlet** **和** **RegistServlet** **程序为** **UserServlet** **程序**

![image-20211022133824691](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211022133824691.png)

![image-20211022133841256](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211022133841256.png)

![image-20211022133856941](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211022133856941.png)

![image-20211022133910023](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211022133910023.png)

**优化代码二：使用反射优化大量** **else if** **代码：**

![image-20211022133937269](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211022133937269.png)

**抽取** **BaseServlet** **程序。**

![image-20211022134002486](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211022134002486.png)

**数据的封装和抽取** **BeanUtils** **的使用** 

**BeanUtils 工具类，它可以一次性的把所有请求的参数注入到 JavaBean 中。** 

**BeanUtils 工具类，经常用于把 Map 中的值注入到 JavaBean 中，或者是对象属性值的拷贝操作。** 

**BeanUtils 它不是 Jdk 的类。而是第三方的工具类。所以需要导包。** 

**1、导入需要的 jar 包：** 

**commons-beanutils-1.8.0.jar** 

**commons-logging-1.1.1.jar** 

**2、编写 WebUtils 工具类使用：** 

**WebUtils 工具类：** 

```java
import javax.servlet.http.HttpServletRequest;
import java.util.Map;

/**
* 把 Map 中的值注入到对应的 JavaBean 属性中。 
* @param value
* @param bean 
*/
public class WebUtils {
    public static <T> T copyParamToBean(Map value, T bean) {
        try {
            BeanUtils.populate(bean,value);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return bean;
    }
}
```

## day11

**MVC** **概念** 

**MVC** 

**全称：Model 模型、 View 视图、 Controller 控制器。** 

**MVC** 

**最早出现在 JavaEE 三层中的 Web 层，它可以有效的指导 Web 层的代码如何有效分离，单独工作。** 

**View 视图：只负责数据和界面的显示，不接受任何与显示数据无关的代码，便于程序员和美工的分工合作——** 

**JSP/HTML。**

**Controller 控制器：只负责接收请求，调用业务层的代码处理请求，然后派发页面，是一个“调度者”的角色——Servlet。** 

**转到某个页面。或者是重定向到某个页面。** 

**Model 模型：将与业务逻辑相关的数据封装为具体的 JavaBean 类，其中不掺杂任何与数据处理相关的代码——** 

**JavaBean/domain/entity/pojo。** 

**MVC 是一种思想** 

**MVC 的理念是将软件代码拆分成为组件，单独开发，组合使用（目的还是为了降低耦合度）。**

## day13

### Cookie

#### 创建cookie

![image-20211023173258595](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211023173258595.png)

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-23-16:09
 */
public class CookieServlet extends BaseServlet{
    protected void createCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1 创建 Cookie 对象
        Cookie cookie = new Cookie("key1","value1");
        //2 通知客户端保存 Cookie
        resp.addCookie(cookie);
        resp.getWriter().write("Cookie创建成功");
    }
}
```

#### 获取cookie

```java
package com.atguigu.servlet;

import utils.CookieUtils;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-23-16:09
 */
public class CookieServlet extends BaseServlet{
    protected void createCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("key1","value1");
        resp.addCookie(cookie);
        resp.getWriter().write("Cookie创建成功");
    }

    protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie[] cookies = req.getCookies();
        for (Cookie cookie : cookies) { 
            // getName 方法返回 Cookie 的 key（名） 
            // getValue 方法返回 Cookie 的 value 值 
            resp.getWriter().write("Cookie[" + cookie.getName() + "=" + cookie.getValue() + "] <br/>"); 		}
        Cookie key1 = CookieUtils.findCookie("key1", cookies);
        System.out.println(key1);
    }
}
```

**cookie工具类的封装**

```java
package utils;

import javax.servlet.http.Cookie;

/**
 * @author yh
 * @create 2021-10-23-17:40
 */
public class CookieUtils {
    public static Cookie findCookie(String name,Cookie[] cookies) {
        if(name == null || cookies == null || cookies.length == 0) {
            return null;
        }
        for (Cookie cookie : cookies) {
            if(name.equals(cookie.getName())) {
                return cookie;
            }
        }
        return null;
    }
}

```

#### 修改cookie

```java
package com.atguigu.servlet;

import utils.CookieUtils;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-23-16:09
 */
public class CookieServlet extends BaseServlet{
    protected void createCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("key1","value1");
        resp.addCookie(cookie);
        resp.getWriter().write("Cookie创建成功");
    }

    protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie[] cookies = req.getCookies();
        Cookie key1 = CookieUtils.findCookie("key1", cookies);
        System.out.println(key1);
    }

    protected void updateCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //方案一：
        // 1、先创建一个要修改的同名（指的就是 key）的 Cookie 对象
        // 2、在构造器，同时赋于新的 Cookie 值。
        // 3、调用 response.addCookie( Cookie );
//        Cookie cookie = new Cookie("key1","newValue1");
//        resp.addCookie(cookie);
//        resp.getWriter().write("Cookie修改成功");

        //方案二： 
        // 1、先查找到需要修改的 Cookie 对象 
        // 2、调用 setValue()方法赋于新的 Cookie 值。 
        // 3、调用 response.addCookie()通知客户端保存修改
        Cookie[] cookies = req.getCookies();
        Cookie cookie = CookieUtils.findCookie("key1", cookies);
        if(cookie != null) {
            cookie.setValue("newValue1");
            resp.addCookie(cookie);
        }
    }
}
```

#### Cookie生命控制

**Cookie 的生命控制指的是如何管理 Cookie 什么时候被销毁（删除）** 

**setMaxAge()** 

**正数，表示在指定的秒数后过期** 

**负数，表示浏览器一关，Cookie 就会被删除（默认值是-1）** 

**零，表示马上删除 Cookie** 

```java
package com.atguigu.servlet;

import utils.CookieUtils;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-23-16:09
 */
public class CookieServlet extends BaseServlet{
    protected void createCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("key1","value1");
        resp.addCookie(cookie);
        resp.getWriter().write("Cookie创建成功");
    }

    protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie[] cookies = req.getCookies();
        Cookie key1 = CookieUtils.findCookie("key1", cookies);
        System.out.println(key1);
    }

    protected void dafaultLife(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       Cookie cookie = new Cookie("defaultLife","defaultLife");
       cookie.setMaxAge(-1);
       resp.addCookie(cookie);
    }

    protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = CookieUtils.findCookie("key1",req.getCookies());
        if(cookie != null) {
            cookie.setMaxAge(0);
            resp.addCookie(cookie);
            resp.getWriter().write("删除成功");
        }
    }

    protected void life3600(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("life33600","life3600");
        cookie.setMaxAge(3600);
        resp.addCookie(cookie);
        resp.getWriter().write("一小时");
    }

    protected void updateCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //方案一：
        // 1、先创建一个要修改的同名（指的就是 key）的 Cookie 对象
        // 2、在构造器，同时赋于新的 Cookie 值。
        // 3、调用 response.addCookie( Cookie );
//        Cookie cookie = new Cookie("key1","newValue1");
//        resp.addCookie(cookie);
//        resp.getWriter().write("Cookie修改成功");

        //方案二：
        // 1、先查找到需要修改的 Cookie 对象
        // 2、调用 setValue()方法赋于新的 Cookie 值。
        // 3、调用 response.addCookie()通知客户端保存修改
        Cookie[] cookies = req.getCookies();
        Cookie cookie = CookieUtils.findCookie("key1", cookies);
        if(cookie != null) {
            cookie.setValue("newValue1");
            resp.addCookie(cookie);
        }
    }
}
```

#### Cookie设置路径

**Cookie 的 path 属性可以有效的过滤哪些 Cookie 可以发送给服务器。哪些不发。** 

**path 属性是通过请求的地址来进行有效的过滤。** 

**CookieA** **path=/工程路径** 

**CookieB** **path=/工程路径/abc** 

**请求地址如下：** 

**http://ip:port/工程路径/a.html** 

**CookieA 发送** 

**CookieB 不发送** 

**http://ip:port/工程路径/abc/a.html** 

**CookieA 发送** 

**CookieB 发送** 

```java
package com.atguigu.servlet;

import utils.CookieUtils;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-23-16:09
 */
public class CookieServlet extends BaseServlet{
    protected void testPath(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("path","path");
        cookie.setPath(req.getContextPath() + "/abc");
        resp.addCookie(cookie);
        resp.getWriter().write("创建了一个带有path路径的cookie");
    }
}
```



### Session

**什么是** **Session** **会话****?** 

**1、Session 就一个接口（HttpSession）。** 

**2、Session 就是会话。它是用来维护一个客户端和服务器之间关联的一种技术。** 

**3、每个客户端都有自己的一个 Session 会话。** 

**4、Session 会话中，我们经常用来保存用户登录之后的信息。**

**如何创建** **Session** **和获取****(id** **号****,****是否为新****)** 



**如何创建和获取 Session。它们的 API 是一样的。** 

**request.getSession()** 

**第一次调用是：创建 Session 会话** 

**之后调用都是：获取前面创建好的 Session 会话对象。** 

**isNew(); 判断到底是不是刚创建出来的（新的）** 

​	**true 表示刚创建** 

​	**false 表示获取之前创建** 

**每个会话都有一个身份证号。也就是 ID 值。而且这个 ID 是唯一的。** 

**getId() 得到 Session 的会话 id 值。** 



#### Session的创建和获取

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-23-19:54
 */
public class SessionServlet extends BaseServlet{
    protected void createOrGetSession(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        boolean isNew = session.isNew();
        String id = session.getId();
        resp.getWriter().write(id);
        resp.getWriter().write("isNew:" + isNew);
    }
}
```

#### Session属性的存和取

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-23-19:54
 */
public class SessionServlet extends BaseServlet{

    protected void setAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getSession().setAttribute("key1","value1");
    }

    protected void getAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Object key1 = req.getSession().getAttribute("key1");
        resp.getWriter().write("" + key1);
    }
}

```

#### Session生命周期控制

**public void setMaxInactiveInterval(int interval) 设置 Session 的超时时间（以秒为单位），超过指定的时长，Session就会被销毁。**

​	**值为正数的时候，设定 Session 的超时时长。** 

​	**负数表示永不超时（极少使用）** 

**public int getMaxInactiveInterval()获取 Session 的超时时间** 

**public void invalidate() 让当前 Session 会话马上超时无效。** 



**Session 默认的超时时长是多少！** 

**Session 默认的超时时间长为 30 分钟。** 

**因为在 Tomcat 服务器的配置文件 web.xml中默认有以下的配置，它就表示配置了当前 Tomcat 服务器下所有的 Session** 

**超时配置默认时长为：30 分钟。** 

**<session-config>** 

​	**<session-timeout>30</session-timeout>** 

**</session-config>**

**如果说。你希望你的 web 工程，默认的 Session 的超时时长为其他时长。你可以在你自己的 web.xml 配置文件中做** 

**以上相同的配置。就可以修改你的 web 工程所有 Seession 的默认超时时长。** 

**表示当前 web工程。创建出来 的所有Session默认是20分钟超时时**

![image-20211023205735443](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211023205735443.png)

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-23-19:54
 */
public class SessionServlet extends BaseServlet{

    protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        int maxInactiveInterval = req.getSession().getMaxInactiveInterval();
        resp.getWriter().write(maxInactiveInterval);
    }

    protected void life3(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        session.setMaxInactiveInterval(3);
        resp.getWriter().write("设置3秒");
    }

    protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        session.invalidate();
        resp.getWriter().write("session超时");
    }
}

```

#### 浏览器和Session之间关联的技术内幕

**Session 技术，底层其实是基于 Cookie 技术来实现的。**

![image-20211023211551719](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211023211551719.png)

### 表单重复提交问题

**表单重复提交有三种常见的情况：** 

**一：提交完表单。服务器使用请求转来进行页面跳转。这个时候，用户按下功能键 F5，就会发起最后一次的请。造成表单重复提交问题。解决方法：使用重定向来进行跳转** 

**二：用户正常提交服务器，但是由于网络延迟等原因，迟迟未收到服务器的响应，这个时候，用户以为提交失败，就会着急，然后多点了几次提交操作，也会造成表单重复提交。** 

**三：用户正常提交服务器。服务器也没有延迟，但是提交完成后，用户回退浏览器。重新提交。也会造成表单重复提交**

![image-20211023214407164](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211023214407164.png)



### **谷歌** kaptcha图片验证码的使用

**谷歌验证码 kaptcha 使用步骤如下：** 

**1、导入谷歌验证码的 jar 包** 

**kaptcha-2.3.2.jar** 

**2、在 web.xml 中去配置用于生成验证码的 Servlet 程序** 

```xml
<servlet>
    <servlet-name>KaptchaServlet</servlet-name>
    <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>KaptchaServlet</servlet-name>
    <url-pattern>/kaptcha.jpg</url-pattern>
</servlet-mapping>
```

**3.在表单中使用 img 标签去显示验证码图片并使用它**

```html
<form action="http://localhost:8080/tmp/registServlet" method="get"> 
    用户名：<input type="text" name="username" > <br> 
    验证码：<input type="text" style="width: 80px;" name="code"> 
    <img src="http://localhost:8080/tmp/kaptcha.jpg" alt="" style="width: 100px; height: 28px;"> <br> 		<input type="submit" value="登录"> 
</form>
```

**4、在服务器获取谷歌生成的验证码和客户端发送过来的验证码比较使用。**

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

import static com.google.code.kaptcha.Constants.KAPTCHA_SESSION_KEY;

/**
 * @author yh
 * @create 2021-10-23-19:34
 */
public class LoginServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String token = (String) req.getSession().getAttribute(KAPTCHA_SESSION_KEY);
        req.getSession().removeAttribute(KAPTCHA_SESSION_KEY);
        String code = req.getParameter("code");
        if(token != null && token.equalsIgnoreCase(code)) {
            System.out.println("保存到数据库：" + username); 
            resp.sendRedirect(req.getContextPath() + "/ok.jsp");
        } else {
            System.out.println("请不要重复提交表单");
        }
    }
}
```

## day15

### FIlter过滤器

**1、Filter 过滤器它是 JavaWeb 的三大组件之一。三大组件分别是：Servlet 程序、Listener 监听器、Filter 过滤器** 

**2、Filter 过滤器它是 JavaEE 的规范。也就是接口** 

**3、Filter 过滤器它的作用是：拦截请求，过滤响应。** 

**拦截请求常见的应用场景有：** 

**1、权限检查** 

**2、日记操作** 

**3、事务管理** 

**……等等** 

![image-20211024172146151](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211024172146151.png)



**Filter 过滤器的使用步骤：** 

**1、编写一个类去实现 Filter 接口** 

**2、实现过滤方法 doFilter()** 

**3、到 web.xml 中去配置 Filter 的拦截路径**

```java
package com.atguigu.filter;

import javax.imageio.spi.ServiceRegistry;
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-24-17:26
 */
public class AdminFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        HttpSession session = httpServletRequest.getSession();
        Object User = session.getAttribute("user");
        if(User == null) {
            servletRequest.getRequestDispatcher("/login.jsp").forward(servletRequest,servletResponse);
            return;
        } else {
            // 让程序继续访问
            filterChain.doFilter(servletRequest,servletResponse);
        }
    }

    @Override
    public void destroy() {

    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <filter>
        <filter-name>AdminFilter</filter-name>
        <filter-class>com.atguigu.filter.AdminFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>AdminFilter</filter-name>
        <!-- 配置拦截路径 -->
        <url-pattern>/admin/*</url-pattern>
    </filter-mapping>
</web-app>
```

### Filter的生命周期

**Filter 的生命周期包含几个方法** 

**1、构造器方法** 

**2、init 初始化方法** 

**第 1，2 步，在 web 工程启动的时候执行（Filter 已经创建）** 

**3、doFilter 过滤方法** 

**第 3 步，每次拦截到请求，就会执行** 

**4、destroy 销毁** 

**第 4 步，停止 web 工程的时候，就会执行（停止 web 工程，也会销毁 Filter 过滤器）**



### FilterConfig

**FilterConfig 类见名知义，它是 Filter 过滤器的配置文件类。** 

**Tomcat 每次创建 Filter 的时候，也会同时创建一个 FilterConfig 类，这里包含了 Filter 配置文件的配置信息。** 

**FilterConfig 类的作用是获取 filter 过滤器的配置内容** 

**1、获取 Filter 的名称 filter-name 的内容** 

**2、获取在 Filter 中配置的 init-param 初始化参数** 

**3、获取 ServletContext 对象**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <filter>
        <filter-name>AdminFilter</filter-name>
        <filter-class>com.atguigu.filter.AdminFilter</filter-class>
        <init-param>
            <param-name>username</param-name>
            <param-value>123</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>AdminFilter</filter-name>
        <url-pattern>/admin/*</url-pattern>
    </filter-mapping>
</web-app>
```

```java
package com.atguigu.filter;

import javax.imageio.spi.ServiceRegistry;
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-24-17:26
 */
public class AdminFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println(filterConfig.getFilterName());
        System.out.println("初始化参数" + filterConfig.getInitParameter("username"));
        System.out.println(filterConfig.getServletContext());
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        HttpSession session = httpServletRequest.getSession();
        Object User = session.getAttribute("user");
        if(User == null) {
            servletRequest.getRequestDispatcher("/login.jsp").forward(servletRequest,servletResponse);
            return;
        } else {
            // 让程序继续访问
            filterChain.doFilter(servletRequest,servletResponse);
        }
    }

    @Override
    public void destroy() {

    }
}
```

### FilterChain 过滤器链

FilterChain 就是过滤器链（**多个过滤器如何一起工作**）

![image-20211024191532935](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211024191532935.png)

### Filter的拦截路径

- **精确匹配**

**<url-pattern>/target.jsp</url-pattern>** 

**以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/target.jsp** 

- **目录匹配**

**<url-pattern>/admin/*</url-pattern>** 

**以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/admin/*** 

- **后缀名匹配**

**<url-pattern>*.html</url-pattern>** 

**以上配置的路径，表示请求地址必须以.html 结尾才会拦截到** 

**<url-pattern>*.do</url-pattern>** 

**以上配置的路径，表示请求地址必须以.do 结尾才会拦截到** 

**<url-pattern>*.action</url-pattern>** 

**以上配置的路径，表示请求地址必须以.action 结尾才会拦截到** 

**Filter 过滤器它只关心请求的地址是否匹配，不关心请求的资源是否存在！！！**



### ThreadLocal的使用

**ThreadLocal 的作用，它可以解决多线程的数据安全问题。** 

**ThreadLocal 它可以给当前线程关联一个数据（可以是普通变量，可以是对象，也可以是数组，集合）** 

**ThreadLocal 的特点：** 

**1、ThreadLocal 可以为当前线程关联一个数据。（它可以像 Map 一样存取数据，key 为当前线程）** 

**2、每一个 ThreadLocal 对象，只能为当前线程关联一个数据，如果要为当前线程关联多个数据，就需要使用多个** 

**ThreadLocal 对象实例。** 

**3、每个 ThreadLocal 对象实例定义的时候，一般都是 static 类型** 

**4、ThreadLocal 中保存数据，在线程销毁后。会由 JVM 虚拟自动释放。** 

```java
public class OrderService { 
    public void createOrder(){ 
        String name = Thread.currentThread().getName(); 
        System.out.println("OrderService 当前线程[" + name + "]中保存的数据是：" + ThreadLocalTest.threadLocal.get()); 
        new OrderDao().saveOrder(); 
    } 
}

public class OrderDao { 
    public void saveOrder(){ 
        String name = Thread.currentThread().getName(); 
        System.out.println("OrderDao 当前线程[" + name + "]中保存的数据是：" + ThreadLocalTest.threadLocal.get()); 
    } 
}

public class ThreadLocalTest { 
    // public static Map<String,Object> data = new Hashtable<String,Object>(); 
    public static ThreadLocal<Object> threadLocal = new ThreadLocal<Object>(); 
    private static Random random = new Random();
	public static class Task implements Runnable { 
        @Override 
        public void run() { 
            // 在 Run 方法中，随机生成一个变量（线程要关联的数据），然后以当前线程名为 key 保存到 map 中 
            Integer i = random.nextInt(1000); 
            // 获取当前线程名 
            String name = Thread.currentThread().getName(); 
            System.out.println("线程["+name+"]生成的随机数是：" + i); 
            // data.put(name,i); 
            threadLocal.set(i); 
            try {
                Thread.sleep(3000); 
            } catch (InterruptedException e) { 
                e.printStackTrace(); 
            }
            new OrderService().createOrder(); 
            // 在 Run 方法结束之前，以当前线程名获取出数据并打印。查看是否可以取出操作 
            // Object o = data.get(name); 
            Object o = threadLocal.get(); 
            System.out.println("在线程["+name+"]快结束时取出关联的数据是：" + o); 
        } 
    }
    public static void main(String[] args) { 
        for (int i = 0; i < 3; i++){ 
            new Thread(new Task()).start(); 
        } 
    } 
}
```

### 使用Filter和ThreadLocal组合管理事务

![image-20211025110009740](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211025110009740.png)

**使用事务对jdbc工具类进行修改**

```java
package com.atguigu.utils;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.pool.DruidDataSourceFactory;

import java.io.InputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

/**
 * @author yh
 * @create 2021-10-20-15:41
 */
public class JdbcUtils {
    private static DruidDataSource dataSource;
    private static ThreadLocal<Connection> conns = new ThreadLocal<>();
    static {
        try {
            Properties properties = new Properties();
            InputStream inputStream = JdbcUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
            properties.load(inputStream);
            dataSource = (DruidDataSource) DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取数据库连接池
     * @return
     */
    public static Connection getConnection() {
        Connection conn = conns.get();
        if(conn == null) {
            try {
                conn = dataSource.getConnection();
                conns.set(conn);
                conn.setAutoCommit(false);
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        return conn;
    }

    public static void commitAndClose() {
        Connection connection = conns.get();
        if(connection != null) {
            try {
                connection.commit();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            } finally {
                try {
                    connection.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
        conns.remove();
    }

    public static void rollbackAndClose() {
        Connection connection = conns.get();
        if(connection != null) {
            try {
                connection.rollback();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            } finally {
                try {
                    connection.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
        conns.remove();
    }

    /**
     * 关闭连接
     * @param conn
     */
    /*public static void close(Connection conn) {
        if(conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }*/
}

```

**对BaseDao的修改，去掉连接的关闭并向外抛出异常**

```java
package com.atguigu.dao.impl;

import com.atguigu.utils.JdbcUtils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;

import java.io.ObjectStreamException;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-20-16:09
 */
public abstract class BaseDao {
    private QueryRunner queryRunner = new QueryRunner();

    public int update(String sql,Object ...args) {
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.update(conn,sql,args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }

    public <T> T queryForOne(Class<T> type,String sql,Object ...args) {
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.query(conn,sql,new BeanHandler<T>(type),args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }

    public <T> List<T> queryForList(Class<T> type,String sql,Object ...args) {
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.query(conn,sql,new BeanListHandler<T>(type),args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }

    public Object queryForSingleValue(String sql,Object ...args) {
        Connection conn = JdbcUtils.getConnection();
        try {
            return queryRunner.query(conn,sql,new ScalarHandler(),args);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
}
```

**对servlet进行try-catch的处理**



### 使用Filter过滤器统一给所有的Service方法都加上try-catch来进行实现的管理

![image-20211025111541098](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211025111541098.png)

**Filter代码**

```java
package com.atguigu.filter;

import com.atguigu.utils.JdbcUtils;

import javax.servlet.*;
import java.io.IOException;

/**
 * @author yh
 * @create 2021-10-25-11:18
 */
public class TransactionFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        try {
            filterChain.doFilter(servletRequest,servletResponse);
            JdbcUtils.commitAndClose();
        } catch (Exception e) {
            JdbcUtils.rollbackAndClose();
            e.printStackTrace();
        }
    }

    @Override
    public void destroy() {
        Filter.super.destroy();
    }
}
```

**web.xml配置**

```xml
<filter> 
    <filter-name>TransactionFilter</filter-name> 
    <filter-class>com.atguigu.filter.TransactionFilter</filter-class> 
</filter> 
<filter-mapping> 
    <filter-name>TransactionFilter</filter-name> 
    <!-- /* 表示当前工程下所有请求 --> 
    <url-pattern>/*</url-pattern> 
</filter-mapping>
```

**对baseServlet中的异常要抛出**

```java
package com.atguigu.web;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.Method;

/**
 * @author yh
 * @create 2021-10-23-0:21
 */
public class BaseServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 解决post请求中文乱码问题
        // 一定要在获取请求参数之前调用才有效
        req.setCharacterEncoding("UTF-8");

        String action = req.getParameter("action");
        try {
            // 获取action业务鉴别字符串，获取相应的业务 方法反射对象
            Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class, HttpServletResponse.class);
//            System.out.println(method);
            // 调用目标业务 方法
            method.invoke(this, req, resp);
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
}
```

### 将所有异常都统一交给Tomcat，让Tomcat展示友好的错误信息页面

```xml
<!--error-page 标签配置，服务器出错之后，自动跳转的页面--> 
<error-page> 
    <!--error-code 是错误类型--> 
    <error-code>500</error-code> 
    <!--location 标签表示。要跳转去的页面路径--> 
    <location>/pages/error/error500.jsp</location> 
</error-page> <!--error-page 标签配置，服务器出错之后，自动跳转的页面--> 
<error-page> 
    <!--error-code 是错误类型--> 
    <error-code>404</error-code> 
    <!--location 标签表示。要跳转去的页面路径--> 
    <location>/pages/error/error404.jsp</location> 
</error-page>
```

## day16

### json

#### javaBean和json的互转

- 先导入gsonjar包

```java
public void test1(){
    Person person = new Person(1,"国哥好帅!");
    // 创建Gson对象实例
    Gson gson = new Gson();
    // toJson方法可以把java对象转换成为json字符串
    String personJsonStsring = gson.toJson(person);
    System.out.println(personJsonString);
    // fromJson把json字符串转换回Java对象
    // 第一个参数是json字符串
    // 第二个参数是转换回去的Java对象类型
    Person person1 = gson.fromJson(personJsonString, Person.class);
    System.out.println(person1);
}
```

#### List和json的相互转换

```java
// 1.2.2、List 和json的互转
@Test
public void test2() {
    List<Person> personList = new ArrayList<>();

    personList.add(new Person(1, "国哥"));
    personList.add(new Person(2, "康师傅"));

    Gson gson = new Gson();

    // 把List转换为json字符串
    String personListJsonString = gson.toJson(personList);
    System.out.println(personListJsonString);

    List<Person> list = gson.fromJson(personListJsonString, new PersonListType().getType());
    System.out.println(list);
    Person person = list.get(0);
    System.out.println(person);
}
```

#### json和map的相互转换

```java
//    1.2.3、map 和json的互转
@Test
public void test3(){
    Map<Integer,Person> personMap = new HashMap<>();

    personMap.put(1, new Person(1, "国哥好帅"));
    personMap.put(2, new Person(2, "康师傅也好帅"));

    Gson gson = new Gson();
    // 把 map 集合转换成为 json字符串
    String personMapJsonString = gson.toJson(personMap);
    System.out.println(personMapJsonString);

    //        Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new PersonMapType().getType());
    Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new TypeToken<HashMap<Integer,Person>>(){}.getType());

    System.out.println(personMap2);
    Person p = personMap2.get(1);
    System.out.println(p);
}
```

### ajax

```java
package com.atguigu.servlet;

import com.atguigu.pojo.Person;
import com.google.gson.Gson;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class AjaxServlet extends BaseServlet {

    protected void javaScriptAjax(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("Ajax请求过来了");
        Person person = new Person(1, "国哥");
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }

    protected void jQueryAjax(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("  jQueryAjax == 方法调用了");
        Person person = new Person(1, "国哥");
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }

    protected void jQueryGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("  jQueryGet  == 方法调用了");
        Person person = new Person(1, "国哥");
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }

    protected void jQueryPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("  jQueryPost   == 方法调用了");
        Person person = new Person(1, "国哥");
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }


    protected void jQueryGetJSON(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("  jQueryGetJSON   == 方法调用了");
        Person person = new Person(1, "国哥");
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }

    protected void jQuerySerialize(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("  jQuerySerialize   == 方法调用了");

        System.out.println("用户名：" + req.getParameter("username"));
        System.out.println("密码：" + req.getParameter("password"));

        Person person = new Person(1, "国哥");
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }
}
```

#### jquery的serialize方法

**表单序列化 serialize()** 

**serialize()可以把表单中所有表单项的内容都获取到，并以 name=value&name=value 的形式进行拼接。**

```js
// ajax 请求
$("#submit").click(function(){ 
	// 把参数序列化 
    $.getJSON("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQuerySerialize&" + 		$("#form01").serialize(),function (data) { 
        $("#msg").html(" Serialize 编号：" + data.id + " , 姓名：" + data.name); 
    }); 
});
```

