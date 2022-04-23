# Spring

## 课程介绍

**1、Spring 框架概述**

**2、IOC 容器**

- **IOC 底层原理**

- **IOC 接口（BeanFactory）** 

- **IOC 操作 Bean 管理（基于 xml）** 

- **IOC 操作 Bean 管理（基于注解）**

**3、Aop**

**4、JdbcTemplate**

**5、事务管理**

**6、Spring5 新特性**



## Spring框架概述

**1、Spring 是轻量级的开源的 JavaEE 框架**

**2、Spring 可以解决企业应用开发的复杂性**

**3、Spring 有两个核心部分：IOC 和 Aop**

- **IOC：控制反转，把创建对象过程交给 Spring 进行管理**

- **Aop：面向切面，不修改源代码进行功能增强**

**4、Spring 特点**

- **方便解耦，简化开发**
- **Aop 编程支持**
- **方便程序测试**
- **方便和其他框架进行整合**
- **方便进行事务操作**
- **降低 API 开发难度**



## 入门案例

- **导入spring5的jar包**
- **创建一个普通的类**

```java
package com.atguigu.spring5;

/**
 * @author yh
 * @create 2021-10-25-11:57
 */
public class User {
    public void add() {
        System.out.println("add");
    }
}
```

- **创建spring的配置文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.atguigu.spring5.User"></bean>
</beans>
```

- **代码测试**

```java
package com.atguigu.spring5.testdemo;

import com.atguigu.spring5.User;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * @author yh
 * @create 2021-10-25-18:59
 */
public class TestSpring5 {
    @Test
    public void testAdd() {
        // 1.加载spring配置文件
        ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");

        // 2.获取配置创建的对象
        User user = context.getBean("user", User.class);
        System.out.println(user);
        user.add();
    }
}
```


## IOC
### IOC容器（底层原理1）

**1、什么是 IOC**

- **控制反转，把对象创建和对象之间的调用过程，交给 Spring 进行管理**

- **使用 IOC 目的：为了耦合度降低**

- **做入门案例就是 IOC 实现**

**2、IOC 底层原理**

- **xml 解析、工厂模式、反射**

**3、画图讲解 IOC 底层原理**

![图1](D:\webVideo\java代码文件\spring\笔记\分析图\图1.png)

### IOC容器（底层原理2）

![图2](D:\webVideo\java代码文件\spring\笔记\分析图\图2.png)

### IOC容器（底层原理3）

**1、IOC 思想基于 IOC 容器完成，IOC 容器底层就是对象工厂**

**2、Spring 提供 IOC 容器实现两种方式：（两个接口）**

- **BeanFactory：IOC 容器基本实现，是 Spring 内部的使用接口，不提供开发人员进行使用加载配置文件时候不会创建对象，在获取对象（使用）才去创建对象**

- **ApplicationContext：BeanFactory 接口的子接口，提供更多更强大的功能，一般由开发人员进行使用加载配置文件时候就会把在配置文件对象进行创建**

**3、ApplicationContext 接口有实现类**

![image-20211025202925635](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211025202925635.png)



### IOC容器-Bean管理XML(创建对象和set注入属性)

- **基于xml方式创建对象**

```xml
<bean id="user" class="com.atguigu.spring5.User"></bean>
```

- **在 spring 配置文件中，使用 bean 标签，标签里面添加对应属性，就可以实现对象创建**

  - **在 bean 标签有很多属性，介绍常用的属性**

  - **id 属性：唯一标识**

  - **class 属性：类全路径（包类路径）**

- **创建对象时候，默认也是执行无参数构造方法完成对象创建**



- **基于xml方式注入属性**
  - **DI：依赖注入，就是注入属性**

- **第一种注入方式：使用set方法进行注入**

  - **创建类，定义属性和对应的 set 方法**

  ```java
  package com.atguigu.spring5;
  
  /**
   * @author yh
   * @create 2021-10-25-20:36
   */
  public class Book {
      private String bname;
      private String bauthor;
  
      public void setBauthor(String bauthor) {
          this.bauthor = bauthor;
      }
  
      public void setBname(String bname) {
          this.bname = bname;
      }
  
      public void testDemo() {
          System.out.println(bname + " " + bauthor );
      }
  }
  ```

  - **在 spring 配置文件配置对象创建，配置属性注入**

  ```xml
  <bean id="book" class="com.atguigu.spring5.Book">
          <property name="bname" value="js"></property>
          <property name="bauthor" value="yh"></property>
  </bean>
  ```

- **第二种注入方式：使用有参数构造进行注入**

	- **创建类，定义属性，创建属性对应有参数构造方法**
	
	```java
	package com.atguigu.spring5;
	
	/**
	 * @author yh
	 * @create 2021-10-25-20:44
	 */
	public class Orders {
	    private String oname;
	    private String address;
	
	    public Orders(String oname, String address) {
	        this.oname = oname;
	        this.address = address;
	    }
	
	    public void testDemo() {
	        System.out.println(oname + " " + address);
	    }
	}
	```
	
	- **在 spring 配置文件中进行配置**
	
	```xml
	<bean id="orders" class="com.atguigu.spring5.Orders">
	        <constructor-arg name="oname" value="电脑"></constructor-arg>
	        <constructor-arg name="address" value="huizhou"></constructor-arg>
	</bean>
	```
	
	
### p名称空间注入（了解）

**使用 p 名称空间注入，可以简化基于 xml 配置方式**

- **添加 p 名称空间在配置文件中**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="book" class="com.atguigu.spring5.Book" p:bname="九章" p:bauthor="yh"></bean>
</beans>
```

- **第二步进行属性注入，在bean标签里面进行操作**

```xml
<bean id="book" class="com.atguigu.spring5.Book" p:bname="九章" p:bauthor="yh"></bean>
```



### IOC操作Bean管理（xml注入其他类型属性）

**字面量**

- **null 值**

```xml
<!--null 值--> 
<property name="address">
 <null/>
</property>
```

- **属性值包含特殊符号**

```xml
<!--属性值包含特殊符号
 1 把<>进行转义 &lt; &gt;
 2 把带特殊符号内容写到 CDATA
-->
<property name="address">
 <value><![CDATA[<<南京>>]]></value>
</property>
```



### 注入属性-外部bean

- **创建两个类 service 类和 dao 类** 

- **在 service 调用 dao 里面的方法**

- **在 spring 配置文件中进行配置**

```java
package com.atguigu.spring5.service;

import com.atguigu.spring5.User;
import com.atguigu.spring5.dao.UserDao;
import com.atguigu.spring5.dao.UserDaoImpl;

/**
 * @author yh
 * @create 2021-10-26-0:49
 */
public class UserService {
//    private UserDao userDao = new UserDaoImpl();
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void add() {
        System.out.println("...............");
//        userDao.update();
        userDao.update();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--1 service 和 dao 对象创建-->
    <bean id="userService" class="com.atguigu.spring5.service.UserService">
        <!--注入 userDao 对象
			name 属性：类里面属性名称
			ref 属性：创建 userDao 对象 bean 标签 id 值
 		-->
        <property name="userDao" ref="userDaoImpl"></property>
    </bean>
    <bean id="userDaoImpl" class="com.atguigu.spring5.dao.UserDaoImpl"></bean>
</beans>
```



### 注入属性-内部bean

- **一对多关系：部门和员工一个部门有多个员工，一个员工属于一个部门部门是一，员工是多**

- **在实体类之间表示一对多关系，员工表示所属部门，使用对象类型属性进行表示**

```java
//部门类
public class Dept {
     private String dname;
     public void setDname(String dname) {
     this.dname = dname;
     } 
}
//员工类
public class Emp {
     private String ename;
     private String gender;
     //员工属于某一个部门，使用对象形式表示
     private Dept dept;
     public void setDept(Dept dept) {
     this.dept = dept;
     }
     public void setEname(String ename) {
     this.ename = ename;
     }
     public void setGender(String gender) {
     this.gender = gender;
     } 
}
```

- **在 spring 配置文件中进行配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="Emp" class="com.atguigu.spring5.bean.Emp">
        <property name="name" value="lucy"></property>
        <property name="gender" value="女"></property>
        <property name="dept">
            <bean id="dept" class="com.atguigu.spring5.bean.Dept">
                <property name="name" value="安保部"></property>
            </bean>
        </property>
    </bean>
</beans>
```

- **注入属性-级联赋值**

  - **第一种写法**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
      <bean id="Emp" class="com.atguigu.spring5.bean.Emp">
          <property name="name" value="lucy"></property>
          <property name="gender" value="女"></property>
          <property name="dept" ref="dept"></property>
      </bean>
      <bean id="dept" class="com.atguigu.spring5.bean.Dept">
          <property name="name" value="安保部"></property>
      </bean>
  </beans>
  ```

  - **第二种写法**

  ```xml
  <bean id="emp" class="com.atguigu.spring5.bean.Emp">
       <!--设置两个普通属性-->
       <property name="ename" value="lucy"></property>
       <property name="gender" value="女"></property>
       <!--级联赋值-->
       <property name="dept" ref="dept"></property>
       <property name="dept.dname" value="技术部"></property>
  </bean> 
  <bean id="dept" class="com.atguigu.spring5.bean.Dept">
   	<property name="dname" value="财务部"></property>
  </bean>
  ```

  

### IOC操作Bean管理（xml注入集合属性）

- **注入数组类型属性**

- **注入List集合类型属性**

- **注入Map集合类型属性**

**（1）创建类，定义数组、list、map、set 类型属性，生成对应 set 方法**

```java
package com.atguigu.spring5.bean;

import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Set;

/**
 * @author yh
 * @create 2021-10-26-11:02
 */
public class Stu {
    private String[] courses;
    private List<String> list;
    private Map<String,String> map;
    private Set<String> set;

    public void setSet(Set<String> set) {
        this.set = set;
    }

    public void setCourses(String[] courses) {
        this.courses = courses;
    }

    public void setList(List<String> list) {
        this.list = list;
    }

    public void setMap(Map<String, String> map) {
        this.map = map;
    }

    public void test() {
        System.out.println(Arrays.toString(courses) + " " + list + " " + set + " " + map);
    }
}
```

**（2）在 spring 配置文件进行配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="stu" class="com.atguigu.spring5.bean.Stu">
        <property name="courses">
            <array>
                <value>java</value>
                <value>C++</value>
                <value>js</value>
            </array>
        </property>
        <property name="list">
            <list>
                <value>php</value>
                <value>python</value>
            </list>
        </property>
        <property name="map">
            <map>
                <entry key="JAVA" value="java"></entry>
                <entry key="PHP" value="php"></entry>
            </map>
        </property>
        <property name="set">
            <set>
                <value>mysql</value>
                <value>redis</value>
            </set>
        </property>
    </bean>
</beans>
```



### IOC操作Bean管理（xml注入集合属性2）

**在集合里面设置对象类型值**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="stu" class="com.atguigu.spring5.bean.Stu">
        <property name="courses">
            <array>
                <value>java</value>
                <value>C++</value>
                <value>js</value>
            </array>
        </property>
        <property name="list">
            <list>
                <value>php</value>
                <value>python</value>
            </list>
        </property>
        <property name="map">
            <map>
                <entry key="JAVA" value="java"></entry>
                <entry key="PHP" value="php"></entry>
            </map>
        </property>
        <property name="set">
            <set>
                <value>mysql</value>
                <value>redis</value>
            </set>
        </property>
        <property name="courseList">
            <list>
                <ref bean="course1"></ref>
                <ref bean="course2"></ref>
            </list>
        </property>
    </bean>
    <bean id="course1" class="com.atguigu.spring5.bean.Course">
        <property name="name" value="Spring5"></property>
    </bean>
    <bean id="course2" class="com.atguigu.spring5.bean.Course">
        <property name="name" value="Mybatis"></property>
    </bean>
</beans>
```

**把集合注入部分提取出来**

- **在 spring 配置文件中引入名称空间 util**

- **使用 util 标签完成 list 集合注入提取**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
        <util:list id="bookList">
            <value>就这</value>
            <value>叶</value>
            <value>jk</value>
        </util:list>
    <bean id="book" class="com.atguigu.spring5.bean.Book">
        <property name="list" ref="bookList"></property>
    </bean>
</beans>
```



### IOC操作Bean管理（FactoryBean）

- **Spring有两种类型bean，一种普通bean，另外一种工厂bean（FactoryBean）**

- **普通bean：在配置文件中定义bean 类型就是返回类型**

- **工厂bean：在配置文件定义bean类型可以和返回类型不一样**

**第一步 创建类，让这个类作为工厂 bean，实现接口 FactoryBean**

**第二步 实现接口里面的方法，在实现的方法中定义返回的 bean 类型**

```java
package com.atguigu.spring5.factoryBean;

import com.atguigu.spring5.bean.Course;
import org.springframework.beans.factory.FactoryBean;

/**
 * @author yh
 * @create 2021-10-26-14:02
 */
public class MyBean implements FactoryBean<Course> {
    /**
     * 定义返回bean
     * @return
     * @throws Exception
     */
    @Override
    public Course getObject() throws Exception {
        Course course = new Course();
        course.setName("abc");
        return course;
    }

    @Override
    public Class<?> getObjectType() {
        return null;
    }

    @Override
    public boolean isSingleton() {
        return FactoryBean.super.isSingleton();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myBean" class="com.atguigu.spring5.factoryBean.MyBean"></bean>
</beans>
```

```java
@Test
public void test3() {
    ApplicationContext context = new ClassPathXmlApplicationContext("bean3.xml");
    Course course = context.getBean("myBean", Course.class);
    System.out.println(course);
}
```



###  IOC操作Bean管理（bean作用域）

**在Spring里面，设置创建bean实例是单实例还是多实例**

**在Spring里面，默认情况下，bean是单实例对象**

![image-20211026141944980](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211026141944980.png)

**如何设置单实例还是多实例**

**在 spring 配置文件 bean 标签里面有属性（scope）用于设置单实例还是多实例**

**scope 属性值**

​	**第一个值 默认值，singleton，表示是单实例对象**

​	**第二个值 prototype，表示是多实例对象**

![image-20211026142033448](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211026142033448.png)

**singleton 和 prototype 区别**

- **singleton 单实例，prototype 多实例**

- **设置 scope 值是 singleton 时候，加载 spring 配置文件时候就会创建单实例对象** 

​	   **设置 scope 值是 prototype 时候，不是在加载 spring 配置文件时候创建 对象，在调用getBean 方法时候创建多实例对象**



### IOC操作Bean管理（bean生命周期）

**1、生命周期**

**（1）从对象创建到对象销毁的过程**

**2、bean生命周期**

- **通过构造器创建 bean 实例（无参数构造）**

- **为 bean 的属性设置值和对其他 bean 引用（调用 set 方法）**

- **调用 bean 的初始化的方法（需要进行配置初始化的方法）**

- **bean 可以使用了（对象获取到了）**

- **当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）**

```java
package com.atguigu.spring5.bean;

/**
 * @author yh
 * @create 2021-10-26-14:26
 */
public class Orders {
    private String name;

    public Orders() {
        System.out.println("无参数构造s");
    }

    public void setName(String name) {
        System.out.println("set");
        this.name = name;
    }
    public void initMethod() {
        System.out.println("init");
    }
    public void destroyMethod() {
        System.out.println("destroyed");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="orders" class="com.atguigu.spring5.bean.Orders" init-method="initMethod" destroy-method="destroyMethod">
        <property name="name" value="手机"></property>
    </bean>
    <bean id="myBeanPost" class="com.atguigu.spring5.bean.MyBeanPost"></bean>
</beans>
```

```java
@Test
public void test5() {
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean7.xml");
    Orders orders = context.getBean("orders", Orders.class);
    System.out.println("123");
    context.close();
}
```

**bean的后置处理器，bean生命周期有七步**

- **通过构造器创建bean实例（无参数构造）**

- **为bean的属性设置值和对其他bean引用（调用 set 方法）**

- **把bean实例传递bean后置处理器的方法postProcessBeforeInitialization** 

- **调用bean的初始化的方法（需要进行配置初始化的方法）**
- **bean实例传递 bean后置处理器的方法postProcessAfterInitialization**

- **bean 可以使用了（对象获取到了）**

- **当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）**

```java
package com.atguigu.spring5.bean;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

/**
 * @author yh
 * @create 2021-10-26-14:36
 */
public class MyBeanPost implements BeanPostProcessor {
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("初始化之后");
        return bean;
    }

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("初始化之前");
        return bean;
    }
}
```



### IOC操作Bean管理（xml自动装配）

**1、什么是自动装配**

- **根据指定装配规则（属性名称或者属性类型），Spring 自动将匹配的属性值进行注入**

**2、演示自动装配过程**

- **根据属性名称自动注入**

```xml
<!--实现自动装配
 bean 标签属性 autowire，配置自动装配
 autowire 属性常用两个值：
 byName 根据属性名称注入 ，注入值 bean 的 id 值和类属性名称一样
 byType 根据属性类型注入
-->
<bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byName">
 <!--<property name="dept" ref="dept"></property>-->
</bean> 
<bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
```

- **根据属性类型自动注入**

```xml
<!--实现自动装配
 bean 标签属性 autowire，配置自动装配
 autowire 属性常用两个值：
 byName 根据属性名称注入 ，注入值 bean 的 id 值和类属性名称一样
 byType 根据属性类型注入
-->
<bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byType">
 <!--<property name="dept" ref="dept"></property>-->
</bean> <bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
```



### IOC操作Bean管理(外部属性文件)

**1、直接配置数据库信息**

- **配置德鲁伊连接池**

- **引入德鲁伊连接池依赖 jar 包**

```xml
<!--直接配置连接池--> 
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
 	<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
 	<property name="url" value="jdbc:mysql://localhost:3306/userDb"></property>
 	<property name="username" value="root"></property>
	<property name="password" value="root"></property>
</bean>
```

**2、引入外部属性文件配置数据库连接池**

- **创建外部属性文件，properties 格式文件，写数据库信息**

![image-20211026153953190](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211026153953190.png)

- **把外部 properties 属性文件引入到 spring 配置文件中，引入 context 名称空间**

- **在 spring 配置文件使用标签引入外部属性文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
<!--    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">-->
<!--        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>-->
<!--        <property name="url" value="jdbc:mysql://localhost:3306/test"></property>-->
<!--        <property name="username" value="root"></property>-->
<!--        <property name="password" value="yanghong"></property>-->
<!--    </bean>-->
    <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
        <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
            <property name="driverClassName" value="${prop.driverClass}"></property>
            <property name="url" value="${prop.url}"></property>
            <property name="username" value="${prop.username}"></property>
            <property name="password" value="${prop.password}"></property>
        </bean>
</beans>
```



### IOC操作Bean管理(基于注解方式)

**1、什么是注解**

**注解是代码特殊标记，格式：@注解名称(属性名称=属性值, 属性名称=属性值..)**

**使用注解，注解作用在类上面，方法上面，属性上面**

**使用注解目的：简化 xml 配置**



**2、Spring针对Bean管理中创建对象提供注解**

**@Component**

**@Service**

**@Controller**

**@Repository**

**上面四个注解功能是一样的，都可以用来创建 bean 实例**



**3、基于注解方式实现对象创建**

- **第一步引入依赖**

![image-20211026154832665](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211026154832665.png)

- **第二步开启组件扫描**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"  >
    <!--开启组件扫描
     1 如果扫描多个包，多个包使用逗号隔开
     2 扫描包上层目录
    -->
        <context:component-scan base-package="com.atguigu"></context:component-scan>
</beans>
```

- **第三步创建类，在类上面添加创建对象注解**

```java
package com.atguigu.spring5.service;

import org.springframework.stereotype.Component;

/**
 * @author yh
 * @create 2021-10-26-15:52
 */
//在注解里面 value 属性值可以省略不写，
//默认值是类名称，首字母小写
//UserService -- userService
@Component(value="userService") //<bean id="userService" class=".."/>
public class UserService {
    public void add() {
        System.out.println("service add ......");
    }
}
```



**4、开启组件扫描细节配置**

```xml
<!--示例 1
 use-default-filters="false" 表示现在不使用默认 filter，自己配置 filter
 context:include-filter ，设置扫描哪些内容
-->
<context:component-scan base-package="com.atguigu" use-default-filters="false">
 	<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
<!--示例 2
 下面配置扫描包所有内容
 context:exclude-filter： 设置哪些内容不进行扫描
-->
<context:component-scan base-package="com.atguigu">
 	<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```



### 基于注解方式实现属性注入

**@Autowired：根据属性类型进行自动装配**

**第一步把service和dao对象创建，在service和dao类添加创建对象注解**

**第二步在service注入dao对象，在service类添加dao类型属性，在属性上面使用注解**

```java
@Service
public class UserService {
        //定义 dao 类型属性
        //不需要添加 set 方法
        //添加注入属性注解
        @Autowired 
        private UserDao userDao;
        public void add() {
        System.out.println("service add.......");
        userDao.add();
	} 
}
```

**@Qualifier：根据名称进行注入**

**这个@Qualifier 注解的使用，和上面@Autowired 一起使用**

```java
//定义 dao 类型属性
//不需要添加 set 方法
//添加注入属性注解
@Autowired //根据类型进行注入
@Qualifier(value = "userDaoImpl") //根据名称进行注入
private UserDao userDao;
```

**@Resource：可以根据类型注入，可以根据名称注入**

```java
//@Resource //根据类型进行注入
@Resource(name = "userDaoImpl1") //根据名称进行注入
private UserDao userDao;
```

**@Value：注入普通类型属性**

```java
@Value(value = "abc")
private String name;
```



### 完全注解开发

- **创建配置类，替代 xml 配置文件**

```java
package com.atguigu.spring5.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

/**
 * @author yh
 * @create 2021-10-26-19:42
 */
@Configuration //作为配置类，替代xml配置文件
@ComponentScan(basePackages = {"com.atguigu"})
public class SpringConfig {
}
```

- **编写测试类(使用AnnotationConfigApplicationContext进行创建)**

```java
package com.atguigu.spring5.testdemo;
import com.atguigu.spring5.config.SpringConfig;
import com.atguigu.spring5.service.UserService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * @author yh
 * @create 2021-10-26-0:57
 */
public class TestBean {

    @Test
    public void test6() {
        ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
        UserService userService = context.getBean("userService", UserService.class);
        userService.add();
    }
}

```



## AOP

### 什么是AOP

**面向切面编程（方面），利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。**

**通俗描述：不通过修改源代码方式，在主干功能里面添加新功能**

**使用登录例子说明 AOP**

![图3](D:\webVideo\java代码文件\spring\笔记\分析图\图3.png)



### AOP（底层原理）

**AOP底层使用动态代理**

- **有两种情况动态代理**

  - **第一种 有接口情况，使用JDK动态代理**

  - **创建接口实现类代理对象，增强类的方法**

  - **第二种 没有接口情况，使用CGLIB动态代理**

  - **创建子类的代理对象，增强类的方法**

![图4](D:\webVideo\java代码文件\spring\笔记\分析图\图4.png)



### AOP（JDK动态代理）

- **使用 JDK 动态代理，使用 Proxy 类里面的方法创建代理对象**

  - **调用 newProxyInstance 方法**

  - **方法有三个参数：**

    **第一参数，类加载器**

    **第二参数，增强方法所在的类，这个类实现的接口，支持多个接口**

    **第三参数，实现这个接口 InvocationHandler，创建代理对象，写增强的部分**

- **编写JDK动态代理代码**

  - 创建接口，定义方法

  ```java
  package com.atguigu.spring5.dao;
  
  /**
   * @author yh
   * @create 2021-10-26-20:16
   */
  public interface UserDao {
      public int add(int a,int b);
      public String update(String id);
  }
  
  ```

  - **创建接口实现类，实现方法**

  ```java
  package com.atguigu.spring5.dao;
  
  /**
   * @author yh
   * @create 2021-10-26-20:17
   */
  public class UserDaoImpl implements UserDao{
  
      @Override
      public int add(int a, int b) {
          return a+b;
      }
  
      @Override
      public String update(String id) {
          return id;
      }
  }
  ```

  - **使用 Proxy 类创建接口代理对象**

  ```java
  package com.atguigu.spring5.dao;
  
  import org.aopalliance.intercept.Invocation;
  
  import java.lang.reflect.InvocationHandler;
  import java.lang.reflect.Method;
  import java.lang.reflect.Proxy;
  
  /**
   * @author yh
   * @create 2021-10-26-20:18
   */
  public class JDKProxy {
      public static void main(String[] args) {
          //创建接口实现类的代理对象
          Class[] interfaces = {UserDao.class};
          UserDao userDao = new UserDaoImpl();
          UserDao dao = (UserDao) Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces,new UserDaoProxy(userDao));
          int add = dao.add(1, 2);
          System.out.println(add);
      }
  }
  
  class UserDaoProxy implements InvocationHandler {
      private Object obj;
  
      public UserDaoProxy(Object obj) {
          this.obj = obj;
      }
  
      @Override
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
          System.out.println("方法之前");
          Object invoke = method.invoke(obj, args);
          System.out.println("方法之后");
          return invoke;
      }
  }
  ```



### AOP（术语）

**1、连接点**

**2、切入点**

**3、通知（增强）**

**4、切面**

![图5](D:\webVideo\java代码文件\spring\笔记\分析图\图5.png)

### AOP操作（准备工作） 

**1、Spring框架一般都是基于AspectJ实现AOP操作**

- **AspectJ 不是 Spring 组成部分，独立 AOP 框架，一般把 AspectJ 和 Spirng 框架一起使用，进行 AOP 操作**

**2、基于AspectJ实现AOP操作**

- **基于 xml 配置文件实现**

- **基于注解方式实现（使用）**

**3、在项目工程里面引入AOP相关依赖**

![image-20211026205526888](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211026205526888.png)

**4、切入点表达式**

- **切入点表达式作用：知道对哪个类里面的哪个方法进行增强**

- **语法结构： execution([权限修饰符] [返回类型] [类全路径] [方法名称] [参数列表]] )**

  - **举例 1：对 com.atguigu.dao.BookDao 类里面的 add 进行增强**

  - **execution(* com.atguigu.dao.BookDao.add(..))**

  - **举例 2：对 com.atguigu.dao.BookDao 类里面的所有的方法进行增强**

  - **execution(* com.atguigu.dao.BookDao.* (..))**
  - **举例 3：对 com.atguigu.dao 包里面所有类，类里面所有方法进行增强**

  - **execution(* com.atguigu.dao.*.* (..))**



### AOP操作（AspectJ注解）

**1、创建类，在类里面定义方法**

```java
package com.atguigu.spring5.aopanno;

import org.springframework.stereotype.Component;

/**
 * @author yh
 * @create 2021-10-26-20:58
 */

//被增强的类
@Component
public class User {
    public void add() {
        System.out.println("add");
    }
}
```

**2、创建增强类（编写增强逻辑）**

- **在增强类里面，创建方法，让不同方法代表不同通知类型**

```java
public class UserProxy {
     public void before() {//前置通知
     System.out.println("before......");
     } 
}
```

**3、进行通知的配置**

- **在 spring 配置文件中，开启注解扫描**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 开启注解扫描 -->
    <context:component-scan base-package="com.atguigu.spring5.aopanno"></context:component-scan>
    <!-- 开启Aspect生成代理对象 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

- **使用注解创建 User 和 UserProxy 对象**

![image-20211026212415734](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211026212415734.png)

- **在增强类上面添加注解 @Aspect**

![image-20211026212432805](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211026212432805.png)

- **在 spring 配置文件中开启生成代理对象**

```xml
<!-- 开启 Aspect 生成代理对象--> 
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

**4、配置不同类型的通知**

- **在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置**

```java
package com.atguigu.spring5.aopanno;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

/**
 * @author yh
 * @create 2021-10-26-20:59
 */
//增强的类
@Component
@Aspect //生成代理对象
public class UserProxy {
    //前置通知
    //@Before注解表示作为前置通知
    @Before(value="execution(* com.atguigu.spring5.aopanno.User.add(..))")
    public void before() {
        System.out.println("before");
    }

    @After(value="execution(* com.atguigu.spring5.aopanno.User.add(..))")
    public void after() {
        System.out.println("after");
    }

    @AfterReturning(value="execution(* com.atguigu.spring5.aopanno.User.add(..))")
    public void afterReturning() {
        System.out.println("afterReturning");
    }

    @AfterThrowing(value="execution(* com.atguigu.spring5.aopanno.User.add(..))")
    public void afterThrowing() {
        System.out.println("afterThrowing");
    }

    @Around(value="execution(* com.atguigu.spring5.aopanno.User.add(..))")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕之前");
        proceedingJoinPoint.proceed();
        System.out.println("环绕之后");
    }
}
```

**spirng5的通知执行顺序：环绕前-前置-afterReturning-后置-环绕**

**出现异常的时候：环绕前-前置-后置-异常**



### AOP操作（AspectJ注解2）

- **相同的切入点抽取**

```java
package com.atguigu.spring5.aopanno;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

/**
 * @author yh
 * @create 2021-10-26-20:59
 */
//增强的类
@Component
@Aspect //生成代理对象
@Order(3)
public class UserProxy {
    //相同切入点的抽取
    @Pointcut(value="execution(* com.atguigu.spring5.aopanno.User.add(..))")
    public void pointDemo() {

    }

    //前置通知
    //@Before注解表示作为前置通知
    @Before(value="pointDemo()")
    public void before() {
        System.out.println("before");
    }

    @After(value="pointDemo()")
    public void after() {
        System.out.println("after");
    }

    @AfterReturning(value="pointDemo()")
    public void afterReturning() {
        System.out.println("afterReturning");
    }

    @AfterThrowing(value="pointDemo()")
    public void afterThrowing() {
        System.out.println("afterThrowing");
    }

    @Around(value="pointDemo()")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕之前");
        proceedingJoinPoint.proceed();
        System.out.println("环绕之后");
    }
}

```

- **有多个增强类多同一个方法进行增强，设置增强类优先级**
  - **在增强类上面添加注解 @Order(数字类型值)，数字类型值越小优先级越高**

```java
package com.atguigu.spring5.aopanno;

import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

/**
 * @author yh
 * @create 2021-10-26-21:30
 */
@Component
@Aspect
@Order(1)
public class PersonProxy {
    @Before(value="execution(* com.atguigu.spring5.aopanno.User.add(..))")
    public void afterReturning() {
        System.out.println("Person Before");
    }
}


package com.atguigu.spring5.aopanno;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

/**
 * @author yh
 * @create 2021-10-26-20:59
 */
//增强的类
@Component
@Aspect //生成代理对象
@Order(3)
public class UserProxy {
    //相同切入点的抽取
    @Pointcut(value="execution(* com.atguigu.spring5.aopanno.User.add(..))")
    public void pointDemo() {

    }

    //前置通知
    //@Before注解表示作为前置通知
    @Before(value="pointDemo()")
    public void before() {
        System.out.println("before");
    }

    @After(value="pointDemo()")
    public void after() {
        System.out.println("after");
    }

    @AfterReturning(value="pointDemo()")
    public void afterReturning() {
        System.out.println("afterReturning");
    }

    @AfterThrowing(value="pointDemo()")
    public void afterThrowing() {
        System.out.println("afterThrowing");
    }

    @Around(value="pointDemo()")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕之前");
        proceedingJoinPoint.proceed();
        System.out.println("环绕之后");
    }
}
```



## JdbcTemplate

### JdbcTemplate配置

**1、什么是JdbcTemplate**

- **Spring 框架对 JDBC 进行封装，使用 JdbcTemplate 方便实现对数据库操作**

**2、准备工作**

- **引入相关 jar 包**

![image-20211027125030570](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027125030570.png)

- **在 spring 配置文件配置数据库连接池**

```xml
<!-- 数据库连接池 --> 
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
     <property name="url" value="jdbc:mysql:///user_db" />
     <property name="username" value="root" />
     <property name="password" value="root" />
     <property name="driverClassName" value="com.mysql.jdbc.Driver" />
</bean>
```

- **配置 JdbcTemplate 对象，注入 DataSource**

```xml
<!-- JdbcTemplate 对象 --> 
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
     <!--注入 dataSource-->
     <property name="dataSource" ref="dataSource"></property>
</bean>
```

- **创建 service 类，创建 dao 类，在 dao 注入 jdbcTemplate 对象**
- **配置文件**

```xml
<!-- 组件扫描 -->
<context:component-scan base-package="com.atguigu"></context:component-scan>
```

- **Service**

```java
@Service
public class BookService {
     //注入 dao
     @Autowired
     private BookDao bookDao; 
}
```

- **Dao**

```java
@Repository
public class BookDaoImpl implements BookDao {
     //注入 JdbcTemplate
     @Autowired
     private JdbcTemplate jdbcTemplate;
}
```



### JdbcTemplate操作数据库（添加）

**1、对应数据库创建实体类**

![image-20211027125640508](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027125640508.png)

**2、编写service和dao**

**在 dao 进行数据库添加操作**

**调用 JdbcTemplate 对象里面 update 方法实现添加操作**

![image-20211027125715940](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027125715940.png)

**有两个参数**

**第一个参数：sql 语句**

**第二个参数：可变参数，设置 sql 语句值**

```java
@Repository
public class BookDaoImpl implements BookDao {
     //注入 JdbcTemplate
     @Autowired
     private JdbcTemplate jdbcTemplate;
     //添加的方法
     @Override
     public void add(Book book) {
     //1 创建 sql 语句
     String sql = "insert into t_book values(?,?,?)";
     //2 调用方法实现
     Object[] args = {book.getUserId(), book.getUsername(), book.getUstatus()};
     int update = jdbcTemplate.update(sql,args);
     System.out.println(update);
     } 
}
```

**3、测试类**

```java
@Test
public void testJdbcTemplate() {
     ApplicationContext context =
     new ClassPathXmlApplicationContext("bean1.xml");
     BookService bookService = context.getBean("bookService", BookService.class);
     Book book = new Book();
     book.setUserId("1");
     book.setUsername("java");
     book.setUstatus("a");
     bookService.addBook(book);
}
```



### JdbcTemplate操作数据库（修改和删除）

**1、修改**

```java
@Override
public void updateBook(Book book) {
    String sql = "update t_book set username=?,ustatus=? where user_id=?";
    Object[] args = {book.getUsername(), book.getUstatus(),book.getUserId()};
    int update = jdbcTemplate.update(sql, args);
    System.out.println(update);
}
```

**2、删除**

```java
@Override
public void delete(String id) {
    String sql = "delete from t_book where user_id=?";
    int update = jdbcTemplate.update(sql, id);
    System.out.println(update);
}
```



### JdbcTemplate操作数据库（查询返回某个值）

**1、查询表里面有多少条记录，返回是某个值**

**2、使用JdbcTemplate实现查询返回某个值代码**

![image-20211027130528432](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027130528432.png)

**有两个参数** 

**第一个参数：sql 语句**

**第二个参数：返回类型 Class**

```java
//查询表记录数
@Override
public int selectCount() {
    String sql = "select count(*) from t_book";
    Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
    return count;
}
```



### JdbcTemplate操作数据库（查询返回对象）

**1、场景：查询图书详情**

**2、JdbcTemplate实现查询返回对象**

![image-20211027130656028](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027130656028.png)

**有三个参数** 

**第一个参数：sql 语句**

**第二个参数：RowMapper 是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装**

**第三个参数：sql 语句值**

```java
//查询返回对象
@Override
public Book findBookInfo(String id) {
    String sql = "select * from t_book where user_id=?";
    //调用方法
    Book book = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Book>(Book.class), id);
    return book;
}
```



### JdbcTemplate操作数据库（查询返回集合）

**1、场景：查询图书列表分页…**

**2、调用JdbcTemplate方法实现查询返回集合**

![image-20211027130824389](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027130824389.png)

**有三个参数** 

**第一个参数：sql 语句**

**第二个参数：RowMapper 是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装**

**第三个参数：sql 语句值**

```java
//查询返回集合
@Override
public List<Book> findAllBook() {
    String sql = "select * from t_book";
    //调用方法
    List<Book> bookList = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>(Book.class));
    return bookList;
}
```



### JdbcTemplate操作数据库（批量操作）

**1、批量操作：操作表里面多条记录**

**2、JdbcTemplate实现批量添加操作**

![image-20211027130955218](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027130955218.png)

**有两个参数**

**第一个参数：sql 语句**

**第二个参数：List 集合，添加多条记录数据**

```java
//批量添加
@Override
public void batchAddBook(List<Object[]> batchArgs) {
    String sql = "insert into t_book values(?,?,?)";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}
//批量添加测试
@Test
public void test() {
    List<Object[]> batchArgs = new ArrayList<>();
    Object[] o1 = {"3","java","a"};
    Object[] o2 = {"4","c++","b"};
    Object[] o3 = {"5","MySQL","c"};
    batchArgs.add(o1);
    batchArgs.add(o2);
    batchArgs.add(o3);
    //调用批量添加
    bookService.batchAdd(batchArgs);
}
```

**3、JdbcTemplate实现批量修改操作**

```java
//批量修改
@Override
public void batchUpdateBook(List<Object[]> batchArgs) {
    String sql = "update t_book set username=?,ustatus=? where user_id=?";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}

@Test
public void test() {
    //批量修改
    List<Object[]> batchArgs = new ArrayList<>();
    Object[] o1 = {"java0909","a3","3"};
    Object[] o2 = {"c++1010","b4","4"};
    Object[] o3 = {"MySQL1111","c5","5"};
    batchArgs.add(o1);
    batchArgs.add(o2);
    batchArgs.add(o3);
    //调用方法实现批量修改
    bookService.batchUpdate(batchArgs);
}
```

**4、JdbcTemplate实现批量删除操作**

```java
//批量删除
@Override
public void batchDeleteBook(List<Object[]> batchArgs) {
    String sql = "delete from t_book where user_id=?";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}

@Test
public void test() {
    //批量删除
    List<Object[]> batchArgs = new ArrayList<>();
    Object[] o1 = {"3"};
    Object[] o2 = {"4"};
    batchArgs.add(o1);
    batchArgs.add(o2);
    //调用方法实现批量删除
    bookService.batchDelete(batchArgs);
}
```



## 事务

### 事务操作（事务概念）

**1、什么事务**

**事务是数据库操作最基本单元，逻辑上一组操作，要么都成功，如果有一个失败所有操作都失败**

**典型场景：银行转账**

**lucy 转账 100 元 给 mary**

**lucy 少 100，mary 多 100**

**2、事务四个特性（ACID）**

- **原子性**

- **一致性**

- **隔离性**

- **持久性**



### 事务操作（搭建事务操作环境）

![图6](D:\webVideo\java代码文件\spring\笔记\分析图\图6.png)

**1、创建数据库表，添加记录**

![image-20211027162452677](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027162452677.png)

**2、创建service，搭建dao，完成对象创建和注入关系**

- **service 注入 dao，在 dao 注入 JdbcTemplate，在 JdbcTemplate 注入 DataSource(使用xml进行的配置)**

```java
@Service
public class UserService {
    //注入 dao
    @Autowired
    private UserDao userDao; 
}

@Repository
public class UserDaoImpl implements UserDao {
    @Autowired
    private JdbcTemplate jdbcTemplate; 
}
```

**3、在dao创建两个方法：多钱和少钱的方法，在service创建方法（转账的方法）**

```java
@Repository
public class UserDaoImpl implements UserDao {
    
    @Autowired
    private JdbcTemplate jdbcTemplate;
    
    //lucy 转账 100 给 mary
    //少钱
    @Override
    public void reduceMoney() {
        String sql = "update t_account set money=money-? where username=?";
        jdbcTemplate.update(sql,100,"lucy");
    }
    
    //多钱
    @Override
    public void addMoney() {
        String sql = "update t_account set money=money+? where username=?";
        jdbcTemplate.update(sql,100,"mary");
    } 
}
```

```java
@Service
public class UserService {
    //注入 dao
    @Autowired
    private UserDao userDao;
    //转账的方法
    public void accountMoney() {
        //lucy 少 100
        userDao.reduceMoney();
        //mary 多 100
        userDao.addMoney();
    } 
}
```



### 事务场景

**4、上面代码，如果正常执行没有问题的，但是如果代码执行过程中出现异常，有问题**

![image-20211027162845421](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027162845421.png)

**上面问题如何解决呢？使用事务进行解决**

**事务操作过程**

![image-20211027162946584](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027162946584.png)



### 事务操作（Spring事务管理介绍）

**1、事务添加到JavaEE三层结构里面Service层（业务逻辑层）**

**2、在Spring进行事务管理操作**

- **有两种方式：编程式事务管理和声明式事务管理（使用）**

**3、声明式事务管理**

- **基于注解方式（使用）**

- **基于 xml 配置文件方式**

**4、在Spring进行声明式事务管理，底层使用AOP原理**

**5、Spring事务管理API**

- **提供一个接口，代表事务管理器，这个接口针对不同的框架提供不同的实现类**

![image-20211027163255469](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027163255469.png)



### 事务操作（注解声明式事务管理）

**1、在spring配置文件配置事务管理器**

```xml
<!--创建事务管理器--> 
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--注入数据源-->
    <property name="dataSource" ref="dataSource"></property>
</bean>
```

**2、在spring配置文件，开启事务注解**

- **在 spring 配置文件引入名称空间 tx**

```xml
<beans xmlns="http://www.springframework.org/schema/beans" 
 	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
 	   xmlns:context="http://www.springframework.org/schema/context" 
 	   xmlns:aop="http://www.springframework.org/schema/aop" 
 	   xmlns:tx="http://www.springframework.org/schema/tx" 
 	   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd 
 http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd 
 http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd 
 http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
```

- **开启事务注解**

```xml
<!--开启事务注解--> 
<tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
```

**3、在service类上面（或者service类里面方法上面）添加事务注解**

- **@Transactional，这个注解添加到类上面，也可以添加方法上面**

- **如果把这个注解添加类上面，这个类里面所有的方法都添加事务**

- **如果把这个注解添加方法上面，为这个方法添加事务**

```java
@Service
@Transactional
public class UserService {
    //注入 dao
    @Autowired
    private UserDao userDao;
    //转账的方法
    public void accountMoney() {
        //lucy 少 100
        userDao.reduceMoney();
        //mary 多 100
        userDao.addMoney();
    } 
}
```



### 事务操作（声明式事务管理参数配置）

**1、在service类上面添加注解@Transactional，在这个注解里面可以配置事务相关参数**

![image-20211027164005523](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211027164005523.png)

2、propagation：事务传播行为

- 多事务方法直接进行调用，这个过程中事务是如何进行管理的

![图7](D:\webVideo\java代码文件\spring\笔记\分析图\图7.png)

![事务传播行为](D:\webVideo\java代码文件\spring\笔记\分析图\事务\事务传播行为.bmp)

```java
@Service
@Transactional(propagation = Propagation.REQUIRED)
```

**3、ioslation：事务隔离级别**

- **事务有特性成为隔离性，多事务操作之间不会产生影响。不考虑隔离性产生很多问题**
- **有三个读问题：脏读、不可重复读、虚（幻）读**

- **脏读：一个未提交事务读取到另一个未提交事务的数据**

- **虚读：一个未提交事务读取到另一提交事务添加数据**

- **解决：通过设置事务隔离级别，解决读问题**

![事务隔离级别](D:\webVideo\java代码文件\spring\笔记\分析图\事务\事务隔离级别.bmp)

```xml
@Service
@Transactional(propagation = Propagation.REQUIRED,isolation = Isolation.REPEATABLE_READ)
```

**4、timeout：超时时间**

- **事务需要在一定时间内进行提交，如果不提交进行回滚**

- **默认值是 -1 ，设置时间以秒单位进行计算**

**5、readOnly：是否只读**

- **读：查询操作，写：添加修改删除操作**

- **readOnly 默认值 false，表示可以查询，可以添加修改删除操作**

- **设置 readOnly 值是 true，设置成 true 之后，只能查询**

**6、rollbackFor：回滚**

- **设置出现哪些异常进行事务回滚**

**7、noRollbackFor：不回滚**

- **设置出现哪些异常不进行事务回滚**



### 事务操作（XML声明式事务管理）

1、在spring配置文件中进行配置

第一步 配置事务管理器

第二步 配置通知

第三步 配置切入点和切面

```xml
<!--1 创建事务管理器--> 
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--注入数据源-->
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--2 配置通知--> 
<tx:advice id="txadvice">
    <!--配置事务参数-->
    <tx:attributes>
        <!--指定哪种规则的方法上面添加事务-->
        <tx:method name="accountMoney" propagation="REQUIRED"/>
        <!--<tx:method name="account*"/>-->
    </tx:attributes>
</tx:advice>

<!--3 配置切入点和切面--> 
<aop:config>
    <!--配置切入点-->
    <aop:pointcut id="pt" expression="execution(* com.atguigu.spring5.service.UserService.*(..))"/>
    <!--配置切面-->
    <aop:advisor advice-ref="txadvice" pointcut-ref="pt"/>
</aop:config>
```



### 事务操作（完全注解声明式事务管理）

**1、创建配置类，使用配置类替代xml配置文件**

```java
@Configuration //配置类
@ComponentScan(basePackages = "com.atguigu") //组件扫描
@EnableTransactionManagement //开启事务
public class TxConfig {
    
    //创建数据库连接池
    @Bean
    public DruidDataSource getDruidDataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql:///user_db");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        return dataSource;
    }
    
    //创建 JdbcTemplate 对象
    @Bean
    public JdbcTemplate getJdbcTemplate(DataSource dataSource) {
        //到 ioc 容器中根据类型找到 dataSource
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        //注入 dataSource
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }
    
    //创建事务管理器
    @Bean
    public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource) {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    } 
}
```

