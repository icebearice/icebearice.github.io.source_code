---
layout: article
title: Java基础知识体系串讲2-对象与类
date: 2019-09-03 15:20:40
updated: 2019-09-03 15:55:40
categories: 
  - Java
tags: 
  - Java
---

## Java基础知识体系串讲1-基础知识

#### 面向对象

- 一个类应该有

  - 实例域 私有的数据域
    - 公有的域访问器和域更改器 getter 和 setter
  - 构造方法
  - toString()方法

- 隐式参数和显式参数

  - Number007.raiseSalary(5)
    - 出现在方法名前的是隐式参数 number007
    - 出现在方法名后面括号中的数值是显式参数 5

- 一个方法可以访问所属类的所有对象的私有数据

- final实例域

  - 一个构造器执行之后，这个域的值被设置，并且在后面的操作中，不能够再对它的值进行修改

    ```java
    private final String name;
    ```

  - final关键字若修饰对象的话，只是表示存储在evaluations变量中的对象引用不会再指示其他StringBuilder对象，但是对象是可以修改的

    ```java
    private final StringBuilder evaluations = new StringBuilder();
    public void giveGoldStar(){
    	evaluations.append(LocalDate.now(): ":Gold star!\n");
    }
    ```

 - static域与static方法

    - 静态域

       - 每个类只有一个这样的域，而每个对象对于所有的实例域却都有自己的一份拷贝，这个类的所有实例将共享一个nextId域

         ```	java
         class xxx{
         	private static int nextId = 1;
         }	
         ```

      - 它只属于类，不属于任何独立的对象

   - 静态常量

     ```java
     public static final double PI = 3.1415......
     ```

     - 可以采用Math.PI获得

   - 静态方法

     ```java
     Math.pow(x, a) // x的a次方
     ```

     - 没有隐式参数，只有显式参数
     - 静态方法是一种不能向对象实施操作的方法
     - 在下面两种情况下使用静态方法
       - 一个方法不需要访问对象状态，其所需参数都是通过显式参数提供 Math.pow(x, a)
       - 一个方法只需访问类的静态域 Employee.getNextId()

- 工厂方法

  - 使用工厂方法来构造对象
  - 为什么不利用构造器来完成这些操作呢？
    - 无法命名构造器，所有的构造器只有参数不同，无法根据名字来选择所需要的对象
    - 当好似哟给你构造器时，无法改变所构造的对象类型，构造器获得的对象永远都是初始，不能是继承的对象

- 方法参数

  - 在程序设计语言中，方法接收参数传递的方式有两种

    - 值调用 值传递
      - 表示方法接受的是调用者提供的值
    - 引用调用 引用传递
      - 表示方法接收的是调用者传递的变量地址
    - 名调用（已成为历史）
      - Algol 最古老的程序设计语言之一 用的就是这种调用方式

  - Java总是按值调用 方法得到的是所有参数值的一个拷贝

  - 方法不能修改传递给它的任何参数变量的内容

  - 而对象引用作为参数就不同了

    ```java
    public static void tripleSalary(Employee x){	// works
      x.raiseSalary(200);
    }
    harry = new Employee(. . .);
    tripleSalary(harry);
    ```

    1. x被初始化为harry值的拷贝，这里是一个对象的引用
    2. raiseSalary方法应用于这个对象引用，x和harry同时引用的那个Employee对象的薪金提高了200%
    3. 方法结束后，参数变量x不再使用。当然，对象变量harry继续引用那个薪金增值3倍的雇员对象
    
  - Java中参数的使用情况

    - 一个方法不能修改一个基本数据类型的参数
    - 一个方法可以改变一个对象参数的状态
    - 一个方法不能让对象参数引用一个新的对象

- 突然想到一个问题 我们要如何主动回收对象 在Java里面 emmm 我为什么会想到这呢 我也不知道 emmm

  - 搜索另一篇文章 辣鸡回收 GC finalize 方法小结

- 重载

    - 有些类有多个构造器，有相同的名字和不同的参数，便产生了重载
    - 如果在构造器中没有显式地给域赋予初值，那么就会自动地赋予默认值：0、false或者null
        - 这样子会影响代码的可读性

- 无参数的构造器

    - 编写一个类时没有编写构造器，那么系统就会提供一个无参数构造器，这个构造器将所有的实例域设置为默认值
    - 如果类中提供了至少一个构造器，但是没有提供无参数的构造器，则在构造对象时如果没有提供参数就会被视为不合法

- 一种另类的构造器

    ```java
    public Employee(double s) {
    	// calls Employee(String, double);
    	this("Employee #" + nextId, s)
    	nextID ++;
    }
    ```

- 初始化快

    ```java
    class Employee {
    	private static int nextId;
    	private int id;
    	private String name;
    	private double salary;
    	// object initialization block
    	{
    		id = nextId;
    		nextId ++;
    	}
    	public Employee(String n, double s) {
    		name = n;
    		salary = s;
    	}
    }
    ```

    - 无论使用哪个构造器构造对象，id域都在对象初始化块中被初始化

- 对象析构与finalize方法

    - C++有显式的析构器方法，其中放置一些当对象不再使用时需要执行的清理代码，在析构器中，最常见的操作时回收分配给对象的存储空间
    - 由于Java有自动的垃圾回收器，固Java不支持析构器
    - 可以为任何一个类添加finalize方法，将在垃圾回收器清理对象之前调用。在实际应用中，不要依赖于使用finalize方法回收任何短缺的资源，这是因为很难知道这个方法什么时候才能调用
    - 如果某个资源需要在使用完毕后立刻关闭，那么就需要由人工来管理。对象用完时，可以应用一个close方法完成相应的清理操作

- 类的导入

    - 每个类名之前添加完整的包名
    - 使用import语句，无需添加包前缀
    - 静态导入 import static 无需添加 类名前缀

- Java成员变量的作用域

    | 作用域    | 当前类 | 同一package | 子类   | 其他package |
    | :-------- | ------ | ----------- | ------ | ----------- |
    | public    | 可见   | 可见        | 可见   | 可见        |
    | protected | 可见   | 可见        | 可见   | 不可见      |
    | default   | 可见   | 可见        | 不可见 | 不可见      |
    | private   | 可见   | 不可见      | 不可见 | 不可见      |

