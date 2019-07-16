---
layout: article
title: 关于Java内存泄漏的思考
date: 2019-05-08 10:16:23
updated: 2019-05-08 10:18:19
categories: 
  - Java
tags: 
  - Java
---

# 关于Java内存泄漏的思考



### Java内存回收的机制

Java虚拟机针对垃圾回收有一套自己的良好的机制, 但是, 即便在Java看似完美的垃圾回收机制下, 除了内存溢出 Out of memory 和 堆栈溢出 Stack overflow 两大异常之外, 其实还有内存溢出 Memory leak的现象

### Java内存泄露引起的原因

内存泄漏是指无用对象(不再使用的对象)持续占有内存或无用对象得不到内存释放, 从而造成的内存空间的浪费称为内存泄漏.

内存泄漏不严重的时候, 根本察觉不到, 但严重时, JVM会抛出Out of memory的异常

#### 根本原因

长生命周期对象对短生命周期对象的引用, 导致内存泄漏

尽管短生命周期已经不需要再被使用, 但长生命周期对象持有对它的引用而导致JVM不能对其进行回收, 造成内存泄漏

1. 静态集合类的内存泄漏

   静态集合类Static Vector 或者 Static HashMap 的生命周期和应用程序是一致的, 他们所引用的所有的对象Object也不能被释放

```java
Static Vector v = new Vector(10);
for (int i = 1; i<100; i++) {
	Object o = new Object();
	v.add(o);
	o = null;
}
```

