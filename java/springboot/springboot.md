# Springboot

## 入门程序(四种方式创建)

### 第一种直接通过idea

- 创建新模块，选择Spring Initializr，并配置模块相关基础信息

![image-20211109201912474](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109201912474.png)

- 选择当前模块需要使用的技术集

![image-20211109201940157](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109201940157.png)

- 开发控制器类

```java
//Rest模式
@RestController
@RequestMapping("/books")
public class BookController {
    @GetMapping
    public String getById(){
        System.out.println("springboot is running...");
        return "springboot is running..."; 
    } 
}
```

- 运行自动生成的Application类

![image-20211109202032399](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109202032399.png)

![image-20211109202044128](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109202044128.png)

**小结**

1. 开发SpringBoot程序可以根据向导进行联网快速制作

2. SpringBoot程序需要基于JDK8进行制作

3. SpringBoot程序中需要使用何种功能通过勾选选择技术

4. 运行SpringBoot程序通过运行Application程序入口进行



### 第二钟通过spring官网

基于SpringBoot官网创建项目，地址：https://start.spring.io/

![image-20211109202508008](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109202508008.png)

**小结**

1. 打开SpringBoot官网，选择Quickstart Your Project

2. 创建工程，并保存项目

3. 解压项目，通过IDE导入项目



### 第三种通过阿里云创建

基于阿里云创建项目，地址：https://start.aliyun.com

![image-20211109202623978](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109202623978.png)

**小结**

1. 选择start来源为自定义URL

2. 输入阿里云start地址

3. 创建项目



### 第四种手工创建

手工创建项目（手工导入坐标）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> <modelVersion>4.0.0</modelVersion> 
    <parent> 
        <groupId>org.springframework.boot</groupId> 
        <artifactId>spring-boot-starter-parent</artifactId> 
        <version>2.5.4</version>
    </parent> 
    <groupId>com.itheima</groupId> 
    <artifactId>springboot_01_03_quickstart</artifactId> 
    <version>1.0-SNAPSHOT</version> 
    <dependencies>
        <dependency> 
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>
```

手工创建项目（手工制作引导类）

```java
package com.itheima;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    } 
}
```

**小结**

1. 创建普通Maven工程

2. 继承spring-boot-starter-parent

3. 添加依赖spring-boot-starter-web

4. 制作引导类Application

## 隐藏指定文件/文件夹

![image-20211109203908857](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109203908857.png)

**小结**

1. Idea中隐藏指定文件或指定类型文件
2. Setting → File Types → Ignored Files and Folders
3. 输入要隐藏的文件名，支持*号通配符
4. 回车确认添加



## 入门案例解析

### parent

![image-20211109204127866](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109204127866.png)

1. 开发SpringBoot程序要继承spring-boot-starter-parent

2. spring-boot-starter-parent中定义了若干个依赖管理

3. 继承parent模块可以避免多个依赖使用相同技术时出现依赖版本冲突

4. 继承parent的形式也可以采用引入依赖的形式实现效果



### starter

starter

- SpringBoot中常见项目名称，定义了当前项目使用的所有依赖坐标，以达到减少依赖配置的目的

parent

- 所有SpringBoot项目要继承的项目，定义了若干个坐标版本号（依赖管理，而非依赖），以达到减少依赖冲突的目的

- spring-boot-starter-parent各版本间存在着诸多坐标版本不同

实际开发

- 使用任意坐标时，仅书写GAV中的G和A，V由SpringBoot提供，除非SpringBoot未提供对应版本V 

- 如发生坐标错误，再指定Version（要小心版本冲突）

**小结**

1. 开发SpringBoot程序需要导入坐标时通常导入对应的starter

2. 每个不同的starter根据功能不同，通常包含多个依赖坐标

3. 使用starter可以实现快速配置的效果，达到简化配置的目的



### 引导类

- 启动方式

```java
@SpringBootApplication
public class Springboot01QuickstartApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot01QuickstartApplication.class, args);
    } 
}
```

- SpringBoot的引导类是Boot工程的执行入口，运行main方法就可以启动项目

- SpringBoot工程运行后初始化Spring容器，扫描引导类所在包加载bean

**小结**

1. SpringBoot工程提供引导类用来启动程序

2. SpringBoot工程启动后创建并初始化Spring容器



### 内嵌tomcat

![image-20211109205826906](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109205826906.png)

![image-20211109205846388](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109205846388.png)

**小结**

1. 内嵌Tomcat服务器是SpringBoot辅助功能之一

2. 内嵌Tomcat工作原理是将Tomcat服务器作为对象运行，并将该对象交给Spring容器管理

3. 变更内嵌服务器思想是去除现有服务器，添加全新的服务器



## 复制工程

保留工程基础结构

抹掉原始工程痕迹

![image-20211109211249409](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109211249409.png)

1. 在工作空间中复制对应工程，并修改工程名称

2. 删除与Idea相关配置文件，仅保留src目录与pom.xml文件

3. 修改pom.xml文件中的artifactId与新工程/模块名相同

4. 删除name标签（可选）

5. 保留备份工程供后期使用



## 基础配置

### 属性配置

修改服务器端口

![image-20211109211958825](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109211958825.png)

SpringBoot默认配置文件application.properties，通过键值对配置对应属性

```properties
server.port=80
```

1. SpringBoot默认配置文件application.properties



### 配置属性下

修改配置

- 修改服务器端口

```properties
server.port=80
```

- 关闭运行日志图标（banner） 

```properties
spring.main.banner-mode=off
```

- 设置日志相关

```properties
logging.level.root=debug
```

SpringBoot内置属性查询

https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties

官方文档中参考文档第一项：Application Properties

1. SpringBoot中导入对应starter后，提供对应配置属性

2. 书写SpringBoot配置采用关键字+提示形式书写



### 配置格式

SpringBoot提供了多种属性配置方式

![image-20211109212639285](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109212639285.png)

1. SpringBoot提供了3种配置文件的格式

- properties（传统格式/默认格式）

- **yml**（主流格式）

- yaml

SpringBoot配置文件加载顺序：application.**properties >** application.**yml >** application.**yaml**

1. 配置文件间的加载优先级

- properties（最高）

- yml

- yaml（最低）

2. 不同配置文件中相同配置按照加载优先级相互覆盖，不同配置文件中不同配置全部保留



## 自动提示功能消失解决方案

![image-20211109213216431](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109213216431.png)

![image-20211109213227278](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109213227278.png)

1. 指定SpringBoot配置文件

- Setting → Project Structure → Facets

- 选中对应项目/工程

- Customize Spring Boot

- 选择配置文件



## yaml配置文件编写格式

yaml语法规则

- 大小写敏感

- 属性层级关系使用多行描述，每行结尾使用冒号结束

- 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）

- 属性值前面添加空格（属性名与属性值之间使用冒号+空格作为分隔）

- #表示注释

- 核心规则：**数据前面要加空格与冒号隔开**

![image-20211109213451568](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109213451568.png)



## yaml数据读取

![image-20211109213632333](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109213632333.png)

**小结**

1. 使用@Value配合SpEL读取单个数据

2. 如果数据存在多层级，依次书写层级名称即可



![image-20211109213713872](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109213713872.png)

**小结**

1. 在配置文件中可以使用${属性名}方式引用属性值

2. 如果属性中出现特殊字符，可以使用双引号包裹起来作为字符解析



**封装所有的数据到environment对象**

![image-20211109213800800](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109213800800.png)

**小结**

1. 使用Environment对象封装全部配置信息

2. 使用@Autowired自动装配数据到Environment对象中



![image-20211109213922113](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109213922113.png)

![image-20211109214008044](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211109214008044.png)

**小结**

1. 使用@ConfigurationProperties注解绑定配置信息到封装类中

2. 封装类需要定义为Spring管理的bean，否则无法进行属性注入



## 整合第三方技术

### 整合junit

**测试文件下**

```java
@SpringBootTest
class Springboot07JunitApplicationTests {
    @Autowired
    private BookService bookService;
    @Test
    public void testSave(){
        bookService.save();
    } 
}
```

名称：@SpringBootTest

类型：**测试类注解**

位置：测试类定义上方

作用：设置JUnit加载的SpringBoot启动类

**小结**

1. 导入测试对应的starter

2. 测试类使用@SpringBootTest修饰

3. 使用自动装配的形式添加要测试的对象

![image-20211110125348008](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211110125348008.png)



**classes属性**

classes：设置SpringBoot启动类

```java
@SpringBootTest(classes = Springboot05JUnitApplication.class)
class Springboot07JUnitApplicationTests {}
```

如果测试类在SpringBoot启动类的包或子包中，可以省略启动类的设置，也就是省略classes的设定

**小结**

1. 测试类如果存在于引导类所在包或子包中无需指定引导类

2. 测试类如果不存在于引导类所在的包或子包中需要通过classes属性指定引导类



### 整合mybatis

![image-20211110130619019](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211110130619019.png)

①：创建新模块，选择Spring初始化，并配置模块相关基础信息

![image-20211110130524310](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211110130524310.png)

②：选择当前模块需要使用的技术集（MyBatis、MySQL）

![image-20211110130535062](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211110130535062.png)

③：设置数据源参数

```yml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
```

注意事项

SpringBoot版本低于2.4.3(不含)，Mysql驱动版本大于8.0时，需要在url连接串中配置时区或在MySQL数据库端配置时区解决此问题

```yml
jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
```

④：定义数据层接口与映射配置

```java
package com.itheima.dao;

import com.itheima.domain.Book;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

@Mapper
public interface BookDao {
    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);
}

```

⑤：测试类中注入dao接口，测试功能

```java
package com.itheima;

import com.itheima.dao.BookDao;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class Springboot05MybatisApplicationTests {

    @Autowired
    private BookDao bookDao;

    @Test
    void contextLoads() {
        System.out.println(bookDao.getById(11));
    }
}
```

**小结**

1. 勾选MyBatis技术，也就是导入MyBatis对应的starter

2. 数据库连接相关信息转换成配置

3. 数据库SQL映射需要添加@Mapper被容器识别到

4. MySQL 8.X驱动强制要求设置时区

- 修改url，添加serverTimezone设定

- 修改MySQL数据库配置（略）

5. 驱动类过时，提醒更换为com.mysql.cj.jdbc.Driver



### 整合mybatis-plus

①：手动添加SpringBoot整合MyBatis-Plus的坐标，可以通过mvnrepository获取

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
```

注意事项：由于SpringBoot中未收录MyBatis-Plus的坐标版本，需要指定对应的Version

②：定义数据层接口与映射配置，继承**BaseMapper**

```java
@Mapper
public interface UserDao extends BaseMapper<User> {
}
```

③：其他同SpringBoot整合MyBatis

![image-20211110134329378](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211110134329378.png)

**小结**

1. 手工添加MyBatis-Plus对应的starter

2. 数据层接口使用BaseMapper简化开发

3. 需要使用的第三方技术无法通过勾选确定时，需要手工添加坐标



### 整合druid

指定数据源类型

```yml
spring:
	datasource:
		driver-class-name: com.mysql.cj.jdbc.Driver
		url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
		username: root
		password: root
		type: com.alibaba.druid.pool.DruidDataSource
```

导入Druid对应的starter

```xml
<dependency> 
    <groupId>com.alibaba</groupId> 
    <artifactId>druid-spring-boot-starter</artifactId> 
    <version>1.2.6</version>
</dependency>
```

变更Druid的配置方式

```yml
spring:
	datasource:
		druid:
			driver-class-name: com.mysql.cj.jdbc.Driver
			url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
			username: root
			password: root
```

**小结**

1. 整合Druid需要导入Druid对应的starter

2. 根据Druid提供的配置方式进行配置

3. 整合第三方技术通用方式

- 导入对应的starter

- 根据提供的配置格式，配置非默认值对应的配置项



## Springboot和SSM整合的案例

### 项目准备

1. 勾选SpringMVC与MySQL坐标

2. 修改配置文件为yml格式

3. 设置端口为80方便访问



### 实体类开发

Lombok，一个Java类库，提供了一组注解，简化POJO实体类开发

```xml
<!--lombok-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

常用注解：@Data

```java
package com.itheima.domain;

import lombok.Data;

/**
 * @author yh
 * @create 2021-11-11-13:45
 */
@Data
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```

为当前实体类在编译期设置对应的get/set方法，toString方法，hashCode方法，equals方法等

**小结**

1. 实体类制作

2. 使用lombok简化开发

导入lombok无需指定版本，由SpringBoot提供版本

@Data注解



### 数据层开发

导入MyBatisPlus与Druid对应的starter

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.6</version>
</dependency>
```

配置数据源与MyBatisPlus对应的基础配置（id生成策略使用数据库自增策略）

```yml
spring:
	datasource:
		druid:
			driver-class-name: com.mysql.cj.jdbc.Driver
			url: jdbc:mysql://localhost:3306/ssm_db?servierTimezone=UTC
			username: root
			password: root
mybatis-plus:
	global-config:
		db-config:
			table-prefix: tbl_
			id-type: auto
```

继承BaseMapper并指定泛型

```java
@Mapper
public interface BookDao extends BaseMapper<Book> {
}
```

制作测试类测试结果

```java
@SpringBootTest
public class BookDaoTest {
    @Autowired
    private BookDao bookDao;
    @Test
    void testSave() {
        Book book = new Book();
        book.setName("测试数据");
        book.setType("测试类型");
        book.setDescription("测试描述数据");
        bookDao.insert(book);
    }
    @Test
    void testGetById() {
        System.out.println(bookDao.selectById(13));
    }
}
```

**小结**

1. 手工导入starter坐标（2个）

2. 配置数据源与MyBatisPlus对应的配置

3. 开发Dao接口（继承BaseMapper）

4. 制作测试类测试Dao功能是否有效



### 开启mp日志

为方便调试可以开启MyBatisPlus的日志

```yml
server:
  port: 8080
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC
      username: root
      password: yanghong

mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_
      id-type: auto
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

```

**小结**

1. 使用配置方式开启日志，设置日志输出方式为标准输出



### 数据层开发——分页功能

分页操作需要设定分页对象IPage

```java
@Test
void testGetPage(){
    IPage page = new Page(1,5);
    bookDao.selectPage(page,null);
}
```

IPage对象中封装了分页操作中的所有数据

- 数据

- 当前页码值

- 每页数据总量

- 最大页码值

- 数据总量

分页操作是在MyBatisPlus的常规操作基础上增强得到，内部是动态的拼写SQL语句，因此需要增强对应的功能，

使用MyBatisPlus拦截器实现

```java
@Configuration
public class MpConfig {
    @Bean
    public MybatisPlusInterceptor mpInterceptor() {
        //1.定义Mp拦截器
        MybatisPlusInterceptor mpInterceptor = new MybatisPlusInterceptor();
        //2.添加具体的拦截器
        mpInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mpInterceptor; 
    } 
}
```

**小结**

1. 使用IPage封装分页数据

2. 分页操作依赖MyBatisPlus分页拦截器实现功能

3. 借助MyBatisPlus日志查阅执行SQL语句



### 数据层开发——条件查询功能

使用QueryWrapper对象封装查询条件，推荐使用LambdaQueryWrapper对象，所有查询操作封装成方法调用

```java
@Test
void testGetByCondition(){
    IPage page = new Page(1,10);
    LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<Book>();
    lqw.like(Book::getName,"Spring");
    bookDao.selectPage(page,lqw);
}
```

替换

```java
@Test
void testGetByCondition(){
    QueryWrapper<Book> qw = new QueryWrapper<Book>();
    qw.like("name","Spring");
    bookDao.selectList(qw);
}
```

支持动态拼写查询条件

```java
@Test
void testGetByCondition(){
    String name = "Spring";
    IPage page = new Page(1,10);
    LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<Book>();
    lqw.like(Strings.isNotEmpty(name),Book::getName,"Spring");
    bookDao.selectPage(page,lqw);
}
```

**小结**

1. 使用QueryWrapper对象封装查询条件

2. 推荐使用LambdaQueryWrapper对象

3. 所有查询操作封装成方法调用

4. 查询条件支持动态条件拼装



### 业务层开发

**接口定义**

```java
package com.itheima.service;

import com.baomidou.mybatisplus.core.metadata.IPage;
import com.itheima.domain.Book;

import java.util.List;

/**
 * @author yh
 * @create 2021-11-12-13:40
 */
public interface BookService {
    Boolean save(Book book);
    Boolean update(Book book);
    Boolean delete(Integer id);
    Book getById(Integer id);
    List<Book> getAll();
    IPage<Book> getPage(int currentPage,int pageSize);
}

```

**实现类定义**

```java
package com.itheima.service.impl;

import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.itheima.dao.BookDao;
import com.itheima.domain.Book;
import com.itheima.service.BookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * @author yh
 * @create 2021-11-12-13:43
 */
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;
    @Override
    public Boolean save(Book book) {
        return bookDao.insert(book) > 0;
    }

    @Override
    public Boolean update(Book book) {
        return bookDao.updateById(book) > 0;
    }

    @Override
    public Boolean delete(Integer id) {
        return bookDao.deleteById(id) > 0;
    }

    @Override
    public Book getById(Integer id) {
        return bookDao.selectById(id);
    }

    @Override
    public List<Book> getAll() {
        return bookDao.selectList(null);
    }

    @Override
    public IPage<Book> getPage(int currentPage, int pageSize) {
        IPage page = new Page(currentPage,pageSize);
        bookDao.selectPage(page,null);
        return page;
    }
}

```

测试类定义

```java
package com.itheima.service;

import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.itheima.domain.Book;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

/**
 * @author yh
 * @create 2021-11-12-16:07
 */
@SpringBootTest
public class BookServiceTest {

    @Autowired
    private IBookService bookService;

    @Test
    void testGetById(){
        System.out.println(bookService.getById(4));
    }

    @Test
    void testSave(){
        Book book = new Book();
        book.setType("测试数据123");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        bookService.save(book);
    }

    @Test
    void testUpdate(){
        Book book = new Book();
        book.setId(17);
        book.setType("-----------------");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        bookService.updateById(book);
    }

    @Test
    void testDelete(){
        bookService.removeById(18);
    }

    @Test
    void testGetAll(){
        bookService.list();
    }

    @Test
    void testGetPage(){
        IPage<Book> page = new Page<Book>(2,5);
        bookService.page(page);
        System.out.println(page.getCurrent());
        System.out.println(page.getSize());
        System.out.println(page.getTotal());
        System.out.println(page.getPages());
        System.out.println(page.getRecords());
    }
}
```

**小结**

1. Service接口名称定义成业务名称，并与Dao接口名称进行区分

2. 制作测试类测试Service功能是否有效



### 业务层开发——快速开发

使用MyBatisPlus提供有业务层通用接口（ISerivce<T>）与业务层通用实现类（ServiceImpl<M,T>） 

在通用类基础上做功能重载或功能追加

注意重载时不要覆盖原始操作，避免原始提供的功能丢失



接口定义

```java
package com.itheima.service;

import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.service.IService;
import com.itheima.domain.Book;

public interface IBookService extends IService<Book> {
    boolean saveBook(Book book);
    boolean modify(Book book);
    boolean delete(Integer id);
    IPage<Book> getPage(int currentPage, int pageSize);
    IPage<Book> getPage(int currentPage, int pageSize, Book book);
}

```

实现类定义并追加功能

```java
package com.itheima.service.impl;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.itheima.dao.BookDao;
import com.itheima.domain.Book;
import com.itheima.service.IBookService;
import org.apache.logging.log4j.util.Strings;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

/**
 * @author yh
 * @create 2021-11-12-16:05
 */
@Service
public class BookServiceImpl2 extends ServiceImpl<BookDao, Book> implements IBookService {
    @Autowired
    private BookDao bookDao;

    @Override
    public boolean saveBook(Book book) {
        return bookDao.insert(book) > 0;
    }

    @Override
    public boolean modify(Book book) {
        return bookDao.updateById(book) > 0;
    }

    @Override
    public boolean delete(Integer id) {
        return bookDao.deleteById(id) > 0;
    }

    @Override
    public IPage<Book> getPage(int currentPage, int pageSize) {
        IPage page = new Page(currentPage,pageSize);
        bookDao.selectPage(page,null);
        return page;
    }

    @Override
    public IPage<Book> getPage(int currentPage, int pageSize, Book book) {
        LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<Book>();
        lqw.like(Strings.isNotEmpty(book.getType()),Book::getType,book.getType());
        lqw.like(Strings.isNotEmpty(book.getName()),Book::getName,book.getName());
        lqw.like(Strings.isNotEmpty(book.getDescription()),Book::getDescription,book.getDescription());
        IPage page = new Page(currentPage,pageSize);
        bookDao.selectPage(page,lqw);
        return page;
    }
}

```

**小结**

1. 使用通用接口（ISerivce<T>）快速开发Service

2. 使用通用实现类（ServiceImpl<M,T>）快速开发ServiceImpl

3. 可以在通用接口基础上做功能重载或功能追加

4. 注意重载时不要覆盖原始操作，避免原始提供的功能丢失



### 表现层开发

```java
@RestController
@RequestMapping("/books")
public class BookController {
    @Autowired
    private IBookService bookService;
    @GetMapping
    public List<Book> getAll(){
        return bookService.list();
    } 
}
```

```java
package com.itheima.controller;

import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.itheima.domain.Book;
import com.itheima.service.IBookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

/**
 * @author yh
 * @create 2021-11-12-19:14
 */
//@RestController
@RequestMapping("/books")
public class BookController {
    @Autowired
    private IBookService iBookService;

    @GetMapping
    public List<Book> getAll() {
        return iBookService.list();
    }

    @PostMapping
    public Boolean save(@RequestBody Book book) {
        return iBookService.saveBook(book);
    }

    @PutMapping
    public Boolean update(@RequestBody Book book) {
        return iBookService.update(book,null);
    }

    @DeleteMapping("{id}")
    public Boolean delete(@PathVariable Integer id){
        return iBookService.removeById(id);
    }

    @GetMapping("{id}")
    public Book getById(@PathVariable Integer id){
        return iBookService.getById(id);
    }

    @GetMapping("{currentPage}/{pageSize}")
    public IPage<Book> getPage(@PathVariable int currentPage,@PathVariable int pageSize) {
        return iBookService.getPage(currentPage,pageSize);
    }
}

```

**小结**

1. 基于Restful制作表现层接口

- 新增：POST

- 删除：DELETE

- 修改：PUT

- 查询：GET

2. 接收参数

- 实体数据：@RequestBody

- 路径变量：@PathVariable



### 统一回传格式

设计表现层返回结果的模型类，用于后端与前端进行数据格式统一，也称为**前后端数据协议**

```java
package com.itheima.controller.utils;

import lombok.Data;

/**
 * @author yh
 * @create 2021-11-12-19:42
 */
@Data
public class R {
    private Boolean flag;
    private Object Data;
    private String msg;

    public R() {}

    public R(Boolean flag) {
        this.flag = flag;
    }

    public R(Boolean flag, Object data) {
        this.flag = flag;
        Data = data;
    }

    public R(Boolean flag, String msg) {
        this.flag = flag;
        this.msg = msg;
    }

    public R(String msg) {
        this.flag = false;
        this.msg = msg;
    }
}

```

表现层接口统一返回值类型结果

```java
package com.itheima.controller;

import com.baomidou.mybatisplus.core.metadata.IPage;
import com.itheima.controller.utils.R;
import com.itheima.domain.Book;
import com.itheima.service.IBookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

/**
 * @author yh
 * @create 2021-11-12-19:14
 */
@RestController
@RequestMapping("/books")
public class BookController2 {
    @Autowired
    private IBookService iBookService;

    @GetMapping
    public R getAll() {
        return new R(true,iBookService.list());
    }

    @PostMapping
    public R save(@RequestBody Book book) {
        boolean b = iBookService.saveBook(book);
        return new R(b,b?"添加成功":"添加失败");
    }

    @PutMapping
    public R update(@RequestBody Book book) {
        return new R(iBookService.update(book,null));
    }

    @DeleteMapping("{id}")
    public R delete(@PathVariable Integer id){
        return new R(iBookService.removeById(id));
    }

    @GetMapping("{id}")
    public R getById(@PathVariable Integer id){
        return new R(true,iBookService.getById(id));
    }

    @GetMapping("{currentPage}/{pageSize}")
    public R getPage(@PathVariable int currentPage,@PathVariable int pageSize,Book book){
//        System.out.println("参数==>"+book);

        IPage<Book> page = iBookService.getPage(currentPage, pageSize,book);
        //如果当前页码值大于了总页码值，那么重新执行查询操作，使用最大页码值作为当前页码值
        if( currentPage > page.getPages()){
            page = iBookService.getPage((int)page.getPages(), pageSize,book);
        }
        return new R(true, page);
    }
}
```

**小结**

1. 设计统一的返回值结果类型便于前端开发读取数据

2. 返回值结果类型可以根据需求自行设定，没有固定格式

3. 返回值结果模型类用于后端与前端进行数据格式统一，也称为前后端数据协议



### 基于vue的前端

前后端分离结构设计中页面归属前端服务器

单体工程中页面放置在resources目录下的static目录中（建议执行clean）



### 对异常进行统一的处理

对异常进行统一处理，出现异常后，返回指定信息

```java
package com.itheima.controller.utils;

import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

/**
 * @author yh
 * @create 2021-11-13-14:39
 */
//作为springmvc的异常处理器
@RestControllerAdvice
public class ProjectExceptionAdvice {

    @ExceptionHandler
    public R doException(Exception ex) {
        ex.printStackTrace();
        return new R("服务器故障,请稍后再试");
    }
}

```

修改表现层返回结果的模型类，封装出现异常后对应的信息

flag：false

Data: null

消息(msg): 要显示信息

可以在表现层Controller中进行消息统一处理

```java
@PostMapping
public R save(@RequestBody Book book) throws IOException {
    Boolean flag = bookService.insert(book);
    return new R(flag , flag ? "添加成功^_^" : "添加失败-_-!"); 
}
```

**小结**

1. 使用注解@RestControllerAdvice定义SpringMVC异常处理器用来处理异常的

2. 异常处理器必须被扫描加载，否则无法生效

3. 表现层返回结果的模型类中添加消息属性用来传递消息到页面



### 分页功能

分页查询

```java
@GetMapping("/{currentPage}/{pageSize}")
public R getAll(@PathVariable Integer currentPage,@PathVariable Integer pageSize){
    IPage<Book> pageBook = bookService.getPage(currentPage, pageSize);
    return new R(null != pageBook ,pageBook);
}
```



### 删除功能的维护

对查询结果进行校验，如果当前页码值大于最大页码值，使用最大页码值作为当前页码值重新查询

```java
@GetMapping("{currentPage}/{pageSize}")
public R getPage(@PathVariable int currentPage,@PathVariable int pageSize){
    IPage<Book> page = bookService.getPage(currentPage, pageSize);
    //如果当前页码值大于了总页码值，那么重新执行查询操作，使用最大页码值作为当前页码值
    if( currentPage > page.getPages()){
        page = bookService.getPage((int)page.getPages(), pageSize);
    }
    return new R(true, page);
}
```



### 条件查询

业务层接口功能开发

```java
public interface IBookService extends IService<Book> {
    IPage<Book> getPage(Integer currentPage,Integer pageSize,Book queryBook);
}
```

```java
@Service
public class BookServiceImpl2 extends ServiceImpl<BookDao,Book> implements IBookService {
    public IPage<Book> getPage(Integer currentPage,Integer pageSize,Book queryBook){
        IPage page = new Page(currentPage,pageSize);
        LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<Book>();
        lqw.like(Strings.isNotEmpty(queryBook.getName()),Book::getName,queryBook.getName());
        lqw.like(Strings.isNotEmpty(queryBook.getType()),Book::getType,queryBook.getType());
        lqw.like(Strings.isNotEmpty(queryBook.getDescription()),
                 Book::getDescription,queryBook.getDescription());
        return bookDao.selectPage(page,lqw);
    }
}
```

Controller调用业务层分页条件查询接口

```java
@GetMapping("{currentPage}/{pageSize}")
public R getAll(@PathVariable int currentPage,@PathVariable int pageSize,Book book) {
    IPage<Book> pageBook = bookService.getPage(currentPage,pageSize,book);
    return new R(null != pageBook ,pageBook);
}
```

**小结**

1. 定义查询条件数据模型（当前封装到分页数据模型中）

2. 异步调用分页功能并通过请求参数传递数据到后台



## 总结

**基于SpringBoot的SSMP整合案例**

1. pom.xml 配置起步依赖

2. application.yml 设置数据源、端口、框架技术相关配置等

3. dao 继承BaseMapper、设置@Mapper

4. dao 测试类

5. service 调用数据层接口或MyBatis-Plus提供的接口快速开发

6. service测试类

7. controller 基于Restful开发，使用Postman测试跑通功能

8. 页面 放置在resources目录下的static目录中



基础篇

- 能够创建SpringBoot工程

- 基于SpringBoot实现ssm/ssmp整合

实用篇

- 运维实用篇

- 能够掌握SpringBoot程序多环境开发

- 能够基于Linux系统发布SpringBoot工程

- 能够解决线上灵活配置SpringBoot工程的需求

开发实用篇

- 能够基于SpringBoot整合任意第三方技术

原理篇
