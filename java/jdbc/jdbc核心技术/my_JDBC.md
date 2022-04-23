# 2.4 数据库连接方式举例

## 2.4.1 连接方式一

```java
	@Test
    public void testConnection1() {
        try {
            //1.提供java.sql.Driver接口实现类的对象
            Driver driver = null;
            driver = new com.mysql.jdbc.Driver();

            //2.提供url，指明具体操作的数据
            String url = "jdbc:mysql://localhost:3306/test";

            //3.提供Properties的对象，指明用户名和密码
            Properties info = new Properties();
            info.setProperty("user", "root");
            info.setProperty("password", "abc123");

            //4.调用driver的connect()，获取连接
            Connection conn = driver.connect(url, info);
            System.out.println(conn);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
```

> 说明：上述代码中显式出现了第三方数据库的API

## 2.4.2 连接方式二

```java
	@Test
    public void testConnection2() {
        try {
            //1.实例化Driver
            String className = "com.mysql.jdbc.Driver";
            Class clazz = Class.forName(className);
            Driver driver = (Driver) clazz.newInstance();

            //2.提供url，指明具体操作的数据
            String url = "jdbc:mysql://localhost:3306/test";

            //3.提供Properties的对象，指明用户名和密码
            Properties info = new Properties();
            info.setProperty("user", "root");
            info.setProperty("password", "abc123");

            //4.调用driver的connect()，获取连接
            Connection conn = driver.connect(url, info);
            System.out.println(conn);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

> 说明：相较于方式一，这里使用反射实例化Driver，不在代码中体现第三方数据库的API。体现了面向接口编程思想。

## 2.4.3 连接方式三

```java
	@Test
    public void testConnection3() {
        try {
            //1.数据库连接的4个基本要素：
            String url = "jdbc:mysql://localhost:3306/test";
            String user = "root";
            String password = "abc123";
            String driverName = "com.mysql.jdbc.Driver";

            //2.实例化Driver
            Class clazz = Class.forName(driverName);
            Driver driver = (Driver) clazz.newInstance();
            //3.注册驱动
            DriverManager.registerDriver(driver);
            //4.获取连接
            Connection conn = DriverManager.getConnection(url, user, password);
            System.out.println(conn);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
```

> 说明：使用DriverManager实现数据库的连接。体会获取连接必要的4个基本要素。

## 2.4.4 连接方式四

```java
	@Test
    public void testConnection4() {
        try {
            //1.数据库连接的4个基本要素：
            String url = "jdbc:mysql://localhost:3306/test";
            String user = "root";
            String password = "abc123";
            String driverName = "com.mysql.jdbc.Driver";

            //2.加载驱动 （①实例化Driver ②注册驱动）
            Class.forName(driverName);


            //Driver driver = (Driver) clazz.newInstance();
            //3.注册驱动
            //DriverManager.registerDriver(driver);
            /*
            可以注释掉上述代码的原因，是因为在mysql的Driver类中声明有：
            static {
                try {
                    DriverManager.registerDriver(new Driver());
                } catch (SQLException var1) {
                    throw new RuntimeException("Can't register driver!");
                }
            }

             */


            //3.获取连接
            Connection conn = DriverManager.getConnection(url, user, password);
            System.out.println(conn);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
```

> 说明：不必显式的注册驱动了。因为在DriverManager的源码中已经存在静态代码块，实现了驱动的注册。

## 2.4.5 连接方式五(最终版)

```java
	@Test
    public  void testConnection5() throws Exception {
    	//1.加载配置文件
        InputStream is = ConnectionTest.class.getClassLoader().getResourceAsStream("jdbc.properties");
        Properties pros = new Properties();
        pros.load(is);
        
        //2.读取配置信息
        String user = pros.getProperty("user");
        String password = pros.getProperty("password");
        String url = pros.getProperty("url");
        String driverClass = pros.getProperty("driverClass");

        //3.加载驱动
        Class.forName(driverClass);

        //4.获取连接
        Connection conn = DriverManager.getConnection(url,user,password);
        System.out.println(conn);

    }
```

其中，配置文件声明在工程的src目录下：【jdbc.properties】

```properties
user=root
password=abc123
url=jdbc:mysql://localhost:3306/test
driverClass=com.mysql.jdbc.Driver
```

> 说明：使用配置文件的方式保存配置信息，在代码中加载配置文件
>
> **使用配置文件的好处：**
>
> ①实现了代码和数据的分离，如果需要修改配置信息，直接在配置文件中修改，不需要深入代码
> ②如果修改了配置信息，省去重新编译的过程。

## PrepareStatement实战

```java
package com.atguigu3.preparedstatement.crud;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.Date;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.sql.Statement;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Properties;

import org.junit.Test;

import com.atguigu1.connection.ConnectionTest;
import com.atguigu3.util.JDBCUtils;

/*
 * 使用PreparedStatement来替换Statement,实现对数据表的增删改操作
 * 
 * 增删改；查
 * 
 * 
 */
public class PreparedStatementUpdateTest {
	
	@Test
	public void testCommonUpdate(){
//		String sql = "delete from customers where id = ?";
//		update(sql,3);
		
		String sql = "update `order` set order_name = ? where order_id = ?";
		update(sql,"DD","2");
		
	}
	
	//通用的增删改操作
	public void update(String sql,Object ...args){//sql中占位符的个数与可变形参的长度相同！
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			//1.获取数据库的连接
			conn = JDBCUtils.getConnection();
			//2.预编译sql语句，返回PreparedStatement的实例
			ps = conn.prepareStatement(sql);
			//3.填充占位符
			for(int i = 0;i < args.length;i++){
				ps.setObject(i + 1, args[i]);//小心参数声明错误！！
			}
			//4.执行
			ps.execute();
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			//5.资源的关闭
			JDBCUtils.closeResource(conn, ps);
			
		}
		
		
	}
	
	//修改customers表的一条记录
	@Test
	public void testUpdate(){
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			//1.获取数据库的连接
			conn = JDBCUtils.getConnection();
			//2.预编译sql语句，返回PreparedStatement的实例
			String sql = "update customers set name = ? where id = ?";
			ps = conn.prepareStatement(sql);
			//3.填充占位符
			ps.setObject(1,"莫扎特");
			ps.setObject(2, 18);
			//4.执行
			ps.execute();
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			//5.资源的关闭
			JDBCUtils.closeResource(conn, ps);
			
		}
	}
	

	// 向customers表中添加一条记录
	@Test
	public void testInsert() {
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			// 1.读取配置文件中的4个基本信息
			InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");

			Properties pros = new Properties();
			pros.load(is);

			String user = pros.getProperty("user");
			String password = pros.getProperty("password");
			String url = pros.getProperty("url");
			String driverClass = pros.getProperty("driverClass");

			// 2.加载驱动
			Class.forName(driverClass);

			// 3.获取连接
			conn = DriverManager.getConnection(url, user, password);
			
//		System.out.println(conn);
			
			//4.预编译sql语句，返回PreparedStatement的实例
			String sql = "insert into customers(name,email,birth)values(?,?,?)";//?:占位符
			ps = conn.prepareStatement(sql);
			//5.填充占位符
			ps.setString(1, "哪吒");
			ps.setString(2, "nezha@gmail.com");
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
			java.util.Date date = sdf.parse("1000-01-01");
			ps.setDate(3, new Date(date.getTime()));
			
			//6.执行操作
			ps.execute();
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			//7.资源的关闭
			try {
				if(ps != null)
					ps.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			try {
				if(conn != null)
					conn.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}		
		}	
	}
}
```

**利用到的自己封装的数据库的工具类**

```java
package com.atguigu3.util;

import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

/**
 * 
 * @Description 操作数据库的工具类
 * @author shkstart Email:shkstart@126.com
 * @version
 * @date 上午9:10:02
 *
 */
public class JDBCUtils {
	
	/**
	 * 
	 * @Description 获取数据库的连接
	 * @author shkstart
	 * @date 上午9:11:23
	 * @return
	 * @throws Exception
	 */
	public static Connection getConnection() throws Exception {
		// 1.读取配置文件中的4个基本信息
		InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");

		Properties pros = new Properties();
		pros.load(is);

		String user = pros.getProperty("user");
		String password = pros.getProperty("password");
		String url = pros.getProperty("url");
		String driverClass = pros.getProperty("driverClass");

		// 2.加载驱动
		Class.forName(driverClass);

		// 3.获取连接
		Connection conn = DriverManager.getConnection(url, user, password);
		return conn;
	}
	/**
	 * 
	 * @Description 关闭连接和Statement的操作
	 * @author shkstart
	 * @date 上午9:12:40
	 * @param conn
	 * @param ps
	 */
	public static void closeResource(Connection conn,Statement ps){
		try {
			if(ps != null)
				ps.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		try {
			if(conn != null)
				conn.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	/**
	 * 
	 * @Description 关闭资源操作
	 * @author shkstart
	 * @date 上午10:21:15
	 * @param conn
	 * @param ps
	 * @param rs
	 */
	public static void closeResource(Connection conn,Statement ps,ResultSet rs){
		try {
			if(ps != null)
				ps.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		try {
			if(conn != null)
				conn.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		try {
			if(rs != null)
				rs.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
}
```

## 针对customers表的查询操作

**首先利用ORM思想，将数据库中的表和java类进行对应**

```java
package com.atguigu3.bean;

import java.sql.Date;

/*
 * ORM编程思想  （object relational mapping）
 * 一个数据表对应一个java类
 * 表中的一条记录对应java类的一个对象
 * 表中的一个字段对应java类的一个属性
 * 
 */
public class Customer {
	
	private int id;
	private String name;
	private String email;
	private Date birth;
	public Customer() {
		super();
	}
	public Customer(int id, String name, String email, Date birth) {
		super();
		this.id = id;
		this.name = name;
		this.email = email;
		this.birth = birth;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public Date getBirth() {
		return birth;
	}
	public void setBirth(Date birth) {
		this.birth = birth;
	}
	@Override
	public String toString() {
		return "Customer [id=" + id + ", name=" + name + ", email=" + email + ", birth=" + birth + "]";
	}
}
```

**针对customers的查询操作**

```java
package com.atguigu3.preparedstatement.crud;

import java.lang.reflect.Field;
import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;

import org.junit.Test;

import com.atguigu3.bean.Customer;
import com.atguigu3.util.JDBCUtils;

/**
 * 
 * @Description 针对于Customers表的查询操作
 * @author shkstart  Email:shkstart@126.com
 * @version 
 * @date 上午10:04:55
 *
 */
public class CustomerForQuery {
	
	@Test
	public void testQueryForCustomers(){
		String sql = "select id,name,birth,email from customers where id = ?";
		Customer customer = queryForCustomers(sql, 13);
		System.out.println(customer);
		
		sql = "select name,email from customers where name = ?";
		Customer customer1 = queryForCustomers(sql,"周杰伦");
		System.out.println(customer1);
	}
	
	/**
	 * 
	 * @Description 针对于customers表的通用的查询操作
	 * @author shkstart
	 * @throws Exception 
	 * @date 上午10:23:40
	 */
	public Customer queryForCustomers(String sql,Object...args){
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			conn = JDBCUtils.getConnection();
			
			ps = conn.prepareStatement(sql);
			for(int i = 0;i < args.length;i++){
				ps.setObject(i + 1, args[i]);
			}
			
			rs = ps.executeQuery();
			//获取结果集的元数据 :ResultSetMetaData
			ResultSetMetaData rsmd = rs.getMetaData();
			//通过ResultSetMetaData获取结果集中的列数
			int columnCount = rsmd.getColumnCount();
			
			if(rs.next()){
				Customer cust = new Customer();
				//处理结果集一行数据中的每一个列
				for(int i = 0;i <columnCount;i++){
					//获取列值
					Object columValue = rs.getObject(i + 1);
					
					//获取每个列的列名
//					String columnName = rsmd.getColumnName(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);
					
					//给cust对象指定的columnName属性，赋值为columValue：通过反射
					Field field = Customer.class.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(cust, columValue);
				}
				return cust;
			}
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(conn, ps, rs);
			
		}
		
		return null;
		
		
	}
	
	
	@Test
	public void testQuery1() {
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet resultSet = null;
		try {
			conn = JDBCUtils.getConnection();
			String sql = "select id,name,email,birth from customers where id = ?";
			ps = conn.prepareStatement(sql);
			ps.setObject(1, 1);
			
			//执行,并返回结果集
			resultSet = ps.executeQuery();
			//处理结果集
			if(resultSet.next()){//next():判断结果集的下一条是否有数据，如果有数据返回true,并指针下移；如果返回false,指针不会下移。
				
				//获取当前这条数据的各个字段值
				int id = resultSet.getInt(1);
				String name = resultSet.getString(2);
				String email = resultSet.getString(3);
				Date birth = resultSet.getDate(4);
				
			//方式一：
//			System.out.println("id = " + id + ",name = " + name + ",email = " + email + ",birth = " + birth);
				
			//方式二：
//			Object[] data = new Object[]{id,name,email,birth};
				//方式三：将数据封装为一个对象（推荐）
				Customer customer = new Customer(id, name, email, birth);
				System.out.println(customer);
				
			}
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			//关闭资源
			JDBCUtils.closeResource(conn, ps, resultSet);		
		}		
	}
}
```

## 针对order表的查询操作（引出getColumnLabel）

**order.class**

```java
package com.atguigu3.bean;

import java.sql.Date;

public class Order {
	private int orderId;
	private String orderName;
	private Date orderDate;
	
	
	public Order() {
		super();
	}
	public Order(int orderId, String orderName, Date orderDate) {
		super();
		this.orderId = orderId;
		this.orderName = orderName;
		this.orderDate = orderDate;
	}
	public int getOrderId() {
		return orderId;
	}
	public void setOrderId(int orderId) {
		this.orderId = orderId;
	}
	public String getOrderName() {
		return orderName;
	}
	public void setOrderName(String orderName) {
		this.orderName = orderName;
	}
	public Date getOrderDate() {
		return orderDate;
	}
	public void setOrderDate(Date orderDate) {
		this.orderDate = orderDate;
	}
	@Override
	public String toString() {
		return "Order [orderId=" + orderId + ", orderName=" + orderName + ", orderDate=" + orderDate + "]";
	}	
}
```

**操作代码**

```java
package com.atguigu3.preparedstatement.crud;

import java.lang.reflect.Field;
import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;

import org.junit.Test;

import com.atguigu3.bean.Order;
import com.atguigu3.util.JDBCUtils;

/**
 * 
 * @Description 针对于Order表的通用的查询操作
 * @author shkstart  Email:shkstart@126.com
 * @version 
 * @date 上午10:43:58
 *
 */
public class OrderForQuery {
	/*
	 * 针对于表的字段名与类的属性名不相同的情况：
	 * 1. 必须声明sql时，使用类的属性名来命名字段的别名
	 * 2. 使用ResultSetMetaData时，需要使用getColumnLabel()来替换getColumnName(),
	 *    获取列的别名。
	 *  说明：如果sql中没有给字段其别名，getColumnLabel()获取的就是列名
	 * 
	 * 
	 */
	@Test
	public void testOrderForQuery(){
		String sql = "select order_id orderId,order_name orderName,order_date orderDate from `order` where order_id = ?";
		Order order = orderForQuery(sql,1);
		System.out.println(order);
	}
	
	/**
	 * 
	 * @Description 通用的针对于order表的查询操作
	 * @author shkstart
	 * @date 上午10:51:12
	 * @return
	 * @throws Exception 
	 */
	public Order orderForQuery(String sql,Object...args){
		
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			conn = JDBCUtils.getConnection();
			ps = conn.prepareStatement(sql);
			for(int i = 0;i < args.length;i++){
				ps.setObject(i + 1, args[i]);
			}
			
			//执行，获取结果集
			rs = ps.executeQuery();
			//获取结果集的元数据
			ResultSetMetaData rsmd = rs.getMetaData();
			//获取列数
			int columnCount = rsmd.getColumnCount();
			if(rs.next()){
				Order order = new Order();
				for(int i = 0;i < columnCount;i++){
					//获取每个列的列值:通过ResultSet
					Object columnValue = rs.getObject(i + 1);
					//通过ResultSetMetaData
					//获取列的列名：getColumnName() --不推荐使用
					//获取列的别名：getColumnLabel()
//					String columnName = rsmd.getColumnName(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);
					
					//通过反射，将对象指定名columnName的属性赋值为指定的值columnValue
					Field field = Order.class.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(order, columnValue);
				}
				
				return order;
			}
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			
			JDBCUtils.closeResource(conn, ps, rs);
		}
		
		
		return null;
		
	}
	
	
	@Test
	public void testQuery1(){
		
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			conn = JDBCUtils.getConnection();
			String sql = "select order_id,order_name,order_date from `order` where order_id = ?";
			ps = conn.prepareStatement(sql);
			ps.setObject(1, 1);
			
			rs = ps.executeQuery();
			if(rs.next()){
				int id = (int) rs.getObject(1);
				String name = (String) rs.getObject(2);
				Date date = (Date) rs.getObject(3);
				
				Order order = new Order(id, name, date);
				System.out.println(order);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			
			JDBCUtils.closeResource(conn, ps, rs);
		}	
	}
}
```

![image-20210903134001033](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210903134001033.png)

## 对查询多个结果的通用操作

```java
package com.atguigu3.preparedstatement.crud;

import java.lang.reflect.Field;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.util.ArrayList;
import java.util.List;

import org.junit.Test;

import com.atguigu3.bean.Customer;
import com.atguigu3.bean.Order;
import com.atguigu3.util.JDBCUtils;

/**
 * 
 * @Description 使用PreparedStatement实现针对于不同表的通用的查询操作
 * @author shkstart Email:shkstart@126.com
 * @version
 * @date 上午11:32:55
 *
 */
public class PreparedStatementQueryTest {
	
	@Test
	public void testGetForList(){
		
		String sql = "select id,name,email from customers where id < ?";
		List<Customer> list = getForList(Customer.class,sql,12);
		list.forEach(System.out::println);
		
		String sql1 = "select order_id orderId,order_name orderName from `order`";
		List<Order> orderList = getForList(Order.class, sql1);
		orderList.forEach(System.out::println);
	}
	
	public <T> List<T> getForList(Class<T> clazz,String sql, Object... args){
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			conn = JDBCUtils.getConnection();

			ps = conn.prepareStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}

			rs = ps.executeQuery();
			// 获取结果集的元数据 :ResultSetMetaData
			ResultSetMetaData rsmd = rs.getMetaData();
			// 通过ResultSetMetaData获取结果集中的列数
			int columnCount = rsmd.getColumnCount();
			//创建集合对象
			ArrayList<T> list = new ArrayList<T>();
			while (rs.next()) {
				T t = clazz.newInstance();
				// 处理结果集一行数据中的每一个列:给t对象指定的属性赋值
				for (int i = 0; i < columnCount; i++) {
					// 获取列值
					Object columValue = rs.getObject(i + 1);

					// 获取每个列的列名
					// String columnName = rsmd.getColumnName(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);

					// 给t对象指定的columnName属性，赋值为columValue：通过反射
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				list.add(t);
			}
			
			return list;
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(conn, ps, rs);

		}

		return null;
	}
	
	@Test
	public void testGetInstance(){
		String sql = "select id,name,email from customers where id = ?";
		Customer customer = getInstance(Customer.class,sql,12);
		System.out.println(customer);
		
		String sql1 = "select order_id orderId,order_name orderName from `order` where order_id = ?";
		Order order = getInstance(Order.class, sql1, 1);
		System.out.println(order);
	}
	/**
	 * 
	 * @Description 针对于不同的表的通用的查询操作，返回表中的一条记录
	 * @author shkstart
	 * @date 上午11:42:23
	 * @param clazz
	 * @param sql
	 * @param args
	 * @return
	 */
	public <T> T getInstance(Class<T> clazz,String sql, Object... args) {
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			conn = JDBCUtils.getConnection();

			ps = conn.prepareStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}

			rs = ps.executeQuery();
			// 获取结果集的元数据 :ResultSetMetaData
			ResultSetMetaData rsmd = rs.getMetaData();
			// 通过ResultSetMetaData获取结果集中的列数
			int columnCount = rsmd.getColumnCount();

			if (rs.next()) {
				T t = clazz.newInstance();
				// 处理结果集一行数据中的每一个列
				for (int i = 0; i < columnCount; i++) {
					// 获取列值
					Object columValue = rs.getObject(i + 1);

					// 获取每个列的列名
					// String columnName = rsmd.getColumnName(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);

					// 给t对象指定的columnName属性，赋值为columValue：通过反射
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				return t;
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(conn, ps, rs);
		}
		return null;
	}
}
```

## 练习一：往customers中插入一条数据

```java
package com.atguigu4.exer;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.util.Scanner;

import org.junit.Test;

import com.atguigu3.util.JDBCUtils;
//课后练习1
public class Exer1Test {
	
	@Test
	public void testInsert(){
		Scanner scanner = new Scanner(System.in);
		System.out.print("请输入用户名：");
		String name = scanner.next();
		System.out.print("请输入邮箱：");
		String email = scanner.next();
		System.out.print("请输入生日：");
		String birthday = scanner.next();//'1992-09-08'
		
		String sql = "insert into customers(name,email,birth)values(?,?,?)";
		int insertCount = update(sql,name,email,birthday);
		if(insertCount > 0){
			System.out.println("添加成功");
			
		}else{
			System.out.println("添加失败");
		}
		
	}
	
	
	// 通用的增删改操作
	public int update(String sql, Object... args) {// sql中占位符的个数与可变形参的长度相同！
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			// 1.获取数据库的连接
			conn = JDBCUtils.getConnection();
			// 2.预编译sql语句，返回PreparedStatement的实例
			ps = conn.prepareStatement(sql);
			// 3.填充占位符
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);// 小心参数声明错误！！
			}
			// 4.执行
			/*
			 * ps.execute():
			 * 如果执行的是查询操作，有返回结果，则此方法返回true;
			 * 如果执行的是增、删、改操作，没有返回结果，则此方法返回false.
			 */
			//方式一：
//			return ps.execute();
			//方式二：
			return ps.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			// 5.资源的关闭
			JDBCUtils.closeResource(conn, ps);
		}
		return 0;
	}
}
```

## 练习二

```java
package com.atguigu4.exer;

import java.lang.reflect.Field;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.util.Scanner;

import javax.swing.plaf.synth.SynthStyle;

import org.junit.Test;

import com.atguigu3.util.JDBCUtils;

//课后练习2
public class Exer2Test {

	// 问题1：向examstudent表中添加一条记录
	/*
	 *  Type: 
		IDCard:
		ExamCard:
		StudentName:
		Location:
		Grade:
	 */
	@Test
	public void testInsert(){
		Scanner scanner = new Scanner(System.in);
		System.out.print("四级/六级：");
		int type = scanner.nextInt();
		System.out.print("身份证号：");
		String IDCard = scanner.next();
		System.out.print("准考证号：");
		String examCard = scanner.next();
		System.out.print("学生姓名：");
		String studentName = scanner.next();
		System.out.print("所在城市：");
		String location = scanner.next();
		System.out.print("考试成绩：");
		int grade = scanner.nextInt();
		
		String sql = "insert into examstudent(type,IDCard,examCard,studentName,location,grade)values(?,?,?,?,?,?)";
		int insertCount = update(sql,type,IDCard,examCard,studentName,location,grade);
		if(insertCount > 0){
			System.out.println("添加成功");
		}else{
			System.out.println("添加失败");
		}
		
		
		
	}
	// 通用的增删改操作
	public int update(String sql, Object... args) {// sql中占位符的个数与可变形参的长度相同！
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			// 1.获取数据库的连接
			conn = JDBCUtils.getConnection();
			// 2.预编译sql语句，返回PreparedStatement的实例
			ps = conn.prepareStatement(sql);
			// 3.填充占位符
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);// 小心参数声明错误！！
			}
			// 4.执行
			/*
			 * ps.execute(): 
			 * 如果执行的是查询操作，有返回结果，则此方法返回true;
			 * 如果执行的是增、删、改操作，没有返回结果，则此方法返回false.
			 */
			// 方式一：
			// return ps.execute();
			// 方式二：
			return ps.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			// 5.资源的关闭
			JDBCUtils.closeResource(conn, ps);

		}
		return 0;
	}
	
	//问题2：根据身份证号或者准考证号查询学生成绩信息
	@Test
	public void queryWithIDCardOrExamCard(){
		System.out.println("请选择您要输入的类型：");
		System.out.println("a.准考证号");
		System.out.println("b.身份证号");
		Scanner scanner = new Scanner(System.in);
		String selection = scanner.next();
		if("a".equalsIgnoreCase(selection)){//if(selection.equalsIgnoreCase("a")){
			System.out.println("请输入准考证号：");
			String examCard = scanner.next();
			String sql = "select FlowID flowID,Type type,IDCard,ExamCard examCard,StudentName name,Location location,Grade grade from examstudent where examCard = ?";
			
			Student student = getInstance(Student.class,sql,examCard);
			if(student != null){
				System.out.println(student);
				
			}else{
				System.out.println("输入的准考证号有误！");
			}
			
		}else if("b".equalsIgnoreCase(selection)){
			System.out.println("请输入身份证号：");
			String IDCard = scanner.next();
			String sql = "select FlowID flowID,Type type,IDCard,ExamCard examCard,StudentName name,Location location,Grade grade from examstudent where IDCard = ?";
			
			Student student = getInstance(Student.class,sql,IDCard);
			if(student != null){
				System.out.println(student);
				
			}else{
				System.out.println("输入的身份证号有误！");
			}
		}else{
			System.out.println("您的输入有误，请重新进入程序。");
		}
		
	}
	
	public <T> T getInstance(Class<T> clazz,String sql, Object... args) {
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			conn = JDBCUtils.getConnection();

			ps = conn.prepareStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}

			rs = ps.executeQuery();
			// 获取结果集的元数据 :ResultSetMetaData
			ResultSetMetaData rsmd = rs.getMetaData();
			// 通过ResultSetMetaData获取结果集中的列数
			int columnCount = rsmd.getColumnCount();

			if (rs.next()) {
				T t = clazz.newInstance();
				// 处理结果集一行数据中的每一个列
				for (int i = 0; i < columnCount; i++) {
					// 获取列值
					Object columValue = rs.getObject(i + 1);

					// 获取每个列的列名
					// String columnName = rsmd.getColumnName(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);

					// 给t对象指定的columnName属性，赋值为columValue：通过反射
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				return t;
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(conn, ps, rs);

		}

		return null;
	}
	
	//问题3：删除指定的学生信息
	@Test
	public void testDeleteByExamCard(){
		System.out.println("请输入学生的考号：");
		Scanner scanner = new Scanner(System.in);
		String examCard = scanner.next();
		//查询指定准考证号的学生
		String sql = "select FlowID flowID,Type type,IDCard,ExamCard examCard,StudentName name,Location location,Grade grade from examstudent where examCard = ?";
		Student student = getInstance(Student.class,sql,examCard);
		if(student == null){
			System.out.println("查无此人，请重新输入");
		}else{
			String sql1 = "delete from examstudent where examCard = ?";
			int deleteCount = update(sql1, examCard);
			if(deleteCount > 0){
				System.out.println("删除成功");
			}
		}
	}
	//优化以后的操作：
	@Test
	public void testDeleteByExamCard1(){
		System.out.println("请输入学生的考号：");
		Scanner scanner = new Scanner(System.in);
		String examCard = scanner.next();
		String sql = "delete from examstudent where examCard = ?";
		int deleteCount = update(sql, examCard);
		if(deleteCount > 0){
			System.out.println("删除成功");
		}else{
			System.out.println("查无此人，请重新输入");
		}	
	}
}
```

## 操作blob数据

```java
package com.atguigu5.blob;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Blob;
import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import org.junit.Test;

import com.atguigu3.bean.Customer;
import com.atguigu3.util.JDBCUtils;

/**
 * 
 * @Description 测试使用PreparedStatement操作Blob类型的数据
 * @author shkstart  Email:shkstart@126.com
 * @version 
 * @date 下午4:08:58
 *
 */
public class BlobTest {
	
	//向数据表customers中插入Blob类型的字段
	@Test
	public void testInsert() throws Exception{
		Connection conn = JDBCUtils.getConnection();
		String sql = "insert into customers(name,email,birth,photo)values(?,?,?,?)";
		
		PreparedStatement ps = conn.prepareStatement(sql);
		
		ps.setObject(1,"袁浩");
		ps.setObject(2, "yuan@qq.com");
		ps.setObject(3,"1992-09-08");
		FileInputStream is = new FileInputStream(new File("girl.jpg"));
		ps.setBlob(4, is);
		
		ps.execute();
		
		JDBCUtils.closeResource(conn, ps);
		
	}
	
	//查询数据表customers中Blob类型的字段
	@Test
	public void testQuery(){
		Connection conn = null;
		PreparedStatement ps = null;
		InputStream is = null;
		FileOutputStream fos = null;
		ResultSet rs = null;
		try {
			conn = JDBCUtils.getConnection();
			String sql = "select id,name,email,birth,photo from customers where id = ?";
			ps = conn.prepareStatement(sql);
			ps.setInt(1, 21);
			rs = ps.executeQuery();
			if(rs.next()){
	//			方式一：
	//			int id = rs.getInt(1);
	//			String name = rs.getString(2);
	//			String email = rs.getString(3);
	//			Date birth = rs.getDate(4);
				//方式二：
				int id = rs.getInt("id");
				String name = rs.getString("name");
				String email = rs.getString("email");
				Date birth = rs.getDate("birth");
				
				Customer cust = new Customer(id, name, email, birth);
				System.out.println(cust);
				
				//将Blob类型的字段下载下来，以文件的方式保存在本地
				Blob photo = rs.getBlob("photo");
				is = photo.getBinaryStream();
				fos = new FileOutputStream("zhangyuhao.jpg");
				byte[] buffer = new byte[1024];
				int len;
				while((len = is.read(buffer)) != -1){
					fos.write(buffer, 0, len);
				}
				
			}
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			
			try {
				if(is != null)
					is.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
			
			try {
				if(fos != null)
					fos.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
			
			JDBCUtils.closeResource(conn, ps, rs);
		}	
	}	
}
```

## PreparedStatement实现批量数据的操作

```java
package com.atguigu5.blob;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import org.junit.Test;

import com.atguigu3.util.JDBCUtils;

/*
 * 使用PreparedStatement实现批量数据的操作
 * 
 * update、delete本身就具有批量操作的效果。
 * 此时的批量操作，主要指的是批量插入。使用PreparedStatement如何实现更高效的批量插入？
 * 
 * 题目：向goods表中插入20000条数据
 * CREATE TABLE goods(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(25)
   );
 * 方式一：使用Statement
 * Connection conn = JDBCUtils.getConnection();
 * Statement st = conn.createStatement();
 * for(int i = 1;i <= 20000;i++){
 * 		String sql = "insert into goods(name)values('name_" + i + "')";
 * 		st.execute(sql);
 * }
 * 
 */
public class InsertTest {
	//批量插入的方式二：使用PreparedStatement
	@Test
	public void testInsert1() {
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			
			long start = System.currentTimeMillis();
			
			conn = JDBCUtils.getConnection();
			String sql = "insert into goods(name)values(?)";
			ps = conn.prepareStatement(sql);
			for(int i = 1;i <= 20000;i++){
				ps.setObject(1, "name_" + i);
				
				ps.execute();
			}
			
			long end = System.currentTimeMillis();
			
			System.out.println("花费的时间为：" + (end - start));//20000:83065
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(conn, ps);
			
		}
		
	}
	
	/*
	 * 批量插入的方式三：
	 * 1.addBatch()、executeBatch()、clearBatch()
	 * 2.mysql服务器默认是关闭批处理的，我们需要通过一个参数，让mysql开启批处理的支持。
	 * 		 ?rewriteBatchedStatements=true 写在配置文件的url后面
	 * 3.使用更新的mysql 驱动：mysql-connector-java-5.1.37-bin.jar
	 */
	@Test
	public void testInsert2() {
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			
			long start = System.currentTimeMillis();
			
			conn = JDBCUtils.getConnection();
			String sql = "insert into goods(name)values(?)";
			ps = conn.prepareStatement(sql);
			for(int i = 1;i <= 1000000;i++){
				ps.setObject(1, "name_" + i);
				
				//1."攒"sql
				ps.addBatch();
				
				if(i % 500 == 0){
					//2.执行batch
					ps.executeBatch();
					
					//3.清空batch
					ps.clearBatch();
				}
				
			}
			
			long end = System.currentTimeMillis();
			
			System.out.println("花费的时间为：" + (end - start));//20000:83065 -- 565
		} catch (Exception e) {								//1000000:16086
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(conn, ps);
			
		}
		
	}
	
	//批量插入的方式四：设置连接不允许自动提交数据
	@Test
	public void testInsert3() {
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			
			long start = System.currentTimeMillis();
			
			conn = JDBCUtils.getConnection();
			
			//设置不允许自动提交数据
			conn.setAutoCommit(false);
			
			String sql = "insert into goods(name)values(?)";
			ps = conn.prepareStatement(sql);
			for(int i = 1;i <= 1000000;i++){
				ps.setObject(1, "name_" + i);
				
				//1."攒"sql
				ps.addBatch();
				
				if(i % 500 == 0){
					//2.执行batch
					ps.executeBatch();
					
					//3.清空batch
					ps.clearBatch();
				}
				
			}
			
			//提交数据
			conn.commit();
			
			long end = System.currentTimeMillis();
			
			System.out.println("花费的时间为：" + (end - start));//20000:83065 -- 565
		} catch (Exception e) {								//1000000:16086 -- 5114
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(conn, ps);			
		}		
	}
}
```

## jdbc考虑事务

```java
package com.atguigu1.transaction;

import java.lang.reflect.Field;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;

import org.junit.Test;

import com.atguigu1.util.JDBCUtils;
/*
 * 1.什么叫数据库事务？
 * 事务：一组逻辑操作单元,使数据从一种状态变换到另一种状态。
 * 		> 一组逻辑操作单元：一个或多个DML操作。
 * 
 * 2.事务处理的原则：保证所有事务都作为一个工作单元来执行，即使出现了故障，都不能改变这种执行方式。
 * 当在一个事务中执行多个操作时，要么所有的事务都被提交(commit)，那么这些修改就永久地保存
 * 下来；要么数据库管理系统将放弃所作的所有修改，整个事务回滚(rollback)到最初状态。
 * 
 * 3.数据一旦提交，就不可回滚
 * 
 * 4.哪些操作会导致数据的自动提交？
 * 		>DDL操作一旦执行，都会自动提交。
 * 			>set autocommit = false 对DDL操作失效
 * 		>DML默认情况下，一旦执行，就会自动提交。
 * 			>我们可以通过set autocommit = false的方式取消DML操作的自动提交。
 * 		>默认在关闭连接时，会自动的提交数据
 */
public class TransactionTest {
	
	//******************未考虑数据库事务情况下的转账操作**************************
	/*
	 * 针对于数据表user_table来说：
	 * AA用户给BB用户转账100
	 * 
	 * update user_table set balance = balance - 100 where user = 'AA';
	 * update user_table set balance = balance + 100 where user = 'BB';
	 */
	@Test
	public void testUpdate(){
		
		String sql1 = "update user_table set balance = balance - 100 where user = ?";
		update(sql1, "AA");
		
		//模拟网络异常
		System.out.println(10 / 0);
		
		String sql2 = "update user_table set balance = balance + 100 where user = ?";
		update(sql2, "BB");
		
		System.out.println("转账成功");
	}

	// 通用的增删改操作---version 1.0
	public int update(String sql, Object... args) {// sql中占位符的个数与可变形参的长度相同！
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			// 1.获取数据库的连接
			conn = JDBCUtils.getConnection();
			// 2.预编译sql语句，返回PreparedStatement的实例
			ps = conn.prepareStatement(sql);
			// 3.填充占位符
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);// 小心参数声明错误！！
			}
			// 4.执行
			return ps.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			
			//修改其为自动提交数据
			//主要针对于使用数据库连接池的使用
			try {
				conn.setAutoCommit(true);
			} catch (SQLException e) {
				e.printStackTrace();
			}
			
			// 5.资源的关闭
			JDBCUtils.closeResource(conn, ps);

		}
		return 0;

	}
	
	//********************考虑数据库事务后的转账操作*********************
	
	@Test
	public void testUpdateWithTx() {
		Connection conn = null;
		try {
			conn = JDBCUtils.getConnection();
			System.out.println(conn.getAutoCommit());//true
			//1.取消数据的自动提交
			conn.setAutoCommit(false);
			
			String sql1 = "update user_table set balance = balance - 100 where user = ?";
			update(conn,sql1, "AA");
			
			//模拟网络异常
			System.out.println(10 / 0);
			
			String sql2 = "update user_table set balance = balance + 100 where user = ?";
			update(conn,sql2, "BB");
			
			System.out.println("转账成功");
			
			//2.提交数据
			conn.commit();
			
		} catch (Exception e) {
			e.printStackTrace();
			//3.回滚数据
			try {
				conn.rollback();
			} catch (SQLException e1) {
				e1.printStackTrace();
			}
		}finally{
			
			JDBCUtils.closeResource(conn, null);
		}
		
	}
	
	// 通用的增删改操作---version 2.0 （考虑上事务）
	public int update(Connection conn,String sql, Object... args) {// sql中占位符的个数与可变形参的长度相同！
		PreparedStatement ps = null;
		try {
			// 1.预编译sql语句，返回PreparedStatement的实例
			ps = conn.prepareStatement(sql);
			// 2.填充占位符
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);// 小心参数声明错误！！
			}
			// 3.执行
			return ps.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			// 4.资源的关闭
			JDBCUtils.closeResource(null, ps);

		}
		return 0;

	}
	
	//*****************************************************
	@Test
	public void testTransactionSelect() throws Exception{
		
		Connection conn = JDBCUtils.getConnection();
		//获取当前连接的隔离级别
		System.out.println(conn.getTransactionIsolation());
		//设置数据库的隔离级别：
		conn.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);
		//取消自动提交数据
		conn.setAutoCommit(false);
		
		String sql = "select user,password,balance from user_table where user = ?";
		User user = getInstance(conn, User.class, sql, "CC");
		
		System.out.println(user);
		
	}
	
	@Test
	public void testTransactionUpdate() throws Exception{
		Connection conn = JDBCUtils.getConnection();
		
		//取消自动提交数据
		conn.setAutoCommit(false);
		String sql = "update user_table set balance = ? where user = ?";
		update(conn, sql, 5000,"CC");
		
		Thread.sleep(15000);
		System.out.println("修改结束");
	}
	
	//通用的查询操作，用于返回数据表中的一条记录（version 2.0：考虑上事务）
	public <T> T getInstance(Connection conn,Class<T> clazz,String sql, Object... args) {
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			
			ps = conn.prepareStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}

			rs = ps.executeQuery();
			// 获取结果集的元数据 :ResultSetMetaData
			ResultSetMetaData rsmd = rs.getMetaData();
			// 通过ResultSetMetaData获取结果集中的列数
			int columnCount = rsmd.getColumnCount();

			if (rs.next()) {
				T t = clazz.newInstance();
				// 处理结果集一行数据中的每一个列
				for (int i = 0; i < columnCount; i++) {
					// 获取列值
					Object columValue = rs.getObject(i + 1);

					// 获取每个列的列名
					// String columnName = rsmd.getColumnName(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);

					// 给t对象指定的columnName属性，赋值为columValue：通过反射
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				return t;
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps, rs);
		}
		return null;
	}
}
```

## 封装操作数据库的DAO操作

- **首先创建一个BaseDAO抽象类来实现一些基本的增删改查操作**
- **再创建一个表的DAO接口，比如CustomerDAO**
- **创建一个表的DAO实现类，比如customerDAOImpl继承BaseDAO并且实现CustomerDAO**

**BaseDAO**

```java
package com.atguigu2.dao;

import java.lang.reflect.Field;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import com.atguigu1.util.JDBCUtils;

/*
 * DAO: data(base) access object
 * 封装了针对于数据表的通用的操作
 */
public abstract class BaseDAO {
	// 通用的增删改操作---version 2.0 （考虑上事务）
	public int update(Connection conn, String sql, Object... args) {// sql中占位符的个数与可变形参的长度相同！
		PreparedStatement ps = null;
		try {
			// 1.预编译sql语句，返回PreparedStatement的实例
			ps = conn.prepareStatement(sql);
			// 2.填充占位符
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);// 小心参数声明错误！！
			}
			// 3.执行
			return ps.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			// 4.资源的关闭
			JDBCUtils.closeResource(null, ps);

		}
		return 0;

	}

	// 通用的查询操作，用于返回数据表中的一条记录（version 2.0：考虑上事务）
	public <T> T getInstance(Connection conn, Class<T> clazz, String sql, Object... args) {
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {

			ps = conn.prepareStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}

			rs = ps.executeQuery();
			// 获取结果集的元数据 :ResultSetMetaData
			ResultSetMetaData rsmd = rs.getMetaData();
			// 通过ResultSetMetaData获取结果集中的列数
			int columnCount = rsmd.getColumnCount();

			if (rs.next()) {
				T t = clazz.newInstance();
				// 处理结果集一行数据中的每一个列
				for (int i = 0; i < columnCount; i++) {
					// 获取列值
					Object columValue = rs.getObject(i + 1);

					// 获取每个列的列名
					// String columnName = rsmd.getColumnName(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);

					// 给t对象指定的columnName属性，赋值为columValue：通过反射
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				return t;
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps, rs);

		}

		return null;
	}
	// 通用的查询操作，用于返回数据表中的多条记录构成的集合（version 2.0：考虑上事务）
	public <T> List<T> getForList(Connection conn, Class<T> clazz, String sql, Object... args) {
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {

			ps = conn.prepareStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}

			rs = ps.executeQuery();
			// 获取结果集的元数据 :ResultSetMetaData
			ResultSetMetaData rsmd = rs.getMetaData();
			// 通过ResultSetMetaData获取结果集中的列数
			int columnCount = rsmd.getColumnCount();
			// 创建集合对象
			ArrayList<T> list = new ArrayList<T>();
			while (rs.next()) {
				T t = clazz.newInstance();
				// 处理结果集一行数据中的每一个列:给t对象指定的属性赋值
				for (int i = 0; i < columnCount; i++) {
					// 获取列值
					Object columValue = rs.getObject(i + 1);

					// 获取每个列的列名
					// String columnName = rsmd.getColumnName(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);

					// 给t对象指定的columnName属性，赋值为columValue：通过反射
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				list.add(t);
			}

			return list;
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps, rs);

		}

		return null;
	}
	//用于查询特殊值的通用的方法
	public <E> E getValue(Connection conn,String sql,Object...args){
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			ps = conn.prepareStatement(sql);
			for(int i = 0;i < args.length;i++){
				ps.setObject(i + 1, args[i]);
				
			}
			
			rs = ps.executeQuery();
			if(rs.next()){
				return (E) rs.getObject(1);
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(null, ps, rs);		
		}
		return null;	
	}	
}
```

**CustomerDAO**

```java
package com.atguigu2.dao;

import java.sql.Connection;
import java.sql.Date;
import java.util.List;

import com.atguigu2.bean.Customer;

/*
 * 此接口用于规范针对于customers表的常用操作
 */
public interface CustomerDAO {
	/**
	 * 
	 * @Description 将cust对象添加到数据库中
	 * @author shkstart
	 * @date 上午11:00:27
	 * @param conn
	 * @param cust
	 */
	void insert(Connection conn,Customer cust);
	/**
	 * 
	 * @Description 针对指定的id，删除表中的一条记录
	 * @author shkstart
	 * @date 上午11:01:07
	 * @param conn
	 * @param id
	 */
	void deleteById(Connection conn,int id);
	/**
	 * 
	 * @Description 针对内存中的cust对象，去修改数据表中指定的记录
	 * @author shkstart
	 * @date 上午11:02:14
	 * @param conn
	 * @param cust
	 */
	void update(Connection conn,Customer cust);
	/**
	 * 
	 * @Description 针对指定的id查询得到对应的Customer对象
	 * @author shkstart
	 * @date 上午11:02:59
	 * @param conn
	 * @param id
	 */
	Customer getCustomerById(Connection conn,int id);
	/**
	 * 
	 * @Description 查询表中的所有记录构成的集合
	 * @author shkstart
	 * @date 上午11:03:50
	 * @param conn
	 * @return
	 */
	List<Customer> getAll(Connection conn);
	/**
	 * 
	 * @Description 返回数据表中的数据的条目数
	 * @author shkstart
	 * @date 上午11:04:44
	 * @param conn
	 * @return
	 */
	Long getCount(Connection conn);
	
	/**
	 * 
	 * @Description 返回数据表中最大的生日
	 * @author shkstart
	 * @date 上午11:05:33
	 * @param conn
	 * @return
	 */
	Date getMaxBirth(Connection conn);
}	
```

**CustomerDAOImpl**

```java
package com.atguigu2.dao;

import java.sql.Connection;
import java.sql.Date;
import java.util.List;

import com.atguigu2.bean.Customer;

public class CustomerDAOImpl extends BaseDAO implements CustomerDAO{

	@Override
	public void insert(Connection conn, Customer cust) {
		String sql = "insert into customers(name,email,birth)values(?,?,?)";
		update(conn, sql,cust.getName(),cust.getEmail(),cust.getBirth());
	}

	@Override
	public void deleteById(Connection conn, int id) {
		String sql = "delete from customers where id = ?";
		update(conn, sql, id);
	}

	@Override
	public void update(Connection conn, Customer cust) {
		String sql = "update customers set name = ?,email = ?,birth = ? where id = ?";
		update(conn, sql,cust.getName(),cust.getEmail(),cust.getBirth(),cust.getId());
	}

	@Override
	public Customer getCustomerById(Connection conn, int id) {
		String sql = "select id,name,email,birth from customers where id = ?";
		Customer customer = getInstance(conn,Customer.class, sql,id);
		return customer;
	}

	@Override
	public List<Customer> getAll(Connection conn) {
		String sql = "select id,name,email,birth from customers";
		List<Customer> list = getForList(conn, Customer.class, sql);
		return list;
	}

	@Override
	public Long getCount(Connection conn) {
		String sql = "select count(*) from customers";
		return getValue(conn, sql);
	}

	@Override
	public Date getMaxBirth(Connection conn) {
		String sql = "select max(birth) from customers";
		return getValue(conn, sql);
	}
}
```

## 优化DAO(去掉显示赋值class)

- **将baseDAO定义为一个泛型类**
- **通过子类的创建通过反射直接创建class**

```java
package com.atguigu3.dao;

import java.lang.reflect.Field;
import java.lang.reflect.ParameterizedType;
import java.lang.reflect.Type;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import com.atguigu1.util.JDBCUtils;

/*
 * DAO: data(base) access object
 * 封装了针对于数据表的通用的操作
 */
public abstract class BaseDAO<T> {
	
	private Class<T> clazz = null;
	
//	public BaseDAO(){
//		
//	}
	
	{	
		//获取当前BaseDAO的子类继承的父类中的泛型
		Type genericSuperclass = this.getClass().getGenericSuperclass();
		ParameterizedType paramType = (ParameterizedType) genericSuperclass;
		
		Type[] typeArguments = paramType.getActualTypeArguments();//获取了父类的泛型参数
		clazz = (Class<T>) typeArguments[0];//泛型的第一个参数
		
	}
	
	
	// 通用的增删改操作---version 2.0 （考虑上事务）
	public int update(Connection conn, String sql, Object... args) {// sql中占位符的个数与可变形参的长度相同！
		PreparedStatement ps = null;
		try {
			// 1.预编译sql语句，返回PreparedStatement的实例
			ps = conn.prepareStatement(sql);
			// 2.填充占位符
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);// 小心参数声明错误！！
			}
			// 3.执行
			return ps.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			// 4.资源的关闭
			JDBCUtils.closeResource(null, ps);

		}
		return 0;

	}

	// 通用的查询操作，用于返回数据表中的一条记录（version 2.0：考虑上事务）
	public T getInstance(Connection conn, String sql, Object... args) {
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {

			ps = conn.prepareStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}

			rs = ps.executeQuery();
			// 获取结果集的元数据 :ResultSetMetaData
			ResultSetMetaData rsmd = rs.getMetaData();
			// 通过ResultSetMetaData获取结果集中的列数
			int columnCount = rsmd.getColumnCount();

			if (rs.next()) {
				T t = clazz.newInstance();
				// 处理结果集一行数据中的每一个列
				for (int i = 0; i < columnCount; i++) {
					// 获取列值
					Object columValue = rs.getObject(i + 1);

					// 获取每个列的列名
					// String columnName = rsmd.getColumnName(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);

					// 给t对象指定的columnName属性，赋值为columValue：通过反射
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				return t;
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps, rs);

		}

		return null;
	}
	// 通用的查询操作，用于返回数据表中的多条记录构成的集合（version 2.0：考虑上事务）
	public List<T> getForList(Connection conn, String sql, Object... args) {
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {

			ps = conn.prepareStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}

			rs = ps.executeQuery();
			// 获取结果集的元数据 :ResultSetMetaData
			ResultSetMetaData rsmd = rs.getMetaData();
			// 通过ResultSetMetaData获取结果集中的列数
			int columnCount = rsmd.getColumnCount();
			// 创建集合对象
			ArrayList<T> list = new ArrayList<T>();
			while (rs.next()) {
				T t = clazz.newInstance();
				// 处理结果集一行数据中的每一个列:给t对象指定的属性赋值
				for (int i = 0; i < columnCount; i++) {
					// 获取列值
					Object columValue = rs.getObject(i + 1);

					// 获取每个列的列名
					// String columnName = rsmd.getColumnName(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);

					// 给t对象指定的columnName属性，赋值为columValue：通过反射
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				list.add(t);
			}

			return list;
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps, rs);

		}

		return null;
	}
	//用于查询特殊值的通用的方法
	public <E> E getValue(Connection conn,String sql,Object...args){
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			ps = conn.prepareStatement(sql);
			for(int i = 0;i < args.length;i++){
				ps.setObject(i + 1, args[i]);
				
			}
			
			rs = ps.executeQuery();
			if(rs.next()){
				return (E) rs.getObject(1);
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(null, ps, rs);
		}
		return null;	
	}	
}
```

```java
package com.atguigu3.dao;

import java.sql.Connection;
import java.sql.Date;
import java.util.List;

import com.atguigu2.bean.Customer;

/*
 * 此接口用于规范针对于customers表的常用操作
 */
public interface CustomerDAO {
	/**
	 * 
	 * @Description 将cust对象添加到数据库中
	 * @author shkstart
	 * @date 上午11:00:27
	 * @param conn
	 * @param cust
	 */
	void insert(Connection conn,Customer cust);
	/**
	 * 
	 * @Description 针对指定的id，删除表中的一条记录
	 * @author shkstart
	 * @date 上午11:01:07
	 * @param conn
	 * @param id
	 */
	void deleteById(Connection conn,int id);
	/**
	 * 
	 * @Description 针对内存中的cust对象，去修改数据表中指定的记录
	 * @author shkstart
	 * @date 上午11:02:14
	 * @param conn
	 * @param cust
	 */
	void update(Connection conn,Customer cust);
	/**
	 * 
	 * @Description 针对指定的id查询得到对应的Customer对象
	 * @author shkstart
	 * @date 上午11:02:59
	 * @param conn
	 * @param id
	 */
	Customer getCustomerById(Connection conn,int id);
	/**
	 * 
	 * @Description 查询表中的所有记录构成的集合
	 * @author shkstart
	 * @date 上午11:03:50
	 * @param conn
	 * @return
	 */
	List<Customer> getAll(Connection conn);
	/**
	 * 
	 * @Description 返回数据表中的数据的条目数
	 * @author shkstart
	 * @date 上午11:04:44
	 * @param conn
	 * @return
	 */
	Long getCount(Connection conn);
	
	/**
	 * 
	 * @Description 返回数据表中最大的生日
	 * @author shkstart
	 * @date 上午11:05:33
	 * @param conn
	 * @return
	 */
	Date getMaxBirth(Connection conn);
}	
```

```java
package com.atguigu3.dao;

import java.lang.reflect.ParameterizedType;
import java.lang.reflect.Type;
import java.sql.Connection;
import java.sql.Date;
import java.util.List;

import com.atguigu2.bean.Customer;

public class CustomerDAOImpl extends BaseDAO<Customer> implements CustomerDAO{
	
	
	@Override
	public void insert(Connection conn, Customer cust) {
		String sql = "insert into customers(name,email,birth)values(?,?,?)";
		update(conn, sql,cust.getName(),cust.getEmail(),cust.getBirth());
	}

	@Override
	public void deleteById(Connection conn, int id) {
		String sql = "delete from customers where id = ?";
		update(conn, sql, id);
	}

	@Override
	public void update(Connection conn, Customer cust) {
		String sql = "update customers set name = ?,email = ?,birth = ? where id = ?";
		update(conn, sql,cust.getName(),cust.getEmail(),cust.getBirth(),cust.getId());
	}

	@Override
	public Customer getCustomerById(Connection conn, int id) {
		String sql = "select id,name,email,birth from customers where id = ?";
		Customer customer = getInstance(conn, sql,id);
		return customer;
	}

	@Override
	public List<Customer> getAll(Connection conn) {
		String sql = "select id,name,email,birth from customers";
		List<Customer> list = getForList(conn, sql);
		return list;
	}

	@Override
	public Long getCount(Connection conn) {
		String sql = "select count(*) from customers";
		return getValue(conn, sql);
	}

	@Override
	public Date getMaxBirth(Connection conn) {
		String sql = "select max(birth) from customers";
		return getValue(conn, sql);
	}
}
```

## 使用数据库连接池(主要记住Druid)

**简化版**

```java
 @Test
    public void getConnection() throws Exception {
        Properties pros = new Properties();
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("druid.properties");
        pros.load(is);
        DataSource dataSource = DruidDataSourceFactory.createDataSource(pros);
        Connection conn = dataSource.getConnection();
        System.out.println(conn);
    }
```

**封装版**

```java
package com.atguigu4.util;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

import javax.sql.DataSource;

import org.apache.commons.dbcp.BasicDataSourceFactory;
import org.apache.commons.dbutils.DbUtils;

import com.alibaba.druid.pool.DruidDataSourceFactory;
import com.mchange.v2.c3p0.ComboPooledDataSource;

public class JDBCUtils {
	/**
	 * 
	 * @Description 获取数据库的连接
	 * @author shkstart
	 * @date 上午9:11:23
	 * @return
	 * @throws Exception
	 */
	public static Connection getConnection() throws Exception {
		// 1.读取配置文件中的4个基本信息
		InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");

		Properties pros = new Properties();
		pros.load(is);

		String user = pros.getProperty("user");
		String password = pros.getProperty("password");
		String url = pros.getProperty("url");
		String driverClass = pros.getProperty("driverClass");

		// 2.加载驱动
		Class.forName(driverClass);

		// 3.获取连接
		Connection conn = DriverManager.getConnection(url, user, password);
		return conn;
	}
	
	/**
	 * 
	 * @Description 使用C3P0的数据库连接池技术
	 * @author shkstart
	 * @date 下午3:01:25
	 * @return
	 * @throws SQLException
	 */
	//数据库连接池只需提供一个即可。
	private static ComboPooledDataSource cpds = new ComboPooledDataSource("hellc3p0");
	public static Connection getConnection1() throws SQLException{
		Connection conn = cpds.getConnection();
		
		return conn;
	}
	
	/**
	 * 
	 * @Description 使用DBCP数据库连接池技术获取数据库连接
	 * @author shkstart
	 * @date 下午3:35:25
	 * @return
	 * @throws Exception
	 */
	//创建一个DBCP数据库连接池
	private static DataSource source;
	static{
		try {
			Properties pros = new Properties();
			FileInputStream is = new FileInputStream(new File("src/dbcp.properties"));
			pros.load(is);
			source = BasicDataSourceFactory.createDataSource(pros);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	public static Connection getConnection2() throws Exception{
		
		Connection conn = source.getConnection();
		
		return conn;
	}
	
	/**
	 * 使用Druid数据库连接池技术
	 */
	private static DataSource source1;
	static{
		try {
			Properties pros = new Properties();
			
			InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("druid.properties");
			
			pros.load(is);
			
			source1 = DruidDataSourceFactory.createDataSource(pros);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	public static Connection getConnection3() throws SQLException{
		
		Connection conn = source1.getConnection();
		return conn;
	}
	
	
	/**
	 * 
	 * @Description 关闭连接和Statement的操作
	 * @author shkstart
	 * @date 上午9:12:40
	 * @param conn
	 * @param ps
	 */
	public static void closeResource(Connection conn,Statement ps){
		try {
			if(ps != null)
				ps.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		try {
			if(conn != null)
				conn.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	/**
	 * 
	 * @Description 关闭资源操作
	 * @author shkstart
	 * @date 上午10:21:15
	 * @param conn
	 * @param ps
	 * @param rs
	 */
	public static void closeResource(Connection conn,Statement ps,ResultSet rs){
		try {
			if(ps != null)
				ps.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		try {
			if(conn != null)
				conn.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		try {
			if(rs != null)
				rs.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 
	 * @Description 使用dbutils.jar中提供的DbUtils工具类，实现资源的关闭
	 * @author shkstart
	 * @date 下午4:53:09
	 * @param conn
	 * @param ps
	 * @param rs
	 */
	public static void closeResource1(Connection conn,Statement ps,ResultSet rs){
//		try {
//			DbUtils.close(conn);
//		} catch (SQLException e) {
//			e.printStackTrace();
//		}
//		try {
//			DbUtils.close(ps);
//		} catch (SQLException e) {
//			e.printStackTrace();
//		}
//		try {
//			DbUtils.close(rs);
//		} catch (SQLException e) {
//			e.printStackTrace();
//		}
		DbUtils.closeQuietly(conn);
		DbUtils.closeQuietly(ps);
		DbUtils.closeQuietly(rs);
	}
}
```

## Apache-DBUtils实现CRUD操作

### 使用QueryRunner测试添加数据的操作

```java
//测试插入
@Test
public void testInsert() {
    Connection conn = null;
    try {
        QueryRunner runner = new QueryRunner();
        conn = JDBCUtils.getConnection3();
        String sql = "insert into customers(name,email,birth)values(?,?,?)";
        int insertCount = runner.update(conn, sql, "蔡徐坤","caixukun@126.com","1997-09-08");
        System.out.println("添加了" + insertCount + "条记录");
    } catch (SQLException e) {
        e.printStackTrace();
    }finally{

        JDBCUtils.closeResource(conn, null);
    }
}
```

### 使用QueryRunner测试查询数据的操作

```java
package com.atguigu5.dbutils;

import java.sql.Connection;
import java.sql.Date;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;
import java.util.Map;

import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.ResultSetHandler;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.MapHandler;
import org.apache.commons.dbutils.handlers.MapListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;
import org.junit.Test;

import com.atguigu2.bean.Customer;
import com.atguigu4.util.JDBCUtils;

/*
 * commons-dbutils 是 Apache 组织提供的一个开源 JDBC工具类库,封装了针对于数据库的增删改查操作
 * 
 */
public class QueryRunnerTest {
	
	//测试插入
	@Test
	public void testInsert() {
		Connection conn = null;
		try {
			QueryRunner runner = new QueryRunner();
			conn = JDBCUtils.getConnection3();
			String sql = "insert into customers(name,email,birth)values(?,?,?)";
			int insertCount = runner.update(conn, sql, "蔡徐坤","caixukun@126.com","1997-09-08");
			System.out.println("添加了" + insertCount + "条记录");
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			
			JDBCUtils.closeResource(conn, null);
		}
		
	}
	
	//测试查询
	/*
	 * BeanHander:是ResultSetHandler接口的实现类，用于封装表中的一条记录。
	 */
	@Test
	public void testQuery1(){
		Connection conn = null;
		try {
			QueryRunner runner = new QueryRunner();
			conn = JDBCUtils.getConnection3();
			String sql = "select id,name,email,birth from customers where id = ?";
			BeanHandler<Customer> handler = new BeanHandler<>(Customer.class);
			Customer customer = runner.query(conn, sql, handler, 23);
			System.out.println(customer);
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(conn, null);
			
		}
		
	}
	
	/*
	 * BeanListHandler:是ResultSetHandler接口的实现类，用于封装表中的多条记录构成的集合。
	 */
	@Test
	public void testQuery2() {
		Connection conn = null;
		try {
			QueryRunner runner = new QueryRunner();
			conn = JDBCUtils.getConnection3();
			String sql = "select id,name,email,birth from customers where id < ?";
			
			BeanListHandler<Customer>  handler = new BeanListHandler<>(Customer.class);

			List<Customer> list = runner.query(conn, sql, handler, 23);
			list.forEach(System.out::println);
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			
			JDBCUtils.closeResource(conn, null);
		}
		
	}
	
	/*
	 * MapHander:是ResultSetHandler接口的实现类，对应表中的一条记录。
	 * 将字段及相应字段的值作为map中的key和value
	 */
	@Test
	public void testQuery3(){
		Connection conn = null;
		try {
			QueryRunner runner = new QueryRunner();
			conn = JDBCUtils.getConnection3();
			String sql = "select id,name,email,birth from customers where id = ?";
			MapHandler handler = new MapHandler();
			Map<String, Object> map = runner.query(conn, sql, handler, 23);
			System.out.println(map);
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(conn, null);
			
		}
		
	}
	
	/*
	 * MapListHander:是ResultSetHandler接口的实现类，对应表中的多条记录。
	 * 将字段及相应字段的值作为map中的key和value。将这些map添加到List中
	 */
	@Test
	public void testQuery4(){
		Connection conn = null;
		try {
			QueryRunner runner = new QueryRunner();
			conn = JDBCUtils.getConnection3();
			String sql = "select id,name,email,birth from customers where id < ?";
		
			MapListHandler handler = new MapListHandler();
			List<Map<String, Object>> list = runner.query(conn, sql, handler, 23);
			list.forEach(System.out::println);
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(conn, null);
			
		}
		
	}
	/*
	 * ScalarHandler:用于查询特殊值
	 */
	@Test
	public void testQuery5(){
		Connection conn = null;
		try {
			QueryRunner runner = new QueryRunner();
			conn = JDBCUtils.getConnection3();
			
			String sql = "select count(*) from customers";
			
			ScalarHandler handler = new ScalarHandler();
			
			Long count = (Long) runner.query(conn, sql, handler);
			System.out.println(count);
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(conn, null);
			
		}
		
	}
	@Test
	public void testQuery6(){
		Connection conn = null;
		try {
			QueryRunner runner = new QueryRunner();
			conn = JDBCUtils.getConnection3();
			
			String sql = "select max(birth) from customers";
			
			ScalarHandler handler = new ScalarHandler();
			Date maxBirth = (Date) runner.query(conn, sql, handler);
			System.out.println(maxBirth);
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(conn, null);
			
		}
		
	}
	
	/*
	 * 自定义ResultSetHandler的实现类
	 */
	@Test
	public void testQuery7(){
		Connection conn = null;
		try {
			QueryRunner runner = new QueryRunner();
			conn = JDBCUtils.getConnection3();
			
			String sql = "select id,name,email,birth from customers where id = ?";
			ResultSetHandler<Customer> handler = new ResultSetHandler<Customer>(){

				@Override
				public Customer handle(ResultSet rs) throws SQLException {
//					System.out.println("handle");
//					return null;
					
//					return new Customer(12, "成龙", "Jacky@126.com", new Date(234324234324L));
					
					if(rs.next()){
						int id = rs.getInt("id");
						String name = rs.getString("name");
						String email = rs.getString("email");
						Date birth = rs.getDate("birth");
						Customer customer = new Customer(id, name, email, birth);
						return customer;
					}
					return null;
					
				}
				
			};
			Customer customer = runner.query(conn, sql, handler,23);
			System.out.println(customer);
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(conn, null);		
        }	
	}
}
```

### 使用dbutils进行资源的关闭

```java
/**
	 * 
	 * @Description 使用dbutils.jar中提供的DbUtils工具类，实现资源的关闭
	 * @author shkstart
	 * @date 下午4:53:09
	 * @param conn
	 * @param ps
	 * @param rs
	 */
	public static void closeResource1(Connection conn,Statement ps,ResultSet rs){
//		try {
//			DbUtils.close(conn);
//		} catch (SQLException e) {
//			e.printStackTrace();
//		}
//		try {
//			DbUtils.close(ps);
//		} catch (SQLException e) {
//			e.printStackTrace();
//		}
//		try {
//			DbUtils.close(rs);
//		} catch (SQLException e) {
//			e.printStackTrace();
//		}
		
		DbUtils.closeQuietly(conn);
		DbUtils.closeQuietly(ps);
		DbUtils.closeQuietly(rs);
	}
```



