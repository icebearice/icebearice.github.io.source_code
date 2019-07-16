---
layout: article
title: Java源码
date: 2019-4-23 00:41:00
updated: 2019-04-26 22:50:06
categories: 
  - Java源码
tags: 
  - Java源码
---

# Java源码

### Map HashMap LinkedHashMap TreeMap ConcurrentHashMap HashTable

#### 1 Hash Map

存放键值对

JDK1.8之后，链表长度大于阈值8，将链表转换为红黑树

要将链表转换成红黑树的table的最小结点数为64

(n - 1) & hash 判断位置

jdk1.8后 hash函数 即 扰动函数 扰动一次 1.8前 扰动四次

默认容量 16 负载因子 0.75

**loadFactor太大导致查找元素效率低，太小导致数组的利用率低，存放的数据会很分散。loadFactor的默认值为0.75f是官方给出的一个比较好的临界值**。

**threshold = capacity \* loadFactor**，**当Size>=threshold**的时候，那么就要考虑对数组的扩增了，也就是说，这个的意思就是 **衡量数组是否需要扩增的一个标准**。

hashCode 将key的hashCode 异或^ value的hashCode

Node节点中的equals 1 先确定是否是对象本身 2 若不是对象本身, 则判断是否为相同map.entry的实例 3 若为相同实例, 才能继续判断 对应的key是否equals 	 对应的value是否equals

HashMap中 root 方法调用  根节点 不断地返回 parent 

红黑树 双向 parent left  right prev

resize 扩容函数 若没超最大值 则扩容两倍 newThr = oldThr << 1

resize 尽量避免 因为  resize扩容之后  会将所有的oldtable值  全部重新遍历赋值到newtable中

(e = oldTab[j]) != null)  为什么要这样子写 好处是什么???

构造方法有四种:

1. 无参构造
2. 指定容量大小
3. 指定容量大小和负载因子
4. 包含另一个"Map"的构造函数

HashMap为什么会 线程不安全  从底层的角度思考

首先 HashMap与HashTable的区别就是 HashTable加了synchronized锁 HashMap没加

其次 冲突可能出现在放置元素的put方法中,也可能出现在扩容方法resize中,我们先从put方法说起,假设版本在jdk1.8,冲突的对应链表节点的数量未超过8的情况下,假设同时有两个以上的线程出现同一个哈希碰撞,并且两个线程同时取到了冲突链表上的同一个位置,在这种情况下,必定有一个线程的值会丢失,只有一个坑可以占

另外在resize的扩容方法中,由于扩容之后,需要将所有的节点赋值给已经扩容的新表,假设存在多个线程进行扩容,同时进行的话,只有最后赋值成功的新表,才会成为新表,其他在此线程之前重新赋值的表都作废

表是以节点数组的状态存在的

#### HashCode为什么以一定是2的次幂

##### (看链接)<http://www.cnblogs.com/chengxiao/p/6059914.html#t3>

HashMap和HashTable区别

1. 前者线程不安全, 后者线程安全,原因是因为后者加了synchronized锁
2. 前者可以接受null的key和value, 后者不行
3. 在单线程的情况下, 前者的性能要比后者要快
4. 前者的初始容量为16, 扩容以指数形式扩容, 即2的倍数, 后者的初始容量为11, 扩容是两倍的原容量+1

#### 2 LinkedHashMap 继承HashMap

底层是super调用父类的无参构造方法

存在accessOrder的boolean变量 若为false则插入是有序的, 若为true,则插入和访问的顺序都是有序的, 会对里面的键值对进行排序

实则是本质上是双向链表+HashMap

### 面试中关于HashMap的时间复杂度O(1)的思考

分四步： 
1.判断key，根据key算出索引。 
2.根据索引获得索引位置所对应的键值对链表。 
3.遍历键值对链表，根据key找到对应的Entry键值对。 
4.拿到value。 

以上四步要保证HashMap的时间复杂度O(1)，需要保证每一步都是O(1)，现在看起来就第三步对链表的循环的时间复杂度影响最大，链表查找的时间复杂度为O(n)，与链表长度有关。我们要保证那个链表长度为1，才可以说时间复杂度能满足O(1)。但这么说来只有那个hash算法尽量减少冲突，才能使链表长度尽可能短，理想状态为1。因此可以得出结论：HashMap的查找时间复杂度只有在最理想的情况下才会为O(1)，而要保证这个理想状态不是我们开发者控制的。

查询插入 O(1) 删除O(n)



### ArrayList

有序可重复 可克隆 可序列化

继承abstractList 实现list<E> RandomAccess(快速随机访问 故用for循环遍历效率更高 但用iterator访问更安全 因为当访问的数据发生改变的时候 他会抛异常 异常是) Clonable Serializable

默认初始容量大小为10 初始为空数组 添加第一个元素后 容量才变成10 

通过反射来判断对象是否符合elementData.getClass() != Object[].class

如果未指定Object, 则令elementData为默认空数组

修改实力容量为当前大小(最小化ArrayList实例)是通过copyof实现的

扩容因子为1.5 int newCapacity = oldCapacity + (oldCapacity >> 1)

检查newCapacity是否溢出, 若溢出了则赋值为数组最大值 Integer.MAX_VALUE 同时 对象头的最大信息占用内存不可超过8个字节

为什么最大数组大小为Integer.MAX_VALUE - 8 因为8个字节用来存数组的头信息

占满32位的内存单元长度

遍历O(1) 插入删除O(n)

关于自动装箱与自动拆箱 Java中的八大数据类型都有对应的包装类 Java是以对象的形式来保存八大数据类型的, 实现真正的面向对象

三个构造函数

1. 无参
2. 指定容量
3. 指定集合

copyof为重构当前数组 底层调用了arraycopy 目标函数为新数组

copyofrange是对数组进行复制

arraycopy需要目标函数,拷贝到自定义的数组中

底层采用newcapacity - mincapacity < 0 代替 newcapacity < mincapacity 运算更快

remove 删除对象 fastremove 删除u索引 remove调用了faseremove clear 删除所有元素addAll 追加添加 可指定位置 modCount 改变list大小的次数

扩容机制 为了避免只扩充少量频繁的拷贝的情况 导致降低性能 有着良好的扩容机制 扩容因子为1.5倍

Arrays.toArray() 得到的是Object[] 不可向下转型为其他对象类型

Arrays.toArray(T<>[]) 好用 

##### 那ArrayList为什么线程不安全呢

从底层来讲 举个例子 比如 在当前size为0的情况下 ArrayList底层会在有第一个数据存入的时候 将容量改为初始容量 出现多个线程同时存储值进入list中 只有最后一个存储的线程的值可以存到list中 而其他的线程的值全都会丢失 而不会存入list中 尽管capacity为初始容量 16  但 size还是等于1 只有一个值存了进来

### LinkedList 线程不安全 synchronizedList

在任何位置高效地插入和删除的有序序列 基于 双向链表实现的

继承AbstractSequentialList

有一个头结点 一个尾节点 我们可以从头遍历 也可以从尾遍历

checkPositionIndex(index); 检查index是否在size范围内

始终会记录着first节点和last节点



#### Vector

可变长度的数组	长度变化靠capacity 和capacityIncrement

构造方法三种

1. 无参构造 默认构造初始容量为10
2. initialCapacity 初始化容量 一个参数
3. initialCapacity 初始化容量 capacityIncrement 容量增长值 两个参数

其中构造方法1调用构造方法2 构造方法2调用构造方法3 默认初始容量为10 默认容量增长值为0(默认扩增为两倍原长度)

#### Stack

继承Vector	线程安全

push(E) pop() peek() empty()

