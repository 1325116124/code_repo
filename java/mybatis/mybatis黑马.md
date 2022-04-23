# Mybatis



## 什么是mybatis 

**原始jdbc开发存在的问题如下：**

**① 数据库连接创建、释放频繁造成系统资源浪费从而影响系统性能**

**② sql 语句在代码中硬编码，造成代码不易维护，实际应用 sql 变化的可能较大，sql 变动需要改变java代码。**

**③ 查询操作时，需要手动将结果集中的数据手动封装到实体中。插入操作时，需要手动将实体的数据设置到sql语句的占位**

**符位置**

**应对上述问题给出的解决方案：**

**① 使用数据库连接池初始化连接资源**

**② 将sql语句抽取到xml配置文件中**

**③ 使用反射、内省等底层技术，自动将实体与表进行属性与字段的自动映射**



- **mybatis 是一个优秀的基于java的持久层框架，它内部封装了jdbc，使开发者只需要关注sql语句本身，而不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。**

- **mybatis通过xml或注解的方式将要执行的各种 statement配置起来，并通过java对象和statement中sql的动态参数进行映射生成最终执行的sql语句。**

- **最后mybatis框架执行sql并将结果映射为java对象并返回。采用ORM思想解决了实体和数据库映射的问题，对jdbc 进行了封装，屏蔽了jdbc api 底层访问细节，使我们不用与jdbc api打交道，就可以完成对数据库的持久化操作。**



## Mybatis开发步骤

![image-20211031135218645](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031135218645.png)

MyBatis开发步骤：

① 添加MyBatis的坐标

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>itheima_mybatis_quick</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <properties>
        <maven.compiler.source>16</maven.compiler.source>
        <maven.compiler.target>16</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.32</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.6</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
    </dependencies>
</project>
```

② 创建user数据表

③ 编写User实体类

```java
package com.itheima.domain;

import java.util.Iterator;

/**
 * @author yh
 * @create 2021-10-30-22:14
 */
public class User {
    private Integer id;
    private String username;
    private String password;

    public User(Integer id, String username, String password) {
        this.id = id;
        this.username = username;
        this.password = password;
    }

    public User() {
    }

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

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}

```

④ 编写映射文件UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="userMapper">
    <select id="findAll" resultType="com.itheima.domain.User">
        select * from user
    </select>
</mapper>
```

⑤ 编写核心文件SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 数据源环境 -->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="yanghong"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 加载映射文件 -->
    <mappers>
        <mapper resource="com/itheima/mapper/UserMapper.xml"></mapper>
    </mappers>
</configuration>
```

⑥ 编写测试类

```java
package com.itheima.test;

import com.itheima.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-13:25
 */
public class MyBatisTest {
    @Test
    public void test() throws IOException {
        //获得核心配置文件
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        //获得session工厂对象
        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
        //获得session会话对象
        SqlSession sqlSession = build.openSession();
        //执行操作，参数namespace + id
        List<User> users = sqlSession.selectList("userMapper.findAll");
        //打印数据
        System.out.println(users);
        //释放资源
        sqlSession.close();
    }
}
```



## Mybatis映射文件概述

![image-20211031135849444](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031135849444.png)



## Mybatis插入操作

 **编写UserMapper映射文件**

```xml
<!-- 插入操作 -->
<insert id="save" parameterType="com.itheima.domain.User">
    insert into user values(#{id},#{username},#{password})
</insert>
```

 **编写插入实体User的代码**

```java
@Test
public void test2() throws IOException {
    //获得核心配置文件
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    //获得session工厂对象
    SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
    //获得session会话对象
    SqlSession sqlSession = build.openSession();
    //执行操作，参数namespace + id
    sqlSession.insert("userMapper.save",new User(null,"阳洪","123456"));
    //提交事务
    sqlSession.commit();
    //释放资源
    sqlSession.close();
}
```

**3. 插入操作注意问题**

- **插入语句使用insert标签**

- **在映射文件中使用parameterType属性指定要插入的数据类型**

- **Sql语句中使用#{实体属性名}方式引用实体中的属性值**

- **插入操作使用的API是sqlSession.insert(“命名空间.id”,实体对象);**

- **插入操作涉及数据库数据变化，所以要使用sqlSession对象显示的提交事务，即sqlSession.commit()** 



## Mybatis修改和删除操作

 **编写UserMapper映射文件**

```xml
    <!-- 修改操纵 -->
    <update id="update" parameterType="com.itheima.domain.User">
        update user set username=#{username},password=#{password} where id=#{id}
    </update>
    <!-- 删除 -->
    <delete id="delete" parameterType="java.lang.Integer">
        delete from user where id=#{id}
    </delete>
```

 **编写插入实体User的代码**

```java
@Test
public void test3() throws IOException {
    //获得核心配置文件
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    //获得session工厂对象
    SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
    //获得session会话对象
    SqlSession sqlSession = build.openSession();
    //执行操作，参数namespace + id
    sqlSession.update("userMapper.update",new User(1,"叶芳琳","4560"));
    //提交事务
    sqlSession.commit();
    //释放资源
    sqlSession.close();
}
@Test
public void test4() throws IOException {
    //获得核心配置文件
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    //获得session工厂对象
    SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
    //获得session会话对象
    SqlSession sqlSession = build.openSession();
    //执行操作，参数namespace + id
    sqlSession.delete("userMapper.delete",2);
    //提交事务
    sqlSession.commit();
    //释放资源
    sqlSession.close();
}
```

**修改操作注意问题**

- **修改语句使用update标签**

- **修改操作使用的API是sqlSession.update(“命名空间.id”,实体对象);**

**删除操作注意问题**

- **删除语句使用delete标签**

- **Sql语句中使用#{任意字符串}方式引用传递的单个参数**

- **删除操作使用的API是sqlSession.delete(“命名空间.id”,Object);**



## MyBatis核心配置文件概述

![image-20211031143423526](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031143423526.png)

### 1. environments标签

数据库环境的配置，支持多环境配置

![image-20211031143850364](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031143850364.png)

**1. environments标签**

**其中，事务管理器（transactionManager）类型有两种：**

- **JDBC：这个配置就是直接使用了JDBC 的提交和回滚设置，它依赖于从数据源得到的连接来管理事务作用域。**

- **MANAGED：这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如JEE** 

**应用服务器的上下文）。 默认情况下它会关闭连接，然而一些容器并不希望这样，因此需要将 closeConnection 属性设置**

**为 false 来阻止它默认的关闭行为。**

**其中，数据源（dataSource）类型有三种：**

- **UNPOOLED：这个数据源的实现只是每次被请求时打开和关闭连接。**

- **POOLED：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来。**

- **JNDI：这个数据源的实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置**

**一个 JNDI 上下文的引用。**



### 2. mapper标签

**该标签的作用是加载映射的，加载方式有如下几种：**

- **使用相对于类路径的资源引用，例如：<mapper resource="org/mybatis/builder/AuthorMapper.xml"/>**

- **使用完全限定资源定位符（URL），例如：<mapper url="file:///var/mappers/AuthorMapper.xml"/>**

- **使用映射器接口实现类的完全限定类名，例如：<mapper class="org.mybatis.builder.AuthorMapper"/>**

- **将包内的映射器接口实现全部注册为映射器，例如：<package name="org.mybatis.builder"/>**



### 3. Properties标签

**实际开发中，习惯将数据源的配置信息单独抽取成一个properties文件，该标签可以加载额外配置的properties文件**

![image-20211031144511234](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031144511234.png)



### 4. typeAliases标签

**类型别名是为Java 类型设置一个短的名字。原来的类型名称配置如下**

```xml
<typeAliases>
    <typeAlias type="com.itheima.domain.User" alias="user"></typeAlias>
</typeAliases>
```

```xml
<select id="findAll" resultType="user">
	select * from User
</select>
```

**上面我们是自定义的别名，mybatis框架已经为我们设置好的一些常用的类型的别名**

![image-20211031145018865](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031145018865.png)



## Mybatis相关api

###  SqlSession工厂构建器SqlSessionFactoryBuilder

**常用API：SqlSessionFactory build(InputStream inputStream)**

**通过加载mybatis的核心文件的输入流的形式构建一个SqlSessionFactory对象**

```java
String resource = "org/mybatis/builder/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
SqlSessionFactory factory = builder.build(inputStream);
```

**其中， Resources 工具类，这个类在 org.apache.ibatis.io 包中。Resources 类帮助你从类路径下、文件系统或一个 web URL 中加载资源文件。**



### SqlSession工厂对象SqlSessionFactory

**SqlSessionFactory 有多个个方法创建 SqlSession 实例。常用的有如下两个：**

![image-20211031150013351](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031150013351.png)



### SqlSession会话对象

**SqlSession 实例在 MyBatis 中是非常强大的一个类。在这里你会看到所有执行语句、提交或回滚事务和获取映射器实例的方法。**

**执行语句的方法主要有：**

![image-20211031150038488](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031150038488.png)

**操作事务的方法主要有：**

![image-20211031150051311](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031150051311.png)



## Mybatis的Dao层实现

### 传统开发方式

**1. 编写UserDao接口**

```java
public interface UserDao {
    List<User> findAll() throws IOException; 
}
```

**2. 编写UserDaoImpl实现**

```java
package com.itheima.dao.impl;

import com.itheima.dao.UserMapper;
import com.itheima.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-15:05
 */
public class UserMapperImpl implements UserMapper {

    @Override
    public List<User> findAll() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        List<User> users = sqlSession.selectList("userMapper.findAll");
        System.out.println(users);
        return users;
    }
}

```

**3. 测试传统方式**

```java
@Test
public void testTraditionDao() throws IOException {
    UserDao userDao = new UserDaoImpl();
    List<User> all = userDao.findAll();
    System.out.println(all);
}
```



### 代理开发方式

**采用 Mybatis 的代理开发方式实现 DAO 层的开发，这种方式是我们后面进入企业的主流。**

**Mapper 接口开发方法只需要程序员编写Mapper 接口（相当于Dao 接口），由Mybatis 框架根据接口定义创建接**

**口的动态代理对象，代理对象的方法体同上边Dao接口实现类方法。**

**Mapper 接口开发需要遵循以下规范：**

**1、 Mapper.xml文件中的namespace与mapper接口的全限定名相同**

**2、 Mapper接口方法名和Mapper.xml中定义的每个statement的id相同**

**3、 Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql的parameterType的类型相同**

**4、 Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同**

![image-20211031154243363](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031154243363.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itheima.dao.UserMapper">
    <!-- 查询操作 -->
    <select id="findAll" resultType="com.itheima.domain.User">
        select * from user
    </select>
    <select id="findById" parameterType="int" resultType="user">
        select * from user where id=#{id}
    </select>
</mapper>
```

**配置接口，方法名和id一致，返回值类型和参数类型一致**

```java
package com.itheima.dao;

import com.itheima.domain.User;

import java.io.IOException;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-15:05
 */
public interface UserMapper {
    public List<User> findAll() throws IOException;
    public User findById(int id);
}

```

**测试**

```java
package com.itheima.service;

import com.itheima.dao.UserMapper;
import com.itheima.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-15:08
 */
public class UserService {
    public static void main(String[] args) throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
//        List<User> users = mapper.findAll();
        User user = mapper.findById(1);
        System.out.println(user);
    }
}
```



## MyBatis映射文件深入

 ### 动态sql语句概述

Mybatis 的映射文件中，前面我们的 SQL 都是比较简单的，有些时候业务逻辑复杂时，我们的 SQL是动态变化的，

此时在前面的学习中我们的 SQL 就不能满足要求了。

 **动态 SQL 之<if>** 

我们根据实体类的不同取值，使用不同的 SQL语句来进行查询。比如在 id如果不为空时可以根据id查询，如果

username 不同空时还要加入用户名作为条件。这种情况在我们的多条件组合查询中经常会碰到。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itheima.mapper.UserMapper">
   <select id="findByCondition" resultType="user" parameterType="user">
       select * from user
        <where>
            <if test="id!=0">
                and id=#{id}
            </if>
            <if test="username!=null">
                and username=#{username}
            </if>
            <if test="password!=null">
                and password=#{password}
            </if>
        </where>

   </select>
</mapper>
```

```java
package com.itheima.test;

import com.itheima.mapper.UserMapper;
import com.itheima.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-15:49
 */
public class MapperTest {
    @Test
    public void test1() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        User user = new User();
        user.setId(1);
//        List<User> users = mapper.findAll();
        List<User> users = mapper.findByCondition(user);
        System.out.println(users);
    }
}

```

当查询条件id和username都存在时，控制台打印的sql语句如下：

![image-20211031160456685](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031160456685.png)

当查询条件只有id存在时，控制台打印的sql语句如下：

![image-20211031160508940](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031160508940.png)

**动态 SQL 之<foreach>** 

循环执行sql的拼接操作，例如：SELECT * FROM USER WHERE id IN (1,2,5)。 

```xml
<select id="findByIds" parameterType="list" resultType="user">
    select * from user
    <where>
        <foreach collection="list" open="id in(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```

```java
@Test
public void test1() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<Integer> ids = new ArrayList<>();
    ids.add(1);
    ids.add(3);
    List<User> users = mapper.findByIds(ids);
    System.out.println(users);
}
```

**foreach标签的属性含义如下：**

**<foreach>标签用于遍历集合，它的属性：**

- **collection：代表要遍历的集合元素，注意编写时不要写#{}**

- **open：代表语句的开始部分**

- **close：代表结束部分**

- **item：代表遍历集合的每个元素，生成的变量名**

- **sperator：代表分隔符**



### sql语句的抽取

Sql 中可将重复的 sql 提取出来，使用时用 include 引用即可，最终达到 sql 重用的目的

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itheima.mapper.UserMapper">
    <sql id="selectUser">select * from user</sql>
   <select id="findByCondition" resultType="user" parameterType="user">
       <include refid="selectUser"></include>
        <where>
            <if test="id!=0">
                and id=#{id}
            </if>
            <if test="username!=null">
                and username=#{username}
            </if>
            <if test="password!=null">
                and password=#{password}
            </if>
        </where>

   </select>
    <select id="findByIds" parameterType="list" resultType="user">
        <include refid="selectUser"></include>
        <where>
            <foreach collection="list" open="id in(" close=")" item="id" separator=",">
                #{id}
            </foreach>
        </where>
    </select>
</mapper>
```



## MyBatis核心配置文件深入

###  typeHandlers标签

无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用

类型处理器将获取的值以合适的方式转换成 Java 类型。下表描述了一些默认的类型处理器（截取部分）。

![image-20211031165339059](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031165339059.png)

**你可以重写类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。具体做法为：实现**

**org.apache.ibatis.type.TypeHandler 接口， 或继承一个很便利的类 org.apache.ibatis.type.BaseTypeHandler， 然**

**后可以选择性地将它映射到一个JDBC类型。例如需求：一个Java中的Date数据类型，我想将之存到数据库的时候存成一**

**个1970年至今的毫秒数，取出来时转换成java的Date，即java的Date与数据库的varchar毫秒值之间转换。**

**开发步骤：**

**① 定义转换类继承类BaseTypeHandler<T>**

```java
package com.itheima.handler;

import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Date;

/**
 * @author yh
 * @create 2021-10-31-16:46
 */
public class DateHandler extends BaseTypeHandler<Date> {
    //奖java类型转换为数据库需要的类型
    @Override
    public void setNonNullParameter(PreparedStatement preparedStatement, int i, Date date, JdbcType jdbcType) throws SQLException {
        long time = date.getTime();
        preparedStatement.setLong(i,time);
    }

    //将数据库类型转换为java类型
    //String参数 要转换的字段名称
    //ResultSet 查询出的结果集
    @Override
    public Date getNullableResult(ResultSet resultSet, String s) throws SQLException {
        long aLong = resultSet.getLong(s);
        Date date = new Date(aLong);
        return date;
    }

    @Override
    public Date getNullableResult(ResultSet resultSet, int i) throws SQLException {
        long aLong = resultSet.getLong(i);
        Date date = new Date(aLong);
        return date;
    }

    @Override
    public Date getNullableResult(CallableStatement callableStatement, int i) throws SQLException {
        long aLong = callableStatement.getLong(i);
        Date date = new Date(aLong);
        return date;
    }
}

```

**② 覆盖4个未实现的方法，其中setNonNullParameter为java程序设置数据到数据库的回调方法，getNullableResult为查询时 mysql的字符串类型转换成 java的Type类型的方法**

**③ 在MyBatis核心配置文件中进行注册**

```xml
    <!-- 注册类型处理器 -->
    <typeHandlers>
        <typeHandler handler="com.itheima.handler.DateHandler"></typeHandler>
    </typeHandlers>
```

**④ 测试转换是否正确**

```java
package com.itheima.test;

import com.itheima.mapper.UserMapper;
import com.itheima.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-15:49
 */
public class MapperTest {
    @Test
    public void test1() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        mapper.save(new User(4,"zzm","456",new Date()));
        sqlSession.commit();
        sqlSession.close();
    }
}
```



### plugins标签

**① 导入通用PageHelper坐标**

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>3.7.5</version>
</dependency>
<dependency>
    <groupId>com.github.jsqlparser</groupId>
    <artifactId>jsqlparser</artifactId>
    <version>0.9.1</version>
</dependency>
```

**② 在mybatis核心配置文件中配置PageHelper插件**

```xml
<!-- 配置分页助手插件 -->
<plugins>
    <plugin interceptor="com.github.pagehelper.PageHelper">
        <property name="dialect" value="mysql"/>
    </plugin>
</plugins>
```

**③ 测试分页代码实现**

```java
package com.itheima.test;

import com.github.pagehelper.PageHelper;
import com.github.pagehelper.PageInfo;
import com.itheima.mapper.UserMapper;
import com.itheima.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-15:49
 */
public class MapperTest {
    @Test
    public void test1() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        //设置分页的相关参数 当前页和每页显示的条数
        PageHelper.startPage(1,2);
        List<User> users = mapper.findAll();
        for (User user : users) {
            System.out.println(user);
        }
        //获得与分页相关的参数
        PageInfo<User> pageInfo = new PageInfo<>(users);
        System.out.println(pageInfo.getPageNum());
        System.out.println(pageInfo.getPageSize());
        System.out.println(pageInfo.getTotal());
        System.out.println(pageInfo.getPages());
        System.out.println(pageInfo.getPrePage());
        System.out.println(pageInfo.getNextPage());
        System.out.println(pageInfo.isIsFirstPage());
        System.out.println(pageInfo.isIsLastPage());
    }
}
```



## Mybatis多表查询

###  一对一查询

对应的sql语句：select * from orders o,user u where o.uid=u.id;

**创建Order和User实体**

```java
package com.itheima.domain;

import java.util.Date;
import java.util.List;

public class User {

    private int id;
    private String username;
    private String password;
    private Date birthday;

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
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

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", birthday=" + birthday +
                '}';
    }
}
```

```java
package com.itheima.domain;

import java.util.Date;

public class Order {

    private int id;
    private Date ordertime;
    private double total;

    //当前订单属于哪一个用户
    private User user;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public Date getOrdertime() {
        return ordertime;
    }

    public void setOrdertime(Date ordertime) {
        this.ordertime = ordertime;
    }

    public double getTotal() {
        return total;
    }

    public void setTotal(double total) {
        this.total = total;
    }

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }

    @Override
    public String toString() {
        return "Order{" +
                "id=" + id +
                ", ordertime=" + ordertime +
                ", total=" + total +
                ", user=" + user +
                '}';
    }
}
```

**创建OrderMapper接口**

```java
package com.itheima.mapper;

import com.itheima.domain.User;

import java.util.List;

public interface UserMapper {

    public List<User> findAll();

    public List<User> findUserAndRoleAll();

}
```

**配置OrderMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itheima.mapper.OrderMapper">
    <resultMap id="orderMap" type="order">
        <!-- 手动指定字段与实体属性的映射关系 -->
        <!-- column:数据表的字段名 property:实体的属性名称 -->
        <id column="oid" property="id"></id>
        <result column="ordertime" property="ordertime"></result>
        <result column="total" property="total"></result>
<!--        <result column="uid" property="user.id"></result>-->
<!--        <result column="username" property="user.username"></result>-->
<!--        <result column="password" property="user.password"></result>-->
<!--        <result column="birthday" property="user.birthday"></result>-->
        <!-- property:当前实体(order)中的属性名称(private User user) JavaType:当前实体中的属性的类型 -->
        <association property="user" javaType="user">
            <id column="uid" property="id"></id>
            <result column="username" property="username"></result>
            <result column="password" property="password"></result>
            <result column="birthday" property="birthday"></result>
        </association>
    </resultMap>
    <select id="findAll" resultMap="orderMap">
        select *,o.id oid from orders o,user u where o.uid = u.id
    </select>
</mapper>
```

**测试结果**

```java
@Test
public void test1() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    OrderMapper mapper = sqlSession.getMapper(OrderMapper.class);
    List<Order> orderList = mapper.findAll();
    for (Order order : orderList) {
        System.out.println(order);
    }

    sqlSession.close();
}
```



### 一对多查询

select *,o.id oid from user u left join orders o on u.id=o.uid;

**修改User实体**

```java
package com.itheima.domain;

import java.util.Date;
import java.util.List;

public class User {

    private int id;
    private String username;
    private String password;
    private Date birthday;

    //描述的是当前用户存在哪些订单
    private List<Order> orderList;

    public List<Order> getOrderList() {
        return orderList;
    }

    public void setOrderList(List<Order> orderList) {
        this.orderList = orderList;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
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

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", birthday=" + birthday +
                '}';
    }
}
```

**创建UserMapper接口**

```java
package com.itheima.mapper;

import com.itheima.domain.User;

import java.util.List;

public interface UserMapper {

    public List<User> findAll();

    public List<User> findUserAndRoleAll();

}
```

 **配置UserMapper.xml**

```xml
<resultMap id="userMap" type="user">
    <id column="uid" property="id"></id>
    <result column="username" property="username"></result>
    <result column="password" property="password"></result>
    <result column="birthday" property="birthday"></result>
    <!--配置集合信息
            property:集合名称
            ofType：当前集合中的数据类型
        -->
    <collection property="orderList" ofType="order">
        <!--封装order的数据-->
        <id column="oid" property="id"></id>
        <result column="ordertime" property="ordertime"></result>
        <result column="total" property="total"></result>
    </collection>
</resultMap>

<select id="findAll" resultMap="userMap">
    SELECT *,o.id oid FROM USER u,orders o WHERE u.id=o.uid
</select>
```

**测试结果**

```java
@Test
public void test2() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<User> userList = mapper.findAll();
    for (User user : userList) {
        System.out.println(user);
    }

    sqlSession.close();
}
```



### 多对多查询

**SELECT * FROM USER u,sys_user_role ur,sys_role r WHERE u.id=ur.userId AND ur.roleId=r.id**

 **创建Role实体，修改User实体**

```java
package com.itheima.domain;

import java.util.Date;
import java.util.List;

public class User {

    private int id;
    private String username;
    private String password;
    private Date birthday;

    //描述的是当前用户存在哪些订单
    private List<Order> orderList;

    //描述的是当前用户具备哪些角色
    private List<Role> roleList;

    public List<Role> getRoleList() {
        return roleList;
    }

    public void setRoleList(List<Role> roleList) {
        this.roleList = roleList;
    }

    public List<Order> getOrderList() {
        return orderList;
    }

    public void setOrderList(List<Order> orderList) {
        this.orderList = orderList;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
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

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", birthday=" + birthday +
                ", roleList=" + roleList +
                '}';
    }
}
```

```java
package com.itheima.domain;

public class Role {

    private int id;
    private String roleName;
    private String roleDesc;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getRoleName() {
        return roleName;
    }

    public void setRoleName(String roleName) {
        this.roleName = roleName;
    }

    public String getRoleDesc() {
        return roleDesc;
    }

    public void setRoleDesc(String roleDesc) {
        this.roleDesc = roleDesc;
    }

    @Override
    public String toString() {
        return "Role{" +
                "id=" + id +
                ", roleName='" + roleName + '\'' +
                ", roleDesc='" + roleDesc + '\'' +
                '}';
    }
}
```

**添加UserMapper接口方法**

```java
package com.itheima.mapper;

import com.itheima.domain.User;

import java.util.List;

public interface UserMapper {

    public List<User> findAll();

    public List<User> findUserAndRoleAll();

}
```

**配置UserMapper.xml**

```xml
<resultMap id="userRoleMap" type="user">
    <!--user的信息-->
    <id column="userId" property="id"></id>
    <result column="username" property="username"></result>
    <result column="password" property="password"></result>
    <result column="birthday" property="birthday"></result>
    <!--user内部的roleList信息-->
    <collection property="roleList" ofType="role">
        <id column="roleId" property="id"></id>
        <result column="roleName" property="roleName"></result>
        <result column="roleDesc" property="roleDesc"></result>
    </collection>
</resultMap>

<select id="findUserAndRoleAll" resultMap="userRoleMap">
    SELECT * FROM USER u,sys_user_role ur,sys_role r WHERE u.id=ur.userId AND ur.roleId=r.id
</select>
```

**测试结果**

```java
@Test
public void test1() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    OrderMapper mapper = sqlSession.getMapper(OrderMapper.class);
    List<Order> orderList = mapper.findAll();
    for (Order order : orderList) {
        System.out.println(order);
    }

    sqlSession.close();
}
```



### 总结

**MyBatis多表配置方式：**

**一对一配置：使用<resultMap>做配置**

**一对多配置：使用<resultMap>+<collection>做配置**

**多对多配置：使用<resultMap>+<collection>做配置**



## Mybatis的注解开发

### MyBatis的常用注解

**这几年来注解开发越来越流行，Mybatis也可以使用注解开发方式，这样我们就可以减少编写Mapper**

**映射文件了。我们先围绕一些基本的CRUD来学习，再学习复杂映射多表操作。**

**@Insert：实现新增**

**@Update：实现更新**

**@Delete：实现删除**

**@Select：实现查询**

**@Result：实现结果集封装**

**@Results：可以与@Result 一起使用，封装多个结果集**

**@One：实现一对一结果集封装**

**@Many：实现一对多结果集封装**



### 注解开发

**编写UserMapper**

```java
package com.itheima.mapper;

import com.itheima.domain.User;
import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;

import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-21:26
 */
public interface UserMapper {
    @Insert("insert into user values(#{id},#{username},#{password},#{birthday})")
    public void save(User user);
    @Update("update user set username=#{username},password=#{password} where id=#{id}")
    public void update(User user);
    @Delete("delete from user where id=#{id}")
    public void delete(int id);
    @Select("select * from user where id=#{id}")
    public User findById(int id);
    @Select("select * from user")
    public List<User> findAll();
}

```

**修改SqlMapConfig配置文件**

```xml
<!-- 加载映射关系 -->
<mappers>
    <!-- 指定接口所在的包 -->
    <package name="com.itheima.mapper"/>
</mappers>
```

**测试**

```java
package com.itheima.test;

import com.itheima.domain.User;
import com.itheima.mapper.UserMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Before;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;

/**
 * @author yh
 * @create 2021-10-31-21:28
 */
public class MyBatisTest {
    private UserMapper mapper;
    @Before
    public void before() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        mapper = sqlSession.getMapper(UserMapper.class);
    }

    @Test
    public void testSave() {
        User user = new User();
        user.setUsername("yzl");
        user.setPassword("456");
        mapper.save(user);
    }
}
```



###  MyBatis的注解实现复杂映射开发

实现复杂关系映射之前我们可以在映射文件中通过配置<resultMap>来实现，使用注解开发后，我们可以使用@Results注解，@Result注解，@One注解，@Many注解组合完成复杂关系的配置

![image-20211031223056478](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031223056478.png)

![image-20211031223105935](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211031223105935.png)

####  一对一查询

对应的sql语句：

select * from orders;

select * from user where id=查询出订单的uid;

**创建Order和User实体**

```java
package com.itheima.domain;

import java.util.Date;

/**
 * @author yh
 * @create 2021-10-31-21:57
 */
public class Order {
    private int id;
    private Date ordertime;
    private double total;

    //当前订单属于哪一个用户
    private User user;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public Date getOrdertime() {
        return ordertime;
    }

    public void setOrdertime(Date ordertime) {
        this.ordertime = ordertime;
    }

    public double getTotal() {
        return total;
    }

    public void setTotal(double total) {
        this.total = total;
    }

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }

    @Override
    public String toString() {
        return "Order{" +
                "id=" + id +
                ", ordertime=" + ordertime +
                ", total=" + total +
                ", user=" + user +
                '}';
    }
}

```

```java
package com.itheima.domain;

import java.util.Date;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-21:25
 */
public class User {
    private Integer id;
    private String username;
    private String password;
    private List<Order> orderList;
    private List<Role> roleList;

    public List<Order> getOrderList() {
        return orderList;
    }

    public void setOrderList(List<Order> orderList) {
        this.orderList = orderList;
    }

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


    public User() {
    }

    public User(Integer id, String username, String password) {
        this.id = id;
        this.username = username;
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", orderList=" + orderList +
                ", roleList=" + roleList +
                '}';
    }

    public List<Role> getRoleList() {
        return roleList;
    }

    public void setRoleList(List<Role> roleList) {
        this.roleList = roleList;
    }
}

```

**创建OrderMapper接口**

```java
@Select("select *,o.id oid from orders o,user u where o.uid=u.id")
@Results({
    @Result(column = "oid",property = "id"),
    @Result(column = "ordertime",property = "ordertime"),
    @Result(column = "total",property = "total"),
    @Result(column = "uid",property = "user.id"),
    @Result(column = "username",property = "user.username"),
    @Result(column = "password",property = "user.password"),
})
public List<Order> findAll();
```

**另一种写法**

```java
@Select("select * from orders")
@Results({
    @Result(column = "id",property = "id"),
    @Result(column = "ordertime",property = "ordertime"),
    @Result(column = "total",property = "total"),
    @Result(
        property = "user", //要封装的属性名称
        column = "uid", //根据那个字段去查询user表的信息
        javaType = User.class,
        one = @One(select = "com.itheima.mapper.UserMapper.findById")
    )
})
public List<Order> findAll();
```

**测试**

```java
package com.itheima.test;

import com.itheima.domain.Order;
import com.itheima.mapper.OrderMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Before;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-21:28
 */
public class MyBatisTest2 {
    private OrderMapper mapper;
    @Before
    public void before() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        mapper = sqlSession.getMapper(OrderMapper.class);
    }

    @Test
    public void testSave() {
        List<Order> orders = mapper.findAll();
        for (Order order : orders) {
            System.out.println(order);
        }
    }
}
```



#### 一对多查询

对应的sql语句：

select * from user;

select * from orders where uid=查询出用户的id;

```java
package com.itheima.domain;

import java.util.Date;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-21:25
 */
public class User {
    private Integer id;
    private String username;
    private String password;
    private List<Order> orderList;
    private List<Role> roleList;

    public List<Order> getOrderList() {
        return orderList;
    }

    public void setOrderList(List<Order> orderList) {
        this.orderList = orderList;
    }

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


    public User() {
    }

    public User(Integer id, String username, String password) {
        this.id = id;
        this.username = username;
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", orderList=" + orderList +
                ", roleList=" + roleList +
                '}';
    }

    public List<Role> getRoleList() {
        return roleList;
    }

    public void setRoleList(List<Role> roleList) {
        this.roleList = roleList;
    }
}

```

```java
package com.itheima.domain;

import java.util.Date;

/**
 * @author yh
 * @create 2021-10-31-21:57
 */
public class Order {
    private int id;
    private Date ordertime;
    private double total;

    //当前订单属于哪一个用户
    private User user;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public Date getOrdertime() {
        return ordertime;
    }

    public void setOrdertime(Date ordertime) {
        this.ordertime = ordertime;
    }

    public double getTotal() {
        return total;
    }

    public void setTotal(double total) {
        this.total = total;
    }

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }

    @Override
    public String toString() {
        return "Order{" +
                "id=" + id +
                ", ordertime=" + ordertime +
                ", total=" + total +
                ", user=" + user +
                '}';
    }
}
```

**创建UserMapper接口**

```java
@Select("select * from user")
@Results(
    {
        @Result(column = "id",property = "id"),
        @Result(column = "username",property = "username"),
        @Result(column = "password",property = "password"),
        @Result(
            property = "orderList",
            column = "id",
            javaType = List.class,
            many = @Many(select = "com.itheima.mapper.OrderMapper.findByUid")
        )
    }
)
public List<User> findUserAndOrderAll();
```

```java
@Select("select * from orders where uid=#{uid}")
public List<Order> findByUid(int uid);
```

**测试结果**

```java
@Test
public void testSave() {
    List<User> users = mapper.findUserAndOrderAll();
    for (User user : users) {
        System.out.println(user);
    }
}
```



#### 多对多查询

对应的sql语句：

select * from user;

select * from role r,user_role ur where r.id=ur.role_id and ur.user_id=用户的id

```java
package com.itheima.domain;

public class Role {

    private int id;
    private String roleName;
    private String roleDesc;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getRoleName() {
        return roleName;
    }

    public void setRoleName(String roleName) {
        this.roleName = roleName;
    }

    public String getRoleDesc() {
        return roleDesc;
    }

    public void setRoleDesc(String roleDesc) {
        this.roleDesc = roleDesc;
    }

    @Override
    public String toString() {
        return "Role{" +
                "id=" + id +
                ", roleName='" + roleName + '\'' +
                ", roleDesc='" + roleDesc + '\'' +
                '}';
    }
}
```

```java
package com.itheima.domain;

import java.util.Date;
import java.util.List;

/**
 * @author yh
 * @create 2021-10-31-21:25
 */
public class User {
    private Integer id;
    private String username;
    private String password;
    private List<Order> orderList;
    private List<Role> roleList;

    public List<Order> getOrderList() {
        return orderList;
    }

    public void setOrderList(List<Order> orderList) {
        this.orderList = orderList;
    }

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


    public User() {
    }

    public User(Integer id, String username, String password) {
        this.id = id;
        this.username = username;
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", orderList=" + orderList +
                ", roleList=" + roleList +
                '}';
    }

    public List<Role> getRoleList() {
        return roleList;
    }

    public void setRoleList(List<Role> roleList) {
        this.roleList = roleList;
    }
}
```

**添加UserMapper接口方法**

```java
@Select("select * from user")
@Results({
    @Result(column = "id",property = "id"),
    @Result(column = "username",property = "username"),
    @Result(column = "password",property = "password"),
    @Result(
        property = "roleList",
        column = "id",
        javaType = List.class,
        many = @Many(select = "com.itheima.mapper.RoleMapper")
    )
})
public List<User> findUserAndRoleAll();
```

```java
public interface RoleMapper {
    @Select("select * from user_role ur,role r where ur.roleId = r.id and ur.userId=#{uid}")
    public List<Role> findByUid(int uid);
}
```

**多对多查询**

```java
@Test
public void testSave2() {
    List<User> userAndRoleAll = mapper.findUserAndRoleAll();
    for (User user : userAndRoleAll) {
        System.out.println(user);
    }
}
```

