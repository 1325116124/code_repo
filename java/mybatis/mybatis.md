# Mybatis



## 为什么使用Mybatis

**MyBatis是一个半自动化的持久化层框架。**

**• JDBC**

**– SQL夹在Java代码块里，耦合度高导致硬编码内伤**

**– 维护不易且实际开发需求中sql是有变化，频繁修改的情况多见** 

**• Hibernate和JPA**

**– 长难复杂SQL，对于Hibernate而言处理也不容易**

**– 内部自动生产的SQL，不容易做特殊优化。**

**– 基于全映射的全自动框架，大量字段的POJO进行部分映射时比较困难。**

**导致数据库性能下降。** 

**• 对开发人员而言，核心sql还是需要自己优化**

• **sql和java编码分开，功能边界清晰，一个专注业务、**

**一个专注数据。**

