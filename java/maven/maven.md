# maven

## 基础篇

### maven是什么

maven的本质是一个项目管理工具，将项目开发和管理过程抽象成一个项目对象模型（POM）

**作用**

- **项目构建：提供标准的、跨平台的自动化项目构建方式**
- **依赖管理：方便快捷的管理项目依赖的资源(jar包)，避免资源间的版本冲突问题**
- **统一开发结构 ：提供标准的、统一的项目机构**



### 下载与安装

![image-20211106141439647](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106141439647.png)



### maven的基础概念

- **仓库：用于存储资源，包含各种jar包**

![image-20211106141542882](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106141542882.png)

- **坐标：用于描述资源在仓库中的位置**

![image-20211106141623220](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106141623220.png)

**本地仓库的配置**

![image-20211106141715407](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106141715407.png)

**远程仓库的配置**

![image-20211106141741092](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106141741092.png)

**镜像仓库阿里云的配置**

![image-20211106141802274](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106141802274.png)



### 第一个maven项目

**maven项目的目录结构**

![image-20211106141844843](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106141844843.png)

**pom.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd"> <modelVersion>4.0.0</modelVersion> 
    <groupId>com.itheima</groupId> 
    <artifactId>project-java</artifactId> 
    <version>1.0</version> 
    <packaging>jar</packaging> 
    <dependencies> 
        <dependency> 
            <groupId>junit</groupId> 
            <artifactId>junit</artifactId> 
            <version>4.12</version>
    	</dependency>
    </dependencies>
</project>
```

#### maven项目构建命令

![image-20211106142028118](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106142028118.png)

打包的过程：先进行编译，在进行测试，最后再打包



### IDEA生成maven项目

![image-20211106142141427](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106142141427.png)

![image-20211106142149563](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106142149563.png)

![image-20211106142214318](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106142214318.png)

![image-20211106142220751](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106142220751.png)

#### tomcat7运行插件

```xml
<build> 
    <plugins> 
        <plugin> 
            <groupId>org.apache.tomcat.maven</groupId> 
            <artifactId>tomcat7-maven-plugin</artifactId> 
            <version>2.1</version> <configuration> 
            <port>80</port> 
            <path>/</path>
            </configuration>
        </plugin>
    </plugins>
</build>
```



### 依赖管理

#### 依赖配置

![image-20211106142533141](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106142533141.png)

#### 依赖传递

![image-20211106142615232](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106142615232.png)

**依赖冲突问题**

![image-20211106142647677](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106142647677.png)

#### 可选依赖

**可选依赖指对外隐藏当前所依赖的资源--不透明**

```xml
<dependency> 
    <groupId>junit</groupId> 
    <artifactId>junit</artifactId> 
    <version>4.12</version> <
    optional>true</optional>
</dependency>	
```

#### 排除依赖

**排除依赖指主动断开依赖的资源，被排除的资源无需指定版本--不需要**

```xml
<dependency> 
    <groupId>junit</groupId> 
    <artifactId>junit</artifactId> 
    <version>4.12</version> 
    <exclusions> 
        <exclusion> 
            <groupId>org.hamcrest</groupId> 
            <artifactId>hamcrest-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

#### 依赖范围

![image-20211106143235676](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106143235676.png)

![image-20211106143325375](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106143325375.png)



### 生命周期与插件

![image-20211106143412477](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106143412477.png)

**项目构建生命周期**

- **maven对项目构建的生命周期划分为3套**
  - **clean：清理工作**
  - **default：核心工作，例如编译、测试、打包、部署等**
  - **site：产生报告、发布站点等**

![image-20211106143629899](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106143629899.png)

**插件**

- **插件与生命周期内的阶段绑定，在执行到对应的生命周期时执行对应的插件功能**
- **默认maven在各个生命周期上绑定有预设的功能**
- **通过插件可以自定义其他功能**

```xml
<build> 
    <plugins> 
        <plugin> 
            <groupId>org.apache.maven.plugins</groupId> 
            <artifactId>maven-source-plugin</artifactId> 
            <version>2.2.1</version> <executions> 
            <execution> 
                <goals> 
                    <goal>jar</goal>
                </goals> 
                <phase>generate-test-resources</phase>
            </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

![image-20211106143958919](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20211106143958919.png)

