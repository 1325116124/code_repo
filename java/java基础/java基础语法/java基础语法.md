# java基础语法

- **java的整型常量默认为 int 型，声明long型常量须后加‘l’或‘L’**
- **java程序中变量通常声明为int型，除非不足以表示较大的数，才使用long**

![image-20210924165822287](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924165822287.png)

## 数组

![image-20210924170356967](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924170356967.png)

![image-20210924170437779](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924170437779.png)

**多维数组**

![image-20210924170732437](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924170732437.png)

![image-20210924170935927](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924170935927.png)

**Arrays工具类的使用**

![image-20210924171159599](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924171159599.png)

## 类和对象

![image-20210924172120376](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924172120376.png)

![image-20210924172611265](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924172611265.png)

**方法的重载**

![image-20210924172755944](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924172755944.png)

**可变参数的形参**

![image-20210924173005671](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924173005671.png)

![image-20210924173015627](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924173015627.png)

**值传递和地址值传递**

![image-20210924173221918](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924173221918.png)

**封装性**

![image-20210924174020564](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924174020564.png)

**this的使用**

![image-20210924174318980](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924174318980.png)

**继承**

![image-20210924175335787](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924175335787.png)

![image-20210924175706917](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924175706917.png)

![image-20210924180009993](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210924180009993.png)

**多态性**

![image-20210927184353170](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210927184353170.png)

![image-20210927184846596](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210927184846596.png)

![image-20210927190628316](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210927190628316.png)

**equals方法的重写**

```java
@Override
public boolean equals(Object obj) {
    if(this == obj){
        return true;
    }
    if(obj instanceof MyDate){
        MyDate myDate = (MyDate)obj;
        return this.day == myDate.day && this.month == myDate.month &&
        this.year == myDate.year;
    }
    return false;
}
```

## 包装类的使用

![image-20210927194011437](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210927194011437.png)

**字符串和基本数据类型的转换**

![image-20210927194155434](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210927194155434.png)

## static关键字

![image-20210928005544969](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928005544969.png)

**没有对象的实例时，可以用类名.方法名()的形式访问由static修饰的类方法。**

**在static方法内部只能访问类的static修饰的属性或方法，不能访问类的非static的结构。**

**因为不需要实例就可以访问static方法，因此static方法内部不能有this。(也不能有super ? YES!)**

## 单例设计模式

### 饿汉式

```java
class Singleton{
    private Singleton(){
        
    }
    private static Singleton single = new Singleton();
    public static Singleton getInstance(){
        return single;
    }
}
```

### 懒汉式

```java
class Singleton {
    private Singleton(){
        
    }
    private static Singleton single;
    public  static Singleton getInstance() {
        if(single == null) {
            single = new Singleton();
        }
        return single;SS
    }
}
```

## 代码块

![image-20210928010338793](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928010338793.png)

![image-20210928010455028](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928010455028.png)

## final关键字

![image-20210928010559410](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928010559410.png)

## 抽象类

![image-20210928010947293](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928010947293.png)

## 接口

![image-20210928011329921](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928011329921.png)

![image-20210928011412115](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928011412115.png)

![image-20210928011520737](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928011520737.png)

![image-20210928011618193](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928011618193.png)

**实现接口的类也可以被接口用作多态**

![image-20210928193736797](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928193736797.png)

**java8中接口的改进**

![image-20210928203355068](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928203355068.png)

![image-20210928203648139](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210928203648139.png)

## 异常处理

![image-20210929002951268](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210929002951268.png)

![image-20210929003316599](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210929003316599.png)

**自定义异常**

![image-20210929003626871](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210929003626871.png)

![image-20210929003757385](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210929003757385.png)
