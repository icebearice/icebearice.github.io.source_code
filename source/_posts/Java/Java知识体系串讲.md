---
layout: article
title: Java基础知识体系串讲
date: 2019-08-29 22:20:40
updated: 2019-08-29 22:20:40
categories: 
  - Java
tags: 
  - Java
---

## Java基础知识体系串讲

#### 知识层次分布

- 特性
  - 简单性
    - Java没有头文件 指针运算 结构体 联合 操作符重载 等C++的特性 使用起来相对简单
  - 面向对象
    - 面向对象是一种程序设计技术，它将重点放在数据（即对象）和对象的接口上
    - 与C++不同的点在于多重继承，Java取而代之的是接口概念
  - 分布式（加粗为总结 非加粗为原文）
    - **我的理解：为什么Java有分布式的特性：**
      - **Java有丰富的库，用于处理TCP/IP协议，即可通过对应协议处理URL访问对应网络对象，从而可以建立起一条网络对象与网络对象之间沟通的渠道，从而实现分布式通信网络互联的多处理机体系结构上**
    - Java有丰富的库，用于处理像HTTP和FTP之类的TCP/IP协议
    - Java应用程序能够通过URL打开和访问网络上的对象，极其便携
    - 分布式软件系统是支持分布式处理的软件系统，是在由**通信网络互联**的**多处理机体系结构**上执行任务的系统。它包括分布式操作系统、分布式程序设计语言及其编译（解释）系统、分布式文件系统和分布式数据库系统等
  - 健壮性
    - **Java编译器能够检测许多在其他语言仅在运行时才能够检测出来的问题**
    - **Java采取的指针模型消除了重写内存和损坏数据的可能性**
    - Java投入了大量的精力进行了早期的问题检测、后期动态的（运行时）的检测
  - 安全性
    - Java能够防范各种攻击
      - 运行时堆栈的溢出。如蠕虫和病毒常用的攻击手段
      - 破坏自己的进程空间之外的内存
      - 未经授权读写文件
  - 体系结构中立
    - 编译器生成一个体系结构中立的目标文件格式，这是一种编译过的代码，称为字节码
    - Java编译器通过生成与特定的计算机体系结构无关的字节码指令来实现这一特性
    - 精心设计的字节码可以在任何机器上解释运行
    - 实际运行的环境为机器上的虚拟机
      - 解释虚拟机指令肯定会比全速运行机器指令药慢很多
      - 虚拟机也可以将执行最频繁的字节码序列翻译成机器吗，这一过程称为即时编译
  - 可移植性
    - Java由于上述虚拟机的存在，所有的数据类型都有固定的大小
    - 二进制数据以固定的格式进行存储和传输，消除了字节顺序的困扰
    - 字符串是以表转的unicode格式存储的
    - Java语言编译程序只需将**编译**生成在Java虚拟机运行的字节码，Java虚拟机在执行字节码时，把字节码**解释**成目标平台的机器吗，进行执行，“一次编译，到处运行”
  - 解释型
    - Java解释器可以在任何移植了解释器的机器上执行Java字节码
  - 高性能
  - 多线程
    - 针对多线程有自己的一套库的处理机制
  - 动态性
  
- 基本数据类型（不包括引用数据类型）
  - 整形
    - int		4byte      -20亿～20亿（大于20亿） -2^31~2^31-1
    - short	2byte      -32768～32767 -2^15~2^15-1
    - long	8byte        -2^63~2^63-1
    - Byte     1byte       -2^7~-2^7-1
    - 进制前缀 二进制 0b 八进制 0 十六进制 0x 例如0xCAFE（魔数）
  - 浮点型
    - float    4byte    
    - double   8byte
    - 默认为double float后面有后缀F或f 没有后缀F默认为double
    - 原因是float类型的精度很难满足需求
    - 存在三种特殊的浮点数值
      - 正无穷大 Double.POSITIVE_INFINITY
      - 负无穷大 Double.NEGATIVE_INFINITY
      - NaN（is not number）Douboe.NaN  判断是否是一个数值 Double.isNaN(x)
  - Unicode和char类型
    - 2字节 16位 Unicode-16
  - boolean类型
    - 两个字节 与short同等大小 2byte 16位
  
- 变量与对象

  - 变量一定要初始化，初始化为对应的值，无默认

- 常量

  - 关键字final表示这个变量只能被赋值一次。一旦被赋值之后，就不能再更改
  - 习惯上，常量名使用全大写
  - 使用static final来定义一个类常量
  - 普通常量定义在方法的内部，仅供方法使用
  - 类常量定义在方法外部，可供一个类中的多个方法使用

- 0

  - 整数被除0将会产生一个异常，而浮点数被0除将会得到无穷大或者NaN的结果

  - ```
    public static final double NaN = 0.0d / 0.0;
    public static final double POSITIVE_INFINITY = 1.0 / 0.0;
    public static final double NEGATIVE_INFINITY = -1.0 / 0.0;
    ```

  - emmm Java中浮点数除以0 是无穷大 记住吧 比较特殊

- Math方法

  - Math方法是可以直接调用的，无须import

  - import static java.lang.Math.*;  此为静态导入 静态导入后 调用方法时，无需添加前缀Math

  - 常用方法

    - max
    - min
    - pow
    - sqrt
    - Random 此处贴上一份关于Random的研究
    - 随机数也可以使用Random类来实现 是伪随机数
    - https://blog.csdn.net/qq_33101675/article/details/81028210
    - round 四舍五入

- 强制类型转换    

  - 注意精度
  
    - int转换为float long转换为float long转换为double 都可能损失精度

- 自增自减运算符
  
  - 前缀形式的 会先完成加一 再使用加一后的新变量 ++i
  
  - 后缀形式的 会先使用旧变量 再完成加一 i++
  
- 位运算符

  - & and | or ^ xor(异或) ~ nor(同或) 
  - << 位模式左移 逻辑左移 右边补0
  - \>> 位模式右移 逻辑右移 左边补符号位
  - \>>> 无符号位模式右移 无符号右移 左边补0
  - 优先级 括号优先 判断优先 其次运算 最后赋值

- 枚举类型

  - 变量的取值只在一个有限的集合内，若直接存放在变量中，可能会保存为错误的值

  - 可自定义枚举类型，枚举类型包括有限个命名的值

    ```java
    enum Size{SMALL, MEDIUM, LARGE, EXTRA_LARGE}
    Size s = Size.MEDIUM;
    ```

  - Size类型的变量只能存储这个类型声明中给定的某个枚举值，或者null，null表示这个变量没有设置任何值

- String

  - 概念上讲，Java字符串就是Unicode字符序列

  - Java标准类库中提供了String的预定义类，每个双引号扩起来的字符串都是String类的一个实例

  - 《深入理解Java虚拟机》中写到，String是存放在常量池中的

  - 严谨点的话，String并不是Java中的基本数据类型，是属于引用数据类型，上面讲的其实都是基本数据类型

  - 获取子串

    ```java
    String greeting = "Hello";
    String s = greeting.substring(0, 3);
    ```

    得到“Hel”子串，首参数为开始的位置，次参数为结束的位置（不包括结束位置本身）

  - 拼接字符串

    - 用+号拼接

    - int也可以参与拼接

    - ```java
      int a = 123;
      int b = 456;
      String s = a + "" + b;	// 输出 123456
      String ss = a + b + "";	// 输出 579 
      ```

  - 字符串在Java中是不可变的，需要变化字符串靠拆分与拼接

  - 不可变的好处 编辑器可以让字符串共享 

  - 从jdk6及以前，String常量池存放在方法区中的运行时常量池中，方法区位于永久代

  - jdk7，String常量池转移到堆中

  - Jdk8，永久代被取消，新增MetaSpace

    ```java
    // TODO 虚拟机也得好好复习一遍了 好多东西讲的不是很清楚 串不起来 
    ```

  - String.intern()
    - intern()方法设计的初衷，就是重用String对象，以节省内存消耗
    
  - 使用intern()方法，运行时间增长，这是因为，虚拟机会先new String后再intern指向常量池中的引用
    
  	  ```java
      String s = new String("1");  //常量池中添加“1”，生成堆对象s，返回指向堆中s的引用
    	String intern = s.intern();  //常量池中已有“1”，返回常量池中“1”的引用
    	String s2 = "1";  //常量池中已有“1”，返回常量池中“1”的引用
  		System.out.println(s2 == intern);//true  两者返回的都是常量池中“1”的引用
      System.out.println(s == intern);//false  堆引用地址和常量池引用地址不相等
    
    	原文链接：https://blog.csdn.net/hz90s/article/details/80819619
  	```
    	
    - 实验证明，如引用常量池中的String，会有限返回intern生成的新的堆引用地址的String
    
  - String.equal()

    - 比较字符串的内容是否相等
    - 如果用等号==来比较，会比较位置是否相等，而不是比较内容是否相等

  - 空串和null

    - 空串即长度为0或者内容为空

      ```java
      String = "";
      ```

    - null即表示目前没有任何对象与该变量关联

      ```java
      if(str == null)	// probably appear error
      ```

- 码点与代码单元

  - emmm 这是个我认为比较偏的概念 core 卷1上讲的云里雾里的 如果你求知欲很强的话 就看下去 我会用比较接地气的方式解释这个名词 个人认为 不了解 只是少个知识点而已 影响不大 可不看
  - 但是 谁叫我 什么东西都想知道全面一点呢 没办法 还是吃掉这个知识点吧
  - 先讲两个名词解释
  - 码点：就是某个任意字符在Unicode编码表中对应的代码值
  - 代码单元：是在计算机中用来表示码点的，大部分码点只需要一个代码单元表示，但是有一些是需要两个代码单元来表示的
  - 上面讲到 我们的char采用utf-16编码表示unicode字符 占两个字节 16位 故utf-16的代码单元占2个字节 如果是utf-8的话 就占1个字节
  - 大多数的常用的Unicode字符使用两个字节就能表示 但是 实际上 两个字节 16位 也不够用了 已经出现一些字符 超过两个字节
  - 就比如 U+1D546 长这样 𝕆 要两个代码单元 
  - 为了对比明显 列举一个只要一个代码单元的 U+0022 长这样 “ 就是我们的双引号啦
  - ![屏幕快照 2019-08-29 下午5.23.15](/Users/icebearice/Desktop/屏幕快照 2019-08-29 下午5.23.15.png)
  - 不要使用char放 放不下啊兄弟

- StringAPI (括号中的括号为可选的意思)

  - char charAt(int index)

  - boolean equal(Object other)

  - boolean startsWith(String prefix) 

  - boolean endsWith(String suffix)

  - 如果字符串以prefix 或者 suffxi 开头或结尾 返回true

  - int indexOf(String str (int fromIndex))

  - int indexOf(int cp (int fromIndex))

  - 返回子串或者字符的位置 从fromIndex开始（可选）

  - length() 整个Java中 只有String有length() 加括号 而数组长度为 length 其他什么找长度的 都是size

  - String substring(int beginIndex, (int endIndex))

  - String toLowerCase() 

  - String toUpperCase()

  - String trim() — 修 剪

  - 删除字符串头部和尾部的空格

  - String join(CharSequence delimiter, CharSequence … elements)

  - 看不懂？ 给你个样例

    ```java
    String[] arrStr=new String[]{"a","b","c"};
    
    System.out.println(String.join("-", arrStr));
    
    
    输出：
    1-2-3
    a-b-c
    ```

  - String replace（CharSequence oldString, CharSequence newString）

- StringBuilderAPI

  - int length()
  - StringBUilder append(String str)
  - StringBuilder append(char c)
  - StringBuilder appendCodePoint(int codepoint)
  - 追加一个码点，并将其转换为一个或两个代码大院并返回this
  - void setCharAt(int i, char c)
  - 将第i个代码单元设置为c
  - StringBuilder insert(int offset, String str / Char c)
  - StringBuilder delete(int startIndex, int endIndex)
  - delete startIndex ~ endIndex-1
  - toString()

- 输入输出(两种方式)

  - Scanner in = new Scanner(System.in);
    - String in.nextLine() 下一行
    - String in.next(); 下一个单词
    - int in.nextInt(); 
    - double in.nextDouble();
    - boolean in.hasNext();
    - boolean in.hasNextInt();
    - boolean in.hasNextDouble();
  - BufferReader buffer = new BufferedReader(new InputStreamReader(System.in))
    - buffer.readLine();

- 格式化输出

  - System.out.printf("%s", s);
    - d 十进制整数
    - x 十六进制整数
    - o 八进制整数
    - f 定点浮点数
    - e 指数浮点数
    - g 通用浮点数
    - a 十六进制浮点数
    - s 字符串
    - c 字符
    - b 布尔
    - h 散列码
    - tx or Tx 日期时间
    - % 百分号 （要输入百分号的时候 要打 %%）
    - n 与平台的分隔符有关 windows相当于\n\r unix相当于\r

- Switch

  - 这里提及switch语句是有私心的 因为 我只是想说一下 switch 在go里面 是默认带break的 哈哈哈

- 大数值

  - 若基本的整数和浮点数精度不能够满足需求，java.math有两个很有用的类
    - BigInteger and BigDecimal （我竟然不知道decimal是小数的意思）
    - 前者实现了任意精度的整数运算，后者实现了任意精度的浮点数运算

- 数组

  - 爱怎么初始化怎么初始化
    - int[] a
    - int a[]
    - int [] a = new int[100]

- 不规则数组

  - Java实际上没有多维数组，只有一维数组。多维数组被解释为“数组的数组”

