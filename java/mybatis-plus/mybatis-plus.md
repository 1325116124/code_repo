



# mybatis-plus

## 创建项目

![image-20211114200340025](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211114200340025.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.itcast.mp</groupId>
    <artifactId>itcast-mybatis-plus</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>itcast-mybatis-plus-simple</module>
    </modules>

    <properties>
        <maven.compiler.source>16</maven.compiler.source>
        <maven.compiler.target>16</maven.compiler.target>
    </properties>
    <dependencies>
        <!-- mybatis-plus插件依赖 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus</artifactId>
            <version>3.1.1</version>
        </dependency>
        <!-- MySql -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!-- 连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.11</version>
        </dependency>
        <!--简化bean代码的工具包-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
            <version>1.18.4</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.6.4</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

**创建子项目**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>itcast-mybatis-plus</artifactId>
        <groupId>cn.itcast.mp</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>itcast-mybatis-plus-simple</artifactId>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <properties>
        <maven.compiler.source>16</maven.compiler.source>
        <maven.compiler.target>16</maven.compiler.target>
    </properties>
</project>
```

**mybatis-config.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/mp>
                <property name="username" value="root"/>
                <property name="password" value="yanghong"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
```

**User**

```java
package cn.itcast.mp.simple.pojo;

import com.baomidou.mybatisplus.annotation.TableName;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {

    private Long id;
    private String userName;
    private String password;
    private String name;
    private Integer age;
    private String email;
}
```

**UserMapper**

```java
package cn.itcast.mp.simple.mapper;
import cn.itcast.mp.simple.pojo.User;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import java.util.List;

public interface UserMapper extends BaseMapper<User> {
    List<User> findAll();
}
```

**测试mybatis**

```java
package cn.itcast.mp.simple;

import cn.itcast.mp.simple.mapper.UserMapper;
import cn.itcast.mp.simple.pojo.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;
import java.io.InputStream;
import java.util.List;

public class TestMybatis {

    @Test
    public void testFindAll() throws Exception{

        String config = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(config);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

        //测试查询
        List<User> users = userMapper.findAll();
        for (User user : users) {
            System.out.println(user);
        }
    }
}
```



## mybatis整合mybatis-plus

第一步，将UserMapper继承BaseMapper，将拥有了BaseMapper中的所有方法：

```java
package cn.itcast.mp.simple.mapper;

import cn.itcast.mp.simple.pojo.User;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import java.util.List;

public interface UserMapper extends BaseMapper<User> {
    List<User> findAll();
}
```

第二步，使用MP中的MybatisSqlSessionFactoryBuilder进程构建：

```java
package cn.itcast.mp.simple;

import cn.itcast.mp.simple.mapper.UserMapper;
import cn.itcast.mp.simple.pojo.User;
import com.baomidou.mybatisplus.core.MybatisSqlSessionFactoryBuilder;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.InputStream;
import java.util.List;

public class TestMybatisPlus {

    @Test
    public void testFindAll() throws Exception{

        String config = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(config);
        SqlSessionFactory sqlSessionFactory = new MybatisSqlSessionFactoryBuilder().build(inputStream);

        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

        //测试查询
//        List<User> users = userMapper.findAll();
        List<User> users = userMapper.selectList(null);
        for (User user : users) {
            System.out.println(user);
        }
    }
}
```

在User对象中添加@TableName，指定数据库表名

```java
package cn.itcast.mp.simple.pojo;

import com.baomidou.mybatisplus.annotation.TableName;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
@TableName("tb_user")
public class User {

    private Long id;
    private String userName;
    private String password;
    private String name;
    private Integer age;
    private String email;
}
```

**小结**

由于使用了MybatisSqlSessionFactoryBuilder进行了构建，继承的BaseMapper中的方法就载入到了SqlSession中，所以就可以直接使用相关的方法；



## spring整合mybatis-plus

引入了Spring框架，数据源、构建等工作就交给了Spring管理。

![image-20211115110845077](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211115110845077.png)

**创建子项目**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>itcast-mybatis-plus</artifactId>
        <groupId>cn.itcast.mp</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <properties>
        <spring.version>5.1.6.RELEASE</spring.version>
    </properties>
    <artifactId>itcast-mybatis-plus-spring</artifactId>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.3.9</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>5.3.9</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

**实现查询User**

第一步，编写jdbc.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/mp
jdbc.username=root
jdbc.password=yanghong
```

第二步，编写applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/context ">

    <context:property-placeholder location="classpath:*.properties"/>

    <!-- 定义数据源 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
          destroy-method="close">
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="maxActive" value="10"/>
        <property name="minIdle" value="5"/>
    </bean>

    <!--这里使用MP提供的sqlSessionFactory，完成了Spring与MP的整合-->
    <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--扫描mapper接口，使用的依然是Mybatis原生的扫描器-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="cn.itcast.mp.simple.mapper"/>
    </bean>

</beans>
```

第三步，编写User对象以及UserMapper接口：

```java
package cn.itcast.mp.simple.pojo;

import com.baomidou.mybatisplus.annotation.TableName;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
@TableName("tb_user")
public class User {

    private Long id;
    private String userName;
    private String password;
    private String name;
    private Integer age;
    private String email;
}
```

```java
package cn.itcast.mp.simple.mapper;

import cn.itcast.mp.simple.pojo.User;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;

public interface UserMapper extends BaseMapper<User> {

}
```

第四步，编写测试用例：

```java
package java.cn.itcast.mp.simple;



import cn.itcast.mp.simple.mapper.UserMapper;
import cn.itcast.mp.simple.pojo.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.List;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
public class TestMybatisSpring {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelectList(){
        List<User> users = this.userMapper.selectList(null);
        for (User user : users) {
            System.out.println(user);
        }
    }
}
```



## springboot整合mybatis-plus

使用SpringBoot将进一步的简化MP的整合，需要注意的是，由于使用SpringBoot需要继承parent，所以需要重新创建工程，并不是创建子Module。

**导入依赖**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.itcast.mp</groupId>
    <artifactId>itcast-mp-springboot</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.4.RELEASE</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!--简化代码的工具包-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.20</version>
            <optional>true</optional>
        </dependency>
        <!--mybatis-plus的springboot支持-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.1.1</version>
        </dependency>
        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <properties>
        <maven.compiler.source>16</maven.compiler.source>
        <maven.compiler.target>16</maven.compiler.target>
    </properties>

</project>
```

log4j.properties： 

```properties
log4j.rootLogger=DEBUG,A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=[%t] [%c]-[%p] %m%n
```

**编写application.properties**

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/mp
spring.datasource.username=root
spring.datasource.password=yanghong
```

**编写pojo**

```java
package cn.itcast.mp.pojo;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
@TableName("tb_user")
public class User {
    private Long id;
    private String userName;
    private String password;
    private String name;
    private Integer age;
    private String email;
}
```

**编写mapper**

```java
package cn.itcast.mp.mapper;

import cn.itcast.mp.pojo.User;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;

public interface UserMapper extends BaseMapper<User> {

}
```

**编写启动类**

```java
package cn.itcast.mp;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@MapperScan("cn.itcast.mp.mapper")
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

**编写测试用例**

```java
package cn.itcast.mp;


import cn.itcast.mp.mapper.UserMapper;
import cn.itcast.mp.pojo.User;
import org.junit.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

/**
 * @author yh
 * @create 2021-11-15-11:17
 */
@SpringBootTest
public class TestMybatisSpringBoot {
    @Autowired
    private UserMapper userMapper;

    @Test
    public void test() {
        List<User> users = userMapper.selectList(null);
        for (User user:users) {
            System.out.println(user);
        }
    }
}
```



## 通用CRUD

通过前面的学习，我们了解到通过继承BaseMapper就可以获取到各种各样的单表操作，接下来我们将详细讲解这些操作。

![image-20211115133430028](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211115133430028.png)

**插入操作**

```java
package cn.itcast.mp;

import cn.itcast.mp.mapper.UserMapper;
import cn.itcast.mp.pojo.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

/**
 * @author yh
 * @create 2021-11-15-11:50
 */
@RunWith(SpringRunner.class)
@SpringBootTest
public class TestUserMapper {
    @Autowired
    private UserMapper userMapper;

    @Test
    public void testInsert() {
        User user = new User();
        user.setAge(20);
        user.setEmail("test@itcast.cn");
        user.setName("曹操");
        user.setUserName("caocao");
        user.setPassword("123456");
        int result = this.userMapper.insert(user); //返回的result是受影响的行数，并不是自增 后的id
        System.out.println("result = " + result);
        System.out.println(user.getId()); //自增后的id会回填到对象中
    }
}

```

![image-20211115133657503](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211115133657503.png)

可以看到，数据已经写入到了数据库，但是，id的值不正确，我们期望的是数据库自增长，实际是MP生成了id的值写入到了数据库。

如何设置id的生成策略呢？

MP支持的id策略：

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package com.baomidou.mybatisplus.annotation;

public enum IdType {
    /**
    数据库id自增
    */
    AUTO(0),
    /**
    该类型为未设置主键类型
    */
    NONE(1),
    /**
    * 用户输入ID * <p>该类型可以通过自己注册自动填充插件进行填充</p> 
    */
    INPUT(2),
    /* 
    以下3种类型、只有当插入对象ID 为空，才自动填充。
    */ 
    /**
    * 全局唯一ID (idWorker) 
    */
    ID_WORKER(3),
    /**
    * 全局唯一ID (UUID) 
    */
    UUID(4),
    /**
    * 字符串全局唯一ID (idWorker 的字符串表示) 
    */
    ID_WORKER_STR(5);

    private final int key;

    private IdType(int key) {
        this.key = key;
    }

    public int getKey() {
        return this.key;
    }
}

```

修改User对象(使用数据库自增id)：

```java
package cn.itcast.mp.pojo;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;
    private String userName;
    private String password;
    private String name;
    private Integer age;
    private String email;
}
```

![image-20211115134217072](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211115134217072.png)



## @TableField字段

在MP中通过@TableField注解可以指定字段的一些属性，常常解决的问题有2个：

1、对象中的属性名和字段名不一致的问题（非驼峰）

2、对象中的属性字段在表中不存在的问题

```java
package cn.itcast.mp.pojo;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;
    private String userName;
    private String password;
    private String name;
    private Integer age;
    @TableField("email")
    private String mail;
    @TableField(exist = false)
    private String address;
}
```

其他用法，某字段不参与查询：

```java
package cn.itcast.mp.pojo;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;
    private String userName;
    @TableField(select = false)
    private String password;
    private String name;
    private Integer age;
    @TableField("email")
    private String mail;
    @TableField(exist = false)
    private String address;
}
```



## 更新操作

**更新操作**

在MP中，更新操作有2种，一种是根据id更新，另一种是根据条件更新。

### 根据id更新(updateById)

```java
@Test
public void testUpdateById() {
    User user = new User();
    user.setId(1L);
    user.setAge(19);
    user.setPassword("888888");
    int result = this.userMapper.updateById(user);
    System.out.println(result);
}
```

### 根据条件更新(update)

**QueryWrapper或者UpdateWrapper**

```java
@Test
public void testUpdate() {
    User user = new User();
    user.setAge(20);
    user.setPassword("888888");
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.eq("user_name","zhangsan");
    int result = userMapper.update(user,wrapper);
    System.out.println(result);
}
```

```java
@Test
public void testUpdate2() {
    UpdateWrapper<User> wrapper = new UpdateWrapper<>();
    wrapper.set("age",21).set("password","444444") //更新的字段
        .eq("user_name","zhangsan"); //更新的条件
    int result = userMapper.update(null, wrapper);
    System.out.println(result);
}
```



## 删除操作

### deleteById

```java
@Test
public void testDeleteById() {
    int result = userMapper.deleteById(6L);
    System.out.println(result);
}
```



### deleteByMap

```java
@Test
public void testDeleteByMap() {
    Map<String,Object> columnMap = new HashMap<>();
    columnMap.put("age",20);
    columnMap.put("name","张三");
    //将columnMap中的元素设置为删除的条件，多个之间为and关系
    int result = userMapper.deleteByMap(columnMap);
    System.out.println(result);
}
```



### delete

```java
@Test
public void testDelete() {
    User user = new User();
    user.setName("lisi");
    user.setAge(12);
    //将实体对象进行包装，包装为操作条件
    QueryWrapper<User> wrapper = new QueryWrapper<>(user);
    int result = userMapper.delete(wrapper);
    System.out.println(result);
}
```



### deleteBatchIds(根据id批量删除)

```java
@Test
public void testDeleteBatchId() {
    int result = userMapper.deleteBatchIds(Arrays.asList(1L, 2L, 3L));
    System.out.println(result);
}
```



## 查询操作

### selectById

```java
@Test
public void testSelectById() {
    User user = userMapper.selectById(1L);
    System.out.println(user);
}
```



### selectBatchIds

```java
@Test
public void testSelectBatchIds() {
    List<User> users = userMapper.selectBatchIds(Arrays.asList(1L, 2L, 5L));
    for (User user : users) {
        System.out.println(user);
    }
}
```



### selectOne

```java
@Test
public void testSelectOne() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.eq("age",48);
    //根据条件查询一条数据，如果结果超过一条会报错
    User user = userMapper.selectOne(wrapper);
    System.out.println(user);
}
```



### selectCount

```java
@Test
public void testSelectCount() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.gt("age",24);
    Integer result = userMapper.selectCount(wrapper);
    System.out.println(result);
}
```



### selectList

```java
@Test
public void testSelectList() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.gt("age",42);
    List<User> users = userMapper.selectList(wrapper);
    for (User user : users) {
        System.out.println(user);
    }
}
```



### selectPage

```java
@Test
public void testSelectPage() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.gt("age",24);
    Page<User> page = new Page<>(2,1);
    //根据条件查询数据
    IPage<User> userIPage = userMapper.selectPage(page, wrapper);
    System.out.println(userIPage.getPages());
    System.out.println(userIPage.getTotal());
    List<User> records = userIPage.getRecords();
    for (User record : records) {
        System.out.println(record);
    }
}
```



## 基本配置

### confifigLocation

MyBatis 配置文件位置，如果您有单独的 MyBatis 配置，请将其路径配置到 confifigLocation 中。 MyBatisConfifiguration 的具体内容请参考MyBatis 官方文档

**springboot**

```properties
mybatis-plus.config-location = classpath:mybatis-config.xml
```

**springmvc**

```xml
<!--这里使用MP提供的sqlSessionFactory，完成了Spring与MP的整合-->
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="configLocation" value="classpath:mybatis-config.xml"></property>
</bean>
```



### mapperLocations

MyBatis Mapper 所对应的 XML 文件位置，如果您在 Mapper 中有自定义方法（XML 中有自定义实现），需要进行该配置，告诉 Mapper 所对应的 XML 文件位置。

**Spring Boot：**

```properties
mybatis-plus.mapper-locations = classpath*:mybatis/*.xml
```

**Spring MVC：**

```xml
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="mapperLocations" value="classpath*:mybatis/*.xml"/> 
</bean>
```

**UserMapper.xml：**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.itcast.mp.mapper.UserMapper">
    <select id="findById" resultType="cn.itcast.mp.pojo.User">
        select * from tb_user where id = #{id}
    </select>
</mapper>
```

**UserMapper**

```java
package cn.itcast.mp.mapper;

import cn.itcast.mp.pojo.User;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserMapper extends BaseMapper<User> {
    User findById(Long id);
}
```

测试用例:

```java
@Test
public void testFindById() {
    User user = userMapper.findById(1L);
    System.out.println(user);
}
```



### typeAliasesPackage

MyBaits 别名包扫描路径，通过该属性可以给包中的类注册别名，注册后在 Mapper 对应的 XML 文件中可以直接使用类名，而不用使用全限定的类名（即 XML 中调用的时候不用包含包名）。

**springboot**

```properties
mybatis-plus.type-aliases-package = cn.itcast.mp.pojo
```

**Spring MVC：**

```xml
<bean id="sqlSessionFactory"class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean"> 
    <property name="typeAliasesPackage" value="com.baomidou.mybatisplus.samples.quickstart.entity"/> </bean>
```



## 进阶配置

本部分（Confifiguration）的配置大都为 MyBatis 原生支持的配置，这意味着您可以通过 MyBatis XML 配置文件的形式进行配置。

### mapUnderscoreToCamelCase

- 类型： boolean

- 默认值： true

是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN（下划线命名） 到经典 Java 属性名 aColumn（驼峰命名） 的类似映射。

```properties
#关闭自动驼峰映射，该参数不能和mybatis-plus.config-location同时存在 
mybatis-plus.configuration.map-underscore-to-camel-case=false
```

### cacheEnabled

- 类型： boolean

- 默认值： true

全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存，默认为 true。

```properties
mybatis-plus.configuration.cache-enabled=false
```



## DB策略配置

### idType

类型： com.baomidou.mybatisplus.annotation.IdType

默认值： ID_WORKER

全局默认主键类型，设置后，即可省略实体对象中的@TableId(type = IdType.AUTO)配置。

SpringBoot：

```properties
mybatis-plus.global-config.db-config.id-type=auto
```

SpringMVC：

```xml
<!--这里使用MP提供的sqlSessionFactory，完成了Spring与MP的整合-->
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="configLocation" value="classpath:mybatis-config.xml"></property>
    <property name="globalConfig">
        <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig">
            <property name="dbConfig">
                <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig">
                    <property name="idType" value="AUTO"/>
                </bean>
            </property>
        </bean>
    </property>
</bean>
```



### tablePrefifix

类型： String

默认值： null 

表名前缀，全局配置后可省略@TableName()配置。

SpringBoot：

```properties
mybatis-plus.global-config.db-config.table-prefix=tb_
```

SpringMVC： 

```xml
<!--这里使用MP提供的sqlSessionFactory，完成了Spring与MP的整合-->
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="configLocation" value="classpath:mybatis-config.xml"></property>
    <property name="globalConfig">
        <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig">
            <property name="dbConfig">
                <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig">
                    <property name="idType" value="AUTO"/>
                    <property name="tablePrefix" value="tb_"/>
                </bean>
            </property>
        </bean>
    </property>
</bean>
```



## 条件构造器

### allEq

```java
allEq(Map<R, V> params) 
allEq(Map<R, V> params, boolean null2IsNull) 
allEq(boolean condition, Map<R, V> params, boolean null2IsNull)
```

**全部eq(或个别isNull)**

**个别参数说明: params : key 为数据库字段名, value 为字段值 null2IsNull : 为 true 则在 map 的 value 为** **null 时调用 isNull 方法,为 false 时则忽略 value 为 null 的** 

**例1: allEq({id:1,name:"老王",age:null}) ---> id = 1 and name = '老王' and age is null** 

**例2: allEq({id:1,name:"老王",age:null}, false) ---> id = 1 and name = '老王'**

**个别参数说明: filter : 过滤函数,是否允许字段传入比对条件中 params 与 null2IsNull : 同上**

**例1: allEq((k,v) -> k.indexOf("a") > 0, {id:1,name:"老王",age:null}) ---> name = '老王' and age is null** 

**例2: allEq((k,v) -> k.indexOf("a") > 0, {id:1,name:"老王",age:null}, false) ---> name = '老王'**

测试:

```java
@Test
public void testAllEq() {
    Map<String,Object> map = new HashMap<>();
    map.put("name","张三");
    map.put("age",20);
    map.put("password","8888");
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    //        wrapper.allEq(map);
    //        wrapper.allEq(map,false);
    wrapper.allEq((k,v) -> (k.equals("age")) || (k.equals("name")),map);
    List<User> users = userMapper.selectList(wrapper);
    for (User user : users) {
        System.out.println(user);
    }
}
```



### 基本比较操作

- eq ：等于 =

- ne：不等于 <>

- gt：大于 >

- ge：大于等于 >=

- lt：小于 <

- le：小于等于 <=

- between：BETWEEN 值1 AND 值2

- notBetween：NOT BETWEEN 值1 AND 值2

- in：字段 IN (value.get(0), value.get(1), ...)

- notIn：字段 NOT IN (v0, v1, ...)

```java
@Test
public void testEq() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.eq("password","123456").ge("age",20).in("name","李四","王五","赵六");
    List<User> users = userMapper.selectList(wrapper);
    for (User user : users) {
        System.out.println(user);
    }
}
```



### 模糊查询

like：LIKE '%值%'

- 例: like("name", "王") ---> name like '%王%'

notLike：NOT LIKE '%值%'

- 例: notLike("name", "王") ---> name not like '%王%'

likeLeft：LIKE '%值' 

- 例: likeLeft("name", "王") ---> name like '%王'

likeRight：LIKE '值%'

- 例: likeRight("name", "王") ---> name like '王%'

```java
@Test
public void testWrapper() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.like("name","操");
    List<User> users = userMapper.selectList(wrapper);
    for (User user : users) {
        System.out.println(user);
    }
}
```



### 排序

orderBy：排序：ORDER BY 字段, ...

- 例: orderBy(true, true, "id", "name") ---> order by id ASC,name ASC

orderByAsc：排序：ORDER BY 字段, ... ASC

- 例: orderByAsc("id", "name") ---> order by id ASC,name ASC

orderByDesc：排序：ORDER BY 字段, ... DESC

- 例: orderByDesc("id", "name") ---> order by id DESC,name DESC

```java
@Test
public void testWrapper2() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.orderByAsc("age");
    List<User> users = userMapper.selectList(wrapper);
    for (User user : users) {
        System.out.println(user);
    }
}
```



### 逻辑查询

or：拼接 OR

- 主动调用 or 表示紧接着下一个**方法**不是用 and 连接!(不调用 or 则默认为使用 and 连接) 

and：AND 嵌套

- 例: and(i -> i.eq("name", "李白").ne("status", "活着")) ---> and (name = '李白' and status <> '活着')

```java
@Test
public void testWrapper3() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.eq("name"，"李四").or().eq("age",24);
    List<User> users = userMapper.selectList(wrapper);
    for (User user : users) {
        System.out.println(user);
    }
}
```



### select

在MP查询中，默认查询所有的字段，如果有需要也可以通过select方法进行指定字段。

```java
@Test
public void testWrapper3() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.eq("name"，"李四").or().eq("age",24).select("name","age","id");
    List<User> users = userMapper.selectList(wrapper);
    for (User user : users) {
        System.out.println(user);
    }
}
```



## ActiveRecord

ActiveRecord（简称AR）一直广受动态语言（ PHP 、 Ruby 等）的喜爱，而 Java 作为准静态语言，对于ActiveRecord 往往只能感叹其优雅，所以我们也在 AR 道路上进行了一定的探索，喜欢大家能够喜欢。

>什么是ActiveRecord？

> ActiveRecord也属于ORM（对象关系映射）层，由Rails最早提出，遵循标准的ORM模型：表映射到记录，记录映射到对象，字段映射到对象属性。配合遵循的命名和配置惯例，能够很大程度的快速实现模型的操作，而且简洁易懂。

> ActiveRecord的主要思想是：

> 每一个数据库表对应创建一个类，类的每一个对象实例对应于数据库中表的一行记录；通常表的每个字段在类中都有相应的Field；

> ActiveRecord同时负责把自己持久化，在ActiveRecord中封装了对数据库的访问，即CURD;

> ActiveRecord是一种领域模型(Domain Model)，封装了部分业务逻辑；



### 开启AR之旅

在MP中，开启AR非常简单，只需要将实体对象继承Model即可。

```java
package cn.itcast.mp.pojo;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.extension.activerecord.Model;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class User extends Model<User> {
    private Long id;
    private String userName;
    private String password;
    private String name;
    private Integer age;
    private String email;
}
```



### 根据主键查询

```java
package cn.itcast.mp;

import cn.itcast.mp.mapper.UserMapper;
import cn.itcast.mp.pojo.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

/**
 * @author yh
 * @create 2021-11-17-9:56
 */
@RunWith(SpringRunner.class)
@SpringBootTest
public class TestUserMapper2 {
    private UserMapper userMapper;

    @Test
    public void testAR() {
        User user = new User();
        user.setId(2L);
        User user1 = user.selectById(12);
        System.out.println(user1);
    }
}
```



### 新增数据

```java
@Test
public void testInsert() {
    User user = new User();
    user.setName("刘备");
    user.setAge(30);
    user.setPassword("123456");
    user.setUserName("liubei");
    user.setEmail("liubei@itcast.cn");
    boolean insert = user.insert();
    System.out.println(insert);
}
```



### 删除数据

```java
@Test
public void testDelete() {
    User user = new User();
    user.setId(7L);
    boolean delete = user.deleteById();
    System.out.println(delete);
}
```



### 根据条件查询

```java
@Test
public void testSelect() {
    User user = new User();
    QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
    userQueryWrapper.le("age","20");
    List<User> users = user.selectList(userQueryWrapper);
    for (User user1 : users) {
        System.out.println(user1);
    }
}
```



## Oracle主键Sequence

在mysql中，主键往往是自增长的，这样使用起来是比较方便的，如果使用的是Oracle数据库，那么就不能使用自增长了，就得使用Sequence 序列生成id值了。

### 部署Oracle环境

为了简化环境部署，这里使用Docker环境进行部署安装Oracle。 

```python
#拉取镜像 
docker pull sath89/oracle-12c 
#创建容器 
docker create --name oracle -p 1521:1521 sath89/oracle-12c 
#启动 
docker start oracle && docker logs -f oracle
#下面是启动过程 
Database not initialized. Initializing database. 
Starting tnslsnr
#通过用户名密码即可登录 
用户名和密码为： system/oracle
```

![image-20211117111809394](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211117111809394.png)



### 创建表以及序列

```mysql
--创建表，表名以及字段名都要大写 
CREATE TABLE "TB_USER" ( 
    "ID" NUMBER(20) VISIBLE NOT NULL , 
    "USER_NAME" VARCHAR2(255 BYTE) VISIBLE , 
    "PASSWORD" VARCHAR2(255 BYTE) VISIBLE , 
    "NAME" VARCHAR2(255 BYTE) VISIBLE , 
    "AGE" NUMBER(10) VISIBLE , 
    "EMAIL" VARCHAR2(255 BYTE) VISIBLE 
)
--创建序列 CREATE SEQUENCE SEQ_USER START WITH 1 INCREMENT BY 1
```



### jdbc驱动包

由于版权原因，我们不能直接通过maven的中央仓库下载oracle数据库的jdbc驱动包，所以我们需要将驱动包安装到本地仓库。

```python
#ojdbc8.jar文件在资料中可以找到 
mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc8 -Dversion=12.1.0.1 - Dpackaging=jar -Dfile=ojdbc8.jar
```

安装完成后的坐标：

```xml
<dependency> 
    <groupId>com.oracle</groupId> 
    <artifactId>ojdbc8</artifactId> 
    <version>12.1.0.1</version> 
</dependency>
```



### 修改application.properties

对于application.properties的修改，需要修改2个位置，分别是：

```properties
#数据库连接配置
spring.datasource.driver-class-name=oracle.jdbc.OracleDriver
spring.datasource.url=jdbc:oracle:thin:@192.168.31.81:1521:xe 
spring.datasource.username=system 
spring.datasource.password=oracle 
#id生成策略 
mybatis-plus.global-config.db-config.id-type=input
```



### 配置序列

使用Oracle的序列需要做2件事情：

> 第一，需要配置MP的序列生成器到Spring容器：

```java
package cn.itcast.mp;

import com.baomidou.mybatisplus.extension.incrementer.OracleKeyGenerator;
import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @author yh
 * @create 2021-11-15-15:29
 */
@Configuration
@MapperScan("cn.itcast.mp.mapper")
public class MybatisPlusConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }

    /*** 序列生成器 */
    @Bean public OracleKeyGenerator oracleKeyGenerator() {
        return new OracleKeyGenerator();
    }
}
```

> 第二，在实体对象中指定序列的名称：

```java
@KeySequence(value = "SEQ_USER", clazz = Long.class)
public class User extends Model<User> {
}
```



### 测试

```java
@Test public void testInsert(){ 
    User user = new User(); 
    user.setAge(20); 
    user.setEmail("test@itcast.cn"); 
    user.setName("曹操"); 
    user.setUserName("caocao"); 
    user.setPassword("123456"); 
    int result = this.userMapper.insert(user); //返回的result是受影响的行数，并不是自增 后的id 
    System.out.println("result = " + result); 
    System.out.println(user.getId()); 
    //自增后的id会回填到对象中 
}
```



## 插件

### mybatis的插件机制

MyBatis 允许你在已映射语句执行过程中的某一点进行拦截调用。默认情况下，MyBatis 允许使用插件来拦截的方法调用包括：

1. Executor (update, query, flflushStatements, commit, rollback, getTransaction, close, isClosed)

2. ParameterHandler (getParameterObject, setParameters)

3. ResultSetHandler (handleResultSets, handleOutputParameters)

4. StatementHandler (prepare, parameterize, batch, update, query)

我们看到了可以拦截Executor接口的部分方法，比如update，query，commit，rollback等方法，还有其他接口一些方法等。总体概括为：

1. 拦截执行器的方法

2. 拦截参数的处理

3. 拦截结果集的处理

4. 拦截Sql语法构建的处理

拦截器示例：

```java
package cn.itcast.mp.plugins;

/**
 * @author yh
 * @create 2021-11-17-13:11
 */
import org.apache.ibatis.executor.Executor;
import org.apache.ibatis.mapping.MappedStatement;
import org.apache.ibatis.plugin.*;

import java.util.Properties;

@Intercepts({@Signature(
        type= Executor.class,
        method = "update",
        args = {MappedStatement.class,Object.class})})
public class MyInterceptor implements Interceptor {

    @Override
    public Object intercept(Invocation invocation) throws Throwable {
        //拦截方法，具体业务逻辑编写的位置
        return invocation.proceed();
    }

    @Override
    public Object plugin(Object target) {
        //创建target对象的代理对象,目的是将当前拦截器加入到该对象中
        return Plugin.wrap(target, this);
    }

    @Override
    public void setProperties(Properties properties) {
        //属性设置
    }
}
```

注入到Spring容器：

```java
package cn.itcast.mp;

import cn.itcast.mp.plugins.MyInterceptor;
import com.baomidou.mybatisplus.extension.incrementer.OracleKeyGenerator;
import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @author yh
 * @create 2021-11-15-15:29
 */
@Configuration
@MapperScan("cn.itcast.mp.mapper")
public class MybatisPlusConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }

    /*** 序列生成器 */
    @Bean
    public OracleKeyGenerator oracleKeyGenerator() {
        return new OracleKeyGenerator();
    }

    @Bean
    public MyInterceptor myInterceptor() {
        return new MyInterceptor();
    }
}
```

或者通过xml配置，mybatis-confifig.xml：

```xml
<configuration> 
    <plugins> 
        <plugin interceptor="cn.itcast.mp.plugins.MyInterceptor"></plugin> 
    </plugins> 
</configuration>
```



### 执行分析插件

在MP中提供了对SQL执行的分析的插件，可用作阻断全表更新、删除的操作，注意：该插件仅适用于开发环境，不适用于生产环境。

SpringBoot配置：

```java
package cn.itcast.mp;

import cn.itcast.mp.plugins.MyInterceptor;
import com.baomidou.mybatisplus.core.injector.ISqlInjector;
import com.baomidou.mybatisplus.core.parser.ISqlParser;
import com.baomidou.mybatisplus.extension.incrementer.OracleKeyGenerator;
import com.baomidou.mybatisplus.extension.parsers.BlockAttackSqlParser;
import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
import com.baomidou.mybatisplus.extension.plugins.SqlExplainInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.ArrayList;
import java.util.List;

/**
 * @author yh
 * @create 2021-11-15-15:29
 */
@Configuration
@MapperScan("cn.itcast.mp.mapper")
public class MybatisPlusConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }

    /*** 序列生成器 */
    @Bean
    public OracleKeyGenerator oracleKeyGenerator() {
        return new OracleKeyGenerator();
    }

    @Bean
    public MyInterceptor myInterceptor() {
        return new MyInterceptor();
    }

    @Bean
    public SqlExplainInterceptor sqlExplainInterceptor() {
        SqlExplainInterceptor sqlExplainInterceptor = new SqlExplainInterceptor();
        List<ISqlParser> sqlParserList = new ArrayList<>(); // 攻击 SQL 阻断解析器、加入解析链
        sqlParserList.add(new BlockAttackSqlParser());
        sqlExplainInterceptor.setSqlParserList(sqlParserList);
        return sqlExplainInterceptor;
    }
}
```

测试

```java
@Test public void testUpdate2(){ 
    User user = new User(); 
    user.setAge(20); 
    int result = this.userMapper.update(user, null); 
    System.out.println("result = " + result); 
}
```

可以看到，当执行全表更新时，会抛出异常，这样有效防止了一些误操作。



### 性能分析插件

性能分析拦截器，用于输出每条 SQL 语句及其执行时间，可以设置最大执行时间，超过时间会抛出异常。

> **该插件只用于开发环境，不建议生产环境使用。**

配置：

```xml
<configuration> 
    <plugins> 
        <!-- SQL 执行性能分析，开发环境使用，线上不推荐。 maxTime 指的是 sql 最大执行时长 -->
        <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.PerformanceInterceptor"> 					<property name="maxTime" value="100" /> <!--SQL是否格式化 默认false-->
            <property name="format" value="true" /> 
        </plugin> 
    </plugins> 
</configuration>
```

执行结果：

```
Time：11 ms - ID：cn.itcast.mp.mapper.UserMapper.selectById 
Execute SQL： 
	SELECT
		id, 
		user_name, 
		password, 
		name, 
		age, 
		email 
	FROM
		tb_user 
	WHERE
		id=7
```

可以看到，执行时间为11ms。如果将maxTime设置为1，那么，该操作会抛出异常。



### 乐观锁插件

**主要适用场景**

意图：

当要更新一条记录的时候，希望这条记录没有被别人更新

乐观锁实现方式：

- 取出记录时，获取当前version

- 更新时，带上这个version

- 执行更新时， set version = newVersion where version = oldVersion

- 如果version不对，就更新失败

**插件配置**

spring xml:

```xml
<bean class="com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor"/>
```

spring boot:

```java
@Bean public OptimisticLockerInterceptor optimisticLockerInterceptor() { 
    return new OptimisticLockerInterceptor(); 
}
```

mybatis-plus:

```xml
<plugins>
    <!-- 性能分析插件 -->
    <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.PerformanceInterceptor">
        <!--最大的执行时间，单位为毫秒-->
        <property name="maxTime" value="100"/>
        <!--对输出的SQL做格式化，默认为false-->
        <property name="format" value="true"/>
    </plugin>

    <!--乐观锁插件-->
    <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor"/>
</plugins>
```

**注解实体字段**

需要为实体字段添加@Version注解。

> 第一步，为表添加version字段，并且设置初始值为1：

```mysql
ALTER TABLE `tb_user` ADD COLUMN `version` int(10) NULL AFTER `email`; 
UPDATE `tb_user` SET `version`='1';
```

第二步，为User实体对象添加version字段，并且添加@Version注解：

```java
@Version 
private Integer version;
```

**测试**

```java
@Test 
public void testUpdate(){ 
    User user = new User(); 
    user.setAge(30); 
    user.setId(2L); 
    user.setVersion(1); //获取到version为1 
    int result = this.userMapper.updateById(user); 
    System.out.println("result = " + result); 
}
```

```java
@Test
public void testUpdate(){ 
    User user = new User();
    user.setId(2L);// 查询条件

    User userVersion = user.selectById();

    user.setAge(23); // 更新的数据
    user.setVersion(userVersion.getVersion()); // 当前的版本信息

    boolean result = user.updateById();
    System.out.println("result => " + result);
}
```

可以看到，更新的条件中有version条件，并且更新的version为2。

如果再次执行，更新则不成功。这样就避免了多人同时更新时导致数据的不一致。

**特别说明**

- **支持的数据类型只有**:int,Integer,long,Long,Date,Timestamp,LocalDateTime**

- 整数类型下 newVersion = oldVersion + 1 

- newVersion 会回写到 entity 中

- 仅支持 updateById(id) 与 update(entity, wrapper) 方法

- **在** **update(entity, wrapper)** **方法下**, wrapper **不能复用**!!!



### Sql注入器

我们已经知道，在MP中，通过AbstractSqlInjector将BaseMapper中的方法注入到了Mybatis容器，这样这些方法才可以正常执行。

那么，如果我们需要扩充BaseMapper中的方法，又该如何实现呢？下面我们以扩展fifindAll方法为例进行学习。

**编写MyBaseMapper**

```java
package cn.itcast.mp.mapper;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import java.util.List;

public interface MyBaseMapper<T> extends BaseMapper<T> {
    List<T> findAll();
    // 扩展其他的方法
}
```

其他的Mapper都可以继承该Mapper，这样实现了统一的扩展。

```java
package cn.itcast.mp.mapper;
import cn.itcast.mp.pojo.User;

public interface UserMapper extends MyBaseMapper<User> {
    User findById(Long id);
}
```

**编写MySqlInjector**

如果直接继承AbstractSqlInjector的话，原有的BaseMapper中的方法将失效，所以我们选择继承DefaultSqlInjector进行扩展。

```java
package cn.itcast.mp.injectors;

import com.baomidou.mybatisplus.core.injector.AbstractMethod;
import com.baomidou.mybatisplus.core.injector.DefaultSqlInjector;
import java.util.ArrayList;
import java.util.List;

public class MySqlInjector extends DefaultSqlInjector {

    @Override
    public List<AbstractMethod> getMethodList() {
        List<AbstractMethod> list = new ArrayList<>();
        // 获取父类中的集合
        list.addAll(super.getMethodList());

        // 再扩充自定义的方法
        list.add(new FindAll());
        return list;
    }
}
```

**编写FindAll**

```java
package cn.itcast.mp.injectors;

import com.baomidou.mybatisplus.core.injector.AbstractMethod;
import com.baomidou.mybatisplus.core.metadata.TableInfo;
import org.apache.ibatis.mapping.MappedStatement;
import org.apache.ibatis.mapping.SqlSource;

public class FindAll extends AbstractMethod {

    @Override
    public MappedStatement injectMappedStatement(Class<?> mapperClass, Class<?> modelClass, TableInfo tableInfo) {
        String sql = "select * from " + tableInfo.getTableName();
        SqlSource sqlSource = languageDriver.createSqlSource(configuration,sql, modelClass);
        return this.addSelectMappedStatement(mapperClass, "findAll", sqlSource, modelClass, tableInfo);
    }
}
```

**注册到Spring容器**

```java
package cn.itcast.mp;

import cn.itcast.mp.injectors.MySqlInjector;
import cn.itcast.mp.plugins.MyInterceptor;
import com.baomidou.mybatisplus.core.parser.ISqlParser;
import com.baomidou.mybatisplus.extension.incrementer.OracleKeyGenerator;
import com.baomidou.mybatisplus.extension.parsers.BlockAttackSqlParser;
import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
import com.baomidou.mybatisplus.extension.plugins.SqlExplainInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.ArrayList;
import java.util.List;

@Configuration
@MapperScan("cn.itcast.mp.mapper") //设置mapper接口的扫描包
public class MybatisPlusConfig {


    @Bean //配置分页插件
    public PaginationInterceptor paginationInterceptor(){
        return new PaginationInterceptor();
    }

    @Bean //Oracle的序列生成器
    public OracleKeyGenerator oracleKeyGenerator(){
        return new OracleKeyGenerator();
    }

    @Bean //注入自定义的拦截器（插件）
    public MyInterceptor myInterceptor(){
        return new MyInterceptor();
    }

    @Bean //SQL分析插件
    public SqlExplainInterceptor sqlExplainInterceptor(){

        SqlExplainInterceptor sqlExplainInterceptor = new SqlExplainInterceptor();

        List<ISqlParser> list = new ArrayList<>();
        list.add(new BlockAttackSqlParser()); //全表更新、删除的阻断器

        sqlExplainInterceptor.setSqlParserList(list);

        return sqlExplainInterceptor;
    }

    /**
     * 注入自定义的SQL注入器
     * @return
     */
    @Bean
    public MySqlInjector mySqlInjector(){
        return new MySqlInjector();
    }
}
```

**测试**

```java
@Test 
public void testFindAll(){
    List<User> users = this.userMapper.findAll(); 
    for (User user : users) { 
        System.out.println(user); 
    } 
}
```

至此，我们实现了全局扩展SQL注入器。



## 自动填充功能(其实就是设置默认值)

有些时候我们可能会有这样的需求，插入或者更新数据时，希望有些字段可以自动填充数据，比如密码、version等。在MP中提供了这样的功能，可以实现自动填充。

**添加@TableField注解**

```java
@TableField(fill = FieldFill.INSERT) //插入数据时进行填充 
private String password;
```

为password添加自动填充功能，在新增数据时有效。

FieldFill提供了多种模式选择：

```java
public enum FieldFill { 
    /**
    * 默认不处理 
    */ 
    DEFAULT, 
    
    /**
    * 插入时填充字段 
    */ INSERT, 
    
    /**
    * 更新时填充字段 
    */ UPDATE, 
    
    /**
    * 插入和更新时填充字段 
    */ 
    INSERT_UPDATE 
}
```

**编写MyMetaObjectHandler**

```java
package cn.itcast.mp.handler;

import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    /**
        * 插入数据时填充
     * @param metaObject
     */
    @Override
    public void insertFill(MetaObject metaObject) {
        // 先获取到password的值，再进行判断，如果为空，就进行填充，如果不为空，就不做处理
        Object password = getFieldValByName("password", metaObject);
        if(null == password){
           setFieldValByName("password", "888888", metaObject);
        }
    }

    /**
     * 更新数据时填充
     * @param metaObject
     */
    @Override
    public void updateFill(MetaObject metaObject) {

    }
}
```

**测试**

```java
@Test 
public void testInsert(){ 
    User user = new User(); 
    user.setName("关羽"); 
    user.setUserName("guanyu"); 
    user.setAge(30); 
    user.setEmail("guanyu@itast.cn"); 
    user.setVersion(1); 
    int result = this.userMapper.insert(user); 
    System.out.println("result = " + result); 
}
```



## 逻辑删除

开发系统时，有时候在实现功能时，删除操作需要实现逻辑删除，所谓逻辑删除就是将数据标记为删除，而并非真正的物理删除（非DELETE操作），查询时需要携带状态条件，确保被标记的数据不被查询到。这样做的目的就是避免数据被真正的删除。

MP就提供了这样的功能，方便我们使用，接下来我们一起学习下。

**修改表结构**

为tb_user表增加deleted字段，用于表示数据是否被删除，1代表删除，0代表未删除。

```mysql
ALTER TABLE `tb_user` 
ADD COLUMN `deleted` int(1) NULL DEFAULT 0 COMMENT '1代表删除，0代表未删除' AFTER `version`;
```

同时，也修改User实体，增加deleted属性并且添加@TableLogic注解：

```java
@TableLogic 
private Integer deleted;
```

**配置**

```properties
# 逻辑已删除值(默认为 1) 
mybatis-plus.global-config.db-config.logic-delete-value=1 
# 逻辑未删除值(默认为 0) 
mybatis-plus.global-config.db-config.logic-not-delete-value=0
```

**测试**

```java
@Test 
public void testDeleteById(){ 
    this.userMapper.deleteById(2L); 	
}
```

执行的SQL： 

![image-20211117160617875](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211117160617875.png)

测试查询：

```java
@Test 
public void testSelectById(){ 
    User user = this.userMapper.selectById(2L); 
    System.out.println(user); 
}
```

执行的SQL：

![image-20211117160652884](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211117160652884.png)

可见，已经实现了逻辑删除



## 通用枚举

解决了繁琐的配置，让 mybatis 优雅的使用枚举属性！

**修改表结构**

```mysql
ALTER TABLE `tb_user` 
ADD COLUMN `sex` int(1) NULL DEFAULT 1 COMMENT '1-男，2-女' AFTER `deleted`;
```

**定义枚举**

```java
package cn.itcast.mp.enums;
import com.baomidou.mybatisplus.core.enums.IEnum;

public enum SexEnum implements IEnum<Integer> {
    MAN(1,"男"),
    WOMAN(2,"女");
    private int value;
    private String desc;

    SexEnum(int value, String desc) {
        this.value = value;
        this.desc = desc;
    }

    @Override
    public Integer getValue() {
        return this.value;
    }

    @Override
    public String toString() {
        return this.desc;
    }
}
```

**配置**

```properties
# 枚举包扫描
mybatis-plus.type-enums-package=cn.itcast.mp.enums
```

**修改实体**

```java
private SexEnum sex;
```

**测试**

```java
@Test 
public void testInsert(){ 
    User user = new User(); 
    user.setName("貂蝉"); 
    user.setUserName("diaochan"); 
    user.setAge(20); 
    user.setEmail("diaochan@itast.cn"); 
    user.setVersion(1); 
    user.setSex(SexEnum.WOMAN); 
    int result = this.userMapper.insert(user); 
    System.out.println("result = " + result); 
}
```

SQL： 

![image-20211117165500500](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211117165500500.png)

查询：

```java
@Test 
public void testSelectById(){ 
    User user = this.userMapper.selectById(2L); 
    System.out.println(user); 
}
```

结果：

![image-20211117165619784](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211117165619784.png)

从测试可以看出，可以很方便的使用枚举了。

查询条件时也是有效的：

```java
@Test 
public void testSelectBySex() { 
    QueryWrapper<User> wrapper = new QueryWrapper<>(); 
    wrapper.eq("sex", SexEnum.WOMAN); 
    List<User> users = this.userMapper.selectList(wrapper); 
    for (User user : users) { 
        System.out.println(user); 
    } 
}
```

SQL：

![image-20211117165705552](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211117165705552.png)



## 代码生成器

AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、MapperXML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

**效果**

![image-20211117200740774](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211117200740774.png)

**创建工程**

pom.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.4.RELEASE</version>
    </parent>

    <groupId>cn.itcast.mp</groupId>
    <artifactId>itcast-mp-generator</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!--mybatis-plus的springboot支持-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.1.1</version>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.1.1</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-freemarker</artifactId>
        </dependency>
        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

**代码**

```java
package cn.itcast.mp.generator;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;
import com.baomidou.mybatisplus.core.toolkit.StringPool;
import com.baomidou.mybatisplus.core.toolkit.StringUtils;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.InjectionConfig;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.FileOutConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.TemplateConfig;
import com.baomidou.mybatisplus.generator.config.po.TableInfo;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;

/**
 * <p>
 * mysql 代码生成器演示例子
 * </p>
 */
public class MysqlGenerator {

    /**
     * <p>
     * 读取控制台内容
     * </p>
     */
    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入" + tip + "：");
        System.out.println(help.toString());
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotEmpty(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    /**
     * RUN THIS
     */
    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // 全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("itcast");
        gc.setOpen(false);
        mpg.setGlobalConfig(gc);

        // 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://127.0.0.1:3306/mp?useUnicode=true&useSSL=false&characterEncoding=utf8");
        // dsc.setSchemaName("public");
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("root");
        mpg.setDataSource(dsc);

        // 包配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName(scanner("模块名"));
        pc.setParent("cn.itcast.mp.generator");
        mpg.setPackageInfo(pc);

        // 自定义配置
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };
        List<FileOutConfig> focList = new ArrayList<>();
        focList.add(new FileOutConfig("/templates/mapper.xml.ftl") {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输入文件名称
                return projectPath + "/itcast-mp-generator/src/main/resources/mapper/" + pc.getModuleName()
                        + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
            }
        });
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);
        mpg.setTemplate(new TemplateConfig().setXml(null));

        // 策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
//        strategy.setSuperEntityClass("com.baomidou.mybatisplus.samples.generator.common.BaseEntity");
        strategy.setEntityLombokModel(true);
//        strategy.setSuperControllerClass("com.baomidou.mybatisplus.samples.generator.common.BaseController");
        strategy.setInclude(scanner("表名"));
        strategy.setSuperEntityColumns("id");
        strategy.setControllerMappingHyphenStyle(true);
        strategy.setTablePrefix(pc.getModuleName() + "_");
        mpg.setStrategy(strategy);
        // 选择 freemarker 引擎需要指定如下加，注意 pom 依赖必须有！
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        mpg.execute();
    }
}
```

**测试**

![image-20211117200906809](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211117200906809.png)

代码已生成：

![image-20211117200919656](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211117200919656.png)

实体对象：

```java
package cn.itcast.mp.generator.user.entity;

    import java.io.Serializable;
    import lombok.Data;
    import lombok.EqualsAndHashCode;
    import lombok.experimental.Accessors;

/**
* <p>
    * 
    * </p>
*
* @author itcast
* @since 2019-05-09
*/
    @Data
        @EqualsAndHashCode(callSuper = false)
    @Accessors(chain = true)
    public class TbUser implements Serializable {

    private static final long serialVersionUID = 1L;

            /**
            * 用户名
            */
    private String userName;

            /**
            * 密码
            */
    private String password;

            /**
            * 姓名
            */
    private String name;

            /**
            * 年龄
            */
    private Integer age;

            /**
            * 邮箱
            */
    private String email;

    private Integer version;
            /**
            * 1代表删除，0代表未删除
            */
    private Integer deleted;
            /**
            * 1-男，2-女
            */
    private Integer sex;
}
```



## MybatisX快速开发插件

MybatisX 是一款基于 IDEA 的快速开发插件，为效率而生。

安装方法：打开 IDEA，进入 File -> Settings -> Plugins -> Browse Repositories，输入 mybatisx 搜索并安装。

功能：

- Java 与 XML 调回跳转

- Mapper 方法自动生成 XML

![image-20211117201024747](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211117201024747.png)
